# MicroK8s

## install 

```sh
sudo snap install microk8s --classic
sudo microk8s status --wait-ready
# no sudo
sudo usermod -a -G microk8s $USER
sudo chown -f -R $USER ~/.kube
newgrp microk8s
# store config
microk8s config > kubeconfig.yaml
export KUBECONFIG=$(pwd)/kubeconfig.yaml
kubectl get nodes
```

## activate

```sh
microk8s start
## dns
microk8s enable dns

```
## checks

```sh
kubectl-ns default
kubectl create deployment echoserver --image=k8s.gcr.io/echoserver:1.4 --port=8080
kubectl expose deployment echoserver --type=ClusterIP --port=8070 --target-port=8080
kubectl create deployment busybox --image=busybox -- sleep 3600
kubectl exec -it deploy/busybox sh
# in the pod
wget http://echoserver:8070 && cat index.html
```

## nginx ingress controller

```sh
microk8s enable ingress
# uses sevice abbove
kubectl apply -f ingress.yaml
http http://echoserver.127.0.0.1.nip.io
```

## storage

```sh
microk8s enable storage
kubectl apply -f persistence_hostpath.yaml
kubectl exec -it deploy/busybox-with-vol sh
# in the container
touch /host/alex
exit
# on your host
ls -l hostvolume
```

## deactivate

```sh
microk8s stop
```

