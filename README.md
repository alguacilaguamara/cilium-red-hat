# Install cilium on-premise K8s

You can test cilium eBPF-based Networking (https://cilium.io/) on a on-premise Kubernets.

In this repo you can find a simple vagrant script to test on AlmaLinux 9 (Based on Red Hat 9) OS


## Steps to install vagrant on Fedora

```sudo dnf install @vagrant```
```sudo dnf install vagrant-libvirt```
```sudo vagrant plugin install vagrant-libvirt```
Now you can start the nodes defined on the Vagranfile

```sudo su```
```vagrant up master```
```vagrant up node1```

## Installation of Kubernetes inside the machines

First step is connect via ssh with the machine. If you are using vagrant

```vagrant ssh master```

```sudo yum install epel-release -y```
```sudo yum install yum-utils -y```
```sudo yum install ansible -y```

Now you need to copy install-k8s.yml inside the machine. Then it's time to exec the ansible script:

```sudo su```
```ansible-playbook install.yml```

Now Kubernetes is installed.

# Init Kubernetes and installation of cilium
Time to initialize the Kubernetes. 
The --pod-network-cidr range used for Cilium is 10.1.1.0/24.

```sudo kubeadm init --pod-network-cidr=10.1.1.0/24 --apiserver-advertise-address <Ip_master_Machine>```

When the command is ended, copy the command kubeadm join in the nodes instances of kubernetes(In this machines you only need to install kubernetes using the steps defined on Installation of Kubernetes inside the machines)

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

All will be good if you get:

✅ Cilium was successfully installed! Run 'cilium status' to view installation health



