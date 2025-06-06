## Kubernetes Commands

### List Resources
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
