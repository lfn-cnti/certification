# F5, Inc. - BIG-IP NEXT Edge Firewall CNF

## Prereqs 

The Cloud-Native Network Functions (CNFs) software images and installation Helm charts are provided in a single tar file. Extract the CNFs images and Helm charts.

SSL/TLS keys and certificates must be generated and stored in the cluster per the instructions found here, https://clouddocs.f5.com/cnfs/robin/latest/cnf-secret-install.html


## CNF Testsuite Set Up

Setup the testsuite

```
cnf-testsuite setup
```

Setup the cnf config using the provided configuration file

```
cnf-testsuite cnf_setup cnf-config=./cnf-testsuite.yml
```

## Running the test

Run the certification test

```
cnf-testsuite cert
```
