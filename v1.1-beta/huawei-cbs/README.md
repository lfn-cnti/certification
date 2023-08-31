# HUAWEI - Convergent Billing System (CBS)

## Prereqs

You will need the CNF software all images of CBS, Helm charts and CBS user guide.

## CNF Testsuite Set Up

Set up the testsuite like normal in these directories

```
/home/capv/cert/adapterapp
/home/capv/cert/chargingcache
/home/capv/cert/cbpgmdb
/home/capv/cert/cch
/home/capv/cert/cnfgw
/home/capv/cert/lcmopertaor
/home/capv/cert/mountnfs
/home/capv/cert/nfsapp
/home/capv/cert/nslb
```

```
cnf-testsuite setup
```

```
cnf-testsuite cnf_setup cnf-config=./cnf-testsuite.yml
```

## Running the test

Run the certification test

```
cd /home/capv/cert/adapterapp
cnf-testsuite cert wait_count=1000

cd /home/capv/cert/chargingcache
cnf-testsuite cert wait_count=1000

cd /home/capv/cert/cbpgmdb
cnf-testsuite cert wait_count=1000

cd /home/capv/cert/cch
cnf-testsuite cert wait_count=1000

cd /home/capv/cert/cnfgw
cnf-testsuite cert wait_count=1000

cd /home/capv/cert/lcmopertaor
cnf-testsuite cert wait_count=1000

cd /home/capv/cert/mountnfs
cnf-testsuite cert wait_count=1000

cd /home/capv/cert/nfsapp
cnf-testsuite cert wait_count=1000

cd /home/capv/cert/nslb
cnf-testsuite cert wait_count=1000
```

## Results

Once its complete, it should produce a results file and output a summary of the results

```
adapterapp
RESULTS SUMMARY
  - 35 of 42 total tests passed
  - 12 of 14 essential tests passed
Results have been saved to results/cnf-testsuite-results-20230810-087122-716.yml

chargingcache
RESULTS SUMMARY
  - 37 of 42 total tests passed
  - 12 of 14 essential tests passed
Results have been saved to results/cnf-testsuite-results-20230824-112513-643.yml

cbpgmdb
RESULTS SUMMARY
  - 38 of 42 total tests passed
  - 12 of 14 essential tests passed
Results have been saved to results/cnf-testsuite-results-20230824-112530-770.yml

cch
RESULTS SUMMARY
  - 38 of 42 total tests passed
  - 12 of 14 essential tests passed
Results have been saved to results/cnf-testsuite-results-20230810-072142-004.yml

cnfgw
RESULTS SUMMARY
  - 39 of 42 total tests passed
  - 13 of 14 essential tests passed
Results have been saved to results/cnf-testsuite-results-20230810-014053-423.yml

lcmopertaor
RESULTS SUMMARY
  - 40 of 42 total tests passed
  - 13 of 14 essential tests passed
Results have been saved to results/cnf-testsuite-results-20230824-112817-818.yml

mountnfs
RESULTS SUMMARY
  - 36 of 42 total tests passed
  - 11 of 14 essential tests passed
Results have been saved to results/cnf-testsuite-results-20230824-112519-548.yml

nfsapp
RESULTS SUMMARY
  - 36 of 42 total tests passed
  - 12 of 14 essential tests passed
Results have been saved to results/cnf-testsuite-results-20230824-112731-990.yml

nslb
RESULTS SUMMARY
  - 35 of 42 total tests passed
  - 12 of 14 essential tests passed
Results have been saved to results/cnf-testsuite-results-20230812-017681-000.yml
```


