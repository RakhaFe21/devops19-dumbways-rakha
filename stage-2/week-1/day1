# DAY 1 
## Tasks : [ Appserver ]
- Jelasakan langkah-langkah pembuatan VM di BiznetGio
-  Setelah server sudah terbuat, buatlah suatu user baru lalu implementasikan penggunaan ssh-key pada server tersebut.
-  Deploy aplikasi Wayshub-Frontend menggunakan nodejs versi 14.x
-  Clone repository Wayshub frontend lalu deploy aplikasinya menggunakan PM2
- Install nginx
-  Buatlah reverse proxy dan gunakan domain dengan nama kalian (ex. rakha.studentdumbways.my.id)
## Step by step
### Langkah-langkah pembuatan VM pada BiznetGio:
1. Login ke akun yang telah dibuat atau register akun pada halaman https://portal.biznetgio.com/user/login 
2. Masuk pada menu Compute > NEO Lite 
	![Masuk menu NEP Lite](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-19%2020-10-32.png?raw=true)
3. Buat sebuah VM pada NEO Lite dengan menggunakan product XS.11 yang sesuai dengan ketentuan task. Pada pembuatan VM ini saya melakukan create key yang menghasilkan file rakha.pem
	![Buat VM](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-19%2020-14-35.png?raw=true)
 4. Lakukan redeem code 
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-19%2020-16-23.png?raw=true)
5. Masuk pada server secara remote di local dengan menggunakan key yang telah dibuat di NEO Lite
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-19%2020-51-14.png?raw=true)

### Setup server sesuai pada task
1. Membuat user baru bernama esquire
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-19%2021-26-11.png?raw=true)
2. Untuk konfigurasi sshd_config saya biarkan default untuk kepentingan keamanan agar yang bisa akses adalah orang tertentu saja
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-19%2021-01-59.png?raw=true)
3. Melakukan update dan upgrade pada server agar package-package yang ada tetap up to date
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-19%2021-20-55.png?raw=true)
4. Membuat file authorized_keys pada user baru lalu memasukkan ssh key public dari komputer local kita kedalamnya. Tujuannya yaitu untuk akses server secara ssh dari local kita
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2006-47-33.png?raw=true)
### Deploy aplikasi menggunakan nodejs versi 14 dengan npm start
1. Langkah pertama yaitu membuat directory khusus apps dan selanjutnya clone aplikasi didalam directory tersebut
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-19%2022-07-41.png?raw=true)
2. Selanjutnya yaitu install nvm lalu install nodejs versi 14 menggunakan nvm
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-19%2022-08-51.png?raw=true)
3. Lakukan npm start pada directory aplikasi dan aplikasi telah terdeploy
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2006-57-25.png?raw=true)
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2006-56-58.png?raw=true)
### Deploy aplikasi dengan PM2
1. Tujuan dari penggunaan package pm2 ini adalah agar aplikasi berjalan di background dan tidak mengganggu terminal. Langkah pertama yaitu install PM2 menggunakan npm (node package manager) 
`npm install pm2 -g`
2. Selanjutnya membuat ecosystem dengan perintah 
` pm2 init simple` 
3. Edit script pada file ecosystem.config.js sesuai dengan yang kita inginkan
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2007-07-39.png?raw=true)
4. Jalankan aplikasi dengan perintah 
	`pm2 start ecosystem.config.js`
	dan aplikasi telah terdeploy
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-19%2022-16-24.png?raw=true)
### Installasi Nginx
1. Install nginx dengan perintah sudo `apt install nginx -y`
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-19%2022-19-26.png?raw=true)

### Konfigurasi reverse proxy
1. Membuat file konfigurasi pada directory /etc/nginx/apps dengan nama file rakha.studentdumbways.my.id (sesuai dengan domain) lalu isikan konfigurasi seperti berikut
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-19%2022-31-38.png?raw=true)
2. Buat domain pada cloudflare dengan IP yang dipointing ke server kita
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2007-17-06.png?raw=true)
3. Tambahkan directory include pada konfigurasi nginx.conf
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2007-26-15.png?raw=true)
4. Reload dan restart nginx menggunakan perintah 
	`sudo systemctl reload nginx`
	`sudo systemmctl restart nginx`
5. Aplikasi telah terdeploy menggunakan domain kita dan sudah ter secure menggunakan proxy dari cloudflare
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-19%2022-31-24.png?raw=true)
