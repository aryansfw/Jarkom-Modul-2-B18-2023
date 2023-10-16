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

![topologi](images/1.1.jpeg)

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
