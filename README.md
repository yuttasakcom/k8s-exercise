# Kubernetes Exercise

## Table of Contents

- [POD](POD)

## POD

- kubectl apply -f 1.pod.yaml
- kubectl get all

```yaml
apiVersion: 'v1'
kind: Pod
metadata:
  name: webapp
spec:
  containers:
    - name: webapp
      image: yuttasakcom/node-echoserver
```
