### Resource Lifecycle

`kubectl apply` → API server → Authentication & Authorization → Admission Controller → `etcd`

| **Component**                  | **What happens**                               |
| ------------------------------ | ---------------------------------------------- |
| `kubectl` / argocd             | Declarative config                             |
| API Server                     | Single point of contact, validates schema      |
| Authentication & Authorization | Verifies identity (cert, token), RBAC policies |
| Admission Controller           | Mutate (Linkerd adds proxy sidecar) → Validate |
| Persist to etcd                | Single source of truth of desired state        |
**Controller state reconciliation**

| **Step**      | **What the controller does**                         |
| ------------- | ---------------------------------------------------- |
| Watch         | Watches resources via API server (golang event-loop) |
| Compare       | Compares `spec` (desired) vs `status` (actual)       |
| Act           | Perform imperative reconciliation efforts            |
| Update Status | Writes to `status` field                             |
| Repeat        | Loops forever                                        |
### Scheduling

**Mechanism**

| **Step** | **What the controller does**                                      |
| -------- | ----------------------------------------------------------------- |
| Queue    | Add unscheduled pods to internal scheduling queue                 |
| Filter   | Filter hard constraints i.e. NodeSelector, Taints and Tolerations |
| Score    | Score by soft constraints (affinity), topology and resources      |
| Bind     | Writes to `spec.nodeName` field                                   |
| Kubelet  | Picks up `spec.nodeName` and starts the pod                       |
**Preemption**
If resources are insufficient, scheduler considers preemption by 
1. **Priority Class:** set `preemptionPolicy` to `PreemptLowerPriority`
2. **Quality of Service**

| QoS Class      | Criteria (for all containers in pod)                                                 | Eviction Priority |
| -------------- | ------------------------------------------------------------------------------------ | ----------------- |
| **Guaranteed** | Requests must equal limits for both CPU and memory.                                  | Lowest            |
| **Burstable**  | At least one container has a memory or CPU request/limit set, but requests != limits | Medium            |
| **BestEffort** | No resource requests or limits are set for any container.                            | Highest           |
## Controller vs Operator

**Simple answer:** CRD + domain knowledge + controller
My personal requirement for declarative state management is stricter 

| **Citizen type**      | **Examples**                        | **Intuition**                                                                                                                                                                                                |
| --------------------- | ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| First-class citizens  | Pod, Deployment, Service, ConfigMap | Creating resources take same steps, final behavior is 100% predictable, no hidden mechanism                                                                                                                  |
| Second-class citizens | StatefulSet                         | Unit of StatefulSet still remains predictable, but internal mechanism depends on history and order of pods created                                                                                           |
| Third-party citizens  | Operators (Postgres, zoo, etc.)     | On the surface, it exposes CRDs and declarative config. Behind the scenes, it takes imperative steps laced with domain knowledge. Each deployment differs procedurally, final behavior not fully predictable |

> ZooKeeper Operator elects node 1 as leader; after node 1 goes down and node 2 becomes leader, Kafka still read the old leader address on node 1 and fail—even though the Kubernetes configuration hasn’t changed.

> Crossplane operator: managing AWS resources which during creation, behavior is not 100% predictable, use Terraform not K8s (faster feedback in CI, more coupled)
 
> However, Operators still encapsulate third-party business logic that are hard to dispute on

## Networking

