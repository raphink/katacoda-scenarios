Start by launching the Kubernetes cluster with `launch.sh`{{execute}}.

Verify that the cluster is up by listing the nodes with `kubectl get nodes`{{execute}}


Now let's launch K9s using Docker:

```
docker run -ti \
  -v ~/.kube/config:/root/.kube/config \
  quay.io/derailed/k9s
```{{execute}}


We can list the cluster nodes by typing `:nodes`{{execute}}.
