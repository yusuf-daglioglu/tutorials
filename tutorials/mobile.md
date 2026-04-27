# MOBILE

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Android Support Library

birçok Android sürümü var. bazı Android sürümlerinde standart API'lerin bazıları kaldırılırken, bazıları da yeni eklenmektedir. Dolayısı ile bizim APK'mızın nerede çalışacağını bilemeyiz. Bu sebeple Google bazı API'leri, bağımsız jar dosyaları (dependency) olarak dağıtıyor. Bu şekilde API'yi APK'mıza gömmüş oluyoruz. Bu şekilde:

- eski bir cihazda, yeni bir Android sürümünün özelliğini kullanabiliyoruz.
- yeni bir cihazda, eski olduğu için kaldırılmış bir API'yi kullanabiliyoruz.

Tabi her Android özelliği, "support library" ile desteklenecek diye bir durum yok.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 SIM Application Toolkit (⟷ STK)

SIM kartın servis sağlayıcısı tarafından akıllı telefonlara otomatik yüklenen bir yazılımdır. bu uygulama ile müşteri bazı işlemleri gerçekleştirebilir. örneğin Türkiye'deki servis sağlayıcılar buradan haber alma servislerini aktifleştirebilmemizi sağlayan grafik arayüzü sunmaktadırlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Dalvik Virtual Machine (⟷ DVM)

`Android` altyapısında gömülü olan `JVM` türevidir.

`Android` için kodlar `.dex` uzantılı `Dalvik executable` dosyasına çevrilir. `DVM` bu dosyaları okur ve işletir. `JVM` için ".class" dosyası neyse, `DVM` için `.dex` dosyası da odur.

`Google`, `JIT` ile çalışan __DVM__ yerine artık, AOT çalışan "__Android Runtime (ART)__" geliştirmeye başlanmıştır. Android 4.4 ile birlikte her iki runtime çeşidi de kullanılabilmektedir. Android 5 ile sadece __ART__ kullanılmaya başlandı.

__Android NDK (⟷ Native Development Kit)__ ile `C` ve `C++` gibi diller ile Android'e kod yazılmasını sağlar. Uygulamalar daha hızlı çalışabilmektedir. fakat portabilite daha düşüktür.

ART native derleme yaparsa portabilite düşer. Bu sebeple şöyle bir çözüme gidilmiş: APK'da hala portable `bytecode` var. Ama Android'e kurulduğu anda native koda çevriliyor. Bu da kurulum süresini olumsuz etkiliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Android build version

- min SDK

  en düşük desteklenen Android sürümü

- target SDK

  spesifik bir örnekten gidersek; Android 6 ile permission-exception'lar geldi. biz bu sınıfı IDE'de görmek istiyorsak; target SDK'yı min 6 tutmalıyız.

- compile SDK

  sadece derleme aşamasında kullanılacak SDK versiyonu. genelde target'a eşittir. yada daha güncel derleyici ile kullanmak isteyebiliriz diye yükseltebilir. çünkü güncel compile SDK'ları kullanırsak, bir ihtimal APK'mız daha güncel Android'ler ile daha uyumlu çalışabilir. çünkü güncel API'ler ufak değişiklikler isteyerek bizim sürüm yükseltmeden daha güncel Android sürümlerine destek verebilmemizi sağlayacaktır.

- buildToolsVersion

  bu ise SDK içinde gelen tool'un versiyon numarasıdır.

bu kurala uyulmak zorundadır: min <= target <= compile

## 📌 Proguard

APK'nın boyutunu düşürme özellikleri içeriyor. build.Gradle'da;

- minifyEnabled

  true yapıldığında Proguard özelliği apk içindeki Java kodu sınıf/metot isimlerini kısaltıyor ve çağrılmayanları siliyor.

- shrinkResources

  kullanılmayan resource dosyalarını siliyor.

Proguard için rule dosyasına bazı rule'lar atanabiliyor. örneğin; bir sınıfın Proguard tarafından ignore edebiliriz. yada @Keep metodumuzu ignore etmek istediğimiz sınıfın tepesine atabiliriz. çünkü bazı sınıfları reflection kullanarak çağırıyoruz.

## 📌 Multidex

(`dex` dosyası başka başlıkta anlatılıyor)

`android` sisteminde dex dosyasının içinde 65bin metottan fazla sayıda metot bulundurulamıyor. `Multidex` özelliği `apk` içindeki `class.dex` dosyamızı parçalara bölüyor: `classes1.dex`, `classes2.dex`.

`Pre-dexing`: Java `Bytecode`'un `Android` `Bytecode`'una (`.dex`'e) çevrilme işlemidir.

bazı kütüphanelerimiz pre-dexing'i desteklemiyor ise; bu kütüphaneyi ignore etmeden `multidex` özelliğinden yararlanamayız.

`multidex` özelliği için compile dependency'mize `Multidex` `JAR`'ını eklemeliyiz. fakat `Android` `5+` ile birlikte artık dependency olarak eklememize gerek yok. direk `olarak` `Android` runtime birçok `dex` dosyasını `APK` içinden otomatik buluyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Kotlin

`JVM` ve `DVM` üzerinde çalışabilen (yani `Bytecode`'a çevrilebilen) ve `JS`'e çevrilebilen bir programlama dilidir.

`Android` bu dili resmi olarak desteklemektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Appium

Açık kaynaklı mobil platformlar için test yazılımıdır. Yazılım `selenium` ile aynı mantıkta çalışıyor. `Java` ve diğer programlama dili client'ları `selenium`'dan extend ederek geliştirilmiştir. Aynı `selenium`'daki gibi driver.`findBYId("loginButton").click();` gibi çalışmaktadır.

`Selenium` sunucusu yerine `Appium` sunucusu mevcuttur. Bu sunucu, `Appium`'da `NodeJS` için module olarak yazılmıştır.

`Selenium` türemesi olduğu için hibrit uygulamaları da kolayca test edebilmek için kullanılabilir.

## 📌 Appium-desktop

standalone bir uygulamadır. içinde embedded appium sunucusu çalıştırabilmektedir. istenirse de zaten var olan appium sunucusuna IP:port üzerinden bağlanabilmektedir. appium-desktop uygulaması emulator ekran görüntüsünü gösterip, ekrandaki elementleri inspect edebilmemizi ve inspect ederek seçtiğimiz elementler üzerinde selenium komutları çalıştırmamızı sağlamaktadır. örneğin bir button seçip tıklamamızı sağlıyor. bu işlemleri appium-desktop uygulaması üzerinden yaptığımız için her adımımızı kaydediyor ve selenium Java kodu ortaya çıkarıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 MS Windows mobile vs MS Windows phone

MS Windows phone, artık MS Windows mobile olarak isimlendiriliyor. yıllara göre çıkan sürümler aşağıdadır:

- Windows phone 7 sürümü - 2010
- Windows Phone 8 sürümü - 2012
- Windows Phone 8.1 sürümü - 2014
- Windows 10 Mobile sürümü - 2015

MS Windows 10 mobile projesi resmi olarak sonlandırılmıştır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 .nomedia

Android, bu dosyayı gördüğü klasörlerdeki media dosyalarını (video, resim...), "galeri" gibi son kullanıcı için geliştirilmiş uygulamalarda göstermemektedir. Örneğin; thumbnail dosyalarının bulunduğu bir dizinde bu dosya olmalıdır. Bu dosyanın içi boştur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Android SDK

Android SDK tamamen kurulumsuz ve IDE'den bağımsızdır.

sadece SDK indirildiğinde

> SDK_root/tools/android.bat SDK

yada

> SDK_root/tools/android.bat avd

komutları ile GUI'ler başlatılabiliyordu. fakat güncel sürümlerde (2017 sonrası) artık sadece komut satırından kullanılabilmektedir. Bu sebeple Google'nin sitesinde SDK tek başına "command line tools" ismi ile geçmektedir. Google "Android Studio" içerisinde gömülü GUI barındırmaktadır.

### 📌📌 SDK package details

### 📌📌 SDK Platform sekmesindekiler

- Google play ürünleri emulatorde birebir aynı davranıyor. çoğu Google uygulaması ilk girişte hesap istiyor.

- "Google APIs" prefix'i içerenler

  Google services'in bazı kütüphanelerini de içeriyor.

- "Google play" prefix'i içerenler

  hem Google API's pefix'i içerenleri hem de Google play uygulamasını yüklü getiriyor.

- images'lerden biri seçilmezse emulator başlatılamaz.

- INTEL Atom  vs ARM EABI

  Intel Atom olanlar Intel masaüstü `CPU`'larda daha hızlı çalışıyor. oysa ARM EABI her komutu simüle ediyor. bu sebeple arm çok yavaş kalıyor.

- Android SDK Platform

  kütüphanenin jar'larını indiriyor. jar'ların kodlarına bazen bakmak istenebilir. bu durumda ilgili sürümün "sources for Android" paketi indirilmelidir.

### 📌📌 SDK Tools sekmesindekiler

- Android SDK Build-Tools

  derleme/apk üretme işlemleri için gerekli paket.

- Android SDK Platform-Tools

  adb komut satırı uygulamasını içeriyor.

- Android SDK Tools

  SDK-manager + `AVD (⟷ Android virtual device)` manager + debugging tools gibi Android geliştiricisinin ihtiyaç duyduğu tool'ları barındırıyor.

- Android Emulator

- diğerleri...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Phonegap vs Cordova vs ionic

Cordova; npm ile kurulabilen, NodeJS üzerinde çalışan hibrit mobil uygulaması oluşturmaya yarayan komut satırı tool'udur.

phonegap ise bu tool üzerine kurulan ve ek hizmetler sunan bir framework'tür. Cordova'ya göre ek özellikler:

- mobil uygulamaları bulutta paketleyen yapı

- daha zengin komut satırı uygulaması ile proje ayarlamaları daha kolaylaştırıldı

- komut satırı uygulamasına ek olarak GUI içeren bir yazılım. bu şekilde uygulamalar kolayca testte (preview) edilebiliyor.

- Test süreçlerini kısaltmak için geliştirme ortamı dependency'lerin daha az olduğu bir OS'ta test işlemleri kolay ve hızlı bir şekilde yapılabiliyor.

ionic ise yine Cordova temelli, phonegap'e benzer ek seçenekler sunan bir tool. Cordova'ya göre ek özelliklerin bazıları şunlardır:

- bulutta derleme işlemleri

- hazır notification servisleri

- programlama bilmeden uygulama yapma imkanı

- native UI veren bir hali hazırda temalara sahip olması

- uygulamaların marketten onay almadan, kısmı güncelleme yapabilmeleri

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Airplane mode (⟷ uçak mode'u ⟷ airplane mode ⟷ flight mode ⟷ offline mode)

neyin açık neyin kapalı olacağı, `OS`'lar veya donanımlar ve bunların sürümleri arasında hala farklılıklar var.

Bluetooth, telephony, `Wi-Fi`, `NFC`, radio `GPS` disable bırakılıyor. fakat kullanıcı hala airplane mode'dayken, isterse telefon hattı hariç diğer modülleri aktif edebiliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Mobil OS'lar

| Name         | Open-source | Kernel    | development status | companies supported                     |
|--------------|-------------|-----------|--------------------|-----------------------------------------|
| Palm OS      | no          | Linux     | discontinued       | Palm                                    |
| WebOS        | yes         | Linux     | discontinued       | HP, LG, Palm                            |
| MeeGo        | yes         | Linux     | discontinued       | Nokia                                   |
| Symbian      | no          | -         | discontinued       | Nokia                                   |
| Bada         | no          | -         | discontinued       | Samsung                                 |
| Firefox OS   | yes         | Linux     | discontinued       | Mozilla                                 |
| FireOS       | no          | Android   | continue           | Amazon (Kindle e-reader cihazları için) |
| BlackBerry   | no          | UNIX-like | continue           | BlackBerry                              |
| Tizen        | yes         | Linux     | continue           | Samsung                                 |
| Ubuntu Touch | yes         | Linux     | continue           | Canonical                               |
| Sailfish     | yes         | Linux     | continue           |                                         |

Tizen, 2024 yılında anda en popüler Samsung Galaxy Gear cihazlarda kullanılıyor. "Window manager" olarak, "Enlightenment Window manager" projesi altında geliştirilen, "Enlightenment Foundation Libraries (⟷ EFL)" kullanmaktadır.

## 📌 Bazı Custom Rom'lar

| name                      | development status | note                                                      |
|---------------------------|--------------------|-----------------------------------------------------------|
| OmniROM                   | discontinued       | sadece açık kaynaklı proje                                |
| replicant                 | discontinued       | sadece açık kaynaklı proje                                |
| Paranoid Android          | continue           | sadece açık kaynaklı proje                                |
| CyanogenMod               | discontinued       |                                                           |
| MUIU                      | continue           | çok fazla kapalı kaynak kod içeriyor.                     |
| Android Open Kang Project | continue           | sadece açık kaynaklı kodlar içeriyor.                     |
| LineageOS                 | continue           | CyanogenMod projesi iptal edince community buna çatalladı |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 F-Droid

sadece açık kaynaklı uygulamaları barındıran (Google Play'e alternatif) market uygulaması.

## 📌 Kies

Samsung'un kendi cihazları için itunes alternatifi uygulamadır. sadece MacOS ve MS Windows'a yüklenebilmektedir. data backup, photo management, OS update gibi birçok işlem yapabilmemizi sağlar.

## 📌 ClockworkMod (⟷ CWM ⟷ CWMR) Recovery

açık kaynaklı Android'ler için geliştirilmiş bir recovery yazılımıdır. yüklendiği zaman, telefondaki stock recovery yazılımın yerine geçmekte ve stock'takine göre bir çok ek özellik sunmaktadır. CWM; SD karttaki Zip dosyalarından, cihaza ROM yükleyebilmektedir.

## 📌 Team Win Recovery Project (⟷ TWRP)

açık kaynaklı CWM alternatifi (GUI içeren) bir yazılımdır.

## 📌 PhilZ Touch

Recovery uygulamasıdır. artık geliştirilmemektedir.

## 📌 Heimdall

cross platform çalışabilen, açık kaynaklı Odin alternatifidir. odin'de olduğu gibi sadece Samsung cihazlar için çalışmaktadır.

## 📌 jailbreaking

sadece iOS OS'ları için geçerli olan bir kelime. iOS sisteminde root haklarını ele geçirmeye işlemidir. jailbreaking genel bir terimdir. jailbreaking için birçok açık yada kapalı kaynak kod tool vardır.

## 📌 AOSP (⟷ Android Open Source Project)

Google'ın Android projesinin genel adıdır.

## 📌 Boot ROM code (⟷ boot rom)

BIOS/UEFI'ye denk gelmektedir.

Mobil cihazlarda bootloader'ın bulunduğu kısım önceden belirlidir. Bu sebeple "Boot ROM code" direk olarak cihazın sürücüsündeki Bootloader'ı çalıştırmaktadır.

## 📌 Android bootloader

Android bootloader şu 3ünü açabilir: recovery (örnek: ClockworkMod), system (OS'u açar), radio (cihazın antenini kontrol etmek amaçlı bir arayüzdür).

Cihaz açıldığında bootloader açılır, fakat bootloader genelde neyi açayım diye arayüzden sormaz. genelde tuş kombinasyonları ile bu 3 seçenekten (bazen daha fazlası olabiliyor) biri seçilmiş oluyor. Örneğin Samsung'larda; home+volume-up+power tuşu, bootloader'ı açar ve recovery'yi run etmesini seçer. sadece power tuşu; bootloader'ı açar ve Linux'u (Android'i) run etmesini seçer.

Diskin /boot bölümünde bootloader dosyaları yer alır.

Qualcomm'un Boot Loader ismi LK (Little Kernel)'dir.

## 📌 rom

rom OS'u içeren fiziksel parçanın (disk) içindeki yazılımsal bölüme (partition) verilen isimdir. ROM'un bulunduğu parçaya (diske) normal HDD gibi yazılamadığından, oraya yazılma işlemi "flash" olarak adlandırılmaktadır.

mobil/tablet'ler birer gömülü cihaz olduğundan, içindeki yazılım (burada Android oluyor) firmware olarak adlandırılırlar. yani; custom rom yüklediğimizde; ROM'a yeni firmware yüklemiş (flash'lamış) oluruz. diğer bir değiş ile; rom'a yeni rom yüklemiş oluruz. fakat halk arasında bu tabirler farklı kelimelerle anlatılıyor.

ROM içerisinde Android'den kalan kısım, "internal storage" olarak son kullanıcı tarafından kullanılmaktadır.

## 📌 rooting

Android cihazlarda root'lama işlemi admin yetkilerini ele alma olarak adlandırılır. custom rom yükleme  anlama gelmektedir.

## 📌 ADB (⟷ Android Debug Bridge)

ADB, erişilen cihazda (Wi-Fi, yada USB takılı iken) Android açıkken işlem yapmamızı sağlayan komut satırı uygulamasıdır.

ADB, Android SDK içerisinde gelmektedir.

ADB'ye cevap vermesi için Android OS runtime'ında özel bir process olmalıdır. Bazı farklı mode'larda (OS açılmamış mode'lar recovery, bootloader gibi), ADB'nin tüm komutlarına olmasa da kısmen komutlarını destekleyen yazılımlar çalıştırılıyor. Bu şekilde kısmen ADB komutları çalıştırılabiliyor.

## 📌 Fastboot

ADB ile aynı mantıktadır. Fakat asıl amacı; Android çalışmıyorkenki komutları cihazda çalıştırmaktadır. Cihaz fast-boot mode'da çalışıyor denildiğinde, o andaki mode (recovery, bootloader gibi) fastboot komutlarını destekliyor anlamına gelir.

## 📌 Odin

Samsung cihazlara ROM flash'lamak için geliştirilmiş kapalı kaynak bir MS Windows uygulamasıdır.

## 📌 download mode (⟷ Odin mode)

o andaki mode (recovery, bootloader gibi) Odin uygulamasının komutlarını destekliyor anlamına gelir.

## 📌 yalp store

Açık kaynaklı Android uygulamasıdır. "Google play services" kurulu olmayan cihazda marketten uygulama indirmeye yarıyor. Google hesabı uygulamaya veriliyor. uygulama markete bağlanıp apk'yı indiriyor ve kurulumu son kullanıcıya açıyor.

## 📌 Open GAPPS

Google'ın uygulamaları (Gmail, youtube gibi) kapalı kaynaktır ve AOSP'ye dahil edilmez. lisans sebebi ile bazı custom rom'larla (Google ile anlaşma yapılmadığı sürece) yüklü gelemez. Açık kaynak community, tüm Google uygulamalarını paketleyerek dağıtmaktadır. Bu dağıtılan hazır paketler GAPP diye adlandırırlar. GAPPS paket'inin neler içerdiği burada detaylı yazmaktadır: <https://github.com/opengapps/opengapps/wiki/Package-Comparison> Bu sayfadan AOSP projesinde hangi uygulamaların dahil olup olmadığı anlaşılamaz. çünkü bazı uygulamalar AOSP içerisinde de vardır. Fakat Chromium/Chrome gibi dağıtılır. örneğin; keyboard. "AOSP Keyboard" vardır, bide "Google Keyboard".

GAPPS Paket farklılıkları:

- ARM - 64/32 - Intel mimarilerinin kombinasyonları farklı kurulum paketinde.

- Her Android sürümü için uygun/ paket dağıtılıyor.

- Yukarıdakilerin kombinasyonu ile dağıtılan paketler:

  - Pico: Google uygulamalarının Android'de çalışabilmesi için şart olan arka plan uygulamaları vardır. sadece bunlar yükleniyor. en ufak paket budur.

  - Nano: pico + some apps

  - Micro: Nano + some apps

  - Mini: Micro + some apps

  - Full:  Mini + some apps

  - Stock: nexus telefonlardaki uygulamalar

  - Super: stock + Kalan diğer tüm uygulamalar

  - TVStock: Android TV'ler için ilgili tüm uygulamalar

  - Aroma: "Stock" paketi ile aynı paketleri içeriyor. sadece bir arayüz ile nelerin kurulacağını son kullanıcıya bırakıyor.

## 📌 gapps Google paketlerini nereden buluyor

`Google`'ın ürünleri kapalı kaynak ve birçoğu markette dahi yayınlanmıyor. Open GAPPS paketleri elde etmek için şu yolu izliyor: `Google` resmi sitesinde nexus ve pixel cihazlar için Android'i komple image olarak indirmeye sunuyor. simülatörde ve normal cihaza kurabilmek amaçlı. Open GAPPS bunu simülatörde açık, telefona root yetkilerini açık, apk'ları çekiyor.

## 📌 Google Play Services

GAPPS'in tüm paketlerinde olan uygulama. bu uygulama sadece arka planda çalışır ve diğer tüm Google uygulamaları için zorunludur. Public API'leri mevcuttur. Bunları dışarıdan farklı uygulamalar kullanabilir. Bu API'lere depend olan uygulamalar Google ürünü içermeyen custom rom'larda çalışmayacaktır. Bu sebeple bazı açık kaynaklı uygulamalar (örneğin; `microG (⟷ old-name:NOGAPPS)`), bazı Google API'leri ile aynı paket ismi (Java paket isimleri aynı) içeriyor ve aynı API'leri aynı isimlerle alternatif işlem yaparak sağlıyor. Fakat henüz (2017) piyasada bu API'leri karşılayacak stabil uygulamalar mevcut değildir. Bu API'lerin bazıları; location hizmetleri (açık kaynaklılar bu API'yi Apple, Mozilla Location gibi servislere bağlıyor), notification sistemi, Google hesabı aracılığı ile OAuth, Google analytics, Google maps'ten harita ve görüntü hizmetleri almak (açık kaynaklılar bunu openstreetmaps'e bağlıyor).

## 📌 com.Google.Android.Googlequicksearchbox

Google arama uygulamasının paket ID'si. ismi sadece "Google" olarak markette dağıtılmaktadır. bazı kaynaklarda "Google app" yada "Google app (search)" olarak geçmektedir.

## 📌 Android launcher

MS Windows ve iOS'ta henüz bulunmayan ana ekran yöneticisidir. masaüstü OS'larda masaüstü yöneticisinin mobildeki karşılığı olarak düşünülebilir. bu yazılım diğer Android uygulamaları gibi basitçe kurulabilmekte ve varsayılan olarak belirlenen uygulama ana ekranda açılmaktadır. yüzlerce launcher vardır. aşağıdaki listenin tümü Google marketten indirip herhangi bir telefona kurulabiliyor:

- `Google now launcher (⟷ tr:Google asistan launcher)`

  `com.Google.Android.launcher` paket ID'li launcher.

- `Pixel launcher`

  "com.Google.Android.apps.nexuslauncher" ID'li Google'ın pixel serisi telefonlar için geliştirdiği alternatif launcher.

- `Samsung Experience (⟷ old-name:Samsung TouchWiz Home)`

  "com.sec.Android.app.launcher" paket ID'li launcher. Samsung cihazlarında yüklü geliyor.

- "com.benny.openlauncher"

  a community launcher

## 📌 Google now (⟷ tr:Google Asistan)

"Google now", siri'ye alternatif bir özelliktir. uygulama değildir, çünkü tek başına kullanılamaz. bu sebeple; Googlequicksearchbox içerisinde bulunuyor.

com.Google.Android.launcher'in içinde direk bu özellik gelmiyor, fakat com.Google.Android.launcher, Googlequicksearchbox'e bağımlı. benzer şekilde com.Google.Android.apps.nexuslauncher'da Googlequicksearchbox'a bağımlı.

## 📌 Google Home

paket ID'si: com.Google.Android.apps.chromecast.app

Google'ın speak donanım ürünü. bu cihazı kontrol edebilmek için Android ve iOS için uygulama mevcut. Android'deki uygulamanın adı "Google Home". Bu uygulama ek olarak Chromecast içinde bazı özellikler içeriyor.

## 📌 widget

her Android launcher'ın widget desteği yoktur.

## 📌 com.Google.Android.apps.messaging

Google'ın SMS, mms uygulaması'nın paket ID'si. bu uygulama aynı network'tekilerin haberleşmesini sağlayan ek özellikler de içeriyor.

## 📌 com.Android.mms

AOSP'ye dahil olan SMS ve mms uygulaması.

## 📌 com.Android.documentsui

bir uygulamanın file manager ile dosya seçtirmek istemesiyle açılacak olan pencere için gerekli uygulamanın paket ID'si. uygulama ismi sadece "Files" diye geçiyor.

## 📌 com.Android.settings

settings uygulamasının ID'si.

## 📌 paket ID'leri

com.Android.* : AOSP projesi içine dahil olan açık kaynaklı uygulamalar.

com.sec.* : Samsung'un uygulamaları

com.Google.Android.# : Google'nin kapalı kaynak uygulamaları

## 📌 Xposed Framework

Açık kaynaklı bu proje, root'lu Android'lerde değişiklik yapmayı sağlıyor. bu şekilde custom rom kurmadan sadece root yetkisini alarak da birçok custom rom'da olan özelliği kullanabiliyoruz. hatta custom rom kurup ondan olmayan bir özelliği de kullanabiliriz. framework hiçbir yetenek içermiyor. her yetenek onu kullanan modülleri yüklediğimizde geliyor. framework sistemin dosyalarını değiştirmiyor, sadece RAM üzerinde değişiklik yapıyor. bu sebeple modüllerden biri sisteme zarar verirse, çok rahatlıkla devre dışı bırakılabiliyor.

## 📌 Android one vs nexus vs pixel

Google donanım geliştiricileri ne(dağıtıcılara) destek çıktığı projedir.

bu projenin kapsamı değiştirildikçe isim de değiştirildi. sırası ile:

android one, nexus, pixel isimlerini aldı.

"__Nexus One__" ilk çıkarılan nexus cihazının modeli.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 WebView

## 📌 Android için webview

Android activity içerisinde buttonview, textview kullanırmış gibi webview'da kullanımı aynıdır:

```java
WebView myWebView = (WebView) findViewById(R.id.webview);

WebSettings webSettings = myWebView.getSettings();

webSettings.setJavaScriptEnabled(true);
```

Eğer tüm sayfayı kaplaması isteniyor ise activity'nin full screen olması, varsa webview'ın border'ları kapatılmalı ve webview'ın bulunduğu layout tüm sayfayı kaplamalıdır. bu şekilde bir tarayıcıymış gibi kullanılabilir.

## 📌 Android üzerinde custom component

Android platformu hem fully hem de basic olarak 2 farklı çeşit custom element (Android'de view) yaratmamıza olanak sunuyor. basit custom element, hali hazırda var olan bir elementin sınıfını extend ederek, metotlarını override ederek, yada yeni metot ekleyerek, yeni element yaratmamıza olanak sağlıyor. Fully custom elementlerde ise, çizim gibi metotları da kullanıp tümüyle sıfırdan yeni bir element yaratmamıza izin veriyor.

## 📌 Android System Webview

paket ID: com.Google.Android.webview

Android artık webview'ı ayrı bir apk olarak marketten sunmakta (çoğu telefonda default yüklü gelmektedir). Google Chrome tarayıcısının yüklü olması ile alakasız bir durum. Android eskiden, default tarayıcısının kodlarına dayalı bir webview kullanıyordu. 5. sürüm ve sonrasında Chromium (Chrome) tabanlı bir webview'a geçildi. sistemde birden fazla webview olabilir. yazılımcılar webview kullanmak istediklerinde import ettikleri kütüphane'ye göre hangi webview'a erişeceklerine karar verirler. Chrome öncesi olan webview kütüphanesi: com.Android.webview

## 📌 WebChromeClient vs WebViewClient

"Android Webview"ın içinde olan/kullanılan iki variable'dır. bunlar dışarıda set edilebilir şekilde ayarlanmışlardır. WebChromeClient içerdiği metotlar, ilgili webview'ın loading bar, alert/popup, page title, favicons gibi HTML DOM'u dışında kalan işlerin event'lerinde çağrılırlar. WebViewClient ise; HTML DOM event'lerinde çağrılırlar. örneğin; sayfa yüklendiğinde, sayfa URL'ye istek yaptığında gibi...

## 📌 Firefox based webview

Firefox'un webview'ı için stabil bir çalışma yok.

## 📌 Cordova webview

CordovaWebView custom bir webview'dır ve eklentiler ile bilgi alışverişi yapabilecek şekilde tasarlanmıştır. OS'taki default webview'dan extends edilip özellikler eklenmiştir.

## 📌 Android default browser

"com.Android.browser" paket ID'si ile kullanılan browser'dır. resmi olarak özel bir ismi olmadığı için "stock browser", "Android browser", "webkit bundled browser" gibi isimlerle anılmaktadır.

## 📌 Google Chrome (paket ID: com.Android.Chrome) varken neden hala Android default tarayıcısı yüklü geliştiriliyor?

- Android 4.4 sonrası nexus serili telefonlarda Google Chrome tarayıcısı default gelmektedir. fakat diğer donanım firmaları Google ürünlerine bağımlılık yaratmak istemediğinden Chrome'u yüklü sunmamaktadırlar.

- uygulamaların webview'ları buna bağlı olduğundan, Chrome yüklü gelirse uygulamaların çalışması da riskli olacağından kimse Chrome yüklü getirmiyor.

- "Android default browser" daha hafif olduğunda bazı üreticiler kötü donanımlarında hala bu tarayıcıyı kullanmaktadır.

- donanım firmaları lisansı gereği default tarayıcıya patch ekleyebilirken, Chrome'a ekleyemiyorlar. bu sebeple default tarayıcı yüklü getiriliyor.

## 📌 Apple WebView

Apple üzerinde custom webview kullanma olanağı yok. Apple'ın kendi webview'ı (yeni adı:WKWebView, old-name:UIWebView) kullanılmalıdır.

## 📌 Crosswalk

Proje Cordova projesinden türemiştir. Birçok ek özellik sunar. Temel özelliği Android'lerde Chromium tarayıcısına dayalı webview yaratıp, uygulamamızın apk'sına gömmeleri ve her Android cihazda aynı kalmasıdır. fakat Android 5'ten sonra, Android default webview Chrome'a dayalı olacağı için, crosswalk projesinin bu avantajı ortadan kalkmıştır.

avantajları ise;

- hala Chrome webview'ın Android'lerde default yüklü gelmemesi

- Chrome sürümünün her Android sisteminde değişik olması. crosswalk, kendi içinde kütüphane olarak barındırıyor webviewe'ı. yazılımcı fix olarak bir sürüm seçebiliyor.

## 📌 Android Custom Tabs

Android uygulaması içerisinde yeni bir tarayıcı penceresi açmadan kullanıcıya tarayıcı kullandırtılır. Tüm ekranı kaplaması şarttır. Fakat tarayıcı özelleştirilebilir. Önceden menüleri ve işlevleri set edilebilir gibi. Eğer OS'ta o anda Google Chrome tarayıcısı kurulu değil ise; uygulama yeni bir pencerede default tarayıcıyı açacaktır.

## 📌 Safari View Controller

Android Custom Tabs'ın Apple'daki versiyonu.

## 📌 Apple üzerinde diğer tarayıcılar

Apple üzerinde yüklenen tarayıcılar Apple politikası sebebi ile Apple tarayıcısının rendering mekanizmasını kullanmak zorundadır.

## 📌 Cordova-plugin-inappbrowser Eklentisi

Bu eklenti ile; bir URL, kullanılan webview'da yada sistemin default tarayıcısını açarak gidilmesini sağlıyor. Aynı zamanda ek olarak şu özelliği de sunuyor: yeni bir dialog (popup dialog'daki dialog altyapısı ile ufak bir blok) içinde; geri, ileri, dur button'ları ve farklı bir de webview elementi olan layout yaratıyor (Java kodu ile) yaratıyor. bu dışarıdan bakıldığında Android Custom Tabs ve Safari View Controller'a çok benziyor. bu dialog içerisinde bir web sitesi açılmasını sağlıyor.

## 📌 Samsung internet

paket ID: com.sec.Android.app.sbrowser

Samsung kendi telefonlarında 2017 öncesi stock Android browser'ı yüklü getiriyordu. 2017 ve sonrasında Chromium tabanlı kendi tarayıcısını yüklü getirdi ve marketten indirmeye açtı.

## 📌 Progressive Web App (⟷ PWA)

Progressive kelime anlamı: yenilikçi, ilerleyici (hastalık).

Web tarayıcılarının direk desteklediği bir tekniktir. Ziyaret edilen web sayfasının HTML>HEAD>META etiketlerinde eğer PWA tanımlamaları yapılmış ise, web tarayıcı son kullanıcıya bu siteyi native-OS uygulaması olarak kurulmuş gibi göstermesi için bir opsiyon sunar. Eğer son kullanıcı bu opsiyonu seçerse, web tarayıcı şu gibi aksiyonlar alır:

- web sayfasına ait tüm HTML, CSS, JS dosyalarını local'e (son kullanıcının göremediği bir dizine) indirir
- masaüstüne veya farklı GUI noktalarına bu siteyi açan bir kısa yol koyar
- son kullanıcı uygulamayı tekrar açtığında, HTML, CSS, JS dosyalarını önce local'den açar, ama paralelden sürekli bu dosyaları server üzerinden günceller.
- açılan PWA'nın sayfasında URL bar yoktur. Bu şekilde native uygulama gibi görünür.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Push Notifications

bir uygulama tüm mobil sistemlerde istediği anda kod akışından son kullanıcıya "notification" gösterebilir. "push notification"lar ise bu durumdan farklıdır.

## 📌 Push notification işleyişi

push notification için OS sunucuları devreye girer:

- `Android`'ler; `Firebase Cloud Messaging (⟷ GCM ⟷ old-name:Google cloud messaging)`

- `ios`'lar; `Apple Push Notification Service (⟷ APNS)`

- `MS Windows`'lar (masaüstü ve mobil); `Windows Push Notification Service (⟷ WNS)`

## 📌 hayat döngüsü

- app, OS'tan bir key alır.
- app, OS'un push-server'ına (apns, wpns gibi) bu key ile mesaj yollar.
- OS'un push-server'ı, bu mesajı ilgili client'a (OS'a) yollar.

## 📌 Amazon Simple Notification Service (⟷ SNS)

- Amazon firmasının bulut servisidir.
- Burası OS push-server'ı gibi çalışıyor. Ama tüm OS push-server'ları için ortak bir API sunuyor.
- Aslında arka planda bizim için OS'un push-server'larına kendi istek atıyor.
- Sadece push notification değil, aynı zamanda email ve SMS desteği de sunuyor.

## 📌 Push notification bulut hizmetleri yerine MQTT kullanımı

`MQTT` açık kaynaklı standartlaşmış bir protocol'dür. `MQTT`, `Facebook` messenger gibi uygulamalarca tercih edilmektedir. `MQTT` için bazı sebepler:

- Mobil OS'un bulut hizmetleri yapılan notification sayısına limit koyabiliyor (veya ek ücret talep edebiliyor)

- mobil OS bulut servisi anlık mesajları yollamıyor (özellikle anlık mesaj uygulamaları için büyük sıkıntı)

- mobil OS bulut hizmeti custom rom'larda yüklü olmayabiliyor

## 📌 Apple'da mqtt kullanma

`Apple` mobil OS'larında background process'lere izin vermiyor. dolayısı ile; push yerine `mqtt` kullanmak imkansız. Apple yeni sürümlerinde sadece mesajlaşma uygulamalarına yada bazı kriterlere göre arka plan process'lerine izin vermektedir. bazı kısıtlamalar olsa da kullanılabilmektedir.

## 📌 MQTT (⟷ Message Queuing Telemetry Transport)

`Kafka` gibi subscribe ve `topic` mantığı vardır.

### 📌📌 QoS

Mesaj gönderen tarafındaki her mesajın iletim kalitesini (logic'ini) belirlenir.

| `QoS` Seviyesi | mesaj client'a iletilir mi                             | aynı mesaj birden fazla iletilir mi | hafif |
|----------------|--------------------------------------------------------|-------------------------------------|-------|
| 0              | disconnected client'lara iletilmez (`fire-and-forget`) |                                     | hafif |
| 1              | kesin iletilir.                                        | olabilir                            | orta  |
| 2              | kesin iletilir.                                        | hayır                               | ağır  |

## 📌 Paho

- açık kaynaklı Client API for MQTT.
- API, `Java` başta olmak üzere birçok major dil için yazılmıştır.
- `JS` API'si `TCP` ile sunucuya bağlanamıyor. Bu sebeple `JS` `WebSocket`'ten yararlanılıyor.

## 📌 Mosquitto

- açık kaynaklı `MQTT` sunucusudur.
- broker olarak geçer.

## 📌 mqtt-spy

`MQTT` GUI client app (test amaçlı kullanılır).

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
