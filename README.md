# Kubernetes Exercise

## Table of Contents

- [POD](#POD)
- [SERVICE](#SERVICE)
- [Replicaset](#Replicaset)
- [Deployment](#Deployment)

## POD

- kubectl apply -f 1.pod.yaml
- kubectl get all
- kubectl delete -f 1.pod.yaml
- kubectl describe pod {pod-name}
- kubectl get po --show-labels

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

## Replicaset

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: webapp
spec:
  selector:
    matchLabels:
      app: webapp
  replicas: 2
  template:
    metadata:
      labels:
        app: webapp
    spec:
      containers:
        - name: webapp
          image: richardchesterwood/k8s-fleetman-webapp-angular:release0-5
---
apiVersion: v1
kind: Service
metadata:
  name: fleetman-webapp
spec:
  selector:
    app: webapp
  ports:
    - name: http
      port: 80
      nodePort: 30080
  type: NodePort
```

## Deployment

- kubectl rollout history deploy webapp
- kubectl rollout status deploy webapp
- kubectl rollout undo deploy webapp

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapp
spec:
  minReadySeconds: 30
  selector:
    matchLabels:
      app: webapp
  replicas: 2
  template:
    metadata:
      labels:
        app: webapp
    spec:
      containers:
        - name: webapp
          image: richardchesterwood/k8s-fleetman-webapp-angular:release0
---
apiVersion: v1
kind: Service
metadata:
  name: fleetman-webapp
spec:
  selector:
    app: webapp
  ports:
    - name: http
      port: 80
      nodePort: 30080
  type: NodePort
```

## Namespace

- kubectl get namespace
- kubectl get po -n kube-system

```

```
