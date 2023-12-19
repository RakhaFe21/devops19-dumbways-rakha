# Day 2_Devops19-dumbways

Nama : Rakha Febryza Rasendriya


# Tugas

1.  Apa itu Basic Network, jelaskan juga apa itu ip public, ip private, ip static, ip dinamis
2.  Apa itu linux Shell? (docs command line)

## 1. Apa itu Basic Network, jelaskan juga apa itu ip public, ip private, ip static, ip dinamis

- **Basic Network**

  Basic network adalah kumpulan perangkat komputer dan peralatan jaringan yang saling terhubung satu sama lain. Tujuan dari basic network adalah untuk memungkinkan perangkat-perangkat tersebut untuk berkomunikasi dan berbagi sumber daya, seperti data, printer, dan internet.

- **IP Address**

	IP address adalah alamat unik yang diberikan kepada setiap perangkat komputer yang terhubung ke jaringan. IP address terdiri dari empat angka biner yang dipisahkan oleh titik, seperti 192.168.1.1. Angka-angka ini mewakili lokasi perangkat tersebut di jaringan.

- **IP Public**

	IP public adalah alamat IP yang dapat diakses dari internet. IP public biasanya digunakan oleh perangkat-perangkat yang ingin terhubung ke internet, seperti router, server, dan komputer.

- **IP Private**

	IP private adalah alamat IP yang tidak dapat diakses dari internet. IP private biasanya digunakan oleh perangkat-perangkat yang terhubung ke jaringan lokal, seperti komputer, printer, dan perangkat IoT.

- **IP Static**

	IP static adalah alamat IP yang tidak berubah. IP statis biasanya digunakan untuk perangkat-perangkat yang membutuhkan akses ke internet secara tetap, seperti server dan perangkat IoT.

- **IP Dinamis**

	IP dinamis adalah alamat IP yang berubah-ubah. IP dinamis biasanya digunakan untuk perangkat-perangkat yang tidak membutuhkan akses ke internet secara tetap, seperti komputer dan perangkat mobile.

- **Analogi IP Public dan IP Private**

	IP public dapat dianalogikan dengan alamat rumah. Alamat rumah dapat dilihat oleh siapa saja, termasuk orang-orang yang tidak dikenal. Begitupula IP public juga dapat dilihat oleh siapa saja, termasuk orang-orang yang tidak dikenal di internet.

	IP private dapat dianalogikan dengan nomor telepon rumah. Nomor telepon rumah hanya dapat dilihat oleh orang-orang yang dikenal. Begitupula IP private juga hanya dapat dilihat oleh orang-orang yang terhubung ke jaringan lokal.

- **Analogi IP Statis dan IP Dinamis**

	IP static dapat dianalogikan dengan nomor telepon rumah yang tetap Nomor telepon rumah tidak akan berubah, kecuali jika kita menggantinya dengan nomor telepon yang baru. IP statis juga tidak akan berubah, kecuali kita mengubahnya secara manual.

	IP dinamis dapat dianalogikan dengan nomor telepon rumah yang berganti-ganti. Nomor telepon rumah dapat berubah setiap bulan, tergantung pada kontrak yang kita miliki dengan penyedia layanan telepon. IP dinamis juga dapat berubah setiap saat, tergantung pada kebijakan penyedia layanan internet (ISP).

## 2. Apa itu linux Shell? (docs command line)
Linux shell adalah program yang menyediakan antarmuka antara pengguna dan kernel Linux. Shell menerima perintah dari pengguna, menerjemahkannya menjadi bahasa yang dapat dimengerti oleh kernel, dan kemudian menjalankan perintah tersebut.

Berikut merupakan dokumentasi command line linux shell yang paling sering digunakan: 

**1. sudo**
Perintah `sudo` digunakan untuk menjalankan perintah sebagai pengguna root atau super user.
![sudo](https://user-images.githubusercontent.com/135587083/285270988-ac4c01c9-18c4-4332-bfb5-1e6767f138e8.png)

**2. mkdir**

Perintah `mkdir` digunakan untuk membuat direktori baru.
![mkdir](https://user-images.githubusercontent.com/135587083/285270983-28d6eada-948e-4b89-b748-19e0593b7452.png)

**3. rmdir**

Perintah `rmdir` digunakan untuk menghapus direktori kosong. Jika direktori yang akan dihapus ada isinya, maka bisa menggunakan perintah `rm -rf`
![rmdir](https://user-images.githubusercontent.com/135587083/285270979-6b25146b-0f85-46ef-9af8-4dfdd0484414.png)

**4. cd**

Perintah `cd` digunakan untuk beralih ke direktori yang diinginkan.
![cd](https://user-images.githubusercontent.com/135587083/285270975-a26897e9-9f22-40f6-8fd7-f825a6ccd240.png)

**5. touch**

Perintah `touch` digunakan untuk membuat file kosong.
![touch](https://user-images.githubusercontent.com/135587083/285270971-41778927-5abb-4889-9834-1a73437fb907.png)

**6. nano**
Perintah `nano` adalah perintah untuk mengedit file teks di Linux.
![nano](https://user-images.githubusercontent.com/135587083/285270962-7af24352-3d1a-4ad5-87f8-1a965967f8c1.png)

**7. ls**

Perintah `ls` digunakan untuk menampilkan daftar file dan direktori di direktori saat ini. Untuk menampilkan secara lengkap kita bisa melakukan `ls -la`
![ls](https://user-images.githubusercontent.com/135587083/285270965-704936ae-633f-4cd1-913d-434098a66c80.png)

**8. cat**

Perintah `cat` digunakan untuk menampilkan isi file.
![cat](https://user-images.githubusercontent.com/135587083/285270959-0fc50af8-bd20-42f2-9ca2-7308e44a77f6.png)

**9. cp**

Perintah `cp` digunakan untuk menyalin/copy file atau direktori.
![cp](https://user-images.githubusercontent.com/135587083/285270956-87bfc5a9-8bfd-4312-bf2d-e6217ecd875e.png)

**10. mv**

Perintah `mv` digunakan untuk memindahkan atau juga bisa untuk mengubah nama file atau direktori.
![mv](https://user-images.githubusercontent.com/135587083/285270952-c25e2020-87ce-4f97-9039-4fc7305244d0.png)

**11. rm**

Perintah `rm` digunakan untuk menghapus file atau direktori.
![rm](https://user-images.githubusercontent.com/135587083/285270947-948d1ebb-7d96-4389-a591-30b4c60810cf.png)

**12. chown**

Perintah `chown` digunakan untuk mengubah kepemilikan file atau direktori.
![chown](https://user-images.githubusercontent.com/135587083/285270942-3bc0691a-27ce-45e1-8e8a-4a0694e7edd0.png)

**13. chmod**

Perintah `chmod` digunakan untuk mengubah izin file atau direktori.
![chmod](https://user-images.githubusercontent.com/135587083/285270934-016fb0fe-6dd2-4569-a4f0-9bb2c2e144b5.png)

**14. grep**

Perintah `grep` digunakan untuk mencari teks dalam file.
![grep](https://user-images.githubusercontent.com/135587083/285270931-2fafe2fd-c785-419b-83d5-ba884b72bc13.png)

**15. wget**

Perintah `wget` digunakan untuk mengunduh file dari internet.
![wget](https://user-images.githubusercontent.com/135587083/285270924-79a48f47-f0d0-4cab-b9f3-2744ab45adb1.png)

**16. history**

Perintah `history` digunakan untuk menampilkan daftar perintah yang telah dijalankan sebelumnya.
![history](https://user-images.githubusercontent.com/135587083/285270922-cebba3fe-227c-4bac-9531-2cfdc5ce751b.png)
**17. zip**

Perintah `zip` digunakan untuk mengarsipkan file atau direktori.
![zip](https://user-images.githubusercontent.com/135587083/285270917-4f4d80ab-d152-47a6-8c3e-72f6b7915b6b.png)

**18. unzip**

Perintah `unzip` digunakan untuk mengekstrak file atau direktori yang telah diarsipkan.
![unzip](https://user-images.githubusercontent.com/135587083/285270914-481ecfd4-ed73-4ac5-a595-311be7495a89.png)

**19. tar**

Perintah `tar` digunakan untuk mengarsipkan file atau direktori yang akan menyimpan dalam bentuk file .tar
![tar](https://user-images.githubusercontent.com/135587083/285270910-22a8adfb-3f5b-4052-997e-3cf8cb09fbac.png)

**20. ping**

Perintah `ping` digunakan untuk memeriksa koneksi jaringan atau dapat juga digunakan untuk menghubungi atau mencapai tujuan yang ditentukan.
![ping](https://user-images.githubusercontent.com/135587083/285270901-1cc3281c-bc50-4a08-a4ee-46a75609235f.png)
