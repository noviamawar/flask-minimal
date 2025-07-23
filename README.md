# Deploy Flask agar Tetap Jalan setelah Reboot EC2 Manual

berikut adalah langkah-langkah agar aplikasi Flask kamu tetap berjalan meskipun EC2 di-reboot atau terminal ssh ditutup:

## 1. Masuk ke EC2 Ubuntu
connect ke EC2 menggunakan SSH

## 2. Ketik Code Berikut
```bash
ls
cd flaks-minimal
sudo nano /etc/systemd/system/flaskapp.service
```
Lalu Masukan Code Didalam Flaskapp.service
```bash
[Unit]
Description=Flask Minimal App
After=network.target

[Service]
User=ubuntu
WorkingDirectory=/home/ubuntu/flask-minimal
Environment="PATH=/home/ubuntu/flask-minimal/venv/bin"
ExecStart=/home/ubuntu/flask-minimal/venv/bin/python3 app.py
Restart=always

[Install]
WantedBy=multi-user.target
```
Setelah dimasukan lalu ctrl x
y
enter

Lalu lanjutkan Dengan Code Ini
```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable flaskapp
sudo systemctl start flaskapp
sudo systemctl status flaskapp
```

## 3. Akses Flask dari Browser

Salin IP Public IPv4 dari EC2 Instance kamu
Lalu buka browser dan tempelkan IP kamu yang sudah disalin dan tambahkan :5000 di belakang IP
Jika muncul hasilnya maka berhasil

## 4. Uji apakah flask tetap berjalan setelah terminal ditutup atau reboot

Tutup tab terminal/ssh
Coba akses ulang 54.208.5.218:5000 dari browser
Kalau masih terbuka → berarti flask tetap hidup meskipun terminal ditutup

MAKA ITU SUDAH BERHASIL 

## 5. Uji otomatisasi saat EC2 di-reboot 
```BASH
sudo reboot
```
Setelah EC2 hidup kembali, connect ulang via ssh
Jalankan:
```BASH
sudo systemctl status flaskapp
```
Akses ulang browser 54.208.5.218:5000 → kalau masih terbuka, berarti flask sukses jalan otomatis saat reboot
