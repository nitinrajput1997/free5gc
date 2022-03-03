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

## Deploy Helm Chart


### create namespace 
```bash
kubectl create namespace free5gc
```

### add helm repository
```bash
helm repo add towards5gs 'https://raw.githubusercontent.com/Orange-OpenSource/towards5gs-helm/main/repo/'
helm repo update
```

### view repository list
```bash
helm repo list
```

### view available charts
```bash
helm search repo
```

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

## Service Monitoring

### add helm repo
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

### view charts
```bash
helm search repo prometheus-community
```

### create k8s namespace
```bash
kubectl create namespace prometheus
```

### deploy chart
```bash
helm install prometheus prometheus-community/kube-prometheus-stack -n prometheus
```

### pods
```bash
kubectl get pods -n prometheus
```

### grafana runs on ClusterIP 80

#### change svc clusterIP to NodePort
```bash
kubectl get svc -n prometheus | grep grafana
```

### port forward
```bash
kubectl port-forward -n prometheus prometheus-grafana-8568977b76-mh9k5 3000
ssh -L localhost:3000:localhost:3000 ubuntu@192.168.5.95
```

### the default credentials(username/password) are admin/prom-operator credentials are base64 encoded
```bash
kubectl get secret --namespace prometheus prometheus-grafana -o yaml
```
