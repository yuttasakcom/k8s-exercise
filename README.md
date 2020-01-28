# Kubernetes Exercise

## Table of Contents

- [POD](#POD)
- [SERVICE](#SERVICE)
- [Replicaset](#Replicaset)
- [Deployment](#Deployment)
- [Namespace](#Namespace)
- [Persitante-Volume](#Persitante-Volume)

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

- kubectl exec -it {pod-name} sh
  - nslookup database

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mysql
  labels:
    app: mysql
spec:
  containers:
    - name: mysql
      image: mysql:5
      env:
        - name: MYSQL_ROOT_PASSWORD
          value: password
        - name: MYSQL_DATABASE
          value: fleetman
---
kind: Service
apiVersion: v1
metadata:
  name: database
spec:
  selector:
    app: mysql
  ports:
    - port: 3306
  type: ClusterIP
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

## Persitante-Volume

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongodb
spec:
  selector:
    matchLabels:
      app: mongodb
  replicas: 1
  template:
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
        - name: mongodb
          image: mongo:3.6.5-jessie
          volumeMounts:
            - name: mongo-persistent-storage
              mountPath: /data/db
      volumes:
        - name: mongo-persistent-storage
          hostPath:
            path: /mnt/some/directory/structure/
            type: DirectoryOrCreate
---
apiVersion: v1
kind: Service
metadata:
  name: service-mongodb
spec:
  selector:
    app: mongodb
  ports:
    - name: mongoport
      port: 27017
  type: ClusterIP
```
