# DAY 3

Tasks : [ Docker ]

-   Jelasakan langkah-langkah melakukan rebuild server BiznetGio, dan ubah menggunakan os ubuntu22
-   Setelah server sudah selesai ter rebuild, buatlah suatu user baru dengan nama  **team kalian**  lalu implementasikan penggunaan ssh dengan menggunakan  **PASSWORD ONLY**  tanpa SSH-KEY.
-   Buatlah bash script se freestyle mungkin untuk melakukan installasi docker.
-   Deploy aplikasi Web Server, Frontend, Backend, serta Database on top  `docker compose`
    -   Buat suatu docker compose yang berisi beberapa service kalian
        -   Web Server
        -   Frontend
        -   Backend
        -   Database
    -   Di dalam docker-compose file buat suatu custom network dengan nama  **team kalian**, lalu pasang ke setiap service yang kalian miliki.
    -   Deploy database terlebih dahulu menggunakan mysql dengan versi terbaru, dan jangan lupa untuk pasang volume di bagian database.
    -   Untuk building image frontend dan backend sebisa mungkin buat dockerized dengan image sekecil mungkin. dan jangan lupa untuk sesuaikan configuration dari backend ke database maupun frontend ke backend sebelum di build menjadi docker images.
    -   Untuk Web Server buatlah configurasi reverse-proxy menggunakan nginx on top docker.
        -   **SSL CLOUDFLARE OFF!!!**
        -   Gunakan docker volume untuk membuat reverse proxy
        -   SSL sebisa mungkin gunakan wildcard
        -   Untuk DNS bisa sesuaikan seperti contoh di bawah ini
            -   Frontend team1.studentdumbways.my.id
            -   Backend api.team1.studentdumbways.my.id
    -   Push image ke docker registry kalian masing".
-   Aplikasi dapat berjalan dengan sesuai seperti melakukan login/register.


# Step by step

## Rebuild server di BiznetGio
1.   Login ke akun yang telah dibuat pada halaman  [https://portal.biznetgio.com/user/login](https://portal.biznetgio.com/user/login)
2. Masuk ke Overview dari VM yang akan di rebuild -> Stop VM -> Klik tombol Rebuild  -> Pilih OS Ubuntu 22.04 -> lalu Rebuild.
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202023-12-28%2017-34-17.png?raw=true)

## Membuat user team dengan SSH Password Only

1. Buat user dengan nama team menggunakan perintah `sudo adduser team3`
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202023-12-28%2021-14-09.png?raw=true)
2. Konfigurasi sshd_config dengan perintah `sudo nano /etc/ssh/sshd_config` seperti berikut
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202023-12-28%2017-41-11.png?raw=true)


## Membuat bash script untuk installasi docker
1. Berkut merupakan bash script yang saya buat untuk installasi docker
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/carbon%20%2820%29.png?raw=true)
3. Tambahkan permission eksekutor pada file dengan perintah `chmod +x docker_install.sh`
4. Jalankan file dengan perintah `./docker_install.sh`

## Deploy aplikasi on top docker compose
1. Berikut merupakan konfigurasi yang saya buat untuk file docker-compose.yml
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/carbon%20%2821%29.png?raw=true)

## Konfigurasi reverse proxy pada web server
1. Membuat domain team3.studentdumbways.my.id dengan ssl cloudlfare off
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-06%2011-27-31.png?raw=true)
2. Isi konfigurasi reverse proxy
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-06%2011-27-54.png?raw=true)
3. Pembuatan SSL wildcard
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-01%2012-34-19.png?raw=true)


## Push image ke docker hub

1. Langkah pertama adalah login ke docker terlebih dahulu dengan perintah `docker login` lalu masukkan username serta password dari akun docker
2. Selanjutnya push image dengan perintah docker push < nama image>:tag (nama image harus sama dengan username, ex: rakhafe/frontend:latest)
3. Image akan otomatis ter-push pada docker registry
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-06%2011-34-34.png?raw=true)


# Akses aplikasi di web browser
1. Akses frontend
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-06%2011-37-18.png?raw=true)
2. Sertifikat SSL frontend
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-06%2011-37-36.png?raw=true)
3. Akses backend
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-06%2011-38-09.png?raw=true)
4. Sertifikat SSL backend
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-06%2011-38-19.png?raw=true)
