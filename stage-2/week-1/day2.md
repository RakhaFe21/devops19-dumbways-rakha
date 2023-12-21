# DAY 2 
## Tasks : Deploy aplikasi Backend
- Jelaskan langkah-langkah pembuatan VM di BiznetGio
- Setelah server sudah terbuat, buatlah suatu user baru lalu implementasikan penggunaan ssh config pada server tersebut.
- Deploy database mysql
	- Setup secure_installation
	- Add password for `root` user
	- Create new user for mysql
	- Create new database
	- Create privileges for new users so they can access the database you created
	- Dont forget to change the mysql bind address on `/etc/mysql/mysql.conf.d/mysqld.cnf`
	- Try to remote your database from your local computer or gateway server

- Deploy aplikasi Wayshub-Backend 
	- Clone wayshub backend application
	- Use Node Version 14
	- Dont forget to change configuration on `wayshub-backend/config/config.json` and then adjust it to your database.
	- Install sequelize-cli 
	- Running migration
	- Deploy apllication on Top PM2

- Deploy aplikasi Wayshub-Frontend
	- Clone wayshub frontend application
	- Dont forget to change configuration on `wayshub-frontend/src/config/api.js` and then adjust it to your backend application.
	- Deploy apllication on Top PM2

- Web Server
	- Frontend (ex. amanda.studentdumbways.my.id)
	- Backend (ex. api.amanda.studentdumbways.my.id)
	- SSL CLOUDFLARE OFF, use certbot for generate certificate. Below is an example of a template for using API keys in Cloudflare using Certbot
	```sh
	dns_cloudflare_email = "demo.dumbways@gmail.com"
	dns_coudflare_api_key = "000safaAGTy21666999LLop20521esWa"
	```
## Step by step
### Langkah-langkah pembuatan VM pada BiznetGio:
1. Login ke akun yang telah dibuat atau register akun pada halaman https://portal.biznetgio.com/user/login 
2. Masuk pada menu Compute > NEO Lite. Buat sebuah VM pada NEO Lite dengan menggunakan product SS 2.2 yang sesuai dengan ketentuan task. Pada pembuatan VM ini saya menggunakan key yang telah dibuat pada task 1 yaitu rakha.pem
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2019-36-02.png?raw=true)
 4. Lakukan redeem code 
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2019-37-15.png?raw=true)
5. Masuk pada server secara remote di local dengan menggunakan key rakha.pem
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2019-40-31.png?raw=true)

### Setup server sesuai pada task menggunakan ssh config
1. Membuat user baru bernama rasendriya
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2019-47-23.png?raw=true)
2. Selanjutnya saya melakukan sebuah konfigurasi ssh config untuk menghubungkan 2 user masing-masing menuju ke 2 server yaitu server github.com dan gitlab.com. Sebelum melakukan konfigurasi ini, masukkan key dari masing-masing user ke folder yang akan dibuat ssh config.
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2020-10-57.png?raw=true)
3. Masukkan public key dari masing-masing user kedalam server yang dituju
User : febryza![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2020-02-48.png?raw=true)
User : rasendriya
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2020-03-40.png?raw=true)
4. Terdapat dua user yaitu febryza dan rasendriya
	febryza terhubung ke github
	rasendriya terhubung ke gitlab
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2020-08-33.png?raw=true)
5. Test clone repository menggunakan ssh
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2020-10-19.png?raw=true)

### Deploy Database MySQL
1. Lakukan update dan upgrade server dengan perintah 
`sudo apt update; sudo apt upgrade -y`
2. Install MySQL dengan perintah 
`sudo apt install mysql-server`
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2020-30-05.png?raw=true)
3. Konfigurasi sekuritas mysql dengan perintah 
	`sudo mysql_secure_installation`
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2020-34-54.png?raw=true)
4. Menambahkan password pada root
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2021-20-02.png?raw=true)
akses database hanya bisa menggunakan password
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2021-22-53.png?raw=true)
5. Membuat user baru yang nantinya akan dibuat untuk akses database. Saya membuat user bernama rakha dengan host % (anyhost)
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2021-25-13.png?raw=true)
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2021-27-04.png?raw=true)
6. Membuat database baru bernama wayshub
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2021-29-36.png?raw=true)
7. Menambahkan privilege untuk user rakha agar dapat memiliki akses ke database wayshub
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2021-38-28.png?raw=true)
8. Akses mysql melalui user rakha lalu menampilkan database yang dapat diakses![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2021-38-54.png?raw=true)
9. Konfigurasi mysql bind agar dapat diakses oleh semua IP (0.0.0.0). Edit file berikut	`/etc/mysql/mysql.conf.d/mysqld.cnf` 
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2021-41-23.png?raw=true) 
	
10.  Akses database secara remote menggunakan software bernama MySQLWorkbench. Software ini nantinya akan memudahkan user dalam memanage database karena berbasis GUI (Graphical User Interface)
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2021-42-56.png?raw=true)
11. Konfigurasi setup configuration dan masukkan parameters dengan konfigurasi yang telah dilakukan sebelumnya. Masukkan username dan password dari user mysql yang tdai telah dibuat
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2021-43-56.png?raw=true)
12. Akses database melalui MySQLWorkbench![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2021-44-14.png?raw=true)
### Deploy aplikasi Wayshub-Backend
1. Langkah pertama clone aplikasi dari resource yang telah disediakan
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2021-51-47.png?raw=true)
2. Konfigurasi pada `config/config.json` dengan menyesuaikan database yang telah dibuat
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2021-56-44.png?raw=true)
3. Gunakan node versi 14 lalu install package sequelize-cli![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-02-07.png?raw=true)
4. Lakukan migrasi database dengan perintah
 `npx sequelize db:migrate`
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-03-23.png?raw=true)
5. Test apakah database sudah berhasil terhubung atau tidak melalui MySQLWorkbench![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-03-32.png?raw=true)
	Semula yang databasenya kosong sekarang sudah terisi, yang membuktikan bahwa konfigurasi penghubungan antara database dan backend telah berhasil
6. Tambahkan pm2 ecosystem simple lalu jalankan ![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-06-16.png?raw=true)
7. Test web pada browser dengan memanggil IP Address dengan port 5000
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-06-13.png?raw=true)
8. Membuat subdomain pada cloudflare yang di pointing pada server gateway. Jangan lupa matikan SSL cloudflare karena kita akan membuat SSL sendiri dengan menggunakan Let's Encrypt
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2021-53-23.png?raw=true)
9. Tambahkan reverse proxy agar web dapat diakses menggunakan domain yang telah dibuat 
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-07-24.png?raw=true)
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-08-00.png?raw=true)
### Deploy aplikasi Wayshub-Frontend
1. Clone wayshub frontend pada server appserver sesuai dengan resource yang disediakan yaitu :
	`https://github.com/dumbwaysdev/wayshub-frontend.git` 
2. Konfirugasi file yang menghubungkan dengan aplikasi backend yaitu pada file `wayshub-frontend/src/config/ap.js` . Masukkan api baseURL dengan domain dari aplikasi backend
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-11-37.png?raw=true)
3. Konfigurasi reverse proxy pada server gateway dengan domain yang telah dibuat
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-13-23.png?raw=true)
4. Buat file ecosystem simple lalu jalankan aplikasi
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-24-39.png?raw=true)
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-25-27.png?raw=true)

### Konfigurasi web SSL pada server gateway
1. Install certbot menggunakan perintah 
	`sudo snap install --classic	certbot`
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-13-56.png?raw=true)
2. Buat file bernama `.secret` lalu masukkan cloudflare email dan api key nya sesuai dengan resource
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-16-42.png?raw=true)
3. Setting command certbot agar dapat di jalankan dengan perintah 
	`sudo ln -s /snap/bin/certbot /usr/bin/certbot`
4. Jalankan perintah untuk create certificates `sudo certbot --nginx` lalu inputkan domain yang akan diberi certificates
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-18-30.png?raw=true)
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-19-01.png?raw=true)
	
5. Pembuktian bahwa certificate telah terpasang otomatis pada konfigurasi aplikasi di nginx
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-19-16.png?raw=true)
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-30-03.png?raw=true)
### Test akses pada web browser
1. Aplikasi telah ter-secure menggunakan SSL
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-30-06.png?raw=true)
2. Mencoba membuat akun baru (sign-up)
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-31-02.png?raw=true)
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-31-08.png?raw=true)
3. Login menggunakan akun baru
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-31-25.png?raw=true)
4. Test apakah data berhasil masuk pada database
	![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-1/assets/Screenshot%20from%202023-12-20%2022-32-34.png?raw=true)
