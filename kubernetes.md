# Kubernetes

## What is Kubernetes
Kubernetes is an open source portable, extensible, container orchestration system for managing containerized workloads and services, that facilitates both declarative configuration and automation.

## When not to use K8?
* Can be overkill if you do not need high availability
* If your app is monolothic as K8 breaks down into smaller services
* It is difficult to learn and master so may take up too much time and resources
* Expensive as there are a lot of resources required as a minimum

## When not to use microservices
* When the project is small and does not need to be broke up into smaller services
* Can become overcomplicated in a small projects
* It is expensive as there are more resoruces and more maintainence

## How does it work?
It orchestartes containerized applications to run on a cluster of hosts. K8 automates the deployment and management of applications using infrastructure.

It works by creating pods and services. A pod is a group of one or more containers that are dedployed together and share a network and IP. Services allow the pods to be exposed to the outside world.

## Benefits
* Self healing
* Faster deployment as everything needed is in one container
* Automation of deployment
* Fault tolerance - K8 automatically identifies errors and can take down faield nodes and replace them with healthy ones
* More efficient due to automation
* Multi-cloud capability - you can spread your workload
* Cheaper than alternatives
* Runs applications with greater stability
* Scalibility - easily scale your application by changing the amount of pods you have depending on the demand

## Who is using it?
* Google - they created it
* Spotify - Kubernetes also allows engineering teams to get a host to run new services in production much faster than they were previously able to.
* Adidas - A mix of Kubernetes running on AWS and Prometheus cut the Adidas website load time in half and allowed the team to release improvements 10,000% faster—from once every 4-6 weeks to 3-4 times per day.
* Airbnb - Airbnb uses Kubernetes to run the hundreds of services they use to operate on a unified and scalable infrastructure including multiple clusters and thousands of nodes.
* The New York Times - The New York Times runs GKE on the Google Cloud Platform (GCP) primarily to increase deployment speed.
* Capital One - Capital One first shifted to Kubernetes to increase the resilience and speed of fraud detection and credit decision systems that worked on millions of daily transactions.

## Installing K8 on Docker
1. First we needed to install K8 on Docker
2. Go to seetings on Docker, click Kubernetes from the left and select enable Kubernetes
3. Then select apply and restart

## Deploying Nginx using K8
1. We need to make a YAML file using ```nginx_deploy.yml```
2. Then enter in the below
```
apiVersion: apps/v1 # which api to use for deployment

kind: Deployment # what kind of service/object do you want to create?

# what would you like to call it - name the service/object


metadata:
  name: nginx-deployment
spec:
  selector: 
    matchLabels:
      app: nginx
  replicas: 3

  template:
    metadata:
      labels:
        app: nginx
    
    spec: 
      containers:
      - name: nginx
        image: billy5549/tech221-app:v1
        ports:
        - containerPort: 80
```
3. For the indentations you must use 2 spaces instead of tab
4. Then in a terminal go to the location of the file and run the following
5. To create the deployment ```kubectl  create -f nginx_deploy.yml```
6. Use ```kubectl get deployment``` to check if the pods are ready
7. If they are all marked as 3/3 then they are all ready
8. Use ```kubectl get pods``` to see the status of each pod
9. If they are all marked as 1/1 then they are ready
10. Use ```kubectl delete deploy nginx-deployment``` to destroy the deployment

## How to deploy the Sparta app
1. Create two new YAML files called ```app_deploy.yml``` and ```app_service.yml```
2. In ```app_deploy.yml``` put the below in
```
apiVersion: apps/v1 # which api to use for deployment

kind: Deployment # what kind of service/object do you want to create?

# what would you like to call it - name the service/object


metadata:
  name: app-deployment
spec:
  selector: 
    matchLabels:
      app: node
  replicas: 3

  template:
    metadata:
      labels:
        app: node
    
    spec: 
      containers:
      - name: node
        image: billy5549/tech221-app:v1
        ports:
        - containerPort: 80
```
3. The only changes we have made is to change ```app``` and ```name``` to ```node```
4. Then in ```app_service.yml``` put the below
```
---
# select the type of API version and type of service
apiVersion: v1
kind: Service

# metadata for name
metadata:
  name: app-svc
  namespace: default # sre

# specification to include poirts selector to connect to the
spec: 
  ports:
  - nodePort: 30002 # range is 30000 - 32768
    port: 3000

    targetPort: 3000

# define the selector and label to connect to node
  selector:
    app: node

# creating a nodeport type of deployment
  type: NodePort # also use loadbalancer - for local use cluster
  ```
  5. For this one we changed the name to app-svc, the ```nodePort``` to ```30002``` as ```30001``` is still engaged with nginx and we changed the port to ```3000```
  6. Run them both by using ```kubectl create -f app_deploy.yml``` and ```kubectl create -f app_service.yml```
  7. Then go to your browser and enter in ```localhost:30002``` to see the Sparta app

  ## Volumes, persistent volumes and persisten volume

  ## What are they?
  * A docker volume is an independent file system entirely managed by docker and exists as a normal file or directory on the host, where data is persisted
  * A PersistentVolume (PV) is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes.
  * A PersistentVolumeClaim (PVC) is a request for storage by a user. It is similar to a Pod however Pods consume node resources and PVCs consume PV resources.

## Deploying MongoDB
1. Create two new files called ```mongo_deploy.yml``` and ```mongo_service.yml```
2. in ```mongo_deploy.yml``` put in the below
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo
spec:
  selector:
    matchLabels:
      app: mongo
  replicas: 1
  template:
    metadata:
      labels:
        app: mongo
    spec:
      containers:
        - name: mongo
          image: mongo
          ports:
            - containerPort: 27017
          volumeMounts:
            - name: storage
              mountPath: /data/db
      volumes:
        - name: storage
          persistentVolumeClaim:
            claimName: mongo-db
```
3. In ```mongo_service.yml``` enter in the below
```
---
# select the type of API version and type of service
apiVersion: v1
kind: Service

# metadata for name
metadata:
  name: mongo-svc
  namespace: default # sre

# specification to include poirts selector to connect to the
spec: 
  ports:
  - nodePort: 30004 # range is 30000 - 32768
    port: 27017

    targetPort: 27017

# define the selector and label to connect to nginx
  selector:
    app: mongo

# creating a nodeport type of deployment
  type: NodePort # also use loadbalancer - for local use cluster
```
4. Then create another new file called ```pvc_mongo.yml``` and enter in the below
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mongo-db
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 256Mi
```
5. Then we need to add an environemnt variable to our ```app-deploy.yml``. This is the updated version below
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-deployment
spec:
  selector:
    matchLabels:
      app: node
  replicas: 1
  template: 
    metadata:
      labels:
        app: node
    spec:
      containers:
        - name: node
          image: billy5549/tech221-app:v1
          
          ports:
            - containerPort: 3000
          env:
            - name: DB_HOST
              value: mongodb://10.104.186.173 :27017/posts
          
          imagePullPolicy: Always

          resources:
            limits:
              memory: 512Mi
              cpu: "1"
            requests:
              memory: 256Mi
              cpu: "0.2"
```
6. We also need to make an autoscaling group using ```asg_mongo.yml``` and enter in the below
```
---
# select the type of API version and type of service
apiVersion: v1
kind: Service

# metadata for name
metadata:
  name: mongo-svc
  namespace: default # sre

# specification to include poirts selector to connect to the
spec: 
  ports:
  - nodePort: 30003 # range is 30000 - 32768
    port: 27017

    targetPort: 27017

# define the selector and label to connect to nginx
  selector:
    app: mongo

# creating a nodeport type of deployment
  type: NodePort # also use loadbalancer - for local use cluster
```
7. Then launch them all by using ```docker create -f 'name of the file'```
8. Go into a browser and enter ```localhost:30004/posts``` to see the Sparta posts page