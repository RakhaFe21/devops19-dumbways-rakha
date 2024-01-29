# Web Server
**Requirements**

-   NGINX/Apache2/Lightspeed on Gateway
-   SSL Certbot using Wildcard
-   Automatic SSL (Ansible/Cronjob/Script etc.)

**Instructions**

-   Create domains:
    -   <name>.studentdumbways.my.id - App
    -   api.<name>.studentdumbways.my.id - Backend API
    -   exporter.<name>.studentdumbways.my.id - Node Exporter
    -   prom.<name>.studentdumbways.my.id - Prometheus
    -   monitoring.<name>.studentumbways.my.id - Grafana
    -   registry.<name>.studentdumbways.my.id - Docker Registry
-   All domains are HTTPS
-   Create Bash Script for Automatic renewal for Certificates

## Create domain required on cloudflare
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2023-42-27.png?raw=true)
## Set up gateway server
- Create folder nginx and create docker compose into it
	```sh
	version: '3'
	services:

	  nginx:
	    image: nginx
	    user: root
	    ports:
	      - "80:80"
	      - "443:443"
	    restart: "unless-stopped"
	    networks:
	      nginx:
	    volumes:
	      - ./conf/:/etc/nginx/conf.d/
	      - ./certbot/:/etc/letsencrypt

	  certbot:
	    image: certbot/dns-cloudflare
	    volumes:
	      - ./certbot:/etc/letsencrypt
	      - ./cloudflare.ini:/root/cloudflare.ini
	    command: >-
	      certonly --dns-cloudflare
	      --dns-cloudflare-credentials /root/cloudflare.ini
	      --dns-cloudflare-propagation-seconds 15
	      --email jagadsatya1103@gmail.com
	      --agree-tos --no-eff-email
	      --force-renewal
	      -d rakha.studentdumbways.my.id
	      -d *.rakha.studentdumbways.my.id
	    networks:
	      nginx:

	networks:
	  nginx:
	```
- Create cloudflare.ini into nginx folder
	```sh
	dns_cloudflare_email = "jagadsatya1103@gmail.com"
	dns_cloudflare_api_key= "61c714f0da1544b03a3340e47194001edbf0a"
	```
-  Create conf folder and create file *.conf for reverse proxy
	```sh
	conf/
	├── api.conf
	├── cadvisor.conf
	├── exporter.conf
	├── fe.conf
	├── graf.conf
	├── nginx.conf
	├── pgadmin.conf
	├── prom.conf
	├── stag-be.conf
	└── stag-fe.conf
	```
	example configuration (other configurations are similar)
	```sh
	server {
	        listen 80;
	        listen 443 ssl;
	        server_name rakha.studentdumbways.my.id;

	        ssl_certificate /etc/letsencrypt/live/rakha.studentdumbways.my.id/fullchain.pem;
	        ssl_certificate_key /etc/letsencrypt/live/rakha.studentdumbways.my.id/privkey.pem;

	        location / {
	                proxy_pass http://103.127.132.63:3000/;
	                proxy_set_header Host $host;
	                proxy_set_header X-Real-IP $remote_addr;
	        }
	}

	```

## Automatic renewal for certificates
- Adding crontab with command `crontab -e`
  ![enter image description here](https://raw.githubusercontent.com/RakhaFe21/devops19-dumbways-rakha/main/stage-2/final/assets/Screenshot%20from%202024-01-28%2006-16-43.png)
	```sh
	5 23 21 */3 *
	```
 which mean "At 23:05 on day-of-month 21 in every 3rd month."
- create file bash to execute renewal
  ![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-28%2006-17-11.png?raw=true)
