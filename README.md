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
- Buat topologi seperti pada soal
![topologi_modul_2](https://user-images.githubusercontent.com/75364000/139527146-eebc3968-d970-41c0-bd22-611048b8bc8c.png)

- Setting network masing-masing node dengan fitur `Edit network configuration`:
**Foosha (sebagai Router)**
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.181.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.181.2.1
	netmask 255.255.255.0
```

**Loguetown (sebagai Client)**
```
auto eth0
iface eth0 inet static
	address 192.181.1.2
	netmask 255.255.255.0
	gateway 192.181.1.1
```

**Alabasta (sebagai Client)**
```
auto eth0
iface eth0 inet static
	address 192.181.1.3
	netmask 255.255.255.0
	gateway 192.181.1.1
```

**EniesLobby (sebagai DNS Master)**
```
auto eth0
iface eth0 inet static
	address 192.181.2.2
	netmask 255.255.255.0
	gateway 192.181.2.1
```

**Water7 (sebagai DNS Slave)**
```
auto eth0
iface eth0 inet static
	address 192.181.2.3
	netmask 255.255.255.0
	gateway 192.181.2.1
```

**Skypie (sebagai Web Server)**
```
auto eth0
iface eth0 inet static
	address 192.181.2.4
	netmask 255.255.255.0
	gateway 192.181.2.1
```

- Jalankan `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.181.0.0/16` pada router `Foosha`.
- Jalankan ```echo nameserver 192.168.122.1 > /etc/resolv.conf ``` pada masing - masing node.
- Restart semua node dan coba `ping google.com`

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
  (- SS)
- Create folder `kaizoku` pada `/etc/bind`.
  ```
  mkdir /etc/bind/kaizoku
  ```
- Copy file `db.local` pada path `/etc/bind` ke folder `kaizoku` dan rename menjadi `franky.A13.com`.
  ```
  cp /etc/bind/db.local /etc/bind/kaizoku/franky.A13.com
  ```
- Edit `/etc/bind/kaizoku/franky.A13.com` seperti berikut:
  (- SS)
- Restart bind9.
  ```
  service bind9 restart
  ```

**Loguetown**
- Edit file `/etc/resolv.conf` seperti berikut:
  (- SS)
- Selanjutnya, ping domain `franky.A13.com` dan `www.franky.A13.com`.
  (- SS)

### Soal No. 3
Setelah itu buat subdomain super.franky.yyy.com dengan alias www.super.franky.yyy.com yang diatur DNS nya di EniesLobby dan mengarah ke Skypie.

**Penyelesaian**
**EniesLobby**
- Edit file `/etc/bind/kaizoku/franky.A13.com` seperti berikut:
  (- SS)
- Restart bind9.
  ```
  service bind9 restart
  ```

**Loguetown**
- Selanjutnya, ping domain `super.franky.A13.com` dan `www.super.franky.A13.com`
  (- SS)

### Soal No. 4
Buat juga reverse domain untuk domain utama.

**Penyelesaian**
**EniesLobby**
- Edit `/etc/bind/named.conf.local` seperti berikut:
  (- SS)
- Copy file `db.local` pada path `/etc/bind` ke dalam folder `kaizoku` dan rename menjadi `2.181.192.in-addr.arpa`.
  ```
  cp /etc/bind/db.local /etc/bind/kaizoku/2.181.192.in-addr.arpa
  ```
- Edit file `/etc/bind/kaizoku/2.181.192.in-addr.arpa` seperti berikut:
  (- SS)
- Restart bind9.
  ```
  service bind9 restart
  ```

**Pada Loguetown**
- Check konfigurasi dengan perintah `host -t PTR 192.181.2.2`.
  (- SS)

### Soal No. 5
Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak, maka buat Water7 sebagai DNS Slave untuk domain utama.

**Penyelesaian**
**EniesLobby**
- Edit `/etc/bind/named.conf.local` seperti berikut:
  (- SS)
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
  (- SS)
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
  (- SS)
- Selanjutnya ping domain `franky.A13.com`.
  (- SS)

### Soal No. 6
Setelah itu terdapat subdomain mecha.franky.yyy.com dengan alias www.mecha.franky.yyy.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo.

**Penyelesaian**
**EniesLobby**
- Edit `/etc/bind/kaizoku/franky.A13.com` seperti berikut:
  (- SS)
- Edit `/etc/bind/named.conf.options` seperti berikut:
  (- SS)
- Edit `/etc/bind/named.conf.local` seperti berikut:
  (- SS)
- Restart bind9.
  ```
  service bind9 restart
  ```

**Pada Water7**
- Edit `/etc/bind/named.conf.options` seperti berikut:
  (- SS)
- Edit `/etc/bind/named.conf.local` seperti pada gambar berikut:
  (- SS)
- Create folder `sunnygo` di dalam `/etc/bind`.
  ```
  mkdir /etc/bind/sunnygo
  ```
- Copy file `db.local` pada path `/etc/bind` ke dalam folder `sunnygo` dan rename `mecha.franky.A13.com`.
  ```
  cp /etc/bind/db.local /etc/bind/sunnygo/mecha.franky.A13.com
  ```
- Edit `/etc/bind/sunnygo/mecha.franky.B09.com` seperti pada gambar berikut:
  (- SS)
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
  (- SS)
- Restart bind9.
  ```
  service bind9 restart
  ```

**Pada Loguetown**
- Selanjutnya ping domain `general.mecha.franky.A13.com` dan `www.general.mecha.franky.A13.com`.
  (- SS)

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
  (- SS)
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
- Rename folder `franky` menjadi `franky.B09.com` dan terdapat isi file seperti pada gambar berikut:
  (- SS)

**Loguetown**
- Install aplikasi lynx.
  ```
  apt-get install lynx -y
  ```
- Buka `www.franky.A13.com` menggunakan lynx.
  (- SS)

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

