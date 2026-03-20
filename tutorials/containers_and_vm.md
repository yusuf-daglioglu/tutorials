# CONTAINERS AND VM

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Network drivers

## 📌 VirtualBox network drivers

`network interface` konusu başka başlıkta anlatılıyor.

1 `guest`'e 1'den fazla network bağdaştırıcısı eklenebilir.

`VirtualBox` network bağdaştırıcısı opsiyonları:

### 📌📌 Not attached

network cihazı takılıdır. ama kablosu takılı değildir.

### 📌📌 NAT

`subnet` oluşturmaz.

`quest` dış dünyaya çıkmak isterse, `VM` driver tarafından `NAT` işlemine tabi tutulur.

`guest`'in aldığı `IP`, private bir `IP`'dir.

`Host` dışından bu `VM`'e erişmek için `port forwarding` yapmak şarttır.

### 📌📌 NAT Network (⟷ NAT Service)

`subnet` kurar. bu ağa bağlı her `quest` birbirini görebilir. `quest` bu `subnet`'ten `IP` alır.

`quest` dış dünyaya çıkmak isterse, `VM` driver tarafından `NAT` işlemine tabi tutulur.

### 📌📌 bridge networking

`host`'un bağlı olduğu `DHCP`'den yeni bir gerçek `IP` alır.

Bunu yapabilmek için random `MAC` adresi üretiyor. Bazı host `OS`'ler veya donanımlar bunu desteklemeyebilir.

### 📌📌 Host-only networking

sanal bir ağ kartı yaratır. bu sanal ağ kartı içerisinde istediğimiz kadar `VM`'i birbiri ile iletişime geçirebilir. dış dünya ile iletişimde olmazlar.

opsiyonel olarak, bu `subnet`'te `DHCP` sunucuda yaratılır. (`VirtualBox` ayarlarından).

### 📌📌 Internal networking

`Host-only networking` yöntemi ile apaynıdır. sadece sanal bir kart yaratılmaz. pnun yerine fiziksel network kartı kullanılır. fakat internete çıkmazlar. yazılım tarafında 1 kart fakat 2 network interface'i oluşur.

sanal bir kart yaratılmaması; var olan bir fiziksel kartın kullanılmasına sebep olur. Böylece `Wireshark` gibi bir uygulama ile `Internal networking`'e bağlı `VM`'lerin network trafiğini izleyebiliriz.

### 📌📌 Cloud networking

`guest`'in uzaktaki bir subnet'teymiş gibi davranabilmesi sağlanır.

### 📌📌 Generic networking

`Virtualbox` driver eklentilerinin çalışmasını sağlar.

## 📌 ("host-only networking" and "internal networking") vs ("NAT" and "NAT Network")

`host-only networking` and `internal networking` private network'ten dışarıya `VirtualBox`'un engine'inde olan sanal bir `Switch` cihazı simülasyonu ile çıkarken, `NAT` and `NAT Network` yine `VirtualBox`'un yarattığı sanal bir `router` ile dışarıya çıkar.

## 📌 Docker network drivers

### 📌📌 none

network cihazı hiç yok gibidir container içinde.

### 📌📌 host

container kendi IP'sini almaz. host ile aynı network'ü kulanır.

### 📌📌 bridge

default.

sanal interface ve `subnet` yaratılır.

container'a bu `subnet`'ten bir `IP` atanır.

her `subnet`'in bir adı var. bunu parametre olarak veririz. default `subnet` adı: `bridge`'dir.

sadece aynı `subnet`'te olan container'lar birbirleri ile haberleşebilir.

### 📌📌 overlay

kelime anlamı: kaplama (örtü).

Bunun için `docker` engine'in diğer uzak makinadaki `docker` engine ile `Docker Swarm` gibi bir araçla bağlanmış olması şarttır.

Uzak network'teki container'lar birbirleri ile aynı `subnet`'teymiş gibi davranılar.

### 📌📌 macvlan

`host`'ın network'ünden kendisine yeni `IP` alır. Yani `VM`'deki `bridge` gibi çalışır.

`host`'un bulunduğu network'ün bilgilerini vermek zorundayız.

```sh
docker network create -d macvlan \
      --subnet="172.16.86.0/24" \
      --gateway="172.16.86.1" \
      -o parent="eth0" \
      "pub_net"
```

parent olarak ne vereceğimiz çalışma mode'u belirleyecektir:

#### 📌📌📌 bridge mode

direk olarak gerçek fiziksel kartımız üzerinden çalışır. bu sebeple gerçek fiziksel arayüzümüzü parent'a yazmak zorundayız.

#### 📌📌📌 802.1q trunk bridge mode

sanal yaratılan bir interface üzerinden fiziksel karta bağlanır. böylece host'un arayüzünde olan routing veya filter'lara takılmamış oluruz.

parent'a `eth0.50` gibi `Docker` engine'in belirttiği şekilde isim vermeliyiz.

### 📌📌 IPvlan

container'lar `host`'un network'üne bağlanır. ama:

- `host` ile aynı `MAC` adresi kullanır.
- sanal bir `subnet` oluşturulur.
- her container bu `subnet`'te `IP` alır (ama host `DHCP` vermez bunu. `Docker` engine verir.)

Artık dış dünyaya istek atarsak ve alırsak, `OS` kime yönlendireceğini bilir.

### 📌📌 Docker plugin

3üncü parti network driver'lar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Virtual machine

## 📌 Hypervisor (⟷ virtual machine monitor ⟷ VMM)

VM yönetimini çalıştıran yazılımlara verilen genel isimdir.

`Hypervisor`, run edildiği katmana göre 2'ye ayrılır:

### 📌📌 Type-1 hypervisor (⟷ native hypervisor ⟷ bare-metal hypervisor)

`VM`'de çalışan `OS`, donanım ile direk temastadır.

Örnek: `Microsoft` `Hyper-V` (Linux'u da desteklemektedir), `Xen`, `VMware ESXi (⟷ old-name:VMware ESX)`

### 📌📌 Type-2 hypervisor (⟷ hosted hypervisor)

`VM`'de çalışan `OS`, donanım ile direk temasta değildir. Arada `host` `OS` vardır.

örnek: `VirtualBox`, `QEmu (⟷ Quick Emulator)`

## 📌 Donanım sanallaştırma (⟷ platform sanallaştırma)

kurulan `OS`'lara donanımları paylaştırma işlemidir. `Hypervisor`'ler bu paylaştırma işlemini yönetir. `Hypervisor`'ler donanımları paylaştırma işlemlerine göre 3'e bölünürler:

- `Tam Sanallaştırma`: tamamiyle sanal olarak bir donanım yaratılır ve `OS`'lara bu donanım kullandırtılır.

- `Para-virtualization`: gerçek donanımlar direk olarak `OS`'lara bağlanır. sanal donanım hiç yoktur.

- `Kısmi Sanallaştırma`: hibrit yöntemdir. `OS`'lara bazı donanımlar sanal verilirken, bazı donanımlar sanal yaratılıp verilir.

## 📌 KVM (⟷ Kernel-based Virtual Machine)

- özel bir isimdir.
- `KVM`, `Linux` modülüdür.
- `Hypervisor`'dür.
- `type-1`'dir.

## 📌 Parallels Desktop (⟷ Parallels Desktop for Mac)

`MacOS` için `Hypervisor`.

## 📌 yazılım sanallaştırma

`Docker` gibi uygulamalara verilen genel isimdir.

## 📌 Intel VT-x vs AMD-V

Sanallaştırma teknolojileri için `CPU`'ların yapmış olduğu özel teknoloji isimleri.

## 📌 Nested virtualization

`VM` içinde `VM` çalıştırmaktır.

## 📌 ThinApp

- `VMware` firmasının uygulaması.
- yazılım sanallaştırması yapar.

## 📌 Emülatör (⟷ öykünücü ⟷ emulator) vs simulator (⟷ simülatör)

`emulator`; çalıştırılacak gerçek executable'ı run eder. bunu yaparken, her step'i (en ufak çalıştırma birimini) o anda gerçek çalışan native CPU'nun anlayacağı dile çevirir.

`simülatör` ile karşılaştırma doğru olmaz. Çünkü simülatör daha üst seviyelidir. Örneğin; `Android` `emulator`, `android` `OS` çalıştırmak için yapılan yazılımdır. Ama `android` simülatörü, `android` `OS` içinde gömülü yüklü gelen yazılımdır. Android simülatörü şunu yapar:

- 1- 

  `APK`, `Android` API'lerini çağırdığı zaman, o metodları fake olarak implemente eder. Örneğin `google` `play services` varmı diye sorgularsa `true` olarak dönüş yapar, oysa böyle bir servis hiç yoktur.

- 2-

  Örneğin; `APK`, `for each` döngüsü kurmuşsa, 
  - bunu `emulator` gibi `CPU`'ya çevirerekte yollayabilir,
  - direk te yolayabilir (eğer `CPU` instruction'ları zaten aynı ise)

örnekler:

- `Google`'ın `AVD`'si aracılığı ile, `android` hem native, hem de `emulator` modunda çalışabilir.
  - `android` için özel image üretiliyor. bu image direk desktop intel `CPU`'larının anlayacağı dil için derlenmiş. bu sebeple bu native çalışıyor. (yani `emulator` modunda değil).
  - `android`'in orjinal disk image'leri `ARM`'de çalışacak şekilde tasarlandı. bunları android `emulator`'da çalıştırdığımızda, `emulator` arka planda her işlemi anlık convert eder. bu kat ve kat daha yavaş çalışmasını sağlar. (`emulator` modu)

- `Android` `TV` için `emulator` çalıştırdığımızda, `emulator`,
  - `TV` ortamının sadece fiziksel ekran görüntüsünü simüle eder,
  - ancak android'in `CPU` işlemlerini emüle eder.

- eğitimlerde kullanılan uçak oyunları simülatördür. üzerinde farklı paketler execute edilip edilmemesi onun sıfatını değiştirmez. ama case'deki simülasyon, gerçek hayatın simülasyonu anlamında kullanılır.

## 📌 VirtualBox OSE (⟷ VirtualBox open source edition)

`Virtualbox` 4üncü sürümden önce kapalı kaynak ve açık kaynaklı sürüm olarak dağıtılıyordu. 4 üncü sürüm ile birlikte ikisi birleştirildi. ve lisans gerektiren ek özellikler; kapalı kaynak eklentiler olarak indirilebiliyor.

## 📌 VMware

`VMware`'in birçok farklı ürünü mevcut. Tüm ürünler kapalı kaynaktır.

## 📌 VMware Workstation (⟷ VMware Workstation Pro)

`VM` çalıştırıcısıdır. lisans gerektiren versiyon.

## 📌 VMware Workstation Player (⟷ VMware Player)

Kişisel kullanım için ücretiz sunuluyor. `VMware Workstation`'dan farklı bir executable olup, `PRO`'ya göre daha az özellik içeriyor.

## 📌 VMware Fusion

`MacOS`'in altyapısı farklı olduğundan, `MacOS` versiyonu farklı build üretilmiştir. Build o kadar farklı ki, farklı bir ürün ismi ile dağıtılmaktadır.

## 📌 Virtual Machine Manager (⟷ virt-manager) vs Gnome-Boxes vs Cockpit

tümü birbirine alternatif GUI uygulamalarıdır.

`virt-manager` özel bir uygulama ismidir.

`gnome-boxes` aşırı az özellik içerir.

`virt-manager` native desktop GUI'si var ve artık geliştirilmiyor. onu yerine `Web UI` ile hizmet veren `Cockpit` geliştirilmeye başlandı.

## 📌 Vagrant

`Virtualbox`, `VMWare` gibi uygulamaların içerisinde olan makinaları, `gitops` tarzı yönetimini sağlayan açık kaynaklı bir komut satırı uygulamasıdır.

örnek işlemler:

- multi `hypervisor`'ü tek bir API'den yönetebilmemizi sağlar.
- `OS`'u hazır bulut image'lerden indirip açar.
- `OS` run, stop eder.
- `OS` içinde bir user tanımlar.
- bu user'ın içine `SSH` komutu ile girip, istediğimizi çalıştırabilmemizi sağlar.

## 📌 Vagrant nasıl guest OS'un içine girip komut çalıştırabiliyor

## 📌📌 SSH

OS imaj dosyaları özeldir ve içinde default `vagrant` isminde user ve `SSH` server yüklü gelir ve TCP porttan default olarak dinlemeye başlar. Vagran buraya istek atar.

## 📌📌 WinRM (⟷ Windows Remote Management)

sadece `MS Windows` guest için çalışır.

`MS Windows` açıldığında bir `TCP` portundan komut çalıştırılmasına izin verir.

## 📌📌 Diğerleri

Bu alt başlıktakileri `Vagrant`'ın destekleyip desteklemediğini bilmiyorum.

Ama bu alt başlıktakilerde host ile guest arasında bir veri paylaşımı yapılabilmesini sağlıyor.

### 📌📌📌 guest tools

#### 📌📌📌📌 PCI device

guest'e bir `PCI` kablosu takılır. VM manager buradan zaten guest'te yürklü olan guest tools yazılımına bilgi yollar ve alır.

#### 📌📌📌📌 Backdoor I/O port

`CPU`'ya bilgi atarken ve alırken özel komutlar üzerinden data alışverişi sağlanır.

### 📌📌📌 Cloud/Remote provisioning (cloud-init, metadata, vs.)

basit bir text config dosyası devoloper tarafında doldurulur. bu config dosyası okunarak sistem kurulur veya boot olurken okunur.

`Cloud-init` veya `user-data` veya `metadata`: `OS` boot olurken çalışır.

`Preseed` veya `Kickstart`: `OS` install olurken çalışır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 VM vs app container

karşılaştırılmaları doğru değil. biri `VM` container'ı, diğeri uygulama container'ı.

container, `OS` çekirdeği içermez. ortak kullanır. `VM` içerir.

- `VM`'in açılması dakikalar alırken, container saniyeler alır.

- `VM` fully portable'dır. fakat container değildir. container, `OS`'u sanallaştırmıyor. var olan `OS` çekirdeğini kullanıyor. bu sebeple başka makinaya atılan container, aşağıdaki durumlarda farklı tepkiler verebilir:

  - `host` `OS` çekirdeği farklı sürüm ise
  - `host` `OS` çekirdeği farklı ise
  - `host` `OS` çekirdeğin bazı özellikleri aktif ise, başka makinada aynı özellikler aktif olmayabilir (örnek `Linux` kernel module yüklü olmayabilir)
  - donanım farklı ise (özellikle donanıma bağlı kodlarda)

  Bu durumlar bazen `VM`'de de geçerli olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

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

  `pod` yapısına `pause container (⟷ sandbox container ⟷ infra container)` denir.

  `K8s` ise pod yapısını `parent container` olarak isimlendirir.

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

### 📌📌 IPC çeşitleri

#### 📌📌📌 network socket

fiziksel veya sanal network cihazı yok ise bile, sanal olan `loopback` gibi `network interface` üzerinden yine haberleşilebilir.

#### 📌📌📌 Message passing

Mesaj karşıya sync olarak atılır ve direk atılır.

Örnekler: `Java` `RMI`, `CORBA`, `DCOM`.

#### 📌📌📌 Message queue

simple `OS`-based local bus.

Examples:

- `POSIX.1-2001`
- `SYS V` (Used by `Unix`)

#### 📌📌📌 shared memory
  
Share part of the `RAM` directly. Fastest.

example:

- `POSIX shared memory`
  
  `POSIX` standartında olan özel bir API ismidir. Bu API'ye uyan herhangi bir kütüphane aracılığı ile 2 process `OS`'a ortak `RAM` kullanmak için haber verebilir.

  `nmap`, `Unix`'in sunduğu bir system call. `POSIX shared memory` yapabilmek için `nmap` system call'undan yararlanmak zorundayız.

#### 📌📌📌 Named Pipe

`Unix domain socket`'e alternatiftir ama daha basit bir yapıdır.

#### 📌📌📌 Unix domain socket

network socket'i ile alakası yoktur.

file tekniği ile aynıdır. sadece dosyanın tipi socket'tir.

`Java` example:

```java
File socketFile = new File("/tmp/mysocket.sock");
AFUNIXServerSocket server = AFUNIXServerSocket.bindOn(socketFile)
server.accept().getOutputStream().write("hello".getBytes());
```

Arka planda `TCP/IP` stack'i kullanılmaz.

network socket yerine tercih edilir. çünkü:

- network ağı olmayan gömülü sistemlerde, `TCP/IP` modülleri yüklü olmayabilir.

- herkes network portuna herkes istek yapabilir. bu güvensizdir. dışarıya kapatmak için ekstra config gerekebilir.

- `loopback` işlemleri yavaştır.

#### 📌📌📌 DBus (⟷ D-Bus)

`DBus` üst seviyeli kalıyor. Kernel değilde, user yetkileriyle çalışabiliyor. `DBus` daemon'a istek atan client'lar `TCP` socket veya `Unix domain socket` kullanabilir.

sync veya `publish-subscribe` mantığında işlem yapabilmemizi sağlar.

`DBus` daemon'a istek atabilen birçok client var:
- `GDBus` (`GNOME`)
- `QtDBus` (`Qt` or `KDE`)
- `dbus-java`
- `libdbus` (reference implementation)

her yazılım, `DBus` aracılığı ile dışarıya birçok servis açabilir. her servis ID'si tüm OS'ta tekil olmalıdır. bu servislerin her birine `bus` denir. örnek bir `bus`:

```text
/com/appname/account
```

yukarıda `account` servisi var. bu servis içinde birçok metot olabilir.

#### 📌📌📌 file

bir app bir dosyaya yazar diğeri okur.

#### 📌📌📌 Memory-Mapped File

shared memory ile aynıdır. ama dosya aynı zamanda disk'te de tutulur. böylece kalıcılık sağlanır.

#### 📌📌📌 process signal

bir process diğer process'e bilgi atamaz ama basit Signal'ler yollayabilirler.

`PID namepaces`'e dayanır.

### 📌📌 container teknolojilerinde IPC için çözümler

Paylaşım için 2 çözüm var:

- `pod` (mümkünse tercih edilmeli)
- `system container`

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

  bu iş `pod` ile de yapılabilir. fakat pod içinde `init process` kullanmak durumundayız. bu da zaten `system container` ile aynı kapıya çıkar.

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

`container engine`, `OCI` ile uyumlu ise, OCI'nin tüm implementasyonu olan `container runtime`'larını çalıştırabilir.

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

- __Mount namespaces__

  bir grup proses, spesifik bir dizini görebilirken, diğer hiçbir proses göremeyebilir.

- `Network namespaces`

  birden fazla network arayüzü (`eth`, `Wi-Fi` gibi) birden fazla network namespace'sine bağlanır. network namespace'si iptable gibi routing ayarlarını içerir.

- `IPC namespaces`

  bir `IPC namespace`'si içindeki process, diğer `IPC`'lere mesaj atamaz.

- __PID namespaces__

  process ID grubu oluşturuluyor. bu şekilde, X pid namespace'indeki process'ler sadece birbirlerini görebiliyor.

- `uts namespaces (⟷ UNIX Time-Sharing namespaces)`

  domain ve host name'leri okurken farklı gruplar yaratabiliyoruz

- __User namespaces__

  bir process başlatıldığında o process'in kendini X user'ı ve Y grubu altındaymış gibi sanmasını sağlayabiliyoruz.

  Bu yetenek aynı zamanda sandbox özelliğini de getirdi. Çünkü yeni user ve grup açtıktan sonra artık burada özgürüz ve istediğimiz namespace'i kapatabiliyoruz. yani; unshare komutu; eğer yeni user-namespace yaratıldıysa, root yetki olmadan network'ü kapatabilir. fakat aynı komut ile yeni user-namespace yaratılmamış ise, network'ü ancak root yetkisi ile kapabiliriz.

- __time namespaces__

  her farklı time nameSpace'inde olan process'ler sistem saatini farklı algılıyor. (bu namespace tipi 2020 ile Linux 5.6'ya geldi)

- __cgroup namespaces__ vs __cgroup (⟷ control group)__

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

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 VM disk formatları

```text
 extension   native-support      support               açıklama

- VMDK       VMWare              Virtualbox, QEMU/KVM

- VDI        VirtualBox          QEMU/KVM              Genel olarak VDI ve WDMK arasında önemli farklar mevcut değil.

- raw/img    QEMU/KVM                                  hiçbir özellik barındırmıyor. sanal diskte ne varsa direk burada binary olarak tutuluyor. format header dahi barındırmıyor. en iyi performans bunda.

- cow        QEMU/KVM

- qcow       QEMU/KVM            VirtualBox            cow'un yeni sürümü.

- qcow2      QEMU/KVM                                  qcow'un yeni sürümü.

- vhdx       Hyper-V             VirtualBox            vhd'nin yeni sürümü.

- vhd        Hyper-V             VirtualBox

- cloop      QEMU/KVM                                  özel amaçlı kullanılan bir format.

- qed        QEMU/KVM            VirtualBox            qcow2'nin özellikleri silinerek performans kazandırılmaya çalışılıyor. fakat proje durduruldu.
```

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

`K8s` içerisinde oluşturduğumuz her `Pod` içinde, istio (otomatik olarak) kendi container'larını açar. bu container'lar ile sidecar proxy pattern'ini baz alarak load balancing, logging gibi birçok servise mesh işlemimizi halleder.

'istioctl' komut satırı uygulaması ile yönetimi sağlar.

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

#### 📌📌📌 Kubeadm

komut satırı uygulamasıdır. `K8s`'in kendisini update edebilmemizi, cluster oluşturabilmemizi sağlıyor.

### 📌📌 components

### 📌📌 Control Plane Components (⟷ Master Components)

Cluster'daki tüm node'ları ve sistemi yöneten modüllerin tümüdür. Aşağıdaki parçalardan oluşur:

#### 📌📌📌 Kube-apiserver

API'nin sağlanması için gereken module. API olarak servis sunuyor. buraya HTTP isteği olarak yollanan emirleri Kubelet'lere yolluyor. Kube-apiserver master node içerisindedir.

#### 📌📌📌 etcd

key-value DB'dir. bütün sistemin meta bilgilerinin yönetimi buradan yapılır.

#### 📌📌📌 kube-scheduler

scheduler'ları düzenli olarak çalıştıran modüldür. aynı zamanda yeni oluşturulan `POD`'ları node'lara atayan modüldür.

#### 📌📌📌 kube-controller-manager

- node'ların ayakta olup olmadığını kontrol eder
- açılan `Pod`'ların doğru adet olduğunu kontrol eder

gibi işlevleri vardır.

#### 📌📌📌 cloud-controller-manager

`kube-controller-manager`'in bulut hizmetler (`Amazon` `AWS` gibi) için geliştirilmiş türevidir.

### 📌📌 node components

node bir VM yada fiziksel makinedir. node components, her node içinde yüklü olan modüllerdir.

#### 📌📌📌 Kubelet

her node'da bir adet yüklüdür. bu yazılım kube-controller-manager'dan emirleri alır ve yerine getirir. o node üzerinde yeni Pod'ların ayağa kaldırılması kapatılması gibi işlevleri yerine getiriyor.

#### 📌📌📌 kube-proxy

her node'da bir adet yüklüdür. `K8s` service, port yönlendirme, gibi network'sel işlemlerinin altyapısının her node'da çalışmasını sağlayan yazılımdır. network proxy yazılımıdır.

### 📌📌 Container Runtime

her node'da kurulmuş olması gereken Docker yada alternatifi container manager'ı.

### 📌📌 addons vs extension vs plugin

`K8s` dünyasında 3'ü birbirinden farklı kavramlardır.

Plugin POD yönetimi yaparken kullanılan kaynaklar üzerinde manipülasyon yapar. örnek: "volume plugin".

Addon'lar direk `K8s`'in kendi özelliklerini değiştirir. Yani `K8s` admin için daha çok gerekli özelliklerdir.

Extension'lar ise, `K8s` API'yi genişletir.

### 📌📌 namespace, context, cluster

#### 📌📌📌 namespace

her cluster'da birçok namespace olabilir. her namespace kendi içinde Pod ve servisler bulundurur.

tüm namespace'leri listele:
> kubectl get namespaces

yeni namespace yarat:
> kubectl create namespace dev

#### 📌📌📌 context

kubectl komutunu çalıştırırken, server'a login olacak bilgileri ve hangi namespace'te komut çalıştıracağımız bilgisi gereklidir. bu login ve namespace bilgilerinin bütününe birlikte "context" denir. örneğin bir context yaratalım:

> kubectl config set-context minidev --cluster=cluster1 --user=user1 --namespace=namespace1

switch to context:

> kubectl config use-context minidev

#### 📌📌📌 cluster

`K8s` cluster altyapısını destekleyen platformdur. örneğin Google cloud, Amazon cloud bu yapıyı destekliyor. lokalden hangi cluster'a gideceğimizi komut satırından belirtmemiz gerekmektedir.

Bir yazılım projesinin development, test, prod gibi ortamlarının ayrı cluster'lara (namespace'lere değil!) kurulması önerilir.

## 📌 K8s Objects

`K8s`'te yaşayan ve API üzerinden manage edilen her feature birer objedir. Obje listesi aşağıda verilmiştir.

- Pod
- service
- volume
- namespace
- Controllers
  - ReplicaSet
  - Deployment
  - StatefulSet
  - DaemonSet
  - Job

```yaml
apiVersion: v1
kind: Service # this is "object".
metadata:
  name: my-object-name
  labels:
    run: my-object-label 
spec:
  # the field under "spec", depend on the object type.
```

## 📌 Pod

Pod tanımının bulunduğu dosya örneği:

```yaml
apiVersion: v1 # in which K8s version this file works.
kind: Pod # can be other types like: Service, Pod...
metadata:
  name: multi-container-example # name of this Pod.
  namespace: test
  labels:
    app: web # labels are custom key-values. (labels are explained in another topic)
spec:
  containers:
  - name: nginx
    image: nginx:stable-alpine
    resources:
      limits:
        memory: "600Mi" # if it will try to use more memory, this Pod will be terminate automatically by K8s and will re-create again.
        CPU: "500m" # pod will not be killed if will try to use more then 500m CPU. source: https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/ title: "How Kubernetes applies resource requests and limits", paragraph: 2nd paragraph from the end.

        # CPU unit is the core CPU (virtual) count of the current machine. for example:
        # - 3000m ---> 3    CPU core of the current machine
        # - 10m   ---> 0.01 CPU core of the current machine
        
    ports:
    - containerPort: 80 # port which is exported to outside
    volumeMounts:
    - name: html
      mountPath: /usr/share/nginx/html # path which will mount inside container file system
  - name: content
    image: alpine:latest
    volumeMounts:
    - name: html
      mountPath: /html # path which will mount inside container file system
    command: ["/bin/sh", "-c"]
    args:
      - while true; do
          echo $(date)"<br>" >> /html/index.html;
          sleep 5;
        done
  volumes:
  - name: html # which is the reference name of this volume.
    emptyDir: {} # creates an empty volume.

    # alternative of "emptyDir":
    # hostPath:*
    #   path: /data # the full path of the namespace cluster machine
```

context'imizi komut satırından belirledikten sonra `Pod`'umuzu yaratabiliriz:

> kubectl create -f multi-container-example.yaml

Yukarıda 1 Pod içinde 2 adet container bulundurulmuştur. Mount şekli Docker'dan biraz farklıdır. Mount tanımı container tanımının dışındadır (üst seviyesindedir). bu mounting'e ihtiyacı olan container'lar "mountPath" ile bu tanımı kendileri için kullanabilirler. Yukarıdaki "html" isimli volume her 2 container tarafından kullanılmış. böyle durumlarda aynı dosyalar 2 container tarafından da görülüp yazılabilmektedir.

Aşağıdaki komut ile var olan Pod'ları listeleyebiliriz:

> kubectl get pods --show-labels

Log'a bakmak için:

> kubectl logs -f pod-name --tail 200

Eğer bir Pod içerisinde birden fazla container varsa, sadece ilgili container'ın log'una bakmak istiyorsak:

> kubectl logs -f pod-name --tail 200 -c container-name

get detail of running Pod:

> kubectl describe pod pod-name

delete Pod:

> kubectl delete pod pod-name

yada

> kubectl delete -f pod-manifest.yaml

## 📌 port-forward

`pod-manifest.yaml` dosyamızda dışarıya açılacak portu belirtmezsek; sonradan komut satırında şu şekilde portu dışarıya açabiliriz:

> kubectl port-forward pod-name 8081:8080

## 📌 CNI (⟷ Container network interface)

`K8s` network kurmaz, port yönlendirmeleri yapmaz. Aracı bir plugin kullanır. O plugin2lere ne yapmaları gerektiğini söyler.

örnek plugin'ler: `Calico`, `Flannel`, `Weave Net`.

`K8s`'te default olarak CNI yüklü gelmez. Bu sebeple eklentisiz overlay network yetenekleri çalışmaz.

## 📌 label and selector

`Pod` veya `K8s` service `YAML`'mizin başında istediğimiz isimde label'lar kullanabiliriz. bu label'lar sayesinde daha sonra birçok sorgu, update gibi işlemleri belli `Pod` veya servislerimize sağlamış olacağız.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-example
  labels:
    app: nginx
    environment: prod
    # others labels here
```

## 📌 service configuration

`K8s` service; selector'ler ile belirttiğimiz `Pod` grubuna referans edecek olan sanal yapıdır. örneğin; bir `K8s` service istek yaparsak, aslında ona bağlı `Pod`'lardan bir tanesine yapmış oluruz.

example `YAML` file:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: user-service
  labels:
    run: user-service # the label of this service.
spec:
  type: NodePort # there are other alternatives for this.
  ports:
    - port: 8080 # port of this service (only for communication between POD's. not outside of the cluster.)
                 # If it is null (as default) than  port = targetPort.

      targetPort: 8081 # when run a dockerfile, it expose a port. this port is the exposed port.
                       # example redis works 7777 as default. on dockerfile we expose that port via 8888.
                       # in this case 8888 must be the targetPort.

      nodePort: 8083 # the exported port of this service for outside of the cluster.
                     # on K8s we must have at least 1 cluster.
      
      protocol: TCP
      name: http-port # this is only a label for this port.
    - port: 8090
      targetPort: 8091
      nodePort: 8093
      protocol: TCP
      name: metrics-port
  selector:
    run: user-service-pod-label # this service is in front of all the POD's which have "user-service-pod-label".
```

## 📌 alternatives of "type"

- __NodePort__
  
  clsuter dışına port açabilmemizi sağlayan service tipi.

- __ClusterIP__ (default)

  cluster dışından erişilemeyen tamamen sanal private bir `IP` yaratıp bunu bir container'a atayabilmemizi sağlar.
  
  bu internal programlar için tercih edilir. Örneğin DB. dışarıdan erişim olsun istenmez.

- __LoadBalancer__

  Cluster'daki implementasyona göre çalışan bir yapıdır. Örneğin; cloud sistemlerde direk native load balancer'ı kullanır.

  `K8s` default olarak bir implementasyon sunmaz. dolaysı ile dışarıya port açamayız.

  `minikube` de bir implementasyon sunmaz. fakat `minikube` run olurken, ayrı bir terminal'de şu komutu çalıştırırsak:

  `minikube tunnel`

  yeni açılan process, `K8s` API'sine bağlanır ve kendini bir load balancer gibi tanıtır. böylece dışarıya `IP` açabiliriz.

- __ExternalName__

  `DNS` kaydı oluşturabişmemizi sağlar. böylece dış dünyaya veya içerdeki bir `pod`'a direk bu `DNS` üzerinden istek atabiliriz.

## 📌 Ingress

`K8s` service type değildir. kendisi başlıca `K8s` service yapısına alternatif bir özelliktir.

`NodePort` yapısına benzerdir. fakat `Ingress` ek olarak; gelen istekteki URL-PATH değerine göre istediğimiz servise yönlendirme yapabileceğimiz kurallar dizisi tanımlayabilmekteyiz.

## 📌 deployment

`K8s` deployment içerisine birçok `replicaSet` ve `Pod` barındırabilen bir yapıdır.

`YAML` içerisinde "`kind: deployment`" yazmamız gereklidir.

## 📌 Replica Controller

istediğimiz `Pod` sayısının ayakta kalmasını sağlar.

`YAML` içerisinde "`kind: ReplicationController`" yazmamız gereklidir.

## 📌 Replica Set

`Replica Controller`'ın alternatifidir. temelde aynı görevi görür.

`YAML` içerisinde "`kind: ReplicaSet`" yazmamız gereklidir.

## 📌 desired state vs actual state

her `K8s` objesinin bir 2 state'i vardır. biri yazılımcının istediği durum, diğer ise şu andaki durumudur. örneğin  `ReplicaSet` ile 100 adet container ayağa kaldırmak istemiş olalım, fakat henüz sadece 50 tanesi ayakta olsun. bu durumda: 

- `desired state` 100
- `actual state`":50

demektir.

## 📌 health check

`Health check` işlemi başarısız olursa `K8s` 2 farklı aksiyon alabilir:

- __Readiness probe__

  `probe` kelime anlamı: soruşturma/yoklama.

  Eğer `probe` işlemi (yapılan bu `health check` işlemi) başarısız olursa, `K8s` bu `pod`'a gelen request'leri yönlendirmez.

- __Liveness probe__

  Eğer `probe` işlemi (yapılan bu `health check` işlemi) başarısız olursa, `K8s` bu `pod`'u kill eder.

`K8s` `Health check` yapabilmek için 3 farklı seçenek sunuyor:

- `HTTP` isteği (`pod`'un `health check` için belirlenen HTTP isteğine 200 dönmesi yeterli)
- `TCP` isteği (`K8s` `health check` için belirlenen `TCP` portuna bağlanma isteği yapar. Eğer connection açılırsa, `pod` `health check`'i başarılıdır anlamına gelir)
- Komut (`K8s` ilgili `pod`'da bir komut çalıştırır. eğer komut, exit code 0 döndürürse o zaman `pod` sağlıklı anlamına gelir)

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
