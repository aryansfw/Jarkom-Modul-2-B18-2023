# Jarkom-Modul-2-B18-2023

## Kelompok B18

|          Nama          |    NRP     |
| :--------------------: | :--------: |
|  Aryan Shafa Wardana   | 5025211031 |
| Shazia Ingeyla Naveeda | 5025211203 |

## Nomor 1

### Soal <br>

Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi [dengan pembagian](https://docs.google.com/spreadsheets/d/1OqwQblR_mXurPI4gEGqUe7v0LSr1yJViGVEzpMEm2e8/edit#gid=1475903193).sebagai berikut. Folder topologi dapat diakses pada [drive berikut.](https://drive.google.com/drive/folders/1Ij9J1HdIW4yyPEoDqU1kAwTn_iIxg3gk)

### Pembahasan<br>

Topologi yang kelompok B18 gunakan adalah topologi [04,](https://drive.google.com/drive/folders/1Ij9J1HdIW4yyPEoDqU1kAwTn_iIxg3gk) dimana **NAT1** terhubung ke **Router**, yang kemudian terhubung ke 3 switch yang masing-masing terhubung ke beberapa node. Seperti **Switch1** yang terhubung ke node **DNS**, lalu **Switch2** yang terhubung ke node **Load Balancer** dan **Web Server**, sedangan **Switch3** terhubung ke node **Client**

> Topologi Kelompok B18

![topologi](https://github.com/aryansfw/Jarkom-Modul-2-B18-2023/assets/114483889/53a2e6eb-f4e6-429e-84fc-15547805a4ae)


**Konfigurasi Node**

- ROUTER - _Pandudewanata_

  ```
  auto eth0
  iface eth0 inet static
  	address 192.187.0.1
  	netmask 255.255.255.0

  auto eth1
  iface eth1 inet static
  	address 192.187.1.1
  	netmask 255.255.255.0

  auto eth2
  iface eth2 inet static
  	address 192.187.2.1
  	netmask 255.255.255.0

  auto eth3
  iface eth3 inet dhcp
  ```

- DNS MASTER - _Yudhistira_

  ```
  auto eth0
  iface eth0 inet static
  	address 192.187.2.2
  	netmask 255.255.255.0
  	gateway 192.187.2.1
  ```

- DNS SLAVE - _Werkudara_

  ```
  auto eth0
  iface eth0 inet static
  	address 192.187.2.3
  	netmask 255.255.255.0
  	gateway 192.187.2.1
  ```

- LOAD BALANCER - _Arjuna_

  ```
  auto eth0
  iface eth0 inet static
  	address 192.187.1.5
  	netmask 255.255.255.0
  	gateway 192.187.1.1
  ```

- WEB SERVER - _Abimanyu_

  ```
  auto eth0
  iface eth0 inet static
  	address 192.187.1.2
  	netmask 255.255.255.0
  	gateway 192.187.1.1
  ```

- WEB SERVER - _Prabukusuma_

  ```
  auto eth0
  iface eth0 inet static
  	address 192.187.1.3
  	netmask 255.255.255.0
  	gateway 192.187.1.1
  ```

- WEB SERVER - _Wisanggeni_

  ```
  auto eth0
  iface eth0 inet static
  	address 192.187.1.4
  	netmask 255.255.255.0
  	gateway 192.187.1.1
  ```

- CLIENT - _Nakula_

  ```
  auto eth0
  iface eth0 inet static
  	address 192.187.0.2
  	netmask 255.255.255.0
  	gateway 192.187.0.1
  ```

- CLIENT - _Sadewa_

  ```
  auto eth0
  iface eth0 inet static
  	address 192.187.0.3
  	netmask 255.255.255.0
  	gateway 192.187.0.1
  ```

## Nomor 2 dan Nomor 3

### Soal Nomor 2

Buatlah website utama pada node arjuna dengan akses ke **arjuna.yyy.com** dengan alias **www.arjuna.yyy.com** dengan yyy merupakan kode kelompok.

### Soal Nomor 3

Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke **abimanyu.yyy.com** dan alias **www.abimanyu.yyy.com**.

### Pembahasan

**Pembuatan Domain**

- Edit konfigurasi **named.conf.local** dengan menambahkan zone untuk masing-masing domain. Lakukan perintah berikut pada _Yudhistira_

  ```
  nano /etc/bind/named.conf.local
  ```

- Isikian Konfigurasi domain seperti syntax berikut:

  ```
  zone "arjuna.b18.com" {
  type master;
  file "/etc/bind/website/arjuna.b18.com";
  };

  zone "abimanyu.b18.com" {
  type master;
  file "/etc/bind/website/abimanyu.b18.com";
  };
  ```

- Kemudian buka file masing-masing domain. Edit name server dan alamat IPnya. Dan tambahkan konfigurasi untuk membuat alias dengan tipe `CNAME`

> Domain Arjuna

```
$TTL    604800
@       IN      SOA     arjuna.b18.com. root.arjuna.b18.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      arjuna.b18.com.
@       IN      A       192.187.1.5       ; IP LB- Arjuna
www     IN      CNAME   arjuna.bl8.com.
@       IN      AAAA    ::1
```

> Domain Abimanyu

```
$TTL    604800
@       IN      SOA     abimanyu.b18.com. root.abimanyu.b18.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      abimanyu.b18.com.
@       IN      A       192.187.1.2       ; IP Abimanyu
www     IN      CNAME   abimanyu.bl8.com.
@       IN      AAAA    ::1
```

**Testing**

## Nomor 4

### Soal

Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain **parikesit.abimanyu.yyy.com** yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

### Pembahasan

Subdomain adalah bagian dari sebuah nama domain induk. Subdomain umumnya mengacu ke suatu alamat fisik di sebuah situs. Karena diminta untuk membuat subdomain dari domain utama abimanyu maka kita hanya akan menambahkan konfigurasi subdomain abimanyu.b18.com di file abimanyu yang mengarah ke IP abimanyu.

- Lakukan perintah berikut pada _Yudhistira_ untuk mengedit konfigurasi **abimanyu.b18.com**

  ```
  nano /etc/bind/website/abimanyu.b18.com
  ```

- Lalu tambahkan subdomain untuk **abimanyu.b18.com** yang mengarah ke IP abimanyu.

  ```
  ;
  ; BIND data file for local loopback interface
  ;
  $TTL    604800
  @       IN      SOA     abimanyu.b18.com. root.abimanyu.b18.com. (
  							2         ; Serial
  						604800        ; Refresh
  						86400         ; Retry
  						2419200       ; Expire
  						604800 )      ; Negative Cache TTL
  ;
  @      			IN      NS      abimanyu.b18.com.
  @       		IN      A       10.29.1.2       	; IP Abimanyu
  www     		IN      CNAME   abimanyu.d15.com.
  parikesit       IN      A       10.29.1.2       	; IP Abimanyu
  www.parikesit   IN      CNAME   parikesit.abimanyu.b18.com.
  @       		IN      AAAA    ::1
  ```

## Nomor 11

### Soal

Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server **www.abimanyu.yyy.com**. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy

### Pembahasan

Pada soal-soal sebelumnya, kita sudah membuat DNS yang menggunakan IP abimanyu yang mengarah pada www.abimanyu.b18.com. Untuk membuat konfigurasi Apache Web Server, kita bisa ke node Abimanyu lalu install apache2 dan php menggunakan command

```bash
$ apt-get install apache2 libapache2-mod-php7.0 php php-fpm
```

Lalu pada drive resources, tersedia folder **abimanyu.yyy.com** yang bisa kita rename menjadi **abimanyu.b18** dan copy ke path `/var/www/`. Supaya web tersebut bisa diakses, kita harus bikin file konfigurasi apache2 **abimanyu.b18.com.conf** di `/etc/apache2/sites-available/` yang berisi

> /etc/apache2/sites-available/abimanyu.b18.com.conf

```apache
<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/abimanyu.b18
	ServerName abimanyu.b18.com
	ServerAlias www.abimanyu.b18.com

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Supaya bisa diakses sebagai website, kita bisa menggunakan command

```
$ a2ensite abimanyu.b18.com.conf
```

Lalu mengaktifkan service `php7.0-fpm` dan `apache2` menggunakan command

```
$ service php7.0-fpm start
$ service apache2 restart
```

Untuk mencoba webnya, kita bisa pindah ke node Client-Nakula atau Client-Sadewa, lalu membuka web menggunakan command

```
$ lynx www.abimanyu.b18.com
```

Maka akan muncul tampilan seperti ini.

![abimanyu.b18.com](images/11.png)

## Nomor 12

### Soal

Setelah itu ubahlah agar url **www.abimanyu.yyy.com/index.php/home** menjadi **www.abimanyu.yyy.com/home**.

### Pembahasan

Pada node Abimanyu, pada path `/etc/apache2/sites-available/abimanyu.b18.com.conf`, kita bisa menambahkan line berikut

> /etc/apache2/sites-available/abimanyu.b18.com.conf

```apache
<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/abimanyu.b18
	ServerName abimanyu.b18.com
	ServerAlias www.abimanyu.b18.com

	Alias "/home" "/var/www/abimanyu.b18/index.php/home"

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Lalu restart service `apache2` menggunakan

```
$ service apache2 restart
```

Untuk mencobanya, kita bisa ke node client lalu menjalankan command

```
$ lynx www.abimanyu.b18.com/home
```

Maka akan muncul tampilan seperti ini.

![abimanyu.b18.com/home](images/12.png)

## Nomor 13

### Soal

Selain itu, pada subdomain **www.parikesit.abimanyu.yyy.com**, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy

### Pembahasan

Kita bisa menggunakan cara yang sama dengan nomor 11. Kita bisa membuat file konfigurasi seperti berikut di node Abimanyu.

> /etc/apache2/sites-available/parikesit.abimanyu.b18.com.conf

```apache
<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/parikesit.abimanyu.b18
	ServerName parikesit.abimanyu.b18.com
	ServerAlias www.parikesit.abimanyu.b18.com

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Lalu copy folder yang ada di drive resources ke `/var/www/` dengan nama **parikesit.abimanyu.b18**. Setelah itu, kita bisa mengaktifkan website tersebut dan restart service `apache2` menggunakan

```
$ a2ensite parikesit.abimanyu.b18.com.conf
$ service apache2 restart
```

Untuk mencobanya, kita bisa ke node client lalu menjalankan command

```
$ lynx www.parikesit.abimanyu.b18.com
```

Maka akan muncul tampilan seperti ini.

![parikesit.abimanyu.b18.com](images/13.png)

## Nomor 14

### Soal

Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).

### Pembahasan

Pada soal ini, kita bisa ke node Abimanyu lalu edit konfigurasi web servernya menjadi seperti berikut.

> /etc/apache2/sites-available/parikesit.abimanyu.b18.com.conf

```apache
<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/parikesit.abimanyu.b18
	ServerName parikesit.abimanyu.b18.com
	ServerAlias www.parikesit.abimanyu.b18.com

	<Directory /var/www/parikesit.abimanyu.b18/public>
    		Options +Indexes
	</Directory>

	<Directory /var/www/parikesit.abimanyu.b18/secret>
		Deny from all
	</Directory>

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Lalu restart service `apache2` menggunakan

```
$ service apache2 restart
```

Untuk mencobanya, kita bisa ke node client lalu menjalankan command

```
$ lynx www.parikesit.abimanyu.b18.com/public
```

dan

```
$ lynx www.parikesit.abimanyu.b18.com/secret
```

Maka akan muncul tampilan seperti ini.

![public and secret](images/14.png)

## Nomor 15

### Soal

Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.

### Pembahasan

Pada soal ini, kita bisa ke node Abimanyu lalu edit konfigurasi web servernya menjadi seperti berikut.

> /etc/apache2/sites-available/parikesit.abimanyu.b18.com.conf

```apache
<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/parikesit.abimanyu.b18
	ServerName parikesit.abimanyu.b18.com
	ServerAlias www.parikesit.abimanyu.b18.com

	<Directory /var/www/parikesit.abimanyu.b18/public>
    		Options +Indexes
	</Directory>

	<Directory /var/www/parikesit.abimanyu.b18/secret>
		Deny from all
	</Directory>

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined

	ErrorDocument 403 /error/403.html
	ErrorDocument 404 /error/404.html
</VirtualHost>
```

Lalu restart service `apache2` menggunakan

```
$ service apache2 restart
```

Untuk mencobanya, kita bisa ke node client lalu menjalankan command

```
$ lynx www.parikesit.abimanyu.b18.com/secret
```

untuk mencoba error 403 forbidden, dan

```
$ lynx www.parikesit.abimanyu.b18.com/link-random
```

untuk mencoba error 404 not found. Maka akan muncul tampilan seperti ini.

![secret dan link random](images/15.png)

## Nomor 16

### Soal

Buatlah suatu konfigurasi virtual host agar file asset **www.parikesit.abimanyu.yyy.com/public/js** menjadi **www.parikesit.abimanyu.yyy.com/js**

### Pembahasan

Pada soal ini, kita bisa ke node Abimanyu lalu edit konfigurasi web servernya menjadi seperti berikut.

> /etc/apache2/sites-available/parikesit.abimanyu.b18.com.conf

```apache
<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/parikesit.abimanyu.b18
	ServerName parikesit.abimanyu.b18.com
	ServerAlias www.parikesit.abimanyu.b18.com

	<Directory /var/www/parikesit.abimanyu.b18/public>
    		Options +Indexes
	</Directory>

	<Directory /var/www/parikesit.abimanyu.b18/secret>
		Deny from all
	</Directory>

	Alias "/js" "/var/www/parikesit.abimanyu.b18/public/js"

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined

	ErrorDocument 403 /error/403.html
	ErrorDocument 404 /error/404.html
</VirtualHost>
```

Lalu restart service `apache2` menggunakan

```
$ service apache2 restart
```

Untuk mencobanya, kita bisa ke node client lalu menjalankan command

```
$ lynx www.parikesit.abimanyu.b18.com/js
```

Maka akan muncul tampilan seperti ini.

![parikesit.abimanyu.b18.com/js](images/16.png)

## Nomor 17

### Soal

Agar aman, buatlah konfigurasi agar **www.rjp.baratayuda.abimanyu.yyy.com** hanya dapat diakses melalui port 14000 dan 14400.

### Pembahasan

Pada soal ini, kita harus membuat webserver untuk www.rjp.baratayuda.abimanyu.b18.com terlebih dahulu. Kita bisa menggunakan cara yang sama seperti nomor 11, yaitu ke node Abimanyu kemudian rename folder **rjp.baratayuda.abimanyu.yyy.com** yang diperoleh dari drive resources menjadi **rjp.baratayuda.abimanyu.b18** lalu copy folder tersebut ke `/var/www`. Selanjutnya, tambahin file konfigurasi di `/etc/apache2/sites-available/rjp.baratayuda.abimanyu.b18.com.conf` berisi

> /etc/apache2/sites-available/rjp.baratayuda.abimanyu.b18.com.conf

```apache
<VirtualHost *:14000 *:14400>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/rjp.baratayuda.abimanyu.b18
	ServerName rjp.baratayuda.abimanyu.b18.com
	ServerAlias www.rjp.baratayuda.abimanyu.b18.com

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

<VirtualHost *:80>
	ServerName rjp.baratayuda.abimanyu.b18.com
	ServerAlias www.rjp.baratayuda.abimanyu.b18.com

	Redirect 403 /
</VirtualHost>

```

Setelah itu, kita bisa mengaktifkan website tersebut dan restart service `apache2` menggunakan

```
$ a2ensite rjp.baratayuda.abimanyu.b18.com.conf
$ service apache2 restart
```

Untuk mencobanya, kita bisa ke node client lalu menjalankan command

```
$ lynx www.rjp.baratayuda.abimanyu.b18.com
```

Tampilan yang muncul yaitu 403 forbidden.
![port 80](images/17-1.png)

Jika kita menjalankan command berikut,

```
$ lynx www.rjp.baratayuda.abimanyu.b18.com:14000
atau
$ lynx www.rjp.baratayuda.abimanyu.b18.com:14400
```

Maka akan muncul tampilan seperti ini.

![port 14000 and 14400](images/17-2.png)

## Nomor 18

### Soal

Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.

### Pembahasan

Pada soal ini, kita bisa ke node Abimanyu lalu menjalankan perintah berikut untuk membuat user untuk autentikasi

```
$ htpasswd -c /etc/apache2/.htpasswd Wayang
New password: baratayudab18
Re-type new password: baratayudab18
Adding password for user Wayang
```

Kemudian kita bisa edit konfigurasi seperti berikut

> /etc/apache2/sites-available/rjp.baratayuda.abimanyu.b18.com.conf

```apache
<VirtualHost *:14000 *:14400>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/rjp.baratayuda.abimanyu.b18
	ServerName rjp.baratayuda.abimanyu.b18.com
	ServerAlias www.rjp.baratayuda.abimanyu.b18.com

	<Directory /var/www/rjp.baratayuda.abimanyu.b18>
		AuthType Basic
		AuthName "Autentikasi Dulu"
		AuthUserFile "/etc/apache2/.htpasswd"
		Require user Wayang
	</Directory>

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

<VirtualHost *:80>
	ServerName rjp.baratayuda.abimanyu.b18.com
	ServerAlias www.rjp.baratayuda.abimanyu.b18.com

	Redirect 403 /
</VirtualHost>
```

Setelah itu, kita bisa restart service `apache2` menggunakan

```
$ service apache2 restart
```

Untuk mencobanya, kita bisa ke node client lalu menjalankan command

```
$ lynx www.rjp.baratayuda.abimanyu.b18.com:14000
atau
$ lynx www.rjp.baratayuda.abimanyu.b18.com:14400
```

Maka akan muncul tampilan seperti ini.

![autentikasi username](images/18-1.png)
![autentikasi password](images/18-2.png)
![berhasil autentikasi](images/18-3.png)

## Nomor 19

### Soal

Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com **(alias)**

### Pembahasan

Untuk soal ini, kita bisa ke node Abimanyu lalu edit konfigurasi seperti berikut.

> /etc/apache2/sites-available/abimanyu.b18.com.conf

```apache
<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/abimanyu.b18
	ServerName abimanyu.b18.com
	ServerAlias www.abimanyu.b18.com
	ServerAlias 192.187.1.2

	Alias "/home" "/var/www/abimanyu.b18/index.php/home"

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Lalu restart service `apache2` menggunakan

```
$ service apache2 restart
```

Untuk mencobanya, kita bisa ke node client lalu menjalankan command

```
$ lynx 192.187.1.2
```

Maka akan muncul tampilan seperti ini.

![192.187.1.2](images/19.png)

## Nomor 20

### Soal

Karena website **www.parikesit.abimanyu.yyy.com** semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.

### Pembahasan

Untuk soal ini, kita bisa ke node Abimanyu lalu di path `/var/www/parikesit.abimanyu.b18`, kita dapat menambahkan file `.htaccess` seperti ini

> /var/www/parikesit.abimanyu.b18/.htaccess

```apache
RewriteEngine On
RewriteBase /
RewriteCond %{REQUEST_URI} !/public/images/abimanyu.png
RewriteRule .*abimanyu.*\.(jpg|jpeg|png|webp)$ /public/images/abimanyu.png [NC,L]
```

Penjelasan:

- **RewriteEngine On**: untuk mengaktifkan modul rewrite
- **RewriteBase /**: rewrite akan diaplikasikan pada URL relatif dari root website
- **RewriteCond %{REQUEST_URI} !/public/images/abimanyu.png**: kondisi ini cek apabila URI tidak mengandung `/public/images/abimanyu.png`
- **RewriteRule ._abimanyu._\.(jpg|jpeg|png|webp)$ /public/images/abimanyu.png [NC,L]**: jika URI terdapat "abimanyu" dan merupakan tipe file gambar seperti jpg, jpeg, png, dan webp maka akan menulis kembali URL menjadi `/public/images/abimanyu.png`

Kemudian, kita bisa edit konfigurasi seperti berikut

> /etc/apache2/sites-available/parikesit.abimanyu.b18.com.conf

```apache
<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/parikesit.abimanyu.b18
	ServerName parikesit.abimanyu.b18.com
	ServerAlias www.parikesit.abimanyu.b18.com

	<Directory /var/www/parikesit.abimanyu.b18>
		Options +FollowSymLinks -Multiviews
		AllowOverride All
	</Directory>

	<Directory /var/www/parikesit.abimanyu.b18/public>
    		Options +Indexes
	</Directory>

	<Directory /var/www/parikesit.abimanyu.b18/secret>
		Deny from all
	</Directory>

	Alias "/js" "/var/www/parikesit.abimanyu.b18/public/js"

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined

	ErrorDocument 403 /error/403.html
	ErrorDocument 404 /error/404.html
</VirtualHost>

```

Lalu mengaktifkan modul `rewrite` dan restart service `apache2` menggunakan

```
$ a2enmod rewrite
$ service apache2 restart
```

Untuk mencobanya, kita bisa ke node client lalu menjalankan command

```
$ lynx www.parikesit.abimanyu.b18.com/ini/adalah/test/testabimanyutest.png
```

Maka akan muncul tampilan seperti ini untuk mendownload file `/public/images/abimanyu.png`

![download abimanyu.png](images/20.png)
