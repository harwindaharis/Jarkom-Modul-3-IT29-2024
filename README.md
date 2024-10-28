# Jarkom-Modul-3-IT29-2024

## Anggota Kelompok
| Nama | NRP |
|---------|---------|
| Harwinda | 5027231079   |
| Muhammad Syahmi Ash Shidqi | 5027231085   |


## Topologi
<img width="626" alt="image" src="https://github.com/user-attachments/assets/cbc98ce8-eb80-46ea-b7d8-bb02ad184919">



## Prefix
| IT29 | 10.78 |
|---------|---------|

## Network Configuration (Config)
### Paradis (Router / DHCP Relay)
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.78.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.78.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.78.3.1
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 10.78.4.1
	netmask 255.255.255.0
```

### Tybur (DHCP Server)
```
auto eth0
iface eth0 inet static
	address 10.78.4.3
	netmask 255.255.255.0
	gateway 10.78.4.1
  up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Fritz (DNS Server)
```
auto eth0
iface eth0 inet static
	address 10.78.4.2
	netmask 255.255.255.0
	gateway 10.78.4.1
  up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Warhammer (Database Server)
```
auto eth0
iface eth0 inet static
	address 10.78.3.4
	netmask 255.255.255.0
	gateway 10.78.3.1
  up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Beast (Load Balancer Laravel)
auto eth0
```
iface eth0 inet static
	address 10.78.3.2
	netmask 255.255.255.0
	gateway 10.78.3.1
  up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Colossal (Load Balancer PHP)
```
auto eth0
iface eth0 inet static
	address 10.78.3.3
	netmask 255.255.255.0
	gateway 10.78.3.1
  up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Annie (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 10.78.1.2
	netmask 255.255.255.0
	gateway 10.78.1.1
  up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Bertholdt (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 10.78.1.3
	netmask 255.255.255.0
	gateway 10.78.1.1
  up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Reiner (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 10.78.1.4
	netmask 255.255.255.0
	gateway 10.78.1.1
  up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Armin (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 10.78.2.2
	netmask 255.255.255.0
	gateway 10.78.2.1
  up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Eren (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 10.78.2.3
	netmask 255.255.255.0
	gateway 10.78.2.1
  up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Mikasa (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 10.78.2.4
	netmask 255.255.255.0
	gateway 10.78.2.1
  up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

### Zeke & Erwin (Client)
```
auto eth0
iface eth0 inet dhcp
```

## Soal 0

Pulau Paradis telah menjadi tempat yang damai selama 1000 tahun, namun kedamaian tersebut tidak bertahan selamanya. Perang antara kaum Marley dan Eldia telah mencapai puncak. Kaum Marley yang dipimpin oleh Zeke, me-register domain name [marley.yyy.com](http://marley.yyy.com/) untuk worker Laravel mengarah pada Annie. Namun ternyata tidak hanya kaum Marley saja yang berinisiasi, kaum Eldia ternyata sudah mendaftarkan domain name [eldia.yyy.com](http://eldia.yyy.com/) untuk worker PHP mengarah pada Armin. (0)

<details>
<summary> Jawaban </summary>

### Setup DNS pada Fritz (DNS Server)

a. Instalasi dependencies yang diperlukan pada `nano /root/.bashrc`

```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install bind9 -y

```

b. Buat `nano dns.sh` lalu jalankan dengan `./dns.sh`, namun sebelumnya pastikan telah menjalankan command `chmod +x dns.sh`

```

#!/bin/bash

# Update sistem
apt-get update

# Install bind9
apt-get install bind9 -y

# Mulai layanan bind9
service bind9 start

# Tambahkan konfigurasi domain pada named.conf.local
cat <<EOT >> /etc/bind/named.conf.local
zone "marley.it29.com" {
    type master;
    file "/etc/bind/it29/marley.it29.com";
};

zone "eldia.it29.com" {
    type master;
    file "/etc/bind/it29/eldia.it29.com";
};
EOT

# Buat direktori jika belum ada
mkdir -p /etc/bind/it29

# Buat DNS record untuk marley.it29.com
cat <<EOT > /etc/bind/it29/marley.it29.com
\$TTL    604800
@       IN      SOA     marley.it29.com. root.marley.it29.com. (
                        2               ; Serial
                        604800          ; Refresh
                        86400           ; Retry
                        2419200         ; Expire
                        604800 )        ; Negative Cache TTL
;
@       IN      NS      marley.it29.com.
@       IN      A       10.78.1.2       ; IP Annie
www     IN      CNAME   marley.it29.com.
EOT

# Buat DNS record untuk eldia.it29.com
cat <<EOT > /etc/bind/it29/eldia.it29.com
\$TTL    604800
@       IN      SOA     eldia.it29.com. root.eldia.it29.com. (
                        2               ; Serial
                        604800          ; Refresh
                        86400           ; Retry
                        2419200         ; Expire
                        604800 )        ; Negative Cache TTL
;
@       IN      NS      eldia.it29.com.
@       IN      A       10.78.2.2       ; IP Armin
www     IN      CNAME   eldia.it29.com.
EOT

# Restart layanan bind9 untuk menerapkan perubahan
service bind9 restart

```

</details>

## Soal 1

Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.
Jauh sebelum perang dimulai, ternyata para keluarga bangsawan, Tybur dan Fritz, telah membuat kesepakatan sebagai berikut:
**Semua Client** harus menggunakan **konfigurasi ip address dari keluarga Tybur** (dhcp).
<details>
<summary> Jawaban </summary>

### Setup Tybur (DHCP Server)

a. Instalasi dependencies yang diperlukan: `nano /root/.bashrc`

```
apt-get update
apt-get install isc-dhcp-server -y
service isc-dhcp-server start

```

b.  Buat `nano tybur.bashrc` lalu jalankan dengan `./tybur.bashrc`, namun sebelumnya pastikan telah menjalankan command `chmod +x tybur.bashrc`

```
echo 'INTERFACES="eth0"' > /etc/default/isc-dhcp-server

echo 'subnet 10.78.1.0 netmask 255.255.255.0 {
	option routers 10.78.1.1;
	option broadcast-address 10.78.1.255;
	option domain-name-servers 10.78.4.2; #IP DNS Server (Fritz)
}
subnet 10.78.2.0 netmask 255.255.255.0 {
	option routers 10.78.2.1;
	option broadcast-address 10.78.2.255;
	option domain-name-servers 10.78.4.2; #IP DNS Server (Fritz)
}

subnet 10.78.3.0 netmask 255.255.255.0 {}

subnet 10.78.4.0 netmask 255.255.255.0 {}' > /etc/dhcp/dhcpd.conf

```

### Setup Paradis (DHCP Relay)

a. Instalasi dependencies yang diperlukan: `nano /root/.bashrc`

```
apt-get update
apt-get install isc-dhcp-relay -y
service isc-dhcp-relay start

```

b. buat `nano paradis.bashrc` lalu jalankan dengan `./paradis.bashrc`, namun sebelumnya pastikan telah menjalankan command `chmod +x paradis.bashrc`

```
echo 'SERVERS="10.78.4.3" # IP DHCP Server (Tybur)
INTERFACES="eth1 eth2 eth3 eth4"
OPTIONS=""' > /etc/default/isc-dhcp-relay

echo 'net.ipv4.ip_forward=1' > /etc/sysctl.conf

service isc-dhcp-relay restart

```

</details>

## Soal 2 dan 3

**Client** yang melalui bangsa **marley** mendapatkan range IP dari [prefix IP].1.05 - [prefix IP].1.25 dan [prefix IP].1.50 - [prefix IP].1.100 (2)
**Client** yang melalui bangsa **eldia** mendapatkan range IP dari [prefix IP].2.09 - [prefix IP].2.27 dan [prefix IP].2 .81 - [prefix IP].2.243 (3)
<details>
<summary> Jawaban </summary>

### Setup Tybur (DHCP Server)

a. pada file tybur.bashrc (`nano tybur.bashrc`) Edit konfigurasi subnet 10.78.1.0 dan 10.78.2.0 menjadi seperti berikut:

```
subnet 10.78.1.0 netmask 255.255.255.0 {
	range 10.78.1.05 10.78.1.25;
	range 10.78.1.50 10.78.1.100;
	option routers 10.78.1.1;
	option broadcast-address 10.78.1.255;
}
subnet 10.78.2.0 netmask 255.255.255.0 {
	range 10.78.2.09 10.78.2.27;
	range 10.78.2.81 10.78.2.243;
	option routers 10.78.2.1;
	option broadcast-address 10.78.2.255;
}

```

> lalu jalankan dengan ./tybur.bashrc, namun sebelumnya pastikan telah menjalankan command chmod +x tybur.bashrc
> 

### Testing

a. Command `ifconfig` pada client (Zeke & Erwin)

<img width="530" alt="image" src="https://github.com/user-attachments/assets/7cb9d139-090f-43cc-a965-4eb6bf06123b">
<img width="530" alt="image" src="https://github.com/user-attachments/assets/dbe67c9a-7cab-4f1c-b0d9-945143e9aed5">

</details>

## Soal 4

**Client** mendapatkan DNS dari keluarga **Fritz** dan dapat terhubung dengan internet melalui DNS tersebut
<details>
<summary> Jawaban </summary>

### Setup Tybur (DHCP Server)

a. pada file tybur.bashrc (`nano tybur.bashrc`) Edit konfigurasi subnet 10.78.1.0 dan 10.78.2.0 menjadi seperti berikut:

```
subnet 10.78.1.0 netmask 255.255.255.0 {
	range 10.78.1.05 10.78.1.25;
	range 10.78.1.50 10.78.1.100;
	option routers 10.78.1.1;
	option broadcast-address 10.78.1.255;
	option domain-name-servers 10.78.4.2; # IP DNS Server (Fritz)
}
subnet 10.78.2.0 netmask 255.255.255.0 {
	range 10.78.2.09 10.78.2.27;
	range 10.78.2.81 10.78.2.243;
	option routers 10.78.2.1;
	option broadcast-address 10.78.2.255;
	option domain-name-servers 10.78.4.2; # IP DNS Server (Fritz)
}

```

> lalu jalankan dengan ./tybur.bashrc, namun sebelumnya pastikan telah menjalankan command chmod +x tybur.bashrc

### Testing
a. ping domain dari Client (Erwin)


<img width="474" alt="image" src="https://github.com/user-attachments/assets/b75f80fa-f42d-469b-8f84-9d5600225eed">


<img width="476" alt="image" src="https://github.com/user-attachments/assets/9ddc4343-73a5-447a-93a3-ff72bbf1f1ee">




b. ping domain dari Client (Zeke)


<img width="438" alt="image" src="https://github.com/user-attachments/assets/903454e7-73d1-4b60-8f4f-332648e6206f">


<img width="420" alt="image" src="https://github.com/user-attachments/assets/b227649b-f08e-4496-b290-d55920b8f4c2">



</details>

## Soal 5

Dikarenakan keluarga Tybur tidak menyukai kaum eldia, maka mereka hanya meminjamkan ip address ke kaum eldia selama 6 menit. Namun untuk kaum marley, keluarga Tybur meminjamkan ip address selama 30 menit. Waktu maksimal dialokasikan untuk peminjaman alamat IP selama 87 menit. (5)
<details>
<summary> Jawaban </summary>

## Setup Tybur (DHCP Server)

a. pada file tybur.bashrc (`nano tybur.bashrc`) Edit konfigurasi subnet 10.78.1.0 dan 10.78.2.0 menjadi seperti berikut:

```
# Kaum Marley
subnet 10.78.1.0 netmask 255.255.255.0 {
	range 10.78.1.05 10.78.1.25;
	range 10.78.1.50 10.78.1.100;
	option routers 10.78.1.1;
	option broadcast-address 10.78.1.255;
	option domain-name-servers 10.78.4.2; # IP Fritz (DNS Server)
	default-lease-time 1800;
	max-lease-time 5220;
}

# Kaum Eldia
subnet 10.78.2.0 netmask 255.255.255.0 {
	range 10.78.2.09 10.78.2.27;
	range 10.78.2.81 10.78.2.243;
	option routers 10.78.2.1;
	option broadcast-address 10.78.2.255;
	option domain-name-servers 10.78.4.2; # IP Fritz (DNS Server)
	default-lease-time 360;
	max-lease-time 5220;
}

```

###Testing

Restart lalu buka web console pada client

```
service isc-dhcp-server restart
```

<img width="516" alt="image" src="https://github.com/user-attachments/assets/8befef05-aae1-472c-b3b4-f9f0775a4a09">

<img width="520" alt="image" src="https://github.com/user-attachments/assets/250d0afb-0790-426e-a474-aab9621cf1a3">



</details>



