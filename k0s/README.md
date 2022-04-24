# k0s

## install

```sh
wget https://github.com/k0sproject/k0s/releases/download/v1.23.5%2Bk0s.0/k0s-v1.23.5+k0s.0-amd64
mv k0s-v1.23.5+k0s.0-amd64 k0s
chmod +x k0s
sudo mv k0s /usr/local/bin/k0s
```

## activate

```sh
sudo k0s install controller --single
sudo k0s status
sudo k0s kubeconfig admin > kubeconfig.yaml
export KUBECONFIG=$(pwd)/kubeconfig.yaml
kubectl get nodes
```


## deactivate

```sh
sudo k0s stop
sudo k0s reset
```