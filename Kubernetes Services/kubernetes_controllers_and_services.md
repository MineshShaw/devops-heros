# Kubernetes Workloads and Networking

This document outlines key differences and responsibilities of Kubernetes controllers and networking components.

## Deployment vs ReplicaSet

*   **Purpose:** A **ReplicaSet** ensures a specified number of pod replicas are running at any given time. A **Deployment** is a higher-level abstraction that provides declarative updates for Pods and ReplicaSets.
*   **Pod Management:** ReplicaSets manage Pods directly based on label selectors. Deployments manage ReplicaSets, which in turn manage the Pods.
*   **Scaling:** Both support scaling, but it is best practice to scale the Deployment, which propagates the changes down to the ReplicaSet.
*   **Rolling Updates:** Deployments support rolling updates, rollbacks, and versioning natively. ReplicaSets do not. 
*   **Relationship:** A Deployment creates and owns a ReplicaSet. When you update a Deployment, it creates a new ReplicaSet and scales it up while scaling the old one down.

## Deployment vs DaemonSet vs StatefulSet

| Feature | Deployment | DaemonSet | StatefulSet |
| :--- | :--- | :--- | :--- |
| **Use Cases** | Stateless applications, web servers. | Node-level agents (monitoring, logging). | Stateful applications, databases. |
| **Pod Creation** | Randomly generated names. | One pod per node. | Predictable, ordered names (e.g., `web-0`, `web-1`). |
| **Scaling** | Scales by adjusting replica count. | Scales automatically as nodes are added/removed. | Scales sequentially and gracefully. |
| **Networking** | Uses standard ClusterIP/NodePort Services. | Often uses HostNetwork or standard services. | Requires a Headless Service for stable network identities. |
| **Storage** | Ephemeral or shared persistent storage. | Local node storage (HostPath). | Dedicated PersistentVolumeClaim (PVC) per pod. |
| **Examples** | Nginx frontend, Node.js API. | Fluentd, Prometheus Node Exporter. | MySQL, MongoDB, Kafka. |

## ReplicaSet vs Service

*   **ReplicaSet Responsibility:** Guarantees the availability and desired count of Pods. If a Pod crashes, the ReplicaSet spins up a new one.
*   **Service Responsibility:** Provides a stable network endpoint (IP and DNS name) and load balances traffic across a dynamic set of Pods.
*   **Why a Service is Required:** Pods are ephemeral; their IP addresses change whenever they are recreated. A Service abstracts this away, ensuring clients don't need to track individual Pod IPs.
*   **How Traffic Reaches Pods:** A client requests the Service's stable IP/DNS. `kube-proxy` (via iptables or IPVS) intercepts this request and routes it to the IP of an available, ready Pod matching the Service's label selector.