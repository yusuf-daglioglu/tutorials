# VIRTUALIZATION JENKINS

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Jenkins'i Docker ile kurup job içinde Docker kullanma

Docker içerisinde Docker kullanma konusuna girmekteyiz. bunun için 3 yöntem var:

- __dockerd'nin socket dosyasına erişerek__

container'ımız dışındaki Dockerd'ye bağlanmamız gerekli.

Jenkins'i başlatırken dockerd unix socket dosyasını bağlamamız gerekli:

```sh
docker container run -d -p 8080:8080 -v /var/run/docker.sock:/var/run/docker.sock jenkins-docker
```

Jenkins-Docker içerisinde, Docker client'ın kurulu olması gerekli.

"/var/run/docker.sock" bir UNIX socket dosyasıdır. jenkins dışındaki "Dockerd" sürekli buradan dinleme yapar ve buradan gelen komutları kabul eder.

Bu madde tavsiye edilmiyor, çünkü sıkıntı yaratıyor. kaynak: http://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/ "The solution" başlığı.

- __dockerd API kullanmak__

Docker client ile container dışında olan bir Dockerd'ye istek atarız.

bu madde tavsiye edilen çözümdür. kaynak: http://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/ tüm makaledeki en son satır.

- __sysbox__

direk nested-container yapısını destekleyen container kullanmak.

en modern çözüm.

- __DIND (⟷ Docker in Docker)__

Docker içerisinde Docker'ı kurarız.

bunu yapan hazır bir Docker image'si mevcut: https://github.com/docker-library/docker

Docker'a "privileged" parametresi ile yetki verilir. Bu açılan docker'a fazla yetki verir ve güvenliği düşürür.

Bu madde zorluklar çıkarıyor. kaynak: http://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/
