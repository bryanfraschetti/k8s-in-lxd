### On host machine

```
lxc profile create k8s-controller < k8s-controller.yaml
lxc profile create k8s-worker < k8s-worker.yaml
lxc launch --vm ubuntu:noble --profile k8s-controller k8s-controller-1
lxc launch --vm ubuntu:noble --profile k8s-worker k8s-worker-1
```

### On control plane

```
kubeadm init --pod-network-cidr=10.244.0.0/16 --upload-certs
. .bashrc
kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml
```

### On host machine

```
sudo iptables -I FORWARD -p tcp --dport 6443 -j ACCEPT
sudo iptables -I FORWARD -p tcp --sport 6443 -j ACCEPT
```

### On new machines

```
kubeadm join 10.18.55.13:6443 --token <TOKEN> \
        --discovery-token-ca-cert-hash <CA-Hash>
```
