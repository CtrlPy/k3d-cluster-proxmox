# k3d-cluster-proxmox

install:

curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash

#

touch k3d.yaml

```
kind: Simple
apiVersion: k3d.io/v1alpha2
name: my-cluster
image: rancher/k3s:v1.31.1-k3s1
servers: 1
agents: 0
ports:
  - port: 6443:6443
    nodeFilters:
      - server:0
  - port: 80:80
    nodeFilters:
      - server:0
options:
  k3s:
    extraServerArgs:
      - --tls-san=192.168.1.87
      # - --no-deploy=traefik
```

k3d cluster create --config k3d.yaml

ssh -i ~/.ssh/yoshi-ubuntu-new cat@192.168.1.87

scp -i ~/.ssh/yoshi-ubuntu-new cat@192.168.1.87:/home/cat/.kube/config ~/.kube/proxmox-config.yaml
#
change proxmox-config.yaml  add entry:

server: https://192.168.1.87:36911


#
export KUBECONFIG=~/.kube/config:~/.kube/proxmox-config.yaml
kubectl config view --merge --flatten > ~/.kube/merged-config
mv ~/.kube/merged-config ~/.kube/config

kubectl config get-contexts

kubectl config use-context k3d-my-cluster
