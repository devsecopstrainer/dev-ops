#Amazon Machine Image : Ubuntu Server 24.04 LTS (HVM)
#Instance Type : t3.micro
#Storage : 15 GB
#Save this as master.sh

#1. Rename the hostname
  echo "... Starting the script on master ..."
  echo *... Rename Host name to master ..."
  sudo hostnamectl set-hostname master
  sleep 2

#2. Disable swap
#Swap = disk space used as virtual memory when RAM is full.
#When RAM fills up, the system moves some memory pages to the swap area on disk.
#when you run swapoff -a 
#1. The system stops using swap memory.
#2. If RAM does not have enough free space, the command may fail or cause performance issues.
#Swap must be disabled during Kubernetes installation 
  echo "... disable all swap spaces on the system ..."
  sudo swapoff -a
  sleep 2

#3. Enable IP forwarding and save that setting in a configuration file.
#This is required for container networking in Kubernetes.
#net.ipv4.ip_forward = Controls whether the system forwards network packets between interfaces.
#= 1 => Enable IP forwarding
#So the system behaves like a router, allowing traffic to pass from one network interface to another.
#echo "net.ipv4.ip_forward = 1" ==> Prints the text.
#| => The pipe sends the output of echo to another command.
#sudo tee /etc/sysctl.d/k8s.conf
#=> tee creates a file => /etc/sysctl.d/k8s.conf 
#=> and writes this input to the file => net.ipv4.ip_forward = 1
#Why this is used for Kubernetes:
#In Kubernetes: Pods communicate across nodes.
#Container networks require packet forwarding.
#Without enabling ip_forward, networking between pods may fail.

  echo "... Enable IPv4 packet forwarding ..."
  echo "net.ipv4.ip_forward = 1" | sudo tee /etc/sysctl.d/k8s.conf
  sleep 2

#4. Restart sysctl
#Reloads all Linux kernel configuration settings from sysctl files and applies them immediately.
#This activates the setting without rebooting the system.
  echo *... Restart sysctl ..."
  sudo sysctl --system
  sleep 2

#5. verify ipv4.ip_forward is 1 or not
#You may see:
#net.ipv4.ip_forward = 0
#or
#net.ipv4.ip_forward = 1
  echo *... verify ipv4.ip_forward is 1 or not ..."
  sysctl net.ipv4.ip_forward
  sleep 5

#6. system update and upgrade 
  echo *... Update the system ..."
  sudo apt update && sudo apt upgrade -y
  sleep 2

#7. Install containerd and verify the version
  echo *... Install containerd ..."
  sudo apt-get install containerd -y
  sleep 2
  echo "... Verify installation ..."
  ctr --version
  sleep 2

#8. Downloading cni-plugins 
#CNI - Container Network Interface
#CNI allows containers to communicate over the network

  echo "... Downloading cni-plugins ..."
  sudo mkdir -p /opt/cni/bin

#This prepares the installation directory for networking plugins.

  CNI_VERSION=$(curl -sSL "https://api.github.com/repos/containernetworking/plugins/releases/latest" | jq -r '.tag_name')
#Explanation of above command:
#curl downloads release information from GitHub.
#It queries the repository of containernetworking plugins.
#jq extracts the latest version tag.
#The version is saved in the variable CNI_VERSION.
#Example result:
#v1.5.1
#So the variable now contains the latest plugin version.

  wget -q "https://github.com/containernetworking/plugins/releases/download/${CNI_VERSION}/cni-plugins-linux-amd64-${CNI_VERSION}.tgz"
#Meaning:
#wget downloads the plugin archive
#-q → quiet mode (no output)
#${CNI_VERSION} inserts the version automatically
#Example file downloaded:
#cni-plugins-linux-amd64-v1.5.1.tgz
#This archive contains networking binaries like:
#bridge
#host-local
#loopback
#macvlan

#Extract plugins to the CNI directory
  sudo tar Cxzf /opt/cni/bin "cni-plugins-linux-amd64-${CNI_VERSION}.tgz"
#tar → extract archive
#C → extract into a specific directory
#x → extract files
#z → gzip archive
#f → file name
#/opt/cni/bin → destination directory
  echo "**** Downloaded cni-plugins to/opt/cni/bin ..."
  sleep 2

#9. Configure containerd
  echo *... Configure containerd ..."
  sudo mkdir /etc/containerd
  echo *... Generate the config.toml file ..."
  sudo containerd config default | sudo tee /etc/containerd/config.toml
#After running the command, /etc/containerd/config.toml contains the full containerd configuration.
  sleep 2

#10. Change SystemdCgroup to true and restart containerd
  echo *... Change SystemdCgroup to true in config.toml file and restart containerd ..."
  sudo sed -i -e 's/SystemdCgroup = false/SystemdCgroup = true/g' /etc/containerd/config.toml
#This is required because:
#containerd uses systemd cgroup driver
#it matches Kubernetes kubelet configuration
#prevents resource management issues
  echo *... Restarting containerd ..."
sudo systemctl restart containerd
sleep 2
