# How to submit CNTI Certification results

## The tests

There are [57 tests](docs/CNFCertification-1.0-beta.md) included in the CNF Certification v1.0-beta. These tests are classified as Essential, Normal, or Bonus. They are also organized into 7 categories: 
- Compatibility, Installability, and Upgradability
- Configuration
- Security
- State
- Microservice
- Observability
- Reliability, Resilience and Availability

## Prereqs

**K8s Multi-node Cluster**
- [Access](https://kubernetes.io/docs/tasks/access-application-cluster/access-cluster/) to a working [Certified K8s](https://cncf.io/ck) multi-node cluster using the [containerd](https://containerd.io/) runtime

**Workstation**
- Linux OS
- wget and curl binaries available in PATH
- Public internet access for downloading dependencies
- An admin `kubeconfig` file referenced from the [KUBECONFIG environment variable](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/#set-the-kubeconfig-environment-variable). (See [K8s Getting started guide](https://kubernetes.io/docs/setup/) for options)

If you do not have a K8s cluster running, you can deploy your own cluster with [various tools](https://kubernetes.io/docs/setup/) or use a [public cloud or turnkey solution](https://kubernetes.io/docs/setup/production-environment/turnkey-solutions/)

Kind is one option tested and supported by the CNTI Test Catalog team.  Follow the [kind install](https://github.com/cnti-testcatalog/testsuite/blob/main/KIND-INSTALL.md) instructions to setup a cluster in [kind](https://kind.sigs.k8s.io/)


## Running
The standard tool for running these tests is the [CNTI Test Catalog](https://github.com/cnti-testcatalog/testsuite). 

1. Download the [certification binary release](https://github.com/cnti-testcatalog/testsuite/releases/tag/v0.29.1) of the test suite
1. Run setup to prepare the cnf-testsuite: `cnf-testsuite setup`
3. Create a configuration file for testing your CNF
    1. Review the [CNF_TESTSUITE_YML_USAGE.md](https://github.com/cnti-testcatalog/testsuite/blob/main/CNF_TESTSUITE_YML_USAGE.md) document on formatting and other requirements.
    1. Create a cnf-testsuite.yml with the required information
4. Initialize the test suite for using the CNF: `cnf-testsuite cnf_setup cnf-config=./cnf-testsuite.yml`
5. Run the certification tests: `cnf-testsuite cert`

The results file will be saved to: results/cnf-testsuite-results-YEAR-MMDD-HHMMSS-NNN.yml

Example:
- Test results have been saved to results/cnf-testsuite-results-2022-0412-201220-225.yml

NOTE: The results file and the config file are required in the submission results.


<!--1. Pull down an example CNF configuration to try: curl -o cnf-testsuite.yml https://raw.githubusercontent.com/cnti-testcatalog/testsuite/main/example-cnfs/coredns/cnf-testsuite.yml-->


## Uploading

Prepare a Pull Request (PR) to [https://github.com/lfn-cnti/certification](https://github.com/lfn-cnti/certification).

Here are [directions](https://help.github.com/en/articles/creating-a-pull-request-from-a-fork) to prepare a pull request from a fork.

The PR title should look like this: `CNTI Certification results for vX.Y/DIR`

In the PR title above, the `X.Y` refers to the CNTI Certification major and minor version, and `DIR` is a short subdirectory name to hold the results for your product.  Examples

- `CNTI Certification results for v1.0/mychargingapp`
- `CNTI Certification results for v1.2/fancyfirewall`


### Contents of the PR

If submitting test results for multiple versions, submit a PR for each product, ie. one PR for vX.Y results and a second PR for vX.Z

Your PR should have the subdirecty (as mentioned above) with the following files:

- vX.Y/DIR/README.md: A script or human-readable description of how to reproduce
your results
- vX.Y/DIR/cnf-testsuite.yml test suite configuration for your CNF
- vX.Y/DIR/cnf-testsuite-results-YYYY-MMDD-HHMMSS-NNN.yml log output (from CNF Test Suite)
- vX.Y/DIR/PRODUCT.yaml: See below
<!-- - vX.Y/DIR/test.log: Test log output (from CNF Certification).-->


#### PRODUCT.yaml

This file describes your product. It is YAML formatted with the following root-level fields. Please fill in as appropriate.

| Field               | Description |
| ------------------- | ----------- |
| `vendor`            | Name of the legal entity that is certifying. This entity must have a signed participation form on file with the LFN  |
| `name`              | Name of the product being certified. |
| `version`           | The version of the product being certified. |
| `website_url`       | URL to the product information website |
| `repo_url`          | If your product is open source, this field is necessary to point to the primary GitHub repo containing the source. It's OK if this is a mirror. OPTIONAL  |
| `documentation_url` | URL to the product documentation. OPTIONAL |
| `product_logo_url`  | URL to the product's logo, (must be in SVG format -- not a PNG -- and include the product name). If not supplied, we'll use your company logo. |
| `type`              | What kind of application? Eg. Firewall |
| `description` | One sentence description of your offering |

Examples below are for a fictional DHCP-proxy CNF implementation called _Turbo DHCP-Proxy_ produced by a company named _Yoyodyne_.

```yaml
vendor: Yoyodyne
name: Turbo DHCP-Proxy
version: v1.7.4
website_url: https://yoyo.dyne/turbo-dhcp-proxy
repo_url: https://github.com/yoyo.dyne/turbo-dhcp-proxy
documentation_url: https://yoyo.dyne/turbo-dhcp-proxy/docs
product_logo_url: https://yoyo.dyne/assets/turbo-dhcp-proxy.svg
type: dhcp-proxy
description: "The Yoyodyne Turbo DHCP-Proxy is a superb DHCP proxy 
              for all of your proxying needs."
```

## Review
A reviewer will shortly comment on and/or accept your pull request, following [this process](https://github.com/lfn-cnti/certification/blob/main/reviewing.md). If you don't see a response within 5 business days, please send an email to lfn-cnti@lfnetworking.org.

## Issues
If you have problems self-certifying that you feel is due to an issue with the CNTI Certification program (and not just your own implementation), you can file an issue in this repository. Questions and comments can also be sent via email to lfn-cnti@lfnetworking.org.


