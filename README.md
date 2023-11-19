# Jarkom-Modul-3-E29-2023
## Anggota
Kelompok E29
| Nama | NRP |
| --- | --- |
| Sony Hermawan | 5025211226 |
| Sekar Ambar Arum | 5025211041 |


# Laporan Resmi
# Laporan Resmi
* [Topologi](#Topologi)
* [File GNS3](#File-GNS3)
* [Config](#Config)
  * [Aura (DHCP Relay)](#Aura-(DHCP-Relay))
  * [Himmel (DHCP Server)](#Himmel-(DHCP-Server))
* [Soal (0)](#Soal-0)

## Topologi
![Cuplikan layar 2023-11-18 230254](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/90591077/a8a2371e-71ac-4f97-a3a0-3557f3877046)

## File GNS3
https://drive.google.com/drive/folders/1H-8TAG1jnOSrgBXoLuEiRrDLTi_M31MO?usp=sharing 

## Config

### - Aura (DHCP Relay)
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.51.1.0
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.51.2.0
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.51.3.0
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 10.51.4.0
	netmask 255.255.255.0
```

### - Himmel (DHCP Server)
```
auto eth0
iface eth0 inet static
	address 10.51.1.1
	netmask 255.255.255.0
	gateway 10.51.1.0
```

### - Heiter (DNS Server)
```
auto eth0
iface eth0 inet static
	address 10.51.1.2
	netmask 255.255.255.0
	gateway 10.51.1.0
```

### - Denken (Database Server)
```
auto eth0
iface eth0 inet static
	address 10.51.2.1
	netmask 255.255.255.0
	gateway 10.51.2.0
```

### - Eisen (Load Balancer)
```
auto eth0
iface eth0 inet static
	address 10.51.2.2
	netmask 255.255.255.0
	gateway 10.51.2.0
```

### - Lawine (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 10.51.3.2
	netmask 255.255.255.0
	gateway 10.51.3.0
```

### - Linie (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 10.51.3.2
	netmask 255.255.255.0
	gateway 10.51.3.0
```

### - Lugner (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 10.51.3.1
	netmask 255.255.255.0
	gateway 10.51.3.0
```

### - Frieren (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 10.51.4.1
	netmask 255.255.255.0
	gateway 10.51.4.0
```

### - Flamme (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 10.51.4.2
	netmask 255.255.255.0
	gateway 10.51.4.0
```

### - Fern (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 10.51.4.3
	netmask 255.255.255.0
	gateway 10.51.4.0
```

### -  Revolte, Richter, Sein, Stark (Client)
```
auto eth0
iface eth0 inet dhcp
```

## Setup
### Script
#### Himmel DHCP Server
```
apt-get update
apt-get install isc-dhcp-server
dhcpd --version

echo '
# Defaults for isc-dhcp-server (sourced by /etc/init.d/isc-dhcp-server)

# Path to dhcpd'\''s config file (default: /etc/dhcp/dhcpd.conf).
#DHCPDv4_CONF=/etc/dhcp/dhcpd.conf
#DHCPDv6_CONF=/etc/dhcp/dhcpd6.conf

# Path to dhcpd'\''s PID file (default: /var/run/dhcpd.pid).
#DHCPDv4_PID=/var/run/dhcpd.pid
#DHCPDv6_PID=/var/run/dhcpd6.pid

# Additional options to start dhcpd with.
#       Don'\''t use options -cf or -pf here; use DHCPD_CONF/ DHCPD_PID instead
#OPTIONS=""

# On what interfaces should the DHCP server (dhcpd) serve DHCP requests?
#       Separate multiple interfaces with spaces, e.g. "eth0 eth1".
INTERFACES="eth0 eth1 eth2"' >/etc/default/isc-dhcp-server

echo 'subnet 10.51.1.0 netmask 255.255.255.0 {
}

subnet 10.51.2.0 netmask 255.255.255.0 {
}

#Soal 1-5
subnet 10.51.3.0 netmask 255.255.255.0 {
        range 10.51.3.16 10.51.3.32;
        range 10.51.3.64 10.51.3.80;
        option routers 10.51.3.0;
        option broadcast-address 10.51.3.255;
        option domain-name-servers 10.51.1.2;
        default-lease-time 180;
        max-lease-time 5760;

}

subnet 10.51.4.0 netmask 255.255.255.0 {
        range 10.51.4.12 10.51.4.20;
        range 10.51.4.160 10.51.4.168;
        option routers 10.51.4.0;
        option broadcast-address 10.51.4.255;
        option domain-name-servers 10.51.1.2;
        default-lease-time 720;
        max-lease-time 5760;
}' >/etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

#### Aura DHCP Relay
```
echo -e "apt-get update\napt-get install isc-dhcp-relay -y\n
service isc-dhcp-relay start" >>/root/.bashrc

echo '
# Defaults for isc-dhcp-relay initscript
# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts

#
# This is a POSIX shell fragment
#

# What servers should the DHCP relay forward requests to?
SERVERS="10.51.1.1"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3 eth4"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""' >/etc/default/isc-dhcp-relay

echo 'net.ipv4.ip_forward=1' >/etc/sysctl.conf
```
## Soal 0
Setelah mengalahkan Demon King, perjalanan berlanjut. Kali ini, kalian diminta untuk melakukan register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP (0) mengarah pada worker yang memiliki IP [prefix IP].x.1.
### Script
#### Heiter DNS Server
Melakukan register domain untuk ``riegel.canyon.e29.com`` dan ``granz.channel.e29.com`` pada DNS Server
```
apt-get update
apt-get install bind9 -y

echo 'zone "riegel.canyon.e29.com" {
        type master;
        file "/etc/bind/jarkom/riegel.canyon.e29.com";
};

zone "granz.channel.e29.com" {
        type master;
        file "/etc/bind/jarkom/granz.channel.e29.com";
};' >/etc/bind/named.conf.local

mkdir /etc/bind/jarkom

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.e29.com. root.riegel.canyon.e29.com. (
                        2023111301      ; Serial
                        604800          ; Refresh
                        86400           ; Retry
                        2419200         ; Expire
                        604800 )        ; Negative Cache TTL
;
@               IN      NS      riegel.canyon.e29.com.
@               IN      A       10.51.2.2 ; IP Load Balancer' >/etc/bind/jarkom/riegel.canyon.e29.com

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.e29.com.  root.granz.channel.e29.com.  (
                        2023111301      ; Serial
                        604800          ; Refresh
                        86400           ; Retry
                        2419200         ; Expire
                        604800 )        ; Negative Cache TTL
;
@               IN      NS      granz.channel.e29.com.
@               IN      A       10.51.2.2 ; IP Load Balancer' >/etc/bind/jarkom/granz.channel.e29.com

echo 'options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1;
        };

        // dnssec-validation auto;
        allow-query{any;};
        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
}; ' >/etc/bind/named.conf.options

service bind9 restart
```
### Hasil
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/43c525a7-6f9d-419b-bf35-79c9d6d49a00)

## Soal 1
Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.
### Script
Bisa dilihat pada [Topologi](#Topologi) dan untuk contoh disini kami mengambil 1 konfigurasi IP Static dan Dynamic
#### Sein
``` Dynamic
auto eth0
iface eth0 inet dhcp
```
#### Frieren
``` Static
auto eth0
iface eth0 inet static
	address 10.51.4.1
	netmask 255.255.255.0
	gateway 10.51.4.0
```
### Hasil
Kita akan mengetes jika konfigurasi berhasil dengan melakukan ping pada ``google.com``
#### Sein
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/30dd0fd2-0259-455b-9c9b-bfe0b1f5e438)
#### Frieren
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/b5f986e2-df33-4a57-903c-079042823128)

## Soal 2
Semua CLIENT harus menggunakan konfigurasi dari DHCP Server.
Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80
### Script
#### Heiter
Bisa dilihat pada soal 0 kami sudah membuat konfigurasi sebagai berikut dengan ``range`` untuk membatasi IP
```
subnet 10.51.3.0 netmask 255.255.255.0 {
        range 10.51.3.16 10.51.3.32;
        range 10.51.3.64 10.51.3.80;
	...
}
```
### Hasil
#### Revolte
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/b8e24264-7fb8-4f41-89ae-032e24552c16)

## Soal 3
Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168
### Script
#### Heiter
Sama seperti soal sebelumnya menggunakan ``range``
```
subnet 10.51.4.0 netmask 255.255.255.0 {
        range 10.51.4.12 10.51.4.20;
        range 10.51.4.160 10.51.4.168;
	...
}
```
### Hasil
#### Sein
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/f8a7c82c-8b15-41e6-baad-c0518da75074)

## Soal 4
Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut
### Script
#### Heiter
Pada setup Heiter soal ini juga sudah termasuk dengan menggunakan ``option broadcast-address`` dan ``option domain-name-servers``
```
subnet 10.51.3.0 netmask 255.255.255.0 {
	...
        option broadcast-address 10.51.3.255;
        option domain-name-servers 10.51.1.2;
	...

}

subnet 10.51.4.0 netmask 255.255.255.0 {
      	...
        option broadcast-address 10.51.4.255;
        option domain-name-servers 10.51.1.2;
	...
}
```
Karena DHCP Relay sudah dilakukan oleh Aura diatas maka dengan demikian DNS dapat terhubung ke internet
### Hasil
#### Sein
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/30dd0fd2-0259-455b-9c9b-bfe0b1f5e438)

## Soal 5
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit
### Script
Soal ini juga termasuk pada setup DHCP Heiter diatas yaitu pada bagian ``default-lease-time`` dan ``max-lease-time`` 
```
subnet 10.51.3.0 netmask 255.255.255.0 {
	...
        default-lease-time 180;
        max-lease-time 5760;

}

subnet 10.51.4.0 netmask 255.255.255.0 {
	...
        default-lease-time 720;
        max-lease-time 5760;
}
```
### Hasil
### Revolte - Switch3
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/b8e24264-7fb8-4f41-89ae-032e24552c16)
### Sein - Switch4
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/f8a7c82c-8b15-41e6-baad-c0518da75074)

## Soal 6
Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3.
### Script
#### Eisen - Load Balancer
Melakukan setup ``nginx`` yang akan digunakan sebagai load balancer
```
apt-get update
apt-get install bind9 nginx -y
service nginx start
service nginx status

echo '
 upstream myweb  {
        server 10.51.3.1; #IP Lugner
        server 10.51.3.2; #IP Linie
        server 10.51.3.3; #IP Lawine
 }

 server {
        listen 80;
        server_name granz.channel.e29.com;

        location / {
        proxy_pass http://myweb;
        }
 }' >/etc/nginx/sites-available/lb-jarkom

ln -s /etc/nginx/sites-available/lb-jarkom /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service nginx restart
nginx -t
```
#### Lawine, Linie, Lugner - PHP Worker
Setup nginx pada worker, dan melakukan download link drive untuk template website
```
apt-get update
apt-get install wget -y
apt-get install unzip -y
apt-get install nginx -y
apt-get install php7.3 -y
apt-get install php7.3-fpm -y

apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2
curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg
sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
apt-get install php8.0-mbstring php8.0-xml php8.0-cli php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y

mkdir -p /var/www/granz.channel.e29

wget --no-check-certificate 'https://drive.google.com/uc?export=download&id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1' -O /var/www/granz.channel.e29.zip
unzip /var/www/granz.channel.e29.zip -d /var/www/granz.channel.e29
mv /var/www/granz.channel.e29/modul-3 /var/www/granz.channel.e29
rm -rf /var/www/granz.channel.e29.zip

echo nameserver 10.51.1.2 >/etc/resolv.conf

echo '
server {

        listen 80;

        root /var/www/granz.channel.e29;

        index index.php index.html index.htm;
        server_name _;

        location / {
                        try_files $uri $uri/ /index.php?$query_string;
        }

        # pass PHP scripts to FastCGI server
        location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
        }

location ~ /\.ht {
                        deny all;
        }

        error_log /var/log/nginx/jarkom_error.log;
        access_log /var/log/nginx/jarkom_access.log;
 }' >/etc/nginx/sites-available/granz.channel.e29

echo '<!DOCTYPE html>
<html>
<head>
    <title>Granz Channel Map</title>
    <link rel="stylesheet" type="text/css" href="css/styles.css">
</head>
<body>
    <div class="container">
        <h1>Welcome to Granz Channel</h1>
        <p><?php
            $hostname = gethostname();
            echo "Request ini dihandle oleh: $hostname<br>"; ?> </p>
        <p>Enter your name to validate:</p>
        <form method="POST" action="index.php">
            <input type="text" name="name" id="nameInput">
            <button type="submit" id="submitButton">Submit</button>
        </form>
        <p id="greeting"><?php
            if(isset($_POST['name'])) {
                $name = $_POST['name'];
                echo "Hello, $name!";
            }
        ?></p>
    </div>

    <script src="js/script.js"></script>
</body>
</html>' >/var/www/granz.channel.e29/index.php

ln -s /etc/nginx/sites-available/granz.channel.e29 /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service php7.3-fpm start
service php7.3-fpm restart
service nginx restart
nginx -t
```
#### Stark
```
echo 'nameserver 10.51.1.2 # IP DNS Server(Heiter)' >/etc/resolv.conf
apt-get update
apt-get install lynx -y
lynx granz.channel.e29.com
```
### Hasil
#### Stark
Dengan melakukan lynx pada ``granz.channel.e29.com`` kita dapat mengakses web PHP
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/57c475de-6c79-4b26-9089-504270eaed81)

## Soal 7
Kepala suku dari Bredt Region memberikan resource server sebagai berikut:
Lawine, 4GB, 2vCPU, dan 80 GB SSD.
Linie, 2GB, 2vCPU, dan 50 GB SSD.
Lugner 1GB, 1vCPU, dan 25 GB SSD.
aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second.
### Script
Karena sudah melakukan setup load balancer pada soal 6, pada tahap ini hanya lakukan install ``apache workbench`` yang akan digunakan untuk melakukan benchmark
#### Sein
```
apt-get update && apt-get install apache2-utils -y
apt-get install apache2-utils -y
ab -V
ab -n 1000 -c 100 granz.channel.e29.com/
```
### Hasil
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/48fb8f3e-c51e-473c-8395-dc52db6bdd73)

## Soal 8
Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
- a) Nama Algoritma Load Balancer
- b) Report hasil testing pada Apache Benchmark
- c) Grafik request per second untuk masing masing algoritma. 
- d) Analisis
### Script
#### Eisen - Least Connection
Disini akan hanya 1 script yang dibahas secara lengkap yaitu ``Least Connection``
```
echo '
 upstream myweb  {
        least_conn;
        server 10.51.3.1; #IP Lugner
        server 10.51.3.2; #IP Linie
        server 10.51.3.3; #IP Lawine
 }

 server {
        listen 80;
        server_name granz.channel.e29.com;

         location / {
                proxy_pass http://myweb;
                proxy_set_header    X-Real-IP $remote_addr;
                proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header    Host $http_host;
        }

error_log /var/log/nginx/lb_error.log;
access_log /var/log/nginx/lb_access.log;
 }' >/etc/nginx/sites-available/lb-jarkom

service nginx restart
nginx -t
```
#### Eisen - IP Hash
```
 upstream myweb  {
	ip_hash;
        server 10.51.3.1 weight=4; #IP Lugner
        server 10.51.3.2 weight=2; #IP Linie
        server 10.51.3.3 weight=1; #IP Lawine
 }
```
#### Eisen - Generic Hash
```
upstream myweb  {
   	hash $request_uri consistent;
        server 10.51.3.1; #IP Lugner
        server 10.51.3.2; #IP Linie
        server 10.51.3.3; #IP Lawine
}
```
#### Sein
```
ab -n 200 -c 10 granz.channel.e29.com/
```
### Hasil
Hasil lengkap ada pada ``grimoire`` yang diupload di github ini
#### Round Robin
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/a2135f71-57cf-4a3c-9e08-79a05126c4ad)
#### Weighted Round Robin
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/4baa9871-f88e-4996-a748-f9def936af9e)
#### Least Connection
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/29e3a18c-428e-4648-8341-ad31a7a623c9)
#### IP Hash
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/e1c85815-a0cb-48a4-ae8d-517a7f049b17)
#### Generic Hash
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/7ebd22bf-2384-4088-9d55-c2e2046c6b8f)
#### Grafik
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/964473e1-fedd-4027-a8c3-7bd7dcf4000c)

## Soal 9
Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire.
### Script
Melakukan comment pada worker yang ingin dimatikan pada Eisen contoh 2 ``worker``
#### Eisen
```
echo '
 upstream myweb  {
        server 10.51.3.1; #IP Lugner
        server 10.51.3.2; #IP Linie
        # server 10.51.3.3; #IP Lawine
 }

 server {
        listen 80;
        server_name granz.channel.e29.com;

        location / {
        proxy_pass http://myweb;
        }
 }' >/etc/nginx/sites-available/lb-jarkom

service nginx restart
nginx -t

```
#### Sein
```
ab -n 100 -c 10 granz.channel.e29.com/
```
### Hasil
Hasil lengkap ada pada ``griomoire``
#### Grafik
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/dec37e88-aed5-4782-bd70-dadb471b5afd)

## Soal 10
Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/
### Script
#### Eisen
Bagian yang harus di highlight pada soal ini adalah ``htpasswd`` dengan username ``netics`` dan password ``ajke29``, kemudian pada bagian ``auth_basic`` dan ``auth_basic_user_file``
```
apt-get update
apt-get install apache2-utils -y
mkdir /etc/nginx/rahasisakita
htpasswd -b -c /etc/nginx/rahasisakita/.htpasswd netics ajke29

echo '
 upstream myweb  {
        server 10.51.3.1; #IP Lugner
        server 10.51.3.2; #IP Linie
        server 10.51.3.3; #IP Lawine
 }

 server {
        listen 80;
        server_name granz.channel.e29.com;

        location / {
        proxy_pass http://myweb;
        proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header    Host $http_host;

            auth_basic "Administrator'\''s Area";
            auth_basic_user_file  /etc/nginx/rahasisakita/.htpasswd;
        }

        location ~ /\.ht {
            deny all;
        }

 }' >/etc/nginx/sites-available/lb-jarkom

service nginx restart
nginx -t
```
#### Stark
Percobaan pertama tanpa username dan password, percobaan kedua menggunakan username dan password
```
lynx http://granz.channel.e29.com/
lynx -auth=netics:ajke29 http://granz.channel.e29.com/
```
### Hasil
Disini hanya foto percobaan gagal karena foto percobaan berhasil sudah ada pada soal 6
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/a68b4506-4534-40da-adf8-15084e547f68)

## Soal 11
Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id.
### Script
Pada script ini karena sudah banyak dibahas diatas akan hanya ditunjukan bagian yang penting yaitu ``location``
#### Eisen
```
echo '
 upstream myweb  {
        ...
 }

 server {
       	...

        location /its {
            proxy_pass https://www.its.ac.id;
        }

 }' >/etc/nginx/sites-available/lb-jarkom

service nginx restart
nginx -t
```
#### Sein
```
lynx http://granz.channel.e29.com/its
```
### Hasil
Dengan menggunakan proxy pass jika mendeteksi ``/its`` akan diarahkan ke web https://www.its.ac.id.
#### Sein
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/67aaca11-f071-4959-94bd-dd7a9a7ecf19)

## Soal 12
Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168
### Script
#### Eisen
Sama seperti sebelumnya karena script sudah dibahas maka akan ditunjukan bagian yang penting 
```
echo '
 upstream myweb  {
	...
 }

 server {
        listen 80;
        server_name granz.channel.e29.com;

        location / {
            allow 10.51.3.69;
            allow 10.51.3.70;
            allow 10.51.4.167;
            allow 10.51.4.168;
            deny all;

        ...

 }' >/etc/nginx/sites-available/lb-jarkom

service nginx restart
nginx -t
```
#### Stark
Stark mengganti konfigurasi menggunakan ip static untuk mendapatkan prefix yang diizinkan
```
echo '
    auto eth0
    iface eth0 inet static
	address 10.51.4.167
	netmask 255.255.255.0
	gateway 10.51.4.0
' >/etc/network/interfaces
```
### Hasil
Akan ada 2 contoh yaitu yang gagal dan berhasil
#### Sein
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/3be1d6a1-97af-4c73-859e-fc7c6bdf7a9d)
#### Stark
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/ff4b7cd2-c36c-454c-8f91-22c28362a6a3)

## Soal 13
Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern.
### Script
#### Denken
Denken akan melakukan setup sql server
```
apt-get update
apt-get install mariadb-server -y
service mysql start
sed -i 's/127.0.0.1/0.0.0.0/g' /etc/mysql/mariadb.conf.d/50-server.cnf

echo '#
# This group is read both by the client and the server
# use it for options that affect everything
[client-server]

# Import all .cnf files from configuration directory
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mariadb.conf.d/

# Options affecting the MySQL server (mysqld)
[mysqld]
skip-networking=0
skip-bind-address
' >/etc/mysql/my.cnf

service mysql restart

mysql -u root --password= <~/setup_script.sql
```
#### Frieren, Flamme, Fern - Laravel Worker
```
echo 'nameserver 10.51.1.2' >/etc/resolv.conf
apt-get update
apt-get install mariadb-client -y

apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2
curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg
sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
apt-get update
apt-get install php8.0-mbstring php8.0-xml php8.0-cli php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y
apt-get install nginx -y

service nginx start
service php8.0-fpm start
```
#### Frieren - Test
```
mariadb --host=10.51.2.1 --port=3306 --user=kelompoke29 --password=passworde29 dbkelompoke29 -e "SHOW DATABASES;"
```
### Hasil
#### Frieren
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/2e0712bf-db46-49f8-aed7-d0e2e4abb78c)

## Soal 14
Frieren, Flamme, dan Fern memiliki Riegel Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer
### Script
#### Frieren, Flamme, dan Fern - Worker
```
wget https://getcomposer.org/download/2.0.13/composer.phar
chmod +x composer.phar
mv composer.phar /usr/local/bin/composer

apt-get install git -y
cd /var/www && git clone https://github.com/martuafernando/laravel-praktikum-jarkom
cd /var/www/laravel-praktikum-jarkom && composer update
cd /var/www/laravel-praktikum-jarkom && composer install

cd /var/www/laravel-praktikum-jarkom && cp .env.example .env
echo 'APP_NAME=Laravel
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://localhost

LOG_CHANNEL=stack
LOG_DEPRECATIONS_CHANNEL=null
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=10.51.2.1
DB_PORT=3306
DB_DATABASE=dbkelompoke29
DB_USERNAME=kelompoke29
DB_PASSWORD=passworde29

BROADCAST_DRIVER=log
CACHE_DRIVER=file
FILESYSTEM_DISK=local
QUEUE_CONNECTION=sync
SESSION_DRIVER=file
SESSION_LIFETIME=120

MEMCACHED_HOST=127.0.0.1

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_MAILER=smtp
MAIL_HOST=mailpit
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="hello@example.com"
MAIL_FROM_NAME="${APP_NAME}"

AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=
AWS_USE_PATH_STYLE_ENDPOINT=false

PUSHER_APP_ID=
PUSHER_APP_KEY=
PUSHER_APP_SECRET=
PUSHER_HOST=
PUSHER_PORT=443
PUSHER_SCHEME=https
PUSHER_APP_CLUSTER=mt1

VITE_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
VITE_PUSHER_HOST="${PUSHER_HOST}"
VITE_PUSHER_PORT="${PUSHER_PORT}"
VITE_PUSHER_SCHEME="${PUSHER_SCHEME}"
VITE_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"' >/var/www/laravel-praktikum-jarkom/.env
cd /var/www/laravel-praktikum-jarkom 
php artisan key:generate
php artisan config:cache
php artisan migrate
php artisan db:seed
php artisan storage:link
php artisan jwt:secret
php artisan config:clear
chown -R www-data.www-data /var/www/laravel-praktikum-jarkom/storage
```
#### Flamme
Lakukan setup lagi pada worker kali ini sesuai dengan port pada contoh ini Flamme ``8002``
```
apt-get update
apt-get install nginx
service nginx start
service nginx status

echo 'server {
    listen 8002;

    root /var/www/laravel-praktikum-jarkom/public;

    index index.php index.html index.htm;
    server_name _;

    location / {
            try_files $uri $uri/ /index.php?$query_string;
    }

    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
      include snippets/fastcgi-php.conf;
      fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
    }

    location ~ /\.ht {
            deny all;
    }

    error_log /var/log/nginx/implementasi_error.log;
    access_log /var/log/nginx/implementasi_access.log;
}' >/etc/nginx/sites-available/laravel-worker

ln -s /etc/nginx/sites-available/laravel-worker /etc/nginx/sites-enabled

service nginx restart
nginx -t
```
#### Flamme Test
Lakukan test localhost + port pada contoh ini ``8002``
```
apt-get install lynx -y
lynx localhost:8002
```
### Hasil
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/81e4310d-833e-48d1-b8c4-d0c802a10ab7)

## Soal 15
Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.
POST /auth/register 
### Script
#### Sein
Kita akan lakukan test API POST /auth/register pada ``riegel.canyon.e29.com``, disini menggunakan file berisi json untuk mengirim username dan password yang akan diregister pada auth tersebut
```
apt-get update && apt-get install apache2-utils -y
apt-get install apache2-utils -y
ab -V
echo '
{
  "username": "kelompoke29",
  "password": "passworde29"
}' >soal15.json

ab -n 100 -c 10 -p soal15.json -T application/json http://riegel.canyon.e29.com/api/auth/register
```
### Hasil
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/bfdb3c74-b3ad-4908-aa82-710601e7c6fe)

## Soal 16
POST /auth/login
### Script
#### Sein
Kita akan lakukan test API POST /auth/login pada ``riegel.canyon.e29.com``,  disini menggunakan file berisi json untuk mengirim username dan password yang akan login pada auth tersebut
```
echo '
{
  "username": "kelompoke29",
  "password": "passworde29"
}' >soal16.json

ab -n 100 -c 10 -p soal16.json -T application/json http://riegel.canyon.e29.com/api/auth/login
```
### Hasil
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/539e0f0b-8410-4956-9ba7-5bcccb403ff8)

## Soal 17
GET /me 
### Script
#### Sein
Mengambil token pada API login agar berhasil melakukan auth pada /api/me
```
apt-get install jq
token=$(curl -X POST -H "Content-Type: application/json" -d @soal16.json http://10.51.4.2:8002/api/auth/login | jq -r '.token')
ab -n 100 -c 10 -H "Authorization: Bearer $token" http://10.51.4.2:8002/api/me
```
### Hasil
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/06443817-91d7-4dba-a357-be1c243fed98)

## Soal 18
Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Riegel Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern.
### Script
#### Eisen
```
echo 'upstream myweb2 {
    server 10.51.4.1:8001;
    server 10.51.4.2:8002;
    server 10.51.4.3:8003;
}

server {
    listen 80;
    server_name riegel.canyon.e29.com;

    location / {
        proxy_pass http://myweb2;
    }
} 
' >/etc/nginx/sites-available/laravel-worker

ln -s /etc/nginx/sites-available/laravel-worker /etc/nginx/sites-enabled/laravel-worker

service nginx restart
nginx -t
```
#### Sein
```
ab -n 100 -c 10 -p soal16.json -T application/json http://riegel.canyon.e29.com/api/auth/login
```
### Hasil
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/983b212c-85a0-4562-b091-8d81374f2cb7)

## Soal 19
Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan 
- pm.max_children
- pm.start_servers
- pm.min_spare_servers
- pm.max_spare_servers. sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire.
### Script
#### Frieren, Flamme, Fern - Script 1
Untuk script 1 akan lengkap dan script lainnya akan dipotong
```
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 5
pm.start_servers = 3
pm.min_spare_servers = 1
pm.max_spare_servers = 5' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart
```
#### Frieren, Flamme, Fern - Script 2
```
pm = dynamic
pm.max_children = 15
pm.start_servers = 5
pm.min_spare_servers = 5
pm.max_spare_servers = 10
```
#### Frieren, Flamme, Fern - Script 3
```
pm = dynamic
pm.max_children = 50
pm.start_servers = 25
pm.min_spare_servers = 10
pm.max_spare_servers = 25
```
#### Sein - Test
```
ab -n 100 -c 10 -p soal16.json -T application/json http://riegel.canyon.e29.com/api/auth/login
```
### Hasil
#### Script 1
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/b450d211-773b-46b1-8aa8-c4ad8f7f3555)
#### Script 2
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/60cfa3f9-45bb-4d98-830c-9de54a69010b)
#### Script 3
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/3758c9d0-6b9e-425b-a8bf-934a802aac45)
Selengkapnya ada pada griomoire

## Soal 20
Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second.
### Script
#### Eisen
```
echo 'upstream mywe2b {
    least_conn;
    server 10.51.4.1:8001;
    server 10.51.4.2:8002;
    server 10.51.4.3:8003;
}

server {
    listen 80;
    server_name riegel.canyon.e29.com;

    location / {
        proxy_pass http://myweb2;
    }
} 
' >/etc/nginx/sites-available/laravel-worker

ln -s /etc/nginx/sites-available/laravel-worker /etc/nginx/sites-enabled/laravel-worker

service nginx restart
nginx -t
```
#### Sein - Test
```
ab -n 100 -c 10 -p soal16.json -T application/json http://riegel.canyon.e29.com/api/auth/login
```
### Hasil
![image](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/48209612/53c283da-260c-46aa-9b57-fb18a9edfdca)
