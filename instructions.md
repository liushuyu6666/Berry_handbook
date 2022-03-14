# Ubuntu Server on raspberry pi
# Microk8s cluster

## overview
After install Ubuntu Server 64-bit, you can install microk8s on each node. Please follow this [how to build microk8s cluster](https://ubuntu.com/tutorials/how-to-kubernetes-cluster-on-raspberry-pi#1-overview)

## steps

1. install microk8s by running command 

	```
	sudo snap install microk8s --classic --channel=1.19/stable
	```

	where 1.19 specify the version of the microk8s.

2. For convenience, change the hostname and add microk8s to your `sudo`:

	```
	sudo usermod -a -G microk8s ubuntu && sudo reboot
	```
3. check the status by

	```
	microk8s status
	```
4. run `microk8s add-node` on the master and add node by running `microk8s join `
5. when you need to add another additional node, run commands in the step 3 again
6. disable sleep or suspend on each node

	```
	sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
	```

## check
1. if you install the version 1.9, when you run `microk8s status`, you can get the detail like this: if you are using any higher system, you should get the same output.

	```
	microk8s is running
	high-availability: no
	  datastore master nodes: 127.0.0.1:19001
	  datastore standby nodes: none
	addons:
	  enabled:
	    ha-cluster           # Configure high availability on the current node
	  disabled:
	    dashboard            # The Kubernetes dashboard
	    dns                  # CoreDNS
	    helm                 # Helm 2 - the package manager for Kubernetes
	    helm3                # Helm 3 - Kubernetes package manager
	    host-access          # Allow Pods connecting to Host services smoothly
	    ingress              # Ingress controller for external access
	    linkerd              # Linkerd is a service mesh for Kubernetes and other frameworks
	    metallb              # Loadbalancer for your Kubernetes cluster
	    metrics-server       # K8s Metrics Server for API access to service metrics
	    rbac                 # Role-Based Access Control for authorisation
	    registry             # Private image registry exposed on localhost:32000
	    storage              # Storage class; allocates storage from host directory
	```
which indicates that the ha-cluster has already been enabled.

## reference
1. [what is high availability](https://microk8s.io/high-availability)
2. [how to build the microk8s cluster](https://ubuntu.com/tutorials/how-to-kubernetes-cluster-on-raspberry-pi#1-overview)
3. [microk8s tutorial](https://microk8s.io/docs/getting-started)

# microk8s dashboard

## overview
dashboard provide a visual panel of the kubernetes cluster from remote browser

## steps

1. enable `dashboard` and `dns`

	```
	microk8s enable dashboard dns
	```
2. check services on kubernete

	```
	microk8s kubectl get services -n kube-system
	```
	the output should be like this

	```
	NAME                        TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                  AGE
metrics-server              ClusterIP   10.152.183.53    <none>        443/TCP                  14m
kubernetes-dashboard        ClusterIP   10.152.183.111   <none>        443/TCP                  13m
dashboard-metrics-scraper   ClusterIP   10.152.183.223   <none>        8000/TCP                 13m
kube-dns                    ClusterIP   10.152.183.10    <none>        53/UDP,53/TCP,9153/TCP   8m43s
	```
3. get the token

	```
	microk8s kubectl -n kube-system describe secret $(microk8s kubectl -n kube-system get secret | grep default-token | awk '{print $1}')
	```
4. set port-forwarding to enable external access

	```
	microk8s kubectl port-forward -n kube-system service/kubernetes-dashboard --address 0.0.0.0 10443:443
	```
	or run it at backend ???
	```
	microk8s kubectl port-forward -n kube-system service/kubernetes-dashboard --address 0.0.0.0 10443:443 &&
	```
5. then it is accessible from the remote browser by this URL, only the firefox browser can work, so you need to download firefox.

	```
	https://(MicroK8s primary node's Hostname or IP address):10443/
	```
	
	
## reference
1. [the most important one](https://www.server-world.info/en/note?os=Ubuntu_20.04&p=microk8s&f=4)	

# install openFAAS
## 

## troubleshooting
