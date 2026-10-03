# AWS EC2 - Compute

Amazon Elastic Compute Cloud (EC2) provides resizable virtual servers, called **instances**, in AWS. You choose an operating system image, hardware profile, network placement, storage, and access controls, then pay for the capacity you use.

## Core concepts

### AMIs

An Amazon Machine Image (AMI) is a template used to launch an instance. It includes the operating system, software, configuration, and references to the root storage volumes. AWS publishes images such as Amazon Linux; organizations can create custom AMIs with approved software and hardening.

Choose an AMI for the correct architecture (`x86_64` or `arm64`), Region, boot mode, and operating system support period. Treat AMIs as versioned, immutable build artifacts and patch or rebuild them regularly.

### Instance types

An instance type defines virtual CPUs, memory, network performance, and sometimes local storage or accelerators. Families are optimized for different workloads:

| Family category | Typical use |
| --- | --- |
| General purpose (`t`, `m`) | Web servers and balanced applications |
| Compute optimized (`c`) | CPU-heavy services and batch processing |
| Memory optimized (`r`, `x`, `z`) | In-memory databases and analytics |
| Storage optimized (`i`, `d`) | High-throughput or low-latency local storage |
| Accelerated computing (`g`, `p`, `inf`) | GPU, ML, or specialized acceleration |

Select a size based on measured CPU, memory, network, and I/O requirements. Burstable `t` instances use CPU credits and are not interchangeable with continuously high-CPU instances.

### Key pairs

A key pair is a public/private cryptographic key pair used for operating-system login, commonly SSH on Linux. AWS stores the public key; the private key is downloaded once when the pair is created.

Protect the private key with restrictive file permissions and never put it in a repository. Use Systems Manager Session Manager where possible to avoid inbound SSH and long-lived key management. Windows instances use the private key to decrypt the initial administrator password.

### Security groups

A security group is a stateful virtual firewall attached to an instance network interface. Rules allow inbound or outbound traffic; there are no deny rules. Return traffic is automatically allowed for an established connection.

Use separate security groups by role, such as a load balancer group, application group, and database group. Permit only required ports from known security groups or CIDR ranges, and avoid exposing administrative ports to `0.0.0.0/0`.

### EBS

Amazon Elastic Block Store (EBS) provides persistent block volumes for EC2. Volumes can be encrypted, snapshotted, resized, and attached within their Availability Zone. Common volume types include general-purpose SSD (`gp3`), provisioned-IOPS SSD (`io2`), and throughput-optimized HDD (`st1`).

Snapshots are incremental backups stored in Amazon S3-managed infrastructure. Plan capacity, IOPS, throughput, encryption keys, snapshot retention, and recovery testing. Instance store is different: it is temporary local storage and data is lost when the instance is stopped, terminated, or the underlying host fails.

### Public and private IP addresses

- A **private IPv4 address** is used inside the VPC and normally remains associated with the network interface while the instance is running.
- A **public IPv4 address** provides internet reachability through an Internet Gateway and can change when the instance is stopped and started.
- An **Elastic IP address** is a static public IPv4 address that can be associated with a network interface, subject to AWS limits and charges.

Prefer private addresses for application communication. Put public-facing access behind a load balancer or bastion alternative and use private subnets for internal services.

### Instance lifecycle

Typical states are:

`pending` → `running` → `stopping` / `stopped` → `pending` / `running`, or `terminating` → `terminated`.

Stopping and starting generally preserves EBS volumes but may assign a new public IP. Terminating deletes the root EBS volume by default when `DeleteOnTermination` is enabled and cannot be undone. Rebooting keeps the instance on its host and usually preserves networking. Configure termination protection and backups for important workloads.

## Common use cases

- Hosting web servers, APIs, and background workers.
- Running self-managed databases or software that needs OS-level control.
- Performing batch jobs, build workloads, and scientific computing.
- Hosting container workloads when managed container services are not suitable.
- Running development environments, jump hosts, or specialized appliances.

## Operational checklist

Use IAM instance profiles instead of embedded credentials, patch the operating system, encrypt EBS volumes, send logs and metrics to monitoring services, limit security-group exposure, and use Auto Scaling and multiple Availability Zones for highly available services.

## Further reading

- [Amazon EC2 User Guide](https://docs.aws.amazon.com/ec2/)
- [Amazon EC2 instance types](https://aws.amazon.com/ec2/instance-types/)
- [Amazon EBS User Guide](https://docs.aws.amazon.com/ebs/)

