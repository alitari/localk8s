# k3s

## install

```sh
curl -sfL https://get.k3s.io | sh -
```

## activate

```sh
sudo k3s server --write-kubeconfig kubeconfig.yaml
export KUBECONFIG=$(pwd)/kubeconfig.yaml

```