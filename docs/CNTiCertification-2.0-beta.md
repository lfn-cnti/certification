# CNTi Certification List of Tests - v2.0-beta

## Summary
This document provides a summary of the tests included in the Cloud Native Telecom Initiative (CNTi) v2.0-beta Certification. Each test lists a general overview of what the test does, a link to the test code for that test, and links to additional information when relevant/available.

To learn how to run these tests, see the "Instructions." For further details see the [USAGE guide](https://github.com/lfn-cnti/testsuite/blob/main/USAGE.md).

To learn why these tests were written, see the [TEST_DOCUMENTATION.md](https://github.com/lfn-cnti/testsuite/blob/main/docs/TEST_DOCUMENTATION.md).

### Essential Certification Tests
- **Essential**: 19 total

V2.0-beta certification requires passing at least 15 of the 19 total Essential tests.

### The List of Essential Tests are:
1. [latest_tag](https://github.com/lfn-cnti/testsuite/blob/main/docs/TEST_DOCUMENTATION.md#latest-tag)
2. [selinux_options](https://github.com/lfn-cnti/testsuite/blob/main/docs/TEST_DOCUMENTATION.md#selinux-options)
3. [single_process_type](https://github.com/lfn-cnti/testsuite/blob/main/docs/TEST_DOCUMENTATION.md#single-process-type-in-one-container)
4. [sig_term_handled](https://github.com/lfn-cnti/testsuite/blob/main/docs/TEST_DOCUMENTATION.md#sigterm-handled)
5. [specialized_init_system](https://github.com/lfn-cnti/testsuite/blob/main/docs/TEST_DOCUMENTATION.md#specialized-init-systems)
6. [zombie_handled](https://github.com/lfn-cnti/testsuite/blob/main/docs/TEST_DOCUMENTATION.md#zombie-handled)
7. [node_drain](https://github.com/lfn-cnti/testsuite/blob/main/docs/TEST_DOCUMENTATION.md#node-drain)
8. [liveness](https://github.com/lfn-cnti/testsuite/blob/main/docs/TEST_DOCUMENTATION.md#helm-chart-liveness-entry)
9. [readiness](https://github.com/lfn-cnti/testsuite/blob/main/docs/TEST_DOCUMENTATION.md#helm-chart-readiness-entry)
10. [log_output](https://github.com/lfn-cnti/testsuite/blob/main/docs/TEST_DOCUMENTATION.md#use-stdoutstderr-for-logs)
11. [container_sock_mounts](https://github.com/lfn-cnti/testsuite/blob/main/docs/TEST_DOCUMENTATION.md#container-socket-mounts)
12. [privileged_containers](https://github.com/lfn-cnti/testsuite/blob/main/docs/TEST_DOCUMENTATION.md#privileged-containers)
13. [non_root_containers](https://github.com/lfn-cnti/testsuite/blob/main/docs/TEST_DOCUMENTATION.md#non-root-containers)
14. [cpu_limits](https://github.com/lfn-cnti/testsuite/blob/main/docs/TEST_DOCUMENTATION.md#cpu-limits)
15. [memory_limits](https://github.com/lfn-cnti/testsuite/blob/main/docs/TEST_DOCUMENTATION.md#memory-limits)
16. [hostpath_mounts](https://github.com/lfn-cnti/testsuite/blob/main/docs/TEST_DOCUMENTATION.md#hostpath-mounts)
17. [hostport_not_used](https://github.com/lfn-cnti/testsuite/blob/main/docs/TEST_DOCUMENTATION.md#hostport-not-used)
18. [hardcoded_ip_addresses_in_k8s_runtime_configuration](https://github.com/lfn-cnti/testsuite/blob/main/docs/TEST_DOCUMENTATION.md#hardcoded-ip-addresses-in-k8s-runtime-configuration)
19. [increase_decrease_capacity](https://github.com/lfn-cnti/testsuite/blob/main/docs/TEST_DOCUMENTATION.md#increase-decrease-capacity)



