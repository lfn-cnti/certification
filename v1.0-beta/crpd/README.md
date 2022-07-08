# Containerized Routing Protocol Daemon#
In order to run these tests, you need the image of cRPD loaded into the system, and the attached crpd.yaml file configured.
# Set Up
Prepare Test Environment
```
cnf-testsuite setup
```
Create Test Files
```
mkdir tmp
cp crpd.yaml tmp/
export KUBECONFIG=~/.kube/config
cnf-testsuite generate_config config-src=./tmp output-file=./cnf-testsuite.yml
```
Validate Test Suite
```
cnf-testsuite validate_config cnf-config=cnf-testsuite.yml
```
Prepare Test Suite
cnf-testsuite cnf_setup cnf-config=./cnf-testsuite.yml
# Running the Tests
The previous command takes several minutes and should create all the folders for the test results
```
cnf-testsuite cert
```
# Results
Test results should be stored under folder "Results" and the end of the file should like like below
```
maximum_points: 1521
total_passed: 22 of 39
essential_passed: 10 of 14"
```
