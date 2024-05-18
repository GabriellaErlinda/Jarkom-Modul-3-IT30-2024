# Lapres-Jarkom-Modul-3-IT30-2024

## Anggota

| Nama                            | NRP          |
| ------------------------------- | ------------ |
| Gabriella Erlinda Wijaya        | `5027221018` |
| Aras Rizky Ananta               | `5027221053` |

### Topologi Jaringan
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/76bd6c9c-e7f8-42fa-be7e-a3e54cfdafb1)

### SOAL 0
> Planet Caladan sedang mengalami krisis karena kehabisan spice, klan atreides berencana untuk melakukan eksplorasi ke planet arakis dipimpin oleh duke leto mereka meregister domain name atreides.yyy.com untuk worker Laravel mengarah pada Leto Atreides . Namun ternyata tidak hanya klan atreides yang berusaha melakukan eksplorasi, Klan harkonen sudah mendaftarkan domain name harkonen.yyy.com untuk worker PHP (0) mengarah pada Vladimir Harkonen
1. Pada terminal Irulan
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf

# Install necessary packages
apt-get update
apt-get install -y bind9
```
2. Buka dan edit file `/etc/bind/named.conf.local`
```
zone "atreides.it30.com" {
  type master;
  file "/etc/bind/atreides/atreides.it30.com";
};

zone "harkonen.it30.com" {
  type master;
  file "/etc/bind/harkonen/harkonen.it30.com";
};
```
3. Kembali ke terminal, buat direktori untuk domain atreides dan harkonen
```
mkdir /etc/bind/atreides
mkdir /etc/bind/harkonen
```
4. Copy paste file db.local ke direktori domain atrides dan harkonen, lalu ganti namanya sesuai domain yang kita buat
```
cp /etc/bind/db.local /etc/bind/atreides/atreides.it30.com
cp /etc/bind/db.local /etc/bind/harkonen/harkonen.it30.com
```
5. Edit file `/etc/bind/atreides/atreides.it30.com`
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     atreides.it30.com. root.atreides.it30.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      atreides.it30.com.
@       IN      A       192.248.2.2 ; IP Leto Atreides
@       IN      AAAA    ::1
```
6. Edit file `/etc/bind/harkonen/harkonen.it30.com`
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     harkonen.it30.com. root.harkonen.it30.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      harkonen.it30.com.
@       IN      A       192.248.1.2 ; IP Vladimir Harkonen
@       IN      AAAA    ::1
```
7. Restart bind9 dengan `service bind9 restart`

### SOAL 1
> Lakukan konfigurasi sesuai dengan peta yang sudah diberikan
#### Network Configuration
#### Arakis (Router/DHCP Relay)
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.248.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.248.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.248.3.1
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 192.248.4.1
	netmask 255.255.255.0
```
#### Mohiam (DHCP Server)
```
auto eth0
iface eth0 inet static
    address 192.248.3.2
    netmask 255.255.255.0
    gateway 192.248.3.1
```
#### Irulan (DNS Server)
```
auto eth0
iface eth0 inet static
    address 192.248.3.3
    netmask 255.255.255.0
    gateway 192.248.3.1
```
#### Chani (Database Server)
```
auto eth0
iface eth0 inet static
    address 192.248.4.2
    netmask 255.255.255.0
    gateway 192.248.4.1
```
#### Stilgar (Load Balancer)
```
auto eth0
iface eth0 inet static
    address 192.248.4.3
    netmask 255.255.255.0
    gateway 192.248.4.1
```
#### Leto (Laravel Worker)
```
auto eth0
iface eth0 inet static
    address 192.248.2.2
    netmask 255.255.255.0
    gateway 192.248.2.1
```
#### Duncan (Laravel Worker)
```
auto eth0
iface eth0 inet static
    address 192.248.2.3
    netmask 255.255.255.0
    gateway 192.248.2.1
```
#### Jessica (Laravel Worker)
```
auto eth0
iface eth0 inet static
    address 192.248.2.3
    netmask 255.255.255.0
    gateway 192.248.2.1
```
#### Vladimir (PHP Worker)
```
auto eth0
iface eth0 inet static
    address 192.248.1.2
    netmask 255.255.255.0
    gateway 192.248.1.1
```
#### Rabban (PHP Worker)
```
auto eth0
iface eth0 inet static
    address 192.248.1.3
    netmask 255.255.255.0
    gateway 192.248.1.1
```
#### Feyd (PHP Worker)
```
auto eth0
iface eth0 inet static
    address 192.248.1.4
    netmask 255.255.255.0
    gateway 192.248.1.1
```
#### Dmitri (Client)
```
auto eth0
iface eth0 inet dhcp
```

#### Paul (Client)
```
auto eth0
iface eth0 inet dhcp
```

Pada bash script Arakis, tuliskan konfigurasi berikut:
```
apt-get update
apt-get install isc-dhcp-server
apt-get install isc-dhcp-relay -y
service isc-dhcp-relay start

iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.248.0.0/16

echo '
# IP DHCP Server -> IP Mohiam
SERVERS="192.248.3.2"
# Interfaces to listen on
INTERFACES="eth1 eth2 eth3 eth4"
# Options to pass to the DHCP relay
OPTIONS=""
' > /etc/default/isc-dhcp-relay

service isc-dhcp-server start
```
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/b1f6c139-f95a-4e3a-a7a0-442b9c2782ec)
Dengan `192.248.3.2` adalah IP Mohiam yang berperan sebagai DHCP server

### SOAL 2
> Client yang melalui House Harkonen mendapatkan range IP dari [prefix IP].1.14 - [prefix IP].1.28 dan [prefix IP].1.49 - [prefix IP].1.70

1. Setup dhcp server pada Mohiam
```
apt-get update
apt-get install isc-dhcp-server
```
2. Edit file `/etc/default/isc-dhcp-server`
```
INTERFACESv4="eth0"
```
3. Pada `/etc/default/isc-dhcp-server` di Mohiam
```
# Subnet Switch Harkonen
subnet 192.248.1.0 netmask 255.255.255.0 {
    range 192.248.1.14 192.248.1.28;
    range 192.248.1.49 192.248.1.70;
    option routers 192.248.1.1;
    option broadcast-address 192.248.1.255;
    option domain-name-servers 192.248.3.3;
    default-lease-time 300;
    max-lease-time 600;
}
```
4. Stop isc-dhcp-server dengan `service isc-dhcp-server stop` lalu restart dengan `service isc-dhcp-server start`
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/f999ced6-1203-43f3-95eb-c8fb68ccb8f7)


### SOAL 3
> Client yang melalui House Atreides mendapatkan range IP dari [prefix IP].2.15 - [prefix IP].2.25 dan [prefix IP].2 .200 - [prefix IP].2.210
1. Pada `/etc/default/isc-dhcp-server` di Mohiam
```
# Subnet Switch Atreides
subnet 192.248.2.0 netmask 255.255.255.0 {
    range 192.248.2.15 192.248.2.25;
    range 192.248.2.200 192.248.2.210;
    option routers 192.248.2.1;
    option broadcast-address 192.248.2.255;
    option domain-name-servers 192.248.3.3;
    default-lease-time 1200;
    max-lease-time 2400;
}

subnet 192.248.3.0 netmask 255.255.255.0 {}
subnet 192.248.4.0 netmask 255.255.255.0 {}
```
#### Testing pada client
- Dmitri
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/1768f375-ef77-4693-b260-6a8e719f7b6f)
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/d4ff5845-33ad-46f0-95d2-e0318e41f0e6)

- Paul
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/2b65f0d4-ea88-4dfb-a73e-44b5afd268cc)
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/537b14d0-da22-49f1-8433-477303200677)



### SOAL 4
> Client mendapatkan DNS dari Princess Irulan dan dapat terhubung dengan internet melalui DNS tersebut
1. Pada terminal Irulan, edit file `/etc/bind/named.conf.options`
- Uncomment bagian berikut, dan ganti ke IP Arakis
```
forwarders {
    192.168.122.1;
};
```
- Comment bagian berikut:
```
// dnssec-validation auto;
```
- Tambahkan line berikut:
```
allow-query{any;};
```
2. Arahkan koneksi kedua client ke DNS Server Irulan dengan `echo nameserver 192.168.3.3 > /etc/resolv.conf`
3. Testing client
- Dmitri
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/657c7a13-4622-4997-8d99-78bca91bf21a)

- Paul
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/1281c79f-7920-4852-aa6d-8b6bc442c798)


### SOAL 5
> Durasi DHCP server meminjamkan alamat IP kepada Client yang melalui House Harkonen selama 5 menit sedangkan pada client yang melalui House Atreides selama 20 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 87 menit
Di sini saya gabungkan konfigurasi file `/etc/default/isc-dhcp-server` pada Mohiam dengan soal nomor 2 menjadi seperti ini:
```
# Subnet Switch Harkonen
subnet 192.248.1.0 netmask 255.255.255.0 {
    range 192.248.1.14 192.248.1.28;
    range 192.248.1.49 192.248.1.70;
    option routers 192.248.1.1;
    option broadcast-address 192.248.1.255;
    option domain-name-servers 192.248.3.3;
    default-lease-time 300;
    max-lease-time 600;
}

# Subnet Switch Atreides
subnet 192.248.2.0 netmask 255.255.255.0 {
    range 192.248.2.15 192.248.2.25;
    range 192.248.2.200 192.248.2.210;
    option routers 192.248.2.1;
    option broadcast-address 192.248.2.255;
    option domain-name-servers 192.248.3.3;
    default-lease-time 1200;
    max-lease-time 2400;
}

subnet 192.248.3.0 netmask 255.255.255.0 {}
subnet 192.248.4.0 netmask 255.255.255.0 {}
```

### SOAL 6
> Vladimir Harkonen memerintahkan setiap worker(harkonen) PHP, untuk melakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3
Pada setiap bashrc PHP Worker (Vladimir, Rabban, Feyd) tuliskan script berikut
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install nginx -y
apt-get install wget -y
apt-get install unzip -y
apt-get install lynx -y
apt-get install htop -y
apt-get install apache2-utils -y
apt-get install php7.3-fpm php7.3-common php7.3-mysql php7.3-gmp php7.3-curl php7.3-intl php7.3-mbstring php7.3-xmlrpc php7.3-gd php7.3-xml php7.3-cli php7.3-zip -y
apt update
apt install wget

service nginx start
service php7.3-fpm start

wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1lmnXJUbyx1JDt2OA5z_1dEowxozfkn30' -O /var/www/harkonen.com
unzip -o /var/www/harkonen.com -d /var/www/
rm /var/www/harkonen.com
mv /var/www/modul-3 /var/www/harkonen.com

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/harkonen.com
ln -s /etc/nginx/sites-available/harkonen.com /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

echo 'server {
    listen 80;
    server_name _;

    root /var/www/harkonen.com;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.3-fpm.sock; 
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}' > /etc/nginx/sites-available/harkonen.com

service nginx restart
```
Lalu jalankan bash script dengan `source ~/.bashrc`
#### Testing
Setelah semua bashrc dijalankan pada PHP Worker, lakukan testing pada masing-masing PHP Worker dengan `lynx localhost` maka seharusnya akan muncul laman berikut
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/bf690c59-627a-4e08-8932-6d514580e0c2)


### SOAL 7
> Aturlah agar Stilgar dari fremen dapat dapat bekerja sama dengan maksimal, lalu lakukan testing dengan 5000 request dan 150 request/second.
1. Sebelum melakukan testing request, tambahkan konfigurasi Load Balancer Stilgar di bawah ini pada DNS Server Irulan:
```
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     atreides.it30.com. root.atreides.it30.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      atreides.it30.com.
@       IN      A       192.248.4.3 ; IP Load Balancer Stilgar
@       IN      AAAA    ::1' > /etc/bind/atreides/atreides.it30.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     harkonen.it30.com. root.harkonen.it30.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      harkonen.it30.com.
@       IN      A       192.248.4.3 ; IP Load Balancer Stilgar
@       IN      AAAA    ::1' > /etc/bind/harkonen/harkonen.it30.com
```
2. Lalu pada script Stilgar, tambahkan konfigurasi untuk nginx sebagai berikut:
```
echo 'nameserver 192.248.3.3' > /etc/resolv.conf

# Setup
apt-get update
apt-get install apache2-utils -y
apt-get install nginx -y
apt-get install lynx -y

service nginx start

# Copy file nginx default
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb_php

# nginx configuration
echo ' upstream worker {
    server 192.248.1.2;
    server 192.248.1.3;
    server 192.248.1.4;
}

server {
    listen 80;
    server_name harkonen.it30.com www.harkonen.it30.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        proxy_pass http://worker;
    }
} ' > /etc/nginx/sites-available/lb_php

ln -s /etc/nginx/sites-available/lb_php /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

# restart nginx
service nginx restart
```
#### Testing pada client
Jalankan perintah berikut pada client Dmitri dan Paul
```
ab -n 5000 -c 150 http://www.harkonen.it30.com/
```
- Dmitri
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/e3d69c11-2537-450c-b1d8-adc1b58cb305)
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/04601488-0b41-4298-bcdd-1c8a049a06e0)


- Paul
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/8208716a-c3e4-4661-a491-575192b6950e)
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/97b801c4-82e2-4902-bb7c-f6087230e347)


### SOAL 8
> Karena diminta untuk menuliskan peta tercepat menuju spice, buatlah analisis hasil testing dengan 500 request dan 50 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
> a. Nama Algoritma Load Balancer
> b. Report hasil testing pada Apache Benchmark
> c. Grafik request per second untuk masing masing algoritma. 
> d. Analisis

Edit file `/etc/nginx/sites-available/lb_php` dan tambahkan algoritma load balancer
```
upstream worker {
    server 192.248.1.2;
    server 192.248.1.3;
    server 192.248.1.4;
}

upstream roundrobin_worker {
    server 192.248.1.2;
    server 192.248.1.3;
    server 192.248.1.4;
}

upstream leastconn_worker {
    least_conn;
    server 192.248.1.2;
    server 192.248.1.3;
    server 192.248.1.4;
}

upstream weightedrr_worker {
    server 192.248.1.2 weight=3;
    server 192.248.1.3 weight=2;
    server 192.248.1.4 weight=1;
}

upstream iphash_worker {
    ip_hash;
    server 192.248.1.2;
    server 192.248.1.3;
    server 192.248.1.4;
}

upstream hash_worker {
    hash $request_uri consistent;
    server 192.248.1.2;
    server 192.248.1.3;
    server 192.248.1.4;
}

server {
    listen 80;
    server_name harkonen.it30.com www.harkonen.it30.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        proxy_pass http://worker;
        auth_basic "Restricted Content";
        auth_basic_user_file /etc/nginx/supersecret/htpasswd;
    }
    location /roundrobin {
        proxy_pass http://roundrobin_worker;
    }
    location /leastconn {
        proxy_pass http://leastconn_worker;
    }
    location /weightedrr {
        proxy_pass http://weightedrr_worker;
    }
    location /iphash {
        proxy_pass http://iphash_worker;
    }
    location /hash {
        proxy_pass http://hash_worker;
    }
}
```
#### Testing client
- Round Robin
![Screenshot 2024-05-18 132657](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/32693765-8a8d-4f2b-9f9c-6ff8c1d364a0)
![Screenshot 2024-05-18 132731](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/51b41f4e-bce9-4ef7-b649-d3217a56d659)

- Least Connection
![Screenshot 2024-05-18 132821](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/10a6af63-ed42-4bf0-aa97-d05833ee9a98)
![Screenshot 2024-05-18 132842](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/70b7b6c4-22cb-4942-9bba-3e4e8b580913)

- Weighted Round Robin
![Screenshot 2024-05-18 132945](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/d0deb1ee-00cd-40cd-b767-8b5d96a1d9e7)
![Screenshot 2024-05-18 133019](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/22d6fa35-a57b-44dc-9a54-300afa4c1feb)

- IP Hash
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/5f95bca8-dfde-4972-bc27-2c915fd645bc)
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/a2bd3ebd-692f-45f4-906d-38ff82aaa6d0)

- Generic Hash
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/70c75865-9f47-43c7-8dd7-c1bf289a2f07)
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/87b78134-8fa0-42f8-bfcf-1b9c0af5da72)

#### Analisis
Grafik Perbandingan Request Per Second
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/929cd4b3-2771-481d-8713-218d0e4a0db3)

Grafik Perbandingan Persentase Request yang Dilayani dalam Jangka Waktu Tertentu
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/8ab87c31-c9eb-47c4-be41-3844984ffba7)


### SOAL 9
> Dengan menggunakan algoritma Least-Connection, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 1000 request dengan 10 request/second, kemudian tambahkan grafiknya pada peta.

### SOAL 10
> Selanjutnya coba tambahkan keamanan dengan konfigurasi autentikasi di LB dengan dengan kombinasi username: “secmart” dan password: “kcksyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/supersecret/

