# Jarkom-Modul-2-A13-2021
**Modul 2: **

|Nama|NRP|
|----|-----|
|Afifah Nur Sabrina Syamsudin|05111940000022|
|James Rafferty Lee|05111940000055|
|Zulfiqar Fauzul Akbar|05111940000101|

### Soal No. 1
EniesLobby akan dijadikan sebagai DNS Master, Water7 akan dijadikan DNS Slave, dan Skypie akan digunakan sebagai Web Server. Terdapat 2 Client yaitu Loguetown, dan Alabasta. Semua node terhubung pada router Foosha, sehingga dapat mengakses internet.

**Penyelesaian**



### Soal No. 2
Luffy ingin menghubungi Franky yang berada di EniesLobby dengan denden mushi. Kalian diminta Luffy untuk membuat website utama dengan mengakses franky.yyy.com dengan alias www.franky.yyy.com pada folder kaizoku.

**Penyelesaian**



### Soal No. 3
Setelah itu buat subdomain super.franky.yyy.com dengan alias www.super.franky.yyy.com yang diatur DNS nya di EniesLobby dan mengarah ke Skypie.

**Penyelesaian**



### Soal No. 4
Buat juga reverse domain untuk domain utama.

**Penyelesaian**


### Soal No. 5
Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak, maka buat Water7 sebagai DNS Slave untuk domain utama.

**Penyelesaian**



### Soal No. 6
Setelah itu terdapat subdomain mecha.franky.yyy.com dengan alias www.mecha.franky.yyy.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo.

**Penyelesaian**



### Soal No. 7
Untuk memperlancar komunikasi Luffy dan rekannya, dibuatkan subdomain melalui Water7 dengan nama general.mecha.franky.yyy.com dengan alias www.general.mecha.franky.yyy.com yang mengarah ke Skypie.

**Penyelesaian**



### Soal No. 8
Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.franky.yyy.com. Pertama, luffy membutuhkan webserver dengan DocumentRoot pada /var/www/franky.yyy.com

**Penyelesaian**



### Soal No. 9
Setelah itu, Luffy juga membutuhkan agar url www.franky.yyy.com/index.php/home dapat menjadi menjadi www.franky.yyy.com/home

**Penyelesaian**



### Soal No. 10
Setelah itu, pada subdomain www.super.franky.yyy.com, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/super.franky.yyy.com

**Penyelesaian**



### Soal No. 11
Akan tetapi, pada folder /public, Luffy ingin hanya dapat melakukan directory listing saja.

**Penyelesaian**



### Soal No. 12
Tidak hanya itu, Luffy juga menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache.

**Penyelesaian**



### Soal No. 13
Luffy juga meminta Nami untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.super.franky.yyy.com/public/js menjadi www.super.franky.yyy.com/js

**Penyelesaian**




### Soal No. 14
Dan Luffy meminta untuk web www.general.mecha.franky.yyy.com hanya bisa diakses dengan port 15000 dan port 15500.

**Penyelesaian**




### Soal No. 15
dengan autentikasi username luffy dan password onepiece dan file di /var/www/general.mecha.franky.yyy

**Penyelesaian**




### Soal No. 16
Dan setiap kali mengakses IP Skypie akan dialihkan secara otomatis ke www.franky.yyy.com

**Penyelesaian**


### Soal No. 17
Dikarenakan Franky juga ingin mengajak temannya untuk dapat menghubunginya melalui website www.super.franky.yyy.com, dan dikarenakan pengunjung web server pasti akan bingung dengan randomnya images yang ada, maka Franky juga meminta untuk mengganti request gambar yang memiliki substring “franky” akan diarahkan menuju franky.png. Maka bantulah Luffy untuk membuat konfigurasi dns dan web server ini!

**Penyelesaian**


### Kendala yang Dialami
- Tidak bisa menginstall DNS3 (Sabrina)

