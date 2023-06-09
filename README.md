# Install cilium on-premise K8s

You can test cilium eBPF-based Networking (https://cilium.io/) on a on-premise Kubernets.

In this repo you can find a simple vagrant script to test on AlmaLinux 9 (Based on Red Hat 9) OS


## Steps to install vagrant on Fedora

```sh
sudo dnf install @vagrant
sudo dnf install vagrant-libvirt
sudo vagrant plugin install vagrant-libvirt
```
Now you can start the nodes defined on the Vagranfile

```sh
sudo su
vagrant up
```

Now we can connect to node1 and node2
```sh
vagrant ssh node1
```

And in another terminal:
```sh
sudo su
vagrant ssh node2
```

# Init Kubernetes and installation of cilium
Time to initialize the Kubernetes. 
Important: The --pod-network-cidr range used for Cilium is 10.1.1.0/24.

```sh
sudo kubeadm init --pod-network-cidr=10.1.1.0/24 --apiserver-advertise-address <Ip_master_Machine>
```
When the command finishes, to start using your cluster, you need to run the following as a regular user:

```sh
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Also copy the command kubeadm join with sudo privileges in the nodes instances of kubernetes(In this machines you only need to install kubernetes using the steps defined on Installation of Kubernetes inside the machines)

To finish, we need execute the script of cilium (More details in https://docs.cilium.io/en/v1.13/gettingstarted/k8s-install-default/)

First, install the Cilium CLI

```sh
CILIUM_CLI_VERSION=$(curl -s https://raw.githubusercontent.com/cilium/cilium-cli/master/stable.txt)
CLI_ARCH=amd64
if [ "$(uname -m)" = "aarch64" ]; then CLI_ARCH=arm64; fi
curl -L --fail --remote-name-all https://github.com/cilium/cilium-cli/releases/download/${CILIUM_CLI_VERSION}/cilium-linux-${CLI_ARCH}.tar.gz{,.sha256sum}
sha256sum --check cilium-linux-${CLI_ARCH}.tar.gz.sha256sum
sudo tar xzvfC cilium-linux-${CLI_ARCH}.tar.gz /usr/local/bin
rm cilium-linux-${CLI_ARCH}.tar.gz{,.sha256sum}
```

And then:

```sh
cilium install
```

All will be good if you obtain:

✅ Cilium was successfully installed! Run 'cilium status' to view installation health


## Installation of Kubernetes inside the machines 

If you have another machines with Centos or Red Hat with a version higher than 8 and do you like to test this Kubernetes, you can use this steps:

```sh
sudo yum install epel-release -y
sudo yum install yum-utils -y
sudo yum install ansible -y
```

Now you need to copy install-k8s.yml inside the machine. Then it's time to exec the ansible script:

```sh
sudo su
ansible-playbook install.yml
```

Now Kubernetes is installed.



