# VIRTUALIZATION

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 app container vs system container (⟷ OS Container) vs Pod vs init container vs sidecar container

### 📌📌 terminoloji

`container` 1 process tetiklenerek başlatılır. Eğer bu process kapanırsa, `container` otomatik kapanır. Fakat ilgili process başka process'ler başlatabilir. Ama `container` asıl ilk process'i takip eder. ona göre kapanıp kapanmayacağına karar verir.

Eğer biz birden fazla bağımsız process çalıştıracak isek ve process'lerin tümü kapanmadan `container` kapanmasın istiyorsak, o zaman `init process` kullanmak zorundayız.

- `app container` sadece bir process içerir. sadece ilk process başka process açabilir.

  `system container` ise aynı `container` içinde birden fazla process içerebilir. bunu sağlarken de `init process` kullanmak zorunda değiliz (istersek kullanabiliriz).

  `kaynak: https://containerjournal.com/features/system-containers-vs-application-containers-difference-matter/ "System Containers" başlığı 1inci paragraf.`

  `kaynak: https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux_atomic_host/7/html/managing_containers/running_system_containers "System containers provide a way to containerize services that need to run before the Docker daemon is running".`

  `kaynak: https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction "Operating System Containers" başlığı.`

- `system container` ile `OS Container` eş anlamda kullanılır. `kaynak: https://docs.jelastic.com/what-are-system-containers 1inci paragraf, ilk cümle.`

- `pod` içinde birçok process var ama bu process'lerin her biri ayrı `container`'larda çalıştırılıyor. bunları manage eden özel yapıya `pod` deniliyor.

  Aslında `pod` şöyle çalışıyor: multi `container` başlatıyor, ama bazı namespace'leri ortak veriyor. Yani `Linux` çekirdeği tarafında `container` dışında özel olarak `pod` diye bir kavram yok.

  `pod` yapısına `pause container (⟷ sandbox container ⟷ infra container)` denir.

  `K8s` ise `pod` yapısını `parent container` olarak isimlendirir.

  `pod` teknolojisi çıkınca ile `system container` terimi:

  - microservice dünyasında rafa kalktı.
  - fakat farklı durumlarda kullanılmaktadır (başka başlıkta anlatılıyor)

- `system container` platformları:
  - `BSD jails`
  - `Linux vServer`
  - `Solaris Zones`
  - `LXD`
  
  `kaynak: https://docs.jelastic.com/what-are-system-containers son paragraf.`

- `init container` ve `sidecar container` `K8s`'in terminolojisinde vardır. bunlar multi process veya içindeki process'in nasıl çalıştığı ile ilgili termler değildir. Örneğin;
   
   - `init container`, ona ilişkilendirilmiş olan bir container başlamadan önce otomatik başlatılır.
   - `sidecar container`, her zaman ilişkilendirilmiş olan container ile aynı `Pod` içinde koşmak zorundadır.

### 📌📌 aynı container'da birden fazla process açmak için sebepler

#### 📌📌📌 POD çözümü yeten durumlar

aşağıdaki örnekler için `POD` çözümü yeterli. sadece ek `namespace` ayarı gerekebilir.

not: `kubernetes` ve `podman`, `pod` içindeki `container`'lara farklı `namespace`'leri paylaştırıyorlar default'ta.

- `container` içinde `SSH` server açabilmek için.

  `kaynak: https://docs.jelastic.com/what-are-system-containers 2inci paragraf.`

- `crond`

  `kaynak: https://docs.jelastic.com/what-are-system-containers 2inci paragraf.`

- `Syslogd`

  `kaynak: https://docs.jelastic.com/what-are-system-containers 2inci paragraf.`

- `sidecar` pattern'i uygulayabilmemiz için.

  tüm `service mesh` teknolojileri buna bağlıdır.

  `kaynak: https://ahmet.im/blog/minimal-init-process-for-containers/`

#### 📌📌📌 POD çözümü yetmeyen durumlar

- `container` içinde bir web tarayıcısı açmak istiyoruz. böyle bir durumda container'ın `system container`'ı olması gerekir.

  bu iş `pod` ile de yapılabilir. fakat `pod` içindeki neredeyse tüm `container`'ları ortak `namespace` ile çalıştırmış oluruz. bu da zaten `system container` ile aynı kapıya çıkar.

- `container` içinde sürekli bir process'imiz açık kalmayabilir. örneğin `distrobox` ile development amaçlı local çalışmalar yaparken.

  bu iş `pod` ile de yapılabilir. fakat `pod` içinde `init process` kullanmak durumundayız. bu da zaten `system container` ile aynı kapıya çıkar.

## 📌 container runtime

- `LXC`

  `Linux Containers`'tan türeyen özel bir isimdir.

  `Linux` kernel modülü değildir.

- `Libcontainer`
- `Runc`
- `sysbox`

  `Runc`'nin fork'u.
  
  nested container support için geliştirilmiştir.

  Örneğin `Docker`, `sysbox`'ı kullanarak nested container açabilir:

  ```sh
  docker run --runtime=sysbox-runc ubuntu
  ```

- `Crun`
- `Railcar`
- `Katacontainers`
- `Containerd`

  `Runc`'yi wrap ediyor.
  
  `ctr` komut satırı client uygulamasıdır.

yukarıdaki liste için kaynak: `https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction "Container Runtime" başlığı.`

Tüm `container runtime`'ler, `Linux`'un içinde native gelen "`cgroups`" ve `Linux namespaces` özelliklerini kullanarak çalışır. Ek olarak `SELinux` ve `AppArmor` gibi özelliklerden de sıkça yararlanır.

## 📌 Open Containers Initiative (⟷ OCI)

`OCI`'nin referans implementasyonu `Runc`'dir.

`container engine`, `OCI` ile uyumlu ise, `OCI`'nin tüm implementasyonu olan `container runtime`'larını çalıştırabilir.

## 📌 Container engines

`LXC` gibi özelliklerin son kullanıcı tarafından direk kullanılması zordur çünkü alt seviyelidir. Bu yüzden `Docker` ve `LXD` gbi projeler ortaya çıkmıştır.

| `container engine` name | company                                      | based on `container runtime`                                            |
|-------------------------|----------------------------------------------|-------------------------------------------------------------------------|
| `Docker`                | `Docker`                                     | Sırası ile bu teknolojileri kullandı: 1-`LXC` 2-`Libcontainer` 3-`Runc` |
| `LXD`                   | `canonical`                                  | `LXC` which is also develop by `Canonical`                              |
| `Rkt (⟷ Rocket)`        | `Red Hat` (`CoreOS Team`)                    |                                                                         |
| `CRI-O`                 | `Cloud Native Computing Foundation (⟷ CNCF)` | `Runc`                                                                  |
| `Podman`                |                                              | `Runc`                                                                  |

yukarıdaki liste için kaynak: `https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction "Container Engine" 1inci paragraf.`

## 📌 Container Orchestration

`container engine`'leri wrap eder.

- `K8s`
- `Istio` (based on `K8s`)
- `OpenShift` (based on `K8s`)
- `Swarm` (develop by `Docker`)
- `UCP (⟷ Universal Control Plane)` (develop by `Docker`)
- `Mesos` (develop by `Apache`)

yukarıdaki liste için kaynak: `https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction "Container Orchestration" başlığı son paragraf.`

## 📌 container runtime vs container engine

aşağıdaki tüm bilgiler buradan kopyalanmıştır: `https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction "Container Runtime" başlığı ve "Container Engine" başlığı.`

The `container runtime` is responsible for:

- Consuming the container `mount` point provided by the `Container Engine` (can also be a plain directory for testing)
- Consuming the container metadata provided by the `Container Engine` (can be a also be a manually crafted `config.json` for testing)
- Communicating with the kernel to start containerized processes (clone `system call`)
- Setting up `cgroups`
- Setting up `SELinux` Policy
- Setting up `App Armor` rules

the `container engine` is responsible:

- Handling user input
- Handling input over an API often from a `Container Orchestrator`
- Pulling the Container Images from the Registry Server
- Expanding decompressing and expanding the container image on disk using a Graph Driver (block, or file depending on driver)
- Preparing a container `mount` point, typically on `CoW` storage (again block or file depending on driver)
- Preparing the metadata which will be passed to the container `Container Runtime` to start the Container correctly
  - Using some defaults from the container image (example: `ArchX86`)
  - Using user input to override defaults in the container image (example: `CMD`, `ENTRYPOINT`)
  - Using defaults specified by the container image (example: `SECCOM` rules)
- Calling the `Container Runtime`

## 📌 Podman

`Pod Manager`'in kısaltmasından özel isim türetilmiştir.

`Podman`; resmi sitesinden `daemonless container engine` olarak geçiyor. çünkü bir engine'e ihtiyacı yok. zira kendisi (sadece client komut satırı uygulaması) `Linux`'un içerisinde gelen teknolojileri kullanıyor. Dolayısı ile ek kurulumda gerektirmiyor.

root yetkisiz de process açabiliyor (tabi kısıtlı özellikler sunarak).

`Podman`, `pod` ve `container` olarak 2 farklı seçeneği de destekliyor.

## 📌 Linux namespaces

bazı kaynaklarda sadece `namespace` olarak geçer. bu sebeple karışıklığa sebep olabilir. zira `namespace` kelimesi birçok farklı alanda kullanılmaktadır.

`Linux` üzerinde proseslerin hangi kaynakları diğer prosesler ile ortak kullanıp kullanamayacağını belirleyen native `Linux` özelliğidir. örnek `Linux namespaces` type'lar;

- `Mount namespaces`

  bir grup proses, spesifik bir dizini görebilirken, diğer hiçbir proses göremeyebilir.

- `Network namespaces`

  birden fazla network arayüzü (`eth`, `Wi-Fi` gibi) birden fazla network namespace'sine bağlanır. network namespace'si iptable gibi routing ayarlarını içerir.

- `IPC namespaces`

  bir `IPC namespace`'si içindeki process, diğer `IPC`'lere mesaj atamaz.

- `PID namespaces`

  process ID grubu oluşturuluyor. bu şekilde, X pid namespace'indeki process'ler sadece birbirlerini görebiliyor.

- `uts namespaces (⟷ UNIX Time-Sharing namespaces)`

  domain ve host name'leri okurken farklı gruplar yaratabiliyoruz

- `User namespaces`

  bir process başlatıldığında o process'in kendini `X` user'ı ve `Y` grubu altındaymış gibi sanmasını sağlayabiliyoruz.

  Bu yetenek aynı zamanda sandbox özelliğini de getirdi. Çünkü yeni user ve grup açtıktan sonra artık burada özgürüz ve istediğimiz namespace'i kapatabiliyoruz. yani; unshare komutu; eğer yeni user-namespace yaratıldıysa, root yetki olmadan network'ü kapatabilir. fakat aynı komut ile yeni user-namespace yaratılmamış ise, network'ü ancak root yetkisi ile kapabiliriz.

- `time namespaces`

  her farklı time nameSpace'inde olan process'ler sistem saatini farklı algılıyor. (bu namespace tipi 2020 ile Linux 5.6'ya geldi)

- `cgroup namespaces` vs `cgroup (⟷ control group)`

  __cgroup__ Linux üzerinde proseslerin CPU, memory, network gibi kaynaklardan ne kadar öncelikli yararlanabileceği, yada limitli yararlanabilmeleri için yönetimi sağlayan Linux çekirdeğinin native özelliğidir.

  __cgroup namespaces__ bir process'in cgroup grubuna dahil olduğunu belirler.

## 📌 Linux namespaces nasıl çalışır?

```text
/proc/<PROCESS_ID>/ns/
```

dizini altında o process'in tüm namespace'leri ayrı ayrı link dosyalarıdır:

```text
/proc/<PROCESS_ID>/ns/ipc
```

```text
/proc/<PROCESS_ID>/ns/mnt
```

İşte bu dosyalar link olduğundan, örneğin X namespace'sine link eder. İşte o process'i, `Y` namespace'sine yönlendirirsek o zaman container yapmış oluruz.

`Linux namespaces`'leri `system call`'lar ile yapılır ve programatik olarak yapmak için `system call` yapan kütüphaneye ihtiyacımız vardır. örneğin `C`'de bunu yapmak için `clone` fonksiyonu kullanılabilir (başka başlıkta anlatılıyor).

## 📌 sandbox vs container

container içinde uygulamalar çalıştırabilmek için (yani yeni namespace'ler yaratabilmek için) root yetkisine ihtiyaç vardır. fakat sandbox için root yetkisine gerek yoktur. örnek Docker root yetkisi ister, fakat Chromium based tarayıcılar için sandbox için user yetkisi yeterlidir.

Chromium-based tarayıcılar sandbox kullanılır. Başka başlıkta anlatılıyor.

## 📌 Idle process

`init process`'ten tamamen farklı ve bağımsız programlardır.

`CPU` donanımsal mimarisi gereği boşta bekleyemez. Sürekli bir işlem yapmak zorundadır. Bu sebeple sonsuz döngülü bir program, `OS` tarafından arka planda (eğer başka bir process çalışmıyorsa) çalıştırılır. Bu process'e genel olarak `idle process` denir. `MS Windows` dünyasında process'in özel bir ismi vardır: `System idle process (⟷ sistem boşta işlemi)`.

`Linux`'ta process ID'si 0'dır.

## 📌 init process

`OS` servis ve program yönetimi yazılımlarına verilen genel isimdir. sistem başlangıcında ilk başlayan işlemdir. bu sebeple `init process` olarak da adlandırılırlar. tüm servisler ve process'lerin yönetimini sağlar. `OS` çekirdeği tarafından başlatılır.

process ID'leri 1'dir.

`init process`'ler bir `OS` içerisinde birkaç tane olabilir. bunun için bazı örnek durumlar:

- her `container` içerisinde `init process` olabilir
- her `OS` user'ı için ayrı `init process` olabilir. `kaynak: kaynak: http://man.he.net/man8/init "NOTES" başlığı, 1inci paragraf.`

Unix dağıtımlarda ilk geliştirilen `init process`, `System V` ismi ile dağıtılan dağıtımda kullanılıyordu. Bunun çıkması ile birlikte birçok `Linux` dağıtımı dahi, buna uyumlu `init process` sistemleri ile dağıtılıyordu. herkes kendi `init process`'ini yazıyordu. bu sebeple bu tarz `init process`'ler için `System V style init`, `traditional init` gibi terimler kullanılır.

şu anda piyasada birçok `init process` alternatifi mevcut: `systemd`, `upstart`

`Upstart` daha çok sıralı bir açılışa ağırlık verir. örneğin; update hizmeti, log servisine bağlı ise önce log servisinin başlatılması beklenir. hazır olunca update servisi başlar. oysa `systemd`'de işlemler paralel başlatılır. birbirlerine depend eden istekler beklemeye alınır. işlemler paralel başladığından `CPU` daha az boşta kalır ve daha hızlı açılış gözlenir.

`systemd` vs `Upstart` için komut satırı uygulamaları mevcut. bunların kullanım örnekleri aşağıdadır:

| Operation                          | `Upstart` Command                 | `Systemd` command                 |
|------------------------------------|-----------------------------------|-----------------------------------|
| Start service                      | start $job                        | systemctl start $unit             |
| Stop service                       | stop $job                         | systemctl stop $unit              |
| Restart service                    | restart $job                      | systemctl restart $unit           |
| See status of services             | initctl list                      | systemctl status                  |
| Check configuration is valid       | init-checkconf /tmp/foo.conf      | systemd-analyze verify $unitFile  |
| Show job environment               | initctl list-env                  | systemctl show-environment        |
| Set job environment variable       | initctl set-env foo=bar           | systemctl set-environment foo=bar |
| Remove job environment variable    | initctl unset-env foo             | systemctl unset-environment foo   |
| View job log                       | cat /var/log/upstart/$job.log     | sudo journalctl -u $unit          |
| tail -f job log                    | tail -f /var/log/upstart/$job.log | sudo journalctl -u $unit -f       |
| Show relationship between services | initctl2dot                       | systemctl list-dependencies --all |

not: `service` komutu bir `shell` script dosyasıdır ve `init process` türünü bilmeden işlem yapabilmemize olanak sağlayan bir wrapper'dır.

`Systemd` servisleri başlatırken hangi servisleri başlatacağına `target`'lar aracılığı ile karar verir. örneğin sunucumuzda grafik arayüzü hiç kurulmamış ise, daha sonra `GUI` kurarsak ve varsayılan olarak `GUI` servislerinin açılmasını istiyorsak, default `target`'ı değiştirmeliyiz:

```sh
systemctl set-default graphical.target
```

output:

```text
Removed symlink /etc/systemd/system/default.target.
Created symlink from /etc/systemd/system/default.target to /usr/lib/systemd/system/graphical.target.
```

sistemde var olan target'ları görebilmek için:

```sh
systemctl list-units --type=target
```

## 📌 Linux dışındaki OS'larda Docker nasıl çalışıyor

`container` yapısı sadece `Linux`'ta vardır. fakat `MS Windows` ve `MacOS`, `container` yapısı desteklemez. `Docker` uygulaması kurulduğunda kendi bir sanal `Linux` makinesi kurmaktadır.

`Docker` var olan `OS`'un çekirdeğini kullandığı için (ortada ikinci `OS` olmadığı için), `Docker`'ın `Hypervisor` gibi `CPU` teknolojilerine bağımlılığı yoktur. en azından `Linux` için bu şekilde. fakat `MS Windows`, 2016 yılı sonrası sürümleri itibari ile container yapısını desteklemeye başlamıştır. fakat teknik altyapıda donanıma bağımlılık vardır. hyper-v tarzı teknolojilere dayanmaktadırlar.

`MS Windows` base image olarak temelde şu 4ünü resmi olarak sunuyor:

- `Nano Server` - is an ultra-light `MS Windows` offering for new application development. `.NET Core` kullanan uygulamalar için bu image yeterlidir.
- `Server Core` - is medium in size for `MS Windows` `Server` apps.
- `Windows` is the largest image and has full `MS Windows` `API` support.
- `Windows Server` is slightly smaller than the `MS Windows` image, has full `MS Windows` `API` support, and allows you to use more server features.

Bunların üstüne kurulu `.Net` gibi birçok özel, ayrı ayrı hazır image'ler de resmi olarak sunuluyor.

## 📌 Docker machine

ek bir komut satırı uygulaması. bu uygulama ile yeni sanal `OS`'ları kurabiliyoruz. bu şekilde container desteklemeyen `OS`'larda, kolayca `Linux` `VM` yaratarak onun üzerinde Docker çalıştırmamızı sağlamaktadır. aynı şekilde sadece lokalde değil, uzak bilgisayarlardaki `VM`'ler üzerinde de çalışabilmemizi sağlamaktadır.

`Docker machine`, `MS Windows` ve `Linux`'ta `VirtualBox` kullanırken, MacOS'te HyperKit kullanmaktadır.

`Docker machine` driver'ları sayesinde `VMware`, `VirtualBox`, `Microsoft` `Azure`, `Amazon Web Services` gibi birçok farklı bulut altyapısındaki makina sistemlerine veya `VM` yöneticilerinde uzaktan işlem yapabiliyoruz.

## 📌 Docker toolbox

`Docker engine` ve `Docker compose` ve `Docker machine` ve `Kitematic` uygulamasını bir arada sunan bir setup'tır.

`MS Windows` ve `MacOS` native destek sağladıklarından beri artık `Docker toolbox`'a olan destek kaldırıldı. `MS Windows` ve `MacOS`'e kurulumlarda `Docker for Windows (⟷ Docker Desktop for Windows)` veya `Docker for mac (⟷ Docker Desktop for Mac)` kurulması öneriliyor.

`Docker for Windows` ve `Docker for Mac`, `Kubectl` ve `Minikube` yazılımını da içinde bulundurmaktadır.

## 📌 Kitematic

cross platform `Docker` client GUI'sidir.

## 📌 Docker tooling

`Eclipse` için `Docker` client eklentisidir. direk container'ların içine komut yazabilmemizi de sağlıyor.

## 📌 Docker container vs Docker image vs Dockerfile

`Dockerfile`, `Docker` image'i yaratmak için Docker'ın anlayacağı tipte bilgiler/komutlar içeren dosyadır. bu dosya işlenerek (build işlemi) image image yaratılır.

Docker image, Dockerfile ile yaratılmış run edilmeye hazır sanal pakettir.

Docker image run edildiğinde, Docker container olur.

## 📌 Container OS

container'ı çalıştıran her `OS`, host `OS`'tur. fakat bazı `OS`'lar sadece container yürütmek için özel tasarlanmıştır. içinde gereksiz paketler yoktur. bunlara `Container OS` denir.

örnek:

- `Snappy Ubuntu Core`
- `CoreOS`
- `RancherOS`
- `Project Atomic`
- `Photon`

## 📌 Snappy Ubuntu Core (⟷ Snappy Ubuntu ⟷ Ubuntu Core)

- sadece snap paket yöneticisi yüklüdür.
- IoT cihazlarında tercih edilmektedir.

## 📌 Base OS (⟷ base image OS)

container içinde koşan `OS`'tur. `FROM` komutu ile `OS` baz alınırken, `OS`'un minimum olması beklenmektedir. bu durum; hem bellekten kazandırır hem de bağımlılıkların daha iyi belirlenmesini sağlar. buradaki `OS`'ları tamamen farklı bir dizinde (`chroot` tarzı) saklanmaktadır. `container`'da run edilen uygulamalar sadece burayı görebilmektedirler. container içindeki uygulama, host `OS`'tan sadece `Linux` çekirdeğini ortak kullanmaktadır. onun dışında tüm kaynaklar mount edilen yeni dizin altındadır.

bazen `base OS` terimi, `minimal OS` terimi yerine yanlış kullanılmaktadır. ikisi farklı şeyler. `minimal OS`, normal desktop sistemler içinde olabilir. `minimal OS`, sadece ek paketlerin olmadığı anlamına gelmektedir.

bazen de `container OS` terimi `base OS` ile karıştırılmaktadır.

`Container` içerisinde sadece `OS` çekirdeğini kullanan bir uygulama çalıştırılırsa, bu durumda `base OS`'a dahi ihtiyacımız olmaz. Aslında `Base OS`, içinde `OS` çekirdeği barındırmaz, sadece kütüphaneler grubunu barındırır.

örnekler:

- `Alpine` (özelliği ufak olması)
- `Ubuntu`
- `Busybox` (özelliği ufak olması)
- `CentOS`
- `Fedora`
- `Nanoserver` (`MS Windows` native `container`'lar için)

## 📌 container user

`container` üzerinde çalışan uygulamalar `root` kullanıcı ile çalıştırılır. `Docker`, runtime sırasında, `root` kullanıcısında komut yürütmemize izin verir.

## 📌 Moby

2017 yılı ile Docker açık kaynaklı proje olarak Moby projesini baz almaya başladı. Moby projesi DockerCE ve EE olarak Docker tarafından kapatılıp üzerinde bazı değişiklikler yapıp dağıtılmaktadır.

## 📌 union file system (⟷ UnionFS)

`UnionFS`, `file system`'in özel ismidir.

"__Union mount__" ise dosya sistemlerinde olan bir feature'dir. Bu özellik desteklenen dosya sistemlerinde, bir dosyanın farklı branch'leri (versiyonları) olabilmektedir.

UnionFS'i, Docker, run edilen container'lar için kullanır. örnek: Java kurduk ve uygulamamızı run ettik. Java ve Ubuntu-OS otomatik kurulur. bu 2 container read-only'dir. run ettiğimiz Java uygulaması bir şeyler kaydetti. bu dosyalar Ubuntu-OS'un dosyalarını da değiştirmiş olabilir yada ek dosya atayabilir. böyle durumlar için `file system` böyle işliyor: Ubuntu-OS ve Java'nın olduğu bir `file system` read-only (no:1) oluşturuluyor. daha sonra farklı bir `file system`'a (no:2) Java uygulamamız dosya yazıyor. eğer container içinden bir dosya okumaya kalkarsak; UnionFS önce no:2 `file system`'a bakıyor. eğer bir dosya silme bilgisi yada yeni dosya oluşturulmuş ise bu dosyayı bize dönüyor. eğer hiçbir bilgi bulamaz ise no:1 `file system`'daki dosyayı dönüyor. bu sistemde no:2 `file system`'a no:1'in branch'i adı veriliyor. istediğimiz kadar branch oluşturabiliyoruz.

var olan ve container açıldıktan sonra düzenlenen bir dosya branch'e kopyalanır ve düzenlenir. tabi eğer taşınan bu dosya çok büyükse ilk kopyalama işlemi zaman alacaktır.

UnionFS'e birçok alternatif var. bu alternatifleri Docker engine'den seçebiliyoruz. bazı dosya sistemleri tüm dosyayı değilde, dosyayı belli bloklara bölüyor ve sadece değişen kısmın bloğunu taşıyor. bu tarz farklı yöntemler mevcut.

## 📌 volume

bir container'a host bilgisayarımızdaki bir dizini mount edebiliriz. buna alternatif olarak sanal bölümler Docker tarafından oluşturulabilir. bunlar host makina tarafından okunamazlar. mount ve volume karşılaştırılacak olursa asıl önemli avantaj: volume'ların Docker tarafından mount edilmesi olacaktır. host bilgisayarda mount edeceğimiz dizinin yetkileri, `file system`'ın desteklenip desteklenmeyeceğini gibi sorunlarla karşılaşabiliriz. oysa volume'larda böyle bir sorun yok.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 OpenStack

açık kaynaklı bulut bilişim altyapı projesidir. `AWS` gibi bulut bilişim sunan bir firmada olması gereken tüm yazılımlar bu proje altında açık kaynaklı olarak geliştirilmektedir.

projede öncelik `IaaS`'tır.

`OpenStack` projesi resmi sitesinde `OS` diye geçiyor, ama `OS` üzerinde çalışan bir yazılımdır.

`OpenStack` projesi eklenti alyapısına sahiptir.

## 📌 OpenShift

 | name                                                                  | description                                                  |
 |-----------------------------------------------------------------------|--------------------------------------------------------------|
 | `OKD (⟷ Origin Community Distribution ⟷ old-name:OpenShift Origin)`   | açık kaynaklı proje. community desteği var sadece.           |
 | `OpenShift Container Platform (⟷ OCP)`                                | `OKD`'nin forku. enterprise destek sunuluyor.                |
 | `Red Hat OpenShift Online (⟷ RHOO)`                                   | `Red Hat`'ın kendi sunucularındaki hazır `OKD`               |
 | `OpenShift Dedicated`                                                 | `AWS` gibi sunucularda kurulan `OKD`                         |
 | `OpenShift.io`                                                        | Bulut tabanlı geliştirme ortamıydı. geliştirmesi durduruldu. |
 | `OpenShift Kubernetes Engine (⟷ old-name:OpenShift Container Engine)` | Saf `K8s` ve ek birkaç özellik.                              |
 | `OpenShift Platform Plus`                                             | `OCP`'ye ek bazı özellikler daha sunuluyor.                  |

`OCP`, kendi içinde `K8s`'i wrap ederek kullanan onun üzerine ek özellikler sunan bir yazılımdır:

- hazır `Web UI` (web console)
- hazır operatörler bulundurur:
  - 3üncü parti (fakat sertifikalı) `CI/CD` ve Monitoring hizmetleri için gömülü entegrasyon özelliği.
  - logging operatörü ile `vector` uygulaması her pod'dan direk log'ları toplar.
- integrated image registry
- resmi ücretli destek
- güvenlik yamaları ek olarak sunulmaktadır (en azından açıkların `K8s`'e nazaran daha hızlı kapatılacağının garantisini vermeye çalışıyorlar)

`OpenShift`, `oc` komut satırı uygulaması yada web arayüzünden yönetilmektedir.

`Openshift`'te her proje, `K8s`'teki namespace'e denk gelmektedir. Diğer daha ufak birimler tamamiyle aynı isimde kullanılır.

`OpenShift`'in yönetim paneli için sunduğu web arayüzünde, bir proje içerisinde yeni bir `Pod` ayağa kaldırılmak istendiğinde `namespace` bilgisi ister. burada istenen `namespace` bilgisi, `Pod`'un çalışacağı `namespace` değil, `OpenShift`'in sunduğu `imagestream` özelliği için kullanılacak namespace bilgisidir.

### 📌📌 Minishift

komut satırı uygulamasıdır. lokalde tek cluster `OpenShift` ayağa kaldırır.

## 📌 Service Mesh

`Mesh` Türkçe kelime anlamı: çark, kafes.

microservice'lerin ağ yönetimi konuları için sunulan çözümlere verilen genel bir terimdir.

Ele aldığı ve çözdüğü bazı konular:

- `sidecar proxy`
- `service discovery`
- `load balancing`
- `circuit breaker`
- `SSL` (but not `Oauth`)
- monitoring network tools

Fakat bazı `service mesh` framework'lerine yazılan ek plugin'ler ile `service mesh` dışında da bazı konulara dokunabiliriz.

`istio` sadece `service mesh` yönetimi sunar.

`OpenShift`, hem `orchestration`, hem de `service mesh` özellikleri sunar.

## 📌 Istio

`K8s` içerisinde oluşturduğumuz her `Pod` içinde, istio (otomatik olarak) kendi container'larını açar (sidecar proxy pattern'i). bu sayede şunları sunar:

- servisler arası mTLS
- Gözlemlenebilirlik (observability)
  - Otomatik metrics (latency, error rate, traffic)
  - Distributed tracing (request nereden geçti)
  - Service graph (kim kiminle konuşuyor)
- gelişmiş load balancing:
  - Canary / blue-green deploy (trafik % ile yönlendirme)
  - A/B test (header/cookie bazlı routing)
  - Retry, timeout, circuit breaker

`istioctl` komut satırı uygulaması ile yönetimi sağlar.

## 📌 Kubernetes (⟷ abb:K8s)

`UCP`, `Docker swarm` ve `Docker compose`'a alternatif olan ve daha fazla özelliği içeren açık kaynaklı yazılımdır. `K8s` container'ları yönetmek için kullanılır. container'ların ne ile yönetildiği ile ilgilenmez. yani `K8s`, `Docker` yada `Rkt` ile de çalışabilir. kaldırdığı container'ları istediğimiz herhangi bir container yazılımı ile kaldırabiliriz.

### 📌📌 command line tools

aşağıdaki tüm komut satırı uygulamaları `K8s`'ten ve birbirlerinden bağımsızdır. sadece istediğimiz paketin binary'sini indirip lokalde çalıştırabiliriz.

#### 📌📌📌 Kubectl

`K8s` command line tool'udur.

#### 📌📌📌 Minikube

komut satırı uygulamasıdır. lokalde test amaçlı çalışmalar için geliştirilmiştir. local makinede `VM` açmakta ve içerisinde tek node'lu `K8s` yürütmektedir.

eğer local makinede `Docker desktop` yüklü ise; `VM`'siz de direk `K8s` yürütebilmektedir çünkü `K8s` tool'ları `Docker desktop` ile yüklü gelmektedir.

`Minikube`, `Minikube` isminde bir `context` oluşturuyor ve `Kubectl`'ye default `context` olarak set ediyor.

Minikube başlatma:

```sh
minikube start --driver="none"
```

Opsiyonel olarak vereceğimiz driver'lar aşağıda listelenmiştir. Driver, cluster'ın tümüyle hangi ortama kurulacağını belirliyor.

- VirtualBox

VirtualBox üzerinde yeni VM açıyor ve bunun içine tümüyle `K8s` admin ortamı/cluster'ı kuruyor.

- Hyperv

VirtualBox ile aynı mantıkta çalışıyor.

- KVM

VirtualBox ile aynı mantıkta çalışıyor.

- VMware

VirtualBox ile aynı mantıkta çalışıyor.

- none

`K8s` admin ortamını ve bunun üzerinde bir cluster'ı local OS'umuza kuruyor. Bu pek tavsiye edilen bir yöntem değil. Çünkü var olan OS'umuzdaki dosyalarda değişiklik yapıyor.

`K8s` cluster'ı ve admin ortamını kuracağımız ortamda hem Docker requirement'i vardır hem de container'lar haricinde normal process'lerde çalıştırılır ve OS'un dosyalarına müdahale gerçekleşir. (zaten bunlardan kaçınmak için Minikube çıktı ve bu sebeple bu driver tavsiye edilmiyor)

- Docker

OS'umuzda bir Docker başlatıyor. Bu Docker'ın içerisine gerçek bir Docker kuruyor ve Kubeadm kurulumu yapıyor.

kaynak: https://github.com/kubernetes/minikube/pull/6151 Pull request'in başlığında "Kind" kullanıldığı belirtilmiş.

__KIND__ `K8s`'in Docker ortamına kurulması için geliştirilen bir Docker-image projedir. KIND, Docker içerisine Docker kurulumu yapar ve image içerisine `K8s` kurar. kaynak: https://kind.sigs.k8s.io/docs/user/resources/ "Deep Dive: KIND - Benjamin Elder & Antonio Ojea" videosunda, 2:08'de slide ekranında Docker içerisine neler kurulduğu yazılmış.

## 📌 distrobox vs toolbx

birbirlerine alternatif projelerdir.

çok kolay şekilde `$HOME` ve `GUI` nin `host` ile ortak olarak kullanılması için gerekli config'leri container'ı initialize ederken ona parametre geçiyor.

`distrobox` %100 `shell` script projesidir. resmi sitesindeki `install` komutu, sadece `shell` script'leri bi dizine atar. yani tamamen portable'dır.

`toolbx` ise `golang` ile yazılmış bir projedir.

`distrobox` config dosyası tutmaz. onun yerine her açtığı container'a `label` atar. oradaki `label`'lardan hangi container'ın `distrobox` tarafından manage edilip edilmediğini anlar.

örnek komutlar:

```sh
distrobox create -n ubuntu1 -i ubuntu:24.10

###### installs basic apps
###### and log-in to shell of guest OS.
distrobox enter ubuntu1

###### note:
###### - you can check the installing apps and mounted points from 
###### /distrobox-installation-path/bin/distrobox-init.sh file.
###### - "flatpak" executable is linked from host, if it is installed on host.
```

`toolbx`, `fedora`'da yüklü geliyor.

```sh
toolbox create fedora1 --image registry.fedoraproject.org/fedora-toolbox:41

toolbox enter fedora1
```

Bu projede: https://github.com/toolbx-images/images `enter` yapmadan önce gerekli konfigurasyonların ve paketlerin yüklü olduğu image'ler yaratılıyor. opsiyonel olarak bu image'ler `distrobox` veya `toolbx` tarafından kullanılabilir. Fakat bunlar dışında istediğimiz image'yi indirip, `distrobox` veya `toolbx` ile kullanabiliriz. hazır image'lerde hangi paketlerin yüklü olduğunu burada detaylarını ögrenebiliriz:

https://raw.githubusercontent.com/toolbx-images/images/refs/heads/main/centos/stream10/Containerfile

## 📌 Helm

`Helm` kelime anlamı: dümen.

`Helm` komut satırı uygulamasıdır. `K8s` client'ını wrap eder (arkada kubectl'i call ederek çalışır) ve sadece `Helm` tanımlamaları ile daha kolay `K8s` ortamına deploy yapabilmemizi sağlar.

Server-side tarafta `Helm`'e ait özel bir kurulum yapılması gerekmemektedir.

## 📌 Bitnami

`Bitnami` projesi altında bulut sistemler, `Docker`, `Helm` gibi sistemler için hazır `VM` veya container'lar sunulmaktadır. Bunları sunarken de her platform için ortak ayarlar sunulmaktadır. Bu şekilde bir kere yapılan ayarlar tüm ortamlarda kolay taşınabilirlik sağlamaktadır. `kaynak: https://hub.docker.com/r/bitnami/mariadb "Bitnami containers, virtual machines and cloud images use the same components and configuration approach - making it easy to switch between formats based on your project needs.".`

## 📌 Kustomize

Burada çok basit bir anlatım mevcut: https://github.com/kubernetes-sigs/kustomize/tree/master/examples/helloWorld

Aşağıdaki gibi bir dosya yapısı yaratıyoruz. production ve `uat-1` gibi farklı ortamlarımız olsun. Sadece farklılıkları `overlays` dizini altına koyuyoruz.

```text
/project-directory
├── base
│   ├── configMap.yaml --> K8s'in native tanıdığı dosya biçimi
│   ├── deployment.yaml --> K8s'in native tanıdığı dosya biçimi
│   ├── kustomization.yaml --> Kustomize'ın tanıdığı dosya biçimi
│   └── service.yaml --> K8s'in native tanıdığı dosya biçimi
└── overlays
    ├── production
    │   ├── deployment.yaml --> K8s'in native tanıdığı dosya biçimi
    │   └── kustomization.yaml --> Kustomize'ın tanıdığı dosya biçimi
    └── uat-1
        ├── kustomization.yaml --> Kustomize'ın tanıdığı dosya biçimi
        └── map.yaml --> K8s'in native tanıdığı dosya biçimi
```

Artık direk komut satırından ilgili ortama deploy alabiliriz:

```sh
OVERLAYS=/project-directory/overlays/production
kustomize build $OVERLAYS/production
kustomize build $OVERLAYS/production | kubectl apply -f -
```

## 📌 Minikube vs K3s vs KIND (⟷ Kubernetes IN Docker)

`KIND`, `K8s`'i `Docker` içerisinde çalıştıran bir projedir. `minikube`, `VM`'de değilde, `Docker` driver'ı ile initialize edilirse, `KIND` projesinden yararlanıyor.

`Minikube`, saf `K8s`'i olduğu gibi kullanıyor. sadece, initializer `Kubectl` ve `Kubeadm`'yi wrap ediyor ve kurulumları spesifik config'ler ile gerçekleştiriyor.

`K3s`, resmi `Github` sayfasındaki `Readme` dosyasında yazana göre: https://github.com/k3s-io/k3s saf `K8s`'i upstream olarak kullanıyorlar fakat patch uygulayarak şu gibi farklılıklar uygulamışlar:

- bazı özellikleri silinmiş:
  - In-tree storage drivers
  - In-tree cloud provider
- ortalama 1000 satırlık code değişikliği var
- var olan özellikler değiştiriliyor:
  - `Etcd3` yerine `Sqlite3` kullanılmış.
- ek özellikler içeriyor:
  - Managing the `SSL` certificates of `K8s` components
  - Managing the connection between worker and server nodes
  - Auto-deploying `K8s` resources from local manifests in realtime as they are changed.
