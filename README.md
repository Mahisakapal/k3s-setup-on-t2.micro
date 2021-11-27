# Rancher's k3s - First steps
# k3s-setup-on-t2.micro

no need docker installed or kubectl installed

Lightweight, easy, fast Kubernetes distribution with a very small footprint

https://k3s.io or https://k3d.io for the dockerized version of k3s.

## Demo

I'll use Linode for that demo.

### Install Master

`curl -sfL https://get.k3s.io | sh -s - --no-deploy traefik --write-kubeconfig-mode 644 --node-name k3s-master-01`

## what ever name we want to give our worker node make sure it's not repiting when join node we have to give same name here we use k3s-master-01 so in join cmd we use same name 

### Install Worker

Grab token from the master node to be able to add worked nodes to it: 

`cat /var/lib/rancher/k3s/server/node-token`

Install k3s on the worker node and add it to our cluster:

`curl -sfL https://get.k3s.io | K3S_NODE_NAME=k3s-worker-01 K3S_URL=https://<IP>:6443 K3S_TOKEN=<TOKEN> sh - `

## here we have to give master node name we give k3s-worker-01 becouse when we create master we use same name.
## and master node privite or publice ip in place of <ip> and token what we got by abow cmd in place of <TOKEN>

### Install nginx ingress controller


Website: https://kubernetes.github.io/ingress-nginx/

`kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.47.0/deploy/static/provider/cloud/deploy.yaml`

### Additional information

You can change the settings of k3s by changing the service settings e.g. with `nano /etc/systemd/system/k3s.service`.

Make sure to restart the service afterwards: `systemctl restart k3s`

In many cases you can just run the installer with different variables again and it will configure your cluster accordingly without deleting it in the first place.

### Deploy demo application Deploy demo application is in same repo. in manifest.yaml there in <your_ip> we have to give our master server publice ip

## Note : in ingress we have give public dns junrraly we got that from aws but if not work you can give publice_ip.nip.io (ex: 30.4.76.2.nip.io)


`kubectl apply -f manifest.yaml`

