# K3S on x86_64 bare metal machines, or Virtual Machines

master node = x86_64
worker node = x86_64

## Master node install/uninstall

### Master node Install

```bash
curl -sfL https://get.k3s.io | sh -
```

Label master

```bash
kubectl label node ubuntu-master kubernetes.io/role=master
kubectl label node ubuntu-master node-role.kubernetes.io/master=""
```

Do not schedule app pod on master node 

```bash
kubectl taint nodes ubuntu-master node-role.kubernetes.io/master=effect:NoSchedule
```

```bash
sudo cat /var/lib/rancher/k3s/server/node-token
sudo k3s kubectl get node -o wide
```

Label worker node 

``` bash
kubectl label node ubuntu-node1 kubernetes.io/role=node
kubectl label node ubuntu-node1 node-role.kubernetes.io/node=""
```

### Master node Uninstall

``` bash
/usr/local/bin/k3s-killall.sh
/usr/local/bin/k3s-uninstall.sh
```

## Worker node agent install/uninstall

### Worker node Install

```bash
export NODE_TOKEN=

curl -sfL https://get.k3s.io | K3S_URL=https://ubuntu-master:6443 K3S_TOKEN=${NODE_TOKEN} sh -
```

Using docker

```bash
curl -sfL https://get.k3s.io | K3S_URL=https://ubuntu-master:6443 K3S_TOKEN=${NODE_TOKEN} sh -s - --docker
```

### Worker node Uninstall

```bash
/usr/local/bin/k3s-killall.sh
/usr/local/bin/k3s-agent-uninstall.sh
```
