
git config --global user.name "Enver Onder Uslu"
git config --global user.email "enveronderuslu@gmail.com"

sudo usermod -aG docker enver sonra reboot yap

docker version
docker info
docker  command detaylarini verir
Mesela managementt commands var.  örnegin
docker image --help  (image burada management command)

portainer.io  burada web araayüzü kullanarak one can compose docker

What is docker imaji? bir linux dagitimi icinde cekirdek olmayani. Bir Blueprint. Her yerde bi comtainer olusturursun

docker image rm --help

docker container run --name firstcontainer  ozgurozturknet/app1

container i arka planda calistirmak icin -d kullanilir

docker container run -d -p 8081:80 --name  deneme  ozgurozturknet/adanzyedocker

Açıklama:
 
-p <host_port>:<container_port>
Yani:

Soldaki 8081 → senin Kali (host) sistemindeki port 80

Sağdaki 80 → container içindeki port 80

Ne işe yarar?
Container içinde çalışan uygulama (örneğin bir web sunucusu), 80 numaralı portu kullanıyorsa…

Bu seçenek sayesinde:

Tarayıcıda http://localhost:8081 ya da http://127.0.0.1:8081 yazarsan, host makinedeki 8081 numaralı port, container içindeki web uygulamasına yönlendirilir. hostun 8081 portuna gelenleri containerin 80 portuna yönlendirilir

docker  ps -a  calisaan calismayan tüm containerlari  listeler
docker container rm -f 236 861   bunlar id lerin ilk üc basamagi. hepini yazmaya gerek yok

docker container exec -it websunucu sh 

-it birlikte şunu yapar: Container içinde terminal açar, seninle iletişim kuracak şekilde aktif halde tutar. Özellikle bash veya sh gibi shell komutlarıyla içine girerken çok kullanılır.

dteached calisan bi container in loglari "docker logs container_adi" ile görülebilir

"docker container run alpine ls" alpine container da default uygulama yerine "ls" calistirir

docker volume inspect alistirmavol

docker container run --name cont4  -p 8080:8080  -it -v alistirmavol:/test alpine sh alpin den cont4 isimli bir container  olusturup it ile icine sh ile girip alistirmavol isimli volume u test klasörüne bagladik

alistirmavol:/test:ro  yazarsan dosyayi sadece okuyabilirsin

"docker container run --name  websitesi1 -d -p 80:80  -v /home/kali/Desktop/websunuc:/usr/local/apache2/htdocs                     ozgurozturknet/adanzyedocker
"
volume olusturmadan masaüstünde bir dosyayi bagladim. masaüstündeki dosyada ne yaparsam artik container icinde  o görünecek

  
docker container run -it -v firstvolume:/uygulama alpine sh

olusturdugum firstvolume alpine container i icinde  uygulama dosyasina baglandi. Container artik uygulama dosyasina kayit yapiyor gibi calisacak fakat firstvolume tarafina kaydedilecek.
 
docker container run --rm -it -v secvol:/sectest ozgurozturknet/adanzyedocker sh
burada --rm : container kapatilinca otomatik sil denmek

docker container run -d -p 80:80 -v  /home/kali/Desktop/bolum28/websitesi:/usr/share/nginx/html nginx
burada nginx html ve lokaldeki websitesi dosyalarini mount ettim ve artik icerigi lokalden cekiyor. zirt pirt iceri girmek zorunda kalmiyorum
//////////****************///////////////

docker system prune -a herseyi siler. calisir vaziyettikileri stop etmelisin

cat /etc/os-release

DOCKERFILE

bir K klasöründe bir index.html file olusturulur. Sonra asagidaki dockerfile olusturulur;
"""
FROM nginx:latest (buradaki base image degistirilecek)

COPY ./index.html /usr/share/nginx/html/index.html

EXPOSE 80
"""
sonra """ docker build  -t enver . """ calistirilir ve enver:latest  image olusturulur ve image lar listesine eklenir. 

///////////****************///////////////
 docker container (image, network, volume) inspect container_adi
objeyle ilgili detayli bilgiler verilir










DOCKER 101 (LINUX)

1. WHAT IS DOCKER?
----------------------
Docker software to run in isolated environments called containers. 

2. KEY CONCEPTS
----------------------
Image: A read-only template with all configuration needed to create a container  
Container: A running instance of an image in an isolated environment  
Dockerfile: Script file used to define and build Docker images  
Volume: Persistent storage for data that survives container removal  
Network: Allows communication between containers  
Registry: A server to store and share Docker images (e.g., Docker Hub)

4. BASIC DOCKER COMMANDS
------------------------------
docker version              # Show Docker version  
docker info                 # Display system-wide information  
docker images               # List available images  
docker ps                   # List running containers  
docker ps -a                # List all containers (including stopped ones)  
docker pull <image>         # Download an image from a registry  
docker run <image>          # Run a container from an image  
docker run -it ubuntu bash  # Start interactive terminal inside a container  
docker run -d -p 8080:80 nginx  # Run container in background  
docker stop <id>            # Stop a running container  
docker rm <id>              # Remove a container  
docker rmi <image_id>       # Remove an image  
docker logs <id>            # Show logs from a container  
docker exec -it <id> bash   # Access a running container's shell

5. DOCKERFILE
----------------------
Example Dockerfile for a Python Flask app:

FROM python:3.9-slim  
WORKDIR /app  
COPY requirements.txt .  
RUN pip install -r requirements.txt  
COPY . .  
CMD ["python", "app.py"]

Build the image:
docker build -t myflaskapp .

6. VOLUMES (PERSISTENT DATA)
------------------------------

Docker Volume & Bind Mount Notes
------------------------------
Create a volume with:
docker volume create firstvolume
List existing volumes:
docker volume ls
View detailed info:
docker volume inspect firstvolume
Pay special attention to the Mountpoint field—this shows where the volume is stored on the host system.

Attaching a Volume to a Container
docker container run -it -v firstvolume:/the_folder_in_container <image> sh
firstvolume: the volume name on the host.

/the_folder_in_container: the mount path inside the container (automatically created if it doesn’t exist).

Deleting and recreating the container won’t delete your data—mounting the same volume to the same path preserves it.

One volume can be shared by multiple containers simultaneously.

-v firstvolume:/the_folder_in_container:ro

Bind Mounts
Instead of a Docker-managed volume, you can mount a host folder directly into the container:

docker run -p 8081:80 \
  --name some-nginx \
  -v /home/kali/Desktop/HTMLCSS/CSS_101:/usr/share/nginx/html:ro \
  -d nginx
/home/kali/Desktop/HTMLCSS/CSS_101: a directory on the host.

/usr/share/nginx/html: the mount point inside the container.

-d runs the container in detached (background) mode.Mount current local 
Example: directory into container:
docker run -v $(pwd):/app ubuntu

7. NETWORK
------------------------------
docker network ls 
docker network inspect bridge
docker container run -it --net host  --name deneme  ozgurozturknet/adanzyedocker
böylece default yerine host network kullanildi. Böylece izolasyon kalkti ve üzerinde calistigi makineyle ayni agdaymis gibi calisti.

CASE STUDY
srever da container lari gruplamak ve gruplar arasinda keine Verbindung istiyorum. O zaman birden fazla bridge network gerekir.Ayrica default bridge network icinde birbirlerinin isimlerini cözemezler. Napalim AQ?

Create a custom network:
sunmet1/2 olusturp iclerine  websunucu1/2 va database1/2 koyacaz
docker network create subnet1 
docker network ls
docker network create subnet2
docker network inspect subnet1
docker network inspect subnet2

----subnetler olusturuldu

docker container run -dit --name  websunucu1 --net subnet1  ozgurozturknet/adanzyedocker
docker container run -dit --name  database1 --net subnet1  ozgurozturknet/adanzyedocker 
docker container exec -it websunucu1 bash
docker container run -dit --name  websunucu2 --net subnet2  ozgurozturknet/adanzyedocker
docker container run -dit --name  database2 --net subnet2  ozgurozturknet/adanzyedocker

docker container exec -it websunucu2 sh
ping database2 calisir
ping websunucu 1 calismaz. farkli networkteler

bir container üretilen bir subnet e baglanurmi
docker network connect subnet1 websunucu2 (disconnect de mümkün)

artik websunucu2 mknesi subnet1 icinde de görünüyor. 

8. DOCKER COMPOSE
------------------------------
Example docker-compose.yml:

version: "3"  
services:  
  app:  
    build: .  
    ports:  
      - "5000:5000"  
  redis:  
    image: "redis:alpine"

Commands:
docker-compose up             # Start services  
docker-compose down           # Stop and remove services  
docker-compose build          # Build services from Dockerfile

9. CLEANUP COMMANDS
------------------------------
Remove stopped containers: docker container prune

Remove unused images: docker image prune -a

Remove unused volumes: docker volume prune

Remove unused networks: docker network prune

Remove everything not in use: docker system prune -a --volumes

10. USEFUL ALIASES (for .bashrc or .zshrc)
-------------------------------------------
alias dps="docker ps"  
alias dpa="docker ps -a"  
alias dimg="docker images"
  
alias drm="docker rm \$(docker ps -aq)"  
ACHTUNG AQ: Burada \ isaretini tirnak icinde kullanirsin . Dolar isaretu düz metin olarak algilanmali. Normal kod olarak calistiracakasan: docker stop $(docker ps -aq)   ve sonra docker rm $(docker ps -aq) calstirirsin.  

alias dclean="docker system prune -a --volumes -f"  
alias dexec="docker exec -it"

11. EXTRA COMMANDS
------------------------------
Get container IP address:
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <id>

Copy a file into a container:
docker cp ./file.txt <id>:/target

Ping another container (in the same Docker network):
docker exec -it <id> ping <other_container_name>
