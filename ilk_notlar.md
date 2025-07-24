
[Ansible, Teraform, Openshift, OpenStack]
git config --global user.name "Enver Onder Uslu"
git config --global user.email "enveronderuslu@gmail.com"

sudo usermod -aG docker enver sonra reboot yap

container i arka planda calistirmak icin -d kullanilir

-it birlikte şunu yapar: Container içinde terminal açar, seninle iletişim kuracak şekilde aktif halde tutar. Özellikle bash veya sh gibi shell komutlarıyla içine girerken çok kullanılır.

docker container run --name cont4  -p 8080:8080  -it -v alistirmavol:/test alpine sh alpin den cont4 isimli bir container  olusturup it ile icine sh ile girip alistirmavol isimli volume u test klasörüne bagladik

docker container run --rm -it -v secvol:/sectest ozgurozturknet/adanzyedocker sh
burada --rm : container kapatilinca otomatik sil denmek

docker system prune -a herseyi siler. calisir vaziyettikileri stop etmelisin

1. WHAT IS DOCKER?
----------------------
Docker software to run in isolated environments called containers. 
Build Once, Run Anywhere
2. KEY CONCEPTS
----------------------
Image: A read-only template with all configuration needed to create a container  
Image: Bir linux dagitimi icinde cekirdek olmayani. Bir Blueprint. Her yerde bi comtainer olusturursun
Container: A running instance of an image in an isolated environment  
Dockerfile: Script file used to define and build Docker images  
Volume: Persistent storage for data that survives container removal  
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

6. VOLUMES & Bind Mount
------------------------------
Create a volume with: docker volume create firstvolume
List existing volumes: docker volume ls
View detailed info: docker volume inspect firstvolume
Attaching a Volume to a Container
docker container run -it -v <vplume_name_Host>:/folder_in_container <image> sh
"-v firstvolume:/the_folder_in_container:ro" seklinde de olur
One volume can be shared by multiple containers simultaneously.

You can mount a host folder directly into the container:

docker run -p 8081:80 --name some-nginx \
-v ~/Desktop/HTMLCSS:/usr/share/nginx/html:ro  -d nginx
 
Example: docker run -v $(pwd):/app ubuntu

docker container run -d -p 80:80 -v  /home/kali/Desktop/bolum28/websitesi:/usr/share/nginx/html nginx
burada nginx html ve lokaldeki websitesi dosyalarini mount ettim ve artik icerigi lokalden cekiyor. zirt pirt iceri girmek zorunda kalmiyorum

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
docker network inspect subnet1  subnet2

docker container run -dit --name  websunucu1 --net subnet1  ozgurozturknet/adanzyedocker
docker container run -dit --name  database1 --net subnet1  ozgurozturknet/adanzyedocker 
docker container exec -it websunucu1 bash
docker container run -dit --name  websunucu2 --net subnet2  ozgurozturknet/adanzyedocker
docker container run -dit --name  database2 --net subnet2  ozgurozturknet/adanzyedocker
veya iceri gireceksen
docker container exec -it websunucu2 sh 
ping database2 calisir
ping websunucu 1 calismaz. farkli networkteler

bir container üretilen bir subnet e baglanurmi
docker network connect subnet1 websunucu2 (disconnect de mümkün)


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

10. ENVIROMENT VARIABLES
-------------------------------------------
isletim sistemi düzeyinde tanimli ve her yerden cagrilabilen degiskenler.
piton code daki variable sadece orada gecerli lokal. 

Get-ChildItem Env: windowstaki variables verir
örnegin HOME variable \Users\enver dir. Yani " cd $HOME " ile home directory olan yere gidersin 
Env:mekan="C:\Users\enver\Desktop"
Peki linuxta?  printenv ile hepini cagir. yenisini olusturmak istieisen '└─# export testcik="/home/kali/Desktop/envanter"'.  Permanent olmasi  icin bunu .bashrc icine yazman  gerekir-
Docker da nasil olustururum: 
docker run -it -d --env VAR1=deneme1 ubuntu
docker run -it  --name mycon  --env-file /home/kali/envlist  ubuntu sh  Burada birden fazla degisken dosya icine yazilarak otomatik atanir


11. USEFUL ALIASES (for .bashrc or .zshrc)
-------------------------------------------
alias dps="docker ps"  
alias dpa="docker ps -a"  
alias dexec="docker exec -it"
alias drm="docker stop \$(docker ps -aq)"
alias drm="docker rm \$(docker ps -aq)"  (ACHTUNG AQ: Burada \ isaretini tirnak icinde kullanirsin . Dolar isaretu düz metin olarak algilanmali. Normal kod olarak calistiracaksan)

12. EXTRA COMMANDS
------------------------------
docker container (image, network, volume) inspect <id> objeyle ilgili detayli bilgiler verilir
Copy a file into a container:
docker cp ./file.txt <id>:/target

Ping another container (in the same Docker network):
docker exec -it <id> ping <other_container_name>

container icine giris cikis zor diye 
docker container logs <cont.name>
docker container top <container_name>
docker container stats (host üzerinde calisan tüm containerlat)
docker container stats <con> (host üzeriindeki con containeri )
--memory=limit ile ram kullanimini sinirlayabilirsin. 
ram kullanimi asarsa swap icin yer ayarlanabilir --memory-swap=limit
--cpus="1.5" sadece 1.5  tane cpu kullanacak