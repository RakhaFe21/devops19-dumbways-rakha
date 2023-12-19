# Day 1_Devops19-dumbways

Nama : Rakha Febryza Rasendriya


# Tugas

1. Fundamental devops
2. Step by step pembuatan ubuntu server (penjelasan pada saat setup network/storage dsb)

## 1. Fundamental Devops

DevOps adalah sebuah filosofi dan praktik untuk menyatukan pengembangan perangkat lunak (Dev) dan operasi TI (Ops) untuk meningkatkan kecepatan, kualitas, dan keamanan pengiriman perangkat lunak.

DevOps didasarkan pada beberapa prinsip utama, yaitu:

-   **Kolaborasi:**  DevOps mendorong kolaborasi yang erat antara tim Dev dan Ops. Kedua tim harus bekerja sama untuk memahami kebutuhan satu sama lain dan untuk mengembangkan proses yang efisien untuk pengiriman perangkat lunak.
-   **Automasi:**  DevOps memanfaatkan otomatisasi untuk mengurangi tugas manual dan meningkatkan kecepatan dan keandalan pengiriman perangkat lunak.

Adapun manfaat adanya DevOps bagi organisasi, yaitu:

-   Peningkatan kecepatan pengembangan perangkat lunak
-   Peningkatan kualitas perangkat lunak 

Devops memiliki cara kerja atau yang biasa disebut dengan lifecycle. Adapun penjelasan dari lifecycle sebagai berikut:

![Devops lifecycle](https://user-images.githubusercontent.com/135587083/285249830-5afb4c96-61dc-4fc8-a04d-95bf180578dd.jpeg)

**1. Plan**

-   **Pengertian:** Fase plan adalah fase awal dari lifecycle DevOps, di mana tim DevOps akan menentukan tujuan dan persyaratan untuk pengembangan perangkat lunak. Tim juga akan mengembangkan rencana untuk mencapai tujuan tersebut.
    
-   **Tujuan:** Tujuan dari fase plan adalah untuk memastikan bahwa pengembangan perangkat lunak dilakukan secara efisien dan efektif.
-   **Output:** Keluaran dari fase perencanaan adalah rencana pengembangan perangkat lunak yang mencakup tujuan, persyaratan, dan jadwal.

**2. Code**

-   **Pengertian:** Fase code adalah fase di mana pengembang akan menulis kode untuk perangkat lunak. Kode akan ditulis dalam lingkungan yang terisolasi atau biasanya di local developer untuk mencegah kesalahan.
    
-   **Tujuan:** Tujuan dari fase code adalah untuk mengembangkan kode yang berkualitas dan berfungsi dengan benar.
-   **Output:** Output dari fase kode adalah kode yang telah diuji dan siap untuk diintegrasikan.

**3. Build**

-   **Pengertian:** Fase build adalah fase di mana kode akan dikompilasi dan dikemas menjadi satu unit.
    
-   **Tujuan:** Tujuan dari fase build adalah untuk memastikan bahwa kode dapat berjalan dengan benar.
-   **Output:** Output dari fase build adalah kode yang telah dikompilasi dan dikemas.

**4. Test**

-   **Pengertian:** Fase test adalah fase di mana kode, system, dsb akan diuji untuk memastikan bahwa project tersebut berfungsi dengan baik.
    
-   **Tujuan:** Tujuan dari fase test adalah untuk memastikan bahwa project bebas dari kesalahan dan berfungsi sesuai dengan persyaratan.
-   **Output:** Output dari fase pengujian adalah laporan testing yang menunjukkan hasil pengujian. Biasanya pada fase ini dilakukan oleh QA Engineer (Quality Assurance)

**5. Release**

-   **Pengertian:** Fase release adalah fase di mana perangkat lunak yang telah dikembangkan dan diuji akan diluncurkan ke lingkungan production.
    
-   **Tujuan:** Tujuan dari fase release adalah untuk memastikan bahwa perangkat lunak dapat diluncurkan ke production.
-   **Output:** Output dari fase release adalah perangkat lunak yang telah diluncurkan ke lingkungan produksi.

**6. Deploy**

-   **Pengertian:** Fase deploy adalah fase di mana perangkat lunak yang telah dirilis akan dideploy ke lingkungan produksi.
    
-   **Tujuan:** Tujuan dari fase deploy adalah agar perangkat lunak dapat dengan cepat dan aman diakses oleh pengguna.
-   **Output:** Output dari fase deploy adalah perangkat lunak yang telah dideploy ke lingkungan produksi dan dapat diakses oleh pengguna.

**7. Operate**

-   **Pengertian:** Fase operate adalah fase di mana perangkat lunak yang telah dideploy akan dikelola dan dipantau.
    
-   **Tujuan:** Tujuan dari fase operate adalah untuk memastikan bahwa perangkat lunak berfungsi dengan andal dan aman.
-   **Output:** Output dari fase operate adalah perangkat lunak yang berfungsi dengan andal dan aman.

**8. Monitor**

-   **Pengertian:** Fase monitor adalah fase di mana perangkat lunak yang telah dideploy akan dipantau untuk memastikan bahwa perangkat lunak berfungsi dengan baik.

-   **Tujuan:** Tujuan dari fase monitor adalah untuk mengidentifikasi masalah apa pun dengan perangkat lunak dan untuk mengambil tindakan yang diperlukan untuk mengatasi masalah tersebut.
-   **Output:** Output dari fase monitor adalah laporan pemantauan yang menunjukkan hasil pemantauan.

## 2. Step by step pembuatan ubuntu server

Berikut merupakan settingan konfigurasi yang saya lakukan pada penginstallan ubuntu server di Virtualbox: 
1. Setup Virtual machine
   ![vbox](https://user-images.githubusercontent.com/135587083/285251723-01b7845e-3e8c-437c-ac8f-00286555fd3f.png)
2. Setting network
   ![local-ip](https://user-images.githubusercontent.com/135587083/285252427-dc960b51-847c-47ae-b8cf-d2ba0b528432.png)
   ![set](https://user-images.githubusercontent.com/135587083/285240956-241aa1eb-f18b-4a60-b7e0-185f92d953df.png)
   ![set](https://user-images.githubusercontent.com/135587083/285240953-0e746d0c-ce65-4339-9a11-0341e09c1149.png)
3. Setting storage

   ![storage](https://user-images.githubusercontent.com/135587083/285240936-a4d8614b-59a4-4ecf-bdea-bc9780efd133.png)
4. Profile setup configuration
   ![profile](https://user-images.githubusercontent.com/135587083/285240934-670b0ffa-2f69-46ef-a231-43a294991207.png)
5. Install SSH agar dapat diakses secara remote
   ![ssh](https://user-images.githubusercontent.com/135587083/285240931-5e948ab7-5da9-4f96-9387-b3d92e75b414.png)
6. Installasi

   ![install](https://user-images.githubusercontent.com/135587083/285240922-11b96d02-5ebb-4775-976c-8b9fd6dbb2fb.png)
7. Akses secara remote pada komputer local
   ![access](https://user-images.githubusercontent.com/135587083/285240914-69b9c5ee-de4a-4c7a-99b3-5bfe383e0582.png)
   kita dapat mengakses secara remote server kita dengan perintah`ssh user@ip address` dan masukkan password nya

