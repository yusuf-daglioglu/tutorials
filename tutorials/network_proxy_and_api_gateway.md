# NETWORK PROXY AND API GATEWAY

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 L4 vs L7 proxy

Direk `Layer7` isminde api gateway'lar var. karışıklığa sebep olabiliyor.

`L4` `proxy`'ler `TCP/IP` katmanının 4üncü seviyesinde (`Transport`: `TCP`, `UDP`) manipülasyon yapabilir.

`L7` `proxy`'ler `TCP/IP` katmanının 7inci seviyesinde (`application layer`: en üst seviye) manipülasyon yapabilirler.

Bazı durumlarda; bir `proxy` türü diğer `proxy`'nin yaptığı işi de yapabilir. Ama biz genelde en sık nereye odaklandıklarına göre gruplandırırız.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Direct server return (⟷ DSR)

bu tarz proxy'ler dönen response'ları kendi üzerlerinden geçirmezler. response'lar direk client'a node tarafından yollanır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 downstream proxy vs upstream proxy

upstream kelime anlamı: akıntı yönüne karşı.

downstream kelime anlamı: akıntı yönüne doğru.

proxy dünyasında; downstream request'i yollayan, upstream ise cevabı yollayan node'dur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 API gateway (⟷ api-gateway)

piyasada şu terimlerle de kullanılır fakat bunlar pek net/doğru değildir:

- api-server
- gateway
- service gateway
- api manager
- veya tek bir işe özellikle odaklanmışsa direk onun ismini verenler oluyor. örnekler:
  - sadece "rate limiter"
  - sadece "cache limiter"
  - sadece "api compositor"

Yukarıdaki terimler "server" suffix'i ile de kullanılıyor.

"__edge server__" terimi farklıdır başka başlıkta anlatılıyor.

"API gateway" bir sunucu yazılımdır. özellikleri:

- __Sticky session__

  cookie'lere bakarak, kullanıcını aynı session'ı boyunca aynı node'a gitmesini sağlar.

- __Yük dengeleme (Load Balancing)__

  Örnekler:
  
  - __Round robin__ algoritması ile:

    Örneğin 3 sunucun varsa (A, B, C):

    1.istek → A

    2.istek → B

    3.istek → C

    4.istek → A

    5.istek → B

    diye devam eder.

  - __Random__

    Tamamen rastgele istek atar.

  - __Least Connections__

    En az aktif bağlantıya sahip sunucuya yönlendirir.

  - __Weighted Round Robin__

    `CPU` gibi kaynakları güçlü sunucuya daha fazla istek atar.

  - __IP Hashing__

    Client'ın IP'sine göre hep aynı sunucu seçilir.

  - __Response Time (Latency-based)__

    En hızlı yanıt veren sunucuya yönlendirir.

- Farklı protocol'ler arası dönüşüm.

  örnek: `SOAP->REST`, `TCP->SOAP` (`adapter` pattern)

- __DTO mapping__

  `DTO` içindeki field'lar geriye uyumluluk için mapping yapılabilir (`adapter` pattern).

- __authentication ve authorization__

  örnek `JWT` token  burada açılır, diğer servislere header'da açık yollanır.

  bunun avantajları:
  - basitlik
  - security kodlarında güncelleme olursa, diğer servislerde güncellemeye ihtiyaç olmaz.
  - daha hızlı (her servis token'ı açmakla zaman kaybetmez)

  dezavantajları:
  - internal network'te token açık gider.
  - yanlışlıkla header'da bir servis manipülasyon yaparsa, hata alınır.

- __log'lama__

  izleme, bildirim, istatistik toplama (kim ne kadar istek yapmış)

- __API versiyonlama__

- __API grubu devreye alım/kapatma/açma__

- __api composition pattern__

  "service composition" da denir. fakat çok genel bir ifade olduğu için tercih edilmemeli.

  birden fazla iç servisi, tek URL path'i üzerinden, 1 servis gibi dışarıya açabilme.

- __caching__

- __Canary Release Desteği__

  Trafiğin %5’ini yeni servise yönlendirme gibi işlemler.

- __Circuit Breaker__

  kendi içinde Circuit Breaker içerir. bunu normalde her servis yapabilir ama hepsinin önünde olması ekstra bir avantaj sağlar.

- __ek güvenlik__ önlemleri
  - CORS policies burada tanımlanabilir
  - SQL injection, XSS gibi saldırılara karşı ilk savunma hattı olabilir.

- __SSL burada açılır__

  diğer servisler SSL ile uğraşmaz.

- __darboğaz (throttling) yönetimi & rate limiter__

  her user'dan gelen istekleri sınırlandırmak. yani; belli zaman dilimlerinde, bazı isteklere limit getirebilmek. böylece sistemin yavaşlaması engellenmiş olur.

- __Quota (Kota) Yönetimi__

  rate limiter ile aynı mantıkta çalışır. ama amaç farklıdır. buradaki limitler (loyalty) kapsamında değerlendirilir ve son kullanıcının bildiği limitlerdir. örneğin X kampanyası açısından kullanıcının maximum istek hakkı olması gibi. rate-limiter ise güvenlik önlemi amaçlıdır.

- __Static Response__

  response directly to client. examples:

  - `health check` `endpoints`
  - warning API or HTML response messages for maintenance.

- Ek özellikler:
  - Admin `Web UI` sunabilirler (config güncelleme, status görüntüleme gibi)
  - gRPC, WebSocket, HTTP, TCP, UDP gibi protocol'leri destekleyebilir
  - eklenti altyapıları olabilir (Örnek: HTTP interceptor yazmamızı sağlar)

## 📌 Piyasadaki bazı API gateway'ler

- `Kong`

  `Kong`'da eklentiler `Lua` ile yazılmaktadır. eklenti altyapısı oldukça basit. `Servlet` filter'ı yazar gibi request ve response'u alıp manipüle edip bir sonraki eklentiye yollayabiliyoruz.

- `Envoy`

  `Service mesh` için geliştirtirildi. Bu sebeple odaklandığı konular:

  - native `GRPC` desteği
  - üstünden geçen istekler hakkında çok detay sunabilir (monitoring için)
  - restart istemeden config değiştirilebilir.
  - ufak boyutlu ve hızlı

  bu sebeple `sidecar` pattern'de sürekli tercih ediliyor.

- `Tyk API Gateway`

- `Layer7 API Gateway`

  `CA Technologies` (grup firması: `Broadcom`) firmasının ürününün ismidir.

  `Nginx` ve `Envoy` gibi kendi bağımsız bir process olarak ağaya kalkıyor, sadece config dosyaları ile yönetilebiliyor.

- `Nginx`

  `HTTP proxy` ve `API gateway`'dır.

- `Apache HTTP Proxy`

  `HTTP proxy`'dir.

  `API gateway` değildir! Fakat bazı özellikleri `Api gateway`'ların sunduğu bazı özellikleri sunar.

  `Nginx` neredeyse her konuda `Apache HTTP Proxy`'den daha başarılı.

- `HAProxy`

  `Load balancer`'dır.

  `API gateway` değildir. Fakat bazı özellikleri `Api gateway`'ların sunduğu bazı özellikleri sunar.

- aşağıdaki liste `JAR` olarak projeye ekleniyor, `api gateway` özellikleri programatik olarak yönetilebiliyor:
  - `Zuul` (`Netflix` tarafından geliştiriliyor. deprecated oldu)
  - `Zuul 2` (`Netflix` kapalı kaynak olarak kendi şirket içinde kullanıyor)
  - `Spring Cloud Gateway`

Not: `Zuul`'ün `edge-server` olduğu birçok yerde yazıyor. Fakat network mimarimizde, uç noktada olmasından kaynaklı bu şekilde bir algı var. Fakat bu tam olarak doğru değil veya bir tartışma konusu.

## 📌 Gateway

çok genel bi terimdir. birçok şeyi kapsar:

- Api Gateway
- edge server
- HTTP proxy
- evde kullandığımız modem/router
- şirketteki network firewall cihazı
- ve daha birçoğu...

## api gateway vs proxy

proxy temel olarak URL (Path, port) gibi bilgileri baz alarak işlem yapar. genelde yönlendirme işlemi yapar. Oysa `api gateway`, data'nın kendisini de okuyabilir.

Bu ayrım keskin çizgilerle belirli değildir.

## 📌 edge server (⟷ edge-server)

çoğu yeteneği aynı olabilir. aralarında amaç farkı vardır:

`edge server` son kullanıcıya en yakın fiziksel noktaya konumlandırılır. böylece diğer business-servislerimize istek gelmeden bazı işleri bizim yerimize halleder. Bu sebeple `edge server` terimi sıkça CDN'lerde kullanılır.

`Edge server`'lardaki temel amaç, asıl sunucularımıza yük gelmesini azaltmaktır. Bu sebeple bu konu `Edge computing` ve `distributed computing model` konu başlıkları altında yer alır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 VPN (⟷ Virtual Private Network) vs Proxy server (⟷ Vekil sunucu ⟷ yetkili sunucu)

Temelde aşırı benzer teknik mimarilerdir. Fakat amaçları farklıdır. VPN client'ı başka bir network'teymiş gibi davrandırtırken, proxy sadece isteklere aracılık etmek için vardır.

VPN ve Proxy yazılımları, OSI modelde en üst seviyede katmanında oldukları için bir çok opsiyonel ek özellik sunabilirler. Örneğin;

- yasaklı internet sitelerine erişmek veya tersi (engellemek)
- ek şifreleme yapabilirler (böylece HTTPS olmayan bağlantıları ISP dahi göremez),
- güvenlik için paketleri inceleyebilir,
- log sistemi olabilir,
- GUI sunabilir,
- isteklere aracılık yaparak internet hızını arttırmak,
- tüm client'lar için statik data'lara cache koyabilir...

Hem VPN hem de Proxy için:

- hem client hem de server tarafta yazılımın olması şarttır.
- OS seviyesinde veya admin yetkisi ile çalışılmak zorunda değildir. Sadece user seviyesi yeterlidir.
- Aynı OS'ta, sadece istediğimiz istediğimiz (bazı) uygulamalarda VPN ve/veya Proxy kullanabiliriz. Fakat;
  
  - özellikle şirketler, çalışanlarının makinelerine admin yetkisi ile VPN ve/veya proxy kurar. Bu şekilde o VPN uygulamasının tüm OS uygulamalarında geçerli olmasını sağlarlar.

  - VPN vey Proxy uygulamaları admin olarak çalışmazlarsa, runtime'daki tüm uygulamaların connection'larını yönetemezler. OS izin vermez. Dolayısı ile her programın içine girip, VPN veya Proxy ayarlarını manuel set etmek gerekir ki bu çok uzun ve genelde elle yapılabilen bir işlemdir. Bu sebeple çoğu VPN veya Proxy uygulaması admin olarak çalışacak şekilde tasarlanır. VPN admin olarak çalıştırıldığında, kendini OS'a NIC olarak ekler. Bu NIC'i, ipconfig komutu ile basitçe görebiliriz. (Proxy'ler NIC'e mi ekliyor kendini bilmiyorum).

- Aynı OS'ta, aynı yazılımda farklı `TCP` bağlantılarında farklı VPN'ler ve/veya Proxy'ler kullanabiliriz veya kullanmayabiliriz. Yani VPN ve Proxy connection bazlıdır.

## 📌 OpenVPN

Açık kaynaklı VPN uygulaması ve protocol'üdür. Aynı zamanda bu yazılımı geliştiren firma, OpenVPN protocol'ü ve yazılımları ile bulut VPN hizmeti de sunmaktadır.

## 📌 SSL

Proxy ve/veya VPN SSL'i isterse yönetebilir. Fakat web tarayıcısı sertifika uyarısı verecektir. Eğer SSL'i yönetirse ve sertifika uyarısını ignore edersek, VPN ve/veya Proxy data'ları açık şekilde görebilir. Fakat eğer Web tarayıcısında JS'te değil isek, yani; native OS uygulaması kullanıyor isek, SSL yokmuş gibi connection açmadan önce (bağımsız) ek bir şifreleme yapabiliriz. Bu şekilde SSL açılsa bile aracı olan VPN ve/veya Proxy, data'ları anlamlandıramayacaktır. (Aslında bu işlemi Browser'da da yapabiliriz. Fakat network admin JS'teki key'i çok rahat elde edebileceği için, bir anlamı kalmayacaktır).

Hiçbir proxy türü, client'a sertifika uyarısı verdirtmeden aradaki data'ları göremez/açamaz. Bunu şuradan çıkarabiliriz: <https://docs.mitmproxy.org/stable/concepts-howmitmproxyworks/> Bu sayfada `Explicit HTTPS` başlığında `HTTPS` isteklerinin standart bir Proxy üzerinden nasıl iletildiğini anlatıyor. Hemen bir alt başlığın olan `The MITM in mitmproxy` de ise, `mitmproxy`'nin sertifika kullanarak işlemi kendi şifrelediğini anlayabiliyoruz.

### 📌📌 HTTPS proxy vs HTTP proxy

Bu tanımların RFC gibi akademik kaynaklarda karşılığını bulamadım.

HTTP protocol'ünü destekleyen proxy'lerde şu özellikler olabilir:

- protocol desteklemesi:
  - sadece HTTP destekler, HTTPS desteklemez.
  - hem HTTP, hem de HTTPS destekler.
- client ve proxy arasındaki bağlantıyı ek SSL protocol'ü ile bir şifreleme yapar veya yapmaz. Böylece client ve proxy arasında olan;
  - HTTP istekleri (SSL olmayanlar)
  - HTTPS fakat bağlantı kurulurkenki handshake header'larını
  - Eski sürüm SSL kullanan bağlantıları

    kötü niyetli aracılardan ve ISP'den gizlemiş olur.

Bazı makaleler, yukarıdaki iki özellikten sadece birini destekleyen sunuculara HTTPS proxy diyor. Bazı makaleler ise her iki özelliği birden destekleyen sunuculara HTTPS proxy deniliyor. Dolayısı ile okuduğumuz makalede bu durumunu netleştirmek zor.
