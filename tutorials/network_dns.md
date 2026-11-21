# NETWORK DNS

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 DNS (⟷ Domain Name System)

internet adres resolving sistemini temsil eden genel bir terimdir.

__DNS Server (⟷ Name Server)__ ise `IP`'leri resolving yapmakla yükümlü yazılımdır. Bazen __Domain Name Server__ denir fakat yanlış bir kullanımdır.

## 📌 International Corporation for Assigned Names and Numbers (⟷ ICANN)

2019 yılı itibariyle hala en yetkili kuruluştur. Amerika'ya bağlıdır. __registrar (⟷ kaydedici)__ firmalar (örnek: Godaddy), ICANN'dan lisans almak zorundalar.

## 📌 IANA (⟷ Internet Assigned Numbers Authority)

ICANN için gerekli bazı teknik konuları tamamlayan kuruluştur.

## 📌 RIR (yad Regional Internet Registry)

ICANN ve IANA'nın altında dünyanın 5 farklı bölgesinde yetkilendirilmiş kuruluşlar vardır. Bu kuruluşlar kendi bölgelerinden sorumludur ve devlete bağlı kuruluş değillerdir.

- AfriNIC

  African Region Internet Registry: Afrika

- APNIC

  Asia Pacific Network Information Centre: Asya ve Pasifik

- ARIN

  American Registry for Internet Numbers: Kuzey Amerika

- LACNIC

  Latin America and Caribbean Internet Addresses Registry: Latin Amerika ve Karayipler bölgesi

- RIPE NCC

  Reseaux `IP` Europeens Network Coordination Centre: Avrupa bölgesi.

RIR alanları yukarıda yazıldığı kadar keskin çizgilerle ayrılmaz. Örneğin, ARIN bölgesi esas olarak Kuzey Amerika olmakla beraber Güney Amerika ve Afrika'ya da servis verir. RIPE bölgesi esas olarak avrupa olmakla beraber Orta Doğu ve Orta Asya içlerine kadar servis vermektedir. Türkiye, RIPE NCC bölgesindedir. RIPE NCC'nin merkezi Hollanda'dadır.

## 📌 LIR (⟷ Local Internet Registry)

kullanıcıya `IP` veren kuruluşlardır. ISP (⟷ Internet Sevice Provider) (Türktelekom, Superonline gibi)'lere denk gelir. Yasal anlamda şirketi olan herkes bu görevi alabilir. Türkiye'de u servisi sağlayan 100'den fazla kuruluş vardır.

## 📌 Web tarayıcının DNS sorgusu

sırası ile:

- web browser `DNS` cache
- `OS` `DNS` cache
- `hosts` file
- DNS server

  public `DNS` kendi içinde bilgiyi bulamaz ise, kendi gidip root `DNS server`'a sorar.

## 📌 hierarchy of DNS system

### 📌📌 1- root DNS servers

en üstte olan ve en yetkili sunuculardır.

Hangi TLD server'ın ne bilgi tuttuğunu bilir.

Kendi bilgi tutmaz. Kendisine gelen istekleri TLD'lere yönlendirir.

root server'lar dünyada 12 adettir (yıl 2019). bunlar ICANN tarafından belirlenmiştir. fakat sadece 1 tanesi ICANN kuruluşa direk bağlı bir sunucudur. bu sunucuya __ICANN Managed Root Server (⟷ IMRS)__ deniyor. diğerlerinin ICANN'a bağlı olmamasının sebebi, merkezi yönetimden kaçınmaktır.

ICANN'ın resmi sayfası olan <www.dns.icann.org> sitesinde root-servers.org'a referans verilmiştir. buradan 12 server'ı görebiliriz.

12 adet Root `DNS server` var, fakat bunlarda farklı instance'larla kendilerini çoğaltmışlardır. Yine bu instance'larda root-servers.org sayfasında görülebilir.

### 📌📌 2- top-level DNS server (o TLD server)

her TLD'nin, ".com", ".org" gibi sorumluluk alanları vardır.

Hangi authoritative server'ın ne bilgi tuttuğunu bilir.

Kendi bilgi tutmaz. Kendisine gelen istekleri authoritative server'lara yönlendirir.

### 📌📌 3- authoritative DNS server

Tüm DNS DB bu server'lardadır.

Yetkili sunuculardır.

### 📌📌 4- public DNS (⟷ non-authoritative DNS server ⟷ recursive DNS server ⟷ recursive DNS resolver)

Resmi terminolojide bu seviye yoktur. Bu seviyeye istersek Google DNS gibi public DNS'lerimizi koyabiliriz.

## 📌 whois server

registrar'a yeni bir domain eklendiğinde, registrar bu bilgiyi o domain'in uzantısının sorumlusu olan "whois server" tarafına yollar. örneğin ".com" ve `.Net` alan isimleri için verisign şirketinin whois sunucusu merkezi sunucudur. daha sonra bu bilgi diğer üçüncü parti whois server'lar tarafından klonlanır.

Dolayısı ile; root server'lar dahi ".com" ve `.Net` için gerekli tüm domain'leri local'lerinde tutmazlar.

Verisign firması ICANN'a bağlıdır ve ICANN tarafından yetkilendirilmiştir.

ICANN'dan her firma Godaddy gibi kolayca yetki alamaz. fakat Godaddy'ye kolayca aracılık edebilir, yani Godaddy'nin bayisi olabilir.

## 📌 hostname vs domain name

domain name bir bilgisayara DNS sunucusu tarafından atanmış string'dir. oysa hostname local bir network'te her makinanın kendisini dışarıya tanıttığı isimdir. network içinde hostname kullanılırken, dışarıya gidişlerde domain kullanılır. dışarıdaki bir yere giderken, oradaki network'teki bir alt makineye erişmek istediğimizde hostname.domainname.com gibi bir adresten yararlanılır. hostname ve domain name aynı görevi görür. "www" internet tarayıcıları için gerekli varsayılan makinanın hostname'idir.

## 📌 subdomain

"mail.Google.com", "map.Google.com" domain'lerindeki "mail" ve "map" kelimeleri birer subdomain'dir. ayrı `IP`'leri vardır. fakat istenirse aynı `IP`'ye de yönlendirilebilir.

## 📌 fully qualified domain name (⟷ FQDN)

hostname + domain name bir arada olduğunda verilen isimdir. örnek: tr.wikipedia.com, <www.wikipedia.com>

## 📌 top-level domain (⟷ TLD)

.com, .org, tr gibi terimlere verilen isimdir. TLD'nin çeşitleri vardır:

### 📌📌 country code top-level domain (ccTLD)

.tr, .fr, .de, .us gibi ülke bazlı gruplama için gerekli domain bilgisini ifade eder.

### 📌📌 generic top-level domains (⟷ gTLD)

.com, .org gibi kurumların tipini belli eden domain ifadeleridir.

## 📌 domain levels

example: mail.Google.com

- 1st level domain (⟷ first-level domain)

  com

- 2nd level domain (⟷ second-Level domain)

  Google

- 3rd level domain (⟷ third-level domain)

  mail

## 📌 Ters DNS Çözümlemesi (⟷ Reverse DNS lookup ⟷ reverse DNS resolution ⟷ rDNS)

`IP`'den `DNS` bulma işlemidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 DNS over TLS (DoT) vs DNS over HTTPS (DoH)

Standart `DNS` client'lar, `DoT` ve `DoH` sunucularına istek yapamazlar. çünkü farklı protocol'lerdir.

standart `DNS`'te isteğin çoğu açık gider/gelir. oysa `DoT` ve `DoH` 'ta istek tamamen şifrelenmiştir.

`DoT`, `TCP` soket bağlantısı açıp, o soketi üzerinden `TLS` ile `DNS` sorgusu atar. `DoH` ise `HTTP` protocol'ünden yararlanarak `DNS` isteği atar.

Bu iki protocol'ün amacı `ISP`'lerden gizlenmek değildir. çünkü `ISP`, `DNS` sorgusundan sonra, gideceğimiz `IP` adresini zaten görecektir. buradaki amaç;

- `man-in-the-middle (⟷ MITM)` attack'ı önlemek.
- neyi sorguladığımız local network admin'i veya `ISP` tarafından trace edilemez.
- `ISP`'lerin `DNS` sunucularındaki yaptığı engellemeleri ortadan kaldırmak.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 DNS record

DNS veritabanında birçok fakrlı kayıt çeşidi olabilir.

örnekler:

- `A`

  IP version 4

- `AAAA`

  IP version 6

- `NS`

  kayıt örneği:

  ```
  www.my-web-site.com CNAME my-custom-dns.com
  ```

  `www.my-web-site.com`'ın `IP` adresi sorulduğunda, `my-custom-dns.com` sunucusuna sorulması gerektiğini belirtiyor.

- `CNAME`

## 📌 Canonical Name record (⟷ CNAME record)

`canonical` kelime anlamı: kurallara uygun, geleneklere uygun.

`DNS` server'da şu kaydı oluştururuz:

```
www.my-web-site.com CNAME my-custom-service.com
```

Artık `DNS` server'a sorgu atanlara, `my-custom-service.com` `IP`'si dönecek.

`my-custom-service.com` bir `DNS` server değildir. Bu geleni farklı bir adrese yönlendiren gateway'dır. Client'lar gelirken, `HTTP` ile geliyorsa header'da, `TCP` ile geliyorsa `SSL`'de asıl gitmek istediği hostname'i `my-custom-service.com`'a bağlantı açarken yollar. `my-custom-service.com` bunu doğru sunucuya manuel yönlendirir. Buradan sonrası `my-custom-service.com` yazılımına kalmış artık.

Web tarayıcıları veya eklentileri 3üncü party siteleri engellemeye başladığından, bazı internet siteleri kendi sub domain'lere istek yaptırarak diğer 3üncü parti sitelere ulaştırmaktadırlar. Bu yönelndirmenin sebepleri:
- 3üncü parti server'ların reklamlarını göstermek
- 3üncü parti server'ların `CDN`'lerini kullanmak
- 3üncü parti server'ların Analytics sistemlerini kullanmak

## 📌 Azure DNS

`authoritative` `DNS Server`'dır. Domain kaydımızı burada tutarız.

Domain'i satın aldığımız firmadaki admin panelinde, `dns server olarak` `Azure DNS` adresini vermemiz yeterlir. `CNAME` kaydına ihityacımız kalmaz.

arkaplanda healtcheck gibi özellikler sunmaz. statiktir. basittir.

## 📌 Azure Traffic Manager

`authoritative` `DNS Server`'dır. Domain kaydımızı burada tutarız. Ama arkaplanda bizim app'lerimize healcheck yapar. Ona göre client'lara çalışan `IP`'yi döndürür.

`CNAME` kaydımız şu şekilde olmalıdır:

```
www.my-web-site.com CNAME my-unique-key-here.profilemanager.net
```

## 📌 Amazon Route 53

`authoritative` `DNS Server`'dır. Aynı zamanda arkaplanda healcheck hizmeti de sunar. Yani hem `Azure` `Traffic Manager` hemde `Azure DNS`'in yaptığını aynı hizmette sağlar.