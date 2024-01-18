# Day 9
Sebelum mengerjakan tugas, mohon persiapkan :

-   Akun Github dan buat repository dengan judul "devops19-dumbways-<nama kalian>"
-   Gunakan file README.md untuk isi tugas kalian
-   Buatlah langkah-langkah pengerjaan tugas beserta dokumentasinya

**Repository && Reference:** [Ingress-Nginx](https://docs.nginx.com/nginx-ingress-controller/) [k3s docs](https://docs.k3s.io/) [Alvin Docs](https://github.com/RizqyAlvindra/kubernetes/tree/master/k3s) [Kubernetes Docs](https://kubernetes.io/docs/home/) [mongodb](https://hub.docker.com/_/mongo)

**Kubernetes Tasks**

1.  Buatlah sebuah cluster menggunakan k3s yang berisikan 2 node as a master and worker.
2.  Install ingress nginx using manifest
3.  Deploy aplikasi nginx lalu buatlah suatu ingress yang mengarah ke aplikasi nginx kalian.
4.  Setup persistent volume on k3s config
5.  Deploy aplikasi mongodb database on top kubernetes, menggunakan statefull.set dan juga pasangkan pvc ke dalamnya
6.  Buat database di mongodb kalian, dan coba isi dummy data

## Infrastruktur Project
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-4/assets/1_svd2l7yAE-8LV7pR4bnVIA.png?raw=true)
## Step by step
### Create Kubernetes cluster 
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
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-4/assets/Screenshot%20from%202024-01-18%2019-20-52.png?raw=true)
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
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-4/assets/carbon%20%2858%29.png?raw=true)

- Verify installation	
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-4/assets/Screenshot%20from%202024-01-18%2020-23-03.png?raw=true)
### Create remote between local and kubernetes cluster 
- Retrieve k3s.yaml in master node 
`cat /etc/rancher/k3s/k3s.yaml`
- Create folder and file `.kube/config` and paste config k3s.yaml into it. Don't forget to change url server using master node IP
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-4/assets/Screenshot%20from%202024-01-18%2019-53-30.png?raw=true)
### Deploy NginX application using ingress
- Create file nginx.yaml
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-4/assets/carbon%20%2856%29.png?raw=true)
### Setup persistent volume
- Add configuration file config.yaml
	```sh
	cat /etc/rancher/k3s/config.yaml
	# add configuration with text bellow
	cluster-init: true
	disable: servicelb
	disable: traefik
	default-local-storage-path: /mnt/disk
	```
-  Result
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-4/assets/Screenshot%20from%202024-01-18%2020-30-34.png?raw=true)
### Deploy mongo-db application
- Create pvc mongodb
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-4/assets/carbon%20%2859%29.png?raw=true)
- Create mongodb.yaml
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-4/assets/carbon%20%2860%29.png?raw=true)
- Access the mongodb database and insert dummy data
 	```sh
 	# access the pod of mongodb
	kubectl exec -it  pod/mongo-db-0 -- /bin/bash -n rakha
	# login mongodb
	mongo -u rakha
	password: kakikukaku
	```
- Insert dummy data in test database
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-4/assets/Screenshot%20from%202024-01-18%2020-43-05.png?raw=true)
	
### Deploy Frontend application (additional)
- Create fe.yaml
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-4/assets/carbon%20%2861%29.png?raw=true)
