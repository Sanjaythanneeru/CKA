Day 07 documentation

## Cheatsheet for Kubernetes commands:
```
https://kubernetes.io/docs/reference/kubectl/quick-reference/
```
## Replicaset
```
https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/
```
![image](https://github.com/user-attachments/assets/32dd27f9-097a-4627-a286-a98111297d12)

## Sample Replicaset template
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: frontend
  labels:
    app: guestbook
    tier: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      tier: frontend #should match with the pod labels
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: my-nginx
        image: nginx
```
## Deployment
```
https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
```
![image](https://github.com/user-attachments/assets/fb352bc6-1632-4ebc-8c09-5a35f5b60cd6)

## Sample Deployment template
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  labels:
    app: guestbook
    tier: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      tier: frontend #should match with the pod labels
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: my-nginx
        image: nginx
```
## Replication controller
```
https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/
```
