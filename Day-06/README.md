Day 06 Documentation

## Different ways of creating a Kubernetes object
- Imperative way ( Through command or API calls)
- Declarative way ( By creating manifest files)

![image](https://github.com/user-attachments/assets/e01b9bba-2b03-42ae-83b1-7b4cdddaa4fe)

## Below is the sample command for imperative way:
```
kubectl run -d -p 80:80 mynginx --image=nginx
```

## Below is the sample pod YAML for declerative way:

```YAML
# This is a sample pod yaml

apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    env: demo
    type: frontend
spec:
  containers:
  - name: nginx-container
    image: nginx
    ports:
    - containerPort: 80
```
