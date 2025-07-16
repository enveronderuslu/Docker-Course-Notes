
  git config --global user.name "Enver Onder Uslu"
  git config --global user.email "enveronderuslu@gmail.com"
  git clone https://github.com/enveronderuslu/huawei_DevOps_Kursu.git
  git version
  
   75  git push origin main 
   76  git push origin dev
   77  git pull origin main

//////////*******************////////////////////
sudo usermod -aG docker enver (her docker  dan önce sudo yazmak istemiyorsan bunu calistir sonra reboot yap)
//////////*******************////////////////////

docker version
docker info
docker  command detaylarini verir
Mesela managementt commands var.  örnegin
docker image --help  (image burada management command)

portainer.io  burada web araayüzü kullanarak one can compose docker

docker image rm --help
 örnek: docker container run --name firstcontainer  ozgurozturknet/app1

dockerhub tan ozgur… image ini cekti. lokalde firstco.. isimli bir container olusturup calistirdi 

container i arka planda calistirmak icin -d kullanilir

docker container run -d -p 80:80 --name  deneme  ozgurozturknet/adanzyedocker

docker  ps -a  calisaan calismayan tüm containerlari  listeler

docker container rm -f 236 861   bunlar id lerin ilk üc basamagi. hepini yazmaya gerek yok

docker container exec -it websunucu sh         Calisan bir container a exec komutuyla baglaniyoruz. Execute uygula Peki neyin üzerinde websunucu üzerinde . sh ise uygulama alpine ile yazildigindan o distronun shell ine baglanmak icin. image ubuntu tabanli olaydi bash kullanirdik. ( 
-i (veya --interactive)
Bu, etkileşimli modda çalışmanı sağlar. Yani terminal girişini açık tutar. Sen komut yazabilesin diye stdin’i (standart giriş) aktif hale getirir.

🔹 -t (veya --tty)
Bu da terminal (TTY) emülasyonu sağlar. Yani komutları çalıştırabileceğin bir terminal ekranı gibi davranır.

🔹 Kısacası -it birlikte şunu yapar:
Container içinde terminal açar, seninle iletişim kuracak şekilde aktif halde tutar. Özellikle bash veya sh gibi shell komutlarıyla içine girerken çok kullanılır.)

komut calistiktan sonra ps yazarsan cont icinde calisan uygulamalari  görürsün. Docker her zaman buradaki pid 1 uygulamayi gözler ve bunu calistirir. Mesela conta… shell de "kill  1" calistirsan direk seni ana sshele atar. container durur. docker ps ile kontroö et. -a yazma duran durmayan hepini gösterir. Docker start deyince tekrar calismaya baslar

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
/

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
