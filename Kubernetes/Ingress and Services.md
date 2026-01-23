# Overview
**Ingress = L7 (HTTP/HTTPS) routing _into_ Services.**  
**Service = L4 (TCP/UDP) routing _inside_ the cluster.**

## Services

![[Kubernetes/diagrams/Diagram.svg]]

| Service Type   | ClusterIP (default)                            | NodePort                                           | LoadBalancer                                      |
| -------------- | ---------------------------------------------- | -------------------------------------------------- | ------------------------------------------------- |
| What It Does   | Exposes service on a virtual IP inside cluster | Exposes service on each Node’s IP at a static port | Provisions external LB via cloud provider         |
| Reachable From | Pods inside cluster only                       | NodeIP:NodePort                                    | External clients                                  |
| When to Use    | Internal microservices, backends, DBs          | Simple external access, ports 30000–32767 only     | Production external traffic                       |
| Traffic Flow   | Pod → ClusterIP → Pod<br>                      | Client → NodeIP:NodePort → ClusterIP → Pod<br>     | Client → External LB → NodePort → ClusterIP → Pod |
## Ingress

**Required parts**
- Ingress resource (rules: host/path → Service)
- Ingress controller (e.g., nginx/traefik)
  No controller ⇒ no traffic.
