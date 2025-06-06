## Kubernetes Commands

### List Resources
- `kubectl cluster-info --context kind-<name>` &mdash; details cluster context
- `kind get clusters` &mdash; List all clusters
- `kubectl get pods` &mdash; List all pods
- `kubectl get replicasets` &mdash; List all ReplicaSets
- `kubectl describe pod <name>` &mdash; details pod

### Create Resources
- `kind create cluster --config=k8s/kind.yaml --name=<name>` &mdash; Create a cluster using a config file
- `kubectl apply -f k8s/pod.yaml` &mdash; Create a pod from a YAML file
- `kubectl apply -f k8s/replicaset.yaml` &mdash; Create a ReplicaSet from a YAML file

### Delete Resources
- `kubectl delete pod <name>` &mdash; Delete a pod by name
- `kubectl delete replicasets <name>` &mdash; Delete a replicaset by name

### Rollout & Revision
- `kubectl rollout history <type of object> <name>` 
- `kubectl rollout undo <type of object> <name>` &mdash; rollback the last version in running
- `kubectl rollout undo <type of object> <name> --to-revision=<number of revision>` &mdash; rollback the specific version