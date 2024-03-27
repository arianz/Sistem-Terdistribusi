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


6.
