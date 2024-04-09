# Load Balancing

Disini, saya akan membagikan langkah-langkah lanjutan sistem terdistribusi tugas 1. Adapun langkah-langkahnya sebagai berikut :

## Membuat dan kofigurasi LXC Microservice
1. Install Debian 10 (buster) pada Microservice 3,4,5 dengan
```
sudo lxc-create -n microservice[angka]-t download -- --dist debian --release buster --arch amd64 --force-cache --server images.linuxcontainers.org
```
2. Mulai jalankan Microservice dengan `sudo lxc-start -n microservice[angka]`
3. Cek status Microservice saat ini dengan `sudo lxc-ls -f`

Adapun hasilnya seperti berikut :

![1](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/4e86f4bf-80d3-42aa-a185-c01b4cf5bd06)

Dari gambar tersebut, kita dapat mengetahui IP untuk microservice 3,4,5
- 10.0.3.113/24 brd 10.0.3.255
- 10.0.3.95/24 brd 10.0.3.255
- 10.0.3.233/24 brd 10.0.3.255

4. Konfigurasi ke salah satu microservice baik 3,4 atau 5 dengan `sudo lxc-attach -n microservice[angka]`
5. Setelah itu, install beberapa tools pada masing-masing microservice

Tools tersebut diantaranya :

- nano dengan `apt install nano`
- curl dengan `apt install curl`
- nginx dengan `sudo apt install nginx nginx-extras`

Nb : sebelum install, lakukan update maupun upgrade dahulu dengan `apt update; apt upgrade -y`

6. Konfigurasi Hosts pada masing-masing mircroservice dengan `nano /etc/hosts`

Adapun berikut tampilannya

- Microservice 3

![2](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/1a590307-ec52-405f-83e5-bcc24289a4d3)

- Microservice 4

![3](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/b8ba3ed3-8b04-4985-8844-3b934ba2be57)

- Microservice 5

![4](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/23a962f7-3d8c-4c1d-bb88-18e43fba02bc)

7. Buka dan edit file index.nginx-debian.html dengan `nano /var/www/html/index.nginx-debian.html`

- Microservice 3

![7](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/424b2ea2-3547-4153-b661-723b37a0249c)

- Microservice 4

![8](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/b1d8eddd-9af9-4098-b696-665063908a65)

- Microservice 5

![9](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/580b8e84-8f53-4aa0-a07e-2d6b489c330a)

## Konfigurasi Load Balance pada Hosts WSL

1. Kembali ke direktori parents dan lakukan konfigurasi pada hosts parent dengan `nano /etc/hosts`

![5](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/1ad8e8e1-e16d-4517-b38b-bf39151da023)

2. Lakukan konfigurasi pada file sister.local dengan `sudo nano /etc/nginx/sites-enabled/sister.local`

![6](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/351578f5-903d-48c9-8e4b-923ef8613c4c)

3. Cek hasil konfigurasi pada Nginx dengan `sudo nginx -t`
4. Reload proses konfigurasi pada Nginx dengan `sudo nginx -s reload`
5. Setelah itu, kita cek apakah tampilan aplikasi sister.local sesuai dengan terminal dengan `curl -i app.sister.local`
6. Terakhir, lakukan cek tampilan aplikasi sister.local dengan mengetik "app.sister.local" pada browser

Adapun tampilannya sebagai berikut

- Output App sister.local microservice 3

![10](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/bbadd903-dd19-482e-96b6-dc38a6c7ac9d)

- Output App sister.local microservice 4

![11](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/8ce5e945-0cce-42c6-b812-65394713d57c)

- Output App sister.local microservice 5

![12](https://github.com/arianz/Sistem-Terdistribusi/assets/55643185/fbf58e0c-df1f-4550-a1b8-149f917657b9)
