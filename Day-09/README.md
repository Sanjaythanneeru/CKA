Day 09 documentation

### What is a Namespace in Kubernetes

- Provides isolation of resources
- Avoid accidental deletion/modification
- Separated by resource type or environment or domain and so on
- Resources can access each other in the same namespace with their first name by the DNS name for other namespaces

## Connectivity between resources

- By default we can access any pod from any pod with IP Address
- But we cannot access the pods with the names

![image](https://github.com/user-attachments/assets/3d95a595-2355-4fee-b1fd-95595558398a)

## DNS Resolution

- With the service we can access the pods with the service names in the same namespcae
- But we cannot access the pods with the service names in other namespace directly for that we have to use FQDN (Fully Qualified Donmain Name)
- FQDN = \<service-name>.\<namespace-name>.svc.cluster.local"

![image](https://github.com/user-attachments/assets/48a45bd9-9fef-4ed7-9c50-c3e03ab8c087)
