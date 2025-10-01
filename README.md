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






### Soal dengan malware

**14. Setelah gagal mengakses FTP, Melkor melancarkan serangan brute force terhadap  
Manwe. Analisis file capture yang disediakan dan identifikasi upaya brute force Melkor.
nc 10.15.43.32 3401**

**Langkah**
1. Akses di terminal dengan nc yang diberikan
   - muncul pertanyaan pertama "How many packets are recorded in the pcapng file?"
   <img width="472" height="148" alt="image" src="https://github.com/user-attachments/assets/5bd10ac4-965c-4cee-a9ae-5775e675325e" />
  - Akses file yang diberikan di wireshark, kemudian perhatikan pojok bawah pada bagian Packets untuk melihat jawaban untuk pertanyaan pertama
   <img width="218" height="46" alt="image" src="https://github.com/user-attachments/assets/8d628d69-0d1f-4c38-a7cf-f328d0a8b2b3" />
2. Pertanyaan ke 2 "What are the user that successfully logged in?"
  - Akses 
   

**Kelompok:** K-20  
---
## 
