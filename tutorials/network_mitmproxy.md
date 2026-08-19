# NETWORK PROXY AND API GATEWAY

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Web Debugging Proxy Application

Yazılımlar network üzerinden Ajax işlemi yaparlar. Bu Ajax'ları intercept edip devam ettirmek ve manipüle etmek için aracı programlardan yararlanılır. Bu programlara genelde "Web Debugging Proxy Application" uygulamaları denir.

tüm network trafiğini intercept eden uygulamalar:

- `Fiddler`
- `Charles`
- `Mitmproxy` (açık kaynaklı): ismi `man-in-the-middle-proxy` kısaltmasından gelmektedir.
- `Tamperchrome`: `Chrome` için `Google`'nin geliştirdiği eklenti. sadece `Chrome` üzerinden çıkan Ajax işlemlerini intercept edebiliyor.

## 📌 Temel çalışma mantığı

"Web Debugging Proxy Application" aslında bir proxy uygulaması. bu sebeple örneğin; Firefox uygulamasından gidecek Ajax istekleri incelenecek ise; Firefox'un proxy ayarlarına "Web Debugging Proxy Application" proxy sunucusunun açtığı IP:port bilgisi veriliyor. artık Firefox'un yaptığı istekler "Web Debugging Proxy Application" GUI ekranında listelenmeye başlıyor.

## 📌 localhost problemi

lokalde development yaparken "Web Debugging Proxy Application" uygulamaları localhost'a yada 127.0.0.1'e yapılan istekleri yakalayamayabilir. bu gibi durumlarda localhost yerine makine-name'mimizin host-name'ini kullanmayı deneyebiliriz. çünkü localhost direk `loopback` arayüzüne yönlendirilirken, makine-host-name'mimiz network üzerinden sorgulamaya çalışılıyor.

## 📌 Web Debugging Proxy Application'lerin SSL bağlantılarını izlemesi

normal koşullarda SSL bağlantısını kimse takip edemez. fakat, Proxy uygulamamız kendi `CA` sertifikasını OS'a yada takip edeceğimiz client'a (`Java` app, web browser gibi) kurdurtuyor. Client isteği proxy'ye atıyor. proxy o anda, sisteme tanıtılmış olan `CA`'ye uygun bir sertifika üretiyor. proxy, isteği sunucuya kendi atıyor(yönlendirmiyor). dönen cevabı da kendi sertifikası ile imzalayıp döndürüyor. yani proxy, gerçek isteği yönlendirilmiyor. proxy arada sahtecilik yapıyor.

burada bazı sorular/sıkıntılar var. çözümleri ile birlikte açıklamak gerekirse:

- web tarayıcı DNS sorgusu sonrası sunucuya IP ile gidiyor. dolayısı ile HTTP isteği IP ile bağlantı kurulup yapılıyor. yani proxy hangi domain'e gitmek istediğini bilemiyor. bu sebeple nasıl o domain'e uygun sertifikayı o anda üretiyor?

  Proxy IP'ye aynı anda bir request atıp handshake işlemi yapıyor. proxy o IP'li sunucunun sertifikasına bakıyor. sertifikasında olan "Common Name" bilgisi zaten domain bilgisini vermektedir.

- bazı sertifikalar birden fazla domain bilgisi içerebiliyor. bu gibi durumda, client'ın hangi domain'e gitmek istediğini proxy nereden bilebilir?

  Proxy sunucudan indirdiği SSL sertifikasındaki "Subject Alternative Name" değerine bakıyor. burada birden fazla domain ismi oluyor. proxy o anda tüm bu domain'lere uygun bir sertifika üretiyor ve client ile bununla haberleşiyor.

şirketler çalışanlarının HTTPS bağlantıları bu yöntemle takip etmektedirler. her OS'a `CA` tanıtmaktadırlar.

eğer bir kullanıcının `TCP` ile iletişim kurulacak yazılımına `CA` kurulmamış ise; tüm network trafiği izlense de kullanıcının bilgileri görünemez. çünkü client taraf, daha ilk mesajından itibaren sunucunun `private key`'i ile şifreli şekilde data'ları yolluyor. şirketler `CA` sertifikasını tüm web tarayıcılarına ve OS'a baştan root yetkisi ile kuruyor ve kimseye sildirtmiyor. bu sahte `CA` sayesinde şirket aradaki tüm bağlantıları açık şekilde izleyebiliyor.

peki biz portable bir Firefox kurup, OS'tan sertifikaları algılamamasına göre konfigüre edersek ne olacak? bu sefer bağlantıyı şirket takip edemeyecek fakat bağlantıda giden gelen data'lar anlamsızlaşacak ve iletişim kurulamayacak.

## 📌 Mitmproxy modes

`Mitmproxy` birden fazla mode ile çalışabilir. bazıları:

- __SOCKS proxy__

  Client ile proxy arasında kullanılan protocol'e `SOCKS` denir.

  Birçok sürümü vardır: `4`, `4a`, `5`.

  HTTP-proxy'ler sadece `HTTP` ve `HTTPS` isteklerini karşılarken, `SOCKS` proxy daha çok bağlantı çeşidini destekliyor: örnekler: `TCP`, `FTP`, `POP3`, `SMTP`.

- __Reverse proxy__

  `reverse proxy`'lere bazı kaynaklarda `gateway` de deniliyor. `kaynak: http://httpd.apache.org/docs/2.4/mod/mod_proxy.html "Forward Proxies and Reverse Proxies/Gateways" başlığı, 1inci paragraf.`

  Fakat `gateway` terimi çok genel (birçok amacı kapsayan) bir terim olduğu için tercih edilmemeli.

  proxy olup olmadığını client'ın bilmesine gerek kalmadığı proxy'dir.

  sunucu tarafta (data center'ında) olan proxy'dir. target sunucularının önünde durur.

- __regular proxy__

  default mode. "__forward proxy__" olarak davranır.

  client, proxy olarak bunu kullanacağını bilmelidir.

- __Upstream proxy__

  birden fazla proxy zinciri varsa, Mitmproxy bu zincirdeki bir proxy olarak çalışabilir.

- __Transparent proxy__

  Transparent: saydam, şeffaf, transparan

  `rfc 2616` standardında belirtildiği gibi request ve response'u hiç değiştirmeyen proxy'lere denir. __non-transparent proxy__ ise request veya response üzerinde bazen yada her zaman değişiklik yapan proxy'lerdir.

  aynı bilgi burada da yazmaktadır: `<https://www.fortinet.com/resources/cyberglossary/transparent-proxy> 1inci paragraf.`

  önce router'a istek atan client, daha sonra router tarafından `Mitmproxy`'ye yönlendirilir. daha sonra `Mitmproxy` default mode'daki gibi gelen istekleri yönlendirir. (aşağıdaki başlıkta daha detaylı anlatılıyor)

  Bu proxy'ler genelde network admin'leri tarafından, giden-geleni log'lama, kötü domain'leri engelleme gibi amaçlar için kullanılır.

## 📌 (only for mitmproxy) Transparent Proxy (⟷ intercepting proxy ⟷ inline proxy ⟷ forced proxy)

Mitmproxy, Transparent mode'a ayarlandığında, aşağıdakilerden sadece bir tanesi yapılması yeterlidir:

- spoof edilecek cihazın network ayalarında gateway olarak Mitmproxy'nin çalıştığı bilgisayarın IP'si verilmelidir. yani gateway = Mitmproxy olacaktır. (bu durumda router'ın ayar yapmasına gerek kalmaz)
- router'ımızda Mitmproxy konfigürasyonu yapılmalıdır. bu ayar ile client, önce router'a gitmekte, router istekleri ise Mitmproxy'nin olduğu sunuya yönlendirmektedir. (bu durumda client'ın ayar yapmasına gerek kalmaz) (kaynak: https://docs.mitmproxy.org/stable/concepts-modes "Transparent Proxy" başlığı, 1inci cümle.)

yukarıda 2 farklı çözüm ile Transparent mode'un çalışabileceği için kaynak: https://docs.mitmproxy.org/stable/concepts-modes "Common Configurations" başlığı.
