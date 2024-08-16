# Performing a Version Upgrade using Kubeadm
## Steps: Upgrading Control Plane Nodes
- Check the current version:
```
  kubectl version
```

- Drain the control plane node:
```
kubectl drain <node-name> --ignore-daemonsets
```

- Upgrade Kubeadm:
```
# replace x in 1.31.x-* with the latest patch version
sudo apt-mark unhold kubeadm && \
sudo apt-get update && sudo apt-get install -y kubeadm='1.31.x-*' && \
sudo apt-mark hold kubeadm

```

- Plan the Upgrade:
```
kubeadm upgrade plan
```

- Apply the Upgrade:
```
# replace x with the patch version you picked for this upgrade
sudo kubeadm upgrade apply v1.31.x
```

- Upgrade Remaining Control Plane Nodes:
```
sudo kubeadm upgrade node
```

- Upgrade Kubelet and Kubectl:
```
# replace x in 1.31.x-* with the latest patch version
sudo apt-mark unhold kubelet kubectl && \
sudo apt-get update && sudo apt-get install -y kubelet='1.31.x-*' kubectl='1.31.x-*' && \
sudo apt-mark hold kubelet kubectl
```

- Restart the kubelet:
```
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```

- Uncordon the node:
```
# replace <node-to-uncordon> with the name of your node
kubectl uncordon <node-to-uncordon>
```
#  Upgrading Worker Nodes
- Upgrade kubeadm
```
# replace x in 1.31.x-* with the latest patch version
sudo apt-mark unhold kubeadm && \
sudo apt-get update && sudo apt-get install -y kubeadm='1.31.x-*' && \
sudo apt-mark hold kubeadm
```

- Call "kubeadm upgrade"
```
# For worker nodes this upgrades the local kubelet configuration
sudo kubeadm upgrade node
```

- Drain the Node:
```
# execute this command on a control plane node
# replace <node-to-drain> with the name of your node you are draining
kubectl drain <node-to-drain> --ignore-daemonsets
```

- Upgrade the kubelet and kubectl:
```
# replace x in 1.31.x-* with the latest patch version
sudo apt-mark unhold kubelet kubectl && \
sudo apt-get update && sudo apt-get install -y kubelet='1.31.x-*' kubectl='1.31.x-*' && \
sudo apt-mark hold kubelet kubectl
```

- Restart the kubelet:
```
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```

- Uncordon the node:
```
# execute this command on a control plane node
# replace <node-to-uncordon> with the name of your node
kubectl uncordon <node-to-uncordon>
```

Refernce: [Kubeadm Upgrade Guide](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)

