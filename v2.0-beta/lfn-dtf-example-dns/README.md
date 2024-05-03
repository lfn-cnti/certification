# Linux Foundation Networking D&TF example - External DNS

This is an example for the LFN CNTi D&TF session

## Set Up
Set up the testsuite like normal in this directory

```
~/cnf-testsuite/external-dns
```
```
cnf-testsuite setup
```
```
cnf-testsuite cnf_setup cnf-config=./cnf-testsuite.yml
```

## Running the test

Once the CNF is set up, the cert test can be run

```
cnf-testsuite cert
```

## Results

Once its complete, it should produce a results file and output a summary of the results

```
total_passed: 46 of 51
essential_passed: 18 of 20
```
