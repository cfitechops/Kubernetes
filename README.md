# Kubernetes

#### Passer en root

```sh
sudo su
```

#### Configurer les noms d'hôtes dans /etc/hosts

```sh
printf "\n192.168.129.203 master\n192.168.129.204 node01\n192.168.129.205 node02\n" >> /etc/hosts
```

#### Charger les modules kernel nécessaires

```sh
printf "overlay\nbr_netfilter\n" >> /etc/modules-load.d/containerd.conf
modprobe overlay
modprobe br_netfilter
```

#### Configurer les paramètres sysctl

```sh
printf "net.bridge.bridge-nf-call-iptables = 1\nnet.ipv4.ip_forward = 1\nnet.bridge.bridge-nf-call-ip6tables = 1\n" >> /etc/sysctl.d/99-kubernetes-cri.conf
sysctl --system
```

#### Installer containerd

```sh
wget https://github.com/containerd/containerd/releases/download/v1.7.13/containerd-1.7.13-linux-amd64.tar.gz -P /tmp/
tar Cxzvf /usr/local /tmp/containerd-1.7.13-linux-amd64.tar.gz
wget -4 https://raw.githubusercontent.com/containerd/containerd/main/containerd.service -P /etc/systemd/system/
systemctl daemon-reload
systemctl enable --now containerd
```

#### Installer runc

```sh
wget https://github.com/opencontainers/runc/releases/download/v1.1.12/runc.amd64 -P /tmp/
install -m 755 /tmp/runc.amd64 /usr/local/sbin/runc
```

#### Installer les plugins CNI

```sh
wget https://github.com/containernetworking/plugins/releases/download/v1.4.0/cni-plugins-linux-amd64-v1.4.0.tgz -P /tmp/
mkdir -p /opt/cni/bin
tar Cxzvf /opt/cni/bin /tmp/cni-plugins-linux-amd64-v1.4.0.tgz
```

#### Configurer containerd

```sh
mkdir -p /etc/containerd
containerd config default | tee /etc/containerd/config.toml
```

#### Éditer manuellement /etc/containerd/config.toml et changer SystemdCgroup à true

```sh
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
  SystemdCgroup = true
```

#### Redémarrer containerd

```sh
systemctl restart containerd
```

#### Désactiver le swap

- Éditer /etc/fstab et commenter la ligne du swap

```sh
#/swap.img      none    swap    sw      0       0
```

#### Installer Kubernetes

```sh
apt-get update
apt-get install -y apt-transport-https ca-certificates curl gpg

mkdir -p -m 755 /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

apt-get update

apt-get install -y kubelet=1.31.5-1.1 kubeadm=1.31.5-1.1 kubectl=1.31.5-1.1
apt-mark hold kubelet kubeadm kubectl
```

#### Redémarrer le système

```sh
reboot
```

## Configuration du nœud maître uniquement

#### Initialiser le cluster

```sh
kubeadm init --pod-network-cidr 10.10.0.0/16 --kubernetes-version 1.31.5 --node-name master
```

#### Configurer kubectl

```sh
export KUBECONFIG=/etc/kubernetes/admin.conf
kubectl get nodes
```

#### Installer Calico CNI

```sh
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.2/manifests/tigera-operator.yaml
wget https://raw.githubusercontent.com/projectcalico/calico/v3.27.2/manifests/custom-resources.yaml
```

#### Éditer custom-resources.yaml si nécessaire pour ajuster le CIDR

```sh
cidr: 10.10.0.0/16
```

#### Appliquer la configuration

```sh
kubectl apply -f custom-resources.yaml
```

#### Vérifier l'installation

```sh
kubectl get pods --all-namespaces
kubectl get po -n kube-system -w
```

#### Générer la commande pour joindre les nœuds workers

```sh
kubeadm token create --print-join-command
```

#### Configuration des nœuds workers uniquement

- Exécuter la commande générée par kubeadm token create --print-join-command sur le nœud maître.
  - Vérifications finales (sur le nœud maître)

```sh
kubectl get pods -n kube-system
kubectl get po -n kube-system -v=9
kubectl get po -n kube-system -v=5
kubectl api-resources

kubectl explain po
kubectl explain po.metadata
kubectl explain po.spec
kubectl explain po.spec.containers
```

#### Configuration pour un utilisateur non-root (optionnel)

```sh
mkdir -p /home/barry/.kube
cp /root/.kube/config /home/barry/.kube/
chown barry:barry /home/barry/.kube/config

export KUBECONFIG=~/.kube/config
kubectl get nodes
```

#### Resources

- [UBUNTU SERVER](https://releases.ubuntu.com/jammy/)
- [KUBERNETES](https://kubernetes.io/releases/)
- [CONTAINERD](https://containerd.io/releases/)
- [RUNC](https://github.com/opencontainers/runc/releases)
- [CNI PLUGINS](https://github.com/containernetworking/plugins/releases)
- [CALICO CNI](https://docs.tigera.io/calico/3.27/getting-started/kubernetes/quickstart)
