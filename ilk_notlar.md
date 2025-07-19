
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

Tarayıcıda http://localhost:8081 ya da http://127.0.0.1:8081 yazarsan, host makinedeki 8081 numaralı port, container içindeki web uygulamasına yönlendirilir.

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

olusturdugum firstvolume alpine container icinde  uygulama dosyasina baglandi. Container artik uygulama dosyasina kayit yapiyor gibi calisacak fakat firstvolume tarafina kaydedilecek.
 
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
Docker is an open-source platform that allows software to run in isolated environments called containers. Containers are lightweight structures that contain all dependencies of the application and run the same way everywhere.

Advantages:
- Lightweight and fast
- Dependency isolation
- Portability (runs the same in every environment)
- Suitable for microservice architectures
- Consistency in dev, test, and prod environments

2. KEY CONCEPTS
----------------------
Image: A read-only template with all configuration needed to create a container.
Container: A running instance of an image in an isolated environment.
Dockerfile: Script file used to create Docker images.
Volume: Used for persistent data storage. Data survives even if the container is removed.
Network: Enables containers to communicate with each other.
Registry: Server where Docker images are stored and shared (e.g. Docker Hub).

3. INSTALLING DOCKER ON LINUX (Ubuntu/Debian)
-----------------------------------------------
In the terminal, run the following:

sudo apt update
sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/docker.gpg
echo "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io

After installation:

sudo systemctl enable docker
sudo systemctl start docker

Check Docker version:
docker --version

To run Docker without sudo:
sudo usermod -aG docker $USER
exit
(then log back in or run 'newgrp docker')

4. BASIC DOCKER COMMANDS
------------------------------
docker version              # Show version info
docker info                 # Show system info
docker images               # List images
docker ps                   # List running containers
docker ps -a                # List all containers
docker pull <image>         # Download image
docker run <image>          # Run container
docker run -it ubuntu bash  # Interactive terminal
docker run -d -p 8080:80 nginx  # Port mapping
docker stop <id>            # Stop container
docker rm <id>              # Remove container
docker rmi <image_id>       # Remove image
docker logs <id>            # View logs
docker exec -it <id> bash   # Enter container shell

5. DOCKERFILE
----------------------
Example for a Python Flask app:

FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "app.py"]

Build image:
docker build -t myflaskapp .

6. VOLUMES (PERSISTENT DATA)
------------------------------
Create volume:
docker volume create mydata

Run container with volume:
docker run -v mydata:/data ubuntu

Mount local directory:
docker run -v $(pwd):/app ubuntu

7. NETWORK
------------------------------
Create network:
docker network create mynetwork

Run containers in the same network:
docker run -d --network=mynetwork --name web nginx
docker run -it --network=mynetwork ubuntu

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
docker-compose up
docker-compose down
docker-compose build

9. CLEANUP COMMANDS
------------------------------
Remove stopped containers:
docker container prune

Remove unused images:
docker image prune -a

Remove unused volumes:
docker volume prune

Remove unused networks:
docker network prune

Remove everything:
docker system prune -a --volumes

10. USEFUL ALIASES (for bashrc or zshrc)
-----------------------------------------
alias dps="docker ps"
alias dpa="docker ps -a"
alias dimg="docker images"
alias drm="docker rm \$(docker ps -aq)"
alias dclean="docker system prune -a --volumes -f"
alias dexec="docker exec -it"

11. EXTRA COMMANDS
------------------------------
Get container IP address:
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <id>

Copy file to container:
docker cp ./file.txt <id>:/target

Ping another container (must be in same network):
docker exec -it <id> ping <other_container_name>
