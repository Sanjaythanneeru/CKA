Day 10 Documentation

## Multi-Container Pods

## Init Containers
- init containers: specialized containers that run before app containers in a Pod.
- Init containers are exactly like regular containers, except:
    - Init containers always run to completion.
    - Each init container must complete successfully before the next one starts.
- Init containers run and complete their tasks before the main application container starts. Unlike sidecar containers, init containers are not continuously running alongside the main containers.

![image](https://github.com/user-attachments/assets/81200a60-298c-4a16-982e-16954c6e8670)

## Demo on Init containers

**Yaml for init containers**
```
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app.kubernetes.io/name: MyApp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox:1.28
    command: ['sh', '-c', "until nslookup myservice.$(cat /var/run/secrets/kubernetes.io/serviceaccount/namespace).svc.cluster.local; do echo waiting for myservice; sleep 2; done"]
  - name: init-mydb
    image: busybox:1.28
    command: ['sh', '-c', "until nslookup mydb.$(cat /var/run/secrets/kubernetes.io/serviceaccount/namespace).svc.cluster.local; do echo waiting for mydb; sleep 2; done"]
```
1. You can start this Pod by running:
```
kubectl apply -f myapp.yaml
```
2. And check on its status with:
```
kubectl get -f myapp.yaml
```
3. To see logs for the init containers in this Pod, run:
```
kubectl logs myapp-pod -c init-myservice # Inspect the first init container
kubectl logs myapp-pod -c init-mydb      # Inspect the second init container
```
At this point, those init containers will be waiting to discover Services named mydb and myservice
Here's a configuration you can use to make those Services appear:
```
---
apiVersion: v1
kind: Service
metadata:
  name: myservice
spec:
  ports:
  - protocol: TCP
    port: 80
    targetPort: 9376
---
apiVersion: v1
kind: Service
metadata:
  name: mydb
spec:
  ports:
  - protocol: TCP
    port: 80
    targetPort: 9377
```
4. To create the mydb and myservice services:
```
kubectl apply -f services.yaml
```
5. Now you can check the status of the Pod
```
kubectl get -f myapp.yaml
```
