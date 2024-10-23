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

up echo nameserver 192.168.122.1 > /etc/resolv.conf
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
