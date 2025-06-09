### Linux 1

Membuat user dengan Non-Interactive Shell
~~~
sudo useradd -s /sbin/nologin john
~~~
Digunakan untuk mencegah login interaktif, cocok untuk backup agent tool.

Membuat akun dengan Tanggal Kedaluwarsa:
~~~
sudo useradd -m -e 2026-01-28 mark
~~~
Verifikasi konfigurasi user
~~~
sudo chage -l mark 
~~~
Menambahkan aturan untuk port 5003/tcp
~~~
sudo firewall-cmd --permanent --zone=public --add-port=5003/tcp
~~~
 untuk mengedit crontab (jadwal cron) milik user root, alias menjadwalkan tugas otomatis yang dijalankan sebagai root
 ~~~
 sudo crontab -e
 ~~~
 