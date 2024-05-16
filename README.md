# Lapres-Jarkom-Modul-3-IT30-2024

## Anggota

| Nama                            | NRP          |
| ------------------------------- | ------------ |
| Gabriella Erlinda Wijaya        | `5027221018` |
| Aras Rizky Ananta               | `5027221053` |

### Topologi Jaringan
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-3-IT30-2024/assets/128443451/76bd6c9c-e7f8-42fa-be7e-a3e54cfdafb1)

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

### SOAL 2
> Client yang melalui House Harkonen mendapatkan range IP dari [prefix IP].1.14 - [prefix IP].1.28 dan [prefix IP].1.49 - [prefix IP].1.70
1. Pada /etc/default/isc-dhcp-server di Mohiam
```
subnet 192.248.1.0 netmask 255.255.255.0 {
    range 192.248.1.14 192.248.1.28;
    range 192.248.1.49 192.248.1.70;
    option routers 192.248.1.1;
    option domain-name-servers 192.248.3.3;
};
```

### SOAL 3
> Client yang melalui House Atreides mendapatkan range IP dari [prefix IP].2.15 - [prefix IP].2.25 dan [prefix IP].2 .200 - [prefix IP].2.210
1. Pada /etc/default/isc-dhcp-server di Mohiam
```
subnet 192.248.2.0 netmask 255.255.255.0 {
    range 192.248.2.15 192.248.2.25;
    range 192.248.2.200 192.248.2.210;
    option routers 192.248.2.1;
    option domain-name-servers 192.248.3.3;
    default-lease-time 1200;
    max-lease-time 5220;
};
```

### SOAL 4
> Client mendapatkan DNS dari Princess Irulan dan dapat terhubung dengan internet melalui DNS tersebut

### SOAL 5
> Durasi DHCP server meminjamkan alamat IP kepada Client yang melalui House Harkonen selama 5 menit sedangkan pada client yang melalui House Atreides selama 20 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 87 menit

### SOAL 6
> Vladimir Harkonen memerintahkan setiap worker(harkonen) PHP, untuk melakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3

### SOAL 7
> Aturlah agar Stilgar dari fremen dapat dapat bekerja sama dengan maksimal, lalu lakukan testing dengan 5000 request dan 150 request/second.

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

