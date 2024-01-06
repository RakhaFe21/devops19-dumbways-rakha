# DAY 4


Tasks : [ Jenkins ]

-   Jelasakan langkah-langkah melakukan rebuild server BiznetGio, dan ubah menggunakan os ubuntu22
-   Installasi Jenkins on top Docker or native
-   Setup SSH-KEY di local server jenkins kalian, agar dapat login ke dalam server menggunakan SSH-KEY
-   Reverse Proxy Jenkins
    -   gunakan domain ex. pipeline-team4.studentdumbways.my.id
    -   reverse proxy sesuaikan dengan ketentuan yang ada di dalam Jenkins documentation
-   Buatlah beberapa Job untuk aplikasi kalian
    -   Job Frontend
    -   Job Backend
    -   Untuk script CICD atur flow pengupdate an aplikasi se freestyle kalian dan harus mencangkup
        -   Pull dari repository
        -   Dockerize/Build aplikasi kita
        -   Test application
        -   Deploy aplikasi on top Docker
        -   Push ke Docker Hub
-   Auto trigger setiap ada perubahan di SCM
-   Buat job notification ke discord

# Step by step

## Rebuild server di BiznetGio
1.   Login ke akun yang telah dibuat pada halaman  [https://portal.biznetgio.com/user/login](https://portal.biznetgio.com/user/login)
2. Masuk ke Overview dari VM yang akan di rebuild -> Stop VM -> Klik tombol Rebuild  -> Pilih OS Ubuntu 22.04 -> lalu Rebuild.
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202023-12-28%2017-34-17.png?raw=true)

## Installasi jenkins on top docker compose
1. Langkah pertama yaitu melakukan konfigurasi docker-compose.yml seperti berikut
2. Masukkan initial admin password untuk memulai installasi jenkins
3. Install plugin suggested
4. membuat user dengan nama team3
5. Jika sudah masuk, installasi lagi plugin yang dibutuhkan



## Setup SSH pada server
1. Konfigurasi ssh config agar dapat diakses oleh ssh key saja
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-07%2006-25-32.png?raw=true)

## Konfigurasi Reverse Proxy
1. Konfigurasi dari reverse proxy jenkins
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-07%2006-27-55.png?raw=true)

## Konfigurasi Jenkinsfile
1. Konfigurasi Jenkinsfile pada backend
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/carbon%20%2825%29.png?raw=true)
2. Konfigurasi Jenkinsfile pada frontend
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/carbon%20%2830%29.png?raw=true)

## Penambahan credential pada jenkins
1. Tambahkan sebuah credential dengan menggunakan username dan private key dari server appserver
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-07%2006-20-11.png?raw=true)
## Konfigurasi Job Jenkins
1. Buatlah sebuah folder bernama literature-apps yang nantinya akan diisi dengan job dari backend dan frontend
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-07%2006-37-19.png?raw=true)
3.  buat sebuah pipeline job dengan menggunakan pipeline from scm dan arahkan ke url dari repository kita, arahkan ke credential yang sudah dibuat. Jangan lupa untuk check list `GitHub hook trigger for GITScm polling`
## Dokumentasi Jenkins
1. Pipeline dari backend
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-07%2006-39-07.png?raw=true)
2. Pipeline dari frontend
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-07%2006-40-15.png?raw=true)
3. Hasil docker compose
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-07%2006-43-05.png?raw=true)
4. Hasil docker images
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-07%2006-44-14.png?raw=true)
6. Hasil docker hub
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-07%2006-47-33.png?raw=true)

## Auto trigger job
1. Masuk ke repository di github -> settings -> webhhoook -> lalu tambahkan url dari jenkins `https://pipeline.team3.studentdumbways.my.id/webhook-github/`
