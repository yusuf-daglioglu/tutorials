# SECURITY

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Centralized Networks

It use __centralized servers__.

Systems like Google, Facebook, Wikipedia... It has only one authority.

## 📌 Federated Networks

It use __decentralized servers__.

Federated messengers use multiple and independent centralized servers that are able to talk to each other.

Example:

- Email system (Gmail, Hotmail are independent but can communicate with each others)
- Matrix protocol ("Element" application use this protocol. You can find many independent matrix servers.)

## 📌 Peer-to-peer networks

It use no servers. It is distributed network.

Examples:

- `Briar` application (it send messages directly using other client's `IP` address)
- `Bittorrent` protocol.
- `Bitcoin` and many other `cryptocurrency` (But not all of them are `Peer-to-peer`).

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 threat model (⟷ tehdit modeli)

A threat model is a list of the most probable threats for a specific issue/topic. Since there is no way securing everything we should focus/prioritize the threads.

Example threat models:

- A threat model of journalist might be a foreign government. He has to protect himself against foreign government.
- A company's manager's threat model might be (protecting themselves against) a hacker.
- The average citizen's threat model might be (hiding their data from) large tech corporations.

The security precautions depends on the threat model. Examples for an average citizen who want to hide his data can be:

- If he use MS Windows, it's better for him to do not use a third party antivirus software. Microsoft has already a built-in defender and firewall.
- On iOS the web browsers should use Safari's web engine. This is a rule of Apple company. So in this case, for an iOS user it is better to use Safari for web browsing.
- He should not prefer 3rd party DNS providers, if his ISP has encrypted DNS.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 zero-day (⟷ sıfır-gün ⟷ 0-gün ⟷ sıfır-saat)

zero-day is the software vulnerability which is not known yet by vendors or it's publicly not known.

The "zero-day" term means the vulnerability itself. but in some articles authors use "zero-day vulnerability" term.

n-day (example: 1-day, 2-day) means the number of the days since the first disclosure of the vulnerability.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 impersonation

Türkçe kelime anlamı: kimliğe bürünme

impersonate --> taklit etme

bir user ile başka bir user'ın şifresini bilmeden onun yerine giriş yapmak isteyebilir. bu genelde yöneticiler tarafından yapılmaktadır. bu işleme impersonation denir.

Fakat yazılımın arka planda impersonation'dan haberi olup olmayacağı standartlarda esnek bırakılmıştır. kaynak: RFC 8693 title: "1.1.  Delegation vs. Impersonation Semantics". Şu şekilde belirtilmiştir: "It is true that some members of the identity system might have awareness that impersonation is going on, but it is not a requirement.".

## 📌 delegation

impersonation'dan farklıdır. delegation'da; A user'ı, B user'ını delegate ediyor ise, "merhaba $userName" basan bir sayfada "merhaba A" görecektir. Yani, A kendi kimliğini taşımaya devam etmektedir. A user'ı, B'nin rollerini (önceden delege edilen (konfigüre edilen) rollerini) taşımaktadır (tümünü taşımak zorunda değil).

## 📌 keycloak ile impersonation

keycloak token impersonation ile Oauth 2.0'a ek uzantı olan "OAuth 2.0 Token Exchange" özelliğini kullanmaktadır. kaynak: RFC 8693. Fakat süreç tamamen aynı değildir ve production-ready değildir (2021 Mart).

- Production-ready destek olmadığı için keycloak'u şu şekilde başlatmak gerekiyor:

```sh
/keycloak-12.0.4/bin/standalone.sh -Dkeycloak.profile.feature.token_exchange=enabled -Dkeycloak.profile.admin_fine_grained_authz=enabled
```

- impersonation yapacak olan user'a, "realm-management" altındaki "impersonator" rol'ü atanmalıdır.

- User normal login sürecini tamamlar ve kendi access_token'ını alır.

- keycloak'a şu istek atılır:

```yaml
POST http://localhost:8080/auth/realms/realm1/protocol/openid-connect/token

grant_type: urn:ietf:params:oauth:grant-type:token-exchange
client_id: $client-id
requested_subject: 1ab7cfd4-5401-4985-b264-145c25aa6c19 (taklit edilecek user'ın Keycloack-ID'si)
subject_token: $current(impersonator)-access-token
client_secret: $client-secret
```

Response olarak bize access ve refresh token dönecektir. Bu dönen JWT'leri decode edersek içerisinde taklit edilen user'ın bilgilerinin (role, isim...) olduğunu göreceğiz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 vault

`Hashicorp` firması tarafından geliştirilen açık kaynaklı key management yazılımı.

## 📌 config file

`HCL` (`HashiCorp Configuration Language` kısaltmasıdır) formatındadır. `HCL`, `JSON` ve `YAML`'ye alternatif olarak geliştirilmiştir. örnek syntax:

```hcl
service {
    key = "value"
}

service2 {
    key2 = "value2"
}

// comment-1

## comment-2

/* multi-line
   comment */

root1 "child1" {
    prop1 = "value1"
}
// equivalent to YAML:
// root1.child1.prop1: value1
```

## 📌 initialization

`vault` sunucusu başladığında sadece 1 kere init etmek gerekli. init output'u olarak şunlar basılıyor:

- 5 adet unseal-key'lerı

  unseal-key'lerden en az 3 tanesi ile vault ancak unseal edilebiliyor. Unsealing süreci her sunucu restart'ında gerektiği için bu key'lere ihtiyacımız olacak.

- 1 adet root token

  bu token ile client her işlemi yapabiliyor.

## 📌 secret engine

vault'ta her şeyi gruplamak amaçlı her "secret" belli 1 adet "secret engine" altında olmalıdır. bu şekilde bir secret engine'i komple disable edebiliriz yada enable edebiliriz.

## 📌 authentication

client'ın vault'tan data çekmesi veya manipülasyon yapabilmesi için her uygulamada olduğu gibi önce login olmalıdır. "token" ile authentication'da buna bir örnektir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 subresource integrity

web tarayıcıları indirdikleri dosyanın hash'ine bakarak validasyon yapar. eğer valid değilse o dosyayı HTML'e import etmez.

```html
<script src="https://example.com/example-framework.js"
        integrity="sha384-oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/uxy9rx7HNQlGYl1kPzQho1wx4JwY8wC"
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Trust on first use (⟷ TOFU ⟷ trust upon first use ⟷ TUFU)

2 makinenin haberleşmesinde ilk aşamada birbirlerine bir identifier üzerinden güvenmeleri tekniğidir. TOFU kullanan birkaç örnek üzerinden gidelim:

- `SSH`

  SSH client sunucuya bağlandığında, eğer bu sunucuya giderken (eğer bu sunucu için daha önce son kullanıcıdan onay alınmamış ise), son kullanıcıya sunucunun identifier'ini gösterir. eğer son kullanıcı onaylar ise artık bu bilgiyi shh client $HOME dizinine kaydeder ve bir daha son kullanıcıya sormaz.

- `HPKP`

  browser sunucuya erişip sertifikasını aldığında onu valide ederse doğru olarak kabul eder ve bu bilgiyi DB'ye kaydeder. İlerideki zamanlarda tekrar aynı sunucuya gidildiğinde, DB'de bulunan key'lere göre sunucuyu valide eder.

- `Signal` messenger

  `Signal` karşı tarafın identifier'ini son kullanıcıya sadece gösterir ve karşı taraf ile iletişimi son kullanıcının onayı olmadan başlatır. yani `TOFU`'yu direk(onaysız) uygular. Son kullanıcı eğer isterse identifier'ı karşıdaki kişiden manuel barcode aracılığı ile kontrol edebilir.

  Eğer karşıdakinin identifier'i değişirse, signal bizi uyarır. fakat yine iletişime devam eder.

  eğer biz karşı tarafın identifier'ini signal uygulaması içerisinden "verified" olarak işaretlersek ve karşıdakinin identifier'i değişirse, signal sadece uyarmakla kalmaz aynı zamanda iletişimi block'lar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Crypto-shredding

shredding kelime anlamı: parçalama

şifrelenmiş data'ları kullanılamaz hale getirmek için tüm şifrelenmiş data'ları tek tek override etmek veya silmek yerine, sadece decryption key'i silme çözümüdür.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 3 state of data in computer science

- __Data at rest__

  data'nın, kalıcı storage'de durma durumudur.

- __Data in transit (⟷ data in motion ⟷ data in flight)__

  data'nın, network üzerinde taşındığı durumlardır. IPC gibi durumlar bu gruba dahil değildir.

- __Data in use__

  data'nın, RAM, "CPU cache" gibi kalıcı olmayan bellekte olma durumudur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 confused deputy problem

deputy kelime anlamı: vekil

client olarak; bir servise bir istek yaptık, bu servis işlemi yapabilmek için diğer servislere de istek yapıyor. dolayısı ile diğer servisler ilgili isteğin nereden geldiğini bilebilmeli. güvenliği buna göre implemente etmeliyiz. çünkü diğer servislere dışarıdan da istek gelebiliyor. bu istek internal'dan mı geliyor yoksa client'tan mı, her servis bunu ayırt edebilmeli. Aynı zamanda client yaptığı bu istek ile diğer servislere istek tetikleyebilmiş oluyor. burada farklı zafiyetler söz konusu olabiliyor. bu durum "__confused deputy__" problem olarak adlandırılıyor. Çünkü her aracı servis client adına istek atmış oluyor. bu aracı servisler, bu problem kapsamında vekil olarak adlandırılıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 secret zero problem (⟷ secret-zero problem)

bir data'yı şifrelemek için şifre kullanırsınız. bu şifreyi de şifrelemek için bir şifre kullanırsınız. bu şekilde 100 adım dahi yapabiliriz. fakat her zaman mutlaka bir şifreyi koruma gereği duyacağız. bu şifre'ye "secret zero" denir. "secret-zero problemi" ise; bu şifreyi nasıl koruyacağımızdır.

"secret zero" problemine çözüm bulmak teorikte imkansız. çünkü hack'lenecek her zaman son bir sistem/şifre olmalıdır. fakat bazı sistemler son katmanı parçalara bölerek "secret zero" problemini kısmen de olsa çözmektedirler. örneğin, bir "master key", "secret zero" olsun. bu master key'i, 2 ye bölersek, o zaman bu master key ile işlem yapmak istediğimizde bu 2 kişiye de ulaşmamız gerekecek. yani; "secret zero" tek bir yere bağımlı değil. her kişide multi-factor authentication sistemleri ile korunur. bu şekilde secret-zero'yu korumak için farklı bağımlılıklarda eklenmiş olur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 SQL injection (⟷ SQLi)

- SQL injection'dan kaçmak için ORM framework sürümümüzün güncel olmasına dikkat etmeliyiz. çünkü ORM'nin aşağıda örneklendirilen bind işleminde kontroller gerçekleştiriyor.

- Dışarıdan alınan parametreleri bind ederek tanımlamalıyız:

```java
Query query = createQuery("from LoginUsers where userName=:userName");

query.setParameter("username", userName); //ORM burada userName'in tipini biliyor ve kontrolleri kendisi yapıyor.
```

Bind etmeyi sadece SQL komutları için değil, JPQL için dahi yapmalıyız. SQL ile JPQL farksızdır. JPQL; ancak bind edilirse güvenliği sağlar.

- Dinamik query yapılacak ise Criteria API'den faydalanılmalı

## 📌 tipleri

### 📌📌 In-band SQLi (⟷ Classic SQLi)

#### 📌📌📌 Error-based SQLi

Exception detayları uygulamadan dönüyor ise, bunlardan yola çıkarak sunucudan bilgi toplanır.

#### 📌📌📌 Union-based SQLi

```sql
select * from X where ' $VAR '
```

gibi SQL'lerde VAR input'unu değiştirerek sunucudan bilgi toplanırız. VAR yerine bir şey eklememiz ancak ve ancak UNION sorguları ile mümkündür. bu sebeple bu ataklara Union-based denir.

### 📌📌 Inferential SQLi (⟷ Blind SQLi)

#### 📌📌📌 Boolean-based (⟷ content-based) Blind SQLi

Sunucuda bu şekilde bir HTTP Controller olsun:

```java

@HTTP_GET
isExist(String input){

    boolean exist = runSQL("SOME SQL" + input);

    if(exist){
      return "true";
    } else {
      return "false";
    }
}
```

Bu controller'dan bilgi almak için şöyle bir yöntem geliştirilebiliriz:

input'taki SQL'imiz önce tüm tabloları çeken SQL'i çalıştıracak. sonra aynı SQL'de, bu tablo listesindeki ilk elemanın (ilk tablo adının) ilk karakteri A mı diye bakılacak. eğer A ise "boolean exist = true" olacak şekilde dönüş yapılmalı. eğer true olursa demek ki ilk karakter A.

Aynı işlemi ilk tablonun ikinci elemanı içinde yapmalıyız ve tüm alfabe ve özel karakterler için yapmalıyız. Her tablo adı 100 karakter olsa, 100 tablo olsa, 50 karakter kombinasyonu olsa, 100\*100*50 adet HTTP isteğinde tüm tablo listesini almış oluruz.

Bu işlemi daha hızlandırmak için farklı yöntemler mevcut. Örneğin; Tablo isminin ilk karakterinin ASCII karşılığı alınır. Örneğin bu değer 50 olsun. Biz SQL sorgumuzda bu değer 50'den küçükse "boolean exist = true" dönecek şekilde ayarlarız. Eğer true dönerse; ASCII 50'nin üzerindeki hiçbir karakter için HTTP denemesi yapmamıza gerek kalmayacak. Bu tarz trick'lerle deneme sayımızı çok azaltabiliriz.

#### 📌📌📌 Time-based Blind SQLi

controller'ımız boolean dahi dönmüyor olsun:

```java
@HTTP_GET
anyMethod(String input){

    runSQL("SOME SQL" + input);
}
```

Buradan bilgi alabilmek için Boolean-based ile aynı SQL'leri çalıştıracağız. Fakat dönüşümüzde true false alamadığımızdan, dönüş değerimiz yerine zaman aralığından faydalanacağız. SQL'lerimiz true durumunda sleep(7) gibi bir SQL fonksiyonu çalıştıracak. eğer 7 saniye içinde cevap gelmezse, o SQL sonucunu true olarak varsayacağız.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Insecure Direct Object References (⟷ IDOR)

örneğin DB satırının primary-ID'leri API'de CRUD amaçlı kullanılıyorsa, bunlar IDOR'dur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 PIN (⟷ Personal Identification Number)

geçici olarak kullanılan şifredir.

## 📌 PIN pad (⟷ PIN entry device ⟷ PED)

PIN girilen cihazlara verilen genel isimdir.

## 📌 Multi-factor authentication (⟷ MFA)

factor Türkçe kelime anlamı: faktör, etken

birden fazla faktörden onay alınması. faktörler:

- Bilgi (Knowledge)

  Şifre, PIN, güvenlik sorusu

- Sahiplik (Possession)

  Telefon, donanım anahtarı

- Biyometri (Inherence)

  Parmak izi, yüz, retina

MFA örnekleri:

- SMS codes
- email codes
- app push notifications
- Time-based One-time Passwords (TOTP)
- Yubico OTP
- FIDO2/U2F

## 📌 Two-factor authentication (⟷ 2FA)

MFA'nın 2 adımlı olanıdır.

## 📌 Two-step verification (⟷ two-step authentication)

MFA değildir.

örneğin CAPTCHA + şifre kullanmak Two-step verification'dır. Ama 2FA değildir.

## 📌 TOTP (⟷ Time-Based One-Time Password)

Bir OTP çeşididir. Offline olarak OTP üretir. Bu OTP sadece belirli bir süre geçerlidir. Bu sebeple Time based'dir.

## 📌 Google authenticator

TOTP kullanımını sağlayan bir GUI uygulamasıdır. Kullanıcı parametre olarak OTP'nin yenileneceği döngü süresi (varsayılan 30 saniye) ve secret-key'i Google'den alır. Bu parametreler ile 30 saniyede bir yeni bir OTP üretir. Bu OTP'nin algoritması bellidir. Bu sebeple offline olarak OTP üretilebilir.

"Google authenticator" açık kaynaklı bir algoritmaya dayanır. bu sebeple alternatifi birçok client ve programlama dili API'si mevcuttur.

## 📌 otp hakkında

otp süresi algoritma göre değil, kullanıldığı hizmetin kendisine göre değişir. yani; Google'ın kullandığı algoritma 30 saniyeliktir. fakat aynı algoritmayı 40 saniye olacak şekilde de ayarlayabiliriz.

otp şifresi T zamanında üretilmiş olsun. artık bu T zamanında üretilen otp Google için 30 saniye süresinde kabul görecektir. T+1 zamanında aynı secret key ile bir OTP üretelim. Bu password'de artık 30 saniye geçerli olacaktır. dolayısı ile T+1 de oluşturduğumuz key daha farklıdır. Fakat T+2 anında her 2 key'imizde geçerlidir.

Örneğin; <https://github.com/bilelmoussaoui/Authenticator> uygulaması ile masaüstünde aynı secret key ile iki şifre oluşturduğumuzda, iki şifre farklı zamanlarda oluşturulacağı için farklı key göstereceklerdir. her 30 saniyede bir yeni key oluşturacaklar ve her ikisinin döngüsünün sona erdiği an farklı olduğundan hep farklı key üretecekler.

Bazı servisler her 2 anahtarı da kabul etmektedir. Fakat Google bunu kabul etmiyor. "Google authenticator" uygulaması bu durumu şu şekilde çözüyor. "Google authenticator" kendi içinde belli zaman aralıkları belirlemiş. Her 30 saniye bir zaman aralığı içinde, yani 00:00:00 - 00:00:30 ilk zaman aralığı.  00:00:30 - 00:00:60 ikinci zaman aralığı. Eğer 00:00:35 anında bir key üretirsek; bizi ikinci zaman aralığına otomatik alıyor ve 00:00:30 anında şifreyi üretmişiz gibi varsayıyor ve buna göre hesaplamaları yapıyor. Dolayısı ile her 30 saniye için en fazla 1 farklı key üretebiliyoruz. Bu tamamen "Google authenticator" yazılımının önyüzde uyguladığı bir mantıktır. anahtar üretim algoritması ile bağlantılı bir durum değildir.

Aynı durum <https://github.com/bilelmoussaoui/Authenticator> için geçerli değildir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Post-quantum cryptography (⟷ quantum-proof ⟷ quantum-safe ⟷ quantum-resistant)

`Kuantum` hesaplamaları fizik kuralları gereği her türlü hesaplamayı verimli şekilde yapamamaktadır. Bu sebeple `kuantum` teknolojisinin hızlı hesaplamaya tabi tutamayacağı algoritmalar ortaya atılmıştır.

2019 yılı itibari ile çoğu asimetrik şifreleme (`private key` ve `public key` yapısına dayanan şifreleme) algoritmaları `kuantum` bilgisayarlarına karşı dirençsiz. `Simetrik` algoritmalar ise dirençli. Fakat simetrik algoritmalar'ın anahtarları yarıya düşürülmüş kadar daha hızlı çözülebiliyor. örneğin 512'lik bir anahtara sahip bir şifreleme algoritması, eğer `kuantum` bilgisayarı ile kırılırsa, quantum olmayan bir bilgisayarda 256'lıkmış hızında kırılabiliyor.

2019 yılı itibari ile özgür lisanslı ve ticari anlamda tümüyle kullanılabilecek quantum resistant algoritması yoktur. Olanlar ya lisanslı, yada key'leri büyük olması gibi önemli dezavantajları mevcut.

## 📌 quantum algorithms

kuantum bilgisayarların verimli çözdüğü algoritmalardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 HTTP Public Key Pinning (⟷ HPKP)

`Ssl` işlemlerine ek güvenlik yapmak için kullanılan bir yöntemdir. Sunucu cevaplarının header'ına şunu ekler:

```text
Public-Key-Pins:
         pin-sha256="cUPcTAZWKaASuYWhhneDttWpY3oBAkE3h2+soZS7sWs=";

         pin-sha256="M8HztCzM3elUxkcjR2S5P4hhyBNf6lHkmjAHKhpGPWE=";

         max-age=5184000
```

Tarayıcı bu değerleri tüm yaşamı boyunca (en fazla `max-age` e göre) saklar. `Pin-SHA` değeri sunucunun `public key` `SHA`'dır. Eğer ileride bir hacker sunucunun döndürdüğü `public key`'i değiştirirse; tarayıcı (sunucuya hiç gitmeden) eski `SHA`'ları da kontrol edeceği için bir sorun olduğunu anlayacak ve kullanıcıyı uyaracaktır.

2 tane pin olmasının sebebi max-age süresince sunucunun gerçekten de `public key`'ini değiştirmesi durumunda kullanılacak backup'tır. Sunucu dilediği kadar `pin-SHA` yollayabilir.

Bir siteye bir tarayıcıdan ilk defa gidiliyorsa, bu güvenlik önlemi bir işe yaramaz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 browser fingerprint

bu bilgilerin kombinasyonları ile yine browser'ın tekilliğini tespit edebilmektedirler.

- browser'ın destekleyip desteklemediği her feature
- hangi fontların yüklü olduğu listesi
- user agent
- OS tipi
- CPU tipi
- timezone
- screen resolution
- dil

gibi daha bir çok parametre ile tarayıcının tekilliği ortaya koyulabilmektedir.

hatta bazı özellikler tek başlarına %100 unique'liği sağlayabilmekte, bazı özellikler yine tek başlarına %99 oranında unique'liği sağlayabilmektedirler.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Biyometri (⟷ biometry)

yaşayan organizmaların ölçümlerine verilen genel isimdir. tüm niteliklerin sayısal ortama aktarılmış halidir.

## 📌 biyometrik kimlik doğrulama yöntemleri (⟷ biometric verification methods)

- parmak izi (⟷ fingerprint) (parmaklara gözle detaylı bakıldığında bu şekiller görülebilir)
- yüz tanıma (⟷ face recognition)
- damar izi (⟷ vein scar)
  - parmak damar izi (⟷ finger vein print)
  - avuç içi damar izi (⟷ palm vein print)
- göz tarama
  - retina taraması (⟷ retinal scan)
  - iris tanıma (⟷ iris recognition)
- ses tanıma (⟷ voice recognition)
- (el yazılı) imza

yukarıdaki biyometrik bilgiler genişleyebilir/büyüyebilir fakat şekilleri değişmez. bu sebeple ömür boyu aynı kalırlar.

engelli kişiler, bazı biyometrik bilgileri veremediği unutulmamalıdır.

her veri, teorik olarak replika edilebilir. fakat başka başlıkta anlatıldığı gibi; authentication amaçlı yukarıdaki her biyometrik veri kullanılabilir. çünkü authentication yapılırken, %100 aynı olan replica'lar kabul edilememektedir.

hatalar genelde şunlardan kaynaklanmaktadır:

- yazılımsal/algoritmik hatalar
- kontrol edilen biyometrik cihazların ucuz olmasından kaynaklı hassas data toplanamaması (örneğin; kameranın kalitesiz olması durumunda, görüntü detaylı pixel'lere sahip değildir. bu da hatalara sebebiyet verebilir. çünkü hassas ölçümler yapılamaz.)

Tüm biyometrik sistemler aşağıda açıklanmış olan beş özelliğe sahip olmalıdır:

- Evrensellik: Tüm bireyler biyometrik özelliklere sahip olmalıdır.
- Eşsiz olma: Biyometrik karakteristiğin her insanda farklı bir şekilde yer alması.
- Süreklilik: Karakteristiğin zamanla değişmemesi.
- Elde edilebilirlik: Biyometrik özelliklerin bazı pratik cihazlarla ölçülebilir olması.
- Kabul edilebilirlik: Bireylerin biyometriğin ölçüm ve toplanmasında itirazları olmamalı

| Biyometrik Karakteristik | Evrensellik | Eşsizlik | Süreklilik | Elde Edilebilirlik | Performans | kabul Edilebilirlik | Yaygınlık |
|--------------------------|-------------|----------|------------|--------------------|------------|---------------------|-----------|
| DNA                      | Y           | Y        | Y          | D                  | Y          | D                   | D         |
| Kulak                    | O           | O        | Y          | O                  | O          | Y                   | O         |
| Yüz                      | Y           | D        | O          | Y                  | D          | Y                   | Y         |
| Yüz Termogramı           | Y           | Y        | D          | Y                  | O          | Y                   | D         |
| Parmak İzi               | O           | Y        | Y          | O                  | Y          | O                   | O         |
| El Geometrisi            | O           | O        | O          | Y                  | O          | O                   | O         |
| İris                     | Y           | Y        | Y          | O                  | Y          | D                   | D         |
| Retina                   | Y           | Y        | O          | D                  | Y          | D                   | D         |
| İmza                     | D           | D        | D          | Y                  | D          | Y                   | Y         |
| Ses                      | O           | D        | D          | O                  | D          | Y                   | Y         |

Y:Yüksek O:Orta D:Düşük

## 📌 parmak izi (⟷ fingerprint)

parmak izi en sık kullanılan, en basit ve ucuz yöntemlerle tespit edilen ve karşılaştırılabilen metotlardandır. bu sebeple sık kullanılır.

parmak izinin %100 benzersiz olduğuna dair bir tez yoktur. fakat şu ana dek aynı parmak izi olduğu görülmemiştir. bu sebeple herkes eşsiz olarak varsayar. bazı kurumlar, bu sebepten ötürü sadece 1 parmağın değil, garantilemek amacı ile birden fazla parmak izini birden saklamaktadır. Bunun ayrı bir sebebi de, kötü niyetli kişinin, fiziksel olarak başkasının parmağını kullanabilme ihtimalidir. Bu durumda kötü niyetli kişinin birden fazla parmağı ele geçirmiş olması gerekecektir.

parmak izi benzerliğini kontrol etmek için kullanılan birçok algoritma mevcut.

alınan parmak izinin ne kadar detaylı alındığı da önemlidir. çok kalitesiz alınan parmak izi kaydı, yanlış sonuçlara yol açabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Pentest (⟷ penetration test)

Penetration Türkçe anlamı nüfuz etmek, delmek, içine girmektir.

sisteme sızma testleridir. bu testler ile güvenlik açıkları tespit edilir.

## 📌 Whitebox vs Blackbox vs Greybox

Pentest tipleridir. Pentest'i yapacak ekibe sistem hakkında:

- hiçbir bilgi verilmiyor ise Blackbox testidir.
- tüm bilgiler (kodlar, network) veriliyor ise Whitebox testidir.
- kısmen/bazı bilgileri veriliyor ise Greybox testidir.

## 📌 Blackbox

kelime anlamı: mat (parlak olmayan, ışık geçirmeyen).

Blackbox bilişim dünyasında sadece input ve output'ları gözlenebilen fakat içi görünmeyen her şeye denir.

## 📌 Vulnerability Assessment (⟷ zafiyet tarama)

direk güvenlik açığı bulmaz. fakat güvenliği tetikleyici etkenleri tespit eder.

örneğin; Java sürümü eski ise uyarır.

## 📌 Alpha Testing Vs Beta Testing

JUnit integration ve sistem testleri koşulduktan sonra yapılan testlerdir. buradan sonra yapılan testler "user acceptance test" olduğu için alpha ve beta da user acceptance testi olarak geçer.

alpha testleri genelde bu işin uzmanı kişilerin yaptığı testlerdir. beta testleri ise public fakat sadece belli kişilerin test etmesi sürecidir.

## 📌 functional requirement

- yazılım/sistem ihtiyaç analizleri için kullanılan terimdir.
- spesifik bir fonksiyonalite/özellik için yazılmış olması gereken ihtiyaca verilen sıfattır. örnek:
  - son kullanıcı formu yolla tuşuna ancak şifresini girdikten sonra başarabilecektir.
  - ikinci sayfadan sonra direk dördüncü sayfaya geçiş mümkün olmalıdır.

## 📌 Non-functional requirement

bu ihtiyaçlar spesifik özellikler için belirtilmez. genel sisteme hitap eder. örnek:

- sistem 10.000 kişiyi destekleyecektir.
- yazılım güncel Linux sistemlerini destekleyecektir.

Bazı kaynaklarda __cross-functional requirement__ olarak geçmektedir. Kaynak: "Sam Newman" "Building microservices" kitabının 151'inci sayfasında "cross-functional testing" başlığı, 2inci paragraf.

## 📌 Functional Testing vs Non-Functional Testing

fonksiyonel test sadece belirli bir özelliğe istinaden koşulan testlerdir. oysa Non-Functional performans gibi kriterleri ölçmek için yapılan testlerdir.

## 📌 regresyon testleri

yazılıma eklenen yeni bir özelliğin, sistemde zaten var olan diğer özellikleri etkileyip etkilemediğinin testleridir. bu durumda aslında tüm sistem baştan sona test edilmesi gerekir fakat bu yüksek maliyet olacaktır. bu sebeple; sadece etkisi olabilecek potansiyel modüller test edilmelidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 BackTrack

pentest yapabilmek için gerekli tool'ların bir arada sunan, Linux tabanlı, live CD/USB ile çalışan OS'tur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 IPSec

Internet protocol katmanında gizlilik ve doğruluk sağlamaya yönelik güvenlik standartları grubudur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Rootkit

sistemin admin kullanıcı haklarını ele geçirip işlem yapmaya çalışan malware'dir. genelde algılanmaları zordur, çünkü zaten en yüksek seviyede yetkiye sahiptirler ve bu sebeple kendilerini gizlemeleri kolaydır.

## 📌 IP spoofing (⟷ IP sahteciliği)

IP adresini farklı gösterip request gönderme işlemdir. Tabi cevap bizim isteği yolladığımız makinaya değil, IP sahibi makinaya gidecektir. Fakat IP sahteciliğinde dönüş cevabı önemsenmez. Yapılmasının amaçları farklıdır. Örneğin; sunucunun bant kaynaklarını tüketmek.

## 📌 DoS attack (⟷ Denial of Service attack)

sistemin kaynaklarını (örneğin bant genişliği, CPU) tüketip, hizmetin kullanıcılara ulaşmasını engellemeye çalışan saldırı tipidir.

Denial-of-service terimi "servis reddi" anlamına gelir. yani; servis kullanıcılarını reddetmektedir (zorunda kalır).

burada bilgi hırsızlığı söz konusu değildir.

## 📌 DDoS attack (⟷ Distributed DoS attack)

birçok makineden paralel DoS atağı yapma saldırısıdır. DDoS saldırıları bazen Botnet ağından, target'a yapılmaktadır.

## 📌 Hijack attack

Hijack kelime anlamı: hırsızlık.

## 📌 Malware (⟷ malicious software)

category of all dangerous softwares.

## 📌 Spyware

kullanıcıdan habersiz arka planda bilgi toplamak amaçlı yazılmış zararlı uygulamadır. örnek: keylogger.

## 📌 Phishing

Password'ün ilk karakteri ve fishing'in (balık avlama, oltalama) birleşiminden oluşan bir terimdir.

şifre gibi bilgileri çalmak için, gerçek bir kurumdan geliyormuş gibi e-posta yada benzeri sayfalar ile hazırlanan tuzaklara verilen isimdir.

## 📌 Social engineering

Sosyal mühendislik, internette insanların zafiyetlerinden faydalanarak çeşitli ikna ve kandırma yöntemleriyle istenilen bilgileri elde etmeye çalışmaktır.

## 📌 Adware

reklam gösteren uygulamalardır.

## 📌 Ransomware

Ransom Türkçe kelime anlamı: fidye

fidye isteyen yazılımlar. genelde sistemde belli şeyleri şifrelerler, para karşılığında karşılığında bilgisayarı tekrar virüsten arındıracağını iddia eder.

## 📌 Scareware

Scare kelime anlamı: korkmak

farklı bir yazılım yükleme vaadiyle (gerçekten o yazılım yüklenir) kurulup, arka planda kötü amaçlı farklı yazılımlar çalıştıran uygulamalardır.

## 📌 Botnet (⟷ zombi ağı)

her bilgisayara Botnet dediğimiz bu zararlı yazılım yüklenir. bu zararlı yazılım, kurulduğu makineye uzaktan erişim için güvenlik açığı sağlar. bu şekilde tek bir noktadan tüm uzak bilgisayarlar yönetilebilir olur. bu kurulan network ağına "zombi ağı" denir. her bilgisayara ise "zombi (⟷ zombie)" denir.

## 📌 backdoor

bilgisayarın uzaktan yönetilebilmesi için açılan güvenlik açığına verilen isimdir.

## 📌 Trojen (⟷ Truva atı)

Scareware ile farkı genel olarak yoktur.

## 📌 virüs

otomatik yada kullanıcı tıklamalarıyla farklı makinelere yayılma özelliği olan malware'lerdir.

## 📌 Worm (⟷ solucan)

başka makinelere otomatik yayılır.

## 📌 Brute-force attack (⟷ kaba kuvvet atağı)

Sadece deneme yanılma ile sürekli olarak farklı girdilerle doğru çıktıları elde etmeye çalışma işlemidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 TLS (⟷ Transport Layer Security) vs SSL (⟷ Secure Sockets Layer)

Aynı protocol'ün farklı sürümleridir ama isim değişikliği yaşamıştır.

## 📌 SSL version history

| version     | publish date |
|-------------|--------------|
| `SSL` `1.0` | Unpublished  |
| `SSL` `2.0` | 1995         |
| `SSL` `3.0` | 1996         |
| `TLS` `1.0` | 1999         |
| `TLS` `1.1` | 2006         |
| `TLS` `1.2` | 2008         |
| `TLS` `1.3` | 2018         |

Artık `TLS` `1.1` ve öncesinin resmi olarak güvensizdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 mTLS (⟷ Mutual TLS)

`TLS` sadece server tarafın sertifikasını onaylanır. Oysa `mTLS` hem server hemde client'ın sertifikasını onaylanır.

`mTLS` bir protocol değil. Genel bir pattern'dir. Yani nasıl yapılacağı ile ilgili bilgi içermez.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 sunucunun sertifika detaylarını görme

Birçok farklı yöntem kullanılabilir:

- https://www.ssllabs.com/ssltest/index.html gibi birçok site girdiğimiz domain name adresine istek yaparak detayları web tarayıcıda bize raporluyor. hangi client'ların (web tarayıcı, java) sürümleri ile bu sertifikada sorun çıkarıp çıkarmayacağını da raporluyor.

- https://badssl.com/ birçok farklı sertifika içeren sayfaya linkler içeriyor. bu şekilde web tarayıcımızın her sertifika için nasıl uyarı verdiğini veya diğer davranışlarını inceleyebiliyoruz.

- komut satırı uygulamaları ile sertifika ve handshake işlemlerinin detaylarını print edebiliriz:
  - openssl s_client -showcerts -connect ma.ttias.be:443
  - curl -vvI <https://gnupg.org>

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 HTTP'den HTTPS'e yönlendirme

HTTP ile gidilen site, dönüş değerinde HTTPS'e yönlendirme isteğinde bulunur. web tarayıcıda kullanıcıyı HTTPS'e yönlendirir.

eğer arada trafiğimizi izleyip manipüle eden birileri varsa, bu isteğin cevabında tarayıcı HTTPS'e yönlendirme yapmaz. yapmaz ise, normal HTTP sitenin get isteğinin cevabı ile sayfada gezinilir. artık son kullanıcı HTTP ile iletişime devam edeceği için tüm iletişim baştan sona izlenebilir.

## 📌 "HTTPS everywhere" vs "smart https"

Web tarayıcı eklentileridir. Smart; her URL'yi sorgulamadan HTTPS'e çevirir. Eğer herhangi bir cevap gelirse siteye gider, yoksa HTTP olana gider. Fakat "HTTPS everywhere" her site için önceden rule set tutar ve aşağıdaki gibi gelişmiş yönlendirmelerde yapabilir.

```text
http://fr.wikipedia.org/wiki/Chose

redirect to --->>

https://secure.wikimedia.org/wikipedia/fr/wiki/Chose
```

## 📌 HSTS (⟷ HTTP Strict Transport Security)

Kullanıcıyı HTTP'den HTTPS'e yönlendirildiğinde sunucu bu header'ı döner:

> Strict-Transport-Security: max-age=16070400; includeSubDomains

max-age süresinde tarayıcı son kullanıcı HTTP'ye gitmek istese bile HTTPS'e otomatik yönlendirecektir.

## 📌 Preloaded HSTS sites

HSTS için bir kere olsun siteye gitmek gerekiyor. bunu da yapmaya gerek kalmaması için tüm web tarayıcıların ortak tuttuğu bir liste var:

<https://cs.chromium.org/chromium/src/net/http/transport_security_state_static.json>

dosya içeriği çok büyük. bir kısmı:

```json
{ "name": "qualitymudjacking.com", "policy": "bulk-1-year", "mode": "force-https", "include_subdomains": true },
{ "name": "qualitypiering.com", "policy": "bulk-1-year", "mode": "force-https", "include_subdomains": true },
{ "name": "qualitypolyjacking.com", "policy": "bulk-1-year", "mode": "force-https", "include_subdomains": true },
{ "name": "qualitywaterproofingco.com", "policy": "bulk-1-year", "mode": "force-https", "include_subdomains": true },
{ "name": "quanquan.space", "policy": "bulk-1-year", "mode": "force-https", "include_subdomains": true },
{ "name": "qulixqa.com", "policy": "bulk-1-year", "mode": "force-https", "include_subdomains": true },
```

Bu listeye kaydolmak ücretsiz. bu bilgileri web tarayıcıları sürekli lokalde güncel tutuyor ve bu listedeki sitelere gitmek isteyen kullanıcıları direk HTTPS'e yönlendiriyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Diffie-Hellman key exchange

known also as:

- `Diffie–Hellman` key agreement
- `Diffie–Hellman` key establishment
- `Diffie–Hellman` key negotiation
- Exponential key exchange
- `Diffie–Hellman` protocol
- `Diffie–Hellman` handshake
- `Diffie–Hellman``-Merkle` `*` (all combinations above)

`Whitfield Diffie` and `Martin Hellman` invented this algorithm.

`Ralph Merkle` is one of the inventor of asymmetric cryptography. `Diffie` and `Hellman` are the inventors of the earliest practical examples of `public key` exchange implemented within the field of cryptography. Therefore `Diffie–Hellman` known as `Diffie–Hellman-Merkle`.

We use key exchange to generate a key (using a not secure connection) which is known only by communicator but not by middle attackers.

Let assume that `Alice` and `Bob` will communicate. They are in key exchange phase. There are many different alternatives for this phase. On this topic we will explain the Diffie-Hellman key exchange algorithm.

"`Eve`" is the name of the middle listener (hacker). `Not`: `Eve` ismi `İngilizce`'de kulak misafiri anlamına gelen `eavesdropper`'dan gelir.

`g` and `p` are generated randomly and publicly by `alice` and `bob`.

- `g = public (prime) base`, known to `Alice`, `Bob`, and `Eve`. `g = 5`
- `p = public (prime) modulus`, known to `Alice`, `Bob`, and `Eve`. `p = 23`
- `a = Alice's private key`, known only to `Alice`. `a = 6`
- `b = Bob's private key`, known only to `Bob`. `b = 15`
- `A = Alice's public key`, known to `Alice`, `Bob`, and `Eve`. `A = g^a mod p = 8`
- `B = Bob's public key`, known to `Alice`, `Bob`, and `Eve`. `B = g^b mod p = 19`

`mod` is the `modulo` operation in math.

| `Alice` Known       | `Alice` unknown | `Bob` Known          | `Bob` Unknown | `Eve`  Known  | `Eve` Unknown |
|---------------------|-----------------|----------------------|---------------|---------------|---------------|
| p = 23              |                 | p = 23               |               | p = 23        |               |
| g = 5               |                 | g = 5                |               | g = 5         |               |
| a = 6               | b               | b = 15               | a             |               | a, b          |
| A = 5^a mod 23      |                 | B = 5^b mod 23       |               |               |               |
| A = 5^6 mod 23 = 8  |                 | B = 5^15 mod 23 = 19 |               |               |               |
| B = 19              |                 | A = 8                |               | A = 8, B = 19 |               |
| s = B^a mod 23      |                 | s = A^b mod 23       |               |               |               |
| s = 19^6 mod 23 = 2 |                 | s = 8^15 mod 23 = 2  |               |               | s             |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 ileri güvenlik (⟷ kusursuz ileri güvenlik ⟷ forward secrecy ⟷ FS ⟷ perfect forward secrecy ⟷ PFS)

her oturum açıldığında veya aynı oturumun farklı zaman aralıklarında farklı anahtar ile şifreleme yapılma tekniğidir. bu şekilde, ilgili oturumun anahtarı çalınsa bile, geriye dönük data'lar (diğer oturumların data'ları) açılamayacaktır.

Burada şunlara dikkat edilmelidir:

- her oluşturulan yeni oturum anahtarının, bir önceki ile ilişkisel olmaması gerekir.
- anahtar oluşturma sıklığı ne kadar fazla ise riski bi o kadar düşürmüş oluruz.
- her oluşturulan yeni oturum anahtarının, client ve server arasında paylaşılırken, aradaki kötü niyetli kişinin iletişi açık şekilde okuyabilse bile, bu anahtarı bilememesi gerekir (bunun için "Diffie–Hellman key exchange" matematiksel algoritmasına bakılabilir).

"Diffie–Hellman key exchange" işlemi sonucunda hacker'ın bilmediği bir anahtar hem client hem de sunucu tarafta oluşturulmaktadır. Bu key her oturumda farklı kullanılarak "ileri güvenlik" sağlanmış olur.

Yukarıdaki duruma web tarayıcılarından örnek verelim: web tarayıcı belli kurallara göre (belli aralıklarda), "Diffie–Hellman key exchange" kullanarak oturumu her defasında şifrelemektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Open Web Application Security Project (⟷ OWASP)

OWASP is an online community which creates freely-available articles, methodologies, documentation, tools, and technologies in the field of web application security.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Bot

Türkçe ismi de bot'tur. Bazı kaynaklarda 'robot' olarak da kullanılır.

Otomatik işlem yapan yazılımlardır. örneğin; oyunlardaki botlar benzerdir fakat tam olarak aynı şey değillerdir.

## 📌 Internet bot

İnternet üzerinde otomatik işlem yapan yazılımlardır.

## 📌 Web crawler (⟷ web spider)

Bir çeşit internet botudur. Sadece web sayfalarını dolaşan yazılımlardır. Bu dolaşma işlemi, sitelerin eski hallerini arşivlemek, siteleri arama motorları için index'lemek, bir siteyi tüm alt sayfaları ile download etmek (bazı download manager'lar yapıyor) gibi birçok amaç için olabilir.

## 📌 Web scraper (⟷ web harvesting ⟷ web data extraction ⟷ Web kazıma ⟷ web hasat ⟷ web veri çekimi)

HTML üzerinde verileri parse edip uygun verileri ayıklamak için gerekli yazılımdır. web crawler siteleri gezerken siteleri arka planda indirirler. ardından, web scraper sitelerin içindeki bilgileri, DB'lere kaydeder.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 string vs char[] for passwords (on java)

şifreler programda string değil char[] tutulması önerilir. bunun bazı sebebleri:

- program exception olup kapanırsa OS variable'ların dump'ını bir yere atar. daha sonra birileri bunu okursa, string değeri okunabilirken, char[]'in değeri okunamaz. çünkü char[]'in toString()'i yoktur.

- char[]'in toString()'i olmadığından; biri yanlışlıkla log atarsa log("" + password); ekranda şifre görülmeyecektir. birinin direk şifreyi ekrana basma ihtimali az, fakat obje içinde obje olduğunda bazen taşınma işlemlerinde yanlışlıkla log basılmış olabilir.

- String'ler immutable olduklarından referansları olmasa bile garbage collector temizleyene kadar RAM'de tutulurlar. eğer uygulama hack'lenirse RAM'den okuma yapacak olan hacker bu değerleri görebilir. string ve char[], RAM'den aynı şekilde okunur, fakat bu maddede bahsedilen, kullanıma ihtiyaç duyulmadığında, string'in memory'den silinmesinin daha uzun zaman alabilmesidir.  http://docs.oracle.com/javase/6/docs/technotes/guides/security/crypto/CryptoSpec.html#PBEEx

Ek not:

- javax.swing kütüphanesinin JPasswordField sınıfı getText() metodu artık char[] dönüyor (eskiden string dönüyordu). bu sebepten böyle düzeltildi.

Ek not:

- `.Net` ortamında `System.Security` `namespace`'si altında `SecureString` isimli bir sınıf vardır. Bu sınıf aynı bir string gibi davranmakta fakat işimiz bittiğinde bu sınıfı "Dispose()" (kelime anlamı: elden çıkarmak) metodu ile yok edebiliyoruz. Aynı zamanda, burada string memory'de şifrelenmiş olarak saklanmaktadır. şifreleme görevini `.Net` ortamı'ı yapmaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Luna

Thales (daha eskiden SafeNet) firması tarafından geliştirilen, HSM (Hardware security Module) donanım ailesidir.

Yazılımsal API'si aracılığı ile network üzerinden aldığı bilgileri şifreleyip açabilmektedir.

Farklı sürümleri ve farklı donanımlar satarlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Content Security Policy (⟷ CSP)

Web sayfası `HTML`'inde hangi origin'lerden ne tip kaynaklar (image, JS) indirilebileceğini tanımlıyoruz. Bu tanımlarımız dışında istek yollandığında tarayıcı engel koyuyor.

```html
<meta http-equiv="Content-Security-Policy"
      content="default-src 'self'; img-src https://*; child-src 'none';">
```

Aynı şekilde Content-Security-Policy header'ı web sayfasının yüklendiği request'te dönebilir. yani HTML'de olmak zorunda değildir.

Not: eğer HTTP response'unda dönecek ise, tüm response'larda dönmek zorundadır. sadece index-html'in olduğu request'ten bu header'ların dönmesi yetmez.

### 📌📌 Policy'lere örnekler

sub domain'ler olmayacak şekilde tüm script'ler sadece şu andaki origin'e yapılabilecektir:

```text
Content-Security-Policy: default-src 'self'
```

Sadece kendine (subdomain'lere istek atamaz) ve trusted.com'un ve onun subdomain'lerinden yükleme yapılabilir.

```text
Content-Security-Policy: default-src 'self' trusted.com *.trusted.com
```

A web site administrator wants to allow users of a web application to include images from any origin in their own content, but to restrict audio or video media to trusted providers, and all scripts only to a specific server that hosts trusted code:

```text
Content-Security-Policy: default-src 'self'; img-src *; media-src media1.com media2.com; script-src userscripts.example.com
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 red team (⟷ kırmızı takım)

bir şirkete bağlı şirketin açıklarını bulmaya çalışan ekibe verilen genel isimdir. bu açıklar sadece yazılımsal olmayabilir, şirket çalışanlarına email üzerinden sosyal mühendislik saldırıları da yaparlar.

etik hacker'lar bu kategoridedir.

## 📌 blue team (⟷ mavi takım)

red team'in tersi olarak şirketi korumaya çalışan ekiptir. audit/monitoring sistemleri kurup, takibi ve yönetimini sağlarlar.

## 📌 purple team (⟷ mor takım)

red ve blue takımın birlikte çalışma kültürü için kullanılan bir terimdir. devops teriminde olduğu gibi bir durum söz konusu.

## 📌 green team (⟷ yeşil takım)

blue team'in bulduğu açıkları kapatmak için yazılım geliştirirler.

## 📌 white team (⟷ beyaz takım)

kırmızı ve mavi takım kapışmalarında hakemlik yapar. sonuçları değerlendirir, kapışmalardaki kuralları belirler...

## 📌 black hat (⟷ black hat hacker ⟷ blackhat)

kötü niyetli (illegal) hacker.

## 📌 grey hat (⟷ greyhat ⟷ gray hat)

illegal olmayabilirler fakat, illegal iş yaptıklarında çok büyük suç işlemezler.

şu gibi kişiler için bu terim kullanılır:

- keşfettiklerinde ürünün sahibine bunun nasıl çalıştığını söylemek yerine, onu küçük bir ücret karşılığında onarmayı teklif edebilirler.
- firmalara ders vermek amacı ile, yazılımlardaki açıkları direk public olarak yayarlar.

## 📌 white hat (⟷ a white-hat hacker ⟷ whitehat)

ethical hacker.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Security through obscurity (⟷ security by obscurity)

obscurity kelime anlamı: belirsizlik, bilinmezlik.

güvenliği sağlarken, güvenlik mekanizmasının nasıl çalıştığını fark ettirmemeye çalışılmasıdır. örnekler:

- JS kodlarını önyüze dönerken, kodlardaki isimlendirmeleri karmaşıklaştırmak.
- DB tablolarımızdaki sütun isimlerini gerçek anlamlarından farklı yaparak, hacker'ları şaşırtmaya çalışmak.

bunları yapmak fayda sağlayabilir fakat bu temelde buna dayanarak güvenlik kesinlikle sağlanmamalıdır. güvenlik öyle olmalıdır ki; metodolojisi bilinse bile, anahtarlarımız çalınmadığı takdirde kırılamamalıdır (Kerckhoffs's principle).

## 📌 Kerckhoffs's principle (⟷ Kerckhoffs ilkesi)

"principle" in title can be referred also as: desideratum, assumption, axiom, doctrine, law

başlıktaki "ilke" bunlarla da kullanılmaktadır: sanı, aksiyom, yasa

Kerckhoffs ilkesi şunu der: şifreleme kullanan bir sistem, anahtar hariç, sistemle ilgili her şey bilinse bile güvenli olmalıdır.

## 📌 Secure by design

güvenlik açıklarına tek tek spesifik kapatmak yerine, kökten (mimarisel) çözüm bulan mimarilere "secure by design" denir. yani dizaynın kendisi zaten güvenlidir. örneğin;

- web tarayıcısı hack'lenebilir. bunun için birçok güvenlik yöntemi ekleyebiliriz. fakat eğer tarayıcının kendisi, Android üzerinde hiçbir yetki kullanmadan çalışır ise; tarayıcı hack'lense bile, hacker dış kaynaklara erişemeyecektir.
- sanal paraları güvenli kılan özelliklerin sistemin kendisi baştan iyi tasarlanmış olmasıdır. sanal para sahipleri kendi paralarının değerinin kaybolmaması için diğer sanal para sunucuları (node'ları) ile uyumlu çalışma durumundadırlar.
- ORM kullanınca zaten SQL injection'ı komple engellemiş oluruz. Tek tek her type (number, string) için SQL koruması/validasyonu yazmayız.
- CQRS yapınca Read-only servislerimizde doğal yoldan koruma sağlamış oluruz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 security vs privacy vs anonymity

- Those are 3 different terms.
- Security means that the system protects you from hacks.
- Privacy means that the system protects your personal data. Every private system has to be secure, but not visa versa. For example WhatsApp use the same protocol as Signal messenger. The protocol is also called "Signal". Signal protocol is secure, therefore both WhatsApp and Signal are secure too. But Signal messenger is private, but WhatsApp is not. Because (we assume) WhatsApp can (may) store your `private key`s (which are using on Signal protocol) in their servers and can give other government authorities in some legal cases. Also WhatsApp can (may) sell your messages with your identity to advertisement companies or any other 3rd parties (because WhatsApp is a private company).
- Anonymity means that the system protects your personal identity. In order to do that it should be secure and private.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Zero trust architecture (⟷ ZTA ⟷ perimeterless security)

`Perimeter` kelime anlamı: Sınır, çevre hattı.

internal network'te olsa bile, hiçbir cihaza veya kullanıcıya güvenmeyip doğrulama yapan mimariler için kullanılan terimdir.

örnekler:

- API gateway gelen `JWT`'yi açıp, internal'da açık yollamamalı. Her microservice kendi `JWT`'yi açmalı.
- internal'da her microservice `TLS` ile haberleşmeli.
- `mTLS`

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Open-source intelligence (⟷ OSINT)

Public olarak yayımlanan data'lardan istihbarat elde etme yöntemidir.

TV, radyo, gazete, sosyal medya gibi kaynaklardan yararlanılır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
