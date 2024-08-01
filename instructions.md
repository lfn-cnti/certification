# How to submit CNTi Certification results

## Participation Prerequisites

- Review the CNTi Participation Terms and Conditions (https://github.com/lfn-cnti/certification/blob/main/Certified_CNTi_Terms.md)
- Complete, sign, and submit the CNTi Participation Form (https://github.com/lfn-cnti/certification/blob/main/Certified_CNTi_Form.md)

## The Certification Tests

There are [19 tests](docs/CNTiCertification-2.0-beta.md) included in the CNTi Certification v2.0-beta. These tests are classified as Essential and are a subset of the total tests that are available for use in the [CNTi Test Catalog](https://github.com/cnti-testcatalog/testsuite). Tests are organized into 7 categories: 
- Compatibility, Installability, and Upgradability
- Configuration
- Security
- State
- Microservice
- Observability
- Reliability, Resilience and Availability

## Technical Prerequisites

**K8s Multi-node Cluster**
- [Access](https://kubernetes.io/docs/tasks/access-application-cluster/access-cluster/) to a working [Certified K8s](https://cncf.io/ck) multi-node cluster using the [containerd](https://containerd.io/) runtime

**Workstation**
- Linux OS
- wget and curl binaries available in PATH
- Public internet access for downloading dependencies
- An admin `kubeconfig` file referenced from the [KUBECONFIG environment variable](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/#set-the-kubeconfig-environment-variable). (See [K8s Getting started guide](https://kubernetes.io/docs/setup/) for options)

If you do not have a K8s cluster running, you can deploy your own cluster with [various tools](https://kubernetes.io/docs/setup/) or use a [public cloud or turnkey solution](https://kubernetes.io/docs/setup/production-environment/turnkey-solutions/)

Kind is one option tested and supported by the CNTi Test Catalog.  Follow the [kind install](https://github.com/cnti-testcatalog/testsuite/blob/main/KIND-INSTALL.md) instructions to setup a cluster in [kind](https://kind.sigs.k8s.io/)


## Running
The standard tool for running these tests is the [CNTi Test Catalog](https://github.com/cnti-testcatalog/testsuite). 

1. Download the [certification binary release](https://github.com/cnti-testcatalog/testsuite/releases/latest) of the test suite
1. Run setup to prepare the cnf-testsuite: `cnf-testsuite setup`
3. Create a configuration file for testing your CNF
    1. Review the [CNF_TESTSUITE_YML_USAGE.md](https://github.com/cnti-testcatalog/testsuite/blob/main/CNF_TESTSUITE_YML_USAGE.md) document on formatting and other requirements.
    1. Create a cnf-testsuite.yml with the required information
4. Initialize the test suite for using the CNF: `cnf-testsuite cnf_setup cnf-config=./cnf-testsuite.yml`
5. Run the certification tests: `cnf-testsuite cert`

The results file will be saved to: results/cnf-testsuite-results-YEAR-MMDD-HHMMSS-NNN.yml

Example:
- Test results have been saved to results/cnf-testsuite-results-2024-0419-201220-225.yml

NOTE: The results file and the config file are required in the submission results.


<!--1. Pull down an example CNF configuration to try: curl -o cnf-testsuite.yml https://raw.githubusercontent.com/cnti-testcatalog/testsuite/main/example-cnfs/coredns/cnf-testsuite.yml-->


## Uploading

Prepare a Pull Request (PR) to [https://github.com/lfn-cnti/certification](https://github.com/lfn-cnti/certification).

Here are [directions](https://help.github.com/en/articles/creating-a-pull-request-from-a-fork) to prepare a pull request from a fork.

The PR title should look like this: `CNTi Certification results for vX.Y/DIR`

In the PR title above, the `X.Y` refers to the CNTi Certification major and minor version, and `DIR` is a short subdirectory name to hold the results for your product.  Examples

- `CNTi Certification results for v1.0/mychargingapp`
- `CNTi Certification results for v1.2/fancyfirewall`


### Contents of the PR

If submitting test results for multiple versions, submit a PR for each product, ie. one PR for vX.Y results and a second PR for vX.Z

Your PR should have the subdirecty (as mentioned above) with the following files:

- vX.Y/DIR/README.md: A script or human-readable description of how to reproduce
your results
- vX.Y/DIR/cnf-testsuite.yml test suite configuration for your CNF
- vX.Y/DIR/cnf-testsuite-results-YYYY-MMDD-HHMMSS-NNN.yml log output (from CNTi Test Suite)
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

## Certification Review
Reviews of certification pull requests are conducted by approved reviewers. Approved reviewers are selected by the CNTi TSC and maintained in ([this file](https://github.com/lfn-cnti/certification/blob/main/CertificationReviewers.md)).

A minimum of 2 approved reviewers, who are not from Related Companies, will comment on and/or approve your pull request, following [this process](https://github.com/lfn-cnti/certification/blob/main/reviewing.md). 

Related Companies are defined as any entity (Company-A) which controls or is controlled by another entity (Company-B) or which, together, is under the common control of a third party (Company-C), in each case where such control results from ownership, either directly or indirectly, of more than fifty percent of the voting securities or membership interests of the entity in question; and “Related Companies” are entities that are each a Related Company as described above.

If you don't see a response within 5 business days, please send an email to lfn-cnti@lfnetworking.org.


## Issues
If you have problems self-certifying that you feel is due to an issue with the CNTi Certification program (and not just your own implementation), you can file an issue in this repository. Questions and comments can also be sent via email to lfn-cnti@lfnetworking.org.
