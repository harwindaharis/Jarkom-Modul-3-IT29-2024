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

Pulau Paradis telah menjadi tempat yang damai selama 1000 tahun, namun kedamaian tersebut tidak bertahan selamanya. Perang antara kaum Marley dan Eldia telah mencapai puncak. Kaum Marley yang dipimpin oleh Zeke, me-register domain name marley.yyy.com untuk worker Laravel mengarah pada Annie. Namun ternyata tidak hanya kaum Marley saja yang berinisiasi, kaum Eldia ternyata sudah mendaftarkan domain name eldia.yyy.com untuk worker PHP mengarah pada Armin. (0)

```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install bind9 -y  

echo "zone \"marley.it29.com\" {
	type master;
	file \"/etc/bind/jarkom/marley.it29.com\";
};

zone \"eldia.it29.com\" {
	type master;
	file \"/etc/bind/jarkom/eldia.it29.com\";
};
" > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

marley="
;
;BIND data file for local loopback interface
;
\$TTL    604800
@    IN    SOA    marley.it29.com. root.marley.it29.com. (
        2        ; Serial
                604800        ; Refresh
                86400        ; Retry
                2419200        ; Expire
                604800 )    ; Negative Cache TTL
;                   
@    IN    NS    marley.it29.com.
@       IN    A    10.78.1.2
"
echo "$marley" > /etc/bind/jarkom/marley.it29.com

eldia="
;
;BIND data file for local loopback interface
;
\$TTL    604800
@    IN    SOA    eldia.it29.com. root.eldia.it29.com. (
        2        ; Serial
                604800        ; Refresh
                86400        ; Retry
                2419200        ; Expire
                604800 )    ; Negative Cache TTL
;                   
@    IN    NS    eldia.it29.com.
@       IN    A    10.78.2.2
"
echo "$eldia" > /etc/bind/jarkom/eldia.it29.com

service bind9 restart

```

## Soal 1

Lakukan konfigurasi sesuai dengan peta yang sudah diberikan. (1)

Sesuai Config di Atas

## Soal [ 2 - 5 ]

Client yang melalui bangsa marley mendapatkan range IP dari [prefix IP].1.05 - [prefix IP].1.25 dan [prefix IP].1.50 - [prefix IP].1.100 (2)

Client yang melalui bangsa eldia mendapatkan range IP dari [prefix IP].2.09 - [prefix IP].2.27 dan [prefix IP].2 .81 - [prefix IP].2.243 (3)

Client mendapatkan DNS dari keluarga Fritz dan dapat terhubung dengan internet melalui DNS tersebut (4)

Dikarenakan keluarga Tybur tidak menyukai kaum eldia, maka mereka hanya meminjamkan ip address ke kaum eldia selama 6 menit. Namun untuk kaum marley, keluarga Tybur meminjamkan ip address selama 30 menit. Waktu maksimal dialokasikan untuk peminjaman alamat IP selama 87 menit. (5)





