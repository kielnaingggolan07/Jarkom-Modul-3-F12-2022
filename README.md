# Jarkom-Modul-3-F12-2022

Nama Anggota | NRP
------------------- | --------------
Hesekiel Nainggolan | 5025201054
Khuria Khusna | 5025201053
Afiq Akram | 5025201270

### Router yang digunakan

- Wise (DNS Server)
- Westalis (DHCP Server)
- Berlint (Proxy Server (1))
- Ostania (DHCP Relay (2))
- Eden (Web Server)

### Konfigurasi Network

Ostania

```
auto eth0
    iface eth0 inet dhcp

auto eth1
    iface eth1 inet static
    address 192.205.1.1
    netmask 255.255.255.0

auto eth2
    iface eth2 inet static
    address 192.205.2.1
    netmask 255.255.255.0

auto eth3
    iface eth3 inet static
    address 192.205.3.1
    netmask 255.255.255.0
```

SSS

```
auto eth0
iface eth0 inet dhcp
```

Garden

```
auto eth0
iface eth0 inet dhcp
```

Wise

```
auto eth0
iface eth0 inet static
    address 192.205.2.2
    netmask 255.255.255.0
    gateway 192.205.2.1
```

Berlint

```
auto eth0
iface eth0 inet static
    address 192.205.2.3
    netmask 255.255.255.0
    gateway 192.205.2.1
```

Westalis

```
auto eth0
iface eth0 inet static
    address 192.205.2.4
    netmask 255.255.255.0
    gateway 192.205.2.1
```

Eden

```
auto eth0
iface eth0 inet static
    address 192.205.3.2
    netmask 255.255.255.0
    gateway 192.205.3.1
```

NewstonCastle

```
auto eth0
iface eth0 inet static
    address 192.205.3.3
    netmask 255.255.255.0
    gateway 192.205.3.1
```

KemonoPark

```
auto eth0
iface eth0 inet static
    address 192.205.3.4
    netmask 255.255.255.0
    gateway 192.205.3.1
```

#### buat script `script.sh`

a. Ostania (DHCP Relay (2))

```
 iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.205.0.0/16
 echo nameserver 192.168.122.1 > /etc/resolv.conf 
 apt-get update
```

b. Westalis (DHCP Server)

```
 echo nameserver 192.168.122.1 > /etc/resolv.conf
 apt-get update
```

c. Berlint (Proxy Server (1))

```
 echo nameserver 192.168.122.1 > /etc/resolv.conf
 apt-get update
```

d. Wise (DNS Server)

```
 echo nameserver 192.168.122.1 > /etc/resolv.conf
 apt-get update
```

e. Eden (Web Server)

```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
```

### Tambahkan kedalam masing-masing script.sh
Wise (DNS Server)

```
 apt-get update
 apt-get install bind9 -y
 service bind9 start
 service bind9 status
```

Westalis (DHCP Server)

```
 apt-get update
 apt-get install isc-dhcp-server -y
```
### 1. Westalis (DHCP Server)

buat script untuk menampung interfaces eth0, `soal1.sh`:
<code>cp /etc/default/isc-dhcp-server soal1.sh</code>

dan tambahkan
<code>INTERFACES=”eth0”</code>

Berlint (Proxy Server)

```
apt-get update
apt-get install squid -y
service squid start
service squid status
```

### 2. Ostania (DHCP Relay)

```
apt-get update
apt-get install isc-dhcp-relay -y
```
buat script untuk menampung interfacesc servers, options, `soal2.sh`:
<code>cp /etc/default/isc-dhcp-relay soal2.sh</code>

dan isi
```
SERVERS=”192.205.2.4"
INTERFACES="eth1 eth2 eth3"
OPTIONS="”
```

dan lakukan <code>service isc-dhcp-relay restart</code>

### 3&4. (3) Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.50 - [prefix IP].1.88 dan [prefix IP].1.120 - [prefix IP].1.155 (4) Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.10 - [prefix IP].3.30 dan [prefix IP].3.60 - [prefix IP].3.85

<strong>Westalis (DHCP Server)</strong>
tambahkan pada <code>nano /etc/dhcp/dhcpd.conf</code>

```

#

# Sample configuration file for ISC dhcpd for Debian

#

# Attention: If /etc/ltsp/dhcpd.conf exists, that will be used as

# configuration file instead of this file.

#

#

# The ddns-updates-style parameter controls whether or not the server will

# attempt to do a DNS update when a lease is confirmed. We default to the

# behavior of the version 2 packages ('none', since DHCP v2 didn't

# have support for DDNS.)

ddns-update-style none;

# option definitions common to all supported networks...

option domain-name "example.org";
option domain-name-servers ns1.example.org, ns2.example.org;

default-lease-time 600;
max-lease-time 7200;

# If this DHCP server is the official DHCP server for the local

# network, the authoritative directive should be uncommented.

#authoritative;

# Use this to send dhcp log messages to a different log file (you also

# have to hack syslog.conf to complete the redirection).

log-facility local7;

# No service will be given on this subnet, but declaring it helps the

# DHCP server to understand the network topology.

#subnet 10.152.187.0 netmask 255.255.255.0 {
#}

# This is a very basic subnet declaration.

#subnet 10.254.239.0 netmask 255.255.255.224 {

# range 10.254.239.10 10.254.239.20;

# option routers rtr-239-0-1.example.org, rtr-239-0-2.example.org;

#}

# This declaration allows BOOTP clients to get dynamic addresses,

# which we don't really recommend.

#subnet 10.254.239.32 netmask 255.255.255.224 {

# range dynamic-bootp 10.254.239.40 10.254.239.60;

# option broadcast-address 10.254.239.31;

# option routers rtr-239-32-1.example.org;

#}

# A slightly different configuration for an internal subnet.

#subnet 10.5.5.0 netmask 255.255.255.224 {

# range 10.5.5.26 10.5.5.30;

# option domain-name-servers ns1.internal.example.org;

# option domain-name "internal.example.org";

# option subnet-mask 255.255.255.224;

# option routers 10.5.5.1;

# option broadcast-address 10.5.5.31;

# default-lease-time 600;

# max-lease-time 7200;

#}

# Hosts which require special configuration options can be listed in

# host statements. If no address is specified, the address will be

# allocated dynamically (if possible), but the host-specific information

# will still come from the host declaration.

#host passacaglia {

# hardware ethernet 0:0:c0:5d:bd:95;

# filename "vmunix.passacaglia";

# server-name "toccata.fugue.com";

#}

# Fixed IP addresses can also be specified for hosts. These addresses

# should not also be listed as being available for dynamic assignment.

# Hosts for which fixed IP addresses have been specified can boot using

# BOOTP or DHCP. Hosts for which no fixed address is specified can only

# be booted with DHCP, unless there is an address range on the subnet

# to which a BOOTP client is connected which has the dynamic-bootp flag

# set.

#host fantasia {

# hardware ethernet 08:00:07:26:c0:a5;

# fixed-address fantasia.fugue.com;

#}

# You can declare a class of clients and then do address allocation

# based on that. The example below shows a case where all clients

# in a certain class get addresses on the 10.17.224/24 subnet, and all

# other clients get addresses on the 10.0.29/24 subnet.

#class "foo" {

# match if substring (option vendor-class-identifier, 0, 4) = "SUNW";

#}

#shared-network 224-29 {

# subnet 10.17.224.0 netmask 255.255.255.0 {

# option routers rtr-224.example.org;

# }

# subnet 10.0.29.0 netmask 255.255.255.0 {

# option routers rtr-29.example.org;

# }

# pool {

# allow members of "foo";

# range 10.17.224.10 10.17.224.250;

# }

# pool {

# deny members of "foo";

# range 10.0.29.10 10.0.29.230;

# }

#}

subnet 192.205.1.0 netmask 255.255.255.0 {
range 192.205.1.50 192.205.1.88;
range 192.205.1.120 192.205.1.155;
option routers 192.205.1.1;
option broadcast-address 192.205.1.255;
option domain-name-servers 192.205.2.2;
default-lease-time 300;
max-lease-time 6900;

}

subnet 192.205.3.0 netmask 255.255.255.0 {
range 192.205.3.10 192.205.3.30;
range 192.205.3.60 192.205.3.85;
option routers 192.205.3.1;
option broadcast-address 192.205.3.255;
option domain-name-servers 192.205.2.2;
default-lease-time 600;
max-lease-time 6900;
}

subnet 192.205.2.0 netmask 255.255.255.0 {
option routers 192.205.2.1;
}
```
kemudaian lakukan <code>cp /etc/dhcp/dhcpd.conf soal3.sh</code>

pada line ke 17 masukan tanda petik “ sebelum dan sesudah example.org (“example.org”)
dan lakukan <code>service isc-dhcp-server restart</code>

### 5. Client mendapatkan DNS dari WISE dan client dapat terhubung dengan internet melalui DNS tersebut

#### WISE

<code>nano /etc/bind/named.conf.options</code> dan tambahkan

```
forwarders {
192.168.122.1;
};
allow-query{any;};
```
kemudan lakukan <code> cp /etc/bind/named.conf.options soal5.sh</code>
dan mengcommand
<code>dnssec-validation auto;</code>
dan <code>service bind9 restart</code>

### 6. Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 5 menit sedangkan pada client yang melalui Switch3 selama 10 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 115 menit.

#### Westalis
<strong>Sudah dilakukan disoal 3&4</strong>
lakukan <code>bash soal3.sh</code>
dan lakukan <code>service isc-dhcp-server restart</code>

### 7. Loid dan Franky berencana menjadikan Eden sebagai server untuk pertukaran informasi dengan alamat IP yang tetap dengan IP [prefix IP].3.13

#### Westalis

<code>nano /etc/dhcp/dhcpd.conf</code> dan tambahkan

```
host Eden {
hardware ethernet 7a:76:36:74:f5:48;
fixed-address 192.205.3.24;
}
```
kemudian lakukan <code>cp /etc/dhcp/dhcpd.conf soal8.sh</code>

lalu lakukan <code>service isc-dhcp-server restart</code>

#### Eden

<code>nano /etc/network/interfaces</code>

```
auto eth0
iface eth0 inet dhcp
hwaddress ether 7a:76:36:74:f5:48
```
kemudian lakukan  <code>cp /etc/network/interfaces soal8.sh</code>

### 8. SSS, Garden, dan Eden digunakan sebagai client Proxy agar pertukaran informasi dapat terjamin keamanannya, juga untuk mencegah kebocoran data.

```
Note: Sebelum melakukan konfigurasi proxy pada client, server proxy harus dikonfigurasi terlebih dahulu (no. 9 dan seterusnya).
```

Konfigurasi Client Proxy SSS

```
export http_proxy="http://192.205.2.3:8080"
```

Konfigurasi Client Proxy Garden

```
export http_proxy="http://192.205.2.3:8080"
```

Konfigurasi Client Proxy Eden

```
export http_proxy="http://192.205.2.3:8080"
```

### 9. Jadikan Berlint sebagai Proxy Server sehingga dapat mengakses internet.

9.1 setelah menginstall squid <strong>Berlint</strong> kemudian lakukan:

```
service squid start
service squid status
```

9.2 Konfigurasi squid

9.2.1 backup file config default squid

```
mv /etc/squid/squid.conf /etc/squid/squid.conf.bak
```

9.2.2 buat konfigurasi squid baru pada file /etc/squid/squid.conf pada file config baru dengan command

```
nano /etc/squid/squid.conf
```

masukkan script:

```
http_port 8080
visible_hostname Berlint
```

9.3 restart squid

```
service squid restart
```

### 10. Client hanya dapat mengakses internet di luar hari dan jam kerja (senin-jumat 08:00 - 17:00) dan hari libur (dapat mengakses 24 jam penuh)

<strong>Berlint</strong>

<code>nano /etc/squid/acl.conf</code> dan tambahkan

```
acl UNAVAILABLE_WORKING time MTWHF 08:00-17:00

nano /etc/squid/squid.conf dan masukan

include /etc/squid/acl.conf
http_port 8080
http_access deny UNAVAILABLE_WORKING
http_access allow all
visible_hostname Berlint
```

dan lakukan <code>service squid restart</code>

### 11. client hanya dapat mengakses domain loid-work.com dan franky-work.com (IP tujuan domain dibebaskan). Saat akses internet dibuka, client dilarang untuk mengakses web tanpa HTTPS. (Contoh web HTTP: http://example.com)

<strong>WISE</strong>

<code>nano /etc/bind/named.conf.local</code> dan masukan

```
zone "loid-work.com" {
type master;
file "/etc/bind/jarkom/loid-work.com";
};

zone "franky-work.com" {
type master;
file "/etc/bind/jarkom/franky-work.com";
};
```

<code>mkdir /etc/bind/jarkom/</code>

<code>nano /etc/bind/jarkom/loid-work.com</code> dan masukan

```
$TTL 604800
@ IN SOA loid-work.com. root.loid-work.com. (
20211108 ; Serial
604800 ; Refresh
86400 ; Retry
2419200 ; Expire
604800 ) ; Negative Cache TTL
;
@ IN NS loid-work.com.
@ IN A 192.205.2.3
@ IN AAAA ::1
www IN CNAME loid-work.com.
```

<code>nano /etc/bind/jarkom/franky-work.com</code> dan masukan

```
$TTL 604800
@ IN SOA franky-work.com. root.franky-work.com. (
20211108 ; Serial
604800 ; Refresh
86400 ; Retry
2419200 ; Expire
604800 ) ; Negative Cache TTL
;
@ IN NS franky-work.com.
@ IN A 192.205.2.3
@ IN AAAA ::1
www IN CNAME franky-work.com.
```

<strong>Berlint</strong>

<code>nano /etc/squid/squid.conf</code> dan tambahkan

```
http_port 5000
visible_hostname loid-work.com
visible_hostname franky-work.com
http_access allow all
```

dan lakukan <code>service squid restart</code>

### 12. Agar menghemat penggunaan, akses internet dibatasi dengan kecepatan maksimum 128 Kbps pada setiap host (Kbps = kilobit per second; lakukan pengecekan pada tiap host, ketika 2 host akses internet pada saat bersamaan, keduanya mendapatkan speed maksimal yaitu 128 Kbps)

<strong>Berlin</strong>

<code>nano /etc/squid/acl-bandwidth.conf</code> dan masukan

```
delay_pools 2
delay_class 1 1
delay_class 1 2
delay_access 1 allow all
delay_access 2 allow all
delay_parameters 2 128000/128000 128000/128000
```

<code>nano /etc/squid/squid.conf</code> dan masukan

```
include /etc/squid/acl-bandwidth.conf
```

lakukan <code>service squid restart</code>
