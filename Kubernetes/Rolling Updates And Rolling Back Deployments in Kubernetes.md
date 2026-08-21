There is a production deployment planned for next week. The Nautilus DevOps team wants to test the deployment update and rollback on Dev environment first so that they can identify the risks in advance. Below you can find more details about the plan they want to execute.



Create a namespace devops. Create a deployment called httpd-deploy under this new namespace, It should have one container called httpd, use httpd:2.4.25 image and 4 replicas. The deployment should use RollingUpdate strategy with maxSurge=1, and maxUnavailable=2. Also create a NodePort type service named httpd-service and expose the deployment on nodePort: 30008.


Now upgrade the deployment to version httpd:2.4.43 using a rolling update.


Finally, once all pods are updated undo the recent update and roll back to the previous/original version.


Note:

a. The kubectl utility on the jump-host has been configured to work with the Kubernetes cluster.


b. Please make sure you only use the specified image(s) for this deployment and as per the sequence mentioned in the task description. If you mistakenly use a wrong image and fix it later, that will also distort the revision history which can eventually fail this task.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd-deploy
  namespace: devops
spec:
  replicas: 4
  selector:
    matchLabels:
      app: httpd
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 2
  template:
    metadata:
      labels:
        app: httpd
    spec:
      containers:
      - name: httpd
          image: httpd:2.4.25
          ports:
          - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: httpd-service
  namespace: devops
spec:
  type: NodePort
  selector:
    app: httpd
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30008

```
```bash
kubectl set image deployments httpd-deploy -n devops httpd=httpd:2.4.43
k rollout undo deployment httpd-deploy --to-revision=1
```