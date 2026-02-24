Before Docker:
============================

 - In physical machine -> we take hardware, install OS, then install the APP in it.
 - It will create issue if we install multiple versions of same software. Ex - Java 8 and 11, path setting issue.
 - Hardware utilization is not being done properly. If RAM will be 8 GB, and app will consume 1 GB of RAM, rest all waste.
 - Then virtualization came to market. On top of physical host, virtualization will be done. ex: Hyper-V, VMware.
 - On top of virtualization, multiple OS can be created.
 - Hardware must support this virtualization.
 - If we are not using the RAM also, the VM will take the RAM, use it or not.

What is Docker?
============================

 - An OS‑level virtualization platform.
 - OS‑level virtualization = containerization
 - Container is a lightweight OS.
 - Allows applications to share the host OS kernel instead of running a separate guest OS.
 - This design makes Docker containers lightweight, fast, and portable, while keeping them isolated from one another.
 - Written in the Go programming language.
 - VMware => hardware‑level virtualization, Docker => OS level virtualization.
 - In case Hyper-V, if we delete the OS, we need to re create it. But docker image can be stored in container registry, ex: Docker Hub, ECR.
 - For monolithic app, hyper-v is OK, but for micro service, containers are suitable.

Components of Docker:
============================

 - Docker Engine:
	  - It enables Docker to run containers on a system. 
	  - It follows a client-server architecture and is responsible for building, running, and managing Docker containers.
	  - Docker Server runs in the background, listening to API requests and managing objects like images, containers, networks, and volumes.
	  - The Docker Client (docker CLI) communicates with the daemon using a REST API. 
	  - Docker Client provides the execution environment where Docker Images are instantiated into live containers.

 - Docker Image: 
	  - It is a template that is used for creating containers, containing the application code and dependencies.

 - Dockerfile: 
	  - It contains instructions to build Docker image.
	  - Application dependencies specified in Dockerfile.
	  - The name must be Dockerfile.

 - Docker Registry : It is a storage distribution system for docker images, where you can store the images in both public and private modes. 
 	- Ex: Docker Hub, ECR


Docker Installation:
============================
 - Launch T2 micro Amazon EC2 instance.
 - Security Group -> inbound - SSH, outbound - all
 - Follow the instructions:
	https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-docker.html

 - sudo yum update -y
 - sudo yum install docker -y
 - sudo systemctl status docker
 - sudo systemctl start docker
 - sudo systemctl enable docker
 - sudo systemctl status docker
 - sudo usermod -a -G docker ec2-user
	 - Add the ec2-user to the docker group so that you can run Docker commands without using sudo.
log out and log back in again.

 - docker -v		-> Check Docker Version
 - docker info		-> Displays system wide information regarding the Docker installation.


 Hello World Example:
 ============================

docker run hello-world
	- checks for hello-world image in local rgistry
	- if found, it will create a container
	- if not found, it will download the image from docker hub and then create a container

Docker Commands
============================

 - docker pull image_name 		-> download the image from docker registry to local registry - https://hub.docker.com/
 - docker images				-> to show all images in local rgistry
 - docker run image_name		-> pull the image if it is not available, run the image to form a container
 - docker ps 					-> To show all running containers
 - docker ps -a 				-> To show all running and stopped containers
 - docker stop cnt_id/cnt_name -> To stop a container
 - docker start cnt_id/cnt_nm	-> To start a container
 - docker rmi img_name			-> To remove an image, it should not be in running condition
							-> If image is latest, only image name is fine, if any version will be there, then provide the version
							-> ex: docker rmi centos:7
							-> We can use image id directly also.

run commands:

 - Ex1: docker run ubuntu
 	- It downloads and runs an instance of ubuntu image and exits immediately.
 	- This is because containers and not made to host OS, but to do a specific task on OS such as host a web server.
	- So, containers exit until the service under them are in running state.

 - Ex2: docker run -it ubuntu
	 - it - Interactive Terminal
	 - We will go inside the container
	 - exit -> to come out from the container

 - Ex3: docker run -it ubuntu
	- come out from the container without stopping it, be in docker host.
		 - 	Ctrl p q
	- go inside the container again:
	 - docker attach cnt_id

 - Ex4: docker run -itd ubuntu
	itd - Interactive Terminal Detached mode
		- Go inside the container, attach the terminal, come out without stopping the container.
		
 - For stopped containers:
	1. start the container
		 - docker start cnt_id
	2. Go inside the container:
		 - docker attach cnt_id
		
 - Ex-5: Give name to the container
	 - docker run -it --name c2 ubuntu

 - Ex-6: Port mapping
	- Run an NGINX container
 - docker run -it --name nginx-ex -p 8080:80 nginx

Dockerfile Keywords:
=====================
	 - # Comment
	 - INSTRUCTION arguments

	 - The instruction is not case-sensitive. However, convention is for them to be UPPERCASE to distinguish them from arguments more easily.
	 - Docker runs instructions in a Dockerfile in order. A Dockerfile must begin with a FROM instruction.
	 - The FROM instruction specifies the base image from which you are building.



