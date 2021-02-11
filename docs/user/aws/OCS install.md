aws
OCP install

export AWS_PROFILE=username
aws configure get region
eu-central-1
aws ec2 describe-images --filters "Name=name,Values=rhcos-4*" --query 'sort_by(Images, &CreationDate)[-1].ImageId' --output text


OCS intall:

https://red-hat-storage.github.io/ocs-training/training/ocs4/ocs.html
https://docs.openshift.com/container-platform/4.5/machine_management/creating-infrastructure-machinesets.html (infra nodes)
https://www.redhat.com/en/blog/mysql-openshift-container-storage-performance-and-failover-under-heavy-load
for pod in $(oc get pods|grep mysql|awk '{print $1}');do echo "pod $pod";oc rsh -c mysql-ocs-fs mysql-ocs-fs-9 mysql -uroot -ppassword -h localhost sysbench -e "SELECT count(1) from sbtest10;";done

misc.
oc get configmaps -A -o yaml | grep -C 3 MachineNetwork
...
  applied: '{"clusterNetwork":[{"cidr":"10.128.0.0/14","hostPrefix":23}],"serviceNetwork":["172.30.0.0/16"],"defaultNetwork":{"type":"OpenShiftSDN","openshiftSDNConfig":{"mode":"NetworkPolicy","vxlanPort":4789,"mtu":8951,"enableUnidling":true}},"disableMultiNetwork":false,"deployKubeProxy":false,"kubeProxyConfig":{"bindAddress":"0.0.0.0"},"logLevel":"Normal"}'
https://github.com/openshift/installer/blob/master/docs/user/aws/customization.md
...
