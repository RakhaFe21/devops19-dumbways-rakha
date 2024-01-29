# Microservices

**Requirements**

-   k3s/ Vanilla /Docker Swarm
-   Kubernetes Engine (RKE, GKE or AWS EKS)

**Instructions**

-   No 5 (Deployment) app runs inside a kubernetes cluster
-   Apps running 100% with backend and db integration

## Preamble
Master node = rakha1
Worker node = rakha0, reg, gw

## Create Kubernetes cluster 
- Master node Configuration
  Install K3S using the install script 
  ```sh 
  curl -sfL https://get.k3s.io | sh -
  ````
- Konfigurasi worker node
  Install K3S agent using the install script 

	```sh 
	curl -sfL https://get.k3s.io | K3S_URL=https://node1:6443 K3S_TOKEN=node1token sh -
	```
		
	Replace url with master node IP and Token can retrieve from etcd master node
	`cat /var/lib/rancher/k3s/server/token`
- Verify installation
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2023-57-18.png?raw=true)
### Install Ingress NginX
- Disable and uninstall traefik on master and worker node first 
	```sh
	cat /etc/rancher/k3s/config.yaml
	# add configuration with text bellow
	cluster-init: true
	disable: servicelb
	disable: traefik
	```
- Create nginx ingress yaml in `/var/lib/rancher/k3s/server/manifests` directory on master node
	```sh
	apiVersion: v1
	kind: Namespace
	metadata:
	  name: ingress-nginx
	---
	apiVersion: helm.cattle.io/v1
	kind: HelmChart
	metadata:
	  name: ingress-nginx
	  namespace: ingress-nginx
	spec:
	  repo: https://kubernetes.github.io/ingress-nginx
	  chart: ingress-nginx
	  targetNamespace: ingress-nginx
	  valuesContent: |-
	    controller:
	      image:
	        tag: "v1.8.1"
	      service:
	        type: LoadBalancer
	```
- Verify installation
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-28%2000-00-44.png?raw=true)

- Create namespace using command 
	```sh
	 kubectl create namespace rakha
	```
- If we want to deploy an application, just attach file to the manifest folder
