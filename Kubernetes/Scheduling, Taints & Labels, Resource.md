# Overview

### Labels & Selectors
| Concept        | What it is            | Used by                                   |
| -------------- | --------------------- | ----------------------------------------- |
| **Label**      | Key–value metadata    | Identification for nodes / pods etc.      |
| **Selector**   | Query on labels (AND) | Select pods for replicaset, services etc. |
| **Annotation** | Arbitrary metadata    | Info, pass config to controllers          |
### Node Selector / Affinity, Taints & Tolerations

| Mechanism                 | Applied To | What It Does                     |
| ------------------------- | ---------- | -------------------------------- |
| NodeSelector              | Pod        | Pod **must** run on labeled node |
| Node Affinity (required)  | Pod        | Advanced nodeSelector            |
| Node Affinity (preferred) | Pod        | Prefer certain nodes             |
| Taints                    | Node       | **Repel** Pods                   |
| Tolerations               | Pod        | **Allow** Pod onto tainted node  |

### Taint Effects

| Effect             | Behavior             |
| ------------------ | -------------------- |
| `NoSchedule`       | New Pods blocked     |
| `PreferNoSchedule` | Avoid if possible    |
| `NoExecute`        | Evicts existing Pods |

**Node Affinity and Tolerations example:**

```
spec:
  nodeSelector:
    role: db
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: disktype
            operator: In
            values:
            - ssd
	  preferredDuringSchedulingIgnoredDuringExecution:
		- weight: 1
		  preference:
		    matchExpressions:
		    - key: zone
		      operator: In
		      values: [east]
  tolerations:
  - key: role
    operator: Equal
    value: db
    effect: NoSchedule
```

## Requests & Limits
### Requests vs Limits

| Field        | Purpose                                     | Used By               | Mechanism                                          |
| ------------ | ------------------------------------------- | --------------------- | -------------------------------------------------- |
| **requests** | Guarantees minimum resources for scheduling | **Scheduler**         | Scheduler looks for nodes with sufficient requests |
| **limits**   | Caps maximum resources for usage            | **kubelet / runtime** | Throttles or kills containers exceeding limits     |
**Node allocatable ≥ sum(Pod requests)**
### Setting Requests and Limits

```yaml
resources:
  requests:
    cpu: "250m"
    memory: "256Mi"
  limits:
    cpu: "500m"
    memory: "512Mi"
```
## PriorityClass & Preemption

**PriorityClass** determines scheduling order and **preemption** if resources are insufficient
- **Higher `value` = Greater priority**
- Only one **PriorityClass** can be global: `globalDefault: true`

| preemptionPolicy       | Meaning                                         |
| ---------------------- | ----------------------------------------------- |
| `PreemptLowerPriority` | Pod may evict lower-priority Pods (**default**) |
| `Never`                | Pod will NOT evict others                       |
## Scheduler Policy & Multiple Schedulers

**What Scheduler does:**
- Chooses a node for a Pod based on constraints and preferences.
- Queue → Filter (hard constraints) → Score (preferences) → Reserve → Bind
- `default-scheduler` handles all pods unless specified otherwise by `schedulerName`

## Admission Controllers

**What Admission Controllers does:**
-  Admission Controllers enforce cluster rules at the API boundary.
- They can add / modify fields to make objects conform to rules
- Authentication → Authorization → Admission Controllers → etcd → Scheduler- 
### Quality of Service (QoS) classes
| QoS Class      | Criteria (for all containers in pod)                                                                         | Eviction Priority             |
| -------------- | ------------------------------------------------------------------------------------------------------------ | ----------------------------- |
| **Guaranteed** | Requests must equal limits for both CPU and memory.                                                          | Lowest (Last to be evicted)   |
| **Burstable**  | At least one container has a memory or CPU request/limit set, but the criteria for `Guaranteed` are not met. | Medium                        |
| **BestEffort** | No resource requests or limits are set for any container.                                                    | Highest (First to be evicted) |
