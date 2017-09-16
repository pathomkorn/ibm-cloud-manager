# openstack-heat-engine Service Starts Fail When Integrate with IBM UCD 6.2.5.2

* Verify that openstack-heat-engine fails because of heat engine version check

```bash
# systemctl status -l openstack-heat-engine
â openstack-heat-engine.service - Openstack Heat Engine Service
   Loaded: loaded (/usr/lib/systemd/system/openstack-heat-engine.service; enabled; vendor preset: disabled)
   Active: failed (Result: exit-code) since Sat 2017-09-16 20:54:32 ICT; 6min ago
  Process: 9900 ExecStart=/usr/bin/heat-engine --config-file /etc/heat/heat.conf --log-file /var/log/heat/engine.log (code=exited, status=1/FAILURE)
 Main PID: 9900 (code=exited, status=1/FAILURE)

Sep 16 20:54:32 icm.piggy.io heat-engine[9900]: 2017-09-16 20:54:32.265 9900 TRACE heat-engine     if juno_or_newer():
Sep 16 20:54:32 icm.piggy.io heat-engine[9900]: 2017-09-16 20:54:32.265 9900 TRACE heat-engine   File "/usr/lib/heat/ibm-cloud-ext/resources/heat_version_helper.py", line 97, in juno_or_newer
Sep 16 20:54:32 icm.piggy.io heat-engine[9900]: 2017-09-16 20:54:32.265 9900 TRACE heat-engine     return heat_is_newer_than("juno", inclusive=True)
Sep 16 20:54:32 icm.piggy.io heat-engine[9900]: 2017-09-16 20:54:32.265 9900 TRACE heat-engine   File "/usr/lib/heat/ibm-cloud-ext/resources/heat_version_helper.py", line 78, in heat_is_newer_than
Sep 16 20:54:32 icm.piggy.io heat-engine[9900]: 2017-09-16 20:54:32.265 9900 TRACE heat-engine     raise Exception("Unable to determine Heat release")
Sep 16 20:54:32 icm.piggy.io heat-engine[9900]: 2017-09-16 20:54:32.265 9900 TRACE heat-engine Exception: Unable to determine Heat release
Sep 16 20:54:32 icm.piggy.io heat-engine[9900]: 2017-09-16 20:54:32.265 9900 TRACE heat-engine
Sep 16 20:54:32 icm.piggy.io systemd[1]: openstack-heat-engine.service: main process exited, code=exited, status=1/FAILURE
Sep 16 20:54:32 icm.piggy.io systemd[1]: Unit openstack-heat-engine.service entered failed state.
Sep 16 20:54:32 icm.piggy.io systemd[1]: openstack-heat-engine.service failed.
```

* Retrieve heat engine version

```bash
# heat-engine --version
2015.1.4
```

* Modify IBM UCD heat extension on kilo line from ```"kilo": ("2015.1.0", "2015.1.4"),``` to ```"kilo": ("2015.1.0", "2015.1.5"),```
```bash
# cd /usr/lib/heat/ibm-cloud-ext/resources/
# vi heat_version_helper.py
```
* Original code
```
MAJOR_VERSIONS = {
    "grizzly": ("2013.1.0", "2013.1.6"),
    "havana": ("2013.2.0", "2013.2.5"),
    "icehouse": ("2014.1.0", "2014.1.5"),
    "juno": ("2014.2.0", "2014.2.5"),
    "kilo": ("2015.1.0", "2015.1.4"),
    "liberty": ("5.0.0", "6.0.0"),
    "mitaka":  ("6.0.0", "7.0.0"),
    "newton": ("7.0.0", "8.0.0"),
    "ocata": ("8.0.0", "9.0.0")
}
```
* Modified code
```
MAJOR_VERSIONS = {
    "grizzly": ("2013.1.0", "2013.1.6"),
    "havana": ("2013.2.0", "2013.2.5"),
    "icehouse": ("2014.1.0", "2014.1.5"),
    "juno": ("2014.2.0", "2014.2.5"),
    "kilo": ("2015.1.0", "2015.1.5"),
    "liberty": ("5.0.0", "6.0.0"),
    "mitaka":  ("6.0.0", "7.0.0"),
    "newton": ("7.0.0", "8.0.0"),
    "ocata": ("8.0.0", "9.0.0")
}
```

* Compile python code
```bash
# python -m py_compile heat_version_helper.py
```

* Modify IBM UCD heat extension on kilo line from ```"kilo": ("2015.1.0", "2015.1.4"),``` to ```"kilo": ("2015.1.0", "2015.1.5"),```
# cd /usr/lib/heat/ibm-sw-orch/heat
# vi heat_version_helper.py
```
* Original code
```
MAJOR_VERSIONS = {
    "grizzly": ("2013.1.0", "2013.1.6"),
    "havana": ("2013.2.0", "2013.2.5"),
    "icehouse": ("2014.1.0", "2014.1.5"),
    "juno": ("2014.2.0", "2014.2.5"),
    "kilo": ("2015.1.0", "2015.1.4"),
    "liberty": ("5.0.0", "6.0.0"),
    "mitaka":  ("6.0.0", "7.0.0"),
    "newton": ("7.0.0", "8.0.0"),
    "ocata": ("8.0.0", "9.0.0")
}
```
* Modified code
```
MAJOR_VERSIONS = {
    "grizzly": ("2013.1.0", "2013.1.6"),
    "havana": ("2013.2.0", "2013.2.5"),
    "icehouse": ("2014.1.0", "2014.1.5"),
    "juno": ("2014.2.0", "2014.2.5"),
    "kilo": ("2015.1.0", "2015.1.5"),
    "liberty": ("5.0.0", "6.0.0"),
    "mitaka":  ("6.0.0", "7.0.0"),
    "newton": ("7.0.0", "8.0.0"),
    "ocata": ("8.0.0", "9.0.0")
}
```

* Compile python code
```bash
# python -m py_compile heat_version_helper.py
```

* Start openstack-heat-engine service
```bash
# systemctl restart openstack-heat-engine
# systemctl status -l openstack-heat-engine
â openstack-heat-engine.service - Openstack Heat Engine Service
   Loaded: loaded (/usr/lib/systemd/system/openstack-heat-engine.service; enabled; vendor preset: disabled)
   Active: active (running) since Sat 2017-09-16 21:04:01 ICT; 6s ago
 Main PID: 12713 (heat-engine)
   CGroup: /system.slice/openstack-heat-engine.service
           ââ12713 /usr/bin/python /usr/bin/heat-engine --config-file /etc/heat/heat.conf --log-file /var/log/heat/engine.log
           ââ12726 /usr/bin/python /usr/bin/heat-engine --config-file /etc/heat/heat.conf --log-file /var/log/heat/engine.log
           ââ12727 /usr/bin/python /usr/bin/heat-engine --config-file /etc/heat/heat.conf --log-file /var/log/heat/engine.log

Sep 16 21:04:08 icm.piggy.io heat-engine[12713]: 2017-09-16 21:04:08.503 12726 INFO oslo_messaging._drivers.impl_rabbit [req-e87dc655-76b1-4aea-bdfe-8493b7353727 03c8ac3c6e43c6aa5044c4c2a861028ac87207d1d038b89383d8c5eced7a6731 d2b94b9f1fab4c9e983e07ef0c1e8e97] Connected to AMQP server on 172.16.6.41:5671
Sep 16 21:04:08 icm.piggy.io heat-engine[12713]: 2017-09-16 21:04:08.515 12726 INFO oslo_messaging._drivers.impl_rabbit [req-f571c600-c2d1-434f-b3a2-722d3919a082 - d2b94b9f1fab4c9e983e07ef0c1e8e97] Connecting to AMQP server on 172.16.6.41:5671
Sep 16 21:04:08 icm.piggy.io heat-engine[12713]: 2017-09-16 21:04:08.516 12727 INFO oslo_messaging._drivers.impl_rabbit [req-17e75d56-ffa9-4df9-b9ec-3d003c22d7da - d2b94b9f1fab4c9e983e07ef0c1e8e97] Connecting to AMQP server on 172.16.6.41:5671
Sep 16 21:04:08 icm.piggy.io heat-engine[12713]: 2017-09-16 21:04:08.541 12726 INFO oslo_messaging._drivers.impl_rabbit [req-e87dc655-76b1-4aea-bdfe-8493b7353727 03c8ac3c6e43c6aa5044c4c2a861028ac87207d1d038b89383d8c5eced7a6731 d2b94b9f1fab4c9e983e07ef0c1e8e97] The exchange Exchange reply_6bb9cf1dc8f4457cbebd7a28a3bd5854(direct) to send to reply_6bb9cf1dc8f4457cbebd7a28a3bd5854 doesn't exist yet, retrying...
Sep 16 21:04:08 icm.piggy.io heat-engine[12713]: 2017-09-16 21:04:08.556 12727 INFO oslo_messaging._drivers.impl_rabbit [req-17e75d56-ffa9-4df9-b9ec-3d003c22d7da - d2b94b9f1fab4c9e983e07ef0c1e8e97] Connected to AMQP server on 172.16.6.41:5671
Sep 16 21:04:08 icm.piggy.io heat-engine[12713]: 2017-09-16 21:04:08.568 12726 INFO oslo_messaging._drivers.impl_rabbit [req-f571c600-c2d1-434f-b3a2-722d3919a082 - d2b94b9f1fab4c9e983e07ef0c1e8e97] Connected to AMQP server on 172.16.6.41:5671
Sep 16 21:04:08 icm.piggy.io heat-engine[12713]: 2017-09-16 21:04:08.569 12727 INFO oslo_messaging._drivers.impl_rabbit [req-17e75d56-ffa9-4df9-b9ec-3d003c22d7da - d2b94b9f1fab4c9e983e07ef0c1e8e97] Connecting to AMQP server on 172.16.6.41:5671
Sep 16 21:04:08 icm.piggy.io heat-engine[12713]: 2017-09-16 21:04:08.588 12726 INFO oslo_messaging._drivers.impl_rabbit [req-f571c600-c2d1-434f-b3a2-722d3919a082 - d2b94b9f1fab4c9e983e07ef0c1e8e97] Connecting to AMQP server on 172.16.6.41:5671
Sep 16 21:04:08 icm.piggy.io heat-engine[12713]: 2017-09-16 21:04:08.619 12727 INFO oslo_messaging._drivers.impl_rabbit [req-17e75d56-ffa9-4df9-b9ec-3d003c22d7da - d2b94b9f1fab4c9e983e07ef0c1e8e97] Connected to AMQP server on 172.16.6.41:5671
Sep 16 21:04:08 icm.piggy.io heat-engine[12713]: 2017-09-16 21:04:08.625 12726 INFO oslo_messaging._drivers.impl_rabbit [req-f571c600-c2d1-434f-b3a2-722d3919a082 - d2b94b9f1fab4c9e983e07ef0c1e8e97] Connected to AMQP server on 172.16.6.41:5671

* Verify heat resource type
```bash
# heat resource-type-list
+------------------------------------------+
| resource_type                            |
+------------------------------------------+
| AWS::AutoScaling::AutoScalingGroup       |
| AWS::AutoScaling::LaunchConfiguration    |
| AWS::AutoScaling::ScalingPolicy          |
| AWS::CloudFormation::Stack               |
| AWS::CloudFormation::WaitCondition       |
| AWS::CloudFormation::WaitConditionHandle |
| AWS::EC2::EIP                            |
| AWS::EC2::EIPAssociation                 |
| AWS::EC2::Instance                       |
| AWS::EC2::InternetGateway                |
| AWS::EC2::NetworkInterface               |
| AWS::EC2::RouteTable                     |
| AWS::EC2::SecurityGroup                  |
| AWS::EC2::Subnet                         |
| AWS::EC2::SubnetRouteTableAssociation    |
| AWS::EC2::VPC                            |
| AWS::EC2::VPCGatewayAttachment           |
| AWS::EC2::Volume                         |
| AWS::EC2::VolumeAttachment               |
| AWS::ElasticLoadBalancing::LoadBalancer  |
| AWS::IAM::AccessKey                      |
| AWS::IAM::User                           |
| AWS::RDS::DBInstance                     |
| AWS::S3::Bucket                          |
| IBM::Azure::Disk                         |
| IBM::Azure::DiskAttachment               |
| IBM::Azure::NetworkInterface             |
| IBM::Azure::NetworkSecurityGroup         |
| IBM::Azure::PublicIP                     |
| IBM::Azure::ResourceGroup                |
| IBM::Azure::Server                       |
| IBM::Azure::StorageAccount               |
| IBM::Azure::Subnet                       |
| IBM::Azure::Template                     |
| IBM::Azure::VirtualNetwork               |
| IBM::EC2::Alarm                          |
| IBM::EC2::AutoScalingGroup               |
| IBM::EC2::AutoScalingPolicy              |
| IBM::EC2::EIP                            |
| IBM::EC2::EIPAssociation                 |
| IBM::EC2::InternetGateway                |
| IBM::EC2::LaunchConfiguration            |
| IBM::EC2::Port                           |
| IBM::EC2::RouteTable                     |
| IBM::EC2::RouteTableAssociation          |
| IBM::EC2::SecurityGroup                  |
| IBM::EC2::Server                         |
| IBM::EC2::Subnet                         |
| IBM::EC2::VPC                            |
| IBM::EC2::Volume                         |
| IBM::EC2::VolumeAttachment               |
| IBM::ElastiCache::Cluster                |
| IBM::ElastiCache::ParameterGroup         |
| IBM::ElastiCache::ReplicationGroup       |
| IBM::ElastiCache::SubnetGroup            |
| IBM::ElasticLoadBalancing::HealthCheck   |
| IBM::ElasticLoadBalancing::LoadBalancer  |
| IBM::ElasticLoadBalancing::Pool          |
| IBM::ElasticLoadBalancing::PoolMember    |
| IBM::Google::Disk                        |
| IBM::Google::DiskAttachment              |
| IBM::Google::Port                        |
| IBM::Google::PublicIP                    |
| IBM::Google::PublicIPAssociation         |
| IBM::Google::Server                      |
| IBM::RDS::Instance                       |
| IBM::RDS::ParameterGroup                 |
| IBM::RDS::SubnetGroup                    |
| IBM::Route53::HostedZone                 |
| IBM::Route53::RecordSet                  |
| IBM::S3::Bucket                          |
| IBM::SoftLayer::FloatingIP               |
| IBM::SoftLayer::FloatingIPAssociation    |
| IBM::SoftLayer::Port                     |
| IBM::SoftLayer::SecurityGroup            |
| IBM::SoftLayer::Server                   |
| IBM::UrbanCode::ApplicationProcess::UCD  |
| IBM::UrbanCode::ResourceTree             |
| IBM::UrbanCode::SoftwareConfig::UCD      |
| IBM::UrbanCode::SoftwareDeploy::UCD      |
| IBM::VCenter::FloatingIP                 |
| IBM::VCenter::FloatingIPAssociation      |
| IBM::VCenter::Network                    |
| IBM::VCenter::Port                       |
| IBM::VCenter::Router                     |
| IBM::VCenter::RouterInterface            |
| IBM::VCenter::SecurityGroup              |
| IBM::VCenter::Server                     |
| IBM::VCenter::Subnet                     |
| IBM::VCenter::Volume                     |
| IBM::VCenter::VolumeAttachment           |
| IBM::VRA::Deployment                     |
| IBM::VRA::FloatingIP                     |
| IBM::VRA::FloatingIPAssociation          |
| IBM::VRA::NetworkInterface               |
| IBM::VRA::SecurityGroup                  |
| IBM::VRA::Server                         |
| IBM::VRA::SoftwareComponent              |
| OS::Ceilometer::Alarm                    |
| OS::Ceilometer::CombinationAlarm         |
| OS::Cinder::Volume                       |
| OS::Cinder::VolumeAttachment             |
| OS::Glance::Image                        |
| OS::Heat::AccessPolicy                   |
| OS::Heat::AutoScalingGroup               |
| OS::Heat::CWLiteAlarm                    |
| OS::Heat::CloudConfig                    |
| OS::Heat::HARestarter                    |
| OS::Heat::InstanceGroup                  |
| OS::Heat::MultipartMime                  |
| OS::Heat::RandomString                   |
| OS::Heat::ResourceGroup                  |
| OS::Heat::ScalingPolicy                  |
| OS::Heat::SoftwareComponent              |
| OS::Heat::SoftwareConfig                 |
| OS::Heat::SoftwareDeployment             |
| OS::Heat::SoftwareDeployments            |
| OS::Heat::Stack                          |
| OS::Heat::StructuredConfig               |
| OS::Heat::StructuredDeployment           |
| OS::Heat::StructuredDeployments          |
| OS::Heat::SwiftSignal                    |
| OS::Heat::SwiftSignalHandle              |
| OS::Heat::UpdateWaitConditionHandle      |
| OS::Heat::WaitCondition                  |
| OS::Heat::WaitConditionHandle            |
| OS::Neutron::Firewall                    |
| OS::Neutron::FirewallPolicy              |
| OS::Neutron::FirewallRule                |
| OS::Neutron::FloatingIP                  |
| OS::Neutron::FloatingIPAssociation       |
| OS::Neutron::HealthMonitor               |
| OS::Neutron::IKEPolicy                   |
| OS::Neutron::IPsecPolicy                 |
| OS::Neutron::IPsecSiteConnection         |
| OS::Neutron::LoadBalancer                |
| OS::Neutron::MeteringLabel               |
| OS::Neutron::MeteringRule                |
| OS::Neutron::Net                         |
| OS::Neutron::NetworkGateway              |
| OS::Neutron::Pool                        |
| OS::Neutron::PoolMember                  |
| OS::Neutron::Port                        |
| OS::Neutron::ProviderNet                 |
| OS::Neutron::Router                      |
| OS::Neutron::RouterGateway               |
| OS::Neutron::RouterInterface             |
| OS::Neutron::SecurityGroup               |
| OS::Neutron::Subnet                      |
| OS::Neutron::VPNService                  |
| OS::Nova::FloatingIP                     |
| OS::Nova::FloatingIPAssociation          |
| OS::Nova::KeyPair                        |
| OS::Nova::Server                         |
| OS::Nova::ServerGroup                    |
| OS::Sahara::Cluster                      |
| OS::Sahara::ClusterTemplate              |
| OS::Sahara::NodeGroupTemplate            |
| OS::Swift::Container                     |
| OS::Trove::Cluster                       |
| OS::Trove::Instance                      |
+------------------------------------------+
```
