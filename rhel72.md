# How to Upgrade RHEL 7.2 on IBM Cloud Manager with OpenStack servers
* Install plain RHEL 7.1
* Do not yum update
* Install CMWO 4.3.0.0
* Upgrade CMWO to 4.3.0.4
* Only install security updates on all nodes (this will prevent upgrade to 7.2):
```
# yum update-minimal --security -y
```
* Deploy the environment
* Upgrade all of the nodes to RHEL 7.2:
```
# yum update
```

Reference: https://www.ibm.com/developerworks/community/forums/html/topic?id=8ae61ca9-ccb7-45ce-8d42-4b46fb5a8ec2&ps=25#repliesPg=0
