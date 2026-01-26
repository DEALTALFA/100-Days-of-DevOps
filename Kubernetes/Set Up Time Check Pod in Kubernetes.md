The Nautilus DevOps team needs a time check pod created in a specific Kubernetes namespace for logging purposes. Initially, it's for testing, but it may be integrated into an existing cluster later. Here's what's required:


Create a pod called time-check in the nautilus namespace. The pod should contain a container named time-check, utilizing the busybox image with the latest tag (specify as busybox:latest).

Create a config map named time-config with the data TIME_FREQ=9 in the same namespace.

Configure the time-check container to execute the command: while true; do date; sleep $TIME_FREQ;done. Ensure the result is written /opt/itadmin/time/time-check.log. Also, add an environmental variable TIME_FREQ in the container, fetching its value from the config map TIME_FREQ key.

Create a volume log-volume and mount it at /opt/itadmin/time within the container.



```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: time-config
  namespace: nautilus
data:
  TIME_FREQ: "9"

---

apiVersion: v1
kind: Pod
metadata:
  name: time-check
  namespace: nautilus
  labels:
    app: time-check
spec:
  volumes:
    - name: log-volume
      emptyDir: {}
  containers:
    - name: time-check
      image: busybox:latest
      volumeMounts:
        - mountPath: /opt/itadmin/time/
          name: log-volume
      envFrom:
        - configMapRef:
            name: time-config
      command: ["/bin/sh", "-c"]
      args:
        [
          "while true; do date; sleep $TIME_FREQ;done > /opt/itadmin/time/time-check.log",
        ]

```

    

Another way of doing

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: nautilus
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: time-config
  namespace: nautilus
data:
    TIME_FREQ: "9"
---
apiVersion: v1
kind: Pod
metadata:
  name: time-check
  namespace: nautilus
spec:
    containers:
    - name: time-check
        image: busybox:latest
        command: ["/bin/sh", "-c", "while true; do date >> /opt/itadmin/time/time-check.log; sleep $TIME_FREQ; done"]
        env:
        - name: TIME_FREQ
        valueFrom:
            configMapKeyRef:
            name: time-config
            key: TIME_FREQ
        volumeMounts:
        - name: log-volume
        mountPath: /opt/itadmin/time
    volumes:
    - name: log-volume
        emptyDir: {}

```


# Approach


| Method | Syntax | Use Case |
|------|-------------------|------------------|
|envFrom (Your 1st example)|configMapRef: name: time-config|The "Bulk" approach. It grabs every key in the ConfigMap and turns them into environment variables.|
|valueFrom (Your 2nd example)|configMapKeyRef: name: time-config, key: TIME_FREQ|The "Surgical" approach. It maps one specific key to a specific environment variable name you choose.
