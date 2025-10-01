# Jarkom-Modul-1-2025-IT-20

**Laporan Resmi Praktikum Modul 1 — Komunikasi Data & Jaringan Komputer 2025**
Gambar ini menampilkan implementasi topologi jaringan virtual yang terbagi menjadi dua segmen utama, dengan Eru berfungsi sebagai titik pusat (Router) dan penghubung ke luar jaringan (NAT).
## Soal 1
1. Peran Eru (Router/Gateway)
- Eru adalah Router utama yang bertugas menghubungkan jaringan internal dengan jaringan luar (NAT1).
- Eru membagi jaringan internal menjadi dua Segmen Lokal (LAN) melalui Switch1 dan Switch2.
2. Pembagian Segmen Lokal (Switch dan Client Ainur)
- Eru menciptakan dua gateway terpisah melalui dua Switch untuk mengisolasi kelompok Client (Ainur):
Segmen 1 (Melalui Switch1)
- Switch1 berfungsi sebagai gateway untuk menghubungkan dua Client Ainur: Melkor dan Manwe
- Semua komunikasi antara Melkor dan Manwe harus melewati Switch1, dan jika ingin keluar segmen, harus melalui Eru.
- Segmen 2 (Melalui Switch2)
- Switch2 berfungsi sebagai gateway untuk menghubungkan dua Client Ainur lainnya: Varda dan Ulmo
- Semua komunikasi antara Varda dan Ulmo harus melewati Switch2, dan jika ingin keluar segmen, harus melalui Eru.

## Soal 2
- Konfigurasi Eru untuk Akses Internet
   - Akses Konfigurasi Jaringan Eru:
- Buka fitur Edit network configuration pada node Eru.
   - Tambahkan Konfigurasi DHCP pada eth0:
Pastikan konfigurasi berikut ada di dalam file /etc/network/interfaces Eru:
```
auto eth0
iface eth0 inet dhcp
```
Penjelasan: Konfigurasi ini memberitahu sistem bahwa interface eth0 harus secara otomatis mendapatkan alamat IP, netmask, dan Gateway (yang akan menunjuk ke NAT) dari DHCP server di jaringan GNS3/Virtual Machine.

## soal 3
Konfigurasi jaringan pada masing-masing node akan diterapkan melalui Edit network configuration. Pastikan seluruh pengaturan sebelumnya dihapus atau ditimpa dengan setting yang terperinci di bawah ini.
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address [Prefix IP].1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address [Prefix IP].2.1
	netmask 255.255.255.0
```
Melkor
```
auto eth0
iface eth0 inet static
	address [Prefix IP].1.2
	netmask 255.255.255.0
	gateway [Prefix IP].1.1
```
Manwe
```
auto eth0
iface eth0 inet static
	address [Prefix IP].1.3
	netmask 255.255.255.0
	gateway [Prefix IP].1.1
```
Varda
```
auto eth0
iface eth0 inet static
	address [Prefix IP].2.2
	netmask 255.255.255.0
	gateway [Prefix IP].2.1
```
Ulmo
```
auto eth0
iface eth0 inet static
	address [Prefix IP].2.3
	netmask 255.255.255.0
	gateway [Prefix IP].2.1
```
## Soal 4
masukkan ini pada router eru 
``` 
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s [Prefix IP].0.0/16 
```
- jika belum terinstall iptables, maka harus install terlebih dahulu:
```
apt update && apt install iptables -y
``` 
Keterangan:
- iptables: iptables merupakan suatu tools dalam sistem operasi Linux yang berfungsi sebagai filter terhadap lalu lintas data. Dengan iptables inilah kita akan mengatur semua lalu lintas dalam komputer, baik yang masuk, keluar, maupun yang sekadar melewati komputer kita. Untuk penjelasan lebih lanjut nanti akan dibahas pada Modul 5.
- NAT (Network Address Translation): Suatu metode penafsiran alamat jaringan yang digunakan untuk menghubungkan lebih dari satu komputer ke jaringan internet dengan menggunakan satu alamat IP.
- Masquerade: Digunakan untuk menyamarkan paket, misal mengganti alamat pengirim dengan alamat router.
- -s (Source Address): Spesifikasi pada source. Address bisa berupa nama jaringan, nama host, atau alamat IP.
- Ketikkan command cat /etc/resolv.conf di Eru
Ingat-ingat IP tersebut karena IP tersebut merupakan IP DNS, lalu ketikkan command ini di node kali yang lain
```
echo nameserver [IP DNS] > /etc/resolv.conf. Jika pada kasus contoh maka
```
atau jika di contoh nyata
```
command-nya adalah echo nameserver 192.168.122.1 > /etc/resolv.conf.
```

## Soal 5
- Jadi agar tidak hilang bisa disimpan melalui konfig langsung di nodenya seperti contoh
```
up apt update && apt install iptables
up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.221.0.0/16
```
ini dimasukkan di eru
- lalu command ini saya masukkan di node setiap client
```
echo nameserver [IP DNS] > /etc/resolv.conf. Jika pada kasus contoh maka
```

## Saoal 6
jadi kita masuk ke manwe dengan menggunakan command
```
telnet ip (port melkor)
```
setelah itu saya download link sesuai soal
```
wget --no-check-certificate "https://docs.google.com/uc?export=download&id=1bE3kF1Nclw0VyKq4bL2VtOOt53IC7lG5" -O traffic.zip
```
- setelah itu melakukan unzip
```
unzip traffic.zip
```
- setelah itu kita start capture di line antara manwe dan shwitch1 agar bisa menangkap tarffic.sh di wireshark
- nah, setelah itu langsung dijalankan, namun karena permission denied kita melakukan `chmod +x traffic.sh`
- setelah itu kita jalankan ./traffic.sh
- lalu melakukan filter di wireshark yaitu `ip.src == 192.221.1.3 or ip.dst == 192.221.1.3`

## soal 7
Step 1 – Langkah pertama adalah melakukan instalasi vsftpd (Very Secure FTP Daemon) dengan perintah:
`apt update && apt install vsftpd -y`
Step 2 – Menambahkan User FTP
Setelah server terinstal, tambahkan user baru yang akan digunakan untuk login ke FTP:
`adduser ainur
adduser melkor`
Dengan ini, sistem memiliki dua user akun FTP yaitu ainur dan melkor.
Step 3 – Aktifkan service FTP agar server bisa menerima koneksi : `service vsftpd start`
Step 4 – Agar dapat melakukan uji coba koneksi, install FTP client dengan perintah: `apt install inetutils-ftp -y`
Step 5 – Uji Koneksi FTP Lokal
Lakukan pengujian dengan mencoba login ke server FTP lokal:
ftp localhost
Jika berhasil login, berarti server FTP sudah berjalan.
Step 6 –Buat direktori khusus untuk berbagi file: `mkdir -p /srv/ftp/shared`
Step 7 – Mengatur Kepemilikan & Hak Akses
Berikan hak akses penuh kepada user ainur pada direktori shared:
```
chown -R ainur:ainur /srv/ftp/shared/
chmod -R 700 /srv/ftp/shared/
```
Step 8 – Edit file konfigurasi utama untuk menyesuaikan pengaturan:
`nano /etc/vsftpd.conf` dengan menambahkan ini
```
listen=YES
listen_ipv6=NO
anonymous_enable=NO
local_enable=YES
write_enable=YES
dirmessage_enable=YES
use_localtime=YES
xferlog_enable=YES
connect_from_port_20=YES
chroot_local_user=YES
pam_service_name=vsftpd
allow_writeable_chroot=YES
local_root=/srv/ftp/shared
tcp_wrappers=YES

user_config_dir=/etc/vsftpd_user.conf
userlist_enable=YES
userlist_file=/etc/vsftpd.user_list
userlist_deny=YES
```
Step 9 – Tambahkan user melkor ke daftar user FTP:
echo "melkor" | tee /etc/vsftpd.user_list
Step 10 – Terapkan konfigurasi terbaru dengan merestart service:
service vsftpd restart
Step 11 – Terakhir, buka file vsftpd.userlist untuk mengatur izin user:
nano /etc/vsftpd.userlist

## Soal 8
pertama masuk ulmo dulu
setelah itu download di ulmo
` wget --no-check-certificate "https://docs.google.com/uc?export=download&id=11ra_yTV_adsPIXeIPMSt0vrxCBZu0r33" -O cuaca.zip`



## Soal 14
Setelah gagal mengakses FTP, Melkor melancarkan serangan brute force terhadap  
Manwe. Analisis file capture yang disediakan dan identifikasi upaya brute force Melkor.
nc 10.15.43.32 3401

**Langkah**
1. Akses di terminal dengan nc yang diberikan
   - muncul pertanyaan pertama "How many packets are recorded in the pcapng file?"
   <img width="472" height="148" alt="image" src="https://github.com/user-attachments/assets/5bd10ac4-965c-4cee-a9ae-5775e675325e" />
   - Akses file yang diberikan di wireshark, kemudian perhatikan pojok bawah pada bagian Packets untuk melihat jawaban untuk pertanyaan pertama
   <img width="218" height="46" alt="image" src="https://github.com/user-attachments/assets/8d628d69-0d1f-4c38-a7cf-f328d0a8b2b3" />
   
2. "What are the user that successfully logged in?"
  - Dikarenakan Brute Force yang diakses menggunakan http jadi kita akan mencari protocol http pada file tersebut.
   <img width="1373" height="641" alt="image" src="https://github.com/user-attachments/assets/4c30d900-d13b-4fde-a858-e257bc600d21" />
  - Kemudian Follow ip yang menghasilkan "Succesfully" dan disitu muncul user yang suses 
    "username=n1enna&password=y4v4nn4_k3m3nt4r1"
    <img width="432" height="27" alt="image" src="https://github.com/user-attachments/assets/e5da5a7f-9198-42fc-bfea-eb9af89a5ad2" />
    <img width="552" height="77" alt="image" src="https://github.com/user-attachments/assets/8dbe37e6-30e3-46b8-9843-533f2d2c8136" />
    
3. "In wich stream were the credentials found?"
  - Perhatikan bagian bawah kolom stream
    <img width="153" height="82" alt="image" src="https://github.com/user-attachments/assets/bf4879b9-e0e7-4643-9de1-76054c2d1233" />
    <img width="498" height="60" alt="image" src="https://github.com/user-attachments/assets/22df1221-7de6-4391-bf0d-07e8e5035877" />

4. "What tools are used for brute force?"
 - Pada isi file yang telah di follow, perhatikan bagian  "User-Agent: Fuzz Faster U Fool v2.1.0-dev"
   <img width="382" height="36" alt="image" src="https://github.com/user-attachments/assets/18c6e594-bcb0-467a-93be-474893dd56e4" />
   <img width="858" height="90" alt="image" src="https://github.com/user-attachments/assets/9d9dc972-3ae8-4dab-978e-d2aeb51bee34" />
   
## Soal 15
Melkor menyusup ke ruang server dan memasang keyboard USB berbahaya pada node Manwe. Buka file capture dan identifikasi pesan atau ketikan (keystrokes) yang berhasil dicuri oleh Melkor untuk menemukan password rahasia.
nc 10.15.43.32 3402

- Buka terminal dan filenya sperti sebelumnya, kemudian muncul pertanyaan pertama check bagian “GET DESCRIPTION Response String” kemudian kebawah bagian string descriptionnya disitu muncul jawabannya adalah USB “Keyboard”.
 <img width="690" height="497" alt="image" src="https://github.com/user-attachments/assets/c3b39c67-4b86-4b79-91d8-54c56da2ceb2" />
 <img width="268" height="46" alt="image" src="https://github.com/user-attachments/assets/7e242900-8b64-40d9-bb5e-1955c193ac21" />



   

**Kelompok:** K-20  
---
## 
