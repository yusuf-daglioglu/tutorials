# UNIX LIKE PACKAGE MANAGERS

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Homebrew

Sadece `MacOS` için yapılan açık kaynaklı paket yöneticisi projesinin ismi. komut satırından `brew` olarak kullanılmaktadır.

`Linux` ve `MS Windows Subsystem Linux` için aynı temele dayanan yazılım paralelden ayrı geliştiriliyordu (`Linux` için olan `Linuxbrew` projesiydi). daha sonra bu projelerin tümü birleştirildi.

`Homebrew`, `Nix` ile çok benzer yapıdadır. fakat `Nix`, `homebrew`'e göre çok daha gelişmiştir.

### 📌📌 Formulae

`Formulae` paket kurulumu için gerekli tanımlamaları içeren `DSL` metindir. `Homebrew` tarafından kullanılır.

### 📌📌 Taps (⟷ Third-Party Repositories)

`Homebrew`, repository mantığını destekler.

`homebrew-cask` (`homebrew/cask`) ve `homebrew-core` (`homebrew/core`) repository'lerinin (`Tap`'ları) hala farklıdır.

### 📌📌 Bottles (⟷ Binary Packages)

`Homebrew` local makinemizde bir uygulama kurarken, her defasında `Formulae`'dan binary compile etmez. Çünkü bu enerji ve zaman tüketir. Bu sebeple önceden build edilmiş binary'ler indirilir. Tabi bu opsiyoneldir. `Homebrew` ile bir paketi komut satırına parametre geçerek compile ettirerek kurdurabiliriz.

`Homebrew` için hazır binary'ler farklı `CPU` mimarileri için farklı olabilir. `Homebrew` uygun olanını bizim için indirecektir. Hazır binary'ler genelde eski `CPU`'lara uygun şekilde build edilmiş oluyor ki, her tarafta çalışabilsin. Fakat bu dezavantajlı bir durum.

### 📌📌 Cask

`Homebrew` modülüdür. `GUI` kullanan uygulamaların yönetimini sağlamaktadır.

ilk başlarda `Homebrew`'den bağımsız geliştiriliyordu. daha sonra `Homebrew` altında ortak geliştirmeye başlandı.

`Cask` sadece `MacOS`'ta desteklenmektedir (yıl 2019).

## 📌 Flatpak (⟷ old-name:xdg-app)

`flatpak`, sadece `UNIX` tabanlı sistemler için geliştirilmiştir. user yetkisinin yeterli olduğu sandbox teknolojisine dayanıyor.

`Flathub` resmi `flatpak` app repository'nin ismidir.

`Linux namespace` kullanıyor.

## 📌 AppImage (⟷ old-name:klik ⟷ old-name:PortableLinuxApps)

- hiçbir paket yöneticisine ihtiyaç barındırmıyor.
- executable çalıştırıldığında sanal bir `file system` yaratır ve çalıştırılan executable bu `file system`'ı ve varolan OS'e okuyup yazabilir. bütün dependency'ler bu `file system` içindedir.
- portable uygulama çalıştırmak için gerekli bir teknolojidir.

## 📌 Nix

- `OS`'tan bağımsız ve tamamen portable şekilde çalışan paketleme sisteminin ismidir.

- `Linux namespace` teknolojilerinden yararlanmıyor. sadece paket build yapıldığında yararlanılıyor.

- `Nix`, `Linux` ve `MacOS`'ta native olarak çalışıyor. `MacOS`'ta `Linux namespaces` olmadığı için paket build'leri `Linux`'taki gibi full izole bir ortamda olmamaktadır.

- `MS Windows`'ta, `Windows Subsystem for Linux` veya `Docker` içinde de çalışabiliyor.

- `Nix` daemon opsiyoneldir.

## 📌 NixOS

tüm uygulamaların `Nix` paketleme yöntemi ile yönetildiği `Linux` tabanlı `OS`.

## 📌 Guix

`Nix`'in sadece açık kaynaklı paketleri kabul eden türevi.

## 📌 wrapped vs unwrapped Nix packages

örneğin `Firefox`'un her iki prefix ile de paketi mevcut. wrapped olanı eklentilerin/patch'lerin kurulu olduğu pakettir. unwrapped ise saf halidir.

## 📌 Guix System Distribution (⟷ GuixSD)

`Guix` paket yöneticisi içeren `OS`.

## 📌 portableapps.com

sadece `MS Windows` için uygulamaların portable hale getirildiği projedir. portable uygulamayı çalıştırmak için paket yöneticisine ihtiyaç yoktur.

## 📌 Snap

`flatpak` alternatifi aynı mantıkta çalışır.

## 📌 Snapcraft

`Snap` formatına uygun uygulama paketlemek için gerekli tool.

## 📌 containerized apps HOME folder

- `snap`

```
$HOME/snap/<app_name>/<app_version>/
```

- `Portableapps.com`

```
<app_name>/Data
```

- `Appimage`

```
<app_name>.home
```

if not exist then it use `$HOME`.

- `Nix`

```
$HOME
```

- `Flatpak`

```
$HOME/app/<app_name>
```

- `Docker` or `Podman`

container içindeki `$HOME`.

eğer özel olarak `Docker`'a parametre verilirse burası host `OS`'in herhangi bir dizini olabilir.

## 📌 nix dizin yapısı

```
/nix/store/HASH_OF_THIS_FILE_OR_DIR-PACKAGE_NAME-APP_VERSION
```

Bu bir dosya veya klasör olabilir. Bu dosyanın dependency'leri kendi içinde değil, yine "store" dizinin altındadır.

Her OS user, bir profil ile ilişkilendirilir. Her profil ise kendi içinde, Nix-store'da yüklü olan aynı uygulamanın farklı sürümlerine bağlanır. Bu sürümlerin executable'ları buradadır:

```
/var/profiles/PROFILE_NAME_1/bin
```

Dolayısı ile OS içinde yeni bir user'ın $PATH'ine yukarıdaki dizinini eklemek yeterlidir. artık tüm uygulamalar kullanılabilir olur.

## 📌 Nix vs containers

nix paket yöneticidir. uygulamaların portable çalışmasını ve birbirine link edebilmelerini sağlar. namespace teknolojisi kullanmaz.uygulama direk olarak native OS uygulaması olarak çalışır. Tek farkı dependency'leri nix-store içerisinde görmesidir.

## distrobox vs containers

`distrobox`, arkaplanda container açar. fakat son kullanıcı için kolaylaştırılmış bir API sunar. Bu API sayesinde:

- container içinde birçok paket hazır yüklenir.
- container'ın içinde `init process` açılabilir.
- container ile host arasında, birçok `Linux namespace` paylaşılabilir. `OS` bazlı istisnalar varsa bunlar için gerekli ayarlar yapılır. Örneğin; `/var/run` paylaşırsa yetki ne olmalıdır?
- böylece container içinde host `OS` ile bütünleşik çalışabilen programlar kurulup çalıştırılabilir.

## 📌 chroot

`Linux namespaces` teknolojisi kullanılmaz. Sadece sistem çağrısı ile root dizini değiştirilerek program başlatılır.

Açılan programlar, belirtilen root dizini dışında hiçbir dosyayı okuyamaz.

## 📌 unshare

başlatılan executable'ın `Linux namespaces`'lerini kısıtlar. fakat `Docker` gibi buna ek özellikler sunmaz. örneğin; sadece 1 parametre ile `network` izole olsun diyebiliriz.

## 📌 firejail

`unshare` ile aşırı benzerdir. ek olarak şunları opsiyonel olarak sunar:

- her program için (örnek `Firefox`) hazır engelleme profilleri sunar.
- `Pulseaudio` gibi desktop entegrasyonları vardır.
- `AppArmor`, `SELinux` gibi entegrasyonlar sunar.

## 📌 File system in USErspace (⟷ FUSE)

root yetkisi olmadan, bir process'i başlatıp, o process'in root dizini altında bazı dizinler varmış gibi çalışabilmesini sağlıyoruz (yani dizin `mount` ediyoruz). tabi bu `mount` edilen dizini sadece ilgili process'in kendisi görebiliyor.

`OS` kernel'inde `fuse` kernel module'ü yüklü olması gerekmektedir. Process'in de `Libfuse Userspace library` kullanması (API'lerini çağırması) gerekmektedir.

`FUSE`'nin desteklediği `OS`, resmi olarak sadece `Linux` ve `BSD`. Fakat 3üncü parti fork'lar `MacOS` (`macFUSE (⟷ old-name:OSXFUSE)`) içinde çalışmasını sağlamaktadır.

`FUSE` bir `sandbox` teknolojisi değildir. Çünkü executable'ı var olan ortamdan izole etmez, sadece ek bir dizin görmesini sağlar.

## 📌 Bubblewrap

`Linux`'ta root yetkisi olmadan izolasyon yapan teknolojileri wrap edip kolaylaştırır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 native or non-isolated package-managers

## 📌 Portage

paket yönetim sistemidir. komut satırı uygulaması: __emerge__'dür.

## 📌 dnf (⟷ Dandified YUM) vs yum (⟷ "Yellowdog Updater, Modified")

rpm (⟷ Red Hat Package Manager) paket yönetim sisteminin komut satırı uygulamalarıdır. dnf yum'un türevidir. Fedora'nın birçok türevinden kullanılıyor.

## 📌 yum group install vs yum install

yum paketleri grup halinde barındırabiliyor. örneğin "graphic user interface" isimli bir grup varsa, bu grubun içinde GUI için gerekli tüm paketler mevcuttur. tek komut ile tümünü kurabiliyoruz.

## 📌 zypper

rpm paket yöneticisi. Suse tarafından kullanılıyor. __ZYpp (⟷ libzypp)__ paket yöneticisinin ismi, onu yöneten komut satırı uygulamasının ismi: __zypper__.

## 📌 deb paket yönetim sistemi

## 📌 apt-get

Debian paket yöneticisi için komut satırı uygulamasıdır. "sudo apt-get install chat"

## 📌 apt-cache

Debian paket yöneticisinin source-list'i ile ilgili işlemlerin yapılmasını sağlayan komut satırı uygulaması. örnek: "sudo apt-cache search chat"

## 📌 apt

apt son kullanıcı için geliştirilmiş daha basitleştirilmiş hem apt-cache hem de apt-get komutlarını basitleştirip farklı bir arayüzde sunuyor.

## 📌 purge vs remove

`apt-get` komutuna geçilen iki parametrede uygulamayı silmemizi sağlar. purge ise configuration dosyalarını da siler. burada bahsi geçen config dosyaları `/home` içindeki dosyalar değil. bahsi geçen config'ler `/etc` içindeki config'lerdir.

note: `/etc` inlcludes config files for all users. example: `/etc/appname/`.

`purge` seçeneği, `Synaptic package manager` UI'ındaki `Tamamen kaldır` seçeneğine tekabül ediyor.

## 📌 autoremove vs remove

remove yerine autoremove kullanılırsa, silinen ilgili paketin yüklemiş olduğu ve diğer paketler tarafından kullanılmayan dependency'ler(paketler) de sistemden silinir. genelde bu paketler çok ufak olduğunda tekrar ihtiyaç olması ihtimaline karşı pek kimse autoremove kullanmıyor.

## 📌 apt-get update

Paket listelerini güncellemek. liste /etc/apt/sources.list (text bazlı dosya) dosyasındadır.

## 📌 apt-get upgrade

kurulu paketleri günceller.

## 📌 apt-get dist-upgrade

OS'u günceller.

## 📌 upgrade vs dist-upgrade

sources.list dosyasına baktığımızda içinde bu tarz URL görebiliriz: "<http://archive.Ubuntu.com/Ubuntu> xenial-security". dikkat edilirse; xenial (Ubuntu'nun bir sürümünün ismi) yazıyor. işte "upgrade" komutu run edildiğinde xenial için desteklenen en son sürüm vlc, Firefox ve diğer paketler kuruluyor. yani vlc'nin en son sürümü kurulacak diye bir durum söz konusu değildir. fakat "dist-upgrade" sources.list dosyasının bu URL'lerini de günceller. işte OS yükseltme farkı budur. OS de paketler topluluğudur. bu bakış açısı ile "xenial" kelimesi sources.list'te olmasaydı tüm sistem sürekli yükselirdi.

## 📌 apt-get clean

/var/cache/apt/archives/ içinde bulunan .deb uzantılı dosyaları siler. ".deb" dosyaları tekrar kurulma ihtimaline karşı hazır tutulur.

## 📌 apt-cache search paket_adı

programı aramak

## 📌 apt-cache show paket_adı

program hakkında bilgi almak

## 📌 repository

sources.list dosyasındaki her URL bir repository'dir. repository paketlerin dışarıdan indirilmesi için açılan sunucudur.

## 📌 ppa (⟷ Personal Package Archive)

paketlerin dışarıdan indirilmesi için açılan Launchpad sunucularıdır. bir ppa sunucusunda birden fazla paket çeşidi indirilebilir. sadece bir paket olmak zorunda değildir.

apt-add-repository ppa:group-name/sub-name ile ppa kurulabilir. eklenen ppa source.list'e eklenmez. ppa farklı bir yerde tutulur: "/etc/apt/sources.list.d" dizininde dosyalar halinde tutulurlar.

## 📌 dpkg

deb dosyaları üzerinde direk işlem yapabilmemizi sağlayan komut satırı uygulamasıdır.
