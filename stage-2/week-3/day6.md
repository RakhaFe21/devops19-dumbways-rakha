# Day 6
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/1_tLaFWxgkVNfYbN3NTKQo-w.gif?raw=true)
Tasks :

-   Setup node-exporter, prometheus dan Grafana menggunakan docker
-   install node-exporter di appserver & gateway
-   Reverse Proxy
    -   bebas ingin menggunakan nginx native / docker
    -   Domain
        -   exporter-$name.studentdumbways.my.id (node exporter)
        -   prom-$name.studentdumbways.my.id (prometheus)
        -   monitoring-$name.studentdumbways.my.id (grafana)
    -   SSL Cloufflare on / certbot SSL biasa / wildcard SSL diperbolehkan
-   Dengan Grafana, buatlah :
    -   Dashboard untuk monitor resource server (CPU, RAM & Disk Usage) buatlah se freestyle kalian.
    -   Buat dokumentasi tentang rumus  `promql`  yang kalian gunakan
    -   Buat alerting dengan Contact Point pilihan kalian (discord, telegram, slack dkk)
    -   Untuk alert :
        -   Boleh menggunakan alert manager / alert rule dari grafana
        -   Ketentuan alerting yang harus dibuat
            -   CPU Usage over 20%
            -   RAM Usage over 75%
    -   Monitoring specific container
    -   deploy application frontend di app-server
        -   monitoring frontend container
   
## Step by step

### Setup environment monitoring
Menggunakan docker compose untuk setup kebutuhan aplikasi yang digunakan untuk monitoring dengan konfigurasi sebagai berikut:
- Script docker compose
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/carbon%20%2838%29.png?raw=true)
- Docker compose on gateway
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/carbon%20%2839%29.png?raw=true)
- Konfigurasi prometheus.yml
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/carbon%20%2841%29.png?raw=true)
	
### Setup reverse proxy on top docker
Menggunakan SSL wildcard dari let's encrypt dengan konfigurasi sebagai berikut:
  - Konfigurasi reverse proxy
	  ![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/carbon%20%2840%29.png?raw=true)
	  
### Create dashboard on Grafana
- CPU Utilization
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/Screenshot%20from%202024-01-10%2023-30-47.png?raw=true)
	- Rumus Promql
		```
		 100 - (avg by(instance)(irate(node_cpu_seconds_total{mode="idle"}[1h]))*100)
		 ```
		 Rumus ini dapat diartikan sebagai 100 dikurangi dengan persentase rata-rata waktu CPU dalam mode 'idle' dalam satu jam terakhir. Semakin tinggi nilai hasil rumus, semakin sedikit waktu CPU yang dihabiskan dalam mode 'idle', yang mungkin mengindikasikan tingkat beban CPU yang lebih tinggi.

- Memory 	
  ![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/Screenshot%20from%202024-01-10%2023-31-04.png?raw=true)
	- Rumus Promql
		```
		100  *  (  1  -  ((avg_over_time(node_memory_MemFree_bytes[10m])  +  avg_over_time(node_memory_Cached_bytes[10m])+  avg_over_time(node_memory_Buffers_bytes[10m]))  /  avg_over_time(node_memory_MemTotal_bytes[10m])))
		```
		Rumus ini dapat diartikan sebagai persentase penggunaan memori terhadap total kapasitas memori yang tersedia dalam interval waktu 10 menit terakhir. Semakin tinggi nilai hasil rumus, semakin tinggi penggunaan memori terhadap kapasitas total yang tersedia.
		

- Disk Usage
  ![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/Screenshot%20from%202024-01-10%2023-31-14.png?raw=true)
	- Rumus Promql
		```
		 node_filesystem_size_bytes{instance="103.127.132.48:9100"}  - node_filesystem_free_bytes {instance="103.127.132.48:9100"}
		 ```
		 rumus ini dapat diartikan sebagai jumlah total ruang pada sistem file dikurangi dengan ruang bebas / free yang masih tersedia pada sistem file pada instance dengan alamat IP 103.127.132.48:9100. Hasilnya memberikan ukuran dari ruang yang telah digunakan pada sistem file tersebut. Semakin tinggi nilainya, semakin banyak ruang yang telah digunakan.
		 
### Monitoring spesific Container
- Frontend monitoring
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/Screenshot%20from%202024-01-11%2005-57-50.png?raw=true)
	- Rumus promql
		```
		 sum  (rate  (container_cpu_usage_seconds_total{image="rakhafe/frontend:54"}[5m]))  by (name)
		 ```
		 rumus ini dapat diartikan sebagai total penggunaan CPU oleh setiap container yang menggunakan image 'rakhafe/frontend:54' dalam lima menit terakhir.
	- Rumus promql jika ingin memantau seluruh container
		```
		 sum  (rate  (container_cpu_usage_seconds_total{image!=""}[5m]))  by (name)
		 ```
		 rumus ini dapat diartikan sebagai total penggunaan CPU oleh setiap container dalam lima menit terakhir, dijumlahkan untuk setiap container.
		 ![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/Screenshot%20from%202024-01-11%2005-58-11.png?raw=true)
	

### Create Alerting to Discord
- Alerting CPU Usage
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/Screenshot%20from%202024-01-11%2006-04-15.png?raw=true)
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/Screenshot%20from%202024-01-11%2006-04-35.png?raw=true)
- Alerting Memory Usage
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/Screenshot%20from%202024-01-11%2006-07-54.png?raw=true)
- Discord contact point
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-3/assets/Screenshot%20from%202024-01-11%2006-09-09.png?raw=true)
