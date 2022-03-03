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

Deploy Helm Chart


# create namespace 
kubectl create namespace free5gc

# add helm repository
helm repo add towards5gs 'https://raw.githubusercontent.com/Orange-OpenSource/towards5gs-helm/main/repo/'
helm repo update

# view repository list
helm repo list


# view available charts
helm search repo

---


## deploy helm
### -n free5gc(namespace) free5gc-v1(name of the helm deployment) towards5gs/free5gc(chart name)
```bash
helm -n free5gc install free5gc-v1 towards5gs/free5gc
```

### verify helm deployment
```bash
kubectl get pods -n free5gc
```


### list services
```bash
kubectl get svc -n free5gc
```

### kubernets port forward to access the NodePort service
```bash
kubectl port-forward --namespace free5gc svc/webui-service 5000:5000
ssh -L localhost:5000:localhost:5000 ubuntu@192.168.5.95
```
