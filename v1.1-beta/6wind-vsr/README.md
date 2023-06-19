# 6WIND - Virtual Service Router (VSR)

## Prereqs 

You will need the CNF software image of VSR, installation Helm charts and 6WIND VSR user guide.

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

## Results

Once its complete, it should produce a results file and output a summary of the results

```
RESULTS SUMMARY
  - 34 of 44 total tests passed
  - 11 of 14 essential tests passed
Results have been saved to results/cnf-testsuite-results-20230601-101246-050.yml
```
