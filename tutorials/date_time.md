# DATE TIME

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 zaman

saniyenin tanımı `International System of Units (⟷ SI)`'da şu şekildedir: `SI` tarafından belirlenen bir atom çeşidinin, belli bir sıcaklık değerindeykenki bir davranışının periyodudur.

Bir saniyeye denk gelen birçok farklı yöntem geliştirilmiştir. Yani her analog veya digital saatler içeren cihaz, `zaman` tanımındaki durumu tam olarak implemente ederek çalışmaz. Tabi durum böyle olunca, her cihaz yıllar sonra ufak sapmalar meydana getirmektedir. Bu sebeple bazı sistemlerde şu çözümlere gitmek gerekebilir:

- belli aralıklarla bulut sistemlerle sync olma
- kendinin ne kadar zamanda, ne kadar sapma yaptığını bilen cihazlar, o aralıkta saniyeleri ileri veya geri alabilir.

## 📌 TimeStamp (⟷ Zaman damgası)

Fiziksel olarak `TimeStamp`; mürekkep ile belge üzerine basılan tarihtir. bu istenilen bir formatta olabilir. örneğin; etrafı çerçeveli, sadece gün, ay ve yıl olan, ek olarak saati içerebilir gibi. Aynı şekilde elektronik ortamda da `TimeStamp` kullanılmaktadır. Bu değer istenilen formatta olabilir ve bazen sadece hesaplama ile bulunacak bir sayı ile temsil edilir.

- `örnek 1`: `POSIX time`

- `örnek 2`: `Tue 01-01-2009 6:00`

- `örnek 3`: `07:38, 11 December 2012 (UTC)`

## 📌 Unix-Epoch time (⟷ POSIX time ⟷ Unix time ⟷ seconds since the Epoch)

`POSIX` standartlarında yazmaktadır. Özel bir ismi yoktur.

`POSIX` dökümanlarında `Epoch`'un hangi tarihi ifade ettiği açıkça belirtilmektedir. Çünkü kelime anlamında `Epoch` spesifik bir tarihe denk gelmez.

1 Ocak 1970 `UTC+0` Greenwich (`Londra`'daki bir semt) tarihinin gece yarısından bu yana geçen zamanın saniyeler cinsinden değeridir.

terminolojik olarak bakıldığında; herhangi bir formatta tarih gösterimi `timestamp` olduğundan, `POSIX time` da bir çeşit `timestamp`tir.

`Unix Epoch` veya sadece `Epoch` terimi genelde piyasada `1 Ocak 1970 UTC+0` tarihini temsil ediyor. Bu sebeple `E` harfini özel isim gibi büyük yazanlar var.

## 📌 gösterim

zaman dilimi dünyanın herhangi bir yerinde aynıdır. sadece bölgesel olarak gösterilmek istendiğinde `UTC+2` gibi ibarelerle gösterilirler. örneğin; aynı anda oluşturulan iki `Java`'nın `calendar` objesi olsun. biri `Amerika`, biri de `Türkiye` bölgesine göre ayarlı olsun. bu değerler sadece ekrana basılırken farklıdır, aslında kendi içlerinde tuttukları `zaman` değeri ikisinde de aynıdır. yani; bu 2 calendar objesinin `POSIX time` değerini alsak, aynı değeri alırız.

## 📌 GMT (⟷ Greenwich Mean Time)

`Londra`'nın `Greenwich` bölgesinden geçen sıfır meridyeninin referans alınarak yapılan saat dilimi sistemidir.

12 saat doğuda, 12 saat batıda parçalara bölünmüştür.

## 📌 Universal Time (⟷ UT)

birçok versiyonu vardır. bunlar sürüm gibi düşünülmemelidir. hepsi birbirine alternatiftir.

- `UTC (⟷ Coordinated Universal Time)`
- `UT0`
- `UT1`
- `UT1R`
- `UT2`

## 📌 Atom saati (⟷ atomic clock)

atom seviyelerinde hesaplamalar yaparak çalışan fiziksel saatlerdir. birçok çeşit başarılı atom saati yapılmıştır. Atom saatlerinde sapmalar yazılımlarda değerlendirilmeyecek kadar küçüktür. örnek 3 milyonda 1 saniye sapma oluyor.

## 📌 International Atomic Time (⟷ TAI)

`Fransızca`'daki terimin kısaltmasıdır.

`International Bureau of Weights and Measures (⟷ BIPM)` ve `International Committee for Weights and Measures (⟷ CIPM)` kuruluşları gerçek saniyenin şu anda ne olduğu hesaplayabilmek için dünyanın birçok farklı bağımsız otoritesinden (2020 yılında 400 farklı otoriteden) güncel saniye bilgilerini almaktadır ve bunun ortalamasını yayınlamaktadır. Her bağımsız otoritenin kendi atom saati vardır.

## 📌 Coordinated Universal Time (⟷ UTC ⟷ Eş Güdümlü Evrensel Zaman)

- `UTC` kısaltması, `Coordinated Universal Time`'ın baş harflerinden elde edilmemektedir. bunun sebebi tamamen siyasidir.

- `UTC`, saniyeleri, `IS` standartlarına göre baz alır (`IS`'de buna `atomik saniye` denir). Oysa `GMT`, saniyeyi astronomik değerlere göre baz alır. `UTC` bu sebeple daha güvenlidir. `UTC`, fiziksel cisimlerin (dünyanın ve çevresindeki tüm fiziksel çekim güçlerinin) duruma göre değişmez. `GMT`'nin açılımı `Greenwich Mean Time`'dır. Buradaki `Mean`, ortalama anlamına gelir. Yani `GMT`'de bir yıl, güneşin ve dünyanın dönüşünü tamamlayacak ortalama en optimum düzeyde hesaplanmıştır ve bu şekilde kabul edilir. Yani dünya yavaş veya hızlı dönüşüne göre `GMT`'de, 1 saniyeye karşılık gelen zaman dilimi değişmektedir. Oysa 1 saniye `UTC`'de sürekli sabittir.

- `GMT` ve `UTC` arasında çok çok az bir zaman farkı vardır. Bu fark zaman geçtikçe artabilir veya azalabilir. Çok ilerideki tarihlerde fark çok açılabilir. 2019 yılına kadar, aradaki fark maximum 1 saniye olmuştur. Bu saniyelerde `artık saniye (⟷ leap second)` uygulaması ile giderilmektedir. Artık saniye `GMT`'ye uygulanmaz. `UTC`'ye uygulanır. `GMT`'ye uyumlu olması için yapılan manuel değişikliktir. Bu değişiklik uluslararası birimler tarafından ortak karar alınarak yapılır.

## 📌 TAI vs UTC

`UTC`, saniye birimini `TAI`'deki ile aynı kullanıyor olsa da, `TAI` ile `UTC` arasında 2017 itibari ile aralarında 37 saniyelik bir fark var. `TAI` `leap second` kullanmıyor, bu sebeple 27 kere yapılan `UTC leap second` sebebi ile 27 saniyelik bir ara açıldı. `UTC` sistemi ilk ortaya atıldığında `TAI` ile arasında özel sebeplerden ötürü 10 saniyelik bir fark ile başlatılmıştı (otoriteler tarafından bu şekilde karar alınmıştı).

Dolayısı ile `27+10=37` saniye toplam fark mevcuttur. `kaynak: https://www.nist.gov/pml/time-and-frequency-division/time-realization/leap-seconds`

## 📌 UT1

`UT1`, `GMT`'nin yeni akademik ismidir. `kaynak: https://www.nist.gov/pml/time-and-frequency-division/nist-time-frequently-asked-questions-faq title:"What is Greenwich Mean Time (GMT)?". Diğer kaynak: https://lweb.cfa.harvard.edu/~jzhao/times.html title:"UT1 - Universal Time".`

`UT1` ve `UTC` arasındaki zaman farkı için `ΔUT (⟷ DUT1)` terimi kullanılıyor. `kaynak: <https://www.nist.gov/pml/time-and-frequency-division/time-realization/leap-secondss>`

`UT1` ve `UTC` arasındaki farkı `±0.9` saniye ile tutmaya çalışıyorlar. Bu dengeyi `leap second` ile yapıyorlar. `kaynak: https://www.nist.gov/pml/time-and-frequency-division/time-realization/leap-seconds`

## 📌 GMT vs GMST (⟷ Greenwich Mean Sidereal Time)

Birbirlerinden farklıdır. Diğer standartlarda bu kaynakta yazmaktadır. `Kaynak: https://lweb.cfa.harvard.edu/~jzhao/times.html#GMST`

## 📌 UTC with Smoothed Leap Seconds (⟷ UTC-SLS)

`UTC` ve `GMT`'ye alternatif'tir.

Programlama dünyasında kullanılması önerilen bir `time-scale`'dir. Çünkü `leap second`'ları ignore eder ve `GMT`'yi sürekli yakalamaktadır.

`GMT` gibi `leap second`'ları ignore eder fakat `UTC` gibi atomic saniyeyi kullanır. Fakat `GMT` ile dengeli kalabilmesi için şu yolu izler: `UTC`'de `leap second` olan bir günün son 1000 saniyesi `0.1%` yavaşlar veya hızlanır. Yani son 1000 saniye atomic saniye kullanmaz. `kaynak: https://www.cl.cam.ac.uk/~mgk25/time/utc-sls/`

`Java`'nın güncel sürümleri ile gelen `java.time` kütüphanesi "__Java Time-Scale__" kullanır. "__Java Time-Scale__" `UTC-SLS`'i baz almaktadır. `kaynak: <https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html> Bu kaynakta şu 2 cümleler yer almaktadır:`

```text
Java Time-Scale is identical to UTC-SLS
```

```text
The Java time-scale is used for all date-time classes. This includes Instant, LocalDate, LocalTime, OffsetDateTime, ZonedDateTime and Duration. 
```

## 📌 artık saniye (⟷ leap second)

Bazı kaynaklarda `negative leap second (⟷ negatif artık saniye ⟷ negatif saniye ⟷ negative second)` da denilmektedir.

çok hassas hesaplamalar sonucu uluslararası alınan karar ile bazı günlerin sonuna (örnek 31 Aralık gecesi) 1 saniye eklenmektedir yani `23:59`'dan `00:00`'a geçiş 2 saniye sürmektedir.

- İlk artık saniye 1972'de uygulanmıştır.
- Şu ana dek (yıl 2019) sadece `Haziran` ve `Aralık` (yılın sonu ve ortası) aylarında artık saniye uygulanmıştır.
- 2021 itibariyle 25'in üstünde artık saniye uygulanmıştır.
- Bazı yıllarda hem `Aralık` hem de `Haziran` ayı artık saniye uygulanmıştır.

`POSIX time`'da bu saniye ignore edilir (yani saniye ekleme yada çıkarma yapılmaz). `kaynak: https://pubs.opengroup.org/onlinepubs/9699919799/xrat/V4_xbd_chap04.html "A.4.16 Seconds Since the Epoch" başlığı ilk paragraf.`

Artık saniye `joda-time`, `java.time` (yıl 2022)'de desteklenmemektedir. Destekleyen kütüphane veya cihazlarda veya sistemlerde ekranda nasıl gösterileceği ile ilgili bir standart yoktur. Bu sebeple genelde bu gibi davranışlar gözlemleyebiliriz:

- Bazı sistemler `23:59`'u artık saniye kadar ekranda göstermektedir.
- Bazı sistemler `23:60` ve `23:61`'i göstermektedir.
- Bazı sistemler `24:00`'ı artık saniye kadar ekranda göstermektedir.
- `RFC 3339`'ye göre saniye'ler `0-60` arasında izin verilmektedir. 60'ıncı saniye eğer o dakikada `leap second` var ise kullanılabiliyor. `kaynak: https://datatracker.ietf.org/doc/html/rfc3339 title: "Appendix D. Leap Seconds".`

`Time4J` gibi bazı kütüphanelerle artık saniyeyi destekleyebiliriz.

Artık saniye sebebi ile daha önce birçok yazılımda sorun yaratmıştır (internette "leap second impact" diye aratıldığında haberler okunabilir). Artık saniye özellikle aşağıdaki sistemlerde sorunlara yol açmaktadır:

- bilimsel hesaplamalarda (hassas hesaplamalarda, uzaydaki uzak mesafelerdeki cihazların birbirlerinde haberleşmesinde)
- missile systems (füze sistemleri)
- GPS kullanan sistemlerde
- leap second kullanan farklı data kaynaklarından (üçüncü parti web servisler, DB'ler...) veri okuma ve yazma. Örneğin DB "23:60" syntax'ını destekliyor ise, veya Java destekliyor ise DB desteklemiyor ise bu durum sorunlara yol açabilir. Bu durum sadece o saniyede olan işlemlerde exception'a sebep olacaktır veya o saniyede yapılan işlemler okunmaya kalktığında exception meydana getirir. Belki DB kaydı işlemine tölerans gösterebiliriz ama örneğin log atma işlemi her saniye binlerce kere yapılan bir işlemdir. Dolayısı ile her leap second'da bu sorunu yaşayabiliriz.
- leap second kullanmayan sistemlerdeki son kullanıcıya görünen saatlerde saniyeler ufak sapmalarla yanlış gösterilebilir. Eğer sistem offline ise veya sync olmuyorsa, dünyada her leap second kararı alındığında 1'er saniye sapma olacak ve son kullanıcıya yanlış bilgi gösterilecektir. Yani; leap second sonrası, kol saatimiz sürekli olarak 1 saniye geriden veya ileriden gösteriyor olacaktır (eğer kol saatimiz sync olmuyor ve leap second'u ignore ediyor ise).
- replica sync işlemlerinde hata alabiliriz. örneğin; 2 bağımsız `in-memory DB` node var. birbirin sync ediyor olsun. biri diğerine saat başı validasyon için şu mesajı atıyor olsun: "saat 13:30:55 itibari ile bendeki data'nın hash'i 123456789'dur". Eğer 2 node'un 1 saniye farkı var ise bu validasyon hiçbir zaman onaylanmayacaktır.

Yukarıdan anlaşıldığı üzere bug'lar sadece leap second anında değil, leap second alındıktan sonra sürekli olarak yaşanma ihtimali de var.

## 📌 artık yıl (⟷ leap year)

`miladi takvim`de yaşanan bir durumdur.

dünya güneş etrafında `365.24` küsür günde dönüyor. `miladi takvim`e göre; her yıl 365 kabul edilmiştir. 4 yılda bir kere 29 Şubat kullanılmaktadır. 29 Şubat olan yıla `artık yıl` denir. `artık yıl`ın, 4 senede 1 kere olmasını belirleyen iki özel kural daha var:

- `kural-1`

  Eğer yıl, 100'ün katı ise; `artık yıl` değildir.

- `kural-2`

  Eğer yıl, 400'ün katı ise; `artık yıl`dır.

`Kural-2`, `kural-1`'i ezmektedir.

örnek:

- `artık yıllar`: 1600, 2000, 2400
- `artık yıl değil`: 1700, 1800, 1900, 2100

Bu kuralların sebebi 1 yılın `365.24` küsür olması. `0.25`, yani `1/4` değil.

## 📌 UTC mi GMT mi "POSIX time" mi kullanmak gerekir?

Örneğin, uzayda herhangi bir gezegende yada noktada, örneğin uydulardaki astronotların, `GMT` ile bilgi alışverişi yapması best practice olmayacaktır. Böyle bir durumda `UTC` kullanılmalıdır. Çünkü `UTC` atomik saniyeyi baz alır.

`GMT`'yi eledik. `POSIX time` ve `UTC` kullanmak ikisi de doğrudur. `UTC` kullanılabiliyorsa tercih edilmelidir. sebepleri:

- debug işlemlerini çok hızlandıracaktır çünkü direk okunduğunda değerin ne olduğu human-readable'dır.
- `POSIX time` saniye bazında zaman tutuyor. eğer saniyeden ufak bir bilgi (birim) tutmaya ihtiyacımız olursa sorun yaşayabiliriz. Ek not: `POSIX time` için bazı kütüphaneler saniyeden ufak birimleri desteklemek için noktadan sonrasında da karakter desteklemektedir (örnek: 123456789.123). Fakat bunun resmi bir standartı yoktur. Dolayısı ile kullanmamakta fayda var.

Not: Özellikle string yerine integer gibi değerlerle kullanılan formatlarda birçok farklı problem de yaşanabiliyor:

- Y2K bug: 2000 yılına girildiğinde, yıl değerinin sadece son iki hanesini tutan sistemler sorun yaşamıştır.
- Y2K38 bug: signed 32-bit integer ile tutulan `POSIX time` değerleri, "03:14:07 UTC on 19 January 2038" sonrasını gösteremeyecek, onun yerine sıfır değerine reset'lenecek yada hata fırlatacaktır.
- diğerleri: https://en.wikipedia.org/wiki/Time_formatting_and_storage_bugs

## 📌 Yaz saati uygulaması (⟷ Daylight saving time ⟷ DST ⟷ daylight savings time ⟷ daylight time ⟷ summer time)

Güneşten yararlanmak için uygulanan bir sistem olduğundan, genelde sonbaharda 1 saat ileri, yazın 1 saat geri alınır.

Dünyanın bazı ülkelerinde uygulanırken bazılarında uygulanmamaktadır.

Her ülke kendi yasalarınca saatleri kendi belirlediği için, her ülkenin saati meridyene göre hesaplanamamaktadır.

Bir ülke belli yıllar bu uygulamayı kullanırken belli yıllar arasında kullanmayabilir.

## 📌 timezone

dünyanın birçok noktasında timezone'lar belirlenmiştir. genelde her ülkenin bir timezone noktası vardır. bu noktanın etrafındaki belli bir bölge, aynı saati kullanır.

Coğrafi konuma (güneşten yararlanmak için) ve ticari sebeplerden birden fazla timezone noktası aynı ülkede olabilir. bu noktalar için saatin kaç olacağı da sadece meridyene göre belirlenmemektedir. ticari, coğrafi ve siyasi sebeplerden meridyenle alakasız saat farkları olabilir. aynı zamanda; yaz saati uygulaması gibi belli dönemler aynı noktadaki saatler farklılık gösterebilir.

Timezone yıllara göre ve bölgeye göre (ülkeye göre olmak zorunda değil) değişmektedir. Örneğin;

- moment-js-with-data kütüphanesi yıllara göre timezone'u buradan almaktadır: http://www.iana.org/time-zones

- JDK'da bulunan timezone'u güncellemek için açık kaynaklı bir updater var. Bu updater'da bilgileri IANA'dan almaktadır. Jdk sürümü eski kalsın, fakat time-zone bilgisine ihtiyaç duyan canlı'da olan projeler için geliştirmiş bir tool(updater) bu.

Timezone'lar text olarak gösterilmek istendiğinde; KıtaAdı/ŞehirAdı formatı kullanılmaktadır. bu resmi bir format değildir. fakat genelde böyle kullanılır. Örneğin; Europe/Istanbul, America/Chicago. Bazı yerler için sadece direk ülke yada şehir ismi kullanıldığı da olur. Bu gibi durumda o ülkede tek 1 adet timezone olmalıdır.

örneğin Türkiye için 3 farklı gösterim ile karşılaşılabilir:

- Europe/Istanbul
- Asia/Istanbul
- Turkey

Bu formatlardan en çok kabul göreni IANA'nın resmi sitesinde public yayınladığı __TZ DB (⟷ Olson time zone DB ⟷ tzinfo ⟷ zoneinfo)__'dır. IANA'dan Zip dosyası indirildiğinde içinde timezone isimleri, tüm artık saniyeler, her timezone için tüm saat dilimi değişiklikleri ve bu dosyaların nasıl parse edileceği, önemli kaynaklardan URL'ler gibi dökümanlarda gelmektedir. Örneğin; <https://www.iana.org/time-zones> sitesinden "tzdb-2021e.tar.lz" dosyasının içinde "europe" isimli dosya vardır. Bu dosyada "Europe/Istanbul" için olan tüm değişiklik kararları mevcuttur. Hangi kararın, hangi kaynaktan (gazeteden veya bakanlıktan gibi) olduğunu ve detayları da yazılmıştır. Dikkat edilirse saat değişiklikleri gün içerisinde de olabilmektedir. Örneğin; "öğlen saat 2'de 1 saat ileri alınacak gibi" kararlarda olabilmektedir.

## 📌 UTC offset

timezone; ülkenin yasalarına göre değişirken (ileri geri alınabilirken), UTC offset hiçbir zaman değişmez. UTC offset +3 ise, bu zaman dilimi sürekli UTC'nin 3 ilerisindedir. Yazılım sistemlerinde sürekli UTC offset tutmak güvenlidir. Sadece son kullanıcıya gösterilirken (GUI'de) timezone tercih edilirse, zaman problemleri ile daha az karşılaşırız.

## 📌 özel timezone isimleri

Özellikle sık kullanılan timezone'lara bu şekilde eş anlamlı özel isimler atanmıştır. örnek:

- `UTC+2`, `Eastern European Time (⟷ EET)` ile eş anlamlı kullanılır.

- `Eastern European Summer Time (⟷ EEST)` ise `UTC+3` ile eş anlamlıdır.

Politik sebeplerden `Central Africa Time (⟷ CAT)` ve `South African Standard Time (⟷ SAST)` `UTC+2` ile eş anlamlıdır.

Yani bir timezone'un birçok eş anlamlı ismi olabilir. genelde sık kullanılanlara bu tarz takma isimler atarlar. Bir ülke (bir bölge) belli bir zaman diliminde (örneğin kışın ve bahar mevsimleri) `EET` kullanır, fakat yaz mevsimine geçerken `EEST` kullanabilir. Yani; `EET` kendi içinde kış veya yaz olarak farklı saat dilimlerine geçiş yapmamaktadır.

## 📌 TRT (⟷ Türkiye Saati ⟷ TSİ ⟷ Turkey time)

Her ülke veya belli bölgeler için bu tarz terimler kullanılır.

`Türkiye` 24 saatlik sisteme ve miladi takvime 1 Ocak 1926'de girdi. Bu sebeple 1926'dan önceki sisteme `Turkish time (⟷ alla turca time ⟷ ezani time)` deniliyor. Eski sistem güneşin batımına ve doğumuna göre ayarlanıyordu.

`Türkiye` 27 Mart 2016 saat `03:00` itibaren saatini bir saat ileri aldı ve yaz saati uygulamasını iptal etme kararı aldı. Yani `+3`'te sabitlendi.

`Türkiye` ve diğer ülkelerdeki diğer tüm kararlar için başka başlıkta anlatılan `IANA` dosyası incelenmelidir.

## 📌 mevsim (⟷ season)

mevsimler programatik olarak bulunması gerektiğinde ay bazında değil gün bazında bulunduğu unutulmamalıdır. örneğin 23 Eylül öncesi kuzeyde yaz mevsimi iken, 23 Eylül sonrası sonbahardır. aynı tarihlerde güney yarım küredeki ülkeler için zıt mevsimlerdir. bu durumda son kullanıcının kendi IP'sinden lokasyon tespiti yapmak gerekebilir.

Her taksimde sezon kavramı farklı olabilir. Örneğin bazı takvimler 1 yılda 6 sezon olacak şekilde tasarlanmıştır.

## 📌 astronomical seasons vs meteorological seasons

İngiltere ve daha kuzeyinde, Irak ve daha güneyinde meteorological mevsim tarihleri kullanılmaktadır. yani mevsimler aynı fakat farklı başlangıç ve bitiş tarihleri vardır. meteorological mevsimler hava durumuna göre ayarlanmaktadır. oysa astronomical mevsimler dünyanın güneş etrafındaki dönüşüne göre ayarlanmaktadır.

## 📌 Gregorian calendar (⟷ miladi takvim ⟷ tr:Gregoryen takvimi)

milat: İsa'nın doğduğu gün'dür. miladi takvim; miladı başlangıç yılı ve 1 yılı 365 gün kabul eden takvimdir.

## 📌 takvim

tüm zaman kavramlarını ele alan zaman sistemidir: gün, saat, yılın kaç ay olduğu, ay isimleri ve hangi ayın kaç gün olduğu, yılın ne zaman bittiği (yılbaşı)...

Gregorian takvimi en çok kullanılan takvim. fakat bazı ülkelerde hala diğer takvimler %50'ye yakın oranla kullanılmaktadır (istatistik tarihi: 2017). Günümüzde hala kullanılan bazı takvimler:

- `Hicri takvim (⟷ İslami takvim ⟷ Hijri calendar)`

  Muhammed'in `Mekke`'den `Medine`'ye yolculuğunu başlangıç yılı (1. yıl) kabul eden ve 1 yılı 354 ya da 355 gün olan takvimdir. Orta Doğu ülkelerinde kullanılmaktadır.

- `Hebrew calendar (⟷ Jewish calendar ⟷ İbrani takvimi ⟷ Yahudi takvimi ⟷ Musevi takvimi)`

  `İsrail`'deki sistem

- `Etiyopya takvimi (⟷ Ethiopian calendar)`

  `Etiyopya` ülkesinde kullanılan takvim

- `Buddhist calendar`

  `Thailand`, `Sri Lanka`, `Malaysia`, `Singapure` (ve onlara yakın bazı ülkeler)

- `Indian national calendar (⟷ Shalivahana Shaka calendar ⟷ Saka calendar)`

  `Hindistan`'daki sistem.

  `Hindu calendar (⟷ hindu takvimi ⟷ Hint takvimi)` terimi bunun yerine kullanılması doğru değil. çünkü `hint` kültüründe birçok takvim vardır.

- `Jülyen takvimi (⟷ Julian calendar)`

  batı tarafından `Miladi takvim`'in kabulüne kadar (1582 yılı) kullanılan takvimdir.

Yukarıdaki takvim sistemleri de kendi içlerinde yıllar içinde değişime uğramıştır.

`java.time.crono` paketi altında:

- `HijrahDate` (`Hijri calendar`)
- `JapaneseDate`
- `LocalDate` (`Gregorian calendar`)
- `ThaiBuddhistDate` (yukarıdaki `Buddhist calendar`)

sınıflarını görebiliriz. Bu sınıflar hakkında bazı bilgiler:

- Bu sınıfların hiçbiri 'java.util.Calendar'dan türememişlerdir. Çünkü tamamen yeni bir kütüphane, yeni bir paket altında sunulmaktadır.
- Bu sınıfların tümü LocalDate ile aynı pakette değildir. Çünkü non-ISO calendar'lar, ISO calendar'lardan farklı bir pakete ayrılmış durumda. Bu sebeple sadece LocalDate, diğer tüm calendar'lardan farklı bir pakettedir.
- "java.time" paketi yayımlandığında, temelde sadece ISO'yu temel alarak geliştirilmiş. Kaynak: <https://docs.oracle.com/javase/8/docs/api/java/time/package-summary.html> 3th sentence. Bu sebeple; "GregorianDate" olarak görmeyi beklediğimiz sınıfın ismini LocalDate olarak görmekteyiz.
- Java kodlarına detaylı baktığımızda, bu sınıfların tümünün ortak sınıflardan implemente ettiğini görebiliriz. Fakat bu tüm süper sınıflar, tek 1 interface değil. Dolayısı ile, common bir interface API'si ile tüm calendar'ları kullanamayız.

## 📌 özel günler

`milli bayramları (⟷ national holiday)`, dini bayramları ve özel günleri programatik olarak her ülke için elde etmek zordur. Burada dikkat edilmesi gereken hususlar:

- `Administrative division` başlığına benzer bir durum söz konusudur.
- her ülkenin resmi tatilleri yaşanan olaylar sebebi ile değişebilmektedir. Türkiye'de 15 Temmuz'un tatil oluşu gibi.
- mezhepler de kendi içlerinde farklı mezhepler türettiklerinden her bölgede aynı bayramlar aynı tarihte kutlanmamaktadır. Tüm hristiyanların Noel'i dahi, aynı coğrafi yörede dahi, aynı gün kutlamaması gibi.
- Paskalya bayramının tarihi her yıl değişmektedir. Anneler günü, babalar günü gibi özel günlerin bazıları ise hem ülkeler arasında değişmektedir. Hatta aynı ülke içerisinde tarihleri her yıl değişebilmektedir.
- her takvimde, `yılbaşı (⟷ new year's day)` farklı zamana denk gelebilir.

## 📌 Bank holiday

bu terim sadece İngiltere ve birkaç ülkede kullanılan bir terimdir. her ülkede ise farklı anlama gelmektedir. örneğin; İngiltere'de resmi tatilleri kapsarken, İrlanda'da sadece banka'ların tatil olduğu günleri temsil eder.

## 📌 time vs date vs datetime

- time: saat/dakika/saniye gibi bilgileri içeriyor.
- date: günün yıl içerisindeki yerini tutuyor
- datetime: hem date hem de time'ı içerir.

## 📌 java.sql.Date

java.util.Date'ten extend etmiştir. SQL.Date time bilgisini içermez sadece date içerir. "time" bilgisi, java.util.Date'te olduğundan extend etmiş sınıfta da vardır. buna çözüm olarak java.sql.Date bu değerleri sıfır olarak set edilir. Benzer durum java.sql.Time 'da da vardır: date bilgisi 0'dır.

## 📌 java.sql.Time

java.util.Date'ten extend etmiştir. date bilgisi içermez fakat time içerir.

## 📌 java.sql.Timestamp

java.util.Date'ten extend etmiştir. java.util.date'e ekstradan nanosecond da içerir. java.util.date en ufak birimi millisecond'dır.

## 📌 java.util.Calendar

java.util.Date'e alternatif Java 7 ile gelen takvim sınıfıdır. java.util.Calendar abstract'tır, bu sebeple direk kullanılamaz. GregorianCalendar ise onu extends etmiştir.

## 📌 java.time.*

java.util.Calendar'e alternatif Java 8 ile gelen takvim paketidir. java.util.date ve java.util.calendar geriye uyumluluk için hala Java içerisinden kaldırılmamıştır.

## 📌 Joda Time

Java için date ve calendar'ın eksiklikleri sebebi ile geliştirilen kütüphane. Java 8 ile gelen "java.time" paketi, Joda'nın fork'udur. Java 8 çıktığında Joda projesi artık resmi olarak gereksiz olmuş ve geliştirilmeme kararı alınmıştır.

## 📌 minimum time unit

| name               | support min time unit |
|--------------------|-----------------------|
| epoch time         | second                |
| Joda               | millisecond           |
| java.time          | nanosecond            |
| java.util.Date     | millisecond           |
| java.util.Calendar | millisecond           |
| java.sql.Time      | millisecond           |
| java.sql.TimeStamp | nanoseconds           |

## 📌 java.time.* (details)

__LocalDate__ sadece gün ay yıl tutuyor.

```java
// init metotları
LocalDate localDate = LocalDate.now();
LocalDate.of(2015, 02, 20);
LocalDate.parse("2015-02-20");

// diğer metotlar
LocalDate tomorrow = LocalDate.now().plusDays(1);

// ChronoUnit, java.time.* içindeki özel bir obje
LocalDate previousMonthSameDay = LocalDate.now().minus(1, ChronoUnit.MONTHS);

// Period, java.time.* içindeki özel bir obje.
// Period date birimleri ile ilgilenirken, Duration sınıfı time birimleri ile ilgilenir.
LocalDate finalDate = initialDate.plus(Period.ofDays(5));


DayOfWeek sunday = LocalDate.parse("2016-06-12").getDayOfWeek();

int twelve = LocalDate.parse("2016-06-12").getDayOfMonth();

boolean notBefore = LocalDate.parse("2016-06-12")
  .isBefore(LocalDate.parse("2016-06-11"));
```

__LocalTime__ sadece saat, dakika, saniye ve ek olarak nano seviyesinde bilgi tutuyor.

```java
// init metotları
LocalTime now = LocalTime.now();
LocalTime sixThirty = LocalTime.parse("06:30");

// diğer metotlar
LocalTime sevenThirty = LocalTime.parse("06:30").plus(1, ChronoUnit.HOURS);

LocalTime localtime = LocalTime.parse("00:01:02.5");
//prints: "seconds: 2 nano seconds: 500000000"
System.out.println("seconds: " + localtime.getSecond() + " nano seconds: " + localtime.getNano() );
```

__LocalDateTime__ date ve time'ı birlikte tutar. LocalTime ve LocalDate ile aynı isimde metotlar sunar.

__ZonedDateTime__; LocalDateTime'e ek olarak time zone bilgisini de tutar. zone bilgisi bölgesel bir bilgidir. Yani; kanunlar gereği bölgedeki saat dilimi değiştirildiğinde, ZonedDateTime değeri de otomatik değişecektir.

__OffsetDateTime__ ZonedDateTime ile aynıdır. tek farkı zone bilgisi değil, sadece offset bilgisi tutar. bu sebeple;kanunlar gereği bir coğrafi bölgedeki saat dilimi değiştirildiğinde OffsetDateTime sınıfının değeri etkilenmez. çünkü OffsetDateTime sadece "+03:00" gibi offset değeri tutar. oysa ZonedDateTime "Asia/Turkey" gibi bir değer tutar.

```java
ZoneId zoneId = ZoneId.of("Europe/Paris");
Set<String> allZoneIds = ZoneId.getAvailableZoneIds();

ZonedDateTime zz = ZonedDateTime.parse("2015-05-03T10:15:30.5+01:00[Europe/Paris]");
ZonedDateTime zonedDateTime = ZonedDateTime.of(localDateTime, zoneId); // LocalDateTime objesine timezone ekler.
```

__ZonedDateTime__ parse ve toString metotlarında ISO 8601 formatını kullanır. tek istisna opsiyonel olarak timezone'un isim olarak \[Europe/Paris] şekilde yazılabilmesidir.

__ZonedDateTime__ objesi __withFixedOffsetZone__ metodunu sunar. Bu metot yine __ZonedDateTime__ döner fakat __ZonedDateTime__'ın __OffsetDateTime__ gibi davranmasını sağlar. Yani artık yeni __ZonedDateTime__ instance'ı, kendi içinde __java.time.ZoneId__  yerine __java.time.ZoneOffset__ kullanır.

## 📌 javax.xml.datatype.XMLGregorianCalendar

JRE içinde bulunan bir interface. implementasyonu da burada:

> com.sun.org.apache.xerces.internal.jaxp.datatype.XMLGregorianCalendarImpl

W3C'ün XML şemasında ("W3C XML Schema 1.0") birçok farklı tip vardır (base64Binary, boolean..). bu tiplerin arasında zaman ile ilgili data tipleri de vardır. örnekler: gYearMonth, gYear, dateTime, time, date...

XML üzerinde örneklersek:

```xml
<myxlmroot>
 <TestgYearMonth>1999-02</TestgYearMonth>
 <testAnyElement>1</testAnyElement>
 <TestdateTime>2002-10-10T12:00:00-05:00</TestdateTime>
</myxlmroot>
```

Yukarıda 1999-02 String'ini hangi formata olacağı W3C tarafından belirlenmiştir. Eğer XSD dosyası bu XML elementinin bir W3C XML şeması olduğunu gösteriyorsa, o zaman Java'daki XML to object parser'lar bunu Java'daki XMLGregorianCalendarImpl'a çeviriyor. Yukarıdaki XML şu Java sınıfına döndürülüyor:

```java
public class Myxlmroot {

  private XMLGregorianCalendar testgYearMonth;

  private int testAnyElement;

  private XMLGregorianCalendar testdateTime;

  // getter and setter

}
```

XMLGregorianCalendarImpl gYear, gYearMonth gibi tüm değerleri tek bir sınıftan okuyabilmemizi sağlıyor. getXMLSchemaType() metodu bize XMLGregorianCalendar'in hangi (çeşit) şemadan (gYearMonth, time) XML'e çevrildiğini döndürüyor.

XMLGregorianCalendar kendi içinde date time işlemleri yapmıyor. zaten gün ekleme çıkarma gibi işlem yaptıran public metotlar sunmuyor. sadece set/get, birçok constructor ve birkaç utility metodu mevcut.

XMLGregorianCalendar kendi içinde GregorianCalendar'ı kullanıyor (fakat ondan extend etmemiş).

## 📌 ISO 8601

date ve time representation format standardıdır. formatta bazı esneklikler vardır. bazı değerler (yıl, ay, saniye...) gösterilebilir yada gösterilmeyebilir.

- sürümler

  Farklı sürümleri vardır: (sırası ile)

  - ISO 8601:1988
  - ISO 8601:1988/COR 1:1991 Technical Corrigendum 1
  - ISO 8601:2000
  - ISO 8601:2004
  - ISO 8601-1:2019 - Part 1: Basic rules
  - ISO 8601-2:2019 - Part 2: Extensions

  Örneğin; 00:00 yerine 24:00 da kullanılabiliyordu. Fakat bu, SO 8601:2004 sonrasında kaldırıldı.

- Gregorian calendar ve UTC kullanılır.

- TZD, __time zone designator__'ın kısaltmasıdır ve formatta +hh:mm seklinde gösterilir. sadece Z kullanılırsa, +00:00 anlamına gelir.

  Ek not: Z harfi, "__Zulu time__" teriminden gelmiştir. 'Zulu time', askeri standartlarda (ACP 121(I)) +0 timezone'a denk gelen özel isimdir. ACP 121(I) standardında, saat bazlı her timezone için birer özel isim vardır. örneğin: Alpha Time Zone: +1, Bravo Time Zone +2, Charlie Time Zone: +3.

- T işareti ise date'in bittiği ve time'a geçilen kısma atılır.

- DD ve diğerleri her zaman çift hane olmak zorunda. 01 yerine 1 yazılamaz.

- AM/PM ayrımı yoktur. 24 saat özelliği kullanılır.

- örnekler;

  - YYYY-MM --> 1997-07

  - YYYY-MM-DDThh:mm:ssTZD --> 1997-07-16T19:20:30+01:00

  - YYYY-MM-DDThh:mm:ss.sTZD --> 1997-07-16T19:20:30.45+01:00

- mm ve MM ayırt edilebilmesi için küçük büyük olup olmadıkları önemlidir.

- eğer 's' karakteri var ise; en az 1 karakter olmak zorundadır. maximum kaç karakter olacağı standartlarda belirtilmemiştir. 's' millisecond değildir. bu kısım yazılımcının kendisine kalmıştır.

- ISO 8601'de haftalar Pazartesi ile başlar. senenin başında hafta ancak Pazartesi ise başlar. yani 1inci hafta ilk Pazartesi olan haftadır. örnek 1 Ocak Çarşamba günü ise; 1 Ocak hala eski yılın son haftasıdır.

  haftalar da output olarak basılabilir. örnek: 2018-W08 Format: YYYY-Www

## 📌 RFC 3339

ISO 8601'a alternatif bir standarttır. Aralarında çok fark var. Fakat bazı gösterimler ortak olarak her iki standart için geçerli olabilir.

Farklar burada detaylandırılmıştır: https://github.com/IJMacD/rfc3339-iso8601. Bu repository'nin DEMO sayfası buradadır: https://ijmacd.github.io/rfc3339-iso8601/

## 📌 java.text.SimpleDateFormat

Java 1.1 ile gelen eski bir sınıftır.

Java içinde gelen SimpleDateFormat'ın kendine özgü formatı vardır (global anlamda bir standart değildir).

- YYYY-MM-DDThh:mm:ss kabul etmez. T harfini ' tırnak içinde ister: YYYY-MM-DD'T'hh:mm:ss

- YYYY'yi büyük harf olduğundan kabul tanımamaktadır. küçük harf olması şarttır.

- MMMMM ayın kelime karşılığını yazıyor: Mart, July gibi.

- a, AM PM 'i ekrana basıyor.

- küçük s second, büyük S millisecond anlamına geliyor. ss hep iki karakter olmak zorunda iken, SSS 3 karakter olmak zorundadır.

- yy yılın son 2 karakterini ekrana basacaktır. örneğin yıl 2012 ise; 12 basacaktır.

- yyyyy; yıl 2012 ise ekrana 02012 basacaktır.

## 📌 java.time.format.DateTimeFormatter

Java 1.8 ile gelmiştir. String formatlama (parse ve tersi) yapmakla yükümlü.

Kendi içerisinde ekstradan final static DateTimeFormatter değerler barındırır. Bunlar sıkça kullanılan DateTimeFormatter'lardır. örnek:

DateTimeFormatter.ISO_LOCAL_DATE bu şekilde string'leri formatlamak için kullanılır: '2011-12-03'.

Diğer sabit değerlere buradan bakılabilir: <https://docs.oracle.com/javase/8/docs/API/java/time/format/DateTimeFormatter.html> title: "Predefined Formatters".

Tüm predefined sabi değerler ISO 8601 ile uyumludur. Tek istisna RFC_1123_DATE_TIME instance'ıdır. RFC_1123_DATE_TIME formatı RFC 1123 ve RFC 822'de geçen özel bir formattır.

## 📌 salise

Saniyenin 1/60'ıdır. bu terimin İngilizce karşılığı yok.

## 📌 AM ve PM

Kısaltmalar Latince kelimelerden geliyor.

AM/PM sistemi "__12-hour clock__" sistemidir. AM/PM sistemi, "__24-hour clock__" sistemine (formatına) alternatif olarak kullanılır.

AM = A.M = Ante meridiem = Before noon = before midday = öğleden önce = ö.ö. = gece yarısı(00:00) ve öğlene kadar(12:00) olan süre

PM = P.M = Post meridiem = After noon = after midday = öğleden sonra = ö.s. = öğleden gece yarısına kadar olan süre

## 📌 Gün içerisindeki zaman aralıkları

aşağıdaki terimlerin resmi zaman veya zaman aralıkları yoktur. Bazı kütüphaneler kendi içlerinde bunları enum gibi sabitlere bağlasa da, sözlük anlamlarına bakıldığında resmi bir zaman veya zaman aralığını temsil etmezler.

- `Öğle (⟷ öğlen ⟷ midday ⟷ noon)`
- `afternoon (⟷ ikindi)`
- `akşam (⟷ evening)`
- `gece (⟷ night)`
- `gece yarısı (⟷ midnight)`

## 📌 0 yılı öncesi ve sonrası

CE = MS = M.S. = Milattan Sonra = Common Era = Current Era = Christian Era = İsa'dan Sonra = İS = İ.S. = Anno Domini (efendimizin yılı anlamına geliyor) = AD

BCE = MÖ = M.Ö. = Milattan Önce = Before Common Era = İsa'dan Önce = İÖ = İ.Ö. = BC = before Christ

## 📌 End of day and start of day

Bir günün başlangıcı için saat, dakika, saniye ve saniyeden küçük tüm birimleri sıfıra set etmemiz yeterlidir.

Gün sonu için yine benzer şekilde saat, dakika, saniye ve daha küçük değerleri en son limite çekmemiz gerekmektedir. Örneğin "java.time.LocalTime" sınıfında şu satırı görebiliriz:

```java
MAX = new LocalTime(23, 59, 59, 999_999_999);
```
