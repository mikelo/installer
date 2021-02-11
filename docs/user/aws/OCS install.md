OCS install on an AWS/OCP cluster
=============================================================================

this is a sequence of simplified instructions and solutions

```
export AWS_PROFILE=username
aws configure get region
eu-central-1
```
just to check that we are connected to the AWS network.

```
RHCOSAMI=$(aws ec2 describe-images --filters "Name=name,Values=rhcos-4*" --query 'sort_by(Images, &CreationDate)[-1].ImageId' --output text)
```
to get the latest AMI.

Now you may proceed create the install-config.yaml

openshift-install create install-config --log-level debug



OCS intall:
=========================================================================

instructions from this training were followed:
https://red-hat-storage.github.io/ocs-training/training/ocs4/ocs.html

special steps were taken to create infra nodes (not needing entitlements)
https://docs.openshift.com/container-platform/4.5/machine_management/creating-infrastructure-machinesets.html (infra nodes)

to test the perfomance of the OCS cluster:
https://www.redhat.com/en/blog/mysql-openshift-container-storage-performance-and-failover-under-heavy-load

CLI view of all the tables that were created.
```
for pod in $(oc get pods|grep mysql|awk '{print $1}');do echo "pod $pod";oc rsh -c mysql-ocs-fs mysql-ocs-fs-9 mysql -uroot -ppassword -h localhost sysbench -e "SELECT count(1) from sbtest10;";done
```

miscellaneaous commands to get info from an OCP cluster
================

```
oc get configmaps -A -o yaml | grep -C 3 MachineNetwork
```


...
https://github.com/openshift/installer/blob/master/docs/user/aws/customization.md
...
