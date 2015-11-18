# AWS Docker Swarm

CloudFormation template to build a CoreOS-based Docker swarm on AWS.

## Parameters

The CloudFormation template takes the following parameters:

| Parameter | Description |
|-----------|-------------|
| InstanceType | EC2 HVM instance type (t2.micro, m3.medium, etc). |
| ClusterSize | Number of nodes in the CoreOS cluster (3-12). |
| DiscoveryURL | A unique etcd cluster discovery URL. Grab a new token from https://discovery.etcd.io/new?size=4 |
| AdvertisedIPAddress | Use 'private' if your etcd cluster is within one region or 'public' if it spans regions or cloud providers. |
| AllowSSHFrom | The net block (CIDR) that SSH and docker are available to (default is Eduserv BRM). |
| KeyName | The name of an EC2 Key Pair to allow SSH access to the instance. |
| VpcAvailabilityZones | Comma-delimited list of three VPC availability zones in which to create subnets |

- The template builds a single master node and a cluster of between 3 and 12 nodes.
- All nodes form part of an initial etcd cluster for node discovery during bootstrapping.
- Nodes are distributed across 3 availability zones.

## Outputs

| Output | Description |
+--------+-------------+
| MasterDockerPs | Command to run a 'docker ps' on the cluster master |
| MasterPublicIP | Public IP for the cluster master |
| MasterPrivateIP | Private IP for the cluster master |

