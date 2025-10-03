# Jarkom-Modul-1-2025-IT-20

**Laporan Resmi Praktikum Modul 1 — Komunikasi Data & Jaringan Komputer 2025**

## Daftar Anggota

| Nama                  | NRP        |
|-----------------------|------------|
| Zahra Khaalishah      | 5027241070 |
| Dimas Muhammad Putra  | 5027241076 |

---

## Soal 1
<img width="2424" height="826" alt="no1" src="https://github.com/user-attachments/assets/5f697eb1-979a-4e16-9725-8a4a94d24547" />
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
	address 192.221.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.221.2.1
	netmask 255.255.255.0
```
Melkor
```
auto eth0
iface eth0 inet static
	address 192.221.1.2
	netmask 255.255.255.0
	gateway 192.221.1.1
```
Manwe
```
auto eth0
iface eth0 inet static
	address 192.221.1.3
	netmask 255.255.255.0
	gateway 192.221.1.1
```
Varda
```
auto eth0
iface eth0 inet static
	address 192.221.2.2
	netmask 255.255.255.0
	gateway 192.221.2.1
```
Ulmo
```
auto eth0
iface eth0 inet static
	address 192.221.2.3
	netmask 255.255.255.0
	gateway 192.221.2.1
```
## Soal 4
masukkan ini pada router eru 
``` 
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.221.0.0/16 
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
<img width="2545" height="1254" alt="no6" src="https://github.com/user-attachments/assets/73ed45ac-3ca9-4a90-aec5-58b53f69e311" />
  - ini adalah hasil tangkapan dari wireshark setelah di filter


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
<img width="1146" height="792" alt="no7" src="https://github.com/user-attachments/assets/73eeb7f9-b9ef-4f98-9c13-c0140f23a9ca" />
- ini adalah bukti tes permission dimana yang bisa mengakses file tersebut hanya ainur
- <img width="813" height="451" alt="no7 2" src="https://github.com/user-attachments/assets/792f2a52-ed45-4292-bec1-7ac57f006d68" />
- ini adalah bukti dimana yang bisa masuk ke ftp localhost hanya ainur





## Soal 8
capture ulmo
- pertama masuk ulmo dulu
- setelah itu download di ulmo
` wget --no-check-certificate "https://docs.google.com/uc?export=download&id=11ra_yTV_adsPIXeIPMSt0vrxCBZu0r33" -O cuaca.zip`
- setelah itu melakukan `apt update && apt install inetutils-ftp -y` 
- setelah mendownload ftp masuk ke ip eru `ftp 192.168.122.251`
- setelah itu masuk user ainur
<img width="1234" height="446" alt="no8" src="https://github.com/user-attachments/assets/2ac287da-bb07-487c-848c-f64717788c4f" />
- lalu lakukan sesuai perintah soal yaitu mengirim ke eru `put cuaca.zip`
- setelah itu bisa di cek di root eru di shared folder
- dan yang terakhir yaitu tangkap di wireshark
<img width="1248" height="340" alt="image" src="https://github.com/user-attachments/assets/a0718160-8ff6-4726-803a-e9a8079a91fd" />
- nah disini sudah tertulis transfer complete

## Soal 10
- sesuai perintah soal yang mengirim banyak paket
- dari melkor : `ping ip eru -c 1000 -f`
Penjelasan
  - ping ip eru → nge-ping alamat IP Eru (buat cek koneksi).
  - -c 1000 → kirim 1000 kali ping, lalu otomatis berhenti.
  - -f → "flood ping", artinya kirim ping secepat mungkin (tanpa jeda).
<img width="1142" height="213" alt="no10" src="https://github.com/user-attachments/assets/19861696-22a1-4623-94b3-a417d4ecc929" />

## Soal 11
(di melkor)
- step pertama yaitu mendownload dulu `apt-get install -y openbsd-inetd telnetd`
- selanjutnya yaitu mengubah inetd.conf dengan kode di bawah `nano /etc/inetd.conf`
```
telnet stream tcp nowait root /usr/sbin/tcpd /usr/sbin/telnetd
```
- setelah itu jalankan terlebih dahulu `service openbsd-inetd restart`
- selanjutnya kita bisa membuat user, bebas mau apa saja karena inti dari soal ini adalah bagimana memperlihatkan kelemahan telnet yang dapat melihat user dan password yang dapat ditangkap di wireshark
```
useradd -m -s /bin/bash melkor
echo "melkor:melkor" | chpasswd
```
(eru)
- telnet 192.221.1.2
- Lalu yang terakhir yaitu login sesuai user yang dibuat pada melkor
- lalu dapat dilihat username dan password yang kita buat tadi di wireshark, contohnya gambar dibawah ini
<img width="1141" height="1307" alt="Screenshot 2025-10-01 141738" src="https://github.com/user-attachments/assets/d116c68b-9439-46b2-85d6-e8c93116f0e2" />

## Soal 12
- di melkor MELKOR
- step pertama yaitu download dulu `apt install ufw apache2 vsftpd -y`
- setelah itu jalankan ini
 ```
service apache2 start
service vsftpd start
```
- lalu membuka port 21 (FTP), 23 (telnet), 80 (HTTP) untuk TCP/UDP, lalu menolak port 666 untuk TCP/UDP.
`ufw allow 21/tcp && ufw allow 21/udp && ufw allow 23/udp && ufw allow 23/tcp && ufw allow 80/tcp && ufw allow 80/udp && ufw deny 666/tcp && ufw deny 666/udp`
- reload aturan UFW dan meng-enable firewall `ufw reload && ufw enable`
- menambahkan aturan iptables untuk menerima koneksi TCP ke port 80, 23, 21, dan 666.
```
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -p tcp --dport 23 -j ACCEPT
iptables -A INPUT -p tcp --dport 21 -j ACCEPT
iptables -I INPUT -p tcp --dport 666 -j REJECT
```
- masuk ke root eru
- install netcat `apt update && apt install netcat-openbsd`
- lalu jalankan ini untuk melihat apakah port ini terbuka/tertutup -vz 192.221.1.2 21 80 666


## Soal 13
- yang pertama download ssh terlebih dahulu `apt-get install -y openssh-server && service ssh start`
- setelah itu adduser eru
- setelah itu buka gns3 lalu start capture line varda yang nyambung ke eru 
- setelah itu masuk ke root eru
- lalu jalankan ssh eru@192.221.2.2
- lalu lihat di wireshark
<img width="2296" height="617" alt="image" src="https://github.com/user-attachments/assets/a92e5a3e-44ef-43da-9280-6c1a5e9d6055" />


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

## Soal 16
Melkor semakin murka ia meletakkan file berbahaya di server milik Manwe. Dari file capture yang ada, identifikasi file apa yang diletakkan oleh Melkor. nc 10.15.43.32 3403

- What credential did the attacker use to log in?
Format: user:pass. follow bagian yg berwarna ungu, kemudian akan muncul tampilan seperti yg dibawah dan terlihat jawabannya
`“USER ind@psg420.com
331 User ind@psg420.com OK. Password required
PASS {6r_6e#TfT1p”`
 <img width="1203" height="417" alt="image" src="https://github.com/user-attachments/assets/e05ef9c7-ef9b-4d3b-99a3-6220dc904bf8" />
 <img width="212" height="46" alt="image" src="https://github.com/user-attachments/assets/6534e701-18e6-41ce-b552-399bd6b88425" />

- “How many files are suspected of containing malware?”
ada berjumlah 5 yg dihitung dari jumlah exe pada isi file tersebut,
ex:  perhatikan bagian “SIZE q.exe”  “RETR w.exe” “ “RETR r.exe”
 <img width="145" height="126" alt="image" src="https://github.com/user-attachments/assets/49884ad2-ca89-40ec-9737-dfdea7c91bf9" />

- Kemudian untuk lanjutannya mencari file sesuai perintah kemudian save as pada device dan buka di terminal baru untuk mengakses isi jawabannya sesuai dengan exe perintahnya, lanjut sampai pertanyaan bagian akhir
 <img width="522" height="497" alt="image" src="https://github.com/user-attachments/assets/c25dcb92-2612-44bc-8623-a248ffab7e79" />

## Soal 17
Manwe membuat halaman web di node-nya yang menampilkan gambar cincin agung. Melkor yang melihat web tersebut merasa iri sehingga ia meletakkan file berbahaya agar web tersebut dapat dianggap menyebarkan malware oleh Eru. Analisis file capture untuk menggagalkan rencana Melkor dan menyelamatkan web Manwe. nc 10.15.43.32 3404

- "What is the name of the first suspicious file?"
  Buka bagian “file’ atas kiri ketika telah meangkses file yg diberikan, tekan “export objects” - “tekan bagian HTTP”
  <img width="561" height="186" alt="image" src="https://github.com/user-attachments/assets/d01c1fbf-3e43-4a51-ac59-651dbef3c842" />
  jika sudah akan muncul spt ini, perhatikan bagian “Filename” terdapat 3 pilihan, coba satu satu sampai benar.
  
- "What is the name of the second suspicious file?"
  <img width="662" height="163" alt="image" src="https://github.com/user-attachments/assets/72ccf4f2-e83f-4479-8e12-6e5eeab3dd72" />

- "What is the hash of the second suspicius file (knr.exe)
  save juga sesuai perintah file knr.exe, akses di (terminal lain) kemudian akan muncul kode unik
  <img width="666" height="283" alt="image" src="https://github.com/user-attachments/assets/112f7758-37a3-46d4-9c1a-f03a56b7c2aa" />

## Soal 18
Karena rencana Melkor yang terus gagal, ia akhirnya berhenti sejenak untuk berpikir. Pada saat berpikir ia akhirnya memutuskan untuk membuat rencana jahat lainnya dengan meletakkan file berbahaya lagi tetapi dengan metode yang berbeda. Gagalkan lagi rencana Melkor dengan mengidentifikasi file capture yang disediakan agar dunia tetap aman. 
nc 10.15.43.32 3405

- Export Objects → SMB pada Wireshark digunakan untuk mengekstrak objek atau file yang ditransfer melalui protokol SMB (Server Message Block). Protokol SMB umumnya digunakan dalam sistem operasi berbasis Windows untuk layanan berbagi berkas (file sharing), printer, maupun komunikasi antar-proses. Dengan memilih opsi ini, Wireshark akan menampilkan daftar file atau objek yang berhasil ditangkap selama sesi komunikasi SMB. Dari daftar tersebut, pengguna dapat menyimpan file tertentu ke dalam komputer lokal untuk dianalisis lebih lanjut.

<img width="621" height="490" alt="image" src="https://github.com/user-attachments/assets/2dd2593a-55ec-4046-aa4f-7090177c6e03" />

- Perhatika yg berformat “exe.” terdapat 2 jumlah untuk jawaban no 1, kemudia nomor 2 dan 3 adalah nama dari tiap file tsb.
<img width="1197" height="155" alt="image" src="https://github.com/user-attachments/assets/8aee9219-a09f-4ac0-891b-7aed601eb850" />

- jika sudah save file exe yg pertama, kemudian akses di terminal yg berbeda untuk menemukan sha256nya
<img width="677" height="62" alt="image" src="https://github.com/user-attachments/assets/f3534460-7c31-41ce-a03b-e0486da704d9" />

-lakukan juga untuk pertanyaan terakhir 
<img width="647" height="58" alt="image" src="https://github.com/user-attachments/assets/e5dcadf1-1284-4ae0-a050-a36c5c2fa95e" />


## Soal 19
Manwe mengirimkan email berisi surat cinta kepada Varda melalui koneksi yang tidak terenkripsi. Melihat hal itu Melkor sipaling jahat langsung melancarkan aksinya yaitu meneror Varda dengan email yang disamarkan. Analisis file capture jaringan dan gagalkan lagi rencana busuk Melkor. nc 10.15.43.32 3406 

- Setelah mengakses ip dan file yg diberikan, terdapat soal pertama. untuk mendapatkan jawaban, akses file kemudian Di baris filter Wireshark tulis: smtp  kemudian follow. dan perhatikan bagian
`MAIL FROM: <YourLife36@7162.com>, “YourLife”` adalah jawabannya, lalu untuk jawaban pertanyaan ke 2 pada bagian `To stop me, pay exactly 1600$ in bitcoin (BTC).`, `1600` adalah jawabannya. Prtanyaan ketiga perhatikan bagian “My bitcoin wallet is: `1CWHmuF8dHt7HBGx5RKKLgg9QA2GmE3UyL` dan kode unik tsb adalah jawabannya.

<img width="853" height="870" alt="image" src="https://github.com/user-attachments/assets/75ca9fac-7c07-4a52-8d99-a6ea764ba2a6" />

<img width="578" height="237" alt="image" src="https://github.com/user-attachments/assets/e63c7293-267b-49b3-9ca7-423016a40c72" />

## Soal 20
Untuk yang terakhir kalinya, rencana besar Melkor yaitu menanamkan sebuah file berbahaya kemudian menyembunyikannya agar tidak terlihat oleh Eru. Tetapi Manwë yang sudah merasakan adanya niat jahat dari Melkor, ia menyisipkan bantuan untuk mengungkapkan rencana Melkor. Analisis file capture dan identifikasi kegunaan bantuan yang diberikan oleh Manwë untuk menggagalkan rencana jahat Melkor selamanya. nc 10.15.43.32 3407

- Akses ip serta file yang di beri, kemudian masuk ke Edit → Preferences → Protocols → TLS Wireshark hanya bisa menampilkan paket TLS sebagai Application Data terenkripsi. Agar bisa membaca isi sebenarnya (misalnya HTTP), kita perlu mengaktifkan fitur dekripsi TLS. Fitur ini diatur melalui menu Preferences, khususnya pada bagian protokol TLS. Dengan membuka pengaturan ini, kita memberi tahu Wireshark bahwa akan ada tambahan informasi (kunci) yang bisa dipakai untuk mendekripsi sesi TLS
 <img width="368" height="281" alt="image" src="https://github.com/user-attachments/assets/a5cc10d0-0edc-4adb-b5a2-8cb80f9a70e7" />
 <img width="305" height="93" alt="image" src="https://github.com/user-attachments/assets/cf94a95b-214c-434d-95dc-7abb5e72455e" />

- Isi (Pre)-Master-Secret log filename dengan keylogfile.txt Dengan memasukkan path file ini, Wireshark dapat menggunakan key tersebut untuk mendekripsi sesi
<img width="455" height="40" alt="image" src="https://github.com/user-attachments/assets/e9b12c04-eeaf-45ac-a23d-de74786fb190" />

- Pada pertanyaaan ke 3 save file invest kemudian buka terminal  baru akses dengan tambahan “sha256sum nama file” 
<img width="652" height="55" alt="image" src="https://github.com/user-attachments/assets/da45fe8b-b6b1-400f-8970-aa9ab0e53732" />


   

**Kelompok:** K-20  
---
## 
