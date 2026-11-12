# Ella Core CNTi certification evidence

**Draft for review.** The recorded standalone run passed 16 of 19 essential
tests with CNTi Test Suite v2.4.1 using the published Ella Core v1.22.0
image, revision `f6eae2c31518cbe214c7baaf0cdacc628fd54055`. The three failed
checks were non-root execution, zombie reaping, and dedicated init.
[Upstream release](https://github.com/ellanetworks/core/releases/tag/v1.22.0)

The attached result covers all 19 essential checks in the
[CNTi v2.0-beta test list](../../docs/CNTiCertification-2.0-beta.md).

## Reproduce the run

Use a Linux amd64 host with Docker, SCTP support, and a kernel with TCX. The
recorded run used Linux 6.8, kind v0.33.0, Kubernetes v1.37.0, containerd
v2.3.4, Helm v3.20.2, and kindnet. Core used the pod's `eth0` for N2, N3, and
N6. The cluster had two workers and shared test storage so CNTi could
reschedule Core during node drain. Multus was installed on the test cluster,
but this deployment requested no secondary interfaces.

From this directory, unpack the exact Helm chart used in the run and generate
a disposable bootstrap password. Keep the generated values and kubeconfig local.

```bash
echo '63ff71d8f8e199a20bc4a20daac70c00f800fc9afad3be929f66df2889df7e86  ella-core-2.3.0.tgz' |
  sha256sum --check
tar -xzf ella-core-2.3.0.tgz
umask 077
printf 'admin:\n  password: "CNTi-%s!"\n' "$(openssl rand -hex 16)" > test-values.yaml
sudo modprobe sctp
export KUBECONFIG="$PWD/kubeconfig"
```

Create a fresh kind cluster. Share a host directory across the nodes for this
test and allow the two pod forwarding sysctls on both workers.

```bash
mkdir -p "$PWD/shared-storage"
cat > kind.yaml <<EOF
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
kubeadmConfigPatches:
  - |
    kind: KubeletConfiguration
    allowedUnsafeSysctls:
      - net.ipv4.ip_forward
      - net.ipv6.conf.all.forwarding
nodes:
  - role: control-plane
    extraMounts:
      - hostPath: $PWD/shared-storage
        containerPath: /var/local-path-provisioner
  - role: worker
    extraMounts:
      - hostPath: $PWD/shared-storage
        containerPath: /var/local-path-provisioner
  - role: worker
    extraMounts:
      - hostPath: $PWD/shared-storage
        containerPath: /var/local-path-provisioner
EOF
kind create cluster --name ella-cnti --image kindest/node:v1.37.0 --config kind.yaml --wait 5m
kubectl -n local-path-storage patch configmap local-path-config --type merge \
  -p '{"data":{"config.json":"{\"nodePathMap\":[],\"sharedFileSystemPath\":\"/var/local-path-provisioner\"}"}}'
kubectl -n local-path-storage rollout restart deployment/local-path-provisioner
kubectl -n local-path-storage rollout status deployment/local-path-provisioner --timeout=2m
```

The Helm configuration pulls the published image from GHCR. Install the
[CNTi Test Suite v2.4.1 binary](https://github.com/lfn-cnti/testsuite/releases/tag/v2.4.1)
on your path.

```bash
cnti-testsuite setup
cnti-testsuite cnf_install --cnf-config ./cnf-testsuite.yml --timeout 900
kubectl -n ella-cnti-v122 rollout status statefulset/ella-v122-core --timeout=5m
kubectl -n ella-cnti-v122 wait --for=condition=complete job/ella-v122-core-bootstrap-1 --timeout=5m
helm test ella-v122 -n ella-cnti-v122 --timeout 2m
CNTI_TESTSUITE_GENERIC_OPERATION_TIMEOUT=180 cnti-testsuite cert
```
