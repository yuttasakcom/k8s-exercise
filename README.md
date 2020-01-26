# Kubernetes Exercise

## Table of Contents

- [POD](#POD)
- [SERVICE](#SERVICE)

## POD

- kubectl apply -f 1.pod.yaml
- kubectl get all
- kubectl delete -f 1.pod.yaml
- kubectl describe pod {pod-name}

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webapp
  labels:
    app: webapp
spec:
  containers:
    - name: webapp
      image: jmalloc/echo-server
```

## SERVICE

- kubectl describe svc service-webapp

```yaml
kind: Service
apiVersion: v1
metadata:
  name: service-webapp
spec:
  selector:
    app: webapp
  ports:
    - name: http
      port: 8080
      nodePort: 30080
  type: NodePort
```
