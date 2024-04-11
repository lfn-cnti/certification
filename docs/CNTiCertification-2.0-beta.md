# WIP: CNTi Certification List of Tests - v2.0-beta

## Summary
This document provides a summary of the tests included in the Cloud Native Telecom Initiative (CNTi) Certification. Each test lists a general overview of what the test does, a link to the test code for that test, and links to additional information when relevant/available.

To learn how to run these tests, see the "Instructions." For further details see the [USAGE guide](https://github.com/cnti-testcatalog/testsuite/blob/main/USAGE.md).

To learn why these tests were written, see the [RATIONALE.md](https://github.com/cnti-testcatalog/testsuite/blob/main/RATIONALE.md).

### Types of Tests (Currently TBD total for the Certification)
- **Essential**: 20 total
- **Normal**: TBD total
- **Bonus**: TBD total

### The first level of certification requires the passing of 15 of the 20 total essential tests.

### The List of Essential Tests are:
1. [latest_tag](#latest-tag)
2. [selinux_options](#selinux-options)
3. [single_process_type](#single-process-type-in-one-container)
4. [sig_term_handled](#SIGTERM-Handled)
5. [specialized_init_system](#specialized-init-system)
6. [zombie_handled](#zombie-handled)
7. [node_drain](#node-drain)
8. [volume_hostpath_not_found](#Volume-hostpath-not-found)
9. [liveness](#helm-chart-liveness)
10. [readiness](#helm-chart-readiness)
11. [pod_dns_error](#Pod-DNS-error)
12. [log_output](#use-stdoutstderr-for-logs)
13. [container_sock_mounts](#container-socket-mounts)
14. [privileged_containers](#privileged-containers-kubescape)
15. [non_root_containers](#non-root-containers)
16. [resource_policies](#resource-policies)
17. [hostpath_mounts](#hostpath-mounts)
18. [hostport_not_used](#hostport-not-used)
19. [hardcoded_ip_addresses_in_k8s_runtime_configuration](#hardcoded-ip-addresses-in-k8s-runtime-configuration)
20. [increase_decrease_capacity](#increase-decrease-capacity)


List of Workload Tests
---

# Compatibility, Installability, and Upgradability Category

## [Increase decrease capacity:](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/compatibility.cr#L168)
- Added to CNF Certification in v1.0
- Expectation: The number of replicas for a Pod increases, then the number of replicas for a Pod decreases <!-- see #Increase-decrease-capacity -->

**What's tested:** The pod is increased and replicated to 3 for the CNF image or release being tested. After `increase_capacity` increases the replicas to 3, it decreases back to 1.

<b>The increase and decrease capacity tests:</b> HPA (horizonal pod autoscale) will autoscale replicas to accommodate when there is an increase of CPU, memory or other configured metrics to prevent disruption by allowing more requests 
by balancing out the utilisation across all of the pods.

Decreasing replicas works the same as increase but rather scale down the number of replicas when the traffic decreases to the number of pods that can handle the requests.

You can read more about horizonal pod autoscaling to create replicas [here](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) and in the [K8s scaling cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/#scaling-resources).
</p>


## [Helm chart published](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/compatibility.cr#L406)
- Added to CNF Certification in v1.0
- Expectation: Helm chart is published

**What's tested:** Checks if a Helm chart is published

## [Helm chart valid](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/compatibility.cr#L449)
- Added to CNF Certification in v1.0
- Expectation: Helm chart is valid

**What's tested:** This runs `helm lint` against the helm chart being tested. You can read more about the helm lint command at [helm.sh](https://helm.sh/docs/helm/helm_lint/)

## [Helm deploy](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/compatibility.cr#L339)
- Added to CNF Certification in v1.0
- Expectation: Helm deploy is successful

**What's tested:** This checks if the CNF was deployed using Helm

## [Rollback](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/compatibility.cr#L87)
- Added to CNF Certification in v1.0
- Expectation: CNF rollback is successful 

**What's tested:** To check if a CNF version can be [rolled back](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-back-a-deployment)

## [CNI compatible](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/compatibility.cr#L588)
- Added to CNF Certification in v1.0
- Expectation: CNF should be compatible with multiple and different CNIs

**What's tested:** This installs temporary kind clusters and will test the CNF against both Calico and Cilium CNIs. 


# Microservice Category

## [Reasonable image size](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/microservice.cr#L268) 
- Added to CNF Certification in v1.0
- Expectation: CNF image size is under 5 gigs

**What's tested:** Checks the size of the image used.

## [Reasonable startup time](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/microservice.cr#L183)
- Added to CNF Certification in v1.0
- Expectation: CNF starts up under 30 seconds

**What's tested:** This counts how many seconds it takes for the CNF to startup.

## [Single process type in one container](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/microservice.cr#L359)
- Added to CNF Certification in v1.0
- Expectation: CNF container has one process type

**What's tested:** This verifies that there is only one process type within one container. This does not count against child processes. Example would be nginx or httpd could have a parent process and then 10 child processes but if both nginx and httpd were running, this test would fail.

## [Service discovery](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/microservice.cr#L413)
- Added to CNF Certification in v1.0
- Expectation: CNFs should not expose their containers as a service

**What's tested:** This tests and checks if a container for the CNF has services exposed. Application access for microservices within a cluster should be exposed via a Service. Read more about K8s Service [here](https://kubernetes.io/docs/concepts/services-networking/service/).
  
## [Shared database](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/microservice.cr#L19)  
- Added to CNF Certification in v1.0
- Expectation: Multiple microservices should not share the same database.

**What's tested:** This tests if multiple CNFs are using the same database.

## [SIGTERM Handled](https://github.com/cnti-testcatalog/testsuite/blob/v0.46.0/src/tasks/workload/microservice.cr#L500)
- Added to CNTi Certification in v2.0-beta
- Expectation: SIGTERM is handled by PID 1 process of containers.
- ID: sig_term_handled

**What's tested:** This tests if the PID 1 process of containers handles SIGTERM.

## [Specialized Init System](https://github.com/cnti-testcatalog/testsuite/blob/v0.46.0/src/tasks/workload/microservice.cr#L787)
- Added to CNTi Certification in v2.0-beta
- Expectation: Container images should use specialized init systems for containers.
- ID: specialized_init_system

**What's tested:** This tests if containers in pods have dumb-init, tini or s6-overlay as init processes.

## [Zombie Handled](https://github.com/cnti-testcatalog/testsuite/blob/v0.46.0/src/tasks/workload/microservice.cr#L437)
- Added to CNTi Certification in v2.0-beta
- Expectation: Zombie processes are handled/reaped by PID 1 process of containers.
- ID: zombie_handled

**What's tested:** This tests if the PID 1 process of containers handles/reaps zombie processes.

# State Category

## [Node drain](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/state.cr#L209) 
- Added to CNF Certification in v1.0
- Expectation: A node will be drained and rescheduled onto other available node(s).

**What's tested:** A node is drained and rescheduled to another node, passing with a liveness and readiness check. This will skip when the cluster only has a single node.

## [No local volume configuration](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/state.cr#L457) 
- Added to CNF Certification in v1.0
- Expectation: Local storage should not be used or configured.

**What's tested:** This tests if local volumes are being used for the CNF.

## [Elastic volumes](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/state.cr#L321) 
- Added to CNF Certification in v1.0
- Expectation: Elastic persistent volumes should be configured for statefulness.

**What's tested:** This checks for elastic persistent volumes in use by the CNF.

## [Volume hostpath not found](https://github.com/cnti-testcatalog/testsuite/blob/v0.46.0/src/tasks/workload/state.cr#L501)
- Added to CNTi Certification in v2.0-beta
- Expectation: Volume host path configurations should not be used.
- ID: volume_hostpath_not_found

**What's tested:** This tests if volume host paths are configured and used by the CNF.


# Reliability, Resilience and Availability Category

## [Pod network latency](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/reliability.cr#L231)
- Added to CNF Certification in v1.0
- Expectation: The CNF should continue to function when network latency occurs

**What's tested:** [This experiment](https://docs.litmuschaos.io/docs/pod-network-latency/) causes network degradation without the pod being marked unhealthy/unworthy of traffic by kube-proxy (unless you have a liveness probe of sorts that measures latency and restarts/crashes the container). The idea of this experiment is to simulate issues within your pod network OR microservice communication across services in different availability zones/regions etc.

The applications may stall or get corrupted while they wait endlessly for a packet. The experiment limits the impact (blast radius) to only the traffic you want to test by specifying IP addresses or application information. This experiment will help to improve the resilience of your services over time.


## [Disk fill](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/reliability.cr#L390)
- Added to CNF Certification in v1.0
- Expectation: The CNF should continue to function when disk fill occurs

**What's tested:** [Stressing the disk](https://litmuschaos.github.io/litmus/experiments/categories/pods/disk-fill/) with continuous and heavy IO for example can cause degradation in reads written by other microservices that use this shared disk for example modern storage solutions for Kubernetes to use the concept of storage pools out of which virtual volumes/devices are carved out. Another issue is the amount of scratch space eaten up on a node which leads to the lack of space for newer containers to get scheduled (Kubernetes too gives up by applying an "eviction" taint like "disk-pressure") and causes a wholesale movement of all pods to other nodes. Similarly with CPU chaos, by injecting a rogue process into a target container, we starve the main microservice process (typically PID 1) of the resources allocated to it (where limits are defined) causing slowness in application traffic or in other cases unrestrained use can cause the node to exhaust resources leading to the eviction of all pods. So this category of chaos experiment helps to build the immunity on the application undergoing any such stress scenario.


##  [Pod delete](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/reliability.cr#L441)
- Added to CNF Certification in v1.0
- Expectation: The CNF should continue to function when pod delete occurs

**What's tested:** [This experiment](https://litmuschaos.github.io/litmus/experiments/categories/pods/pod-delete/) helps to simulate such a scenario with forced/graceful pod failure on specific or random replicas of an application resource and checks the deployment sanity (replica availability & uninterrupted service) and recovery workflow of the application.

## [Memory hog](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/reliability.cr#L495)
- Added to CNF Certification in v1.0
- Expectation: The CNF should continue to function when pod memory hog occurs

**What's tested:** The [pod-memory hog](https://litmuschaos.github.io/litmus/experiments/categories/pods/pod-memory-hog/) experiment launches a stress process within the target container - which can cause either the primary process in the container to be resource constrained in cases where the limits are enforced OR eat up available system memory on the node in cases where the limits are not specified.  


## [IO Stress](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/reliability.cr#L549)
- Added to CNF Certification in v1.0
- Expectation: The CNF should continue to function when pod io stress occurs

**What's tested:** This test stresses the disk with with continuous and heavy IO to cause degradation in reads/ writes by other microservices that use this shared disk. 

## [Network corruption](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/reliability.cr#L284)
- Added to CNF Certification in v1.0
- Expectation: The CNF should continue to function when pod network corruption occurs

**What's tested:** This test uses the LitmusChaos [pod_network_corruption](https://litmuschaos.github.io/litmus/experiments/categories/pods/pod-network-corruption/) experiment.

## [Network duplication](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/reliability.cr#L337)
- Added to CNF Certification in v1.0
- Expectation: The CNF should continue to function when pod network duplication occurs

**What's tested:** This test uses the LitmusChaos [pod_network_duplication](https://litmuschaos.github.io/litmus/experiments/categories/pods/pod-network-duplication/) experiment.

## [Helm chart liveness](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/reliability.cr#L15)
- Added to CNF Certification in v1.0
- Expectation: A liveness probe should be found in the CNF cluster

**What's tested:** This test checks for livenessProbe in the resource and container

## [Helm chart readiness](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/reliability.cr#L45)
- Added to CNF Certification in v1.0
- Expectation: A readiness probe should be found in the CNF cluster

**What's tested:** This test check for readinessProbe in the resource and container


# Observability and Diagnostic Category

## [Use stdout/stderr for logs](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/observability.cr#L13)
- Added to CNF Certification in v1.0
- Expectation: Resource output logs should be sent to STDOUT/STDERR

**What's tested:** This checks and verifies that STDOUT/STDERR is configured for logging.

For example, running `kubectl get logs` returns useful information for diagnosing or troubleshooting issues. 

## [Prometheus installed](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/observability.cr#L42)
- Added to CNF Certification in v1.0
- Expectation: Prometheus is being used for the cluster and CNF for metrics.

**What's tested:** Tests for the presence of [Prometheus](https://prometheus.io/) or if the CNF emit prometheus traffic.

## [Fluentd logs](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/observability.cr#L170)
- Added to CNF Certification in v1.0
- Expectation: Fluentd is capturing logs.

**What's tested:** Checks for fluentd presence and if logs are being captured for fluentd.

## [OpenMetrics compatible](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/observability.cr#L146)
- Added to CNF Certification in v1.0
- Expectation: CNF should emit OpenMetrics compatible traffic.

**What's tested:** Checks if OpenMetrics is being used and or compatible.

## [Jaeger tracing](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/observability.cr#L203)
- Added to CNF Certification in v1.0
- Expectation: The CNF should use tracing

**What's tested:** Checks if Jaeger is configured and tracing is being used.


# Security Category

## [Container socket mounts](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/security.cr#L51)
- Added to CNF Certification in v1.0
- Expectation: Container engine daemon sockets should not be mounted as volumes

**What's tested** This test uses the Kyverno policy called [Disallow CRI socket mounts
](https://kyverno.io/policies/best-practices/disallow_cri_sock_mount/disallow_cri_sock_mount/)

## [Sysctls test]
- Added to CNF Certification in v1.0
- Expectation: TBD

**What's tested: TBD**


## [External IPs](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/security.cr#L31)
- Added to CNF Certification in v1.0
- Expectation: A CNF should not run services with external IPs

**What's tested:** Checks if the CNF has services with external IPs configured


## [Privilege escalation](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/security.cr#L156)
- Added to CNF Certification in v1.0
- Expectation: Containers should not allow for [privilege escalation](https://bit.ly/C0016_privilege_escalation)

**What's tested: TBD** <b>Privilege Escalation:</b> Check that the allowPrivilegeEscalation field in securityContext of container is set to false.

See more at [ARMO-C0016](https://bit.ly/C0016_privilege_escalation)


## [Symlink file system](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/security.cr#L175)
- Added to CNF Certification in v1.0
- Expectation: No containers allow a [symlink](https://bit.ly/C0058_symlink_filesystem) attack

**What's tested:**
This control checks the vulnerable versions and the actual usage of the subPath feature in all Pods in the cluster.

See more at [ARMO-C0058](https://bit.ly/C0058_symlink_filesystem)

## [Application credentials](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/security.cr#L194)
- Added to CNF Certification in v1.0
- Exepectation: Application credentials should not be found in configuration files

**What's tested:**
Check if the pod has sensitive information in environment variables, by using list of known sensitive key names. Check if there are configmaps with sensitive information.

See more at [ARMO-C0012](https://bit.ly/C0012_application_credentials)


## [Host network](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/security.cr#L213)
- Added to CNF Certification in v1.0
- Expectation: PODs should not have access to the host systems network.

**What's tested:** Checks if there is a [host network attached to a pod](https://bit.ly/C0041_hostNetwork). See more at [ARMO-C0041](https://bit.ly/C0041_hostNetwork)

## [Service account mapping](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/security.cr#L232)
- Added to CNF Certification in v1.0
- Expectation: The [automatic mapping](https://bit.ly/C0034_service_account_mapping) of service account tokens should be disabled. 

**What's tested:** Check if service accounts are automatically mapped. See more at [ARMO-C0034](https://bit.ly/C0034_service_account_mapping).


## [Ingress and Egress blocked](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/security.cr#L335)
- Added to CNF Certification in v1.0
- Expectation: [Ingress and Egress traffic should be blocked on Pods](https://bit.ly/3bhT10s).

**What's tested:** Checks Ingress and Egress traffic policy


## [Privileged containers, Kubescape](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/security.cr#L156) 
- Added to CNF Certification in v1.0
- Expectation: Containers should not allow privilege escalation

**What's tested:** Check in POD spec if securityContext.privileged == true. Read more at [ARMO-C0057](https://bit.ly/31iGng3)


## [Insecure capabilities](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/security.cr#L272)
- Added to CNF Certification in v1.0
- Expectation: Containers should not have insecure capabilities enabled

**What's tested:** Checks for insecure capabilities. See more at [ARMO-C0046](https://bit.ly/C0046_Insecure_Capabilities)

This test checks against a [blacklist of insecure capabilities](https://github.com/FairwindsOps/polaris/blob/master/checks/insecureCapabilities.yaml).
    

## [Non-root containers](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/security.cr#L377)
- Added to CNF Certification in v1.0
- Expectation: Containers should run with non-root user with non-root group membership

**What's tested:** Checks if containers are running with non-root user with non-root membership. Read more at [ARMO-C0013](https://bit.ly/2Zzlts3)


## [Host PID/IPC privileges](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/security.cr#L356)
- Added to CNF Certification in v1.0
- Expectation: Containers should not have hostPID and hostIPC privileges

**What's tested:** Checks if containers are running with hostPID or hostIPC privileges. Read more at [ARMO-C0038](https://bit.ly/3nGvpIQ)

## [SELinux options]
- Added to CNF Certification in v1.0
- Expectation: SELinux options should not be used

**What's tested:** Checks if CNF resources use custom SELinux options that allow privilege escalation (selinux_options)

## [Linux hardening](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/security.cr#L251)
- Added to CNF Certification in v1.0
- Expectation: Security services are being used to harden application

**What's tested:** Checks if security services are being used to harden the application. Read more at [ARMO-C0055](https://bit.ly/2ZKOjpJ)

## [Resource policies](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/security.cr#L314)
- Added to CNF Certification in v1.0
- Expectation: Containers should have resource limits defined

**What's tested:**
Check for each container if there is a ‘limits’ field defined. Check for each limitrange/resourcequota if there is a max/hard field defined, respectively. Read more at [ARMO-C0009](https://bit.ly/3Ezxkps).


## [Immutable File Systems](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/security.cr#L441)
- Added to CNF Certification in v1.0
- Expectation: Containers should have immutable file system

**What's tested:**
Checks whether the readOnlyRootFilesystem field in the SecurityContext is set to true. Read more at [ARMO-C0017](https://bit.ly/3pSMtxK)


## [HostPath Mounts](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/security.cr#L462)
- Added to CNF Certification in v1.0
- Expectation: Containers should not have hostPath mounts 

**What's tested:** TBD
Read more at [ARMO-C0045](https://bit.ly/3EvltIL)

## [Default namespaces]
- Added to CNF Certification in v1.0
- Expectation: To check if resources of the CNF are not in the default namespace

**What's tested:** TBD


# Configuration Category


## [Latest tag] 
- Added to CNF Certification in v1.0
- Expectation: Checks if a CNF is using 'latest' tag instead of a version.

**What's tested:** TBD

## [Require labels](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/configuration.cr#L18) 
- Added to CNF Certification in v1.0
- Expectation: Checks if pods are using the 'app.kubernetes.io/name' label

**What's tested:** TBD


## [nodePort not used](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/configuration.cr#L131) 
- Added to CNF Certification in v1.0
- Expectation: Checks for configured node ports in the service configuration.

**What's tested:** TBD

## [hostPort not used](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/configuration.cr#L166) 
- Added to CNF Certification in v1.0
- Expectation: Checks for configured host ports in the service configuration.

**What's tested:** TBD

## [Hardcoded IP addresses in K8s runtime configuration](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/configuration.cr#L213) 
- Added to CNF Certification in v1.0
- Expectation: Checks for hardcoded IP addresses or subnet masks in the K8s runtime configuration.

**What's tested:** TBD

## [Secrets used](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/configuration.cr#L257) 
- Added to CNF Certification in v1.0
- Expectation: Checks for K8s secrets.

**What's tested:** TBD

## [Immutable configmap](https://github.com/cncf/cnf-testsuite/blob/v0.27.0/src/tasks/workload/configuration.cr#L362) 
- Added to CNF Certification in v1.0
- Expectation: Checks for K8s version and if immutable configmaps are enabled.

**What's tested:** TBD

## [Pod DNS error](https://github.com/cnti-testcatalog/testsuite/blob/v0.46.0/src/tasks/workload/reliability.cr#L700)
- Added to CNTi Certification in v2.0-beta
- Expectation: Test if the CNF crashes when pod dns error occurs

**What's tested:** The [pod-dns error experiment](https://litmuschaos.github.io/litmus/experiments/categories/pods/pod-dns-error/) injects chaos to disrupt DNS resolution in kubernetes pods and causes loss of access to services by blocking DNS resolution of hostnames/domains.
