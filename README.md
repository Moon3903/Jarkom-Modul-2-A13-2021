# Jarkom-Modul-2-A13-2021

|Nama|NRP|
|----|-----|
|Afifah Nur Sabrina Syamsudin|05111940000022|
|James Rafferty Lee|05111940000055|
|Zulfiqar Fauzul Akbar|05111940000101|

### Soal No. 1
EniesLobby akan dijadikan sebagai DNS Master, Water7 akan dijadikan DNS Slave, dan Skypie akan digunakan sebagai Web Server. Terdapat 2 Client yaitu Loguetown, dan Alabasta. Semua node terhubung pada router Foosha, sehingga dapat mengakses internet.

**Penyelesaian**
- Buat topologi seperti pada soal
![topologi_modul_2](https://user-images.githubusercontent.com/75364000/139527146-eebc3968-d970-41c0-bd22-611048b8bc8c.png)

- Setting network masing-masing node dengan fitur `Edit network configuration`:
**Foosha (sebagai Router)**
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.175.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.175.2.1
	netmask 255.255.255.0
```

**Loguetown (sebagai Client)**
```
auto eth0
iface eth0 inet static
	address 192.175.1.2
	netmask 255.255.255.0
	gateway 192.175.1.1
```

**Alabasta (sebagai Client)**
```
auto eth0
iface eth0 inet static
	address 192.175.1.3
	netmask 255.255.255.0
	gateway 192.175.1.1
```

**EniesLobby (sebagai DNS Master)**
```
auto eth0
iface eth0 inet static
	address 192.175.2.2
	netmask 255.255.255.0
	gateway 192.175.2.1
```

**Water7 (sebagai DNS Slave)**
```
auto eth0
iface eth0 inet static
	address 192.175.2.3
	netmask 255.255.255.0
	gateway 192.175.2.1
```

**Skypie (sebagai Web Server)**
```
auto eth0
iface eth0 inet static
	address 192.175.2.4
	netmask 255.255.255.0
	gateway 192.175.2.1
```

- Jalankan `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.175.0.0/16` pada router `Foosha`.
- Jalankan ```echo nameserver 192.168.122.1 > /etc/resolv.conf ``` pada masing - masing node.
- Restart semua node dan coba `ping google.com`
![Screenshot (913)](https://user-images.githubusercontent.com/62832487/139534176-6cc39d56-5728-4cba-98e2-dafdf39aa2ca.png)


### Soal No. 2
Luffy ingin menghubungi Franky yang berada di EniesLobby dengan denden mushi. Kalian diminta Luffy untuk membuat website utama dengan mengakses franky.yyy.com dengan alias www.franky.yyy.com pada folder kaizoku.

**Penyelesaian**
**EniesLobby**
- Install aplikasi bind9.
  ```
  apt-get update
  apt-get install bind9 -y
  ```
- Edit `/etc/bind/named.conf.local` seperti berikut:
  ![Screenshot (915)](https://user-images.githubusercontent.com/62832487/139534245-73707f60-b04c-41da-a921-d9efa14675f8.png)

- Create folder `kaizoku` pada `/etc/bind`.
  ```
  mkdir /etc/bind/kaizoku
  ```
- Copy file `db.local` pada path `/etc/bind` ke folder `kaizoku` dan rename menjadi `franky.A13.com`.
  ```
  cp /etc/bind/db.local /etc/bind/kaizoku/franky.A13.com
  ```
- Edit `/etc/bind/kaizoku/franky.A13.com` seperti berikut:
  ![Screenshot (917)](https://user-images.githubusercontent.com/62832487/139534278-50f722ff-0dc2-4638-abc4-ca86a7fdbc52.png)

- Restart bind9.
  ```
  service bind9 restart
  ```

**Loguetown**
- Edit file `/etc/resolv.conf` seperti berikut:
  ![Screenshot (920)](https://user-images.githubusercontent.com/62832487/139534400-94a55673-c1f4-4e9a-9b82-0c037eaec869.png)

- Selanjutnya, ping domain `franky.A13.com` dan `www.franky.A13.com`.
  ![Screenshot (922)](https://user-images.githubusercontent.com/62832487/139534443-06925cff-9ae8-4fa4-9186-47357a451adb.png)


### Soal No. 3
Setelah itu buat subdomain super.franky.yyy.com dengan alias www.super.franky.yyy.com yang diatur DNS nya di EniesLobby dan mengarah ke Skypie.

**Penyelesaian**
**EniesLobby**
- Edit file `/etc/bind/kaizoku/franky.A13.com` seperti berikut:
  ![Screenshot (917)](https://user-images.githubusercontent.com/62832487/139534278-50f722ff-0dc2-4638-abc4-ca86a7fdbc52.png)
- Restart bind9.
  ```
  service bind9 restart
  ```

**Loguetown**
- Selanjutnya, ping domain `super.franky.A13.com` dan `www.super.franky.A13.com`
  ![Screenshot (924)](https://user-images.githubusercontent.com/62832487/139534556-ee36f7c3-6f3d-4be7-b6f1-83b1a277d3a4.png)


### Soal No. 4
Buat juga reverse domain untuk domain utama.

**Penyelesaian**
**EniesLobby**
- Edit `/etc/bind/named.conf.local` seperti berikut:
  ![Screenshot 2021-10-30 202844](https://user-images.githubusercontent.com/62832487/139534598-fbbd1983-8099-4606-a08f-6e46b6a6216c.png)

- Copy file `db.local` pada path `/etc/bind` ke dalam folder `kaizoku` dan rename menjadi `2.181.192.in-addr.arpa`.
  ```
  cp /etc/bind/db.local /etc/bind/kaizoku/2.181.192.in-addr.arpa
  ```
- Edit file `/etc/bind/kaizoku/2.175.192.in-addr.arpa` seperti berikut:
  ![Screenshot 2021-10-30 203020](https://user-images.githubusercontent.com/62832487/139534660-23247efa-649f-4257-9b3b-01a64b711746.png)

- Restart bind9.
  ```
  service bind9 restart
  ```

**Pada Loguetown**
- Check konfigurasi dengan perintah `host -t PTR 192.175.2.4`.
  ![Screenshot 2021-10-30 203634](https://user-images.githubusercontent.com/62832487/139534935-e7764423-2e7f-493b-ba31-c05b1fd3d599.png)


### Soal No. 5
Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak, maka buat Water7 sebagai DNS Slave untuk domain utama.

**Penyelesaian**
**EniesLobby**
- Edit `/etc/bind/named.conf.local` seperti berikut:
  ![Screenshot 2021-10-30 203301](https://user-images.githubusercontent.com/62832487/139534722-8dfa123b-36a0-4b67-804d-bbeb9b44fd41.png)

- Restart bind9.
  ```
  service bind9 restart
  ```

**Water7**
- Install aplikasi bind9.
  ```
  apt-get install bind9 -y
  ```
- Edit `/etc/bind/named.conf.local` seperti berikut:
  ![Screenshot 2021-10-30 203413](https://user-images.githubusercontent.com/62832487/139534785-1a0235a0-8f05-4859-8086-fda66812d4ed.png)

- Restart bind9.
  ```
  service bind9 restart
  ```

**Loguetown**
- Pada `Enieslobby` matikan service bind9.
  ```
  service bind9 stop
  ```
- Edit `/etc/resolv.conf` seperti pada gambar berikut:
  ![Screenshot 2021-10-30 203454](https://user-images.githubusercontent.com/62832487/139534822-cacd174d-411e-4b0b-85c9-f6ea3c6cf137.png)

- Selanjutnya ping domain `franky.A13.com`.
  ![Screenshot 2021-10-30 203538](https://user-images.githubusercontent.com/62832487/139534884-332b0772-bede-422b-805a-a98a39b115a1.png)


### Soal No. 6
Setelah itu terdapat subdomain mecha.franky.yyy.com dengan alias www.mecha.franky.yyy.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo.

**Penyelesaian**
**EniesLobby**
- Edit `/etc/bind/kaizoku/franky.A13.com` seperti berikut:
  ![Screenshot 2021-10-30 203801](https://user-images.githubusercontent.com/62832487/139534978-f0d49617-7914-4f6c-b5ac-ad561faeea42.png)

- Edit `/etc/bind/named.conf.options` seperti berikut:
  ![Screenshot 2021-10-30 203906](https://user-images.githubusercontent.com/62832487/139535009-33615cbb-275e-4126-84f8-d24b6a38cfac.png)

- Edit `/etc/bind/named.conf.local` seperti berikut:
  ![Screenshot 2021-10-30 203932](https://user-images.githubusercontent.com/62832487/139535024-ca79e8ce-ca3b-4913-98d5-ba474b219915.png)

- Restart bind9.
  ```
  service bind9 restart
  ```

**Pada Water7**
- Edit `/etc/bind/named.conf.options` seperti berikut:
  ![Screenshot 2021-10-30 204004](https://user-images.githubusercontent.com/62832487/139535043-7deecf8e-e8f6-45be-8122-2869c60bb49a.png)

- Edit `/etc/bind/named.conf.local` seperti pada gambar berikut:
  ![Screenshot 2021-10-30 204031](https://user-images.githubusercontent.com/62832487/139535059-8c38f347-42d4-49ac-9b4a-2fd6a7294cdd.png)

- Create folder `sunnygo` di dalam `/etc/bind`.
  ```
  mkdir /etc/bind/sunnygo
  ```
- Copy file `db.local` pada path `/etc/bind` ke dalam folder `sunnygo` dan rename `mecha.franky.A13.com`.
  ```
  cp /etc/bind/db.local /etc/bind/sunnygo/mecha.franky.A13.com
  ```
- Edit `/etc/bind/sunnygo/mecha.franky.A13.com` seperti pada gambar berikut:
  ![Screenshot 2021-10-30 204103](https://user-images.githubusercontent.com/62832487/139535079-78a2b324-bf0b-470a-b309-e765ce5a122d.png)

- Restart bind9.
  ```
  service bind9 restart
  ```

**Pada Loguetown**
- Selajutnya ping domain `mecha.franky.A13.com` dan `www.mecha.franky.A13.com`.

### Soal No. 7
Untuk memperlancar komunikasi Luffy dan rekannya, dibuatkan subdomain melalui Water7 dengan nama general.mecha.franky.yyy.com dengan alias www.general.mecha.franky.yyy.com yang mengarah ke Skypie.

**Penyelesaian**
**Water7**
- Edit `/etc/bind/sunnygo/mecha.franky.A13.com` seperti berikut:
  ![Screenshot 2021-10-30 204209](https://user-images.githubusercontent.com/62832487/139535137-66311f1a-e07f-4331-9d42-c25ccfbcada3.png)

- Restart bind9.
  ```
  service bind9 restart
  ```

**Pada Loguetown**
- Selanjutnya ping domain `general.mecha.franky.A13.com` dan `www.general.mecha.franky.A13.com`.
  ![Screenshot 2021-10-30 204257](https://user-images.githubusercontent.com/62832487/139535177-0085a229-3bf6-4bfc-abc2-7f0ccc06d440.png)

### Soal No. 8
Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.franky.yyy.com. Pertama, luffy membutuhkan webserver dengan DocumentRoot pada /var/www/franky.yyy.com

**Penyelesaian**
**Skypie**
- Install aplikasi apache, PHP, dan libapache2-mod-php7.0.
  ```
  apt-get install apache2 -y
  apt-get install php -y
  apt-get install libapache2-mod-php7.0 -y
  ```
- Pindah ke directory `/etc/apache2/sites-available`.
- Copy file `000-default.conf` menjadi file `franky.A13.com.conf`.
- Edit file `franky.A13.com.conf` seperti berikut:
  ![Screenshot 2021-10-30 204446](https://user-images.githubusercontent.com/62832487/139535261-934f8634-ad2b-4692-af9c-c72f85de9baf.png)

- Aktifkan konfigurasi franky.A13.com.
  ```
  a2ensite franky.A13.com
  ```
- Restart apache.
  ```
  service apache2 restart
  ```
- Pindah ke directory `/var/www`.
- Download file zip menggunakan `wget`.
  ```
  wget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/franky.zip
  ```
- Lakukan unzip.
  ```
  unzip franky.zip
  ```
- Rename folder `franky` menjadi `franky.A13.com` dan terdapat isi file seperti pada gambar berikut:
  ![Screenshot 2021-10-30 204656](https://user-images.githubusercontent.com/62832487/139535368-d14347f2-a8a6-45b6-b359-91c93f178956.png)

**Loguetown**
- Install aplikasi lynx.
  ```
  apt-get install lynx -y
  ```
- Buka `www.franky.A13.com` menggunakan lynx.
  ![Screenshot 2021-10-30 204920](https://user-images.githubusercontent.com/62832487/139535502-4c225e59-2fb2-4e77-853d-d15b96e37a68.png)


### Soal No. 9
Setelah itu, Luffy juga membutuhkan agar url www.franky.yyy.com/index.php/home dapat menjadi menjadi www.franky.yyy.com/home

**Penyelesaian**

**Pada Skypie**
- Jalankan perintah `a2enmod rewrite` untuk mengaktifkan module rewrite.
- Restart apache dengan perintah `service apache2 restart`.
- Tambahkan file baru `.htaccess` pada folder `/var/www/franky.A13.com`, di mana file tersebut akan dimodifikasi menjadi:

(- ss)
- Pindah ke directory `/etc/apache2/sites-available`.
- Edit file `franky.A13.com.conf` agar file `.htaccess` dapat berjalan seperti pada gambar berikut:

(- ss)
- Restart apache.

  ```
  service apache2 restart
  ```

**Pada Loguetown**
- Buka `www.franky.A13.com/home` menggunakan lynx.
(- ss)

### Soal No. 10
Setelah itu, pada subdomain www.super.franky.yyy.com, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/super.franky.yyy.com

**Penyelesaian**

**Pada Skypie**
- Pindah ke directory `/etc/apache2/sites-available`.
- Copy file `000-default.conf` menjadi file `super.franky.A13.com.conf`.
- Edit file `super.franky.A13.com.conf` seperti pada gambar berikut:

  (- ss)
- Aktifkan konfigurasi super.franky.A13.com.

  ```
  a2ensite super.franky.A13.com
  ```
- Restart apache.

  ```
  service apache2 restart
  ```
- Pindah ke directory `/var/www`.
- Download file zip menggunakan `wget`.

  ```
  wget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/super.franky.zip
  ```
- Lakukan unzip.

  ```
  unzip super.franky.zip
  ```
- Rename folder `super.franky` menjadi `super.franky.A13.com` dan terdapat isi file seperti pada gambar berikut:

  (- ss)

**Pada Loguetown**
- Buka `www.super.franky.A13.com` menggunakan lynx.

  (- ss)


### Soal No. 11
Akan tetapi, pada folder /public, Luffy ingin hanya dapat melakukan directory listing saja.

**Penyelesaian**

**Pada Skypie**
- Pindah ke directory `/etc/apache2/sites-available`.
- Edit file `super.franky.A13.com.conf` seperti pada gambar berikut:

  (- ss)
- Restart apache.

  ```
  service apache2 restart
  ```

**Pada Loguetown**
- Buka `www.super.franky.A13.com/public` menggunakan lynx.

  (- ss)
- Buka `www.super.franky.A13.com/public/css`, `www.super.franky.A13.com/public/images`, dan `www.super.franky.A13.com/public/js` menggunakan lynx.

  (- ss)

### Soal No. 12
Tidak hanya itu, Luffy juga menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache.

**Penyelesaian**

**Pada Skypie**
- Pindah ke directory `/etc/apache2/sites-available`.
- Edit file `super.franky.A13.com.conf` seperti pada gambar berikut:

  (- ss)
- Restart apache.

  ```
  service apache2 restart
  ```

**Pada Loguetown**
- Buka `www.super.franky.A13.com/publoc` (terdapat typo) menggunakan lynx.

  (- ss)

### Soal No. 13
Luffy juga meminta Nami untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.super.franky.yyy.com/public/js menjadi www.super.franky.yyy.com/js

**Penyelesaian**

**Pada Skypie**
- Pindah ke directory `/etc/apache2/sites-available`.
- Edit file `super.franky.A13.com.conf` seperti pada gambar berikut:

  (- ss)
- Restart apache.

  ```
  service apache2 restart
  ```

**Pada Loguetown**
- Buka `www.super.franky.A13.com/js` menggunakan lynx.

 (- ss)


### Soal No. 14
Dan Luffy meminta untuk web www.general.mecha.franky.yyy.com hanya bisa diakses dengan port 15000 dan port 15500.

**Penyelesaian**

**Pada Skypie**
- Pindah ke directory `/etc/apache2/sites-available`.
- Copy file `000-default.conf` menjadi file `general.mecha.franky.A13.com.conf`.
- Edit file `general.mecha.franky.A13.com.conf` seperti pada gambar berikut:

  (- ss)
- Edit file `/etc/apache2/ports.conf` untuk mengaktifkan port 15000 dan port 15500 seperti pada gambar berikut:

  (- ss)
- Aktifkan konfigurasi general.mecha.franky.A13.com.

  ```
  a2ensite general.mecha.franky.A13.com
  ```
- Restart apache.

  ```
  service apache2 restart
  ```
- Pindah ke directory `/var/www`.
- Download file zip menggunakan `wget`.

  ```
  wget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/general.mecha.franky.zip
  ```
- Lakukan unzip.

  ```
  unzip general.mecha.franky.zip
  ```
- Rename folder `general.mecha.franky` menjadi `general.mecha.franky.A13.com` dan terdapat isi file seperti pada gambar berikut:

  (- ss)

**Pada Loguetown**
- Buka `www.general.mecha.franky.A13.com` menggunakan lynx.

  (- ss)
- Buka `www.general.mecha.franky.A13.com:15000` menggunakan lynx.

  (- ss)
- Buka `www.general.mecha.franky.A13.com:15500` menggunakan lynx.

  (- ss)


### Soal No. 15
dengan autentikasi username luffy dan password onepiece dan file di /var/www/general.mecha.franky.yyy

**Penyelesaian**

**Pada Skypie**
- Pindah ke directory `/etc/apache2/sites-available`.
- Edit file `general.mecha.franky.A13.com.conf` seperti pada gambar berikut:

  (- ss)
- Jalankan perintah berikut untuk membuat akun autentikasi baru dengan username `luffy`. Kita akan diminta untuk memasukkan password baru dan confirm password tersebut diisi `onepiece`.

  ```
  htpasswd -c /etc/apache2/.htpasswd luffy
  ```
- Restart apache.

  ```
  service apache2 restart
  ```

**Pada Loguetown**
- Buka `www.general.mecha.franky.A13.com:15000` menggunakan lynx.

  (- ss)
  (- ss)
  (- ss)



### Soal No. 16
Dan setiap kali mengakses IP Skypie akan dialihkan secara otomatis ke www.franky.yyy.com

**Penyelesaian**

**Pada Skypie**
- Pindah ke directory `/etc/apache2/sites-available`.
- Edit file `000-default.conf` seperti pada gambar berikut:

  (- ss)
- Restart apache.

  ```
  service apache2 restart
  ```

**Pada Loguetown**
- Buka `192.175.2.4` (IP Skypie) menggunakan lynx.

  (- ss)

### Soal No. 17
Dikarenakan Franky juga ingin mengajak temannya untuk dapat menghubunginya melalui website www.super.franky.yyy.com, dan dikarenakan pengunjung web server pasti akan bingung dengan randomnya images yang ada, maka Franky juga meminta untuk mengganti request gambar yang memiliki substring “franky” akan diarahkan menuju franky.png. Maka bantulah Luffy untuk membuat konfigurasi dns dan web server ini!

**Penyelesaian**

**Pada Skypie**
- Jalankan perintah `a2enmod rewrite` untuk mengaktifkan module rewrite.
- Restart apache dengan perintah `service apache2 restart`.
- Tambahkan file baru `.htaccess` pada folder `/var/www/super.franky.A13.com`, di mana file tersebut akan dimodifikasi menjadi:

  (- ss)
- Pindah ke directory `/etc/apache2/sites-available`.
- Edit file `super.franky.A13.com.conf` agar file `.htaccess` dapat berjalan seperti pada gambar berikut:

  (- ss)
- Restart apache.

  ```
  service apache2 restart
  ```

**Pada Loguetown**
- Buka `www.super.franky.A13.com/public/images/franky.png` menggunakan lynx.

(- ss)
- Buka `www.super.franky.A13.com/public/images/eyeoffranky.jpg` menggunakan lynx.

  (- ss)
- Buka `www.super.franky.A13.com/public/images/background-frank.jpg` menggunakan lynx.

  (- ss)

### Kendala yang Dialami
- Tidak bisa menginstall DNS3 (Sabrina)

