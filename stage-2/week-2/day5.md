
# Day 4

Tasks : [ Gitlab ]

-   Buat akun di gitlab.com
-   push SCM kalian dari local-server ke gitlab
-   Buatlah beberapa Job menggunakan gitlabci untuk aplikasi kalian
    -   Job Frontend
    -   Job Backend
    -   Untuk script CICD atur flow pengupdate an aplikasi se freestyle kalian dan harus mencangkup
        -   Pull dari repository
        -   Dockerize/Build aplikasi kita
        -   push image ke docker hub
        -   Test application
        -   pull new image
        -   Deploy application

# Step by step


## Menghubungkan repository lokal ke gitlab
1. Langkah pertama adalah menambahkan ssh public key dari server ke gitlab 
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-07%2012-13-05.png?raw=true)
2. Selanjutnya yaitu membuat repository baru digitlab dan hubungkan dengan repository di server dengan menggunakan perintah git remote add `nama remote` `url repository gitlab`
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-07%2012-00-38.png?raw=true)
## Membuat file .gitlab-ci.yml
1. Selanjutnya adalah membuat file yang nantinya berguna untuk melakukan tugas CI/CD itu sendiri.
- Frontend
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/carbon%20%2830%29.png?raw=true)
- Backend
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/carbon%20%2825%29.png?raw=true)


## Setting repository di gitlab
1. Setting pada bagian variable di ->settings -> CICD -> variable
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-07%2012-11-36.png?raw=true)
2. Tambahkan disocrd notified di -> settings -> Integrations -> Discord notifier
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-07%2012-12-03.png?raw=true)
Adapun pada kolom webhook kita tambahkan url webhook dari channel di server dicord kita

## Hasil Pipeline Gitlab
1. Contoh dari salah satu hasil pipeline dari project cicd gitlab
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-07%2012-16-10.png?raw=true)


## Notifikasi discord
1. Salah satu hasil dari notifikasi discord 
![enter image description here](https://github.com/RakhaFe21/devops19-dumbways-rakha/blob/main/stage-2/week-2/assets/Screenshot%20from%202024-01-07%2012-16-58.png?raw=true)
