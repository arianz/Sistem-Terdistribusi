# Load Balancing
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


## Konfigurasi Load Balance pada Hosts WSL
