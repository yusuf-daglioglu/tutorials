# VIRTUALIZATION

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Docker Registry

internette bulunan hazır container repo'su hizmetlerinin genel adıdır.

## 📌 Docker Hub

`Docker` firmasının sunduğu resmi `Docker Registry`sinin ismidir.

## 📌 Docker-compose

`YAML` konfigürasyon dosyasını parametre olarak alır ve ona göre docker komutlarını koşar.

## 📌 Docker-swarm

`swarm` Türkçe kelime anlamı: sürü

`container` orchestrator'dır.

## 📌 docker-stack

`docker-swarm` kullanarak `docker`'ları başlatan `docker-compose` alternatifidir.

## 📌 Universal Control Plane (⟷ UCP)

`docker-stack`'ten daha gelişmiştir. ekstra sundukları

- `Web UI`
- enterprise seviyesinde user ve role management
- arka planda `docker-swarm` veya `K8s` kullanabilir.

## 📌 UCP-agent (⟷ UCP node)

2 çeşittir:

- manager: `Web UI`'ı sunar ve worker'lara komut gönderir.

- worker: Docker container'larını çalıştırır.

manager özelliğine sahip bir node'da isterse container çalıştırır fakat bu pek tavsiye edilmez. sadece hızlı test için, deneme amaçlı kurulumlar yapılması için manager'ların container çalıştırması serbest bırakılmıştır.

manager ve worker node'ları aynı cluster'da birden fazla olabilir.

## 📌 UCP bundle (⟷ UCP client bundle)

`UCP` panelden download edilir. Bir `Zip` dosyasıdır. içinde gerekli anahtarlar ve komut satırı script'leri bulunur. bu script'ler kullanılarak direk `Docker` yüklü bir makinadan `UCP` `Web UI` panelindeymiş gibi komut satırından işlem yapılabilmesini sağlar.

## 📌 service

service bir container instance'ı değil. örneğin yukarıda 1 servise bağlı 5 instance oluşturduk. istersek hemen 10 tane daha ekleyebiliriz. servise yaptığımız ayar, o servise bağlı tüm container'lara otomatik yapılmaktadır.

servis tanımımızı güncellediğimiz zaman o servisten türemiş tüm container'lar kapanır ve yeni tanımlama ile tekrar açılırlar.

### 📌📌 service types

Global service: her yeni worker node eklendiğinde bu servislerden birer instance otomatik o worker'da açılmaktadır. firewall/antivirüs, monitor-agents gibi uygulamalarımız burada olabilir.

Replicated service: İlgili servisten kaç adet instance'ın çalışacağına sizin karar verdiğiniz servis tipidir.


## 📌 Dockerfile

örnek Dockerfile:

```dockerfile
FROM Ubuntu 
# Docker container ilk açıldığında, boş bir `file system` oluşturur. Burada bir OS kullanmak gerekli. Bu şekilde temel kütüphaneler `file system`'da bulunmuş olur.

ARGS PI = 3 
# container içinden bu değer okunamaz. komut satırından override edilebilir: "docker build --build-arg PI=4"

ENV ANDROID_HOME /my/dir
# yada ENV ANDROID_HOME=/my/dir. komut satırından override edilebilir: 
# docker run -e "ANDROID_HOME=/my/another/dir"

WORKDIR /directory1
# container içinde "cd /directory1" yapmış olduk.

COPY /source/dir:/dest/dir 
# container dışındaki bir dizini yada dosyayı container içine kopyalar

ADD /source/dir:/dest/dir 
# copy ile aynıdır. ekstra olarak ADD; eğer source URL ise onu indirir ve eğer source arşiv dosyası ise (tar, Zip) onu açıp kopyalar.

RUN apt-get install NodeJS 
# sadece "docker build" komutu ile çağrılır.

RUN echo "hello number $some_variable_name" 
# yada "hello number ${some_variable_name}".

CMD ["/usr/bin/node", "/var/www/app.js"] 
# sadece "docker run" komutu ile çağrılır.

ENTRYPOINT ["/usr/bin/node", "/var/www/app.js1", "/var/www/app.js2"]
# CMD ile aynı görevi görür. tek farkı Docker'ı run ederken, ek parametreler alabilir. "Docker run myImage /var/www/app.js3" komutu ENTRYPOINT'e 3üncü parametreyi ekleyecektir.
```

## 📌 userName/containerName vs containerName vs myhub.mysite.com:8081/myContainerName

Dockerfile'da her iki formatta da paket indirilebilir. userName olmadan indirilenler Docker tarafından resmi olarak tag'lenmiş anlamına gelmektedirler.

"Docker registry" için hazır paket yazılımlar bulunmaktadır. yani; self-hosted şekilde kendimiz için ayağa kaldırabiliriz.

## 📌 stop vs remove komutları

iki komutta container'ı durdurmaktadır. fakat remove komutu var olan state'i (container'ın `file system`'ını) silmektedir. oysa stop komutu state'i korumaktadır. bu şekilde state istenirse farklı amaçlar için saklanabilir.

## 📌 attach vs exec komutu

Docker içindeki root user'ına komut satırı üzerinden yönetimini sağlar. attach olduğumuzda yeni bir komut satırı oturumu açılmaz, onun yerine son çalıştırılan komutun çıktılarını görürüz. eğer yeni bir komut satırı istiyor isek; "docker exec -i -t 878978547 bash command arg1 arg2" ile yeni bir bash açabiliriz. exec komutu en sağda yazan komutu container üzerinde çalıştırmamızı sağlar.

## 📌 Docker build command

Dockerfile'ın image olarak hazır hale getirilmesini sağlar. run edip anında ayağa kalkması için build olması şart. örneğin; build sırasında Docker için gerekli dosyaları internetten indiriyor.

## 📌 Docker run command

build edilmiş bir image'yi run etmemize yarar.

-d parametresi eklenince container'ın ID'sini döndürüp arka planda çalıştırır. -d verilmezse komut satırında OS'un system.out log'larını basar.

arka planda çalışan container'ın system.out çıktılarını "Docker log" komutu ile de görebiliriz.

## 📌 Docker commit komutu

bu komut ile çalışmakta olan bir image, kopyalanıyor ve yeni bir image yaratılıyor. bu image'nin ID'si ekrana basılıyor. örneğin; MySQL image'si run edildi, daha sonra üzerine manuel olarak wget kuruldu. bu image kopyalanıp saklanılması isteniyor ise; commit komutundan yararlanılmalıdır. aslında yeni bir isimde klon image oluşturulmuş oluyor.

"commit" yerine Dockerfile dosyasının update edilmesi öneriliyor. çünkü image'in nasıl yaratıldığını daha ilerdeki zamanlarda net bilebiliriz.

## 📌 layer

build komutu sırasında container içinde yapılan her komut sonrası yeni bir katman yaratıyor.

## 📌 Docker diff command

"Docker diff containerId" komutu verildiğinde o containerId'nin yaratıldığı image-layer'ı ile şu andaki layer arasındaki `file system`'daki farkları listeleyecektir.

## 📌 kill vs stop command

stop container'ların içindeki tüm uygulamalara SIGTERM, kill komutu ise SIGKILL sinyalini yollar.

## 📌 -p parameter

container'ı run ederken port numarası mapping'i yapmak için kullanılır. örneğin;

> Docker run -p 80:8080 imageId

Bu şekilde host makinenin 80 portuna gelen istekler, Docker container içindeki 8080 portuna yönlendirilecektir.

## 📌 -v parameter

-p parametresi ile aynı formata kullanılıyor. fakat dizin bağlamak (mount etmek) için yapılıyor. örnek:

> docker run -v /home/host-user/dir:/home/Docker-user/dir imageId

## 📌 docker inspect

inspect yanına yazılan ID, herhangi bir kaynağın ID'si olabilir. örnek: volume ID, image ID, container ID, network ID, stack ID gibi.. tüm detaylı JSON formatında bize döndürür.

## 📌 docker --link

Docker container'ların birer IP si vardır. bu IP'ler birbirine erişebilmelidirler. bu yüzden host dosyasına DNS kaydı otomatik atılabilir. örnek:

> docker run --link myOtherContainerId:aliasOnDnsHostFile imageId

myOtherContainerId IP'si 10.0.0.1 ise; yeni yaratılan container içindeki host dosyasına bakıldığında bu satır görülecektir:

> 10.0.0.1 aliasOnDnsHostFile

## 📌 Docker hub'a image yollama

Docker login komutu ile komut satırından Docker'a login olunabilir. daha sonra herhangi bir image'ı push komutu ile Docker hub'daki hesabımıza yollayabilir.

## 📌 repository vs registry

repository bir Docker-image'sinin farklı sürümlerinin barındırıldığı sunucu yazılımıdır. tag ise Docker-image'nin sürümüdür. repository kelimesi birçok farklı Docker-image'ini barındırıyormuş gibi görünüyor, oysa aynı Docker-image'inin farklı sürümlerini barındırıyor. örneğin: repo:JDK tag:8.

registry ise birçok farklı Docker-image'sinin bulunduğu sunucu yazılımıdır. örnek "Docker hub", Docker Inc firmasının resmi registry'sidir ve default olarak Docker client'ta tanımlı gelir.

Yani 1 registry'de, birden fazla repository vardır. ama tersi doğru değildir. Örneğin; Docker Hub'da (registry), Java repository'si vardır.

## 📌 Docker ce

community edition (free)

## 📌 Docker ee

enterprise edition

## 📌 Docker edge vs stable

edge is beta version.

## 📌 Docker Trusted Registry (⟷ DTR)

locale kurulabilen registry.


## 📌 Docker temizlik

### 📌📌 Docker system prune

prune Türkçe kelime anlamı: budamak.

bu komut sırası ile şu komutları işletiyor:

- Docker container prune

- Docker image prune

- Docker network prune

- Docker volume prune

### 📌📌 Docker system prune -a

-a parametresi durmuş tüm kaynakların silinmesini sağlıyor. yani o sırada run edilenler haricindeki tüm kaynaklar siliniyor. hiçbir container run edilmezken çalıştırılırsa tüm kaynaklar silinir.

### 📌📌 Docker container prune

stop olmuş tüm container'ları siler.

### 📌📌 Docker image prune

dangling image'leri siler.

### 📌📌 Docker image prune -a

hiçbir container tarafından kullanılmayan image'leri siler.

### 📌📌 dangling volume

bir volume container run edildiğinde mount edilir. yani container ile ilişkilendirilir. eğer ilişkisiz bir volume var ise, bu volume dangling'dir. stop olmuş fakat silinmemiş container'lar hala volume'ler ile ilişkilidir.

### 📌📌 Docker volume prune

dangling volume'leri siler. bu komutun "-a" ile bir alternatifi yoktur. çünkü zaten container ile bağlantısı olmayan her volume dangling'dir.

### 📌📌 Docker network prune

volume ile aynı mantıktadır. bu komutun "-a" ile bir alternatifi yoktur. çünkü zaten container ile bağlantısı olmayan her volume dangling'dir.
