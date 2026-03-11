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
