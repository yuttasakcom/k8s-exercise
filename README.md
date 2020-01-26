# Kubernetes Exercise

## Table of Contents

- [POD](#POD)

## POD

- kubectl apply -f 1.pod.yaml
- kubectl get all
- kubectl delete -f 1.pod.yaml
- kubectl describe pod <pod-name>

```yaml
apiVersion: 'v1'
kind: Pod
metadata:
  name: webapp
spec:
  containers:
    - name: webapp
      image: jmalloc/echo-server
```
