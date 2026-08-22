The Nautilus DevOps team is planning to set up a Jenkins CI server to create/manage some deployment pipelines for some of the projects. They want to set up the Jenkins server on Kubernetes cluster. Below you can find more details about the task:


1) Create a namespace jenkins

2) Create a Service for jenkins deployment. Service name should be jenkins-service under jenkins namespace, type should be NodePort, nodePort should be 30008

3) Create a Jenkins Deployment under jenkins namespace, It should be name as jenkins-deployment , labels app should be jenkins , container name should be jenkins-container , use jenkins/jenkins image , containerPort should be 8080 and replicas count should be 1. It should also have the environment variable JAVA_OPTS with the value -Djenkins.install.runSetupWizard=false to skip the initial setup wizard.

```yml
# Step 1: Create a YAML file for the Jenkins namespace
apiVersion: v1
kind: Namespace
metadata:
  name: jenkins

# Step 2: Create a YAML file for the Jenkins Service
apiVersion: v1
kind: Service
metadata:
  name: jenkins-service
  namespace: jenkins
spec:
    type: NodePort
    selector:
        app: jenkins
    ports:
        - protocol: TCP
        port: 8080
        targetPort: 8080
        nodePort: 30008

# Step 3: Create a YAML file for the Jenkins Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: jenkins-deployment
  namespace: jenkins
  labels:
    app: jenkins
spec:
    replicas: 1
    selector:
        matchLabels:
            app: jenkins
    template:
        metadata:
            labels:
                app: jenkins
        spec:
            containers:
            - name: jenkins-container
              image: jenkins/jenkins
              ports:
              - containerPort: 8080
              env:
              - name: JAVA_OPTS
                value: "-Djenkins.install.runSetupWizard=false"
```