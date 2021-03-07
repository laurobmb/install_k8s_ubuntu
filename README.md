# DICA
source <(kubectl completion bash)

kubectl label node NOME_DO_NODE node-role.kubernetes.io/worker=worker

kubectl label node kubernetes-worker node-role.kubernetes.io/worker=worker
kubectl label node kube02.trf5.gov.br node-role.kubernetes.io/worker=worker


sudo hostnamectl set-hostname kubernetes-master

sudo hostnamectl set-hostname kubernetes-worker

sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab

sudo swapoff -a

sudo modprobe overlay

sudo modprobe br_netfilter

cat << EOF >>/etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF

sudo sysctl --system

sudo apt update

sudo apt -y upgrade

export OS=xUbuntu_20.04
export VERSION=1.18

echo "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/ /" > /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list
echo "deb http://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/$VERSION/$OS/ /" > /etc/apt/sources.list.d/devel:kubic:libcontainers:stable:cri-o:$VERSION.list

curl -L https://download.opensuse.org/repositories/devel:kubic:libcontainers:stable:cri-o:$VERSION/$OS/Release.key | apt-key add -
curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/Release.key | apt-key add -

apt update

apt install -y cri-o cri-o-runc

Edit the /etc/crio/crio.conf file as follows:

conmon = "/usr/bin/conmon"         #<-- Edit this line. Around line 108

registries = [                     #<-- Edit and add registries. Around line 351
        "docker.io",
        "quay.io",
]


sudo systemctl daemon-reload
sudo systemctl enable crio
sudo systemctl start crio
sudo systemctl status crio

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add

sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"

sudo apt -y install kubeadm kubelet kubectl kubernetes-cni

cat << EOF >>/etc/default/kubelet
KUBELET_EXTRA_ARGS=--feature-gates="AllAlpha=false,RunAsGroup=true" --container-runtime=remote --cgroup-driver=systemd --container-runtime-endpoint='unix:///var/run/crio/crio.sock' --runtime-request-timeout=5m
EOF

sudo kubeadm init --apiserver-advertise-address=192.168.123.206 --pod-network-cidr=10.244.0.0/16

kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

kubectl get pods --all-namespaces



