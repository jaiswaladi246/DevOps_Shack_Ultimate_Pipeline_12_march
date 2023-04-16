# K8

Step1:

Master and slave 

apt-get update -y
apt-get install docker.io -y
service docker restart  
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -  
echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" >/etc/apt/sources.list.d/kubernetes.list
apt-get update
apt install kubeadm=1.20.0-00 kubectl=1.20.0-00 kubelet=1.20.0-00 -y  

Step2:

Master node:

   kubeadm init --pod-network-cidr=192.168.0.0/16
   kubeadm token create --print-join-command
   
   
Step3: 

Master node: 

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config


   
step4:

Master node:

kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.25.1/manifests/calico.yaml 
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.49.0/deploy/static/provider/baremetal/deploy.yaml



##################################################################################################################################################

YAML

nginx.yaml

apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
	
	
	
service.yaml


apiVersion: v1
kind: Service
metadata:
  name: nginx
  labels:
    name: nginx
spec:
  type: NodePort
  ports:
    - port: 80
      nodePort: 30080
      name: http
    - port: 443
      nodePort: 30443
      name: https
  selector:
    name: nginx
