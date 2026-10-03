# AWS VPC - Networking

Amazon Virtual Private Cloud (VPC) is a logically isolated network in AWS. It defines IP address ranges, subnets, routing, and network controls for resources such as EC2, RDS, and load balancers.

## Core concepts

### CIDR

CIDR notation expresses an IP network as an address and prefix length, such as `10.0.0.0/16`. The prefix determines the number of addresses: a `/16` is larger than a `/24`. Plan non-overlapping ranges for VPCs, on-premises networks, and future peering or Transit Gateway connections.

AWS reserves five IPv4 addresses in every subnet: the network address, router address, DNS address, and the first and last address in the subnet range. IPv6 subnets use `/64` ranges.

### Subnets

A subnet is a range of IP addresses inside one Availability Zone. A subnet is public or private based on its route table, not its name:

- A **public subnet** has a route to an Internet Gateway.
- A **private subnet** has no direct route to an Internet Gateway; it may use a NAT Gateway for outbound IPv4 access.

Use multiple Availability Zones for resilience. Common layouts place load balancers in public subnets and application and database tiers in private subnets.

### Route tables

A route table contains destination CIDR blocks and targets. Each subnet is associated with one route table, explicitly or through the main route table. The most specific matching route wins.

Typical routes include the local VPC route, a default route (`0.0.0.0/0`) to an Internet Gateway or NAT Gateway, and routes to VPN, Direct Connect, Transit Gateway, or VPC peering connections. A route only defines a path; security groups and network ACLs still determine whether traffic is allowed.

### Internet Gateway

An Internet Gateway (IGW) is a horizontally scaled VPC component that enables internet communication for resources with public addressing when the subnet route table sends traffic to it. Attach one IGW to a VPC, add a route to it, and ensure security controls permit the traffic.

An IGW does not automatically make every resource public. The resource needs a public or Elastic IP (for IPv4) and a route to the IGW.

### NAT Gateway

A NAT Gateway allows resources in private subnets to initiate outbound IPv4 connections to the internet without accepting unsolicited inbound internet connections. It is deployed in a public subnet and uses an Elastic IP; private subnet route tables point their default route to it.

For high availability, deploy a NAT Gateway per Availability Zone and route each private subnet to the local gateway. NAT Gateways incur hourly and data-processing charges. For AWS service access, consider VPC endpoints to avoid unnecessary internet paths and NAT costs.

### Security groups

Security groups are stateful, instance-level or network-interface-level firewalls. They contain allow rules only, and response traffic is automatically permitted. Reference another security group instead of hard-coding changing private IP addresses when defining tier-to-tier access.

### Network ACLs

A network ACL (NACL) is a stateless, subnet-level firewall. It has numbered inbound and outbound allow or deny rules, and both directions must allow a connection. Rules are evaluated in number order; the first match wins.

The default NACL allows all traffic. Custom NACLs can provide coarse subnet guardrails, but they must allow ephemeral return ports and are easier to misconfigure than security groups. Use them as defense in depth, not as a substitute for precise security-group rules.

## Example architecture

```text
Internet
   |
Internet Gateway
   |
Public subnets: load balancer, NAT Gateway
   |
Private application subnets: EC2 / containers
   |
Private data subnets: RDS / internal services
```

## Design checklist

1. Plan non-overlapping CIDR ranges and reserve space for growth.
2. Spread critical subnets and resources across at least two Availability Zones.
3. Keep databases and internal services in private subnets.
4. Use security groups for least-privilege workload access.
5. Use NACLs only for intentional subnet-level controls.
6. Prefer VPC endpoints for private access to supported AWS services.
7. Enable flow logs and monitor route, DNS, and security-control changes.

## Further reading

- [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/)
- [VPC routing](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)
- [VPC security groups and network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html)

