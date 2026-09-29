# Kubernetes Networking: FQDN and CoreDNS

## Task 3: FQDN (Fully Qualified Domain Name)

*   **What is FQDN?** An FQDN specifies the exact, complete domain name of a specific host or service within a DNS hierarchy.
*   **Kubernetes Service DNS:** Kubernetes automatically assigns DNS records to Services and Pods, allowing applications to find each other by name instead of IP addresses.
*   **Kubernetes DNS Naming Convention:** 
    *   Services: `<service-name>.<namespace>.svc.cluster.local`
    *   Pods: `<pod-ip-address>.<namespace>.pod.cluster.local`
*   **Namespace-based DNS:** 
    *   If two Pods are in the *same* namespace, they can communicate using just the `<service-name>`.
    *   If they are in *different* namespaces, the requesting Pod must use the FQDN: `<service-name>.<namespace>.svc.cluster.local`.
*   **Pod-to-Service Communication:** A Pod's `/etc/resolv.conf` is configured to point to the cluster's internal DNS server. When a Pod calls a Service by name, the DNS server resolves it to the Service's ClusterIP.
*   **Examples of Kubernetes FQDNs:**
    *   `db-service.prod.svc.cluster.local` (Service in `prod` namespace)
    *   `frontend.default.svc.cluster.local` (Service in `default` namespace)

---

## Task 4: CoreDNS

*   **What is CoreDNS?** CoreDNS is a flexible, extensible DNS server written in Go that serves as the default cluster DNS for Kubernetes.
*   **Why Kubernetes uses CoreDNS:** It is highly performant, memory-efficient, and supports dynamic plugin configurations. It replaced the older `kube-dns` to provide better reliability and customization.
*   **How Service Discovery Works:** CoreDNS watches the Kubernetes API for new, updated, or deleted Services and Endpoints. It dynamically creates and updates DNS records based on this real-time cluster state.
*   **How DNS Queries are Resolved:** 
    1. A Pod makes a DNS query.
    2. The query goes to the CoreDNS Service IP (usually `10.96.0.10`).
    3. CoreDNS checks if the query matches a cluster local domain (`cluster.local`). 
    4. If yes, it returns the internal IP. If no, it forwards the request to an upstream/external DNS server (like `8.8.8.8`).
*   **CoreDNS Configuration:** Configured via a ConfigMap named `coredns` in the `kube-system` namespace. The configuration uses a file called the `Corefile` where plugins (like `kubernetes`, `forward`, `cache`, `errors`) are defined.
*   **How to Troubleshoot DNS Issues:**
    1.  **Test Resolution:** Run a temporary utility Pod (e.g., `busybox` or `dnsutils`) and use `nslookup <service-name>` or `dig <fqdn>`.
    2.  **Check Endpoints:** Ensure the target Service actually has endpoints running (`kubectl get endpoints <service-name>`).
    3.  **Check CoreDNS Pods:** Ensure CoreDNS pods are running (`kubectl get pods -n kube-system -l k8s-app=kube-dns`).
    4.  **Inspect Logs:** View CoreDNS logs for errors (`kubectl logs -n kube-system -l k8s-app=kube-dns`).