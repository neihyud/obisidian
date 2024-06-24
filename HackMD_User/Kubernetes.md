---
title: Kubernetes
tags: [Devops]

---

###### tag `DevOps`

# Kubernete

![image](https://hackmd.io/_uploads/Hy5izvrD6.png)

![image](https://hackmd.io/_uploads/SkS6swSva.png)

![image](https://hackmd.io/_uploads/B12hhDHP6.png)


![image](https://hackmd.io/_uploads/BkU2bFSwa.png)

![image](https://hackmd.io/_uploads/HJftbYrwa.png)

![image](https://hackmd.io/_uploads/Bkv8g_dwT.png)


![image](https://hackmd.io/_uploads/ryOmWtBwT.png)

- dấu '-' chỉ định đại diện cho một container, có thể có nhiều '-'

![image](https://hackmd.io/_uploads/BJ00mKSDT.png)

![image](https://hackmd.io/_uploads/By51NFBvT.png)

---

![image](https://hackmd.io/_uploads/BJotnYHD6.png)

---
![image](https://hackmd.io/_uploads/SyqgW_uDp.png)

![image](https://hackmd.io/_uploads/ry-wXd_D6.png)


![image](https://hackmd.io/_uploads/Bymas9Bwa.png)

---
![image](https://hackmd.io/_uploads/r1Dgp9HwT.png)

![image](https://hackmd.io/_uploads/S1Yo7u_wa.png)


- NodePort Service
![image](https://hackmd.io/_uploads/ry5jzpSPT.png)
    
    - nếu sử dụng minikube -> 
        - search nodes **`kubectl get nodes`**
        - **`kubectl describe node minikube`** -> find **Addresses: InternalIP** -> get id
        - **`kubectl get services`** -> get port in service
        -> `http://{InternalIP}:{port_service}`
    - còn sử dụng docker -> **localhost**

![image](https://hackmd.io/_uploads/ryfsAAHwa.png)
![image](https://hackmd.io/_uploads/rku6QduwT.png)

---

![image](https://hackmd.io/_uploads/SJfWoD_PT.png)

![image](https://hackmd.io/_uploads/rydONdOva.png)


![image](https://hackmd.io/_uploads/HksM6njwT.png)


## Fix Bug
### ErrImagePull, ErrImageNeverPull and ImagePullBackoff Errors
![image](https://hackmd.io/_uploads/ByklyFrP6.png)
![image](https://hackmd.io/_uploads/rkEbyKHwp.png)

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