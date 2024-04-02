# Mencoba LXC

Disini, saya akan membagikan langkah-langkah membuat rangkaian sistem terdistribusi. 
Adapun langkah-langkahnya sebagai berikut :

## Konfigurasi Web Server sister.local

1. Menginstall WSL Ubuntu 22.04

![0](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/a11261ba-8df0-4832-93ab-ace4b731f9ab)

Pertama-tama, buka aplikasi Microsoft Store. Cari "Ubuntu 22.04 LTS" lalu klik button Get untuk menginstall. Kalau sudah akan muncul tampilan seperti berikut. 

![1](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/114b16c9-cedc-486f-a8ae-3a1289acfa43)

2.  Mengganti source list Ubuntu 
`sudo nano /etc/apt/sources.list` menjadi seperti berikut. 

![2](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/d86cb256-dcff-4f24-9678-eab54e50952e) 

kalau sudah selesai lakukan `sudo apt update` dan 
`sudo apt upgrade -y`

3. Install Linux Container

![4](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/840a7812-7eea-463b-8862-36981c44d75b)

Setelah diinstall, buka isi program dari Linux Container

![5](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/2f30c7f6-6f07-4e4d-9f63-d5cc74133863)

4. Install Nginx

![6](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/1a3ee179-fa12-4022-8e4a-ad4d190da2be)

5. Buat dan Konfigurasi File sister.local
`sudo cp default sister.local` dan 
`sudo nano default sister.local`

![IMG_20240327_121917](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/3df132ec-437e-4ec0-aca4-24477eb1b970)


6. Tes Nginx
`sudo nginx -t`

7. Reload Nginx
`sudo nginx -s reload`

8. Masuk ke Direktori HTML
`cd /var/www/html/`

9. Copy Isi File index.nginx-debian.html ke index.html
`sudo cp index.nginx-debian.html index.html`

10. Edit Konfigurasi File index.html
`sudo nano index.html`

![IMG_20240327_123511](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/6036a1b9-88ae-4576-92f7-32f17714525d)

11. Buka Nodepad as Administrator. Buka File hosts Pada Direktori Berikut
`C:\Windows\System32\drivers\etc`

![10](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/b0479beb-625c-423d-886f-8e2f5c96a6e0)

13. Buka Browser dan Ketik "sister.local"

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
`sudo lxc-ls -f`

4. Run microservice

`sudo lxc-start -n microservice1`

`sudo lxc-start -n microservice2`


5. Masuk ke microservice untuk konfigurasi ip statis

`sudo lxc-attach -n microservice1`

`sudo lxc-attach -n microservice2`

6. Saat pertama kali konfigurasi setting new password
`passwd`

7. Install nano sebelum konfig ip
`apt install nano`

8. Cek IP
`ip a`

`inet 10.0.3.198/24 brd 10.0.3.255 (microservice1 blog)`

`inet 10.0.3.175/24 brd 10.0.3.255 (microservice2 aboutus)`

![317057270-0deef76f-4e54-4573-a1f4-fa3cb737b683](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/3e7f557d-5405-4ae3-89f0-3140f06c38d0)

9. Restart
`sudo netplan apply`

10. Tes ping google.com

![microservice1](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/37b8d152-5818-4616-aa4e-3fa8858214d4)

![microservice2](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/cd447488-5b9b-4b2f-bc12-a2a1d571b110)

11. Setelah konfig lakukan update
`apt update` dan 
`apt upgrade -y`


## Membuat Web-Server sister.local/blog & sister.local/aboutus
1. Masuk salah satu microservice
2. Install Nginx dan curl

`sudo apt install nginx nginx-extras`

`apt install curl`

3. Pindah ke direktori html & edit file nginx html

`cd /var/www/html`

`nano index.nginx-debian.html`

![14](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/f41ef765-927f-4cd1-82ec-c17ea949f229)
![15](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/168d0949-1075-4fb1-88d0-306625b29903)

4. Cek sekilas apakah setelah semua file html yang di edit tidak kembali default
`curl localhost`

5. Exit untuk keluar microservice
`exit`

6. Masuk direktori parent
7. Pindah ke direktori Nginx
`cd /etc/nginx/sites-enabled/`

8. Edit sister.local
`sudo nano sister.local`

![IMG_20240328_081752](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/6ff32b28-c9c4-43c7-9230-eda3abe7bc49)

9. Cek konfigurasi
`sudo nginx -t`

10. Reload konfigurasi
`sudo nginx -s reload`

11. Masuk lagi ke dalam microservice untuk membuat mcsv1.local & mcsv2.local

`sudo lxc-attach -n microservice1`

`sudo lxc-attach -n microservice2`

12. Pindah ke direktori Nginx
`cd /etc/nginx/sites-enabled`

kemudian copy file default ke file mcsv

`cp default mcsv1.local`

`cp default mcsv2.local`

13. Edit isi file

`nano mcsv1.local`

`nano mcsv2.local`

![17](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/2fc91182-e468-445a-8f8f-4cbdb7656f78)
![18](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/a8ca41c6-cc87-4015-9361-95ab76debad2)

14. Cek konfigurasi
`sudo nginx -t`

15. Reload konfigurasi
`sudo nginx -s reload`

16. Masuk ke direktori html, kemudian kemudian copy file index nginx ke file index.html

`cd /var/www/html`

`cp index.nginx-debian.html index.html`


17. Edit file hosts
`sudo nano /etc/hosts`

![19](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/d45138fc-837f-402b-aad5-f0a3ec438490)![20](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/6dc40972-90aa-4ecd-9238-18eb99886fc6)

18. Cek sekilas apakah microservice 1 dan 2 sudah sesuai

`curl mcsv1.local`

`curl mcsv2.local`

19. Kembali ke file parent
20. Masuk kembali ke direktori nginx
`cd /etc/nginx/sites-enabled`

21. Edit file hosts, tambahkan paling bawah yaitu 
`10.0.3.198	mcsv1.local` dan 
`10.0.3.175	mcsv2.local`

![21-1 png](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/64ecdc23-b224-43e1-b60f-86fe83b0ae99)

22. Cek konfigurasi
`sudo nginx -t`

22. Reload konfigurasi
`sudo nginx -s reload`

23. Coba pada browser

![22 png](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/bc6a218e-8f5e-4d86-8378-1389ff6688d2)
![23 png](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/24b8e202-13be-4d20-ba46-8545e20662cd)
