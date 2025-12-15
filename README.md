# Kubernetes Notes 
## Definition of Kubernetes
* Open source container orchestration tool.
* Developed by Google.
* Helps manage containers(or docker containers)/ containerised applications that are made up of hundreds or maybe thousands of containers.
* Helps you manage them in different environments like physical or virtual machines or even cloud technologies/ hybrid deployment environments.

## What problems does Kubernetes solve?



## What are the tasks of an orchestration tool?
* Trend from Monolith to Microservices.
* Increased usage of containers
* Demand for a proper way of managing those hundreds of containers
  
## What features do orchestration tools offer?
* High Availability or no downtime - always accessible by users
* Scalability or high performance - application has high performance meaning it loads fast and users have a very high response rate
* Disaster recovery - backup and restore
  
## Kubernetes Basic Architecture
* Made up of one master node and then connected to a couple of worker nodes.
* Each node has a kubelet (primary "node agent") process running on it and cubelet is actually a kubernetes process that makes it possible for the cluster to communicate to each other and carry out some tasks.  
* Each node has different docker applications deployed on it.
* Worker nodes are where the applications are running.
* The Master node carries out important Kubernetes processes running.
* One of these processes is the "API server" which is also a container.
* The "API server is an entrypoint to the kubernetes cluster. This is the process that different Kubernetes clusters will talk to. For example, UI, API, CLI.
* Another process running on the Master node is the "Controller Manager".
* The "Controller Manager" keeps track of what's happening in the cluster.
* Another process is the "Scheduler". This ensures Pods placement.
* Entire cluster is an etcd which is a key-value storage which holds the current state of the kubernetes cluster.
* The virtual network enables these nodes to talk to each other. It helps to create one powerful machine that has the sum of all the resources of individual nodes.
* Worker nodes are more bigger and have more resources due to the amount of workload they have and the hundreds of containers they will be running. 
* Master nodes doesn't carry out many processes compared to worker nodes but is much more important as if you end up loosing it, you won't be able to have access to the cluster anymore. In production, having an extra Master node is ideal so that you will have a backup for the cluster without completely loosing it.
* If you loose a worker node, you may be able to have another one created for you.

## Kubernetes Basic Concepts
* Pods are the smallest unit that we will use in Kubernetes.
* Is a wrapper of a container in each worker node.
* Each worker node has multiple pods and inside each pod, you can have multiple containers.
* Each application would have one pod. However, you could possibly have more than one pod inside a container when you have a main application that needs some helper containers.
* There will be one pod per application such as one pod for the database, one pod for the message broker, one pod for a server and so on.
* A node.js or Java application will also have one separate pod. 
* The virtual network controls the Kubernetes cluster that assigns each pod it's own IP address so each pod is its own self-contained server.
* The way they can communicate within each other is by using the interal IP addresses.
* We only work with the pods within the Kubernetes cluster which is an abstraction layer over containers and pod is a component of Kubernetes that manages the containers running by itself.
* Pods and the containers inside a pod are recreated frequently when they stop working. 
* When a pod stops working and a new one gets created, the new pod gets a new IP address on creation.
* When such a situation takes place, it gets very inconvenient in changing the IP address again and again. In order to substitute for this, another component is used named "Service". 
* This basically is an alternative to the IP addresses and so ensures that each pod can be communicated with. 
* Services have 2 main functionalities. One is a permanent IP address which can be used to communicate between the pods and at the same time is a load balancer.

## Kubernetes Configuration
* All configuration in the Kubernetes cluster goes through the Master node with the process called "API server".
* A Kubernetes' client could be the UI, Kubernetes Dashboard or an API which could be a script or a curl command or a command line tool (cube CTL), they all talk to the API server and they send their configuration requests to the API server which is the main entry point or the only entry point into the cluster.
* The requests should either be in YAML or JSON format. 
* Deployment = a template for creating pods.
* Configuration should be "declarative"  to declare what the desired outcome is.
* Is == Should.
* Controller Manager checks desired state == actual state

## Main K8 Components 
* When we want the browser to be accessible through a browser, we would have to create an external service.
* An external service is a service that opens the communication from external sources but for the database an internal service will be used.
* This is a type of service that I can specify when creating.
* The URL of the external services is normally a HTTP protocol with a Node IP address and the port number of the service which is good for testing purposes but not production.
* For a secure URL, it should be something like this: `https://my-app.com`.
* Another component of Kubernetes is called Ingress.
* Before the request goes to the Service, it first goes to Ingress and it then forwards itself to Service.

## Config Map and Secret
* Pods communicate with each other using a service.
* For example, an application will have a database endpoint named `mongodb service` that uses to communicate with the database.
* To configure the database URL/endpoint, you would usually do it in application properties file. The database URL usually is in the `built` application.
*  If the endpoint of the service name changed to `mongodb`, we would have to adjust that URL in the application so would usually rebuild the application with a new version, push it to the repo and then pull that new image in your pod and restart the whole thing.
*  Since it is a bit tedious to have the small change like database URL, for that purpose. Kubernetes has a component called ConfigMap.
*  ConfigMap creates an external configuration to your application.
*  ConfigMap usually contains configuration data like URLs of a database or some other services that we use in Kubernetes.
*  We just connect it to the Pod so that the Pod actually gets the data that ConfigMap contains.
*  If we change the name of the service (the endpoint of the service), we can just adjust the config map.
*  There is no need to build a new image or have to go through this whole cycle.
*  Part of the external configuration could also be the database username and password. However, this may also change in the application deployment process but putting a password or other credentials in a ConfigMap in a plain text format would be insecure even though it's an external configuration. For this reason, Kubernetes has another component called `Secret`.
*  Secret is just like ConfigMap but the difference is that it is used to store secret data. For example, credentials. This isn't stored in plain text format but in `base64 encoded` format.
*  The built-in security mechanism is not enabled by default.
*  In order to read data from Secret, the Pods needs to be connected to Secret.
*  We can use data from Secret or the ConfigMap using environmental variables or even as a properties file.

## Volumes
* If the container or the Pod gets restarted, the data would be gone.
* If you want data to be logged, and be persisted reliably long term, the way this can be done in Kubernetes is using another components of Kubernetes called `Volumes`.
* It works by attaching a physical storage on a hard drive to the Pod. This storage could either be on a local machine (same server node where the Pod is running) or it could be on the remote storage, meaning outside of the Kubernetes cluster. This could be either cloud storage or our own premise storage which is not part of the Kubernetes cluster.
* When the database Pod or container gets restarted, all the data will still be there.
* Kubernetes doesn't manage data persistance.
* As a Kubernetes user or administrator, we are responsible for backing up the data, replicating and managing the data and making sure it's kept on a proper hardware.

## Deployment and Stateful Set
* When a user can access an application through a browser,  and if an application pod dies, and user has to restart the Pod because the user built a new container image, the user would have a downtime where a user can reach their application which is a bad thing in production.
* This is an advantage to distributed systems and containers so instead of relying on one application pod or one database pod, we are replicating everything on multiple servers.
* This means that another node where a replica or clone of the application would run, will also be connected to the same service.
* Service is like an persistant static IP address with a DNS name so there would be no need to constantly adjust the endpoint when a pod dies.
* A Service also is a load balancer. This means that it actually catches the request and forwards it to whichever part is less busy.
* In order to create the second replica of the application pod, you wouldn't create a second pod but instead would define a blueprint for the application pod and specify how many replicas of that pod you would like to run. That blueprint is called `deployment` which is another component of Kubernetes and in practice, you would not be working with or creating pods, but instead be creating deployments there, you can specify how many replicas and also scale up or scale down with the amount of pods we want.
* Abstraction is on top of pods which makes it more convenient to interact with the pods.
* Pods are a layer of abstraction on top of containers and deployment is another abstraction on top of pods which makes it more convenient to interact with the pods, replicate them and do some other configuration.
* In practice, you would mostly work with deployments and not with pods.
* If one of the replicas of our application pod would die, the service will forward the requests to another one. This would mean that the application would still be accessible for the user. This also applies for the database.
* DB can't be replicated via Deployment. This is because database has a state which is its data.
* This means that if we have clones or replicas of the DB, they would all need to access the same shared data storage and would need some sort of mechanism that manages which pods are currently writing to that storage or which pods are reading from that storage in order to avoid the data inconsistencies and that mechanism in addition to the replicating feature is offered by another Kubernetes component called `StatefulSet`.
* This component is specifically meant for applications like DB. For example, MySQL, MongoDB, elastic search or any other stateful applications or databases should be created using stateful sets and not deployments.
* Deployments for stateLESS Apps.
* StatefulSet for stateFUL  Apps or Databases.
* Making sure the database reads and writes are sychronized so that no database inconsistencies are offered.
* Deploying DB StatefulSet is not easy.
* DB are often hosted outside of K8s cluster and communicate with the external database.
* Once we have two replicas of the application pod and two replicas of the DB and they're both load balanced, our setup is more robust. This means that even if Node 1 was fully rebooted or crashed, and nothing could run on it, we will still have a second node with application and database pods running on it and still being accessible by the user until Node 1 is replicated to avoid downtime.

## 
