# Day 7
Task:
**Ansible**

[Local] Buat konfigurasi Ansible & sebisa mungkin maksimalkan penggunaan ansbile untuk melakukan semua setup dan se freestyle kalian

[ansible] Buatlah ansible untuk :

-   Membuat user baru, gunakan login ssh key & password
-   Instalasi Docker
-   Deploy application frontend yang sudah kalian gunakan sebelumnya menggunakan ansible.
-   Instalasi Monitoring Server (node exporter, prometheus, grafana)
-   Setup reverse-proxy
-   Generated SSL certificate
-   dan yang paling penting make your own kind ansible script dengan rapi dan jelas. dan sebisa mungkin jangan  **MENCONTEK**  milik teman lain karena script akan terlihat sekali perbedaan nya di materi ansible ini.
-   simpan script kalian ke dalam github dengan format tree sebagai berikut:

## Step by step
### Ansible installation on local computer
Install ansible di ubuntu menggunakan referensi berikut :
https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/Screenshot%20from%202024-01-14%2009-39-59.png?raw=true)
### Configuration ansible environment
- Ansible Environment
	- Hosts
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/Screenshot%20from%202024-01-14%2009-44-23.png?raw=true)
	- ansible.cfg
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/Screenshot%20from%202024-01-14%2009-44-16.png?raw=true)
	- group_vars/all
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/Screenshot%20from%202024-01-14%2009-44-29.png?raw=true)
- Create new user
Saya membagi dua part pada file ini yaitu untuk server azure menggunakan login ssh key dan server biznet menggunakan login password
	-	Server azure
		![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/carbon%20%2842%29.png?raw=true)
	-	Server biznet
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/carbon%20%2843%29.png?raw=true)
- Docker installation 
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/carbon%20%2844%29.png?raw=true)
- Deploy frontend application
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/carbon%20%2845%29.png?raw=true)
- Deploy node exporter 
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/carbon%20%2846%29.png?raw=true)
- Setup reverse proxy with SSL wildcard using docker compose
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/carbon%20%2847%29.png?raw=true)

### Command ansible
- untuk eksekusi 	: `ansible-playbook -K file.yaml` (require sudo password)
- untuk check syntax 	: `ansible-playbook --syntax-check file.yaml`

### Ansible format tree
├── ansible.cfg

├── create-user.yaml

├── deploy.yaml

├── docker.yaml

├── group_vars

│   └── all

├── hosts

├── monitoring-app

│   └── docker-compose.yaml

├── monitoring.yaml

├── reverse-ssl

│   ├── cloudflare.ini

│   ├── docker-compose.yaml

│   └── nginx

│   └── nginx.conf

└── reverse-ssl.yaml

