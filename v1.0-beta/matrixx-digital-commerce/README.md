# MATRIXX Digital Commerce

You will need the images loaded on your system. This test uses versions 5240 and 5241.
The main version being tested is 5241.


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

Once every thing is loaded (should take around 6-7mins for all deployments), you can now run the cert test

```
cnf-testsuite cert wait_count=100
```

Please note the wait_count, this is used for increase_decrease_capacity.

## Results

Once its complete, it should produce a results file and output a summary of the results

```
RESULTS SUMMARY
2022-05-15 21:33:58 : [runTest001]    [cnfRun]          - 25 of 56 total tests passed
2022-05-15 21:33:58 : [runTest001]    [cnfRun]          - 10 of 13 essential tests passed
2022-05-15 21:33:58 : [runTest001]    [cnfRun]        Results have been saved to results/cnf-testsuite-results-20220515-170901-190.yml
```