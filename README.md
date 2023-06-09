# Rancher's k3s - First steps

Lightweight, easy, fast Kubernetes distribution with a very small footprint

https://k3s.io or https://k3d.io for the dockerized version of k3s.

## Demo

I'll use Linode for that demo.

### Install Master

`curl -sfL https://get.k3s.io | sh -s - --disable traefik --write-kubeconfig-mode 644 --node-name k3s-master-01`

### Install Worker

Grab token from the master node to be able to add worked nodes to it: 

`cat /var/lib/rancher/k3s/server/node-token`

Install k3s on the worker node and add it to our cluster:

`curl -sfL https://get.k3s.io | K3S_NODE_NAME=k3s-worker-01 K3S_URL=https://<IP>:6443 K3S_TOKEN=<TOKEN> sh - `

### Install nginx ingress controller


Website: https://kubernetes.github.io/ingress-nginx/

`kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.2.1/deploy/static/provider/cloud/deploy.yaml`

#### Important note for AWS, Azure, GCP

I can't explain it in detail but if you run a VM on AWS, Azure or GCP the "ingress-nginx" will just pick up the private IP as external IP. You need to manually edit the service to make your public IP address available for nginx, so it can start listening on your public IP address.

`kubectl -n ingress-nginx edit svc ingress-nginx-controller`

Add your IP as list to `spec.externalIPs` like so:

```
  spec:
    externalIPs:
    - <YOUR_IP>
```

### Additional information

You can change the settings of k3s by changing the service settings e.g. with `nano /etc/systemd/system/k3s.service`.

Make sure to restart the service afterwards: `systemctl restart k3s`

In many cases you can just run the installer with different variables again and it will configure your cluster accordingly without deleting it in the first place.

### Deploy demo application

`kubectl apply -f manifest.yaml`

