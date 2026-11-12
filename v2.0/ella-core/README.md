# Ella Core CNTi certification evidence

**Draft for review.** The recorded standalone run passed 16 of 19 essential
tests with CNTi Test Suite v2.4.1. It used the amd64 CI build of
[Ella Core PR #1907](https://github.com/ellanetworks/core/pull/1907). The image
reports v1.21.0 and revision `77823894ac5472aa15d8af1d7736f21c7fc57bb1`, the CI
merge commit for PR head `d2483a09c0bd92e3fd40b4ffed5da25d66fcec55`. The
version in `PRODUCT.yaml` identifies that build for this draft. The three
failed checks were non-root execution, zombie reaping, and dedicated init.

Before submission, confirm the offering and version with Ella Networks and
that its signed participation form is on file. The v2.0-beta test list matches
the 19 checks in the attached result. The [submission instructions](../../instructions.md)
link to the latest Test Suite release, while the [review guide](../../reviewing.md)
calls for a major/minor match. Confirm that v2.4.1 evidence is acceptable for
v2.0-beta review.

## Reproduce the run

Use a Linux amd64 host with Docker, SCTP support, and a kernel with TCX. The
recorded run used Linux 6.8, kind v0.33.0, Kubernetes v1.37.0, containerd
v2.3.4, Helm v3.20.2, and kindnet. Core used the pod's `eth0` for N2, N3, and
N6. The cluster had two workers and shared test storage so CNTi could
reschedule Core during node drain.

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

Load the exact amd64 build from the [upstream CI run](https://github.com/ellanetworks/core/actions/runs/42844624031).
This step requires access to the Actions artifact while it is retained. Install
the [CNTi Test Suite v2.4.1 binary](https://github.com/lfn-cnti/testsuite/releases/tag/v2.4.1)
on your path.

```bash
gh run download 42844624031 -R ellanetworks/core -n ellacore-image-amd64 -D image
docker load -i image/ellacore_latest-amd64.tar
test "$(docker image inspect ella-core:latest --format '{{.Id}}')" = \
  sha256:a7a05f7a9ffc946b86e04e100ded7d16a13703be011b40d92c00adbfb3279570
docker tag ella-core:latest ella-core:pr1907-d2483a0
kind load docker-image ella-core:pr1907-d2483a0 --name ella-cnti

cnti-testsuite setup
cnti-testsuite cnf_install --cnf-config ./cnf-testsuite.yml --timeout 900
kubectl -n ella-pr1907-pod rollout status statefulset/ella-pr-core --timeout=5m
kubectl -n ella-pr1907-pod wait --for=condition=complete job/ella-pr-core-bootstrap-1 --timeout=5m
helm test ella-pr -n ella-pr1907-pod --timeout 2m
CNTI_TESTSUITE_GENERIC_OPERATION_TIMEOUT=180 cnti-testsuite cert
```
