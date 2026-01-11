# Overview

![[Architecture.svg]]
# Components
### Controlplane

**Kubernetes API Server**
Central point of communication from all users, developers, components
- Ensures all requests conform to k8s schema and authorization policies
- **Only point** of connection to **etcd**

**Scheduler**
Schedules pods onto worker nodes
- Resource requirements (volumes etc)
- Affinitiy/anti-affinity rules
- Taints/tolerance rules on nodes
- Optimize resource distribution and utilization

**Controller Managers**
Collection of controller processes
- Ensures actual state matches desired state
- Watches API server for changes and take action to reconcile
- You can ship your own controller
- E.g. node, replicaset, deployment, service controllers

**ETCD**
Distributed key-value store
- Stores configuration data and cluster's current state
- Highly available and fault tolerant

### Worker Pods

**Kubelet**
Heavy-Lifter Agent that manages pods
- Accepts and sends API calls for Pod specifications
- Ensure access to Secrets/ConfigMaps on creation
- Mounts volumes to Pod, instructs container runtime

**Kube Proxy**
Network proxy
- Route network traffic to the appropriate destination
- Enables communication between services and pods

**Container Runtime**
Underlying container infrastructure (e.g. Containerd)
- Pull container images from registries
- Manages container lifecycle operations