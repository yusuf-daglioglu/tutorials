# JENKINS

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Jenkins

en üst seviyede "item" tanımlarız. item'ların çeşitleri:

- project (⟷ old-name:job): tanımlamalarımızı yürütecek olan süreçtir.

- pipeline: build sürecinin her defasında dinamik olarak build edildiğinde tanımlanabilen bir süreçtir. build'a başlamadan "Jenkinsfile" isimli dosyayı okur ve içindekilere göre build yapar.

- folder: diğer item'ları bir dizinde toplayabilmemizi sağlar.

- external job: Jenkins'den bağımsız process'lerin takibi için yapılmış özel bir durum

- multi branch pipeline: bu bir plug-in'in getirdiği item'dır. default gelmez. çekilen kod'un her branch'i için ayrı ayrı Jenkinsfile'ı çalıştırır.

Jenkins içinde bazı eklentiler varsayılan olarak yüklü gelir.

Her item "build" edilir.

Yeni bir item oluşturduğumuzda; bizden item'ın çeşidini seçmemiz istenir. buradaki çeşitler default mevcuttur bazıları ise Jenkins'e eklediğimiz plugin'ler tarafından sağlanır. Bazı item çeşitlerinin isminde Project keyword'ü yer alır. bu tamamen ismin bir parçasıdır. terminolojide project denilen bir kelime yoktur.

Bir item'ı seçtiğimizde ise yeni bir sayfada birçok ayar istenir. bu ayarlar gruplandırılmıştır. komple bir grup bir Jenkins eklentisi tarafından sağlanıyor olabilir. fakat default sunulan gruplara da eklentiler müdahale edebilir. eklentiler default grupların içine yeni form alanları ekleyebilir. bir form alanının sağ tarafında "?" simgesi vardır. ona tıkladığımızda o alan için kısa bilgi çıkar ve hangi eklentinin bu alanı buraya koyduğunu yazar. Eğer ? simgesi yoksa o alan default Jenkins ile geliyordur.

Hangi grupların ve hangi form alanlarının gösterileceği, item'ın çeşidine göre değişir. item oluşturulduktan sonra item'ın çeşidini değiştiremeyiz.

## 📌 Agent

Jenkins master'a, agent'lar bağlanır ve bu agent'larda task'ları koşabiliriz.

## 📌 node

master ve agent'ların her birine verilen genel isimdir. bir node, agent yada master olabilir.

## 📌 Stage

pipeline (Türkçe kelime anlamı: boru hattı) içindeki her iş grubuna verilen isimdir. örnek pipeline dosyamız ve içindeki stage'ler:

```js
pipeline {

    agent any

    stages {

        stage('Build') {
            steps {
                echo "build started"
                sh 'cd build_dir'
            }
        }

        stage('Test') {
            steps {
                sh 'javac source_dir'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker push *file.jar'
            }
        }
    }
}
```

## 📌 Upstream

Upstream: akış yönünün tersine.

bir item diğer bir item'ı tetikleyebilir. tetikleyen item'a, tetiklenen item'ın upstream'i denir. tabi tetiklenen projede downstream oluyor.

## 📌 view

tüm item'ların listelendiği ana sayfada item'lar gruplandırılabiliyor. her gruba "view" adı veriliyor.

## 📌 workspace directory

her item için bir dizin vardır. her build aynı dizin üzerinde işlem yapar. eğer "workspace temizleme" aktif edilmezse eski build'deki dosyalar sürekli orada kalır.

## 📌 build directory

her build bittiğinde metadata'larını (log, başlangıç ve bitiş saat bilgisi, istatistik bilgisi ve artifact'ları) build dizini içine kopyalar.

## 📌 JENKINS_HOME

- config.xml
- plugins/
- workspace/
  - [ itemName ]
- jobs/
  - [ itemName ]
    - config.xml   (job configuration file)
    - latest       (symbolic link to the last successful build)
    - builds/
    - [ buildId ]/

Bazı Jenkins sürümlerinde workspace dizini, her JENKINS_HOME/jobs/[ itemName ]/'in altındadır.

## 📌 Pipeline types

iki çeşit Pipeline var:

- `Scripted`: `Groovy` dili ile yazılmış dosyadan okuyan pipeline'lardır.

- `declarative`: özel üst seviyeli bir dil (arka planda `Groovy` kullanıyor). Scripted'a göre daha üst seviyeli olduğundan, Scripted'a göre daha kolay fakat daha az özellik sunuyor.

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
