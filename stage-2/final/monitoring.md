# Monitoring
**Requirements**

-   Deployments on top Docker

**Instructions**

-   Create Basic Auth into your Prometheus
-   Monitor resources for  _Appserver & Gateway & Registry server_
-   Create a fully working dashboard in Grafana
    -   Disk
    -   Memory Usage
    -   CPU Usage
    -   VM Network
    -   Monitoring all of container resources on VM
-   Grafana Alert/Prometheus Alertmanager for:
    -   Send Notification to Telegram
    -   CPU Usage
    -   RAM Usage
    -   Free Storage
    -   Network I/O (NGINX Monitoring)
## Setup Environment monitoring on monitoring master node
- Create folder monitoring and create docker compose file into it
	```sh
	version: '3.8'

	volumes:
	  prometheus_data: {}

	services:
	  node-exporter:
	    image: prom/node-exporter:latest
	    container_name: node-exporter
	    restart: unless-stopped
	    volumes:
	      - /proc:/host/proc:ro
	      - /sys:/host/sys:ro
	      - /:/rootfs:ro
	    command:
	      - '--path.procfs=/host/proc'
	      - '--path.rootfs=/rootfs'
	      - '--path.sysfs=/host/sys'
	      - '--collector.filesystem.mount-points-exclude=^/(sys|proc|dev|host|etc)($$|/)'
	    ports:
	      - 9100:9100
	    networks:
	      - monitoring

	  prometheus:
	    image: prom/prometheus:latest
	    container_name: prometheus
	    restart: unless-stopped
	    volumes:
	      - ./prometheus:/etc/prometheus/

	    command:
	      - '--config.file=/etc/prometheus/prometheus.yml'
	      - '--web.config.file=/etc/prometheus/web.yml'
	      - '--storage.tsdb.path=/prometheus'
	      - '--web.console.libraries=/etc/prometheus/console_libraries'
	      - '--web.console.templates=/etc/prometheus/consoles'
	      - '--web.enable-lifecycle'
	    ports:
	      - 9090:9090
	    networks:
	      - monitoring

	  grafana:
	    image: grafana/grafana:latest
	    container_name: grafana
	    restart: unless-stopped
	    ports:
	      - 3001:3000
	    user: root
	    volumes:
	      - ./grafana/data:/var/lib/grafana  
	      - ./grafana/provisioning/datasource:/etc/grafana/provisioning/datasource
	    networks:
	      - monitoring

	  cadvisor:
	    container_name: cadvisor
	    image: gcr.io/cadvisor/cadvisor:latest
	    ports:
	      - "8080:8080"
	    privileged: true
	    devices: 
	      - "/dev/kmsg" 
	    volumes: 
	      - "/:/rootfs:ro"
	      - "/var/run:/var/run:rw"
	      - "/sys:/sys:ro"
	      - "/var/lib/docker/:/var/lib/docker:ro"
	      - "/dev/disk/:/dev/disk:ro"
	    networks:
	      - monitoring
	    
	networks:
	  monitoring:
	    driver: bridge
	```
- Create prometheus.yml for connection node which would took the metrics
	```sh
	global:
	  scrape_interval: 10s

	scrape_configs:

	  - job_name: 'prometheus'
	    scrape_interval: 5s
	    static_configs:
	      - targets: 
	        - 103.127.132.63:9090

	  - job_name: 'node'
	    static_configs:
	      - targets:
	        - 103.127.132.63:9100
	        - 103.127.132.48:9100
	        - 103.179.57.85:9100
	        - 4.193.255.29:9100
	        - 4.193.255.26:9100
	  
	  - job_name: 'cadvisor'
	    static_configs:
	      - targets:
	        - 103.127.132.63:8080
	        - 103.127.132.48:8080
	        - 103.179.57.85:8081
	        - 4.193.255.29:8080
	        - 4.193.255.26:8080
	```
- Create web.yml for basic auth prometheus
	```sh
	basic_auth_users:
	    admin: $2b$12$hNf2lSsxfm0.i4a.1kVpSOVyBCfIB51VRjgBUyv6kdnyTlgWj81Ay # test
	```
## Setup environment monitoring on member node
- Create folder monitoring adn docker compose which is that has already been made by ansible. As for the contents docker compose for node member is :
	```sh
	version: '3.8'

	volumes:
	  prometheus_data: {}

	services:
	  node-exporter:
	    image: prom/node-exporter:latest
	    container_name: node-exporter
	    restart: unless-stopped
	    volumes:
	      - /proc:/host/proc:ro
	      - /sys:/host/sys:ro
	      - /:/rootfs:ro
	    command:
	      - '--path.procfs=/host/proc'
	      - '--path.rootfs=/rootfs'
	      - '--path.sysfs=/host/sys'
	      - '--collector.filesystem.mount-points-exclude=^/(sys|proc|dev|host|etc)($$|/)'
	    ports:
	      - 9100:9100
	    networks:
	      - monitoring

	  cadvisor:
	    container_name: cadvisor
	    image: gcr.io/cadvisor/cadvisor:latest
	    ports:
	      - "8080:8080"
	    privileged: true
	    devices: 
	      - "/dev/kmsg" 
	    volumes: 
	      - "/:/rootfs:ro"
	      - "/var/run:/var/run:rw"
	      - "/sys:/sys:ro"
	      - "/var/lib/docker/:/var/lib/docker:ro"
	      - "/dev/disk/:/dev/disk:ro"
	    networks:
	      - monitoring
	      
	networks:
	  monitoring:
	    driver: bridge
	```
## Create new dashboard grafana
Access dashboard grafana using domain monitoring.rakha.studentdumbways.my.id and create new dashboard
- CPU utilization
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2023-08-14.png?raw=true)
	- Promql formula
		```sh
		  100 - (avg by(instance)(irate(node_cpu_seconds_total{mode="idle"}[1h]))*100)
		```
- Memory usage
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2023-12-01.png?raw=true)
	- Promql formula
		```sh
		 100  *  (  1  -  ((avg_over_time(node_memory_MemFree_bytes[10m])  +  avg_over_time(node_memory_Cached_bytes[10m])+  avg_over_time(node_memory_Buffers_bytes[10m]))  /  avg_over_time(node_memory_MemTotal_bytes[10m])))
		```
		```sh
		avg_over_time(node_memory_MemAvailable_bytes[5m])/1024/1024
		```
- Network I/O VMs
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2023-14-17.png?raw=true)
	-	 Promql formula for Network Input 
			```sh
			sum(rate(node_network_receive_bytes_total[1m])) by (device, instance) * on(instance) group_left(nodename) (node_uname_info)
			```
	- 	Promql formula for Network output
  
	  		```sh
			sum(rate(node_network_transmit_bytes_total[1m])) by (device, instance) * on(instance) group_left(nodename) (node_uname_info)

  			```
- Disk available
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2023-19-17.png?raw=true)
	- Promql formula
		```sh
		node_filesystem_free_bytes{mountpoint="/"} * on(instance) group_left(nodename) (node_uname_info)
		```
- Container CPU 
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2023-21-30.png?raw=true)
	- Promql formula
		```sh
		sum  (rate  (container_cpu_usage_seconds_total{image!=""}[5m]))  by (name)
		```
- Container Nginx I/O
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2023-23-26.png?raw=true)
	- Promql formula
		```sh
		sum by (name) (rate(container_network_receive_bytes_total{name="nginx-nginx-1"} [1m] ) )
		```
		```sh
		sum by (name) (rate(container_network_transmit_bytes_total{name="nginx-nginx-1"} [1m] ) )
		```
## Create notification
- First, create Bot telegram using BotFather and get ID using Get ID bot with /start command
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2023-28-16.png?raw=true)
- Create new contact point on grafana
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2023-26-53.png?raw=true)
- Setting contact point grafana to default
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2023-32-15.png?raw=true)

- Create alert rules like this 
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2023-34-07.png?raw=true)
- Result on telegram
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/final/assets/Screenshot%20from%202024-01-27%2023-36-04.png?raw=true)
