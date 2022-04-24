# [kind](https://kind.sigs.k8s.io/)

Kind stands for **k**ubernetes **in** **d**ocker, and enables a quick way to spawn a K8s cluster on your local workstation. 
See [here](https://kind.sigs.k8s.io/docs/user/quick-start#installing-from-release-binaries) how to install.


## simple 

```bash
kind create cluster --name kind1 --config cluster.yaml
kubectl-ctx kind-kind1
```

## multi

```bash
# create cluster
kind create cluster --name kind31 --config cluster-3-1.yaml
kubectl-ctx kind-kind31
```

## with local registry

The hostname `kind-registry` must resolve to `127.0.0.1 ( localhost)`. Edit your `/etc/hosts` file accordingly

```bash
# run registry container on port 5000
docker run -d --restart=always -p "127.0.0.1:5000:5000" --name "kind-registry" registry:2
# create cluster
kind create cluster --name kind-reg --config cluster-registry.yaml
# connect registry to cluster-network
docker network connect "kind" "kind-registry"
# ctx
kubectl-ctx kind-kind-reg
# config
kubectl apply -f registry-configmap.yaml
# test
docker pull alpine
docker tag alpine kind-registry:5000/alpine
docker push kind-registry:5000/alpine
kubectl run -i -t alpine --image=kind-registry:5000/alpine --restart=Never -- sh
```


## install ingress-controller

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/kind/deploy.yaml
```

##  deploy echo services

```bash
kubectl apply -f echo.yaml
# call through ingress controller
curl -v localhost/foo
curl -v localhost/bar
```

