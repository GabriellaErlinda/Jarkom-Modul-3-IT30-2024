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
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/d67322af-529b-4ca4-96d6-90a29986a1e5)


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
2. Lalu pada script Irulan, tambahkan konfigurasi pada nginx sebagai berikut:
```
echo 'nameserver 192.248.3.3' > /etc/resolv.conf

apt-get update
apt-get install apache2-utils -y
apt-get install nginx -y
apt-get install lynx -y

service nginx start

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb_php

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

service nginx restart
```
#### Testing pada client
Jalankan perintah berikut pada client Dmitri dan Paul

ab -n 1000 -c 100 http://www.atreides.com/ 

 

### SOAL 8
> Karena diminta untuk menuliskan peta tercepat menuju spice, buatlah analisis hasil testing dengan 500 request dan 50 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
> a. Nama Algoritma Load Balancer
> b. Report hasil testing pada Apache Benchmark
> c. Grafik request per second untuk masing masing algoritma. 
> d. Analisis

### SOAL 9
> Dengan menggunakan algoritma Least-Connection, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 1000 request dengan 10 request/second, kemudian tambahkan grafiknya pada peta.

### SOAL 10
> Selanjutnya coba tambahkan keamanan dengan konfigurasi autentikasi di LB dengan dengan kombinasi username: “secmart” dan password: “kcksyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/supersecret/

