# Mencoba LXC

Disini, saya akan membagikan langkah-langkah membuat rangkaian sistem terdistribusi. 
Adapun langkah-langkahnya sebagai berikut :

## Konfigurasi Web Server sister.local

1. Menginstall WSL Ubuntu 22.04

![0](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/a11261ba-8df0-4832-93ab-ace4b731f9ab)

Pertama-tama, buka aplikasi Microsoft Store. Cari "Ubuntu 22.04 LTS" lalu klik button Get untuk menginstall. 

2.  Mengganti source list Ubuntu
```
sudo nano /etc/apt/sources.list
```
menjadi
```
deb http://archive.ubuntu.com/ubuntu/ jammy main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ jammy main restricted universe multiverse

deb http://archive.ubuntu.com/ubuntu/ jammy-updates main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ jammy-updates main restricted universe multiverse

deb http://archive.ubuntu.com/ubuntu/ jammy-security main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ jammy-security main restricted universe multiverse

deb http://archive.ubuntu.com/ubuntu/ jammy-backports main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ jammy-backports main restricted universe multiverse

deb http://archive.canonical.com/ubuntu/ jammy partner
deb-src http://archive.canonical.com/ubuntu/ jammy partner
```
sudo apt update; sudo apt upgrade -y

3. Install Nginx

```
sudo apt intall nginx nginx-extras
```

4. Masuk ke direktori Nginx

```
sudo apt intall nginx nginx-extras
```

5. Buat dan Konfigurasi File sister.local

```
sudo cp default sister.local
```
```
sudo nano default sister.local
```
![IMG_20240327_121917](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/3df132ec-437e-4ec0-aca4-24477eb1b970)


6. Tes Nginx

```
sudo nginx -t
```

7. Reload Nginx

```
sudo nginx -s reload
```

8. Masuk ke Direktori HTML

```
cd /var/www/html/
```

9. Copy Isi File index.nginx-debian.html ke index.html

```
sudo cp index.nginx-debian.html index.html
```

10. Edit Konfigurasi File index.html

```
sudo nano index.html
```
![IMG_20240327_123511](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/6036a1b9-88ae-4576-92f7-32f17714525d)

11. Buka Nodepad as Administrator. Buka File hosts Pada Direktori Berikut

```
C:\Windows\System32\drivers\etc
```
![10](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/b0479beb-625c-423d-886f-8e2f5c96a6e0)

12. Buka Browser dan Ketik "sister.local"

![IMG_20240327_125330](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/ed6642c5-4bd1-49d6-aa31-c06816bb72a9)

## Install Ubuntu 20 & 18 untuk IP Status

1. Install Ubuntu 20 (Focal) as microservice1

```
sudo lxc-create -n microservice1 -t download -- --dist ubuntu --release focal --arch amd64 --force-cache --server images.linuxcontainers.org
```

2. Install Ubuntu 18 (Bionic) as microservice2

```
sudo lxc-create -n microservice2 -t download -- --dist ubuntu --release bionic --arch amd64 --force-cache --server images.linuxcontainers.org
```

3. Cek status run microservice

```
sudo lxc-ls -f
```

4. Run microservice

```
sudo lxc-start -n microservice1
sudo lxc-start -n microservice2
```

5. Masuk ke microservice untuk konfigurasi ip statis

```
sudo lxc-attach -n microservice1
sudo lxc-attach -n microservice2
```

6. Saat pertama kali konfigurasi setting new password

```
passwd
```

7. Install nano sebelum konfig ip

```
apt install nano
```

8. Cek IP

```
ip a
inet 10.0.3.198/24 brd 10.0.3.255 (microservice1 blog)
inet 10.0.3.175/24 brd 10.0.3.255 (microservice2 aboutus)
```
![317057270-0deef76f-4e54-4573-a1f4-fa3cb737b683](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/3e7f557d-5405-4ae3-89f0-3140f06c38d0)

9. Restart

```
sudo netplan apply
```

10. Tes ping google.com

```
ping google.com
```
![13](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/3e8745fc-af9d-4687-a759-3b3d8b06fca0)

11. Setelah konfig lakukan update

```
apt update; apt upgrade -y
```

## Membuat Web-Server sister.local/blog & sister.local/aboutus
1. Masuk salah satu microservice
2. Install Nginx

```
sudo apt install nginx nginx-extras
apt install curl
```
