We are working on an application that will be deployed on multiple containers within a pod on Kubernetes cluster. There is a requirement to share a volume among the containers to save some temporary data. The Nautilus DevOps team is developing a similar template to replicate the scenario. Below you can find more details about it.



Create a pod named volume-share-nautilus.


For the first container, use image debian with latest tag only and remember to mention the tag i.e debian:latest, container should be named as volume-container-nautilus-1, and run a sleep command for it so that it remains in running state. Volume volume-share should be mounted at path /tmp/beta.


For the second container, use image debian with the latest tag only and remember to mention the tag i.e debian:latest, container should be named as volume-container-nautilus-2, and again run a sleep command for it so that it remains in running state. Volume volume-share should be mounted at path /tmp/cluster.


Volume name should be volume-share of type emptyDir.


After creating the pod, exec into the first container i.e volume-container-nautilus-1, and just for testing create a file beta.txt with any content under the mounted path of first container i.e /tmp/beta.


The file beta.txt should be present under the mounted path /tmp/cluster on the second container volume-container-nautilus-2 as well, since they are using a shared volume.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-nautilus
spec:
    containers:
    - name: volume-container-nautilus-1
        image: debian:latest
        command: ["sleep", "infinity"]
        volumeMounts:
        - name: volume-share
          mountPath: /tmp/beta
    - name: volume-container-nautilus-2
        image: debian:latest
        command: ["sleep", "infinity"]
        volumeMounts:
        - name: volume-share
          mountPath: /tmp/cluster
    volumes:
    - name: volume-share
      emptyDir: {}
    ```

```bash
# Step 1: Create the pod using the above YAML configuration
kubectl apply -f volume-share-nautilus.yaml
```
```bash
# Step 2: Wait for the pod to be in Running state
kubectl wait --for=condition=Ready pod/volume-share-nautilus
```
```bash
# Step 3: Exec into the first container and create a file beta.txt with some content
kubectl exec -it volume-share-nautilus -c volume-container-nautilus-1 -- bash -c "echo 'This is a shared volume test' > /tmp/beta/beta.txt"
```
```bash
# Step 4: Verify that the file beta.txt is present in the second container's mounted path
kubectl exec -it volume-share-nautilus -c volume-container-nautilus-2 -- cat /tmp/cluster/beta.txt
```
```bash
# Note: The content of beta.txt should be displayed, confirming that the shared volume is working correctly.
```
```bash
# End of the steps for creating a pod with shared volumes in Kubernetes.
```
