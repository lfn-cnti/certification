mkdir tmp
cp csrx.yaml tmp/
export KUBECONFIG=~/.kube/config
.cnf-testsuite/cnf-testsuite generate_config config-src=./tmp output-file=./cnf-testsuite.yml
.cnf-testsuite/cnf-testsuite validate_config cnf-config=cnf-testsuite.yml
.cnf-testsuite/cnf-testsuite cnf_setup cnf-config=./cnf-testsuite.yml
cp ./tmp/csrx.yaml cnfs/be4b2720-f646-47ec-b221-29069c825185/tmp <<< the hex sequence is from the output of the previous command
.cnf-testsuite/cnf-testsuite workload

