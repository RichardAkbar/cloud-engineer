Setup Grup dan Pengguna SFTP di App Server Stratos Datacenter
Deskripsi
Dokumen ini menjelaskan langkah-langkah untuk membuat grup nautilus_sftp_users dan menambahkan pengguna kano ke grup tersebut di semua App server (stapp01, stapp02, stapp03) di Stratos Datacenter, sesuai dengan tugas administrasi sistem.
Prasyarat

Akses ke jump host dengan pengguna thor dan kata sandi mjolnir123.
Kredensial untuk App server:
stapp01: tony/Ir0nM@n
stapp02: steve/Am3ric@
stapp03: banner/BigGr33n


Perintah Linux seperti groupadd, useradd, usermod, dan id.

Langkah-langkah
1. Login ke Jump Host
Dari terminal, login ke jump host:
ssh thor@jump_host.stratos.xfusioncorp.com

Masukkan kata sandi: mjolnir123
2. Akses Setiap App Server
Gunakan SSH untuk mengakses masing-masing App server dengan kredensial yang sesuai:
# Untuk stapp01
ssh tony@stapp01
# Kata sandi: Ir0nM@n

# Untuk stapp02
ssh steve@stapp02
# Kata sandi: Am3ric@

# Untuk stapp03
ssh banner@stapp03
# Kata sandi: BigGr33n

3. Buat Grup nautilus_sftp_users
Di setiap App server, jalankan perintah berikut untuk membuat grup:
sudo groupadd nautilus_sftp_users

4. Verifikasi atau Buat Pengguna kano
Periksa apakah pengguna kano sudah ada:
id kano

Jika pengguna tidak ada (output: "no such user"), buat pengguna baru:
sudo useradd -m kano

5. Tambahkan kano ke Grup nautilus_sftp_users
Tambahkan pengguna kano ke grup:
sudo usermod -aG nautilus_sftp_users kano

6. Verifikasi Keanggotaan Grup
Pastikan kano ada di grup:
getent group nautilus_sftp_users

Output harus menunjukkan kano sebagai anggota, misalnya: nautilus_sftp_users:x:1001:kano.
7. Ulangi untuk Setiap Server
Ulangi langkah 3-6 di stapp01, stapp02, dan stapp03.
8. (Opsional) Otomatisasi dengan Skrip
Untuk efisiensi, Anda dapat menggunakan skrip shell dari jump host:
#!/bin/bash
SERVERS=("tony@stapp01" "steve@stapp02" "banner@stapp03")
for server in "${SERVERS[@]}"; do
    echo "Mengatur $server..."
    ssh $server "sudo groupadd nautilus_sftp_users; sudo useradd -m kano; sudo usermod -aG nautilus_sftp_users kano"
    if [ $? -eq 0 ]; then
        echo "Sukses di $server"
    else
        echo "Gagal di $server"
    fi
done

Simpan sebagai setup_sftp_users.sh, berikan izin eksekusi (chmod +x setup_sftp_users.sh), lalu jalankan (bash setup_sftp_users.sh).
Verifikasi

Pastikan grup nautilus_sftp_users ada di /etc/group di setiap server.
Pastikan kano terdaftar sebagai anggota grup dengan getent group nautilus_sftp_users.
Verifikasi direktori home kano ada di /home/kano.

