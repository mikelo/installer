OCS install on an AWS/OCP cluster
=============================================================================

this is a sequence of simplified instructions for an OCS soluction running on infra nodes

At first we want to check that we are well connected to the AWS network.

```
export AWS_PROFILE=username
aws configure get region
eu-central-1
```


Next we proceed with the creation the install-config.yaml

```yaml

```

openshift-install will delete this file, so it is good practice to back it up before lauching the installation command:
```
openshift-install create install-config --log-level debug
cp install-config.yaml install-config.yaml.backup
openshift-install create cluster --log-level debug
```

OCS intall:
=========================================================================

Instructions from this training were followed:
https://red-hat-storage.github.io/ocs-training/training/ocs4/ocs.html

In this section you are required to add the nodes to be used especially by OCS, to get the latest available RHCOS AMI:

```
RHCOSAMI=$(aws ec2 describe-images --filters "Name=name,Values=rhcos-4*" --query 'sort_by(Images, &CreationDate)[-1].ImageId' --output text)
```


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
