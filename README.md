# free5gc

### install required packages
```bash
sudo apt update -y
sudo apt upgrade -y
sudo apt install -y curl wget apt-transport-https
```

### install minikube
```bash
wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo cp minikube-linux-amd64 /usr/local/bin/minikube
sudo chmod +x /usr/local/bin/minikube
```

### start minikube
### use calico as cni plugin
```bash
minikube start --driver=docker --cpus=4 --memory=8g --disk-size=20g --network-plugin=cni --cni=calico
```

### verify minikube installation
```bash
minikube status
```

### install kubectl
```bash
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

### install helm
```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

### verify helm installation
```bash
helm list -A
```

### install multus-cni
```bash
git clone https://github.com/k8snetworkplumbingwg/multus-cni
cd multus-cni
cat ./deployments/multus-daemonset-thick-plugin.yml | kubectl apply -f -
```

### verify installation
```bash
kubectl get pods -A
```
