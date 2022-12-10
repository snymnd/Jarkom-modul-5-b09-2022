# Jarkom-modul-5-b09-2022

### Anggota Kelompok
| Nama                 | NRP        |
|----------------------|------------|
| Gery Febrian Setyara | 5025201151 |
| Muhammad Yunus       | 5025201171 |
| Hafiz Kurniawan      | 5025201032 |

## Persiapan
![image](https://user-images.githubusercontent.com/92217354/206858559-47a0cdc3-8a56-47ae-9495-111b598a5bcf.png)


## Pembagian Subnet
![image](https://user-images.githubusercontent.com/92217354/206858904-913f2cb8-efad-4a62-a682-f7846f0f6cf1.png)


## VLSM
|Subnet|Alias  | Jumlah IP  | Length |
|------|-------|------------|--------|
|  A1  |Eden-Wise-Westalis|3|/29|
|  A2  |Westalis-Desmond|701|/22|
|  A3  |Westalis-Forger|63|/26|
|  A4  |Westalis-Strix|2|/30|
|  A5  |Strix-Ostania|2|/30|
|  A6  |Ostania-Blackbell|256|/24|
|  A7  |Ostania-Briar|201|/24|
|  A8  |Ostania-Garden-SSS|3|/29|
|  Total |                       |1231|/21|


## Konfigurasi

- Konfigurasi `Strix`

```
auto eth0
iface eth0 inet static
    address 192.168.122.2
    netmask 255.255.255.0
    gateway 192.168.122.1

auto eth1
iface eth1 inet static
	address 192.177.7.85
	netmask 255.255.255.252



auto eth2
iface eth2 inet static
	address 192.177.7.81
	netmask 255.255.255.252
```

- Konfigurasi `Ostania`

```
auto eth0
iface eth0 inet static
	address 192.177.7.86
	netmask 255.255.255.252
	gateway 192.177.7.85

auto eth1
iface eth1 inet static
	address 192.177.4.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.177.6.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.177.7.73
	netmask 255.255.255.248
```

- Konfigurasi `Westalis`

```
auto eth0
iface eth0 inet static
	address 192.177.7.82
	netmask 255.255.255.252
	gateway 192.177.7.81
	
auto eth1
iface eth1 inet static
	address 192.177.0.1
	netmask 255.255.252.0

auto eth2
iface eth2 inet static
	address 192.177.7.1
	netmask 255.255.255.192


auto eth3
iface eth3 inet static
	address 192.177.7.65
	netmask 255.255.255.248
```

- Konfigurasi `Wise`

```
auto eth0
iface eth0 inet static
	address 192.177.7.67
	netmask 255.255.255.248
	gateway 192.177.7.65
```

- Konfigurasi `Eden`

```
auto eth0
iface eth0 inet static
	address 192.177.7.66
	netmask 255.255.255.248
	gateway 192.177.7.65
```

- Konfigurasi `Garden`

```
auto eth0
iface eth0 inet static
	address 192.177.7.74
	netmask 255.255.255.248
	gateway 192.177.7.73
```

- Konfigurasi `SSS`

```
auto eth0
iface eth0 inet static
	address 192.177.7.75
	netmask 255.255.255.248
	gateway 192.177.7.73
```

- Konfigurasi `Semua Client`

```
 auto eth0
 iface eth0 inet dhcp
 ```

## Routing

- Routing `Strix`

```
 route add -net 192.177.0.0 netmask 255.255.252.0 gw 192.177.7.82
route add -net 192.177.7.0 netmask 255.255.255.192 gw 192.177.7.82
route add -net 192.177.7.64 netmask 255.255.255.248 gw 192.177.7.82

route add -net 192.177.6.0 netmask 255.255.255.0 gw 192.177.7.86
route add -net 192.177.4.0 netmask 255.255.255.0 gw 192.177.7.86
route add -net 192.177.7.72 netmask 255.255.255.248 gw 192.177.7.86
 ```
- Routing `Ostania`

```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.177.7.85
 ```

- Routing `Westalis`

```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.177.7.81
```

## Konfigurasi DHCP 

### DHCP Server
- Konfigurasi pada Wise `/etc/default/isc-dhcp-server` untuk mengarah ke router Westalis
```
INTERFACE="eth0"
```
- Konfigurasi pada `/etc/dhcp/dhcpd.conf` agar subnet dapat terhubung dengan DNS Server
```
subnet 192.177.7.64 netmask 255.255.255.248 {
}

subnet 192.177.0.0 netmask 255.255.252.0 {
    range 192.177.0.2 192.177.3.254;
    option routers 192.177.0.1;
    option broadcast-address 192.177.3.255;
    option domain-name-servers 192.177.7.66;
}

subnet 192.177.7.0 netmask 255.255.255.192 {
    range 192.177.7.2 192.177.7.62;
    option routers 192.177.7.1;
    option broadcast-address 192.177.7.63;
    option domain-name-servers 192.177.7.66;
}
subnet 192.177.4.0 netmask 255.255.255.0 {
    range 192.177.4.2 192.177.4.254;
    option routers 192.177.4.1;
    option broadcast-address 192.177.4.255;
    option domain-name-servers 192.177.7.66;
}

subnet 192.177.6.0 netmask 255.255.255.0 {
    range 192.177.6.2 192.177.6.254;
    option routers 192.177.6.1;
    option broadcast-address 192.177.6.255;
    option domain-name-servers 192.177.7.66;
}
```
### DHCP Relay

- Tambahkan Konfigurasi pada semua router pada `/etc/default/isc-dhcp-relay` agar terhubung dengan DHCP Server

```
SERVERS="192.177.7.67"


INTERFACES="eth0 eth1 eth2 eth3"


OPTIONS=""
```

- Tambahkan Konfigurasi pada semua router pada `/etc/sysctl.conf`


```
net.ipv4.ip_forward=1
net.ipv4.conf.all.accept_source_route = 1
```

### DNS Server
- Tambahkan Konfigurasi DNS Server(Eden) pada `/etc/bind/named.conf.options`

## No.1
Disini diminta agar semua dapat mengakses internet,tapi tidak menggunakan Masquerade
```
iptables -t nat -A POSTROUTING -s 192.177.0.0/16 -o eth0 -j SNAT --to-s 192.168.122.2
```
dengan menggunakan Postrouting sehingga semua dengan IP 192.177 dapat mengakses internet melalui routing router `Strix` 192.168.122.2

## No.2
Untuk Mendrop semua TCP dan UDP diperlukan dengan menggunakan Aturan IPtables
```
iptables -A FORWARD -d 192.177.7.64/29 -i eth0 -p tcp --dport 80 -j DROP
iptables -I INPUT -s 192.177.7.64 -j DROP
iptables -I INPUT -p tcp  --dport 80 -j DROP
```
Jadi ketika dicoba dengan `nc -l -p 80` Eden dan Wise,dan Melakukan `nmap -p 80 192.177.7.67` pada client apapun 

![image](https://user-images.githubusercontent.com/92217354/206860328-a30bb3b8-33ce-46be-b0dd-7343c1ced644.png)

dan ketika dicek diluar IP Topology maka akan error 

![image](https://user-images.githubusercontent.com/92217354/206860361-d6f458e5-b275-4a7b-bc12-d912323235b3.png)

## No.3
Diberikan Aturan Iptables agar hanya dapat Maksimal 2 Koneksi Saja
```
iptables -A INPUT -p icmp -m connlimit --connlimit-above 2 --connlimit-mask 21 -j DROP
```

Jadi ketika dilakukan ping ke DHCP Server dari client maka akan 2 jalan dan sisanya tidak

![image](https://user-images.githubusercontent.com/92217354/206860803-75f6324a-1ae9-4b6e-b5c6-6644a9018736.png)

![image](https://user-images.githubusercontent.com/92217354/206860809-153717b1-4590-4603-8d31-d4bbea7e3624.png)

dan yang tidak jalan

![image](https://user-images.githubusercontent.com/92217354/206860818-7b4e15cd-4f7d-407f-8b07-3cb4f51316e2.png)

## No.4
Menambahkan Peraturan pada IPtables sehingga hanya dapat diakses pada 
```
iptables -A INPUT -s 192.177.7.0/25,192.177.0.0/22 -m time --timestart 07:00 --timestop 16:00 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -s 192.177.7.0/25,192.177.0.0/22 -m time --timestart 16:01 --timestop 00:00 --weekdays Mon,Tue,Wed,Thu,Fri -j DROP
iptables -A INPUT -s 192.177.7.0/25,192.177.0.0/22 -m time --timestart 00:01 --timestop 06:59 --weekdays Mon,Tue,Wed,Thu,Fri -j DROP
iptables -A INPUT -s 192.177.7.0/25,192.177.0.0/22 -m time --weekdays Sun,Sat -j DROP
```

Sehingga ketika diakses pada jam tersebut,maka bisa ping ke DHCP Server
```
date -s "12 DEC 2022 09:00:00"
```

![image](https://user-images.githubusercontent.com/92217354/206861570-f0cdece8-dee6-4c4d-bad5-ce2a370fd769.png)

akan tetapi jika diatur diluar jam tersebut akan auto di reject

![image](https://user-images.githubusercontent.com/92217354/206861585-2d14aa1e-c42d-42c6-bc8f-5b130183b4f4.png)



## Kendala

SUSAAAAHHHHHHHH,dan kurang waktu karena mendekati FP sehingga banyak yang harus dilakukan
