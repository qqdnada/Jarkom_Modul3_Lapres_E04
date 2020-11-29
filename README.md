# Jarkom_Modul3_Lapres_E04
Repository untuk Laporan Resmi Praktikum Modul 3 Jarkom 2020 - E04

* Michael Ricky
  0511184000078
  
* Qatrunada Qori Darwati
  05111840000059

### Soal No. 1
Membuat topologi jaringan seperti gambar di bawah ini.

Buat switch, router, server, klien subnet 1, dan klien subnet 2 pada ```topology.sh```.
```
# Switch
uml_switch –unix switch1 > /dev/null < /dev/null &
uml_switch –unix switch2 > /dev/null < /dev/null &
uml_switch –unix switch3 > /dev/null < /dev/null &

# Router
xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA eth0=tuntap,,,10.151.70.21 eth1=daemon,,,switch21 eth2=daemon,,,switch3 eth3=daemon,,,switch2 mem=256M &

# Server
xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch2 mem=160M &
xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch2 mem=128M &
xterm -T TUBAN -e linux ubd0=TUBAN,jarkom umid=TUBAN eth0=daemon,,,switch2 mem=128M &

# Klien Subnet 1
xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch1 mem=64M &
xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch1 mem=64M &

# Klien Subnet 2
xterm -T BANYUWANGI -e linux ubd0=BANYUWANGI,jarkom umid=BANYUWANGI eth0=daemon,,,switch3 mem=64M &
xterm -T MADIUN -e linux ubd0=MADIUN,jarkom umid=MADIUN eth0=daemon,,,switch3 mem=64M &
```

Langkah selanjutnya bisa dilihat pada Modul Pengenalan UML. Sedangkan untuk setting IP di ```/etc/network/interfaces``` setiap UML adalah sebagai berikut.

**SURABAYA (Router)**

![SURABAYA](https://github.com/qqdnada/Jarkom_Modul3_Lapres_E04/blob/main/images/SURABAYA.JPG)


**MALANG (DNS Server)**

![MALANG](https://github.com/qqdnada/Jarkom_Modul3_Lapres_E04/blob/main/images/MALANG.JPG)


**TUBAN (DHCP Server)**

![TUBAN](https://github.com/qqdnada/Jarkom_Modul3_Lapres_E04/blob/main/images/TUBAN.JPG)


**MOJOKERTO (Proxy Server)**

![MOJOKERTO](https://github.com/qqdnada/Jarkom_Modul3_Lapres_E04/blob/main/images/MOJOKERTO.JPG)

Karena seluruh klien **TIDAK DIPERBOLEHKAN** menggunakan konfigurasi IP Statis maka, pada ```/etc/network/interfaces``` klien, setting IP-nya seperti di bawah ini.

**GRESIK SIDOARJO BANYUWANGI MADIUN (Klien)**

![KLIEN](https://github.com/qqdnada/Jarkom_Modul3_Lapres_E04/blob/main/images/KLIEN.JPG)


### NOTE!
Karena kelompok kami masih belum bisa untuk setup DHCP Server pada TUBAN (dengan relay pada SURABAYA), maka kami setup DHCP pada SURABAYA (tanpa DHCP Relay). Untuk itu, kami menginstall **isc-dhcp-server** pada router SURABAYA, **bind9** pada server MALANG, dan **squid3** pada server MOJOKERTO.


### Soal No. 3 - No. 6
**(3)** Client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200. **(4)** Client pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70. **(5)** Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP. **(6)** Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit, sedangkan client pada subnet 3 mendapatkan peminjaman IP selama 10 menit.

Pada SURABAYA tambahkan script berikut ini di ```/etc/dhcp/dhcpd.conf```.

![SURABAYA-DHCP](https://github.com/qqdnada/Jarkom_Modul3_Lapres_E04/blob/main/images/SURABAYA-DHCP.JPG)
