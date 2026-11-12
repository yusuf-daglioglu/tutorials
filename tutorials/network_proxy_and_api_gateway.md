# NETWORK PROXY AND API GATEWAY

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 L4 vs L7 proxy

Direk `Layer7` isminde `api gateway` var. karışıklığa sebep olabiliyor.

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

piyasada şu terimlerle de kullanılır fakat bunlar pek net değildir:

- `api-server`
- `gateway`
- `service gateway`
- `api manager`
- veya tek bir işe özellikle odaklanmışsa direk onun ismini verenler oluyor. örnekler:
  - sadece `rate limiter`
  - sadece `cache limiter`
  - sadece `api compositor`

Yukarıdaki terimler `server` suffix'i ile de kullanılıyor.

`edge server` terimi farklıdır başka başlıkta anlatılıyor.

## 📌 API gateway capability'leri

- `Sticky session`

  cookie'lere bakarak, kullanıcını aynı session'ı boyunca aynı node'a gitmesini sağlar.

- `Service Discovery`

  3üncü parti yazılımlardan (örnekler: `K8S` `Service`, `K8S` `DNS`, `Consul`, `Netflix` `Eureka`, `etcd`) bir servisin tüm instance'larını otomatik bulur.

- `Yük dengeleme (Load Balancing)`

  Örnekler:
  
  - `Round robin` algoritması ile:

    Örneğin 3 sunucun varsa (A, B, C):

    1.istek → A

    2.istek → B

    3.istek → C

    4.istek → A

    5.istek → B

    diye devam eder.

  - `Random`

    Tamamen rastgele istek atar.

  - `Least Connections`

    En az aktif bağlantıya sahip sunucuya yönlendirir.

  - `Weighted Round Robin`

    `CPU` gibi kaynakları güçlü sunucuya daha fazla istek atar.

  - `IP Hashing`

    Client'ın `IP`'sine göre hep aynı sunucu seçilir.

  - `Response Time (Latency-based)`

    En hızlı yanıt veren sunucuya yönlendirir.

- Farklı protocol'ler arası dönüşüm.

  örnek: `SOAP->REST` (`adapter` pattern)

- `DTO mapping`
  
  (`adapter` pattern).

- `Validation`

  payload veya header validation.

- `authentication ve authorization`

  örnek `JWT` token  burada açılır, diğer servislere header'da açık yollanır.

  bunun avantajları:
  - basitlik
  - security kodlarında güncelleme olursa, diğer servislerde güncellemeye ihtiyaç olmaz.
  - daha hızlı (her servis token'ı açmakla zaman kaybetmez)

  dezavantajları:
  - internal network'te token açık gider.
  - yanlışlıkla header'da bir servis manipülasyon yaparsa, hata alınır.

- `log'lama`

  izleme, bildirim, istatistik toplama (kim ne kadar istek yapmış)

- `API versiyonlama`

- `API grubu devreye alım/kapatma/açma`

- `api composition pattern`

  "service composition" da denir. fakat çok genel bir ifade olduğu için tercih edilmemeli.

  birden fazla iç servisi, tek URL path'i üzerinden, 1 servis gibi dışarıya açabilme.

- `caching`

- `Canary Release Desteği`

  Trafiğin %5’ini yeni servise yönlendirme gibi işlemler.

- `Circuit Breaker` ve retry

- `ek güvenlik` önlemleri
  - CORS policies burada tanımlanabilir
  - SQL injection, XSS gibi saldırılara karşı ilk savunma hattı olabilir.

- `SSL burada açılır`

- `darboğaz (throttling) yönetimi & rate limiter`

- `Quota (Kota) Yönetimi`

  rate limiter ile aynı mantıkta çalışır. ama amaç farklıdır. buradaki limitler (loyalty) kapsamında değerlendirilir ve son kullanıcının bildiği limitlerdir. örneğin `X` kampanyası açısından kullanıcının maximum istek hakkı olması gibi. rate limiter ise güvenlik önlemi amaçlıdır.

- `Static Response`

  response directly to client. examples:

  - `health check` `endpoints`
  - warning `API` or `HTML` response messages for maintenance.

- admin için (config güncelleme, status görüntüleme gibi) `CLI`, `API`, `UI` support

- live reload (config değiştirildiğinde restart istememesi)

- `gRPC`, `WebSocket`, `HTTP`, `TCP`, `UDP` gibi protocol'leri ve bunların farklı versiyonları destekleyebilir

- `otel` metric'leri saçabilir onu üstünden geçen işlemler hakkında

- `grpc` pooling yapabilir

- eklenti altyapıları olabilir (Örnek: `HTTP` interceptor yazmamızı sağlar)

- `K8s` için `gateway` yapısı sunar

- Distributed Tracing support (eğer zaten trace-id yoksa kendi oluşturur)

- Graceful Shutdown

- Tenant Isolation (header'a bakarak her isteği farklı fiziksel adrese yönlendirme)

## 📌 Piyasadaki bazı API gateway'ler

| name                     | focus         | `circuit braker` | written via | `grpc` | `TCP`   | `UDP` | advanced UI | admin `API`             | plugin                                |
|--------------------------|---------------|------------------|-------------|--------|---------|-------|-------------|-------------------------|---------------------------------------|
| `Kong` `Gateway`         | `api gateway` | +                | `Lua`       | +      | +       | +     | -           |                         | `Lua`, `Go`, `Python`, `Js`           |
| `Envoy proxy`            | `proxy`       | +                | `c++`       | +      | +       | +     | -           | +                       | `WASM` (`C++`, `Go`, `Rust`)          |
| `tyk` `Gateway`          | `api gateway` | +                | `Go`        | +      | +       | -     | +           |                         | `Go`, `JS`, `Python`                  |
| `layer7` `API` `Gateway` | `api gateway` | +                | `Java`      | -      | ?       | -     | +           |                         | `Java`, `JS`                          |
| `Nginx`                  | `http` proxy  | not important    | `C`         | +      | +       | +     | +           | + on enterprise version | `C`, `Js`                             |
| `Apache HTTP Proxy`      | `http` proxy  | not important    | `C`         | -      | limited | -     | -           |                         | `C`, `Lua`                            |
| `HAProxy`                | load balancer | +                | `C`         | +      | +       | -     | +           |                         | `Lua`                                 |
| `Apache APISIX`          | `api gateway` | ?                | `Lua`       | +      | +       | +     | +           |                         | `Lua`, `Java`, `Go`, `Python`, `WASM` |
| `Traefik`                | `api gateway` | ?                | `Go`        | +      | +       | +     | -           |                         | `Go`                                  |

`not important` olanlar: limited çünkü yazılımın focus'u değil.

yukarıdaki tablodaki tüm uygulamalarda aşağıdakilere native destek var:
- `http` 2
- `websocket`
- `load balancer`
- cache
- `jwt` token validation
- rate limiter
- data transformation
- `api composition`
- `TLS`

Ek notlar:

- `layer7`, `CA Technologies` (grup firması: `Broadcom`) tarafından geliştiriliyor.

- `Envoy proxy`

  `Service mesh` için geliştirtirildi. Bu sebeple odaklandığı konular:

  - native `GRPC` pooling destekliyor.
  - üstünden geçen istekler hakkında çok detay sunabilir monitoring için. örneğin `MongoDB` bağlantıları hakkında `OTEL` metric'leri sunar.
  - restart istemeden config değiştirilebilir.
  - ufak boyutlu ve hızlı

  bu sebeple `sidecar` pattern'de sürekli tercih ediliyor.

  `envoy gateway` farklı anlama geliyor. başka başlıkta anlatılıyor.

- aşağıdaki liste `JAR` olarak projeye ekleniyor, `api gateway` özellikleri programatik olarak yönetilebiliyor:
  - `Zuul` (`Netflix` tarafından geliştiriliyor. deprecated oldu)
  - `Zuul 2` (`Netflix` kapalı kaynak olarak kendi şirket içinde kullanıyor)
  - `Spring Cloud Gateway`

Not: `Zuul`'ün `edge server` olduğu birçok yerde yazıyor. Fakat network mimarimizde, uç noktada olmasından kaynaklı bu şekilde bir algı var. Fakat bu tam olarak doğru değil veya bir tartışma konusu.

## 📌 Gateway

çok genel bi terimdir. birçok şeyi kapsar:

- `Api Gateway`
- `edge server`
- HTTP proxy
- evde kullandığımız modem ve router
- şirketteki network firewall cihazı
- ve daha birçoğu.

## api gateway vs proxy

ikiside birbirinin işlerini yapabilir. keskin bir ayrım yok. ayrım; yapabildiklerinde değil, amaçlarındadır.

proxy temel olarak `URL` (`Path`, `port`) gibi bilgileri baz alarak işlem yapar. temel amacı; yönlendirme işlemi yapmaktır. Oysa `api gateway`; payload'ın kendisini de okuyarakta yapılabilen yetenekleri sunar.

## 📌 edge server (⟷ edge-server)

api gateway ve proxy ile çoğu yeteneği aynı olabilir. aralarında sadece amaç farklıdır:

`edge server` son kullanıcıya en yakın fiziksel noktaya konumlandırılır. böylece diğer business-servislerimize istek gelmeden bazı işleri bizim yerimize halleder. Bu sebeple `edge server` terimi sıkça `CDN`'lerde kullanılır.

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
- `OS` seviyesinde veya admin yetkisi ile çalışılmak zorunda değildir. Sadece user seviyesi yeterlidir.
- Aynı `OS`'ta, sadece istediğimiz istediğimiz (bazı) uygulamalarda VPN ve/veya Proxy kullanabiliriz. Fakat;
  
  - özellikle şirketler, çalışanlarının makinelerine admin yetkisi ile VPN ve/veya proxy kurar. Bu şekilde o VPN uygulamasının tüm `OS` uygulamalarında geçerli olmasını sağlarlar.

  - VPN vey Proxy uygulamaları admin olarak çalışmazlarsa, runtime'daki tüm uygulamaların connection'larını yönetemezler. `OS` izin vermez. Dolayısı ile her programın içine girip, VPN veya Proxy ayarlarını manuel set etmek gerekir ki bu çok uzun ve genelde elle yapılabilen bir işlemdir. Bu sebeple çoğu VPN veya Proxy uygulaması admin olarak çalışacak şekilde tasarlanır. VPN admin olarak çalıştırıldığında, kendini `OS`'a NIC olarak ekler. Bu NIC'i, ipconfig komutu ile basitçe görebiliriz. (Proxy'ler NIC'e mi ekliyor kendini bilmiyorum).

- Aynı `OS`'ta, aynı yazılımda farklı `TCP` bağlantılarında farklı VPN'ler ve/veya Proxy'ler kullanabiliriz veya kullanmayabiliriz. Yani VPN ve Proxy connection bazlıdır.

## 📌 OpenVPN

Açık kaynaklı VPN uygulaması ve protocol'üdür. Aynı zamanda bu yazılımı geliştiren firma, OpenVPN protocol'ü ve yazılımları ile bulut VPN hizmeti de sunmaktadır.

## 📌 SSL

Proxy ve/veya VPN SSL'i isterse yönetebilir. Fakat web tarayıcısı sertifika uyarısı verecektir. Eğer SSL'i yönetirse ve sertifika uyarısını ignore edersek, VPN ve/veya Proxy data'ları açık şekilde görebilir. Fakat eğer Web tarayıcısında JS'te değil isek, yani; native `OS` uygulaması kullanıyor isek, SSL yokmuş gibi connection açmadan önce (bağımsız) ek bir şifreleme yapabiliriz. Bu şekilde SSL açılsa bile aracı olan VPN ve/veya Proxy, data'ları anlamlandıramayacaktır. (Aslında bu işlemi Browser'da da yapabiliriz. Fakat network admin JS'teki key'i çok rahat elde edebileceği için, bir anlamı kalmayacaktır).

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
