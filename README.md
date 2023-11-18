# Jarkom-Modul-3-E29-2023
## Anggota
Kelompok E29
| Nama | NRP |
| --- | --- |
| Sony Hermawan | 5025211226 |
| Sekar Ambar Arum | 5025211041 |


# Laporan Resmi
* [Topologi](#Topologi)
* [Config](#Config)
* [Soal (0)](#Soal-0)

## Topologi
![Cuplikan layar 2023-11-18 230254](https://github.com/AdonisZK/Jarkom-Modul-3-E29-2023/assets/90591077/a8a2371e-71ac-4f97-a3a0-3557f3877046)

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

## Soal 0
Setelah mengalahkan Demon King, perjalanan berlanjut. Kali ini, kalian diminta untuk melakukan register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP (0) mengarah pada worker yang memiliki IP [prefix IP].x.1.
