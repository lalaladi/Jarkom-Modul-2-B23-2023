# Jarkom-Modul-2-B23-2023
# **Praktikum Modul 2 Jaringan Komputer**
Berikut adalah Repository dari Kelompok B23 untuk pengerjaan Praktikum Modul 2. Dalam Repository ini terdapat daftar anggota kelompok, dokumentasi dan Penjelasan tiap soal, _Screenshot_, dan Kendala yang Dialami. 

# **Anggota Kelompok**
| Nama                      | NRP        | Kelas               |
| ------------------------- | ---------- | ------------------- |
| Armadya Hermawan Sarwono  | 5025211243 | Jaringan Komputer B |
| Dian Lies Seviona Dabukke | 5025211080 | Jaringan Komputer B |

# **Dokumentasi dan Penjelasan Soal**
Berikut adalah dokumentasi yang tiap soal dan penjelasan terkait perintah yang digunakan :

## **Soal Nomor 1**
Buatlah topologi dengan:<br>
Yudhistira – DNS Master<br>
Werkudara – DNS Slave<br>
Arjuna – Load Balancer<br>
Prabukusuma, Abimanyu, dan Wisanggeni – Web Server<br>
Nakula dan Sadewa – Client<br>
<br>**Langkah Penyelesaian Soal 1 :** <br>
a). ![1](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/4c437640-5344-4752-9aed-3becda9ebd6f)
<br>
b). Lakukan konfigurasi IP tiap node<br> 
Pandudewanata
```bash
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.20.1.1
	netmask 255.255.255.0
auto eth2
iface eth2 inet static
	address 10.20.2.1
	netmask 255.255.255.0
auto eth3
iface eth1 inet static
	address 10.20.3.1
	netmask 255.255.255.0
```
Werkudara 
```bash
auto eth0
iface eth0 inet static
	address 10.20.1.5
	netmask 255.255.255.0
	gateway 10.20.1.1
```
Yudhistira
```bash
auto eth0
iface eth0 inet static
	address 10.20.1.4
	netmask 255.255.255.0
	gateway 10.20.1.1
```
Arjuna 
```bash
auto eth0
iface eth0 inet static
	address 10.20.2.2
	netmask 255.255.255.0
	gateway 10.20.2.1
```
Prabukusuma
```bash
auto eth0
iface eth0 inet static
	address 10.20.3.2
	netmask 255.255.255.0
	gateway 10.20.3.1
```
 Abimanyu
 ```bash
auto eth0
iface eth0 inet static
	address 10.20.3.3
	netmask 255.255.255.0
	gateway 10.20.3.1
```
Wisanggeni
```bash
auto eth0
iface eth0 inet static
	address 10.20.3.4
	netmask 255.255.255.0
	gateway 10.20.3.1
```
Nakula
```bash
auto eth0
iface eth0 inet static
	address 10.20.1.2
	netmask 255.255.255.0
	gateway 10.20.1.1
```
Sadewa
```bash
auto eth0
iface eth0 inet static
	address 10.20.1.3
	netmask 255.255.255.0
	gateway 10.20.1.2
```
**Untuk tiap command yang ada dijalankan terlebih dahulu di terminal lalu simpan di /root/** <br>
c). Pada _/root/.bashrc_ dengan Prefix IP kelompok ini (B23) 
```bash
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.20.0.0/16
```
d). Agar dapat terhubung ke internet melalui Pandudewanata ketik dalam _/root/.bashrc_ tiap node ubuntu yang dibuat 
```bash
 echo nameserver 192.168.122.1 > /etc/resolv.conf
```
Lalu jika dilakukan ping google.com pada ubuntu yang dipilih akan terlihat seperti berikut 
![2](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/5abc4a07-c91c-41d6-a125-ac463a8ea528)
![3](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/506adde8-b914-4619-8839-5641a400b031)

## **Soal Nomor 2**
Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok.

<br>**Langkah Penyelesaian Soal 2 :** <br>
a). Pada /root/.bashrc  di Yudhistira :
```bash
apt-get update
apt-get install bind9 -ya
```
 <br>
b). Pembuatan Domain<br>	
- Pada terminal Yudhistira ketik : *nano /etc/bind/named.conf.local* 
<br>
Isi dengan : 
```bash
zone "arjuna.b23.com" {
    type master;
    file "/etc/bind/jarkom/arjuna";
    };
```
<br>
- Pada terminal :
```bash
mkdir /etc/bind/jarkom
cp /etc/bind/db.local /etc/bind/jarkom/arjuna
nano /etc/bind/jarkom/arjuna
```
![4](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/4b8a6610-100c-4d3e-8d94-f0a0d2d211d5)
<br>
- Pada */root/.bashrc*  tambahkan : *service bind9 restart*
<br>
**Untuk pembuktiannya :** <br>
- Pada /etc/resolv.conf, masukkan nameserver IP Yudhistira (nameserver 10.20.1.4)<br>
- Lalu ping domain dan alias yang telah dibuat<br>
![5](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/52981d84-cab9-4e67-8c7d-0f4af51f4214)
![34](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/f3537cbe-1df5-4b9c-a7e2-9ee4a034931c) 
<br>

## **Soal Nomor 3**
Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.

<br>**Langkah Penyelesaian Soal 3 :** <br>
a). Pada _/root/.bashrc_
```bash
apt-get update
apt-get install bind9 -y
```
<br>
b). Pembuatan Domain <br>
Pada terminal Yudhistira ketik _nano /etc/bind/named.conf.local_ <br>
Isi dengan : 
```bash
zone "abimanyu.b23.com" {
    type master;
    file "/etc/bind/jarkom/abimanyu";
};
```
<br>
Pada terminal :
```bash
mkdir /etc/bind/jarkom
cp /etc/bind/db.local /etc/bind/jarkom/abimanyu
nano /etc/bind/jarkom/abimanyu
```
![6](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/95f887f9-f8cf-493d-8839-d725d45a417e)
<br>
Pada terminal : *service bind9 restart*
**Untuk pembuktiannya :** <br>
Pada /etc/resolv.conf, masukkan nameserver IP Yudhistira **nameserver 10.20.1.4**
Lalu ping domain yang telah dibuat.

## **Soal Nomor 4**
Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

<br>**Langkah Penyelesaian Soal 4 :** <br>
a). Edit _/etc/bind/jarkom/abimanyu_ di Yudhistira <br>
![7](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/85ea88f4-07d1-410c-a5f0-aa8906f97513)
<br>
b). Pada terminal Yudhistira : _service bind9 restart_ 

## **Soal Nomor 5**
Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)

<br>**Langkah Penyelesaian Soal 5 :** <br>
a). Pada terminal Yudhistira ketik : _nano /etc/bind/named.conf.local_ <br> 
Isi dengan : 
```bash
zone "3.20.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/reverseabimanyu";
};
```
<br>
b). Copy-kan file db.local :*cp /etc/bind/db.local /etc/bind/jarkom/reverseabimanyu*
<br>
c). Lalu edit file reverseabimanyu
![8](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/28d2c5f3-f828-4eff-8288-fc22e9843554)
<br>
d). Pada terminal : _service bind9 restart_ 
<br>
e). Pada terminal Nakula dan tambahkan isi _/root/.bashrc_ : 
```bash
	apt-get update
	apt-get install dnsutils
```
**Bukti :** <br>
Pada terminal Nakula ketik : 
```bash 
	host -t PTR 10.20.3.3
```
![9](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/f13ac826-cc01-42fb-a950-6cfad4290f26)
<br>

## **Soal Nomor 6**
Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

<br>**Langkah Penyelesaian Soal 6 :** <br>
a).  Pada terminal Yudhistira, edit _/etc/bind/named.conf.local_
```bash
zone "arjuna.b23.com" {
    type master;
    notify yes;
    also-notify {10.20.1.5 ; };
    allow-transfer { 10.20.1.5 ; }; 
    file "/etc/bind/jarkom/arjuna";
};

zone "abimanyu.b23.com" {
    type master;
    notify yes;
    also-notify {10.20.1.5 ; };
    allow-transfer { 10.20.1.5 ; }; 
    file "/etc/bind/jarkom/abimanyu";
};
```
<br>
b). Pada terminal : *service bind9 restart* 
<br>
c). Konfigurasi Werkudara <br>
- Pada terminal dan simpan di /root/.bashrc
```bash
	apt-get update
	apt-get install bind9 -y
```
Tambahkan syntax di /etc/bind/named.conf.local
```bash
zone "arjuna.b23.com" {
    type slave;
    masters {10.20.1.4; };
    file "/var/lib/bind/jarkom/arjuna";

zone "abimanyu.b23.com" {
    type slave;
    masters {10.20.1.4; };
    file "/var/lib/bind/jarkom/abimanyu";
};
```

Pada terminal : service bind9 restart
**Bukti :**
1. Lakukan  service bind9 stop di Yudhistira <br>
2. Lakukan pengaturan nameserver dengan menambahkan isi */etc/resolv.conf* di Nakula :
```bash
nameserver 10.20.1.4
nameserver 10.20.1.5
```
3. Lalu ping arjuna atau abimanyu.

## **Soal Nomor 7**
Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.

<br>**Langkah Penyelesaian Soal 7 :** <br>
a). Konfigurasi pada Yudhistira : <br>
edit _/etc/bind/jarkom/abimanyu_
![10](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/1038d860-a6d5-46cd-bffc-f168fb3b336d)
<br>

Edit  /etc/bind/named.conf.options dengan comment dan tambahkan baris seperti gambar
![11](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/0d1e1b8f-692c-4b6f-9a32-656f39ca89aa) 
Pada terminal : _service bind9 restart_
<br>
b). Konfigurasi pada Werkudara<br>
Edit _/etc/bind/named.conf.options_ dengan comment dan tambahkan baris seperti gambar
![12](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/71d924b7-7524-4788-891c-efc8b59ed843)
<br>
Tambahkan pada /etc/bind/named.conf.local 
```bash
zone "baratayuda.abimanyu.b23.com" {
	type master;
	file "/var/lib/bind/delegasi/baratayuda";
	};
```
<br>

Pada terminal : 
```bash
mkdir /etc/bind/delegasi
cp /etc/bind/db.local /etc/bind/delegasi/baratayuda
```
![13](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/119245b2-ac08-42d9-b7b5-214e09e96e9d)
<br>

Pada terminal : _service bind9 restart_ <br>
**Bukti :** 
![14](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/5bbb0ae2-a5cd-4498-aa17-13d84245ff5b)
<br>

## **Soal Nomor 8**
Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

<br>**Langkah Penyelesaian Soal 8 :** <br>
a). Edit _/etc/bind/delegasi/baratayuda_ di Werkudara
![15](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/5f481705-b350-420a-aa5e-7d8feb953757)	
<br>
b). Pada terminal Werkudara: _service bind9 restart_ <br>
**Bukti :**
![16](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/64d2c08d-905e-4954-9165-cadbbaa37cf1)
![17](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/cf67ee44-8af6-4382-b674-cc2a762216e5)
<br>

## **Soal Nomor 9**
Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.

<br>**Langkah Penyelesaian Soal 9** <br>
Pada 3 Nginx diatas, lakukan hal yang sama:
a). Pada terminal dan /root/.bashrc : 
```bash
apt-get update && apt install nginx php php-fpm -y
```
b). Pada terminal : _mkdir /var/www/arjuna.b23.com_ <br>
c). Masuk ke directory /var/www/arjuna.b23.com dan buat file index.php yang berisi : 
```bash
<?php
 echo "Halo, Kamu berada di Prabukusuma"; #ganti sesuai node
?>
```
<br>
d). Masuk ke directory /etc/nginx/sites-available dan nano arjuna dengan isi 
```bash
  server {

 	listen 80;

 	root /var/www/arjuna.b23.com;

 	index index.php index.html index.htm;
 	server_name _;

 	location / {
 			try_files $uri $uri/ /index.php?$query_string;
 	}

 	# pass PHP scripts to FastCGI server
 	location ~ \.php$ {
 	include snippets/fastcgi-php.conf;
 	fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
 	}

 	location ~ /\.ht {
 	deny all;
 	}

 	error_log /var/log/nginx/arjuna_error.log;
 	access_log /var/log/nginx/arjuna_access.log;
  	}
```
<br>
f). Pada terminal :
```bash
ln -s /etc/nginx/sites-available/arjuna /etc/nginx/sites-enabled
service nginx restart
```
**Bukti :**
nginx -t pada tiap nginx worker
![18](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/c1bf76c5-5f38-4a9e-8b12-cd2bec482bdf)
![19](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/dfc9d9cf-cc63-4a74-97e2-7806be2fa51d)
![20](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/14adc548-7b49-44b9-8d4d-48cb8e76eb44)
<br>
	
## **Soal Nomor 10**
Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh:
- Prabakusuma:8001
- Abimanyu:8002
- Wisanggeni:8003

<br>**Langkah Penyelesaian Soal 10** <br>
a). Pada terminal Arjuna dan simpan di /root/.bashrc 
```bash
apt-get update
apt-get install nginx
```
b). Pada terminal : 
```bash
service nginx start
service nginx status
cd /etc/nginx/sites-available
nano lb-arjuna :
```
![21](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/372d4fa4-edae-4558-b1fe-043fc497db6f)
<br>
c). Pada tiap masing-masing worker : <br>
**Prabukusuma** <br>
masuk ke directory /etc/nginx/sites-available dan nano arjuna dengan isi
```bash
server {

 	listen 8001; #sesuai port yang ada di soal

 	root /var/www/arjuna.b23.com;

 	index index.php index.html index.htm;
 	server_name _;

 	location / {
 			try_files $uri $uri/ /index.php?$query_string;
 	}

 	# pass PHP scripts to FastCGI server
 	location ~ \.php$ {
 	include snippets/fastcgi-php.conf;
 	fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
 	}

 location ~ /\.ht {
 			deny all;
 	}

 	error_log /var/log/nginx/arjuna.log;
 	access_log /var/log/nginx/arjuna_access.log;
	}
```
<br>

**Abimanyu**<br>
masuk ke directory /etc/nginx/sites-available dan nano arjuna dengan isi
```bash
server {

 	listen 8002; #sesuai port yang ada di soal

 	root /var/www/arjuna.b23.com;

 	index index.php index.html index.htm;
 	server_name _;

 	location / {
 			try_files $uri $uri/ /index.php?$query_string;
 	}

 	# pass PHP scripts to FastCGI server
 	location ~ \.php$ {
 	include snippets/fastcgi-php.conf;
 	fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
 	}

 location ~ /\.ht {
 			deny all;
 	}

 	error_log /var/log/nginx/arjuna.log;
 	access_log /var/log/nginx/arjuna_access.log;
 }
```
<br>

**Wisanggeni**<br>
masuk ke directory /etc/nginx/sites-available dan nano arjuna dengan isi
```bash
server {

 	listen 8003; #sesuai port yang ada di soal

 	root /var/www/arjuna.b23.com;

 	index index.php index.html index.htm;
 	server_name _;

 	location / {
 			try_files $uri $uri/ /index.php?$query_string;
 	}

 	# pass PHP scripts to FastCGI server
 	location ~ \.php$ {
 	include snippets/fastcgi-php.conf;
 	fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
 	}

 location ~ /\.ht {
 			deny all;
 	}

 	error_log /var/log/nginx/arjuna.log;
 	access_log /var/log/nginx/arjuna_access.log;
 }
```
<br>

d). Pada terminal : 
```bash
ln -s /etc/nginx/sites-available/lb-arjuna /etc/nginx/sites-enabled
service nginx restart
```
**Bukti :** 
Pada Nakula, install lynx dan simpan di /root/.bashrc
```bash
apt-get update
apt-get install lynx
```
<br>
_lynx http://10.20.3.1:8001_
![22](https://github.com/lalaladi/Jarkom-Modul-1-B23-2023/assets/90541607/74e05af9-e19a-4000-974d-a60e579ed06c)

## **Soal Nomor 11**
Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy

<br>**Langkah Penyelesaian Soal 11 ** <br>
Konfigurasi apache
	a). Install apache dan simpan di /root/.bashrc : apt-get install apache2
      				    service apache2 start
			       				    apt-get install php
							    apt-get install libapache2-mod-php7.0
    service apache2 restart
b).  Pada terminal Abimanyu : cd /var/www/
					Mkdir html
	c). Buat file dengan perintah nano /var/www/html/index.php 
```bash
<?php
	phpinfo();
?>
```
<br>

Konfigurasi port
Pada terminal Abimanyu : cd /etc/apache2/sites-available			      
       Cp 000-default.conf abimanyu.b23.com.conf
			        nano abimanyu.b23.com.conf : 
ganti DocumentRoot menjadi /var/www/abimanyu.b23
 ServerName abimanyu.b23.com
 ServerAlias www.abimanyu.b23.com


Tambahkan pada /root/.bashrc : 
	a2ensite abimanyu.b23.com
	service apache2 restart

## **Soal Nomor 12**
Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.

<br>**Langkah Penyelesaian Soal 12 ** <br>
	a). Pada terminal abimanyu 
		Nano /etc/apache2/sites-available/ abimanyu.b23.com.conf	


Bukti : pada nakula ketik lynx abimanyu.b23.com/home

## **Soal Nomor 13**
Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy

<br>**Langkah Penyelesaian Soal 13 ** <br>
	Pada terminal Abimanyu : cd /etc/apache2/sites-available			      
      	       cp 000-default.conf parikesit.abimanyu.b23.com.conf
			        	      parikesit.abimanyu.b23.com.conf : 
ganti DocumentRoot menjadi /var/www/parikesit.abimanyu.b23
 ServerName parikesit.abimanyu.b23.com
 ServerAlias www.parikesit.abimanyu.b23.com


Tambahkan pada /root/.bashrc : 
	a2ensite parikesit.abimanyu.b23.com.conf
	service apache2 restart
	
## **Soal Nomor 14**
Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).

<br>**Langkah Penyelesaian Soal 14 ** <br>
	a). folder /public
Pada terminal : nano /etc/apache2/sites-available/parikesit.abimanyu.b23.com.conf
Tambahkan: 
Service apache2 restart
Bukti : 
Di nakula, ketik lynx parikesit.abimanyu.b23.com/public: 
              lynx parikesit.abimanyu.b23.com/secret


## **Soal Nomor 15**
Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.

<br>**Langkah Penyelesaian Soal 15 ** <br>
	a). Pada terminal abimanyu : cd /var/www/parikesit.abimanyu.b23.com
					nano .htaccess

b). Edit nano /etc/apache2/sites-available/parikesit.abimanyu.b23.com.conf

service apache2 restart
Bukti : 
Pada nakula ketik : lynx parikesit.abimanyu.b23.com/error/404
		      lynx parikesit.abimanyu.b23.com/error/403

## **Soal Nomor 16**
Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi 
www.parikesit.abimanyu.yyy.com/js
 
<br>**Langkah Penyelesaian Soal 16 ** <br>
	a). Pada terminal abimanyu
		nano /etc/apache2/sites-available/ parikesit.abimanyu.b23.com.conf	

      b). service apache2 restart
     c). Pada terminal pindah ke /var/www/parikesit.abimanyu.b23.com/public/js
Bukti :  

## **Soal Nomor 17**
Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.

<br>**Langkah Penyelesaian Soal 17 ** <br>
a). Konfigurasi port 14000 dan 14400
Masuk dir /etc/apache2/sites-available
cp 000-default.conf rjp.baratayuda.abimanyu.b23.com.conf 
nano rjp.baratayuda.abimanyu.b23.com.conf 


Tambahkan kedua port itu kedalam file ports.conf

Tambahkan pada /root/.bashrc : 
	a2ensite rjp.baratayuda.abimanyu.b23.com.conf
	service apache2 restart
Terminal :  -    cd /var/www
mkdir baratayuda.abimanyu.b23
cd baratayuda.abimanyu.b23
Nano index.php yang berisi : 
 ```bash
<?php
					echo “ini adalah rjp”;
?>
```
<br> 
Bukti : 

## **Soal Nomor 18**
Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.

<br>**Langkah Penyelesaian Soal 18 ** <br>
	a). Terminal abimanyu :
Masuk  cd /var/www/baratayuda.abimanyu.b23
Nano .htpasswd : berisi password dari soal
 - Edit dengan nano /etc/apache2/sites-available/rjp.baratayuda.abimanyu.b23.com.conf 

service apache2 restart
Bukti : 

## **Soal Nomor 19**
Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)

<br>**Langkah Penyelesaian Soal 19 ** <br>
	a). Masuk dir /etc/apache2/sites-available
	b)  nano abimanyu.b23.com.conf seperti  berikut : 


## **Soal Nomor 20**
Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.

<br>**Langkah Penyelesaian Soal 19 ** <br>


