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
## Sidecar containers
- Sidecar containers are the secondary containers that run along with the main application container within the same Pod. These containers are used to enhance or to extend the functionality of the primary app container by providing additional services, or functionality such as logging, monitoring, security, or data synchronization, without directly altering the primary application code
  
![image](https://github.com/user-attachments/assets/7aca6908-b6b8-494d-9c6f-ac704bb484eb)

- Kubernetes implements sidecar containers as a special case of init containers; sidecar containers remain running after Pod startup
- If an init container is created with its restartPolicy set to Always, it will start and remain running during the entire life of the Pod.

Example pod manifest with Sidecar
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
  labels:
    app: myapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: myapp
          image: alpine:latest
          command: ['sh', '-c', 'while true; do echo "logging" >> /opt/logs.txt; sleep 1; done']
          volumeMounts:
            - name: data
              mountPath: /opt
      initContainers:
        - name: logshipper
          image: alpine:latest
          restartPolicy: Always
          command: ['sh', '-c', 'tail -F /opt/logs.txt']
          volumeMounts:
            - name: data
              mountPath: /opt
      volumes:
        - name: data
          emptyDir: {}
```
