Docker:
=
 - package applications into containers.

Docker Hub:
=
 - Cloud-based registry service and repository for Docker container images. 
 - Used to find, share, store, and manage containerized applications.

Docker Compose:
=
 - Docker Compose is for defining and running multi-container applications on a single host machine for development and testing purposes.
 - Auto scaling feature: No
 - Primary Use Case: Local development/testing on a single host

Docker Swarm:
=
 - Docker Swarm is a native, simpler container orchestrator built into Docker for managing a cluster of machines.
 - Auto-Scaling: Manual scaling only
 - Primary Use Case: Simple, small-to-medium scale cluster orchestration

Kubernetes:
=
 - Kubernetes is an open source Container orchestration (Orchestration means management) platform.
 - Containers are the isolated boxes where we run our applications.
 - We use Docker to containerize the applications and K8S to manage Docker Containers.
 - Google originally designed Kubernetes and released it as open source in 2014.
 - The name Kubernetes comes from ancient Greek and means "helmsman" or "pilot".
 - Later it has been donated to CNCF (Cloud Native Computing Foundation).
 - CNCF is a Linux Foundation project that was started in 2015 to help advance container technology.
 - K8S helps to automate the application deployment process completely.
 - K8S provides a framework to manage the application deployment, scale up containers, scale down the containers and load balancing also.

Advantages of K8S:
=
 - Container Orchestration
 - Scalability - We can scale up the containers and scale down the containers based on the requirement.
 - Self-Healing - If any container gets crashed, K8S creates another container for this.
 - Load Balancing

K8S Architecture:
=
 - K8S works based on clustered architecture.
 - Cluster means group of servers or machines connected all together in a single network.
 - A Kubernetes cluster consists of a control plane plus a set of worker machines, called nodes, that run containerized applications. 
 - K8S Cluster = Control Plane + Set of worker machines
 - Every cluster needs at least one worker node in order to run Pods.
 - K8S follows master slave architecture. It has:
		Control Plane ( Master Node / Master Machine)
		Worker machines = Nodes

Control Plane:
=
 - Itis the main entry point for administrators and users to manage the various nodes. 
 - Operations are issued to it either through HTTP calls or connecting to the machine and running command-line scripts. 
 - As the name implies, it controls how Kubernetes interacts with your applications.

Worker Nodes:
=
 - Worker nodes are the machines (virtual or physical) that run the containerized applications.


Pods:
=
 - A Kubernetes pod is a group of containers, and is the smallest unit that Kubernetes administers.
 - Pod represents runtime instance of the application.
 - Pods have a single IP address that is applied to every container within the pod. 
 - Containers in a pod share the same resources such as memory and storage. 
 - This allows the individual Linux containers inside a pod to be treated collectively as a single application, as if all the containerized processes were running together on the same host in more traditional workloads. 
 - It’s quite common to have a pod with only a single container, when the application or service is a single process that needs to run. 
 - But when things get more complicated, and multiple processes need to work together using the same shared data volumes for correct operation, multi-container pods ease deployment configuration compared to setting up shared resources between containers on your own.	

Nodes:
=
 - A Kubernetes node manages and runs pods; it’s the machine (whether virtualized or physical) that performs the given work. 
 - Just as pods collect individual containers that operate together, a node collects entire pods that function together. 
 - When you’re operating at scale, you want to be able to hand work over to a node whose pods are free to take it.

Components of Control Plane:
=
API Server:
-
 - Inside the Control Plane API Server is available.
 - If we want to deploy some application in K8s, we can send the input to K8S via UI or via CLI which is kubectl.
 - Here API Server takes the incoming request and stores it in etcd.
 - Incoming request can be like application deployment, increase/decrease the containers, delete the application from K8S.
 - The API server exposes a REST interface to the Kubernetes cluster. 
 - All operations against pods, services, and so forth, are executed programmatically by communicating with the endpoints provided by it.

etcd
=
 - etcd is the internal database of the K8S cluster which is used to store the request information.
 - etcd is a distributed key-value store that Kubernetes uses to share information about the overall state of a cluster. 
 - Additionally, nodes can refer to the global configuration data stored there to set themselves up whenever they are regenerated.

Scheduler
=
 - Once the request gets stored in etcd, then scheduler schedules the pending tasks available in etcd.
 - The scheduler is responsible for assigning work to the various nodes. 
 - It checks which worker node is free and assigns task to that worker node.
 - In that worker node the application gets deployed as a pod.
 - It keeps watch over the resource capacity and ensures that a worker node’s performance is within an appropriate threshold.
    - 
Controller manager
=
 - The controller-manager will monitor the functionality of all the K8S resources.
 - The controller-manager is responsible for making sure that the shared state of the cluster is operating as expected. 
 - More accurately, the controller manager oversees various controllers which respond to events (e.g., if a node goes down).


Components of Worker Node:
=
	Kubelet:
	=
 - Scheduler gets the information about the available worker nodes by using a node agent which is Kubelet.
 - Communicating with Kubelet we can know the information like health of the worker node, status of the worker node, whether the worker node is available or not, what pods are running in the worker node etc.
 - Scheduler communicates with the worker node by using Kubelet, and based on the availability scheduler assigns tasks to the worker node. 
 - A Kubelet tracks the state of a pod to ensure that all the containers are running. 
 - It provides a heartbeat message every few seconds to the control plane. 
 - If a replication controller does not receive that message, the node is marked as unhealthy.
	
	Kube proxy:
	=
 - Kube-proxy provides network for cluster communication.
 - Control plane machine communicates with worker node machine and vice versa. 
 - There is the network communication present in between them.
 - To establish the network communication, Kube-proxy will provide the network.
 - The Kube proxy routes traffic coming into a node from the service. It forwards requests for work to the correct containers.
	
K8S Cluster Setup:
=
	We can do the setup in multiple ways:
		1. Self managed cluster:
			Create your own cluster and run your application.
				Mini Kube ( Single Node Cluster )
				Kubeadm ( Multi Node Cluster )
		2. Managed K8S Cluster
			Cloud providers will provide you the K8S cluster.
				AWS EKS - Elastic K8S Service
				Azure AKS - Azure K8S Server
				GCP GKE - Google K8s Engine
	




