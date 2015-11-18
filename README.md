# AWS Docker cluster using Swarm

A CloudFormation template to build a CoreOS-based Docker swarm on AWS.

## Parameters

The CloudFormation template takes the following parameters:

| Parameter | Description |
|-----------|-------------|
| InstanceType | EC2 HVM instance type (t2.micro, m3.medium, etc.) for the master and node hosts. |
| ClusterSize | Number of nodes in the Swarm cluster (3-12). |
| DiscoveryURL | A unique etcd cluster discovery URL. Grab a new token from https://discovery.etcd.io/new?size=4 |
| AdvertisedIPAddress | Use 'private' if your etcd cluster is within one region or 'public' if it spans regions or cloud providers. |
| AllowSSHFrom | The net block (CIDR) from which SSH and docker on the Swarm master are available. |
| KeyName | The name of an EC2 Key Pair to allow SSH access to the Swarm master. |
| VpcAvailabilityZones | Comma-delimited list of three VPC availability zones in which to create subnets |

The template builds a new VPC with 3 subnets (in 3 availability zones)
and a single Swarm master and a cluster of between 3 and 12 nodes,
using the standard AWS CoreOS AMI.

All hosts form part of an initial etcd cluster for node
discovery during bootstrapping.
Therefore the size option on the discovery.etcd.io URL must be one more than
the size of the CoreOS cluster.

Hosts are evenly distributed across the 3 availability zones and are created
within an Auto-Scaling Group which can be manually adjusted to alter
the Swarm cluster size post-launch.

A 'docker-swarm' container is run on each host (the Swarm master and the nodes).
All hosts listen on port 4243, leaving the standard docker port (2375)
free for use by the 'docker-swarm' container on the master.

## Outputs

| Output | Description |
|--------|-------------|
| MasterDockerPs | Command to run a 'docker ps' on the Swarm master |
| MasterPublicIP | Public IP for the cluster master |
| MasterPrivateIP | Private IP for the cluster master |
