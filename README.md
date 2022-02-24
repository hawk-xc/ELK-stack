# ELK-stack

ELK-stack merupakan singkatan dari 3 open source proyek keamanan jaringan komputer, ELK-stack merupakan singkatan dari Elasticsearch, Logstash, dan Kibana. 3 proyek open source ini memiliki fungsi yang berbeda tapi tergabung dalam satu kesatuan.

### prerequire

- A Linux system running Ubuntu 20.04 or 18.04
- Access to a terminal window/command line (Search > Terminal)
- A user account with sudo or root privileges
- Java version 8 or 11 (required for Logstash)

### Install Dependencies ;;

Sebelumnya masuk ke account super user **(sudo)** dengan perintah

```bash
sudo su
```

ELK stack membutuhkan java 8 atau 11, versi tersebut support dengan berbagai package ELK. cara installnya sebagai berikut

```bash
apt-get install openjdk-11-jdk -y
```

oke, setelah itu silakan teman-teman install web server, disini saya akan mencoba menggunakan nginx web server. install dengan perintah

```bash
apt-get install nginx -y
```

### Install elasticsearch & configure ;;;

Sebelum menambahkan repository, disini silakan teman-teman menambahkan GPG-key.

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```

next install `apt-transport-https` package.

```bash
sudo apt-get install apt-transport-https
```

tambahkan elasticsearch repository berikut

```bash
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee –a /etc/apt/sources.list.d/elastic-7.x.list
```

baiklah langkah selanjutnya sebelum install package elastic silakan teman-teman update terlebih dahulu repository dengan perintah.

```bash
apt-get update
```

setelah itu install

```bash
sudo apt-get install elasticsearch
```

langkah selanjutnya, kita akan melakukan konfigurasi file elastic difile elasticsearch.yml

```bash
sudo nano /etc/elasticsearch/elasticsearch.yml
```

pada file konfigurasi diatas, silakan teman-teman hilangkan tanda pagar ( **#** ) pada awal syntak dan ubah network.host menjadi **== localhost** seperti berikut.

```
network.host: localhost
http.port: 9200
```

sebelumnya sebelum disimpan tambah-kan syntak baru dibawah, setelah itu simpan dengan _CTRL + X_

```
discovery.type: single-node
```

okee next, sekarang disini kita akan melakukan konfigurasi memory heap pada RAM, disini kita akan melakukan kontrol terhadap memory RAM yang akan dikonsumsi oleh elastic.

```bash
sudo nano /etc/elasticsearch/jvm.options
```

hapus tanda pagar, dan ubah `Xms` dan `Xmx` menjadi 512m. lalu save.

```
-Xms512m
-Xmx512m
```

jika sudah sekarang kita akan menghidupkan service dari elasticsearch.

```bash
systemctl start elasticsearch
systemctl enable elasticsearch
```

### Install Kibana & configure ;;;;


