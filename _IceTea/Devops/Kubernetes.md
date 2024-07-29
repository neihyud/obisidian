---
title: Kubernetes
tags: [Devops]

---

###### tag `DevOps`

# Kubernete
![[Pasted image 20240729205503.png]]
![[Pasted image 20240729205507.png]]
![[Pasted image 20240729205511.png]]
![[Pasted image 20240729205514.png]]
![[Pasted image 20240729205517.png]]
![[Pasted image 20240729205537.png]]
![[Pasted image 20240729205540.png]]

- dấu '-' chỉ định đại diện cho một container, có thể có nhiều '-'
 ![[Pasted image 20240729205549.png]]
 ![[Pasted image 20240729205601.png]]



---

![[Pasted image 20240729205655.png]]

---
![[Pasted image 20240729205713.png]]

![[Pasted image 20240729205720.png]]
![[Pasted image 20240729205726.png]]


---
![[Pasted image 20240729205744.png]]

![[Pasted image 20240729205750.png]]


- NodePort Service
![[Pasted image 20240729205757.png]]
    
- nếu sử dụng minikube -> 
	- search nodes **`kubectl get nodes`**
	- **`kubectl describe node minikube`** -> find **Addresses: InternalIP** -> get id
	- **`kubectl get services`** -> get port in service
	-> `http://{InternalIP}:{port_service}`
- còn sử dụng docker -> **localhost**
![[Pasted image 20240729205845.png]]
![[Pasted image 20240729205850.png]]

---
![[Pasted image 20240729205900.png]]
![[Pasted image 20240729205905.png]]
![[Pasted image 20240729205912.png]]


## Fix Bug
### ErrImagePull, ErrImageNeverPull and ImagePullBackoff Errors
![[Pasted image 20240729205926.png]]
![[Pasted image 20240729205943.png]]

- if no run ingress -> 
- minikube start --vm=true  


- k create secret generic jwt-secret --from-literal=JWT_KEY=keysecret (create secret)
## Setup

Install Minikube

In your terminal run the following:
:::info
- curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64/
- sudo install minikube-linux-amd64 /usr/local/bin/minikube
- sudo usermod -aG docker $USER && newgrp docker
- minikube start
- curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
- sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
- kubectl version
:::

>[!info]
>- curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64/
> - sudo install minikube-linux-amd64 /usr/local/bin/minikube
> - sudo usermod -aG docker $USER && newgrp docker
>- minikube start
>- curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.tx- t)/bin/linux/amd64/kubectl"
>- sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
>- kubectl version


Starting Minikube and Testing Installation

After you have successfully installed Minikube we need to start and test the cluster to make sure everything is working correctly.

1. Add your user to the docker group

Note - If this step was performed when Docker was installed, it can be skipped.

In your terminal, run:

sudo usermod -aG docker $USER && newgrp docker

Log out of the user profile and log back in so these changes take effect. If running inside a VM, you will need to restart the entire machine, not just log out.

2. Start with the default driver:

In your terminal, run:

minikube start

Your output should look similar to this:


2. Check Minikube Status

After you see a Done! message in your terminal, run minikube status to make sure the cluster is healthy. Pay particular attention that the apiserver is in a "Running" state.


3. Install kubectl

In your terminal run the following:

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

4. Test kubectl

Lastly, open up your terminal and make sure that you can run kubectl version


Note - the client and server can be off by one minor version without error or issue.