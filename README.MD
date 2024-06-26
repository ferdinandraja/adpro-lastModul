### Ferdinand Raja Kenedy - 2206046683
## Reflection on Hello Minikube

### 1. Compare the application logs before and after you exposed it as a Service. Try to open the app several times while the proxy into the Service is running. What do you see in the logs? Does the number of logs increase each time you open the app?
<img src = "img/ss1.png">
Every time I open the app, it triggers a log consist of GET method of HTTP protocol. It increase 2 times every time I open the app, this happen because there is a direct user interaction with the app.

### 2. Notice that there are two versions of `kubectl get` invocation during this tutorial section. The first does not have any option, while the latter has `-n` option with value set to `kube-system`. What is the purpose of the `-n` option and why did the output not list the pods/services that you explicitly created?
The -n option in kubectl get helps us specify which group of resources we want to see. When we add -n followed by a specific area, like kube-system, it only shows resources from that area. If we didn't see the pods or services we created when we ran kubectl get without -n, it's likely because they were made in a different area than the default one, so we'd need to specify that area using -n to see them.

## Reflection on Rolling Update & Kubernetes Manifest File
### 1. What is the difference between Rolling Update and Recreate deployment strategy?
Rolling update is a deployment strategy that updates a deployment by replacing the old pods with new pods. It does this by creating new pods and then deleting
Recreate deployment strategy that for temporary, brings down the entire service, replacing all pods simultaneously, which can lead to noticeable downtime.
 ### 2. & 3. Deployment using Recreate
 <img src = "img/ss2.png">
 The manifest file:

 ```
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":{"deployment.kubernetes.io/revision":"4"},"creationTimestamp":"2024-05-14T06:34:29Z","generation":6,"labels":{"app":"spring-petclinic-rest"},"name":"spring-petclinic-rest","namespace":"default","resourceVersion":"5181","uid":"0e6e41f6-bf46-4d4f-a4ed-82f1ca59b27e"},"spec":{"progressDeadlineSeconds":600,"replicas":4,"revisionHistoryLimit":10,"selector":{"matchLabels":{"app":"spring-petclinic-rest"}},"strategy":{"type":"Recreate"},"template":{"metadata":{"creationTimestamp":null,"labels":{"app":"spring-petclinic-rest"}},"spec":{"containers":[{"image":"docker.io/springcommunity/spring-petclinic-rest:3.2.1","imagePullPolicy":"IfNotPresent","name":"spring-petclinic-rest","resources":{},"terminationMessagePath":"/dev/termination-log","terminationMessagePolicy":"File"}],"dnsPolicy":"ClusterFirst","restartPolicy":"Always","schedulerName":"default-scheduler","securityContext":{},"terminationGracePeriodSeconds":30}}},"status":{"conditions":[{"lastTransitionTime":"2024-05-14T06:34:29Z","lastUpdateTime":"2024-05-14T07:03:39Z","message":"ReplicaSet \"spring-petclinic-rest-54f476f68\" has successfully progressed.","reason":"NewReplicaSetAvailable","status":"True","type":"Progressing"},{"lastTransitionTime":"2024-05-14T07:14:04Z","lastUpdateTime":"2024-05-14T07:14:04Z","message":"Deployment does not have minimum availability.","reason":"MinimumReplicasUnavailable","status":"False","type":"Available"}],"observedGeneration":6,"replicas":4,"unavailableReplicas":4,"updatedReplicas":4}}
  creationTimestamp: "2024-05-14T12:57:25Z"
  generation: 1
  labels:
    app: spring-petclinic-rest
  name: spring-petclinic-rest
  namespace: default
  resourceVersion: "15734"
  uid: b7ddb571-2ea7-4511-a8b0-c2b7e5de196a
spec:
  progressDeadlineSeconds: 600
  replicas: 4
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: spring-petclinic-rest
  strategy:
    type: Recreate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: spring-petclinic-rest
    spec:
      containers:
      - image: docker.io/springcommunity/spring-petclinic-rest:3.2.1
        imagePullPolicy: IfNotPresent
        name: spring-petclinic-rest
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  conditions:
  - lastTransitionTime: "2024-05-14T12:57:25Z"
    lastUpdateTime: "2024-05-14T12:57:25Z"
    message: Deployment does not have minimum availability.
    reason: MinimumReplicasUnavailable
    status: "False"
    type: Available
  - lastTransitionTime: "2024-05-14T12:57:25Z"
    lastUpdateTime: "2024-05-14T12:57:26Z"
    message: ReplicaSet "spring-petclinic-rest-54f476f68" is progressing.
    reason: ReplicaSetUpdated
    status: "True"
    type: Progressing
  observedGeneration: 1
  replicas: 4
  unavailableReplicas: 4
  updatedReplicas: 4
 ```
 ### 4. What do you think are the benefits of using Kubernetes manifest files? Recall your experience in deploying the app manually and compare it to your experience when deploying the same app by applying the manifest files (i.e., invoking `kubectl apply -f` command) to the cluster.
 Using Kubernetes manifest files offers several benefits compared to manual deployment. These files provide a declarative configuration, describing the desired state of Kubernetes resources, which enhances reproducibility and reduces errors. Version control enables tracking changes over time, facilitating collaboration and allowing for easy rollbacks if needed. Automation through tools like `kubectl apply` streamlines the deployment process, ensuring consistency and reducing manual intervention.
