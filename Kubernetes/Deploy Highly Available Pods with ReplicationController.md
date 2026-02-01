The Nautilus DevOps team is establishing a ReplicationController to deploy multiple pods for hosting applications that require a highly available infrastructure. Follow the specifications below to create the ReplicationController:


Create a ReplicationController using the nginx image with latest tag, and name it nginx-replicationcontroller.

Assign labels app as nginx_app, and type as front-end. Ensure the container is named nginx-container and set the replica count to 3.


All pods should be running state post-deployment.
```bash
# Step 1: Create a YAML file for the ReplicationController
cat <<EOF > nginx-replicationcontroller.yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx-replicationcontroller
spec:
    replicas: 3
    selector:
        app: nginx_app
        type: front-end
    template:
        metadata:
            labels:
                app: nginx_app
                type: front-end
        spec:
            containers:
            - name: nginx-container
              image: nginx:latest
              ports:
              - containerPort: 80
EOF
```
```bash
# Step 2: Apply the YAML file to create the ReplicationController
kubectl apply -f nginx-replicationcontroller.yaml
```
```bash
# Step 3: Verify that the ReplicationController is created and pods are running
kubectl get rc nginx-replicationcontroller
kubectl get pods -l app=nginx_app,type=front-end
```
```bash
# Note: Ensure that you have the necessary permissions to create resources in the Kubernetes cluster.
```
```bash
# Additional Note: Make sure that the Kubernetes cluster is up and running before applying the YAML file.
```
```bash
# Additional Note: You can use kubectl describe rc nginx-replicationcontroller to get more details about the ReplicationController.
```
```bash
# Additional Note: If the pods are not in Running state, check the pod logs using kubectl logs <pod-name> for troubleshooting.
```
```bash
# End of the steps for creating the ReplicationController.
```
```bash
# Remember to replace placeholders with actual values as needed.
```
```bash
# Additional Note: You can scale the ReplicationController later using kubectl scale rc nginx-replicationcontroller --replicas=<desired-count>
```
```bash
# Additional Note: Consider setting up monitoring for the pods to ensure high availability.
```
```bash
# Additional Note: If you need to update the ReplicationController, modify the YAML file and reapply it using kubectl apply -f nginx-replicationcontroller.yaml
```