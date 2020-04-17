# nested-kvm

## git housekeeping

```bash
git config --global credential.helper "cache --timeout=3600"
```

## start nested vm on libvirt

```ruby
Vagrant.configure("2") do |config|
  config.vm.define :dbserver do |dbserver|
    dbserver.vm.box = "centos/8"
    dbserver.vm.provider :libvirt do |domain|
      domain.memory = 14336
      domain.cpus = 4
      domain.nested = true
      domain.volume_cache = 'none'
      domain.cpu_mode = 'host-passthrough'
      domain.machine_virtual_size = 60
    end
  end
end
```

## increase root fs with xfs format

[from here](https://access.redhat.com/articles/1190213)

1) TODO delete in fdisk the old partition and add a new one with the same start vector
2) increase the file system

```bash
sudo xfs_growfs -d /
```

## update centos8

```bash
sudo dnf -y update
```

## install libvirtd

- [from here](https://computingforgeeks.com/how-to-install-kvm-on-rhel-8/)

```bash
sudo dnf install @virt
```

 ## download 

 ```bash
 sudo dnf -y install virt-top libguestfs-tools
 ```

 ## enable and start libvirt

 ```bash
 sudo systemctl enable --now libvirtd
 ```

 ## install gui optional

 ```bash
 sudo yum -y install virt-manager
 ```

## install minikube

[from here](https://computingforgeeks.com/how-to-install-minikube-on-centos-linux-with-kvm/)

## add group to current user vagrant

```bash
# add group to user
sudo usermod -a -G libvirt vagrant
# set primary group yet for the session until reboot
newgrp libvirt
```

## edit /etc/libvirt/libvirtd.conf

```bash
# sudo vi /etc/libvirt/libvirtd.conf
echo 'unix_sock_group = "libvirt"' | sudo tee -a /etc/libvirt/libvirtd.conf
echo 'unix_sock_rw_perms = "0770"' | sudo tee -a /etc/libvirt/libvirtd.conf
```

## restart libvirtd.service and check status

```bash
# restart
sudo systemctl restart libvirtd.service
# check status
sudo systemctl status -l  libvirtd.service --no-pager
```

## install wget

```bash
sudo dnf install -y wget
```

## Download minikube

[from here](https://computingforgeeks.com/how-to-install-minikube-on-centos-linux-with-kvm/)

```bash
# install
cd && \
wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && \
chmod +x minikube-linux-amd64 && \
sudo mv minikube-linux-amd64 /usr/local/bin/minikube
# check
minikube --help && minikube version
```
## Install kubectl

```bash
# install
cd && \
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl  && \
chmod +x kubectl && \
sudo mv kubectl  /usr/local/bin/

# check
kubectl version --client -o json
```

## start minikube

```bash
minikube start
```


## status minikube

```bash
kubectl cluster-info
kubectl config view
kubectl get nodes
```

## stop minikube
