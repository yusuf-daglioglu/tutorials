# UNIX LIKE

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Everything is a file (approach/principle of Unix-like)

bu dosyalar `özel dosyalar` olarak adlandırılırlar. dosya tipleri:

- `regular file`

  `file system`'daki normal özelliksiz bir dosya

- dizin

  `file system`'daki dizin.

- `Symbolic link (⟷ symlink ⟷ soft link)`

- __Named pipe__

  `IPC` çeşidi.

  `FIFO` mantığındadır ve Socket'e göre daha basittir.

- `Socket`
 
  `IPC` çeşidi.
  
  network soketi ile karıştırılmamalıdır. bazı kaynaklarda `UNIX domain socket` olarak da geçmektedir.

  `Named pipe` yapısına göre daha gelişmiştir.

- `device`

  driver dosyası.

  isimlendirme örnekleri: https://en.wikipedia.org/wiki/Device_file#Naming_conventions

  - `Character device`
  
    karakter okuma yazma yapma zorunluluğu yok. bu isim karışıklığına sebep oluyor.
    
    direk cihaza erişir. yani `OS`'un cache ve buffer'ı olmaz. (hardware'nin kendi içinde cache olabilir)

    klavye, mouse, ses kartı.

  - `block device`
  
    character gibidir ama `OS`'un cache ve buffer'ı vardır.
    
    `SSD`, `HDD`, `CD-ROM`.

  - `Pseudo-devices`

    `/dev/null` gibi, gerçek fiziksel bir donanıma tekabül etmeyen sürücülerdir.

- __Door__

  sadece `Solaris`'te kullanılan `IPC` için gerekli bir dosya.

## 📌 file descriptor (⟷ dosya betimleyicisi)

`OS`, her process'e her açtığı dosya için bir tam sayı verir.

```c
// c code example:

file_descriptor = open("file.txt");

bytes_read = read(file_descriptor);
```

Bu `file descriptor`'lar sanal olarak (fiziksel olarak yoklar):

```
/prod/process-id/file-descriptor
```

ayrı ayrı dosyalarda tutulurlar. örneğin:

```
/prod/1001/fd/48
```

dosyası bir `IPC` soketine veya `regular file`'a denk geliyor olabilir.

## 📌 TCP veya UDP soketleri

`file descriptor`'dır. `special file` değillerdir.

Bu sebeple aslında `Unix`'te `Everything is a file descriptor` demek daha doğrudur.

## 📌 dosya vs API

stream'e komut satırından bile yazılabilir ve dosya API'si tüm `OS` ve framework'lerde vardır ve basittir.

## 📌 Standard streams

neredeyse tüm `OS`'larda standart olarak şu 3 stream uygulamaya parametre geçilir:

- `standard input (⟷ stdin)`
- `standard output (⟷ stdout)`
- `standard error (⟷ stderr)`

## 📌 maximum açık dosya limiti

`OS`'ta bu config

- `OS` bazlı limit
- user bazlı limit
- process bazlı limit

belirlenebilir.

bir programın kendisi çok bellekten harcarsa (örneğin durmadan integer array'ler oluşturursa) memory doldu hatası alacaktır. bu `OS`'u etkilemez. fakat program durmadan soket açarsa, `OS` bunlara karşılık birer dosya oluşturur. bu durumda `file system` bu durumdan etkilenecektir. bunu engellemek için bu limit var.

## 📌 fork-bomb

sürekli olarak process açıp sistemi sıkıntıya sokan programlardır. sürekli dosya açılması sıkıntı yaratır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 freedesktop.org (⟷ X Desktop Group ⟷ XDG)

`X` `window system` üzerindeki uygulamalara (`Gnome`, `KDE` gibi) katkıda bulunan ticari olmayan topluluğun ismidir.

## 📌 XDG Base Directory Specification

Birçok versiyonu vardır. `Kaynak: https://specifications.freedesktop.org/ title:basedir-spec`

Bu standartlara uyulmak zorunda değildir, fakat standart olduğu için birçok yazılım bu dizinleri tercih eder.

Bazı standart `XDG` değişkenleri:

- `$XDG_CACHE_HOME`

  cache directory.

- `$XDG_CONFIG_HOME`

  sadece config dosyalarıdır. başka makineye taşınabilir olmalıdır.

- `$XDG_DATA_HOME`

  Yazılımcı camiasında `static dosya` olarak tabir edilen dosyalar burada olabilir. örnek durumlar:

  - Thumbnail.
  - oyun uygulamasında son kullanıcının indirdiği custom ses dosyaları.
  - chat uygulamasında son kullanıcının indirdiği custom emoji dosyaları.
  - eklentiler.

- `$XDG_STATE_HOME`

  `XDG`'nin eski versiyonlarda yoktu.

  Uygulamanın anlık durumunu tutar. Örneğin:
  - file sync işlemi yapıyorduk, pause edip uygulamamızı kapattık. Sync statümüz burada kaydolmalıdır.
  - text editor ise; en son açılan dosya listesi.
  - media player ise; en son çalınan müzikler listesi.
  - web browser ise; cookie'ler, history.
  - log dosyaları.

- `$XDG_DATA_DIRS`

  `OS` üzerinde, `$XDG_DATA_HOME` dan başka bir dizinde de DATA bilgisi tutuyor olabiliriz. Eğer böyle bir durum var ise, bu variable içerisinde bu dizinlerin listesini tutmalıyız.

- `$XDG_CONFIG_DIRS`

  `$XDG_DATA_DIRS` ile aynı mantıkta bilgi tutmaktadır. Tek farkı, DATA yerine CONFIG bilgisine referans etmesidir.

Default değerler şu şekildedir:

| Type | Purpose    | XDG Environment Variable | Linux & BSD                                      | MacOS                             | MS Windows                      |
|------|------------|--------------------------|--------------------------------------------------|-----------------------------------|---------------------------------|
| Base | home       | HOME                     | $HOME                                            | $HOME                             | {FOLDERID_UserProfile}          |
| Base | cache      | XDG_CACHE_HOME           | $XDG_CACHE_HOME or $HOME/.cache                  | $HOME/Library/Caches              | {FOLDERID_LocalApplicationData} |
| Base | config     | XDG_CONFIG_HOME          | $XDG_CONFIG_HOME or $HOME/.config                | $HOME/Library/Application Support | {FOLDERID_ApplicationData}      |
| Base | data       | XDG_DATA_HOME            | $XDG_DATA_HOME or $HOME/.local/share             | $HOME/Library/Application Support | {FOLDERID_ApplicationData}      |
| Base | dataLocal  | XDG_DATA_HOME            | $XDG_DATA_HOME or $HOME/.local/share             | $HOME/Library/Application Support | {FOLDERID_LocalApplicationData} |
| Base | executable | XDG_BIN_HOME             | $XDG_BIN_HOME or $HOME/.local/bin                | null                              | null                            |
| Base | preference | XDG_CONFIG_HOME          | $XDG_CONFIG_HOME or $HOME/.config                | $HOME/Library/Preferences         | {FOLDERID_ApplicationData}      |
| Base | runtime    | XDG_RUNTIME_DIR          | $XDG_RUNTIME_DIR or null                         | null                              | null                            |
| User | audio      | XDG_MUSIC_DIR            | $XDG_MUSIC_DIR                                   | $HOME/Music                       | {FOLDERID_Music}                |
| User | desktop    | XDG_DESKTOP_DIR          | $XDG_DESKTOP_DIR                                 | $HOME/Desktop                     | {FOLDERID_Desktop}              |
| User | document   | XDG_DOCUMENTS_DIR        | $XDG_DOCUMENTS_DIR                               | $HOME/Documents                   | {FOLDERID_Documents}            |
| User | download   | XDG_DOWNLOAD_DIR         | $XDG_DOWNLOAD_DIR                                | $HOME/Downloads                   | {FOLDERID_Downloads}            |
| User | font       | XDG_DATA_HOME            | $XDG_DATA_HOME/fonts or $HOME/.local/share/fonts | $HOME/Library/Fonts               | null                            |
| User | picture    | XDG_PICTURES_DIR         | $XDG_PICTURES_DIR                                | $HOME/Pictures                    | {FOLDERID_Pictures}             |
| User | public     | XDG_PUBLICSHARE_DIR      | $XDG_PUBLICSHARE_DIR                             | $HOME/Public                      | {FOLDERID_Public}               |
| User | template   | XDG_TEMPLATES_DIR        | $XDG_TEMPLATES_DIR                               | null                              | {FOLDERID_Templates}            |
| User | video      | XDG_VIDEOS_DIR           | $XDG_VIDEOS_DIR                                  | $HOME/Movies                      | {FOLDERID_Videos}               |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 X

## 📌 Window System

Örnek implementasyon: `X (⟷ X Window System)` (Piyasada en çok 11'inci sürüm kullanıldığından `X11` olarak da isimlendirilmektedir)

Görevleri:

- ekrana `600x600`'lük bir pencere çiz
- tüm ekranın (monitörün) boyutu `1600x1200` olsun
- fare veya klavye input'unu focus olunmuş pencereye ilet
- fare imlecini şu konumda tut

Client-server mimarisi ile çalışır:

- `Display Server (⟷ Window Server)`

  ekranı sunan yazılım.

  İmplementasyonlar: `Xpra`, `Xephyr` (sadece `X` için server'lardır)

  `Firejail`'in `x11 sandboxing` özelliği, `Firejail`'in çalıştırıldığı bilgisayarda yeni bir `x` server açıyor. Run edilmesi istenen `GUI` uygulamasının client'ı bu sunucuya bağlanıyor.

- `X client`

  ekranı görüntüleyen yazılım.

  `X client` her açtığımız uygulamadır. Bu uygulamalar yetkileri dahilinde diğer uygulamaları görebilirler (ekran görüntüleri alabilir gibi).

## 📌 X.Org Server

`X Window System` için sunucu tarafıdır. Fakat `X`'in sunucu tarafı için piyasada başka alternatifler mevcuttur.

Bazı kaynaklarda kısaltılarak `X server` olarak kullanılıyor. Fakat `X Server` iki anlama da gelebilir: 

- `X.Org Server` kısaltması (`X`'in resmi server implementasyonu/yazılımı)
- yada `X Window System` için herhangi sunucu yazılımı (`X` için birden fazla sunucu yazılımı var).

bu durum karışıklığa sebep olabilir.

## 📌 MacOS display server

`Quartz Compositor` is the `display server` and compositing `window manager`.

`X11`'den çatallanan `XQuartz` yazılımı, `X11` uyumluluğunu sağlamak için kullanılan yazılımdır.

## 📌 $HOME/.Xauthority dosyası

Bu dosya, user'ın `x.org server` yazılımına eriştiğinde kullandığı oturumun Authentication token'ını saklar. eğer bu dosyaya yazma yetkisi yok ise, o user görüntüyü alamaz. bu sebeple `sudo` ile açılan ve GUI ile çalışan yazılımlarda sorun yaşanabiliyor.

## 📌 Window Manager

`Window System` üzerinde pencerelerin yönetimi ve işlevlerini belirleyen:

- minimize
- maximize
- always on top
- pencereyi fare ile boyutlandırmak

sağlayan yazılımdır.

`Window manager`'ların kurulum paketleri bazen gömülü ek uygulamacıklar olabiliyor. böyle olunca neredeyse bir `masaüstü yöneticisi` (`Gnome` gibi) gibi hizmet verebilmektedirler.

## 📌 Client-side decoration (⟷ CSD)

Pencerenin başlık çubuğu, kenarlıkları, küçültme, kapatma, maximize etme tuşları gibi çerçeve öğelerini çizmek için bir katman gerekli.

Bu dekorasyon katmanı, normalde `X` server'ın görevidir. Fakat `CSD` ile, client'lar da artık kendi pencere yapılarını oluşturabiliyor. Eğer bu görevi (client application'ın direk kendisi) bunu üstleniyorsa `CSD`, eğer üstlenmiyorsa, `Server-Side Decoration (⟷ SSD)` denir.

## 📌 Window Manager Types

- `Compositing Window Manager (⟷ compositor)`

  - pencereler birbirleri üzerine binebilirler (bu durum overlapping olarak adlandırılır)
  - ek olarak transparan yapabilme gibi birçok özelliğe sahiptirler çünkü her pencere objesini manipüle edebilirler.

  örnekler:

  Compton, Sway, Compiz, KWin, Metacity, Mutter, Xfwm, Enlightenment, Xcompmgr, Gala, Muffin.

- `Stacking window manager (⟷ floating window manager)`

  - pencereler birbirleri üzerine binebilirler
  - pencereleri manipüle etme gibi bir durumları yoktur.

  örnekler:

  `2bwm`, `Aewm`, `AfterStep`, `Amiwm`, `Blackbox`, `Cwm`, `Fluxbox`, `FVWM`, `IceWM`, `JWM`, `Matchbox`, `Mwm`, `Openbox`, `Sawfish`, `Twm`, `Window Maker`, `Eggwm`, `Evilwm`, `Flwm`.

- `Tiling window manager`

  - pencereler birbirleri üzerine binemezler.

  örnekler:

  `i3`, `Ion`, `ratpoison`, `wmii`, `StumpWM`, `Larswm`, `Qtile`, `Herbstluftwm`, `Bspwm`, `EXWM`, `Howm`, `Notion`, `Ratpoison`, `Subtle`, `Sway`, `Way-cooler`, `WMFS`, `WMFS2`.

- `dynamic tilling (⟷ dynamic)`

  `Tiling` ve `Stacking` arasında geçiş yapabilen modları mevcuttur.

  örnekler:

  - `DWM`: 2000 satırı geçmeyen sadece `C` ile yazılmış kod olacak şekilde tasarlanmaktadır. sadece temel görevleri yerine getirmek için tasarlanmıştır.

  - `Awesome`: `DWM` türevidir. 2000 satırlık kod limiti bu fork'da aşılmıştır. aynı zamanda `Lua` kullanır.

  - `Spectrwm`, `Xmonad`, `Catwm`, `Echinus`, `FrankenWM`, `Qtile`, `Wmii`

## 📌 compositor

ekrana bir şeylerin hangi efekt ile gösterileceği, hangi efekt ile sağa sola taşınacağı, gözüktüğünde nasıl gözükeceği (transparanlık) gibi işlevleri manipüle eden yazılımdır.

bu yazılım `X`'te opsiyoneldir (yani hiç olmayabilir), `Wayland`'ta mutlaka olmalıdır.

`Compositing Window Manager` ile `compositor` eş anlamlı değildir. `Compositing Window Manager`, içinde gömülü `compositor` bulunduran `window manager`'lara verilen sıfattır.

## 📌 Mir vs Wayland

`X` çok eski bir altyapıya sahiptir. Bu sebeple `X`'e alternatif bu 2 açık kaynaklı projeler geliştirilmiştir.

`X`'te `compositor`, opsiyonel bir yetenekti. Oysa `Wayland`'da zorunlu bir katmandır ve `Wayland`'da `compositor`'ün ayrı modül olması mantığı yoktur. çünkü `wayland`'da `display server` ve `compositor` ve `window manager` tek bir modül haline getirildi.

## 📌 Masaüstü yöneticisi (⟷ desktop manager)

En üst seviyeli `GUI` ortamıdır. Masaüstlerinde sunulacak özellikleri (widget, notification system, simge panelleri, ayarlar sunan yazılımlar, hazır gelen yazılımlar gibi) belirlerler.

Bazı `Window Manager`'lar, `desktop manager` olmadan kullanılabilir. Fakat sadece `Window system` tek başına (`window manager` olmadan) son kullanıcı için bir işe yaramaz.

## 📌 desktop manager'lar

- `Budgie`: `Gnome` 2'inci sürüme benzer, sıfırdan yazılmış

- `KDE Plasma`: en gelişmiş `DM` olma yolundadır

- `Gnome`: genel olarak `KDE`'ye göre daha hafiftir

- `Xfce`: genel olarak `Gnome`'a göre daha hafiftir

- `LXDE`: `Xfce`'ye alternatiftir

- `MATE`: `Gnome`'un 2'inci sürümünden devam eden bir akımdır.

- `Cinnamon`: `Gnome`'un 3'üncü sürümünden devam eden bir akımdır. `Gnome shell`'e alternatiftir. `Gnome` için kabuktur.

- `LXQt`: `LXDE`'nin `Qt` tabanlı türevidir.

- `Sugar`: Çocuklar için tasarlanan arayüzde çok kısıtlı seçenek sunan masaüstüdür

- `Moksha`: `Enlightment` projesinden türemiş, masaüstü yönetici olmaya odaklanmış bir projedir.

- `Lumina`: hafif olmayı amaçlar

- `Pantheon`: `Elementary OS` için yaratılan proje

- `Manokwari`: `Gnome` üzerine kurulu shell (`Unity` gibi)

- `Deepin DE (⟷ DDE)`

- `COSMIC`: İlk başta `Gnome` fork'uydu. Daha sonra `Rust` ile sıfırdan yazıldı. `GUI` olarak `Gnome`'a aşırı benziyor.

## 📌 KDE Plasma vs KDE

`KDE`, projenin genel ismidir. `KDE`; `Plasma masaüstü yöneticisi`, `KDE uygulamaları` ve `KDE kütüphaneleri (frameworks)` üçlüsünden oluşur. Sonuçta asıl amaç; masaüstü ortamı sunmak olduğu için; `KDE`; `K Desktop Environment` kelimelerinin kısaltmasından oluşmaktadır.

## 📌 KDE Neon

`Ubuntu` `LTS` bazlı `KDE` yüklü gelen, `KDE` takımı tarafından geliştirilen `OS`. `KUbuntu` `LTS` varken neden böyle bir `OS` geliştiriliyor? Çünkü `Ubuntu` `LTS` versiyonlarında stabilite gereği hiçbir uygulama (`masaüstü yöneticisi` de dahil) her zaman güncellenmez. Güncellense de genelde sadece güvenlik paketleri güncellenir. `KDE` takımı ise sadece kendi repolarını update edecek şekilde ayarlamıştır `Neon` içerisinde.

## 📌 GNOME Shell vs Unity

Gnome 3 üzerine inşa edilmiş farklı masaüstü yöneticileridir. Unity, Ubuntu tarafından geliştirilmekte, Gnome Shell ise, Gnome takımı tarafından geliştirilmektedir. Bu yüzden Gnome-shell, Gnome projesinin varsayılan ve referans masaüstü olmaktadır. Gnome Shell'in eklenti altyapısı mevcuttur.

Gnome, 3'üncü sürümüne kadar tek başına bir masaüstü yöneticisi olarak paketlenip sunulmaktaydı. 3'üncü sürümü ile artık her topluluk/şirket bunun üzerine ek bir kabuk yazarak dağıtmak durumunda kalıyorlar (Gnome Shell, Unity, Cinnamon gibi).

Bazı OS'lar, Gnome 3'ü hala sade (eski haline benzer) olarak gösterebilmektedirler. bunun birkaç yöntemi var:

1- `GNOME Flashback (⟷ GNOME Fallback)`: Gnome 3 ile eski görüntüyü sağlamak için bazı modüller devre dışı bırakılıyor ve shell'dekinden farklı bir window manager kullanılıyor. + eski panel kullanılıyor.

2- `GNOME Classic (⟷ GNOME Classic (no effects))`:  Gnome 3.8 sürümü ile resmi olarak; birden fazla eklenti ile eklentiler ile eski görünümü Gnome shell üzerine verebiliyor.

Gnome 2'den 3e geçiş sürecinde oturum açma ekranlarında OS'ları kendilerince isimlendirme kullandıkları için karışıklığa sebep olmaktadırlar.

## 📌 Ubuntu Gnome

Ubuntu'nun türevi. Masaüstü yöneticisi olarak sadece "Gnome Shell" yüklü gelmektedir.

Ubuntu 18.04 ile artık varsayılan masaüstü Gnome Shell'dir. Ekstra birkaç eklenti yüklü gelmektedir (örnek kısa yol panel eklentisi). Bu sebeple Ubuntu Gnome projesine ihtiyaç kalmamış ve proje sonlanmıştır.

## 📌 Display manager (⟷ login manager)

OS login ekranında çıkan yazılımdır. masaüstü yöneticisin yada sadece pencere yöneticisini başlatmak için görevlidir. komut satırı yada GUI tabanlı olabilirler.

- Console

  - CDM
  - Console TDM
  - Ly
  - Nodm

- Graphic

  - Entrance - An EFL (Enlightenment Foundation Libraries) based display manager
  - GDM - The GNOME display manager.
  - LightDM - A cross-desktop display manager, can use various front-ends written in any toolkit.
  - LXDM - The LXDE display manager. Can be used independent of the LXDE desktop environment.
  - MDM - used in Linux Mint, a fork of GDM 2.
  - SDDM - The QML-based display manager and successor to KDE4's kdm; recommended for Plasma 5 and LXQt.
  - SLiM - Lightweight and elegant graphical login solution. (discontinued)
  - XDM - The X display manager with support for XDMCP, and host chooser.

## 📌 X display manager

Sadece X session'u başlatan yazılımdır (display manager'dır).

## 📌 Widget toolkit (⟷ widget library ⟷ GUI toolkit ⟷ UX library)

Pencerelerdeki button'ların ve diğer nesnelerin nasıl olacağına ve event'lere karar veren kütüphanelerdir. Window manager ekrana bir pencere açtığında bu kütüphanelerden yararlanır. Doğal olarak; default olarak her window manager'ın yanında bir widget toolkit olmalıdır. Bazı uygulamalar kendi içerisinde widget toolkit'leri gömerler (Java ile yazılmış bir uygulamanın Swing'i kendi içerisinde bulundurması gibi, GIMP'in GTK'yı bulundurması gibi). Böyle durumlarda Window manager o uygulamayı, uygulamanın kendi widget toolkit'i ile açmak zorunda kalır. böyle durumlarda ekranda bazen sıkıntılar yola açabilir. çünkü her pencerede, bir çeşit toolkit aynı anda çalışıyor olur. bir yazılım, esnek tasarlanmış ise birden fazla toolkit ile başlatılabilir.

- Elementary: EFL (Enlightenment Foundation Libraries) proje ailesinin altında geliştiriliyor.
- GTK (⟷ old-name:GIMP Toolkit ⟷ old-name:GTK+)
- Qt
- Swing (Java framework)
- AWT (Java framework)
- Cocoa (for MacOS)
- MS Windows için:
  - Microsoft Foundation Classes (MFC)
  - Windows Template Library (WTL)
  - Object Windows Library (OWL)
  - Visual Component Library (VCL)
  - Windows Forms (WinForms)
  - Windows Presentation Foundation (WPF)
  - Windows UI Library (WinUI)

## 📌 Qt

açık kaynaklı bu proje, C++ ile yazılmış kütüphaneler grubudur. socket işlemleri, DB işlemleri ve hatta Qt widget'ları ile uygulama yazılmasını sağlar. Aynı zamanda bir IDE (pencere tasarlama modülü içeren) sunmaktadır.

Qt API'leri cross platformdur. pazarı yüksek olan tüm OS'ları destekler.

## 📌 Remote desktop using X (X11 Forwarding)

X server'a, fiziksel olarak farklı bir OS'tan (X client'tan) bağlanırsak, açacağımız uygulamayı kendi masaüstümüzdeymiş gibi etkileşimli kullanabiliriz. Örneğin; client OS'taki temaya uygun açabiliriz.

X11'in bu özelliğini kullanmayan diğer uzak bağlantı uygulamaları (örneğin Teamviewer), sadece ekranın yada bir bölümünün ekran görüntüsünü çekerek uzak makinaya aktarırlar. Oysa X11'deki olay direk olarak native bir çalışma sergilemektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 DNS ve host ayarları

`/etc/resolv.conf` dosyasında `DNS` tanımları vardır:

```
nameserver 8.8.8.8

nameserver 8.8.4.4
```

Bu dosya birçok yazılım tarafından aynı anda düzenlemekte ve okunmaktadır. okuyanlar arasında `resolver` uygulaması da vardır.

Herkesin paralel okuması veya buraya yazılması durumuna çözüm getirmek için "resolvconf" yazılımı geliştirildi. diğer tüm uygulamalar (VPN client, network manager...) her DNS değişiminde "resolvconf" uygulamasına bilgi göndermesi beklenir. Fakat "resolvconf" yazılımı sonradan geliştirildi. hala onu tanımayan başka yazılımlar var ve bu sebeple "resolvconf" her zaman tam olarak başarıyı sağlayamıyor.

Son kullanıcı resolv.conf dosyasını düzenlememeli. "/etc/network/interfaces" dosyasını düzenlemelidir.

örnek:

```text
dns-nameservers 12.34.56.78 12.34.56.79
```

network manager (root olarak çalışan yazılım) ve önyüz uygulamaları (önyüz uygulamalarına örnek: nmcli, nmtui) aşağıdaki DNS ayarlarını kendince override eder:

- dhcp ayarlarındaki DNS'leri
- resolv.conf'taki DNS'leri
- /etc/dhcp/dhclient.conf'teki DNS'leri

NetworkManager GUI'den son kullanıcının belirlediği ayarları buraya kaydeder:

> /etc/NetworkManager/system-connections/name-of-connection

örnek:

> "/etc/NetworkManager/system-connections/Wired connection 1".

dosya içeriği örneği:

```text
[802-3-ethernet]
duplex=full
mac-address=XX:XX:XX:XX:XX:XX

[connection]
id=Wired connection 1
uuid=xxx-xxxxxx-xxxxxx-xxxxxx-xxx
type=802-3-ethernet
timestamp=1385213042

[ipv6]
method=auto

[ipv4]
method=auto
dns=208.67.222.222;
ignore-auto-dns=true
```

Artık tüm network manager GUI'leri doğru bilgiyi gösterecektir.

## 📌 /etc/hosts

default örnek dosya içeriği:

```text
cat /etc/hosts
127.0.0.1 localhost
127.0.1.1 machine-id

# The following lines are desirable for IP 6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouter
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 PulseAudio vs Alsa (⟷ Advanced Linux Sound Architecture)

PulseAudio, user seviyesine kurulu olan bir uygulamadır. Bu uygulama daha alt seviyeli olan Alsa'ya sesleri teslim eder.

Alsa normalde tek bir voice-input alabilir. Yani bir den fazla uygulamadan aynı anda ses veremez. Fakat PulseAudio, aynı anda açık olan uygulamalardan sesleri alır ve bunları tek bir input olarak Alsa'ya iletmemizi sağlar.

PulseAudio birçok ses kaynağından bilgi alabildiği için "sound server" denir.

PulseAudio'nun alternatifi olarak "__dmix__" kullanılabilir.

## 📌 Open Sound System (⟷ OSS)

Lisansı tamamen özgür olmadığı için pek tercih edilmiyor. Fakat hem PulseAudio hem de Alsa ile yapılacakları sadece OSS tek başına yapabiliyor.

## 📌 Ses miksajı (⟷ Audio mixing)

çok kanallı kayıtları nihai bir bir ses ürünün birleştirme işlemidir.

## 📌 MIDI (⟷ Musical Instrument Digital Interface)

elektronik müzik aletleri ve bilgisayarlar arasında gerçek zamanlı veri alışverişini sağlayan, endüstri standardı haline gelmiş yaygın bir iletişim protocol'ü.

MIDI protocol'ünde ses verisi değil, temel bazı değişkenlere ilişkin sayısal bilgiler aktarılır; nota bilgileri, enstrüman atamaları, tempo değeri gibi bilgiler bunlardan bazılarıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Ubuntu-restricted-extras

bu paket `Ubuntu` repo'larında duruyor fakat lisans sebepleri gereği ile default yüklü getirilemiyor. içerisinde font, media codec gibi paketler barındırıyor.

## 📌 repositories

`Ubuntu`'da 4 ana repository var:

| repository name | open source | supported by canonical |
|-----------------|-------------|------------------------|
| main            | yes         | yes                    |
| universe        | yes         | no                     |
| Restricted      | no          | yes                    |
| Multiverse      | no/yes      | no                     |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 root of Linux distros

- `Debian` (`deb` based)

  - `SteamOS` (`Gnome` masaüstü de yüklü geliyor. bu şekilde normal `Linux` uygulamaları da çalıştırılıyor.) (yeni sürümleri artık `Arch` tabanlı)

  - `Raspberry Pi OS (⟷ old-name:Raspbian)` (`Raspberry` cihazında çalışması için tasarlanmış. `lxde` yüklü.)

  - `MxLinux` (sadece desktop sürümü olan. içerisinde son kullanıcı birçok için paket yüklü geliyor. bu sebeple çok kolay kullanımı var. `xfce` masaüstünü kullandığı içinde hafif.)

  - `Kali (⟷ old-name:BackTrack)` (güvenlik testleri için geliştirilmiş tool'lar içeriyor)

  - `Tails (⟷ The Amnesic Incognito Live System)` (tüm bağlantılar `TOR` üzerinden yapılır ve gizliliği amaçlar)

  - `Pardus` (`Tübitak` tarafından geliştiriliyor. eskiden `pisi` paket yöneticisi kullanırdı fakat daha sonradan `deb`'e geçti.)

  - `KNOPPIX` (temel amacı live CD ile çalışmak. çok hafif.)

  - `Ubuntu`

    - `Lubuntu` (`Gnome` yerine `Lxde` ile geliyor)

    - `Xubuntu` (`Gnome` yerine `XFCE` yüklü geliyor)

    - `Mint`

    - `Elementary` (sadece masaüstü sürümü mevcut. son kullanıcı için kolaylaştırılmış ayarlar barındırıyor.)

    - `Deepin`

    - `Zorin`

    - `Ubuntu Studio` (`Ubuntu`'nun multimedia uygulamaları yüklü gelen türevi)

    - `Pop!_OS` (`system76` şirketi tarafından özellikle `system76` donanımları ile uyumlu çalışacak driver'ları içeriyor.)

- `Fedora` (`rpm` based)

  `Fedora` default olarak `Gnome` ve `KDE` ile gelen ayrı `ISO`'larını sunmaktadır. Bunlar dışındaki masaüstü yöneticileri ile dağıtılan `ISO`'lar `Fedora Spin` ismi ile dağıtılmaktadır.

  - `Fedora` atomic yapıda olan `ISO`'larını isimlendirirken her masaüstü yöneticisi için ayrı isim vermiş durumda:

    - `Fedora Silverblue` (`Gnome`)
    - `Fedora Kinoite` (`KDE`)
    - `Fedora Sway Atomic` (`Sway`)
    - `Fedora Budgie Atomic` (`Budgie`)
    - `Fedora COSMIC Atomic` (`Cosmic`)

  - `Mandrake`

  - `Red Hat Enterprise Linux (⟷ RHEL)` (ticari)

    - `Amazon Linux` (`AWS`'deki makinelerde çalışmak için optimize edilmiş)

    - `Oracle Linux` (ticari)

    - `Scientific Linux` (akademik çalışmalarda kullanılması için tasarlandı. 2019'da geliştirmesi durduruldu ve `CentOS`'a geçip ona destek verecekleri açıklandı.)

    - `CentOS (⟷ Community Enterprise OS)` (community yönetiyor. `RHEL`'den hiçbir farkı yok. sadece lisansı ve logosu vs değiştiriliyor)

      2020 civarındaki duyuruya göre artık `CentOS Stream` isimli dağıtım sunulmaktadır. `centOS` artık geliştirmesi durdurulmuştur. `CentOS Stream` `RHEL`'ın upstream projesi olmuştur. yani RHEL'in beta versiyonu gibi düşünülebilir.

    - `Rocky Linux` (`CentOS Stream` çıkınca, tekrar `CentOS`'un yerine yapılan proje.)

    - `AlmaLinux` (`Rocky Linux` ile aynı amaçtaydı. Fakat daha sonra bazı ufak değişiklikler ile geldiler. fakat bu değişiklikler, `OS` üstünde çalışan herhangi bir yazılımı etkilemeyecek değişiklikler olduğu belirtiliyor.)

- `Arch` (`Pacman` paket yöneticisi). sistem kurulumunda sadece ihtiyaç olan paketleri seçtirebilmesi konusunda çok çok esnek.

  - `Manjaro`

  - `SteamOS`

- `Gentoo` (package manager: `Portage`) `arch` gibi sistemin paketlerine kadar kullanıcının kendisi kuruyor. arch'a göre daha fazla teknik bilgi gerektiriyor.

  - `Sabayon`

  - `Chrome OS` and `Chromium OS`

- `OpenSuse` (`rpm` based)

  - `SUSE`. enterprise.

- `PCLinuxOS` (end user friendly. sadece masaüstü versiyonu mevcut. `RPM` based.)

- `Mandriva (⟷ old-name:Mandrake)` (`RPM` based) (bu proje sonlandı. bu sebeple aşağıda 2 fork'u oluşturuldu)

  - `Mageia`

  - `OpenMandriva`

- `Puppy` (live `CD/USB` ile çalışabilmek için çok hafif tasarlanmıştır)

- `Slackware`

- `NixOS` (`Nix` package manager as default)

- `Alpine` (package manager: `APK (⟷ Alpine Package Keeper)` ) (çok ufak oluşu en temel özelliği. prod'da kullanılması önerilir.)

- `tiny core` (prod kullanımı için uygun değildir. `X` başlatabilmek için en minimal sistem yaratılmıştır)

- `Solus` (paket yöneticisi: `eopkg` (`pisi` türevidir)) (sadece son kullanıcı için masaüstü sürümü mevcuttur.)

`distrowatch.com`'dan bir `OS`'un detay sayfasına bakıldığında, temel yazılımlar için (browser, office gibi) yüklü olan güncel paket sürümleri gibi tüm detaylar dahi bulunmaktadır.

`distrotest.net` sitesi gerçek makinaya tarayıcı üzerinden `VNC` ile kullanma imkanı sağlıyor.

## 📌 Ubuntu netboot

`Ubuntu`'nun standart `ISO`'sundan farklı olarak:

- `GUI` sunmaz.
- birçok paketi yüklü getirmez.

son kullanıcılar arasında `minimal ISO` yada `mini.iso` olarak da adlandırılır. 

bu `ISO`, `Ubuntu` repo'larına kurulum sırasında bağlanarak indirme yapar. tabi güncel paket kuracağı için update işlemine gerek kalmaz.

`Ubuntu` `18.04` ile standart `ISO` ile kurulum sırasında yeni bir seçenek sunuyor: `minimal installation`. bu seçenek yukarıdakinden tamamen bağımsızdır. bu standart masaüstü bileşenlerini ve utility'lerin kurulmasını, fakat `vlc`, `libreoffice`, oyunlar gibi ekstra paketlerin kurulmamasını sağlıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 immutable distro

standart `Linux`'larda `/home` haricinde hiçbir yere yazamayız. Fakat sudo yetkimiz var ise heryere yazabiliriz.

Oysa `immutable distro`'larda 2 fark var:

- `immutable`:

  `sudo` olsak bile home haricinde hiçbir yere yazamayız.

  `rpm-osstree` gibi uygulamalar ile `sudo` olup native uygulama kurmamız mümkün. fakat şu oluyor:

  - native uygulamanın tüm değişiklikleri farklı bir layer'da tutuluyor. `docker` layer yapısı gibi.
  - uygulama kurulduktan sonra `OS` restart olmadan değişiklikleri runtime'da göremeyiz.
  - `OS` boot loader'da parametre geçerek hangi layer'ı run edeceğimizi seçebiliyoruz.

- `atomic`:

  tüm update'ler atomic'tir. böylece; full rollback native ve kolay desteklenir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 kernel (⟷ çekirdek)

`OS` ve `OS` kernel'ı farklı kavramlardır.

## 📌 UNIX

`OS`'tir.

Kapalı kaynaktır. Fakat ilk zamanlar, araştırma amaçlı üniversitelere kodları dağıtılmıştı. Bu sebeple akademisyenler, ona çok banzeyen farklı `OS`'lar çıkardı:

- `BSD`
- `System III`
- `System V`
- `Xenix`
- `SunOS`
- `AIX`
- `HP-UX`
- `Ultrix`

`UNIX`'in sadece eski sürümlerinin kaynak kodları yayımlanmıştır.

## 📌 BSD (⟷ Berkeley Software Distribution ⟷ Berkeley Standard Distribution)

- ilk çıktığındaki ismi `Berkeley Unix`'ti.
- `OS`'tur.

`BSD`'de ilk zamanlar çok fazla `UNIX`'e benzeyen kod vardı. Lisans sorunları yaratıyordu. Bu sebeple bunlar tamamen sıfırdan yazıldı ve tamamen açık kaynaklı yeni isimlerle `OS`'lar türedi:

- `FreeBSD`
- `NetBSD`
- `OpenBSD`

## 📌 Darwin

`Apple` çok eskiden `UNIX` kullanıyordu. Daha sonra lisans sorunları sebebi ile `BSD` çekirdeğini kullanmaya başladı. `BSD`'den türettikleri `OS` çekirdeğine Darwin adı verildi.

## 📌 Linux

`Linux`, `UNIX` çekirdeğinden çatallanmamıştır.

## 📌 Solaris

`BSD` türevidir. ilk çıktığında `SunOS` olarak isimlendiriliyordu. Daha sonra, sadece `Solaris` adını aldı. Daha sonra ise `Oracle Solaris` ismi ile dağıtıldı.

Şu anda `Solaris`'ten türemiş birçok OS vardır.

## 📌 Unix-like OS (⟷ UN*X ⟷ *nix)

`BSD`, `Linux`, `Unix` ve türevlerini kapsayan OS ailesidir.

Resmi bir `UNIX` Spesifikasyonu var, fakat sertifikayı almamış bir çok `Unix-like` sistemlerde mevcuttur.

## 📌 POSIX (⟷ Portable OS Interface for Unix)

__IEEE (⟷ Institute of Electrical and Electronics Engineers)__ tarafından belirlenen OS standartlarıdır.

- kısmen `POSIX` destekli: `MS Windows`, `BSD`, `Linux`.
- full `POSIX`: `Solaris` ve `MacOS`.

`POSIX` standartları temelde şunlara benzer: 

- `SIGKILL` gibi sinyalleri destekliyor mu
- `OS`'un process ve thread yönetimi API'leri ve davranışları
- tarih (`sleep()` fonskiyonu gibi) API'leri nasıl davranacak
- desteklenen encoding'ler
- desteklenen shell komutları

## 📌 Versioning of POSIX standards

- `IEEE Std 1003.1b-2001` equals to `POSIX.1b-2001`

  where:
  - `Std`: standart
  - `1003`: number for all standards for `POSIX` family
  - `.1`: `C` API
    - other examples:
      - `.2`: shell commands
      - `.7`: network rules
  - `b`: additional information for `1` (which is `C` API)
  - `2001`: verion based on year

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 .desktop dosyaları

- Birçok Linux dağıtımında `.desktop` uzantılı dosyalar mevcuttur.
- Bunlar normal birer text dosyasıdır.
- Run edildiklerinde, içindeki bilgilere göre bir komut çalıştırırlar. 
- örnek `.desktop` dosyası içeriği:

```ini
[Desktop Entry]
Version=1.0
Type=Application
Name=Firefox
Comment=Internet Web Browser
Exec=/usr/bin/firefox.bin
Icon=firefox.jpeg
```

## 📌 "open with" kısa yolları

`Nautilus`'teki `open with` seçenekleri birer `.desktop` dosyalarıdır. `Nautilus` bu dosyaları buralardan toplar:

```
/usr/share/applications

/home/user-name/.local/share/applications
```

ve bunları menüde gösterir. 

`.desktop` dosyalarının içindeki `exec` komutuna `%U`, `open with` parametresi ile birlikte açılacak dosyanın adresini gönderir. 

örneğin `Snap` uygulamaları portable'dır. fakat sistemde yüklü gibi gözükmelerini sağlamak için bir dizinde bütün `Snap` uygulamaların `.desktop` dosyaları bulundurur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Wine (⟷ Wine Is Not Emulator)

`MS Windows` executable dosyalarının `UNIX`-like'larda yürütmeye çalışan yazılım.

`Wine` yukarıdaki sebepten bir emulator değildir. çünkü `CPU` işlemlerini çevirmez aynen çalıştırır.

`Wine` üstüne çalışan uygulama, `OS` `system call` yaptığında çalıştırdığı metot, `POSIX`'te denk gelen en yakın metottur. Bu sebeple `wine` bir uyumluluk katmanı yazılımıdır diyebiliriz.

## 📌 PlayOnLinux

`Wine`'ı wrap eden bir yazılımdır. `Wine` içine kurulan uygulamalarda ekstra ayar yapmak gerekebiliyor (`dll` kurma gibi). Bunları otomatikleştirmiştir.

Farklı `OS`'lar için farklı paket vardır: `PlayOnMac`, `PlayOnBSD`.

## 📌 CrossOver

`PlayOnLinux` ile aynı görevi yapan ticari bir yazılımdır.

## 📌 dll (⟷ Dynamic-link library)

`Microsoft`'un shared-library konsepti için implementasyonudur. `dll` dosyaları `MS Windows` için portable kütüphanelerdir. bunun içindeki metotlar herhangi bir programlama dili ile çağrılabilir.

## 📌 library path

çalıştırdığımız program, kütüphaneleri tararken, taradığı dizinler arasına yeni bir dizin eklemek istersek bu path parametresinden yararlanabiliriz:

```sh
export LD_LIBRARY_PATH="/list/of/library/paths:/another/path"
./program
```

bu bilinen ve standart bir path değeri olduğundan her sistem bunu okur.

`Java`'da özel olarak ikinci bir yöntemde sunulmuştur:

```sh
java -Djava.library.path="/path/to/my/dll" -cp "/my/classpath/goes/here" "MainClass"
```

`Linux`'ta `C ve C++`'ta iki tip shared object vardır.

- Dynamically linked shared object libraries
  - genelde `.so` uzantılı dosyalardır. `Shared Object`'in kısaltmasıdır.
  - bu dosyalar runtime sırasında ilgili program tarafından okunur.
- Static libraries
  - genelde `.a` uzantılı dosyalardır.
  - derleme sırasında ilgili programın içine gömülürler.
  - `gcc` komutu ile bir derlemeye örnek verelim:

    ```sh
    gcc "src-file.c" -lm -lpthread
    ```

    `-l` prefix'inden sonra `m` ve `pthread` gelmektedir. Bu 2 dosya static library olarak kabul edilir. derleme sırasında tüm sistemde `lib` prefix'i ile aranırlar. örnek bulunan dosyalar:
    
    - `/usr/lib/libm.a`
    - `/usr/lib/libpthread.a`
  
  - `a` uzantısı archive teriminin kısaltmasından gelmektedir. `a` dosyası, `ar` (archiver'ın kısaltması) komut satırı uygulaması ile oluşturulur. `.a` dosyası birçok farklı dosyanın birleşimidir. örnek:

    ```sh
    ar rcs "libclass.a" "class1.o" "class2.o" "class3.o"
    ```

    `libclass.a` dosyası birçok `.o` dosyasının birleşimidir.

    `ar` komutu genelde sadece derlemelerde kullanılır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 File system Hierarchy Standard (FHS)

POSIX'ler için dizin standarttır. Aşağıda bazı dizinler verilmiştir. ilgili olanlar gruplandırıldı.

__

/dev device files. everything is file on Linux konusu. example: HDD partitions.

/Proc is a special virtual directory like /dev. Başka başlıkta detaylı anlatılıyor.

__

/media mounted removable devices (CD-ROM, USB)

/mnt temporary mounted volumes (mounted ISO)

/cdrom CD-ROM mount point. it is not Linux standart. because Linux use /media.

/misc automatic mounted device , like archive files (like .ISO), or mounted remote `file system`s

__

/lib shared libraries /lib32 /lib64 gibi dizinler olabilir. çünkü aynı isimde executable'lar aynı sistemde olabilir. bu dizinde aynı zamanda kernel-modülleri de vardır.
bazı OS'larda /lib /usr/lib'e linklenmiş durumdadır. bu tarz linklemeler birçok dizin için yapılmaktadır.

/usr apps own files.

/sbin system-binary'den gelmektedir. Utilities used for system administration (and other root-only commands). mostly recovering, booting, repairing things...

/bin command line based executable apps

/usr/bin commands for end user, GUI apps

/usr/sbin sistem açıldıktan sonra kullanılabilecek yönetimsel komutlar  buradadır. boot, recover, repair için olmayan fakat yönetimsel komutlar bu dizindedir.

/usr/local/sbin kullanıcı tarafından yüklenen sistemsel uygulamalar

/opt optional'dan gelmektedir. 3rd addons, or other system optional softwares.

__

/var app data for all users except config files (because config files are stored on /etc). the files inside these directory are grouped like: log files (/var/log), crash dumps (/var/crash), files which are sent to printer (/var/spool),

/etc config files for all users. example /etc/appname/.

/home/username user specific home folder

/root root user's home folder

__

(others - gruplanamayanlar)

/lost+found hata durumlarında bazı dosyalar `file system`'da yok olabiliyorlar. fakat bu dosyalar tekrar kurtarılabilmeleri için eğer yapılabiliyorlarsa bu dizin altında linkleniyorlar.

/srv data to be served by the system for protocols such as, FTP, www

/tmp temporary files for all apps.

/boot boot files (example: Grub)

## 📌 MacOS dizin yapısı

MacOS'ta POSIX'lerdeki gibi bir yapı vardır. Bazı istisnalar şunlardır:

/Applications --> uygulamaların ve kendi bilgilerinin tutulduğu dizindir. her uygulama kendi isminde bir klasörde tutulur.

/Users --> Linux'taki /home dizinidir. sadece ismi farklıdır.

/Volumes --> mount edilen tüm bölümler buradadır.

/var/root --> Linux'taki /root dizini.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 apparmor vs seLinux

birbirlerine alternatif __Linux kernel security module (⟷ LSM)__'leridir. dosyalara erişimi kısıtlarlar.

"Subject vs Principal vs User" başlığında da anlatıldığı gibi; her security context'inde:

- obje; erişilecek kaynaktır. LSM için: dosya ve dizin.
- subject; objeye erişmek isteyen process'tir. LSM için: OS üzerinde çalışan yazılımlar.

## 📌 discretionary access control (⟷ DAC) vs mandatory access control (⟷ MAC)

discretionary kelime anlamı: isteğe bağlı

mandatory kelime anlamı: zorunlu

apparmor ve seLinux yüklü olmayan UNIX-like sistemlerde, standart UNIX-like dosya koruması vardır. bu koruma DAC grubuna girer. çünkü DAC dosya sahibinin erişimi isteğe bağlı kısıtlayabildiği kontrol mekanizmalarına denir.

MAC ise; sistem yöneticisi tarafından verilen kurallar ile kısıtlama getirilen güvenlik mekanizmalarına denir.

Bu durumda apparmor vs seLinux, MAC grubundadır. Fakat ek olarak DAC yetenekleri de sunarlar.

## 📌 file and directory permissions

Dosya sistemleri birçok farklı (bağımsız) standartta bilgi bulundurabilir:

- __POSIX.1-2017__ (ACL'ye göre çok daha basit yapıdadır. sadece 1 user, 1 grup ve other olarak permission belirlenebiliyor.)
- __POSIX.1e ACL__ (__access control list (⟷ ACL)__)
- __NFSv4__ (ACL'den daha gelişmiş.)
- others...

Bir `file system` aynı anda birden fazla permission scheme'yi destekliyor olabilir.

## 📌 POSIX.1-2017 için mode'lar

Dosyalar için verine R,W ve X yetkileri kolay anlaşılıyor. Fakat dizinler için aynı yetkiler verilebiliyor. Dizinler için verildiğinde mantık daha kompleks bir hal alıyor. Bunun için aşağıdaki açıklamalar okunabilir:

### 📌📌 R

Bir dizinde R izniniz varsa, o dizinin içindeki dosyaları listeleyebilirsiniz.

### 📌📌 W

Bir dizinde write (w) izniniz varsa, dizine yeni dosyalar ekleyebilir veya mevcut dosyaları silebilirsiniz veya rename edebiliriz.

### 📌📌 X

Bir dizin için X yetkisi "cd" (veya onun alternatif'leri pushd, popd) komutu ile girip işlem yapabilmemizi sağlar. Yani o dizini process'imiz için "working directory" yapabilmemizi sağlar. "working directory" olduktan sonra dosyalarda ne yapabileceğimize her dosyanın kendi permission'ı karar verir.

Eğer bir dizinde X yetkimiz yok ise, o dizinin alt dizinlerine (yetkimiz olsa dahi) hiçbir şekilde hiçbir işlem yapamayız.

## 📌 Traverse

Linux'ta "traverse" terimi, bir dizin yapısında gezinme anlamına gelir.

bir dizinde traverse yapabilmek için, o dizinde X iznine ihtiyacımız var.

ABC dizinine sadece read yetkimiz var ise, o dizindeki dosyaları listeleyebiliriz. ama sadece execute yetkimiz var ise, o zaman dizine "cd" komutu ile girebiliriz ama dosyaları listeleyemeyiz.

## 📌 sadece write yetkisi

bir dosyaya veya klasöre read yetkimiz yok, ama write yetkimiz olabilir mi?

evet olabilir. "log" gibi sadece ekleme yapan ama okumayan process'ler olabilir. bu sebeple bu durum mantıklıdır.

## 📌 üst dizinler ve alt dizinlerdeki permission farklı ise

- Üst dizinlerde sadece R yetkisi olmasına rağmen, alt dizinlerde W yetkisi verilebilir

- Sadece alt dizine W izni verilen kullanıcı, bu dizin'e yazma işlemi yapabilir. ancak üst dizine erişim yetkisi yoksa, alt dizine erişebilmek için üst dizine X yetkisi gereklidir.

## 📌 sadece bir dizine write yetkisi neyi ifade eder?

eğer bir dizinde write yetkimiz var ise o dizinde yeni dosya oluşturabiliriz. Fakat halihazırda zaten var olan dosyalarda write yetkimiz olacağı anlamına gelmez. var olan dosyalara yazma yetkimizin olup olmadığı, sadece o dosyalar için belirlenmiş olan permission'lar belirlemektedir.

## 📌 POSIX.1-2017

Linux üzerinde dosyalar üzerindeki haklar kullanıcı(file owner), grup ve diğerleri olarak 3 grupta belirlenebilmektedir.

bir dosya için permission tanımlaması yaparken sadece owner, grup ve others olarak yapılabilir. bu sebepten eğer bir dosyaya, 2 farklı grubun hakkı olması isteniyorsa, yeni grup yaratılıp, o gruba yetki atanmalıdır.

1 kullanıcı birden fazla gruba mensup olabilir.

chown; owner değiştirme, chmod ise permission değiştirme için UNIX'lerde kullanılan komut satırı uygulamalarıdır. örnek:

```sh
chmod rwx-wx--- "myfile.txt"
```

Yukarıdaki ilk 3'lü (rwx) dosyanın owner'ı için geçerli izinlerdir. ikinci üçlü ise dosyanın grubundaki user'lara uygulanacak izinlerdir. Son üçlü ise "others" (yani owner ve grup dışındakiler) için geçerli olacak yetkilerdir.

"ls -la" komutu çıktısında, bulunduğumuz dizindeki dosyaları, yetkileri ile beraber görüntüleyebiliriz.

chmod'un aldığı sayısal parametre'ye UNIX'lerde "mode" olarak isimlendirilmektedir. "mode" şu şekilde belirlenir:

- 4 "read",

- 2 "write",

- 1 "execute"

- 0 "no permission"

her biri owner, grup, other için yan yana koyulur. aşağıdaki örnekler aynı anlama gelir:

```sh
chmod u=rwx,g=rx,o=r "myfile"
chmod 754 "myfile"
chmod rwxr-xr-- "myfile"
```

Sayısal değerleri tek tabloda gösterirsek:

| Octal | Binary | File Mode |
|-------|--------|-----------|
| 0     | 000    | ---       |
| 1     | 001    | --x       |
| 2     | 010    | -w-       |
| 3     | 011    | -wx       |
| 4     | 100    | r--       |
| 5     | 101    | r-x       |
| 6     | 110    | rw-       |
| 7     | 111    | rwx       |

Bazı mode'ların sembolik tanımlamaları da vardır. örnek:

- __a+rwx__ or __+rwx__

  __add (+)__ to __all (a) (owner, group, other)__ __"read" (r)__, __"write" (w)__ and __"execute" (x)__ permissions.

- __o+x__

  __add (+)__ only __"execute" (x)__ permission to __"others" (o)__.
  
  It does not override __"read"__ and __"write"__ permissions of __"others" (o)__.

- __go-rx__

  __remove (-)__ only __"execute" (x)__ and __"read" (r)__ permissions to __"group" (g)__ and __"others" (o)__.
  
  It does not override the __"write"__ permission of __"group" (g)__ and __"others" (o)__.

- __go-rx,u=r__

  2 farklı bilgi içeriyor:

  - __go-rx__
  
    Bu kısım yukarıda anlatıldı.
  
  - __u=r__

    __"user" (u)__ 'ın tüm permission'larını sadece __"read" (r)__ olarak belirle.

## 📌 access control list (⟷ ACL)

It is a `file system` feature. It allows us to set file permissions for multiple users for the same file.

ACLs are not configured via __chmod__ command. Instead __setfacl__ is used. To read the ACLs we can use __getfacl__ command.

Below command adds __"r"__ and __"w"__ permissions to __"group2"__.

Note: __"group2"__ is not required to be the default file's group (of the default Linux `file system`).

```sh
setfacl -m g:group2:rw file.txt
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Superuser

- `Linux`'ta hiç kısıtlaması olmayan kullanıcıdır. Kısıtlanıp kısıtlanmayacağı aslında programın koduna (kendisine) bağlıdır. Fakat bu user'ı engellemke mantıksız olduğu için kimse öyle kod yazmaz.
- `UID` (`user identifier`) sıfırdır.
- bu kullanıcının ismi genelde `root` olarak koyulur fakat bu bir zorunluluk değildir.
- dolayısı ile; terminolojik olarak `root` ve `Superuser` aynı şey değildir.
- Bazı `OS`'ler güvenlik gereği, `Superuser`'a şifre atamaz. böylece kimse login olamaz. sadece `sudo` ile 1 kerelik `Superuser` yetkisi ile işlem çalıştırabiliriz.

## 📌 su (command)

`substitute user` (vekil kullanıcı) ve `switch user`'ın kısaltmalarından gelmektedir. `super user`'ın kısaltması değildir. çünkü `su` komutu, sadece user switch yapmamızı sağlar ve o user ile komut çalıştırabilmemizi sağlar.

`su` komutu, kullanıcı login yapmadan komut satırından diğer kullanıcılara geçiş yapmamıza yarar. `su -c` komutu ile sadece o vereceğimiz komutu run etmemizi sağlar.

başka kullanıcı ile işlem yaparken `exit` komutu ile eski kullanıcıya dönüş yapabiliriz.

`su -` komutu o anda `superuser`'ı olmamızı sağlar.

`su abc` komutu ise; `abc` kullanıcısı olmamızı sağlar.

## 📌 sudo (command)

2 farklı işlem sağlar:
- `superuser` olarak işlem yapabilmemizi,
- veya başka user olarak işlem yapabilmemizi.

Yukarıdaki 2 işlem yapıldığında, process'in user-id'si gerçekten de geçiş yaptığımız user'a aittir. fakat `$HOME` gibi environment değeri `sudo`'yu çalıştıra user'a aittir. Bu sebeple `sudo`'yu dikkatli kullanmalıyız.

`sudo` komutunu çalıştıran kullanıcının, `sudo` çalıştırmaya yetki verilmiş olmalıdır (`sudoers` dosyası aşağıda anlatılıyor). komut çalıştırılırken sorulacak şifre o andaki user'ın şifresidir.

`sudo -i` ile sürekli olarak `superuser` yetkileri ile işlem yapmamızı sağlar.

`sudo` komutunun plugin altyapısı mevcuttur. bu sebeple her sistemdeki davranışları, OS'un plugin kurallarına göre değişkenlik gösterebilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Terminal (⟷ uçbirim)

kelime anlamı:

1. Otobüs, uçak vb. taşıtların yolcularını ilk aldığı veya son bıraktığı yer.
2. Bir veri iletişim ortamında veri giriş çıkışını sağlayan donanım birimi veya donanım birimleri topluluğu.

text input ve output environment. Bir sistemin yönetilebilmesi için giriş ve çıkışları olan parçadır/modüldür.

## 📌 Konsol (⟷ console)

Türkçe sözlükte: "işletmen uç birimi" olarak belirtilmiştir. yani bir terminaldir. fakat işletmen, yani operatör (Bilgisayar vb. teknik aletleri işleten kimse) için geliştirilmiş bir terminaldir.

## 📌 Sanal konsol

OS'lerdeki komut satırı uygulamaları birer "sanal konsol"dur. komutu yazan yazılımcı/programcı için geliştirilmiş konsol.

## 📌 Video oyun konsolu

Digital ortamda oyun oynayabilmek için tasarlanmış konsoldur.

## 📌 shell (⟷ kabuk)

shell, OS'ta son kullanıcı için, OS'a erişmesi için sunulan arayüzüdür. Bu arayüz Gnome, KDE olabilirken, sadece komut satırı masaüstleri yada sadece komut satırı uygulaması da bir shell'dir. çünkü OS'a erişmemiz için bir arayüz sunarlar.

## 📌 UNIX Shell

bu terim; UNIX-like sistemlerde, komut satırı yorumlayıcıları kapsar.

## 📌 Shell Command Language (⟷ sh ⟷ Bourne shell)

programming language + __"command line interpreter"__ ( dili yorumlayan yazılım) described by the POSIX standards. bir spesifikasyondur. implementasyon değildir.

"command line interface" ile "command line interpreter" kısaltmaları aynıdır. fakat ikisi farklı kavramlardır.

## 📌 Bash (⟷ Bourne-Again SHell)

Bir "sh" implementasyonudur.

Geliştirildikten sonra sh standartlarının da dışına çıkmıştır. isteğe bağlı sh uyumlu mode'da çalışabilir. "sh uyumlu mod"da çalıştığında hem bash hem de sh özelliklerini barındıracak şekilde script'leri run eder.

Bash'in pazar payı yüksek olduğundan; Çoğu UNIX-like sistemde /bin/sh uygulaması çalıştırıldığında, aslında bash çalıştırılır. /bin/sh sadece bir sembolik bir linktir (kısa yol). bin/sh sadece bash'i değil, sistemde tanımlı olan default sh implementasyonunu çağırır.

## 📌 Z Shell (⟷ Zsh)

`SH` implementasyonudur.

## 📌 diğer shell'ler

`Fish`, `c shell`, `Dash`, `Xiki Shell (⟷ Xsh)`, `KornShell` gibi.

`MacOS`, `bash` kullanmaktadır. Güncel sürümlerinde (ortalama yıl: 2019) artık `Zsh` ile birlikte gelmektedir.

## 📌 C Shell (⟷ Csh)

bir `shell`'dir. `C` diline benzeyen syntax'ı olduğu için `C` ismini almıştır.

## 📌 Tcsh

`C` `shell` fork'udur. `C Shell` ile geriye uyumludur.

Bazı OS'larda `csh` kısa yolu, `tcsh`'ye yönlendirilmiştir (`sh` in `bash`'e yönlendirilmesi gibi).

## 📌 Fish

Fish is not POSIX or shell compatible. fakat çok daha sonra geliştirilmesi başlandığı için çok güncel teknolojileri baz alıyor. GPL lisanslı. Herkes genel olarak çok başarılı buluyor fakat shell uyumlu olmaması sebebi ile hiçbir OS default yüklü getirmemektedir.

## 📌 Oh My Zsh

sadece Zsh için konfigürasyonlar community tarafından bu proje altında dağıtılmaktadır. bu konfigürasyonlar ile interpreter olan Zsh, son kullanıcı için çok daha fazla yeteneğe sahip oluyor. bu özellikler terminal'den bağımsızdır.

Oh-my-zsh, OS'a kurulduğunda istenirse root izinli bir dizinine, yada istenirse de $HOME/.oh-my-zsh/ dizinine kuruluyor. burada birçok script mevcut. Bu script'lerin devreye girebilmesi için de $HOME/.zshrc dosyasında (bu dosya Zsh tarafından execute ediliyor) bir referans (basit bir script çağırma satırı) mevcut. yani Oh-my-zsh tamamiyle taşınabilir durumda. $HOME/.oh-my-zsh/ dışında hiçbir yere kurulum yapılmıyor. $HOME/.oh-my-zsh/ dizini bir setup'tan değil, "git" aracılığı ile çekiliyor.

## 📌 Terminal Emulator (⟷ CLI ⟷ command line interface)

`shell` yada alternatiflerini kullanabilmemiz için `GUI` yazılımlarıdır.

`Terminal`, `UNIX`-like dünyasında bir `device file`'dır. Bu dosyaya yazma işini `Gnome terminal` gibi yazılımlarla yaparız. `Gnome terminal` bu mimaride `terminal emulator` oluyor.

`Terminal Emulator` içerisine komutlar yazdığımızdan ötürü, `Terminal Emulator` ile `command line interface` eş anlamlı oluyor.

`UNIX` ve benzeri sistemlerde her şey dosyadır ve `terminal`de bir dosyadır. bir komutun çıktısı ve girdisi dosyadan okunacak şekilde basittir. Oysa `powershell` bir `script language`'dir.

## 📌 piyasadaki bazı terminal emulator'ler

| name                 | OS            | notes                                                                                                    |
|----------------------|---------------|----------------------------------------------------------------------------------------------------------|
| `xterm`              | `unix`-like   | ilk olarak `X` `window manager` ile uyumlu çalışabilmesi için tasarlandığından `x` prefix'i takılmıştır. |
| `UXTerm`             | `unix`-like   | `xterm` forku. ek karakter setleri destekliyor.                                                          |
| `GNOME Terminal`     | `Gnome`       |                                                                                                          |
| `ptyxis`             | `Gnome`       | modern. container'ları buluyor, direk onların içine attach edebiliyor.                                   |
| `Guake`              | `Gnome`       | python based.                                                                                            |
| `MacOS` default      | `MacOS`       | özel bir ismi yok                                                                                        |
| `Iterm`              | `MacOS`       | proje sonlanınca, `Iterm2` olarak fork'landı.                                                            |
| `Konsole`            | `KDE`         |                                                                                                          |
| `Terminology`        | `enlightment` |                                                                                                          |
| `hyper`              | all           | `electron` uygulamasıdır. `HyperTerminal`, bambaşka bir yazılım.                                         |
| `Tabby (⟷ Terminus)` | all           | modern, `javascript` based. feature-rich.                                                                |
| `Alacritty`          | all           | modern. `rust` based. it is very fast but not fully configurable.                                        |
| `Putty`              | all           | `SSH` için.                                                                                              |
| `KiTTY`              | `unix`-like   | `Putty` projesinin fork'udur.                                                                            |
| `SecureCRT`          | all           | `SSH` için. ticari.                                                                                      |
| `Mintty`             | `MS Windows`  | aşağıdaki başlıklarda açıklanmıştır                                                                      |
| `Upterm`             | all           | `SSH` için. 2 farklı makineden aynı komutları görebilmek için geliştirilmiştir.                          |
| `Tilda`              | `unix`-like   | few contributions.                                                                                       |
| `Tilix`              | `linux`       | few contributions.                                                                                       |

Diğerleri: https://ucs-detect.readthedocs.io/results.html

## 📌 MS Windows

`terminal emulator`'ler (sırası ile yeni geliştirilene doğru):

- `MS-DOS` Prompt

  özel bir ismi yoktu. `COMMAND.COM` executable dosyası ile `GUI` açılıyordu.

- `MS Windows Console`

- `MS Windows Terminal`

`command line interpreter`'ler (sırası ile yeni geliştirilene doğru):

- `command.com`

  `MS-DOS command interpreter` olarak ta anılır çünkü `MS-DOS` tabanlı sistemlerde bu kullanılıyordu. `kaynak: https://download.microsoft.com/download/5/8/9/58911986-D4AD-4695-BF63-F734CD4DF8F2/ws-commands.pdf 10'uncu sayfa, "Command shell overview" başlığı, ilk paragraf.`

  `.bat` uzantılı dosyaları yürütür.

- `cmd.exe (⟷ Command prompt)`

  terminolojik olarak; `cmd.exe` ve `Command prompt` aynı anlamdalar. `https://devblogs.microsoft.com/commandline/windows-command-line-the-evolution-of-the-windows-command-line/ "Microsoft’s Big Bet – Windows NT" başlığı, son paragraf.`

  `.CMD` uzantılı dosyaları yürütür. fakat `.bat` ile geriye uyumludur.

  `MS Windows` `NT` sürümlerinde default olarak `cmd.exe`'ye geçiş yapmıştır.

- `PowerShell`

  `cmd.exe` ile geriye uyumlu syntax'ı yok.

### 📌📌 MS Windows Console Host (⟷ Console MS Windows Host)

`conhost.exe`'ile yürütülen yazılımdır.

`MS Windows` 7 ile gelmiştir. `kaynak: https://devblogs.microsoft.com/commandline/windows-command-line-inside-the-windows-console/ "So, what’s inside the Windows Console?" başlığı.`

`MS Windows` 7 öncesi `conhost` yerine `ClientServer Runtime System Service (⟷ CSRSS)` kullanılıyordu.

## 📌 Terminal multiplexer

`terminal emulator` ile çok benzer görevi vardır. menubar border içermezler. `Terminal multiplexer`, farklı `terminal emulator` içerisinden çağrılabilirler.

Özellikle şu amaçlar için tasarlanmıştır:
- `desktop manager` veya `window manager` olmayan ortamlar için.
- `ssh` ile bağlanılan makinede, birden çok pencere açabilmek için kullanabilmek için.

Örnek yazılımlar:

- `GNU Screen`
- `Tmux`

## 📌 Linux console

- `Kernel` içinde gelir.
- `GUI` Border vs içermez. son kullanıcı için ek özellik içermez.
- `Terminal Emulator` değildir. Direk terminal'in kendisidir.

## 📌 Prompt

bazı yazılımlar, her yeni komut satırını gösterirken, gösterilen o ekrandaki çıktıya müdahele eder.

`Starship` yazılımı buna örnek verilebilir. `Starship` ek olarak `terminal interpreter` bağımsızdır.

## 📌 Xterm türevleri

`Xterm` açık kaynaklıdır. onu fork eden birçok proje geliştirilmiştir:

- `Ansi_xterm`
- `Color_xterm`
- `Cxterm (Chinese)`
- `Hanterm (Korean)`
- `Mxterm`
- `Nxterm`
- `Kterm (Japanese)`
- `Xterm (forked by "X Consortium")`

Ekranda renkleri gösteren katman `terminal emulator`'dır. `terminal interpreter` karakterleri output olarak verir gerisine karışmaz. aldığı output'larda escape karakterlerin ne olup olmadığı terminal emulator'ın standartlarına bağlıdır. bu standartlara `escape sequences` denir. örnek sequence: `DEC VT220`.

Bazı kaynaklarda __escape sequences__ için protocol terimi de kullanılmaktadır. Bir terminal emulator manual sayfasında hangi protocol'leri desteklediğini yazar. Bazen desteklediği protocol'lerde "xterm" görebiliriz. Bu; xterm'in desteklediği protocol'leri desteklemektedir, anlamında kullanılır.

"Terminal emulator" terimi de "terminal emulation"dan gelir. çünkü VT220 birer terminal cihazlarıdır (__Digital Equipment Corporation (⟷ DEC)__ tarafından geliştirilmiştir). Onların sequence'lerini komut satırı GUI yazılımımız desteklediği için, GUI yazılımımıza terminal emulator denir.

Genelde "$TERM=xterm-*" olan terminaller xterm'in desteklediği escape sequences'ları dinliyor anlamına gelir. Fakat bu kuralı tüm terminaller doğru şekilde uygulamıyor.

Bazı terminaller birden fazla sequence'yi destekliyor. örnek "Gnome terminal".

En başta belirtildiği gibi: Ekranda renkleri gösteren katman terminal emulator'dür. fakat echo ve printf gibi komutlar (yani; herhangi bir çıktı üreten komutlar), escape karakterini dışarıya yansıtıp yansıtmayacağı önemlidir. çünkü yansıtıp yansıtmayacağına göre, terminal emulator onun renkli olup kontrol etmektedir. bu sebeple bazen komutun veya shell'in nasıl davrandığı da renklendirmek için önemli olabiliyor.

## 📌 Cygwin vs MinGW (⟷ Minimalist GNU for MS Windows)

Linux için yazılmış uygulamaları `MS Windows`'ta çalıştırmak için geliştirilen yazılımlardır.

- `Cygwin`, bu `POSIX` API'lerin yerini tutan `MS Windows` 'a uyumlu API'leri içeren `dll`'dir.

- `MinGW` de ise böyle bir `dll` katmanı yoktur. direk olarak kodu `MS Windows` için derler. Çoğu `system call` çalışmaz. hata alınır. çünkü `POSIX` API'leri ile `MS Windows` API'leri uyumsuzdur.

## 📌 MSYS

`Minimal SYStem`'ın kısaltmasıdır.

`MinGW` ile derleme yapmayı kolaylaştırmak için ek geliştirilen bir altyapıdır. `MSYS` bir `UNIX` programının `MS Windows` ortamında derlenebilmesi için gerekli tüm tool'ları barındırır. örnek:

- `make`
- `gawk`
- `grep`
- `bash` (sadece `terminal interpreter`, `terminal emulator` uygulamasını barındırmaz)

## 📌 MSYS vs MSYS2

`MSYS2`, `MSYS`'den bağımsız geliştirilen çalışmadır. fork değildir. aynı amacı güttüğü için bu şekilde isim verilmiş.

`MSYS2` kendi içerisinde `pacman` paket yöneticisini yüklü getirir.

`Git for MS Windows` başlığında `git-bash`, `msysGit` hakkında bilgiler mevcut.

## 📌 mintty

`MS Windows` için açık kaynaklı `terminal emulator`'dır. `MS Windows`'un komut satırı ile hiçbir bağlantısı yoktur. aşağıdaki paketler opsiyonel olarak mintty ile entegreli çalışabilir:

- `MSYS`
- `MSYS2`
- `Cygwin`

## 📌 Termux

- `Android` için açık kaynaklı `terminal emulator`dır.
- `distrobox` gibi kendi izole ortamı üzerinde işlem sağlar. ek olarak; sdcard gibi dizinleri bu izole ortama mount eder.

## 📌 CTRL+ALT+F1

Linux'ta GUI masaüstü ekranı açıkken buna basıldığında, tüm ekranda konsol ekranı açılır. arka planda varsayılan olarak 6 adet ekran açıktır. 7inci ekran GUI'nin kendisini (X veya Wayland gibi) açmış olan session'dır.

CTRL+ALT+F1 ile ilk konsole ekranı açılırken, CTRL+ALT+F2 ile ikincisini açabiliriz.

Eğer GUI ekranında değilsek, yani konsoldaysak, ALT+FX ilx X'inci session'a geçebiliriz. ALT+→ ve ALT+← ile bir önceki veya bir sonraki session'lara gidebiliriz.

## 📌 Windows Subsystem for Linux (⟷ WSL)

- 1.0 ve 2.0 sürümleri arasında çok büyük farklar var.
- 1.0 ve 2.0 sürümleri sadece 64-bit MS Windows'larda ve 64-bit Linux executable'larını yürütebiliyor.
- 1.0 ve 2.0 sürümleri Linux GUI uygulamalarını desteklemeyecek şekilde tasarlanmıştır.
- WSL 2 direk özel bir VM içerisinde çalışıyor. Linux executable'ları çalıştırılmak istendiğinde direk olarak çekirdekte çalıştırılıyor. Oysa WSL 1.0'da istek interpret ediliyor ve eğer varsa `POSIX` `system call`'ları emüle ediliyordu.
- WSL 2'de optimize edilmiş özel bir Linux kernel'i ile geldiği için boot-time ve performance sorunları çok düşük.
- wsl komut satırı uygulaması ile Linux komutları CMD üzerinden de çağrılabilmektedir. örnek:

  ```sh
  wsl echo "hello"
  ```

- WSL'in Linux shell'i üzerinden MS Windows uygulamaları da çağrılabilir:

  ```sh
  ipconfig.exe | grep "IP 4"
  ```

- WSL içindeki Linux ile MS Windows user'ları birbirlerinden tamamen bağımsız.
- MS Windows dosyalarına WSL içerisinden bu şekilde erişiyoruz: "/mnt/c/Program Files"
- MS Windows'tan WSL dosyalarına erişmek için:

  ```text
  \\\wsl$\ubuntu
  ```

  ```text
  \\\wsl$\distro-name
  ```
  
  direk olarak bu değeri path olarak kullanabiliriz. WSL içerisinde gömülü file server geliyor. MS Windows ise client tarafta bu file server'ı dinliyor. Bu sebeple WSL kapalı ise içindeki dosyalara erişemeyiz.

  Aynı zamanda bu durumun bilinen önemli bug'ları var: `https://devblogs.microsoft.com/commandline/whats-new-for-wsl-in-windows-10-version-1903/ "Known issues" başlığı.`

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 komut satırı path değişkenleri

OS aşağıdaki dosyaların içindeki komutları sırası ile işletir.

> /etc/profile  

her login sonrası (tüm user'lar için) çalıştırır.

> ~/.profile

her login sonrası bu home klasörünün sahibi olan user için bu dosyadaki komutlar çalıştırılır.

> ~/.bashrc

shell uygulaması başlatıldığında, ilk olarak bu dosyanın içindeki komutları sırası ile çalıştırılır. örneğin 'bash' komutunu çalıştırdığımızda buradaki kodlar tekrar çalıştırılır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 builtin (internal) command

bu komutlar birer program değildir. bu komutlar `terminal emulator`lerin sunduğu bir özellik değildir. bu komutlar `terminal interpreter`'ın sunduğu keyword'lerdir (özelliklerdir). örneğin `which echo` yazdığımızda sonuç alamayız. çünkü echo bir builtin command'dır.

Bazı terminaller yada OS'ler, shell builtin'leri ile aynı isimde programlar eklerler. bu durumda override eden (ki muhtemelen shell override edemeyecek) program çalışacaktır. Dolayısı ile komutlara aynı parametre yollasak bile farklı ortamlarda farklı sonuçlar alabiliriz.

"bash-builtins" komutu bize bash'in internal komutlarını listeler. bu liste aşağıdaki gibidir:

alias, bg, bind, break, builtin, case, cd, command, compgen, complete, continue, declare, dirs, disown, echo, enable, eval, exec,  exit,  export,  fc,  fg,  getopts, hash, help, history, if, jobs, kill, let, local, logout, popd, printf, pushd, pwd, read,  readonly,  return,  set,  shift,  shopt,  source, suspend,  test,  times,  trap,  type, typeset, ulimit, umask, unalias, unset, until, wait, while

isimleri ilginç fakat bunlarda builtin'dir:

```sh
:
```

```sh
exit 0 # (sağlıklı) dönüş yapan boş bir uygulamadır
```

```sh
.
```

source ile aynı görevi görür. parametre olarak source edilecek dosyayı alır.

```sh
[
```

çoğu yazılımcı bunu syntax olduğunu düşünür, oysa değildir. __"test"__ komutu ile aynı görevi görür.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

### 📌📌 sourcing script vs executing script

topic search keyword: source script vs execute script

```sh
source ./file.sh
# vs
./file.sh
```

İkisi de file'ı aynı şekilde işletir. fark şudur: source dosyayı current-process'te (current-shell'de) execute eder. bu sebeple file'ın değiştireceği her variable tüm shell akışı boyunca kalıcı olur. oysa "./file.sh" yeni bir sub-shell (sub-process) açılır ve file'ın oluşturduğu hiçbir variable/fonksiyon shell'de kalıcı olmaz.

source komutu yerine bu şekilde de kullanım aynı görevi görecektir:

```sh
. command
```

noktalı gösterim yerine source kullanmak daha best-practice'dir. çünkü "source" bir komut satırı uygulamasıdır. komut satırı olunca daha portable oluyor. çünkü built-in'ler her ortama göre değişebiliyor. bazı ortamlarda "." kullanılmıyor. örneğin fish'in güncel versiyonlarında "." kullanılmıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 coreutils

açık kaynaklı `C` ile yazılmış, hiçbir dependency gerektirmeyen en temel komut satırı uygulamalar grubudur. tümü bir `Git` repository'sinde düzenli olarak geliştirilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

### 📌📌 tty

`TeleTYpewriter`'ın kısaltmasından gelir.

Teknoloji dünyasında; __teleprinter (⟷ teletypewriter ⟷ teletype ⟷ TTY ⟷ tr:telem)__ bir metnin uzağa gönderilmesine yarayan fiziksel cihaz.

`tty` komut satırı uygulaması; `stdout`'a, `stdin` (stream) dosyasının full path'ini dönen komut satırı uygulamasıdır.

`tty`, `Linux console` gibi gerçek terminalin kullandığı dosyadır. `PTY (⟷ Pseudoterminal ⟷ Pseudo-TTY ⟷ Pseudotty ⟷ Pseudo-TTY)` dosyası ise, `terminal emulator` tarafından kullanılır.

örnek; `tty` komutu çıktımız `/dev/pts/0` olsun. farklı bir terminal session'ında `echo hello > /dev/pts/0` yazarsak, diğer session'daki `hello` `string`'i gözükür.

her açtığımız terminal session'ı farklı `tty` kullanır: `/dev/pts/0`, `/dev/pts/1`, `/dev/pts/2` gibi.

### 📌📌 pseudoterminal master (⟷ PTM ⟷ pseudoterminal leader) vs pseudoterminal slave (⟷ PTS ⟷ pseudoterminal slave)

her terminal session'ı için şu akış vardır:

```text
                                             ____________          ____________          ____________          ____________ 
   terminal emulator   ----> Input   ---->  |  PTY file  | ---->  | TTY Driver  | ----> |  PTY file  | ---->  |   Zsh      |
    or SSHD            <---- Output  <----  | (PTM file) | <----  | (OS kernel) | <---- | (PTS file) | <----  | (or Bash)  |
                                             ____________          ____________          ____________          ____________ 
```

`Linux console` için akış:

```text
                                           ____________          ____________ 
   Linux              ----> Input   ----> |  TTY file  | ---->  |   Zsh      |
   console            <---- Output  <---- |            | <----  | (or Bash)  |
                                           ____________          ____________ 
```

## 📌 interactive Shell

stdin ve stdout'ların son kullanıcının terminalinden okunduğu shell session'ıdır.

tersi durum non-interactive Shell'dir.

## 📌 login shell

--login ile açılan shell oturumudur. tersi durum non-login shell'dir. örneğin;

```sh
bash --option
```

--login ile açılan shell, user profilinde olan startup script'lerini (örnek: ~/.bash_profile, ~/.bash_login, ~/.profile) otomatik execute eder. exit yapıldığında da ~/.bash_logout dosyasını execute eder.

login ve interactive kavramları birbirinden bağımsızdır.

putty ile uzak makinaya bağlandığımızda login ve interactive shell oturumu kullanırız. İlgili oturumda komut satırına bash yazarsak, açılan yeni oturum non-login shell ve interactive'dir.

Eğer bir shell script'i koşuyorsak (terminalden veya terminalsiz), bu oturum non-login ve non-interactive'tir.
