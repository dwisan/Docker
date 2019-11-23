Step 1: Install Docker on both the nodes

# apt install docker.io

Step 2: Enable Docker on both the nodes

# systemctl enable docker

Step 3: Add the Kubernetes signing key on both the nodes

# curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add

Step 4: Add Xenial Kubernetes Repository on both the nodes

# apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"

Step 5: Install Kubeadm

# apt install kubeadm

Step 6: Disable swap memory

# swapoff -a

Step 7: Initialize Kubernetes on the master node

# kubeadm init --pod-network-cidr=10.244.0.0/16
# mkdir -p $HOME/.kube
# cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
# chown $(id -u):$(id -g) $HOME/.kube/config

Step 8: Deploy a Pod Network through the master node

# kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

Step 9: Add the slave node to the network in order to form a cluster

# kubeadm join 172.18.111.108:6443 --token z9pc4v.q3tc6zr62npdcj3g --discovery-token-ca-cert-hash sha256:75461b9f50da389540eb9f90f98ce1d4b93f605fb6e4cd82b9b199ff16cff794

Step 10: example deploy

# nano nginx-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx0-deployment
  labels:
    app: nginx0-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx0
  template:
    metadata:
      labels:
        app: nginx0
    spec:
      containers:
      - name: nginx
        image: k8s.gcr.io/nginx:1.7.9
        ports:
        - containerPort: 80
# kubectl apply -f nginx-deployment.yaml
# kubectl expose deployment nginx-deployment --type=LoadBalancer --port=80
# kubectl patch svc nginx-deployment -p '{"spec":{"externalIPs":["172.18.111.108"]}}'

# kubectl scale deploy nginx-deployment --replicas=5
# kubectl autoscale deploy nginx-deployment  --min=10 --max=15 --cpu-percent=80

remove node

kubectl drain s106 --ignore-daemonsets --delete-local-data

ref: https://kubernetes.io/docs/reference/kubectl/cheatsheet/ 
