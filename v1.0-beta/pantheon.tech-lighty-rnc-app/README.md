# Lighty RNC application

Download [RNC 16.2.0](https://github.com/PANTHEONtech/helm-charts/blob/main/lighty-rnc-app-helm-16.2.0.tgz)
helm-chart from published repository. Unzip file and save it to folder with CNF configuration file.

## Set Up
Set up the testsuite like normal

```
cnf-testsuite setup
```

Then setup the config

```
cnf-testsuite cnf_setup cnf-config=./cnf-testsuite.yml
```

## Running the test

```
cnf-testsuite cert
```

## Results

Once its complete, it should produce a results file and output a summary of the results

```
RESULTS SUMMARY
  - 31 of 42 total tests passed
  - 13 of 14 essential tests passed
Results have been saved to results/cnf-testsuite-results-20220916-110355-721.yml
```