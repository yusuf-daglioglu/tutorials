# WEB HTTP HEADERS

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 HTTP headers

> Age

proxy üzerinde kaç defa cache'den bu değerin kullanıldığıdır. otomatik proxy tarafından her defasından 1 arttırılır.

> Connection

keep-alive, close gidi değerler alır. Bir sonraki isteğin bu istekte kullanılan tc soket üzerinden gitmesini sağlar. bir sonraki istekler gönderileceği kesin ise; performans artışı sağlar. bu header'dan da anlaşıldığı gibi SSL bağlantıları her istekte eğer keep-alive gidiyorsa yeni soket üzerinden yapılmaz.

> Authorization

Standart authentication işlemleri için kullanılan header

> Proxy-Authorization

Authorization ile aynı görevde fakat proxy'nin önemsediği değerdir.

> Accept

request'in kabul edeceği mimetype

> Accept-Charset, Accept-Encoding, Accept-Language

request'in kabul edeceği tipler

> Content-MD5

sadece body kısmının md5'i.

> Content-Type

sadece body kısmının mimetype'ı.

> TE

Transfer encoding'dir. Body kısmındaki verinin nasıl tutulduğunu belirtir. örneğin compress yapılmış ise; gzip gibi.

> Date

request'in oluşturulduğu tarih

> Max-Forwards

bu isteğin en fazla kaç tane proxy (aracı) üzerinden gidebileceğini limitler.

> DNT

do not track. 1 veya 0 değerini alır.

> Origin

CORS için gereklidir. O anda URL barda bulunan domain adresidir. Bunu Ajax'ı yapan JS değil, tarayıcının kendisi doldurmaktadır.

> Range

Büyük body kısmı olduğunda sürekli olarak arka planda tekrar işlem gönderiliyor. Böyle durumlarda bu header'a body'nin hangi byte'larının (byte aralığı) gönderildiği belirtilmektedir.

> Forwarded

X-Forwarded-For yerine kullanılan standartlaşmış bir header'dır.

client proxy'ye istek yaptığında bu değer boştur. daha sonra ilk proxy 2'inci proxy'ye istek atarken bunu doldurur. 1. proxy 2.inci proxy'ye istek yaparken bu header bu şekildedir:

Forwarded: for=CLIENT_IP

Daha sonra 2. proxy gerçek sunucuya isteği yönlendirdiğinde bu header şöyledir:

Forwarded: for=CLIENT_IP, for=PROXY1_IP; by=PROXY3_IP; proto=http; host=example.com

> Via

forwarded ile aynı bilgileri içerir aynı zamanda kullanıla protocol'lerin sürümlerini de içerir.

> Host

istek yapılan URL'nin adresi ve portu. path buna dahil değildir.

> Referer

Kelime olarak yanlış yazılmıştır fakat artık herkes bu şekilde kullanmaktadır.

Bir başka isteye yönlendirmelerde referer kısmı doldurulur. kullanıcının hangi siteden yönlendiğini anlamak için kullanılır.

> X-Forwarded-For

standart olmayan bir header'dır. Forwarded'dan önce vardı. fakat standart olarak referer sonradan belirlendi.  Fakat hala x-forwarded-for'u kullanan yazılım/donanımlar var.

X-Forwarded-For IP bilgilerini tutar. bir proxy bir isteği başka yere yönlendirdiği zaman bu bilgide orijinal isteği yapan sunucunun IP'sini barındırır. örnek:

X-Forwarded-For: client, proxy1, proxy2

> X-Forwarded-Host

X-Forwarded-For ile aynı bilgiler içerir. sadece IP yerine domain bilgisi tutar.

> X-Forwarded-Proto

X-Forwarded-For varken protocol'lerin yazılması için ayrı bir header belirlenmişti. her aracı bir sonraki aracıya isteği yaparken hangi protocol kullandıysa (örnek https) onu buraya yazar.

> X-Forwarded-IP

standart değildir. mail sunucularına istek yaparken kullanılan bir değerdir.

> Access-Control-Request-Method

CORS için gerekli. "Preflight" isteklerinde kullanılır. Preflight onayı alınırsa gönderilecek asıl isteğin metodudur (örnek : "put").

sunucu asıl request'e izin verip vermeyeceğine karar verebilmek için preflight'ta bu tarz detayları ister.

> Access-Control-Request-Headers

CORS için gerekli. "Preflight" isteklerinde kullanılır. Preflight onayı alınırsa gönderilecek asıl isteğin header'ını barındırır.

sunucu asıl request'e izin verip vermeyeceğine karar verebilmek için preflight'ta bu tarz detayları ister.

> Access-Control-Expose-Headers

CORS isteklerinde sunucu bu header'ı client'a döndürür. web tarayıcı JS tarafında sadece:

- burada belirtilecek olan header'ların
- default olarak web standartlarında belirtilmiş header'ların

okunmasına izin verecektir.

> Access-Control-Allow-Origin

Response'ta bulunur. Preflight ‘a cevapta bulunur. Server'ın izin verdiği tüm domain'leri içerir. bu domain'ler URL bar'daki domain'ler oluyor.

> Access-Control-Allow-Methods, Access-Control-Allow-Headers

Response'ta bulunur. Preflight'a cevapta bulunur. Server'ın izin verdiği bilgilerdir.

> Access-Control-Allow-Credentials

Response'ta bulunur. Preflight ‘a cevapta bulunur. Server'ın izin verdiği güvenlik bilgilerini içerir. örneğin; asıl atılacak istekte cookie olup olmayacağını belirtir.

> Access-Control-Max-Age

Preflight response'unda bulunur. Bu o isteğin ne kadar süre gereli olduğunu belirtir.

> X-Requested-With:

Bu header her zaman XMLHttpRequest değerini alır. Bunun yollanma sebebi isteğin CORS olup olmadığını ayırt edebilmektir. Çünkü CORS standartlarında yollanan isteklerde sadece bazı header'lar yer alabiliyordu. Bu header listede olmadığından CORS standartları çerçevesinde atılmadığını anlamak için kullanılır.

> User-agent

tarayıcı yada isteği yapan client'ın bilgisi

> Upgrade

istek yapılan yere HTTP sürümünü güncellemesi isteğidir.

> Status

HTTP status code

## 📌 Bazı HTTP Status code'lar

- `1xx` Informational responses: payload gönderilmeye devam ediliği zaman, protocol sürümü değiştirileceği zaman...
- `2xx` Success
- `3xx` Redirection
- `4xx` Client errors: URL too long, bad request, method not found, authentication failed.

  - 400 Bad Request

    Örnek: Malformed JSON body.

    Burada busines input validasyonu olmaz.

  - 422 Unprocessable Content

    Input business validation başarılı olmadı.

    Bu ilk WebDAV protokolünde kullanılıyordu, ama daha sonra sadece HTTP kullanan yazılımlar tarafından desteklenmeye başladı.

  - 401 Unauthorized

    Kullanıcı login gerekli. Yetkisi olmaması durumu değil!

  - 403 Forbidden
  
    Kullanıcının yetkisi yok.

  - 404 Not Found

    URL geçersiz veya URL'deki kaynak yok.

- `5xx` Server error: internal problems on server.

  - 500 Internal Server Error

    Teknik hatalar.

    "Business hatası" diye bir şey yotur zaten. Olmamalı. Bir iş akışında, bir işlem yapılamaz veya yapılması yasak olabilir. Ama bunlar error değildir. Bir veriye erişilemiyorsa akış duruyorsa bu hata teknik bir hatadır.

## 📌 HTTP Status code (tüm liste)

- https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml

  Burada her kodun hangi `RFC`'ye karşılık geldiği de yazıyor.

- https://learn.microsoft.com/en-us/troubleshoot/developer/webapps/iis/health-diagnostic-performance/http-status-code

  Burada `ISS`'in döndüğü özel statüsler de var.

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status

  Her bir statü için kısa cümle açıklama aynı sayfada.

## 📌 header isimlendirmesi

Eskiden `x-` ile başlayanlar standart olmayan header için kullanılıyordu. fakat bazı header'lar sık kullanılmaya başlandığından, artık standart hale geldiler. bu sebeple artık custom header'lara farklı bir prefix yazılması önerilir.

## 📌 relative quality factor

`q` parametresi `accept` keyword'ünü içeren header'ların (`Accept-Charset`, `Accept-Encoding` gibi) value kısmında kullanılmaktadır. bu değer 0 ile 1 arasında bir sayı değeri almaktadır. örnek:

```text
Accept: text/plain; q=0.5, text/html, text/x-dvi; q=0.8, text/x-c
```

Yukarıdaki header şu anlam geliyor: `html` ve `x-c` en öncelikli (`q=1` default), eğer bu tipleri sunucu desteklemiyorsa, `dvi` olarak cevap version. eğer onu da desteklemiyorsa, `plain` cevap version.

## 📌 header'lar cookie'leri barındırır

header'lar arasında cookie diye bir değer bulunur. bu değerde cookie'ler taşınır. formatı şu şekildedir:

- client sunucuya atarken Header içerisinde şu satırlar yer alır:

```text
Cookie: name=value; name2=value2
Cookie: name3=value3
```

Yukarıda expire secure gibi nitelikler isteğe bağlıdır.

- Sunucudan client'a response içerisinde dönen header'lar

Yukarıdaki ile aynı formattadır. tek farkı "Cookie:" yerine "Set-Cookie:" yazmasıdır.

```
Set-Cookie: name=value; name2=value2; Expires=12.04.2018; Secure
Set-Cookie: name3=value3
```

her tarayıcı isteğin yapıldığı domain'e bağlı tüm cookie'leri sunucuya otomatik gönderir. her defasında yollanması istenmeyen cookie'ler, cookie olarak değilde, farklı storage'lere atılmalıdır.
