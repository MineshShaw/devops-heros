# Kubernetes Architecture Guide

This document provides a comprehensive overview of the Kubernetes architecture based on the official [Kubernetes Documentation](https://kubernetes.io/docs/concepts/architecture/).

Kubernetes is a portable, extensible, open-source platform for managing containerized workloads and services, that facilitates both declarative configuration and automation. When you deploy Kubernetes, you get a **cluster**. 

A Kubernetes cluster consists of a set of worker machines, called **nodes**, that run containerized applications. Every cluster has at least one worker node. The worker node(s) host the **Pods** that are the components of the application workload. The **control plane** manages the worker nodes and the Pods in the cluster.

---

## 🏗️ High-Level Architecture

At a macro level, Kubernetes follows a client-server architecture:
1.  **Control Plane (The Master):** The brain of the cluster. It makes global decisions about the cluster (e.g., scheduling), and detects and responds to cluster events.
2.  **Nodes (The Workers):** The machines (VMs or physical servers) where your workloads (containers/pods) actually run.

---

## 🧠 The Control Plane Components

The control plane components make global decisions about the cluster, as well as detecting and responding to cluster events (for example, starting up a new pod when a deployment's replicas field is unsatisfied).

Control plane components can be run on any machine in the cluster. However, for simplicity, set up scripts typically start all control plane components on the same machine, and do not run user containers on this machine.

### 1. `kube-apiserver`
The API server is a component of the Kubernetes control plane that exposes the Kubernetes API. It is the front end for the Kubernetes control plane. 
*   **Function:** Validates and configures data for the api objects (pods, services, replication controllers). It services REST operations and provides the frontend to the cluster's shared state through which all other components interact.
*   **Scale:** Designed to scale horizontally (by deploying more instances).

### 2. `etcd`
Consistent and highly-available key value store used as Kubernetes' backing store for all cluster data.
*   **Function:** Stores the entire configuration and state of the cluster. If your Kubernetes cluster uses `etcd` as its backing store, make sure you have a backup plan for those data.

### 3. `kube-scheduler`
Control plane component that watches for newly created Pods with no assigned node, and selects a node for them to run on.
*   **Function:** Factors taken into account for scheduling decisions include: individual and collective resource requirements, hardware/software/policy constraints, affinity and anti-affinity specifications, data locality, inter-workload interference, and deadlines.

### 4. `kube-controller-manager`
Control plane component that runs controller processes. Logically, each controller is a separate process, but to reduce complexity, they are all compiled into a single binary and run in a single process.
*   **Controllers include:**
    *   **Node controller:** Responsible for noticing and responding when nodes go down.
    *   **Job controller:** Watches for Job objects that represent one-off tasks, then creates Pods to run those tasks to completion.
    *   **EndpointSlice controller:** Populates EndpointSlice objects (to provide a link between Services and Pods).
    *   **ServiceAccount controller:** Create default ServiceAccounts for new namespaces.

### 5. `cloud-controller-manager`
A Kubernetes control plane component that embeds cloud-specific control logic. The cloud controller manager lets you link your cluster into your cloud provider's API, and separates out the components that interact with that cloud platform from components that only interact with your cluster.
*   **Function:** Only runs controllers that are specific to your cloud provider (e.g., setting up cloud load balancers, node routing).

---

## 💻 Node Components

Node components run on every node, maintaining running pods and providing the Kubernetes runtime environment.

### 1. `kubelet`
An agent that runs on each node in the cluster. It makes sure that containers are running in a Pod.
*   **Function:** The kubelet takes a set of PodSpecs that are provided through various mechanisms and ensures that the containers described in those PodSpecs are running and healthy. The kubelet doesn't manage containers which were not created by Kubernetes.

### 2. `kube-proxy`
`kube-proxy` is a network proxy that runs on each node in your cluster, implementing part of the Kubernetes Service concept.
*   **Function:** Maintains network rules on nodes. These network rules allow network communication to your Pods from network sessions inside or outside of your cluster. It uses the operating system packet filtering layer if there is one and it's available. Otherwise, `kube-proxy` forwards the traffic itself.

### 3. Container Runtime
The container runtime is the software that is responsible for running containers.
*   **Function:** Kubernetes supports several container runtimes: **containerd**, **CRI-O**, and any other implementation of the Kubernetes CRI (Container Runtime Interface).
