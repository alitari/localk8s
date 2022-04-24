# minikube

## install

```sh
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

## activate

```sh
touch kubeconfig.yaml
export KUBECONFIG=$(pwd)/kubeconfig.yaml
minikube start
kubectl-ctx minikube
kubectl get nodes
```

## some checks

### servicetype nodeport

```sh
kubectl-ns default
kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
kubectl expose deployment hello-minikube --type=NodePort --port=8080
kubectl port-forward service/hello-minikube 7080:8080
# browse to http://localhost:7080
```
### dashboard

```sh
minikube dashboard
```


### servicetype loadbalancer

```sh
kubectl-ns default
kubectl create deployment balanced --image=k8s.gcr.io/echoserver:1.4  
kubectl expose deployment balanced --type=LoadBalancer --port=8080
# get external ip for loadbalancer
minikube tunnel
export EXT_IP=$(kubectl get svc balanced -o=json | jq -r ".status.loadBalancer.ingress[0].ip")
http http://${EXT_IP}:8080
# browse to http://localhost:7080
```

## mgnmt

```sh
# pause
minikube pause
# stop
minikube stop
# status
minikube status
# profiles
minikube profile list
minikube start -p myprofile
minikube delete -p minikube
```

## using Kong ingress controller addon

```sh
# pause
minikube start -p kic
minikube profile kic
minikube addons enable kong
# in another terminal
minikube tunnel
kubectl-ns kong
export KONG_IP=$(kubectl get svc kong-proxy -o=json | jq -r ".status.loadBalancer.ingress[0].ip")
http http://kong.${KONG_IP}.nip.io
```

## registries

### without registry

load images directly in cluster

```sh
docker pull busybox 
docker tag busybox alexkrieg.com/mybusybox
minikube image load alexkrieg.com/mybusybox
kubectl-ns default
kubectl apply -f busybox-deploy.yaml
```


