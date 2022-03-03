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
