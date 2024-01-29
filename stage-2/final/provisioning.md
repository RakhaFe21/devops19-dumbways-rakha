# Provisioning


**Requirements**

-   Local machine w/ Ansible & Terraform
-   Biznet GIO NEO Lite Servers
    -   Appserver - 2 CPU, 2GB RAM
    -   Gateway - 1 CPU, 1GB RAM

**Instructions**

-   Attach SSH keys & IP configuration to all VMs
-   Server Configuration using Ansible:

[ _All Servers_ ]

-   Docker Engine
-   Node Exporter

[ _Appserver_ ]

-   git repo (dumbmerch)
-   Prometheus & Grafana

[ _Gateway_ ]

-   NGINX/Apache2/Lightspeed
-   Reverse Proxy
-   Wildcard SSL Certificate

-----------------------------------------------------------------------------------
## Creating Servers
#### Biznet Gio
1. Login to Biznet GioCloud on https://portal.biznetgio.com/user/login
2. Go into the menu Compute > NEO Lite
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/raw/main/stage-2/week-1/assets/Screenshot%20from%202023-12-19%2020-10-32.png?raw=true)
3. Create 2 VMs which is named **Appserver** and **Gateway** using token promo that has been given by Dumbways
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/raw/main/stage-2/week-1/assets/Screenshot%20from%202023-12-19%2020-16-23.png?raw=true)
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/raw/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2019-37-15.png?raw=true)
#### Microsoft Azure
1. Create Microsoft Azure account and get the free trial $200 for 1 month
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/raw/main/stage-2/week-3/assets/Screenshot%20from%202024-01-14%2010-14-50.png?raw=true)
2. Setting Terraform environment for Microsoft Azure
- Install Terraform on local computer
- Create a file with name main.tf and configure it
	```sh
	provider  "azurerm" {
	features {}
	} 

	resource  "azurerm_resource_group"  "rg" {
		name =  "rakhafegroup"
		location =  "southeastasia"
	} 

	resource  "azurerm_virtual_network"  "vnet" {
		name =  "count-vnet"
		location = azurerm_resource_group.rg.location
		resource_group_name = azurerm_resource_group.rg.name
		address_space = ["10.0.0.0/16"]
	}
	resource  "azurerm_subnet"  "subnet" {
		name =  "subnet1"
		resource_group_name = azurerm_resource_group.rg.name
		virtual_network_name = azurerm_virtual_network.vnet.name
		address_prefixes = ["10.0.1.0/26"]
	} 
	resource  "azurerm_public_ip"  "my_terraform_public_ip" {
		count =  2
		name =  "myPublicIP-${count.index + 1}"
		location = azurerm_resource_group.rg.location
		resource_group_name = azurerm_resource_group.rg.name
		allocation_method =  "Dynamic"
	}   
	resource  "azurerm_network_interface"  "nic" {
		count =  2
		name =  "nic-${count.index + 1}"
		location =  "southeastasia"
		resource_group_name = azurerm_resource_group.rg.name
		ip_configuration {
			name = "ipconfig1"
			subnet_id = azurerm_subnet.subnet.id
			private_ip_address_allocation = "Dynamic"
			public_ip_address_id = azurerm_public_ip.my_terraform_public_ip[count.index].id
		}
	depends_on = [azurerm_subnet.subnet]
	}
	#The count parameter is used to create multiple instances of a resource.

	resource  "azurerm_virtual_machine"  "vm" {
		count =  2
		name =  "vm-${count.index + 1}"
		location =  "southeastasia"
		resource_group_name = azurerm_resource_group.rg.name
		network_interface_ids = [azurerm_network_interface.nic[count.index].id]
		vm_size =  "Standard_DS1_v2"

	storage_image_reference {
		publisher = "Canonical"
		offer = "0001-com-ubuntu-server-jammy"
		sku = "22_04-lts-gen2"
		version = "latest"
	}
	storage_os_disk {
		name = "myosdisk${count.index}"
		caching = "ReadWrite"
		create_option = "FromImage"
		managed_disk_type = "Standard_LRS"
	}
	os_profile {
		computer_name = "rakha${count.index}"
		admin_username = "rakhafe"
		admin_password = "XXXXXXX"
	}
	os_profile_linux_config {
		disable_password_authentication = false
		}
	}
	```
- The next step is Excecution the file 
	```sh
	$ terraform init # Initialize Terraform
	$ terrafrom plan # Create a Terraform execution plan
	$ terraform apply # Apply a Terraform execution plan
	```
- This method will create a resource group containing the resources that we will use
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2013-56-11.png?raw=true)

## Server Configuration using Ansible 
1. Install Ansible on local computer refers this docs https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/raw/main/stage-2/week-3/assets/Screenshot%20from%202024-01-14%2009-39-59.png?raw=true)
2. Attach the SSH Key on every servers, use the same SSH Key
3. Adding the config ssh                                          
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2014-39-45.png?raw=true)
5. Configuration ansible environment
	- Hosts
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2014-09-46.png?raw=true)
	-  ansible.cfg
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/raw/main/stage-2/week-3/assets/Screenshot%20from%202024-01-14%2009-44-16.png?raw=true)
	-  group_vars/all
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2014-15-29.png?raw=true)
6. Create user ansible playbook
	```sh
	- hosts: all
	  become: true
	  tasks:
	
	  - name: "Add the new user"
	    user:
	      name: '{{username}}'
	      password: "$6$ae9NqHtr3bZhkL8J$7c2oniGemVDGYYWBPRzTdOmegFg3pu1wCa15chUVypVaJIvFqRN6sU2G/OFsdvvUqtekVJ2iFAPJCxWWvCw0L." #pw=ansible123
	      shell: /bin/bash
	      groups: sudo
	      append: yes
	      home: /home/{{username}}
	      state: present
	
	  - name: "create .ssh folder"
	    file: 
	      path: ~{{username}}/.ssh
	      state: directory
	      owner: "{{username}}"
	      group: "{{username}}"
	      mode: 0700
	
	  - name: "create authorized_keys"
	    copy:
	      src: ~/.ssh/id_rsa.pub
	      dest: ~{{username}}/.ssh/authorized_keys
	      owner: "{{username}}"
	      group: "{{username}}"
	      mode: 0600
	  
	  - name: "Configuration PasswordAuthentication"
	    lineinfile:
	      path: '{{item}}'
	      regexp: 'PasswordAuthentication yes'
	      line: 'PasswordAuthentication no'
	    loop:
	      - /etc/ssh/sshd_config
	
	  - name: "Configuration PubkeyAuthentication"
	    lineinfile:
	      path: '{{item}}'
	      regexp: 'PubkeyAuthentication no'
	      line: 'PubkeyAuthentication yes'
	    loop:
	      - /etc/ssh/sshd_config
	
	
	  - name: Disable password authentication for root
	    lineinfile:
	      path: /etc/ssh/sshd_config
	      state: present
	      regexp: '^#?PermitRootLogin'
	      line: 'PermitRootLogin prohibit-password'
	
	  - name: "reload sshd"
	    ansible.builtin.systemd_service:
	      name: sshd
	      state: reloaded
	  
	```
7. Setup ssh ansible playbook
	```sh
	- hosts: all
	  become: true
	  tasks:

	  - name: Change port SSH
	    lineinfile:
	      path: /etc/ssh/sshd_config
	      state: present
	      regexp: 'Port 22'
	      line: 'Port 1234'

	  - name: UFW - Allow SSH connections
	    community.general.ufw:
	      rule: allow
	      port: '1234'


	  - name: UFW - Enable and deny by default
	    community.general.ufw:
	      state: enabled
	      default: deny        
	  
	  - name: "reload sshd"
	    ansible.builtin.systemd_service:
	      name: sshd
	      state: reloaded
	  
	```
8. Install docker ansible playbook
	```sh
	- name: install docker
	  hosts: all
	  become: true
	  tasks:
	  
	  - name: update 
	    apt: 
	      update_cache: yes
	  
	  - name: upgrade
	    apt:
	      upgrade: dist

	  - name: install docker dependencies
	    apt:
	      name:
	        - apt-transport-https
	        - ca-certificates
	        - curl
	        - gnupg-agent
	        - software-properties-common
	      update_cache: yes

	  - name: add docker gpg key
	    apt_key:
	      url: https://download.docker.com/linux/ubuntu/gpg
	      state: present
	      keyring: /etc/apt/keyrings/docker.gpg
	  - name: add docker repository
	    apt_repository:
	      filename: docker 
	      repo: deb [arch=amd64 signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu jammy stable
	      state: present
	  - name: install docker engine
	    apt:
	      name:
	        - docker-ce
	        - docker-ce-cli
	        - containerd.io
	        - docker-buildx-plugin
	        - docker-scan-plugin
	        - docker-compose-plugin
	      update_cache: yes

	  - name: update
	    shell: sudo apt update

	  - name: install docker-compose
	    shell: sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

	  - name: set permission for docker
	    shell: sudo chmod +x /usr/local/bin/docker-compose

	  - name: setup docker command without sudo
	    shell: sudo usermod -aG docker {{username}}
	```
9. Install node exporter ansible playbook
	```sh
	- hosts: 
	  - 103.127.132.48
	  - 103.179.57.85
	  - 4.193.255.29
	  - 4.193.255.26
	  become: true
	  tasks: 


	  - name: "create monitoring folder"
	    file: 
	      path: ~{{username}}/monitoring
	      state: directory
	      owner: '{{username}}'
	      group: '{{username}}'
	      mode: 0700
	    

	  - name: "upload docker compose"
	    copy:
	      src: ./monitoring-app/docker-compose.yaml
	      dest: ~{{username}}/monitoring/docker-compose.yaml
	      owner: '{{username}}'
	      group: '{{username}}'
	      mode: 0664

	  - name: "running node application"
	    shell: cd monitoring/ && docker compose up -d

	```
	we can skip the Appserver because different docker compose. The node exporter from the Appserver will be integrated into Prometheus docker compose
	
10. Format tree of ansible final task folder 
	```  
	├── ansible.cfg
	├── create-user.yaml
	├── docker.yaml
	├── group_vars
	│   └── all
	├── hosts
	├── monitoring-app
	│   └── docker-compose.yaml
	├── node-exporter.yaml
	└── setup-ssh.yaml
	```
11. Appserver tree 
	```sh
	├── apps
	│   └── dumbmerch
	│       ├── be-dumbmerch
	│       ├── docker-compose.yaml
	│       ├── fe-dumbmerch
	└── monitoring
	    ├── docker-compose.yaml
	    ├── grafana
	    └── prometheus
	        ├── prometheus.yml
	        └── web.yml
	```
12.  Gateway tree

		```sh
		├── monitoring
		│   └── docker-compose.yaml
		├── nginx
		│   ├── certbot
		│   │   ├── accounts  
		│   │   ├── archive  
		│   │   ├── live  
		│   │   ├── renewal
		│   │   │   └── rakha.studentdumbways.my.id.conf
		│   │   └── renewal-hooks
		│   │       ├── deploy
		│   │       ├── post
		│   │       └── pre
		│   ├── cloudflare.ini
		│   ├── conf
		│   │   ├── api.conf
		│   │   ├── cadvisor.conf
		│   │   ├── exporter.conf
		│   │   ├── fe.conf
		│   │   ├── graf.conf
		│   │   ├── nginx.conf
		│   │   ├── pgadmin.conf
		│   │   └── prom.conf
		│   └── docker-compose.yaml
		└── renewal
		    └── rakha.studentdumbways.my.id.bash
		```
