# k8s 集群搭建

sudo kubeadm init  --apiserver-advertise-address 8.3.29.79  --kubernetes-version=v1.19.0  --pod-network-cidr=10.244.0.0/16



cat > /etc/docker/daemon.json <<EOF { "exec-opts": ["native.cgroupdriver=systemd"], "log-driver": "json-file", "log-opts": { "max-size": "100m" }, "storage-driver": "overlay2", "storage-opts": [ "overlay2.override_kernel_check=true" ] } EOF

```shell
cat > /etc/docker/daemon.json <<EOF 
{ 
"exec-opts": ["native.cgroupdriver=systemd"], 
"log-driver": "json-file",
"log-opts": { "max-size": "100m" },
"storage-driver": "overlay2",
"storage-opts": [ "overlay2.override_kernel_check=true" ]
}
EOF
```

wget https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml

