# k3d

## install

```sh
wget https://github.com/k3d-io/k3d/releases/download/v5.4.1/k3d-linux-amd64
mv k3d-linux-amd64 k3d
chmod +x k3d
sudo mv k3d /usr/bin/k3d
```

## activate

```sh
touch kubeconfig.yaml
export KUBECONFIG=$(pwd)/kubeconfig.yaml
k3d cluster create mycluster

```