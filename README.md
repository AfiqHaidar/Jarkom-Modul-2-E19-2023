# Jarkom-Modul-1-E19-2023
Laporan Resmi Praktikum Modul 2 Kelompok E19
| Nama               |  NRP       | 
|--------------------|-------------|
| M. Armand Giovani | 5025211054  |
| Afiq Fawwaz Haidar | 5025211246  |

## Soal 1
**Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian sebagai berikut. Folder topologi dapat diakses pada drive berikut**

### Penjelasan :

Topologi yang kami dapat adalah `Topologi no 05`. Berikut Topologi yang sudah dibuat :

![image](https://github.com/AfiqHaidar/Jarkom-Modul-2-E19-2023/assets/100523471/3cd03843-1652-4b1f-9cb4-980fdd76e511)

### Konfigurasi
**- Pandudewanata (Router)**
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.46.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.46.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.46.3.1
	netmask 255.255.255.0
```

**- Sadewa (Client)**
```
auto eth0
iface eth0 inet static
	address 10.46.1.3
	netmask 255.255.255.0
	gateway 10.46.1.1
```

**- Nakula (Client)**
```
auto eth0
iface eth0 inet static
	address 10.46.1.2
	netmask 255.255.255.0
	gateway 10.46.1.1
```

**- Yudhistira (DNS Master)**
```
auto eth0
iface eth0 inet static
	address 10.46.2.2
	netmask 255.255.255.0
	gateway 10.46.2.1
```

**- Werkudara (DNS Slave)**
```
auto eth0
iface eth0 inet static
	address 10.46.2.3
	netmask 255.255.255.0
	gateway 10.46.2.1
```

**- Arjuna (Load Balancer)**
```
auto eth0
iface eth0 inet static
	address 10.46.3.5
	netmask 255.255.255.0
	gateway 10.46.3.1
```
**- Prabakusuma (WebServer)**
```
auto eth0
iface eth0 inet static
	address 10.46.3.2
	netmask 255.255.255.0
	gateway 10.46.3.1
```

**- Abimanyu (WebServer)**
```
auto eth0
iface eth0 inet static
	address 10.46.3.3
	netmask 255.255.255.0
	gateway 10.46.3.1
```


**- Wisanggeni (WebServer)**
```
auto eth0
iface eth0 inet static
	address 10.46.3.4
	netmask 255.255.255.0
	gateway 10.46.3.1
```

## Soal 2
**uatlah website utama pada node arjuna dengan akses ke `arjuna.yyy.com` dengan alias `www.arjuna.yyy.com` dengan yyy merupakan kode kelompok.**

Melakukan `Configurasi` pada DNS Master Yudhistira untuk membuat website utama arjuna.E19.com dan www.arjuna.E19.com

**Yudhistira**
```sh
#update
apt-get update
#install bind
apt-get install bind9 -y
#buat zone
echo '
zone "arjuna.E19.com" {
    type master;
    file "/etc/bind/arjuna/arjuna.E19.com";
};
' > /etc/bind/named.conf.local
#buat folder arjuna
mkdir /etc/bind/arjuna
#copy db.local
cp /etc/bind/db.local /etc/bind/arjuna/arjuna.E19.com
#config arjuna.E19.com
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     arjuna.E19.com. root.arjuna.E19.com. (
        2022102401         ; Serial
            604800         ; Refresh
            86400         ; Retry
            2419200         ; Expire
            604800 )       ; Negative Cache TTL
;
@       IN      NS      arjuna.E19.com.
@       IN      A       10.46.3.5       ; IP arjuna
www     IN      CNAME   arjuna.E19.com.   ; alias
@       IN      AAAA    ::1
' > /etc/bind/arjuna/arjuna.E19.com
#restart bind
service bind9 restart
```

- Memasukkan konfigurasi ke `named.conf.local`
- membuat direktori arjuna
- copy `db.local` ke directory arjuna dan ganti namanya menjadi `arjuna.E19.com`
- Terakhir, mengubah data di `arjuna.E19.com` menjadi seperti diatas, kemudian restart bind9.

**Sadewa & Nakula**
Setelah membuat website utama, kita dapat cek website menggunakan ping di `Client Sadewa` atau `Client Nakula` dan cek alias menggunakan host -t CNAME..
```
ping arjuna.E19.com -c 3
host -t CNAME www.arjuna.E19.com
ping www.arjuna.E19.com -c 3
```
### Screenshot hasil:

## Soal 3
**Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke `abimanyu.yyy.com` dan alias `www.abimanyu.yyy.com`.**

Melakukan `Confiigurasi` pada DNS Master Yudhistira untuk membuat website utama abimanyu.E19.com dan www.abimanyu.E19.com

**Yudhistira**
```sh
#update
apt-get update
#install bind
apt-get install bind9 -y
#buat zone
echo '
zone "abimanyu.E19.com" {
    type master;
    file "/etc/bind/abimanyu/abimanyu.E19.com";
};
' > /etc/bind/named.conf.local
#buat folder abimanyu
mkdir /etc/bind/abimanyu
#copy db.local
cp /etc/bind/db.local /etc/bind/abimanyu/abimanyu.E19.com
#config abimanyu.E19.com
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.E19.com. root.abimanyu.E19.com. (
        2022102401         ; Serial
            604800         ; Refresh
            86400         ; Retry
            2419200         ; Expire
            604800 )       ; Negative Cache TTL
;
@       IN      NS      abimanyu.E19.com.
@       IN      A       10.46.3.2       ; IP abimanyu
www     IN      CNAME   abimanyu.E19.com.   ; alias
@       IN      AAAA    ::1
' > /etc/bind/abimanyu/abimanyu.E19.com
#restart bind
service bind9 restart
```

- Memasukkan konfigurasi ke `named.conf.local`
- membuat direktori abimanyu
- copy `db.local` ke directory abimanyu dan ganti namanya menjadi `abimanyu.E19.com`
- Terakhir, mengubah data di `abimanyu.E19.com` menjadi seperti diatas, kemudian restart bind9.

**Sadewa & Nakula**
Setelah membuat website utama, kita dapat cek website menggunakan ping di `Client Sadewa` atau `Client Nakula` dan cek alias menggunakan host -t CNAME..
```
ping abimanyu.E19.com -c 3
host -t CNAME www.abimanyu.E19.com
ping www.abimanyu.E19.com -c 3
```
### Screenshot hasil:

## Soal 4
**Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain `parikesit.abimanyu.yyy.com` yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.**

Melakukan `Configurasi` pada DNS Master Yudhistira untuk membuat subdomain parikesit.abimanyu.E19.com

**Yudhistira**
```sh
#config subdomain abimanyu.E19.com
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.E19.com. root.abimanyu.E19.com. (
        2022102401         ; Serial
            604800         ; Refresh
            86400         ; Retry
            2419200         ; Expire
            604800 )       ; Negative Cache TTL
;
@       IN      NS      abimanyu.E19.com.
@       IN      A       10.46.2.2       ; IP Yudhistira
www     IN      CNAME   abimanyu.E19.com.   ; alias
parikesit IN    A       10.46.3.3       ; IP Abimanyu
@       IN      AAAA    ::1
' > /etc/bind/abimanyu/abimanyu.E19.com


#restart bind
service bind9 restart
```

- Mengubah data di `abimanyu.E19.com` menjadi seperti diatas, dengan menambahkan subdomain parikesit.abimanyu.E19.com dengan IP Abimanyu. kemudian restart bind9.

**Sadewa & Nakula**
Setelah membuat website utama, kita dapat cek website menggunakan ping di `Client Sadewa` atau `Client Nakula` dan cek alias menggunakan host -t CNAME..
```sh
ping parikesit.abimanyu.E19.com -c 3
host -t CNAME www.parikesit.abimanyu.E19.com
ping www.parikesit.abimanyu.E19.com -c 3
```
### Screenshot hasil:

## Soal 5
**Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)**

Melakukan `Configurasi` pada DNS Master Yudhistira untuk membuat reverse domain untuk domain utama.

**Yudhistira**
```sh
#update
apt-get update

#install bind
apt-get install bind9 -y

#buat zone
echo '
zone "3.46.10.in-addr.arpa" {
    type master;
    file "/etc/bind/abimanyu/3.46.10.in-addr.arpa";
};
' > /etc/bind/named.conf.local

# Buat folder abimanyu
mkdir /etc/bind/abimanyu
# Copy db.local ke folder abimanyu
cp /etc/bind/db.local /etc/bind/abimanyu/3.46.10.in-addr.arpa

#config 3.46.10.in-addr.arpa
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.E19.com. root.abimanyu.E19.com. (
             2022100601              ; Serial
                 604800              ; Refresh
                  86400              ; Retry
                2419200              ; Expire
                 604800 )            ; Negative Cache TTL
;
3.46.10.in-addr.arpa.   IN      NS      abimanyu.E19.com.   // reverse domain name 
3                       IN      PTR      abimanyu.E19.com.     // membuat pointer 3.46.10.in-addr.arpa
' > /etc/bind/abimanyu/3.46.10.in-addr.arpa

#restart bind
service bind9 restart
```

- Memasukkan konfigurasi ke `named.conf.local`
- membuat direktori abimanyu
- copy `db.local` ke directory abimanyu dan ganti namanya menjadi `3.46.10.in-addr.arpa`
- Terakhir, mengubah data di `3.46.10.in-addr.arpa` menjadi seperti diatas, kemudian restart bind9.

**Sadewa & Nakula**
Kita dapat cek reverse domain menggunakan host -t PTR di `Client Sadewa` atau `Client Nakula`.
```
host -t PTR 10.46.3.3
```
### Screenshot hasil:


## Soal 6
**Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.**

Melakukan `Configurasi` pada DNS Slave Werkudara untuk membuat DNS Slave untuk domain utama.

**Yudhistira**
```sh
echo '
zone "arjuna.E19.com" {
    type master;
    notify yes;
    also-notify { 10.46.2.3; };        // IP Werkudara
    allow-transfer { 10.46.2.3; };     // IP Werkudara
    file "/etc/bind/arjuna/arjuna.E19.com"; //  file master di yudhistira
};

zone "abimanyu.E19.com" {
    type master;
    notify yes; 
    also-notify { 10.46.2.3; };         // IP Werkudara
    allow-transfer { 10.46.2.3; };      // IP Werkudara
    file "/etc/bind/abimanyu/abimanyu.E19.com"; // file master di yudhistira 
};

zone "3.46.10.in-addr.arpa" {
    type master;
    notify yes;
    also-notify { 10.46.2.3; };           // IP Werkudara
    allow-transfer { 10.46.2.3; };        // IP Werkudara
    file "/etc/bind/abimanyu/3.46.10.in-addr.arpa";
};
' > /etc/bind/named.conf.local

#restart bind
service bind9 restart
```

- Mengubah konfigurasi `named.conf.local`.
- Menambahkan `notify yes`, `also-notify { IP Werkudara };`, `allow-transfer { Werkudara };` pada zone arjuna dan abimanyu.
- Restart bind9.

**Werkudara**
```sh
# nameserver
echo '
nameserver 192.168.122.1 
nameserver 10.46.2.2    
' > /etc/resolv.conf
#update
apt-get update

#install bind
apt-get install bind9 -y

#buat zone
echo '
zone "arjuna.E19.com" {
    type slave; //  master jika di yudhistira
    masters { 10.46.2.2; }; #IP yudhistira  
    file "/var/lib/bind/arjuna.E19.com";
};

zone "abimanyu.E19.com" {
    type slave;
    masters { 10.46.2.2; }; #IP yudhistira
    file "/var/lib/bind/abimanyu.E19.com";
};

zone "baratayuda.abimanyu.E19.com" {
    type master;    // master jika di yudhistira 
    file "/etc/bind/baratayuda/baratayuda.abimanyu.E19.com";
};

zone "3.46.10.in-addr.arpa" {
    type slave; // master jika di yudhistira
    masters { 10.46.2.2; }; #IP yudhistira
    file "/etc/bind/3.46.10.in-addr.arpa";
};
' > /etc/bind/named.conf.local
```

- Mengubah konfigurasi `named.conf.local`.
- Menambahkan zone arjuna dan abimanyu
- `type slave` karena Werkudara akan dijadikan DNS Slave dengan master menuju IP Yudhistira.

**Sadewa & Nakula**

Pertama, menambahkan nameserver DNS Slave ke `resolv.conf`. Dapat dilakukan dengan melakukan `Configurasi Name Server`.
```
echo '
//nameserver 192.168.122.1
nameserver 10.46.2.2
nameserver 10.46.2.3
' > /etc/resolv.conf
```
- Menambahkan nameserver DNS Slave/IP Werkudara.

**Kita akan cek apakah DNS Slave berhasil**

Pertama akan distop bind9 di `Yudhistira`
```
service bind9 stop 
```
Kemudian melakukan ping di `Client Sadewa` atau `Client Nakula`.
```
ping www.abimanyu.E19.com -c 3
ping www.arjuna.E19.com -c 3
```
Jika berhasil, maka DNS Slave sudah benar.

### Screenshot hasil:

## Soal 7
**Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu `baratayuda.abimanyu.yyy.com` dengan alias `www.baratayuda.abimanyu.yyy.com` yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.**

## Soal 8
**Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses `rjp.baratayuda.abimanyu.yyy.com` dengan alias `www.rjp.baratayuda.abimanyu.yyy.com` yang mengarah ke Abimanyu.**
## Soal 9
**Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.**
## Soal 10
**Kemudian gunakan algoritma `Round Robin` untuk Load Balancer pada `Arjuna`. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh**
**- Prabakusuma:8001**
**- Abimanyu:8002**
**- Wisanggeni:8003**
## Soal 11
**Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server `www.abimanyu.yyy.com`. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy**

Dengan konfigurasi ini, sistem akan menggunakan server DNS dalam urutan: 10.46.2.2 `(Yudhistira)`, 10.46.2.3 `(Werkudhara)`, dan 192.168.122.1 `(Router)` saat mencari alamat IP untuk nama domain.
```bash
echo nameserver 10.46.2.2 > /etc/resolv.conf
echo nameserver 10.46.2.3 >> /etc/resolv.conf
echo nameserver 192.168.122.1 >> /etc/resolv.conf
```
Perintah-perintah ini digunakan untuk mengelola paket perangkat lunak pada sistem Linux, dengan menginstal dan mengkonfigurasi server web Apache dengan dukungan PHP, serta alat-alat bantu seperti unzip dan wget pada sistem Linux
```bash
apt-get update
apt-get install apache2 -y
apt-get install php -y
apt-get install libapache2-mod-php7.0 -y
apt-get install unzip -y
apt-get install wget -y
```
Download document untuk root pada konfigurasi `abimanyu.E19.com`
```bash
wget -O '/var/www/abimanyu.E19.com.zip' 'https://drive.usercontent.google.com/download?id=1a4V23hwK9S7hQEDEcv9FL14UkkrHc-Zc'
unzip -o /var/www/abimanyu.E19.com.zip -d /var/www/
mv /var/www/abimanyu.yyy.com /var/www/abimanyu.E19.com
rm /var/www/abimanyu.E19.com.zip
rm -rf /var/www/abimanyu.yyy.com
rm /etc/apache2/sites-available/000-default.conf
```
Konfigurasi tersebut mengatur server web Apache dengan alamat email admin, menentukan lokasi berkas situs web (/var/www/abimanyu.E19.com), serta menyediakan nama domain utama (abimanyu.E19.com) dan alias (www.abimanyu.E19.com). Selain itu, konfigurasi juga menunjuk tempat penyimpanan log kesalahan dan aktivitas akses server web.
```bash
echo '
<VirtualHost *:80>

    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/abimanyu.E19.com

    ServerName abimanyu.E19.com
    ServerAlias www.abimanyu.E19.com    

    #LogLevel info ssl:warn

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    #Include conf-available/serve-cgi-bin.conf
</VirtualHost>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
' > /etc/apache2/sites-available/abimanyu.E19.com.conf
```
Aktifkan (enable) konfigurasi situs web dengan nama domain "abimanyu.E19.com" yang sebelumnya telah diatur dalam file konfigurasi "abimanyu.E19.com.conf" menggunakan a2ensite. Setelah mengaktifkan konfigurasi tersebut, perintah "service apache2 restart" digunakan untuk me-restart layanan Apache agar perubahan konfigurasi berlaku. 
```bash
a2ensite abimanyu.E19.com.conf
service apache2 restart
```
## Soal 12
**Setelah itu ubahlah agar url `www.abimanyu.yyy.com/index.php/home` menjadi `www.abimanyu.yyy.com/home`.**

Menentukan pengaturan untuk direktori /var/www/abimanyu.E19.com/index.php/home. Dalam hal ini, opsi +Indexes diaktifkan, yang akan memungkinkan direktori ini menampilkan daftar isi (index) jika tidak ada halaman indeks yang ditemukan.
```bash
echo -e '<VirtualHost *:80>
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/abimanyu.E19.com

  ServerName abimanyu.E19.com
  ServerAlias www.abimanyu.E19.com

  <Directory /var/www/abimanyu.E19.com/index.php/home>
          Options +Indexes
  </Directory>

  Alias "/home" "/var/www/abimanyu.E19.com/index.php/home"

  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>' > /etc/apache2/sites-available/abimanyu.E19.com.conf
```
Ganti file .htaccess di direktori /var/www/abimanyu.E19.com/ dengan aturan-aturan modul mod_rewrite Apache. Aturan-aturan ini akan mengaktifkan mod_rewrite, dan jika suatu permintaan URL tidak mengarah ke direktori yang ada (RewriteCond %{REQUEST_FILENAME} !-d), maka permintaan URL akan diubah sehingga akan mencoba mencari berkas dengan ekstensi .php (contoh: mengubah namafile menjadi namafile.php) dengan mengabaikan huruf besar/kecil (NC) dan mengakhiri proses rewrite (L). Ini dapat berguna untuk mengizinkan pengguna mengakses halaman web tanpa harus menyertakan ekstensi .php di URL.
```bash
echo '
 RewriteEngine On
 RewriteCond %{REQUEST_FILENAME} !-d
 RewriteRule ^([^\.]+)$ $1.php [NC,L]
 ' > /var/www/abimanyu.E19.com/.htaccess
```

## Soal 13
**Selain itu, pada subdomain `www.parikesit.abimanyu.yyy.com`, DocumentRoot disimpan pada `/var/www/parikesit.abimanyu.yyy`**

Download document untuk root pada konfigurasi `parikesit.abimanyu.E19.com`
```bash
wget -O '/var/www/parikesit.abimanyu.E19.com.zip' 'https://drive.usercontent.google.com/download?id=1LdbYntiYVF_NVNgJis1GLCLPEGyIOreS'
unzip -o /var/www/parikesit.abimanyu.E19.com.zip -d /var/www/
mv /var/www/parikesit.abimanyu.yyy.com /var/www/parikesit.abimanyu.E19
rm /var/www/parikesit.abimanyu.E19.com.zip
rm -rf /var/www/parikesit.abimanyu.yyy.com
```
Konfigurasi tersebut mengatur server web Apache untuk menangani situs web dengan nama domain "parikesit.abimanyu.E19.com" dan alias "www.parikesit.abimanyu.E19.com". Direktori root situs web berlokasi di /var/www/parikesit.abimanyu.E19, dengan catatan kesalahan dan aktivitas akses server web yang dicatat dalam file log yang sesuai.
```bash
echo "<VirtualHost *:80>

        ServerName parikesit.abimanyu.E19.com
        ServerAlias www.parikesit.abimanyu.E19.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/parikesit.abimanyu.E19

        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log combined

</VirtualHost>" > /etc/apache2/sites-available/parikesit.abimanyu.E19.com.conf
```
Aktifkan (enable) konfigurasi situs web dengan nama domain "parikesit.abimanyu.E19.com" yang sebelumnya telah diatur dalam file konfigurasi "parikesit.abimanyu.E19.com.conf" menggunakan a2ensite. Setelah mengaktifkan konfigurasi tersebut, perintah "service apache2 restart" digunakan untuk me-restart layanan Apache agar perubahan konfigurasi berlaku. 
```bash
a2ensite parikesit.abimanyu.E19.com.conf
service apache2 restart
```
## Soal 14
**Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).**

Tambahkan folder `secret` pada DocumentRoot. 

Konfiguras tambahan untuk server web Apache yang mengontrol opsi indeks direktori untuk dua direktori berbeda dalam situs web. Direktori /var/www/parikesit.abimanyu.E19/public memungkinkan tampilan daftar isi jika tidak ada halaman indeks yang ditemukan, sementara /var/www/parikesit.abimanyu.E19/secret melarang tampilan daftar isi, sehingga mengamankan akses ke berkas-berkas dalam direktori tersebut.
```bash
echo "<VirtualHost *:80>

        ServerName parikesit.abimanyu.E19.com
        ServerAlias www.parikesit.abimanyu.E19.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/parikesit.abimanyu.E19

        <Directory /var/www/parikesit.abimanyu.E19/public>
            Options +Indexes
        </Directory>

        <Directory /var/www/parikesit.abimanyu.E19/secret>
            Options -Indexes
        </Directory>


        <Directory /var/www/parikesit.abimanyu.E19/public/images>
            Options +FollowSymLinks +Indexes -Multiviews
            AllowOverride All
        </Directory>

        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log combined

</VirtualHost>" > /etc/apache2/sites-available/parikesit.abimanyu.E19.com.conf
```
## Soal 15
**Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.**

Konfigurasi tambahan untuk menentukan halaman kesalahan kustom (custom error pages) yang akan ditampilkan kepada pengguna ketika terjadi kesalahan 404 (Halaman Tidak Ditemukan) atau 403 (Akses Ditolak) di situs web. 
```bash
echo "<VirtualHost *:80>

        ServerName parikesit.abimanyu.E19.com
        ServerAlias www.parikesit.abimanyu.E19.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/parikesit.abimanyu.E19

        <Directory /var/www/parikesit.abimanyu.E19/public>
            Options +Indexes
        </Directory>

        <Directory /var/www/parikesit.abimanyu.E19/secret>
            Options -Indexes
        </Directory>

        <Directory /var/www/parikesit.abimanyu.E19/public/images>
            Options +FollowSymLinks +Indexes -Multiviews
            AllowOverride All
        </Directory>

        ErrorDocument 404 /error/404.html
        ErrorDocument 403 /error/403.html

        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log combined

</VirtualHost>" > /etc/apache2/sites-available/parikesit.abimanyu.E19.com.conf
```


## Soal 16
**Buatlah suatu konfigurasi virtual host agar file asset `www.parikesit.abimanyu.yyy.com/public/js` menjadi `www.parikesit.abimanyu.yyy.com/js`**

Alias "/js" "/var/www/parikesit.abimanyu.E19/public/js": Ini menciptakan alias sehingga ketika pengguna mengakses URL /js pada situs web, server akan mengarahkan mereka ke direktori /var/www/parikesit.abimanyu.E19/public/js.
```bash
echo "<VirtualHost *:80>

        ServerName parikesit.abimanyu.E19.com
        ServerAlias www.parikesit.abimanyu.E19.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/parikesit.abimanyu.E19

        <Directory /var/www/parikesit.abimanyu.E19/public>
            Options +Indexes
        </Directory>

        <Directory /var/www/parikesit.abimanyu.E19/secret>
            Options -Indexes
        </Directory>

        <Directory /var/www/parikesit.abimanyu.E19/public/js>
            Options +Indexes
        </Directory>

        Alias "/js" "/var/www/parikesit.abimanyu.E19/public/js"

        <Directory /var/www/parikesit.abimanyu.E19/public/images>
            Options +FollowSymLinks +Indexes -Multiviews
            AllowOverride All
        </Directory>

        ErrorDocument 404 /error/404.html
        ErrorDocument 403 /error/403.html

        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log combined

</VirtualHost>" > /etc/apache2/sites-available/parikesit.abimanyu.E19.com.conf
```
## Soal 17
**Agar aman, buatlah konfigurasi agar `www.rjp.baratayuda.abimanyu.yyy.com` hanya dapat diakses melalui port `14000` dan `14400`.**

Download document untuk root pada konfigurasi `rjp.baratayuda.abimanyu.E19.com`
```bash
wget -O '/var/www/rjp.baratayuda.abimanyu.E19.com.zip' 'https://drive.usercontent.google.com/download?id=1pPSP7yIR05JhSFG67RVzgkb-VcW9vQO6'
unzip -o /var/www/rjp.baratayuda.abimanyu.E19.com.zip -d /var/www/
mv /var/www/rjp.baratayuda.abimanyu.yyy.com /var/www/rjp.baratayuda.abimanyu.E19
rm /var/www/rjp.baratayuda.abimanyu.E19.com.zip
rm -rf /var/www/rjp.baratayuda.abimanyu.yyy.com
```
Konfigurasi VirtualHost untuk server web Apache yang meng-host situs web dengan nama domain "rjp.baratayuda.abimanyu.E19.com" dan alias "www.rjp.baratayuda.abimanyu.E19.com" pada dua port, yaitu 14000 dan 14400. Konfigurasi ini menentukan lokasi direktori root situs web, alamat email admin server, serta lokasi dan format file log untuk catatan kesalahan dan aktivitas akses server web.
```bash
echo "<VirtualHost *:14000 *:14400>

        ServerName rjp.baratayuda.abimanyu.E19.com
        ServerAlias www.rjp.baratayuda.abimanyu.E19.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/rjp.baratayuda.abimanyu.E19

        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log combined

</VirtualHost>" > /etc/apache2/sites-available/rjp.baratayuda.abimanyu.E19.com.conf
```
Konfigurasi untuk mengatur port yang akan didengarkan oleh server web Apache. Server Apache akan mendengarkan permintaan pada port 80 (HTTP) serta dua port tambahan, yaitu 14000 dan 14400. 
```bash
echo "Listen 80
Listen 14000
Listen 14400

<IfModule ssl_module>
        Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>" > /etc/apache2/ports.conf
```
Aktifkan (enable) konfigurasi situs web dengan nama domain "rjp.baratayuda.abimanyu.E19.com" yang sebelumnya telah diatur dalam file konfigurasi "rjp.baratayuda.abimanyu.E19.com.conf" menggunakan a2ensite. Setelah mengaktifkan konfigurasi tersebut, perintah "service apache2 restart" digunakan untuk me-restart layanan Apache agar perubahan konfigurasi berlaku. 
```bash
a2ensite rjp.baratayuda.abimanyu.E19.com.conf
service apache2 restart
```
## Soal 18
**Untuk mengaksesnya buatlah autentikasi username berupa `Wayang` dan password `baratayudayyy` dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada `/var/www/rjp.baratayuda.abimanyu.yyy`.**

Konfigurasi untuk menerapkan autentikasi dasar pada seluruh situs web yang di-host oleh server Apache. Pengguna harus memasukkan username dan password yang valid untuk mengakses situs web, dan informasi autentikasi disimpan dalam file .htpasswd.
```bash
echo "<VirtualHost *:14000 *:14400>

        ServerName rjp.baratayuda.abimanyu.E19.com
        ServerAlias www.rjp.baratayuda.abimanyu.E19.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/rjp.baratayuda.abimanyu.E19

        ErrorLog \${APACHE_LOG_DIR}/error.log
        CustomLog \${APACHE_LOG_DIR}/access.log combined

        <Location />
            AuthType Basic
            AuthName "Secret"
            AuthUserFile /etc/apache2/.htpasswd
            Require valid-user
        </Location>

</VirtualHost>" > /etc/apache2/sites-available/rjp.baratayuda.abimanyu.E19.com.conf
```
Menambahkan entri autentikasi untuk pengguna "Wayang" dengan password "baratayudaE19". 
```bash
htpasswd -c -b /etc/apache2/.htpasswd Wayang baratayudaE19
```
## Soal 19
**Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke `www.abimanyu.yyy.com` (alias)**

Konfigurasi VirtualHost di server web Apache yang mengarahkan semua permintaan yang masuk ke alamat IP "10.46.3.3" untuk di-redirect (dialihkan) ke situs web "www.abimanyu.E19.com"
```bash
echo -e '<VirtualHost *:80>
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/abimanyu.E19.com

  ServerName abimanyu.E19.com
  ServerAlias www.abimanyu.E19.com

  <Directory /var/www/abimanyu.E19.com/index.php/home>
          Options +Indexes
  </Directory>

  Alias "/home" "/var/www/abimanyu.E19.com/index.php/home"

  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
<VirtualHost *:80> 
        ServerName 10.46.3.3
        Redirect / http://www.abimanyu.E19.com/
</VirtualHost>
' > /etc/apache2/sites-available/abimanyu.E19.com.conf
```
## Soal 20
**Karena website `www.parikesit.abimanyu.yyy.com` semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.**

Mengonfigurasi mod_rewrite di server web Apache. Aturan-aturan tersebut akan mengarahkan permintaan yang mencocokkan pola tertentu dalam URL ke alamat URL yang berbeda. Lebih spesifiknya, jika URL mengandung "abimanyu" atau berkas dengan ekstensi ".png" di dalam direktori tersebut, dan bukan sama dengan "/public/images/abimanyu.png", maka URL akan diubah untuk mengarahkan pengguna ke "http://parikesit.abimanyu.E19.com/public/images/abimanyu.png" 
```bash
echo '
RewriteEngine On
RewriteCond %{REQUEST_URI} ^/public/images/(.*)abimanyu(.*)
RewriteCond %{REQUEST_URI} ^/public/images/(.*).png(.*)
RewriteCond %{REQUEST_URI} !/public/images/abimanyu.png
RewriteRule abimanyu http://parikesit.abimanyu.E19.com/public/images/abimanyu.png$1 [L,R=301]
' > /var/www/parikesit.abimanyu.E19/public/images/.htaccess
```

## Note
- Pastikan setiap konfigurasi apache2 yang diubah, dilakukan `service apache2 restart` agar konfigurasi terupdate

## Kendala
- pada No.11, File konfigurasi default pada apache2 harus dihapus agar konfigurasi baru yang sudah dibuat dapat dijalankan.
