# Imperative Commands
### Pods & Workloads

| Task                                    | Imperative Command                                               |
| --------------------------------------- | ---------------------------------------------------------------- |
| Create Pod                              | `kubectl run nginx --image=nginx -- sleep 3600`                  |
| Create Pod (YAML only)                  | `kubectl run nginx --image=nginx --dry-run=client -o yaml`       |
| Exec into Pod                           | `kubectl exec -it nginx -- /bin/sh`                              |
| Get Pod logs                            | `kubectl logs nginx`                                             |
| Describe Pod                            | `kubectl describe pod nginx`                                     |
| Delete Pod                              | `kubectl delete pod nginx`                                       |
| Create Daemonset <br>(one pod per node) | Follow documentation (create from YAML)                          |
| Create Static Pod                       | Write to `/etc/kubernetes/manifests` (kubelet will provision it) |
| Inspect Logs                            | `crictl ps -a` then `crictl logs <container-id>`                 |
| Authenticated actions                   | `kubectl auth can-i --list`                                      |
### Deployments & Rollouts

```yaml
strategy:
  type: RollingUpdate / Recreate
  rollingUpdate:
    maxUnavailable: 0
    maxSurge: 1
```

| Task                                                    | Imperative Command                                                     |
| ------------------------------------------------------- | ---------------------------------------------------------------------- |
| Create Deployment                                       | `kubectl create deployment web --image=nginx`                          |
| Deployment YAML only                                    | `kubectl create deployment web --image=nginx --dry-run=client -o yaml` |
| Scale Deployment                                        | `kubectl scale deployment web --replicas=5`                            |
| Horizontal Pod Autoscaler (requires requests to be set) | `kubectl autoscale deployment web --min=2 --max=10 --cpu-percent=80`   |
| In-place Pod Resizing                                   | `resizePolicy` under `containers`                                      |
| Update image                                            | `kubectl set image deploy/web nginx=nginx:1.25`                        |
| Rollout management                                      | `kubectl rollout status/history/pause/resume deploy/web`               |
| Rollback to revision                                    | `kubectl rollout undo deploy/web --to-revision=2`                      |
## Scheduling & Preemption

| Task                          | Imperative Command                              |
| ----------------------------- | ----------------------------------------------- |
|                               |                                                 |
|                               |                                                 |
| Create Priority Class         |                                                 |
| Assign Priority Class to Pod  |                                                 |
| Enable Admission Controllers  | `--enable-admission-plugins` in manifests file  |
| Disable Admission Controllers | `--disable-admission-plugins` in manifests file |

### Services & Networking

| Task                     | Imperative Command                                                          |
| ------------------------ | --------------------------------------------------------------------------- |
| Create ClusterIP service | `kubectl create service clusterip web --tcp=80:80`                          |
| Create NodePort service  | `kubectl create service nodeport web --tcp=80:80`                           |
| Service YAML only        | `kubectl create service clusterip web --tcp=80:80 --dry-run=client -o yaml` |
| Expose Pod               | `kubectl expose pod nginx (--type=NodePort) --port=80`                      |
| Expose Deployment        | `kubectl expose deploy web --port=80 --target-port=80`                      |

| Field        | Belongs To | What It Means                              | Default Behavior   |
| ------------ | ---------- | ------------------------------------------ | ------------------ |
| `port`       | Service    | Port exposed by the **Service**            | **Required**       |
| `targetPort` | Pod        | Port on the **Pod** traffic is sent to     | Defaults to `port` |
| `nodePort`   | Node       | Port on the **Node** traffic is exposed on | Not required       |
### Scheduling, Taints & Labels

| Task             | Imperative Command                                                |
| ---------------- | ----------------------------------------------------------------- |
| Cordon node      | `kubectl cordon node01`                                           |
| Drain node       | `kubectl drain node01 --ignore-daemonsets --delete-emptydir-data` |
| Uncordon node    | `kubectl uncordon node01`                                         |
| Show node labels | `kubectl get nodes --show-labels`                                 |
| Remove label     | `kubectl label node node01 disktype-`                             |
| Taint node       | `kubectl taint node node01 key=value:NoSchedule`                  |
| Remove taint     | `kubectl taint node node01 key=value:NoSchedule-`                 |

### Jobs & CronJobs

| Task           | Imperative Command                                                                    |
| -------------- | ------------------------------------------------------------------------------------- |
| Create Job     | `kubectl create job pi --image=perl -- perl 'print bpi(2000)'`                        |
| Job YAML only  | `kubectl create job pi --image=perl --dry-run=client -o yaml`                         |
| Create CronJob | `kubectl create cronjob hello --image=busybox --schedule="*/1 * * * *" -- echo hello` |
### ConfigMaps & Secrets

| Task                       | Imperative Command                                                |
| -------------------------- | ----------------------------------------------------------------- |
| Create ConfigMap (literal) | `kubectl create configmap app-config --from-literal=env=prod`     |
| Create ConfigMap (file)    | `kubectl create configmap app-config --from-file=app.conf`        |
| Create Secret (generic)    | `kubectl create secret generic db-secret --from-literal=pwd=1234` |
| Create Secret (tls)        | `kubectl create secret tls tls --cert=tls.cert --key=tls.key`     |
### Namespace & Context

| Task                        | Imperative Command                                     |
| --------------------------- | ------------------------------------------------------ |
| Create Namespace            | `kubectl create ns dev`                                |
| Set namespace (current ctx) | `kubectl config set-context --current --namespace=dev` |
| Switch context              | `kubectl config use-context ctx-name`                  |
### RBAC

| Task                      | Imperative Command                                                                       |
| ------------------------- | ---------------------------------------------------------------------------------------- |
| Create ServiceAccount     | `kubectl create sa app-sa`                                                               |
| Create Role               | `kubectl create role pod-reader --verb=get,list --resource=pods`                         |
| Create RoleBinding        | `kubectl create rolebinding read-pods --role=pod-reader --serviceaccount=default:app-sa` |
| Create ClusterRole        | `kubectl create clusterrole node-reader --verb=get,list --resource=nodes`                |
| Create ClusterRoleBinding | `kubectl create clusterrolebinding read-nodes --clusterrole=node-reader --user=john`     |

### Cluster Upgrade
| Task                      | Imperative Command     |
| ------------------------- | ---------------------- |
| Get Node versions         | `kubectl get nodes`    |
| Check upgradable versions | `kubeadm upgrade plan` |
|                           |                        |