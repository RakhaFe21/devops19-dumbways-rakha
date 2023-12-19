# Day 3_Devops19-dumbways

Nama : Rakha Febryza Rasendriya


# Tugas

1.  Jelaskan apa itu VCS GIT menurut kalian (gambarkan menurut kalian flow dari git ini seperti apa)
2.  Dokumentasi beberapa  command dari git (ex git reset, git rebase)

## 1. Jelaskan apa itu VCS GIT menurut kalian (gambarkan menurut kalian flow dari git ini seperti apa)

VCS git adalah sistem yang berfungsi untuk mencatat perubahan yang terjadi pada suatu proyek. Proyek ini dapat berupa kumpulan kode, teks, aset gambar, folder, atau file lainnya. Git akan mencatat setiap perubahan yang dilakukan, termasuk nama pengembang, waktu, dan deskripsi perubahan. Dengan demikian, kita dapat melihat sejarah pengembangan proyek dan mengelola kode dengan lebih baik.

Adapun flow untuk membuat projek git dari awal sebagai berikut:
1.  Langkah pertama adalah membuat folder yang akan dijadikan repo git. Folder ini akan menjadi tempat kita menyimpan semua file dan kode untuk proyek kita. Untuk di github kita juga membuat repository, untuk nama repo lebih baik disamakan dengan yang ada di local
2. Selanjutnya yaitu inisialisasi git dengan cara menjalankan masuk ke folder yang akan dijadikan repo git lalu jalankan perintah `git init`
3. Masukkan pengenal dengan menjalankan perintah
`git config --global user.name "rakha"`
`git config --global user.email rakha@example.com`
4. Lalu tambahkan folder maupun file proyek kita kedalam repo
5. Jika ingin menaikkan kode dari modified ke staging, maka jalankan perintah `git add <nama file>` atau juga bisa sekaligus `git add .`
6. Jika ingin commit, maka jalankan perintah 
`git commit -m "<pesan>"` 
Perintah ini akan menyimpan perubahan kita ke dalam repository dan siap untuk kita push ke server github
7. Hubungkan repository ke server remote (github)
Tujuan remote ini yaitu ketika kita ingin berkolaborasi dengan orang lain untuk proyek yang kita punya. Untuk menambahkan remote menggunakan perintah `git remote add origin <url_server>` 
8. Push perubahan ke server remote
	Berguna untuk meng-up proyek kita ke server remote
	`git push origin <branch>`

## 2. Dokumentasi beberapa  command dari git (ex git reset, git rebase)
Berikut ini merupakan beberapa dokumentasi command dari git:

**1.`git init`**
Berfungsi untuk menginisialisasi folder menjadi repository git
![](https://user-images.githubusercontent.com/135587083/285290054-4b8f7547-b9f6-4be8-8240-655b99595165.png)

**2. `git add`** 
Berfungsi untuk menambahkan/up kode ke staging
![](https://user-images.githubusercontent.com/135587083/285290051-267b4fd4-7b05-44ef-a079-558eb68f13c9.png)

**3.`git commit`**
Berfungsi untuk menyimpan perubahaan di repo
![](https://user-images.githubusercontent.com/135587083/285290048-9e8177ee-d6c3-4088-8313-6c82e4356162.png)

**4.`git remote`**
Berfungsi untuk remote server github yang akan digunakan untuk kolaborasi dsb
![](https://user-images.githubusercontent.com/135587083/285290045-c9c3472f-9b5d-450d-8e2b-d8eda03e59e0.png)

**5.`git push`**
Berfungsi untuk upload proyek ke remote server
![](https://user-images.githubusercontent.com/135587083/285290042-c8ab1937-6f7c-4409-a27a-6f3cfd004fa5.png)

**6.`git pull`**
Berfungsi untuk mengambil kode dari remote server
![](https://user-images.githubusercontent.com/135587083/285290040-404546fa-081c-4393-bede-0533df52ee50.png)

**7.`git clone`**
Berfungsi untuk cloning proyek repository dari remote server
![](https://user-images.githubusercontent.com/135587083/285290039-a14644b5-9b1b-49d5-b5a1-d4d5b4c7b559.png)

**8.`git status`**
Berfungsi untuk melihat status kode
![](https://user-images.githubusercontent.com/135587083/285290037-d0748bc0-10b2-4d79-96ec-a6c970880f69.png)

**9.`git checkout`**
berfungsi untuk pindah branch ataupun untuk membuat branch baru
![](https://user-images.githubusercontent.com/135587083/285290033-635856a6-1ef9-4c0c-ad03-6552f14e27bd.png)

**10.`git branch`**
berfungsi untuk melihat branch saat ini
![](https://user-images.githubusercontent.com/135587083/285290029-d6105a6b-fde3-4cdb-a93d-5e8224eac916.png)

**11.`git merge`**
Berfungsi untuk menyatukan satu branch ke branch tujuan
![](https://user-images.githubusercontent.com/135587083/285290023-84510c4a-ddc3-4baf-b354-c7d0d4411650.png)

**12.`git reset`**
Berfungsi untuk reset ke commit sebelumnya, pada git reset ini terdapat 3 opsi yaitu :
- --soft akan mengebalikan dengan kondisi file dalam keadaan staged
- --mixed akan mengebalikan dengan kondisi file dalam keadaan modified
- --hard akan mengebalikan dengan kondisi file dalam keadaan commited
![](https://user-images.githubusercontent.com/135587083/285293057-ba28b454-b3aa-46c6-a9e1-8edffebadc82.png)

**13.`git revert`**
Berfungsi untuk mengembalikan file dengan tidak menghapus sejarah commit.
![](https://user-images.githubusercontent.com/135587083/285293051-64d9997e-c6d3-4955-9d65-2122423a36da.png)

**14.`git rebase`**
Berfungsi untuk satu branch disamakan tingkat commit nya dari branch lain atau mengambil commit dari branch lain.
![](https://user-images.githubusercontent.com/135587083/285293050-66e5d514-dc5b-41dc-a67a-83d5f9411f0b.png)

**15.`git stash`**
Berfungsi menyimpan semua progress yang sudah dilakukan sejak commit terakhir tanpa membuat sebuah commit untuk state saat ini
![](https://user-images.githubusercontent.com/135587083/285293048-31fd8676-d42d-40d8-b21e-d76667b9dad3.png)

**16.`git log`**
Berfungsi melihat log commit serta author dan juga kapan commitnya
![](https://user-images.githubusercontent.com/135587083/285293039-2ecd0335-7660-4ea7-b12f-c20210a55b00.png)

