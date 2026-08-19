# SECURITY OAUTH

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 authentication (kimlik doğrulama)

`otantik (⟷ authentic)` terimi, `orjinal` anlamına gelir. `authentication` buradan türemiştir. `authentication`; bir şeyin `otantik` olup olmadığını onaylamak için kullanılan fiildir.

`authentication` sadece kullanıcıyı doğrulama mekanizmasıdır. bu kişinin yetkileri kontrolünü kesinlikle kapsamaz.

## 📌 authorization (⟷ yetkilendirme)

Sadece ilgili kullanıcının nelere yetkili olup olmadığı yönetimidir.

## 📌 authentication vs authorization

`authentication`; __authN__, `authorization`; __authZ__ olarak kısaltılır.

Yetki kontrolü yapan sistemde, authentication olmak zorunda değildir. Bunun en bilinen örneği Oauth'dır.

bazı protocol'ler hem authentication hem de authorization işlevini kapsar. örnek "OpenID Connect".

## 📌 OpenID 1.0 ve 2.0

- authentication protocol'üdür.

- açık kaynaklıdır.

- örneğin Twitter gibi bir siteyi provider(kimlik doğrulayıcı) olarak kullanıp, başka yere login olmak isterseniz, login olacağınız sitenin, Twitter'dan anahtar(onay) almış olması şart değildir.

- herkes birer OpenID provider'ı olabilir.

- OpenID provider ile login olunmaya çalışılan site arasındaki haberleşme protocol'üdür.

süreç:

- kullanıcı, login olanacağı web siteye girer (muhtemelen "login with Twitter" gibi bir button'a tıklar)

- web sitesi OpenID-provider'a (örnek: Twitter'a) yönlendirme yapar

- kullanıcı Twitter'a login olur (nasıl login olacağına OpenID hiç karışmaz)

- kullanıcı başarılı yada başarısız olursa login olmaya çalıştığı web sitesine yönlendirilir.

- Twitter, web sitesine yönlendirme yaparken (success URL'ye yönlendirirken), web sitesine kullanıcı için contact bilgilerini de gönderir. burada bir çok farklı bilgi olabilir.

## 📌 Oauth 1.0, 1.1, 2.0

- authorization protocol'üdür. `HTTP` üzerinden çalışır.

- açık kaynaklıdır.

- örneğin `Twitter` gibi bir siteyi provider olarak kullanıp, başka yere login olmak isterseniz, login olacağınız sitenin, `Twitter`'dan anahtar (onay) almış olması şarttır.

- `2.0`, geriye uyumlu değildir.

- `OpenID`'dekine benzer bir süreç vardır. fakat web sitesi Twitter'dan contact bilgilerini almaz. aldığı token ile birlikte sürekli olarak arka planda Twitter'ın kaynaklarına user adına erişip işlem yapabilir. bu sebeple; `Oauth` bir authorization protocol'üdür.

## 📌 OpenID Connect (⟷ OIDC)

- `OpenID` `2.0`'dan sonra çıkan sürümü. isim değişikliği sonrası `1.0` sürümünden devam edilmektedir.

- `OpenID Connect`, `Oauth` `2.0` üzerine kurulu bir yapıdır.

- `OpenID Connect` ile `Oauth` `2.0`'a authentication özelliği de getirmiştir.

## 📌 scope

bu kavram tüm authorization mekanizmalarında var.

scope'lar, client uygulamasının rolleri olarak düşünülmelidir. login olan son kullanıcının

- Google'daki role'ü, Google Drive dosyalarına erişmek olabilir,
- fakat sadece "email" scope'una sahip bir uygulama sadece gmail'e erişebilir.

### 📌📌 predefined scopes

- "OpenID Connect 1.0" sürümünde, scope'larda "OpenID" olmak zorundadır.

- "OpenID Connect 1.0" sürümünde, scope'larda istenirse "offline_access" olabilir. "offline_access" scope'u varsa, son kullanıcı web tarayıcısını/uygulamasını kapatsa dahi, arka planda access token ile user-info bilgilerine erişebileceği anlamına gelir.

  Tam olarak hangi bilgilerin user-info olduğu standartlarda yazmamaktadır. fakat önceden ayrılmış field'lar vardır. kaynak: https://openid.net/specs/openid-connect-core-1_0.html title: "5.1. Standard Claims".

## 📌 Oauth 2.0

### 📌📌 roller

#### 📌📌📌 Resource owner

kullanıcının kendisi. end-user.

#### 📌📌📌 Resource server

API. user'a ait data barındırır.

API'ye gelen isteklerde, user'ın data'sını paylaşıp paylaşmayacağını anlamak için Oauth 2.0'den yararlanır.

bu sunucuya client istek yapacaktır.

#### 📌📌📌 Authorization server

bu kısım istenirse "Resource server" ile aynı uygulama içerisinde de olabilir.

#### 📌📌📌 Client

third-party app.

yukarıdaki rollere bazı örnekler:

- banka uygulaması

  __auth_server__: user-microservice
  
  __Resource server__: diğer tüm microservice'ler
  
  __resource__: card payment history.

  __client__: mobil, ATM.

- Login to "amazon.com" via "Facebook.com"

  __auth_server__: Facebook.com

  __Resource server__: Facebook.com

  __Resource__: user's facebook post and comments

  __Client__: amazon.com

### 📌📌 simülasyon

<https://www.oauth.com/playground/index.html>

### 📌📌 süreç

client taraf prod ortamına çıkmadan önce __auth_server__'dan (provider'dan) bir __client_id__ ve __client_secret__ alması şarttır.

request'lerin isimleri bu şekilde kullanılmaktadır:

```text
+--------+                               +---------------+
|        |--(A)- Authorization Request ->| Authorization |
|        |                               |     Server    |   (Akış-1)
|        |<-(B)-- Authorization Grant ---|               |
|        |                               +---------------+
|        |
|        |                               +---------------+
|        |--(C)-- Authorization Grant -->| Authorization |
| Client |                               |     Server    |   (Akış-2)
|        |<-(D)----- Access Token -------|               |
|        |                               +---------------+
|        |
|        |                               +---------------+
|        |--(E)----- Access Token ------>|    Resource   |   (authentication dışındaki akış)
|        |                               |     Server    |
|        |<-(F)--- Protected Resource ---|               |
+--------+                               +---------------+
```

OAuth 2.0'ın birden fazla akış çeşidi var. Yukarıdaki akış "Authorization Code" kullanarak ilerleyen akışın şemasıdır. Bu akış çeşidinde 2 istek var:

- Akış-1
  - client user'ı redirect ediyor provider'ın sitesine.
  - user şifresini girip, hangi bilgilerin paylaşılacağını onaylıyor.
  - provider, client'a redirect diyor user'ı ve __authorization_code__ döndürüyor.

- Akış-2:
  - client user'ın haberi olmadan __authorization_code__ ile provider'ın "token" endpoint'ine istek atıyor.
  - provider response olarak __access_token__ döndürüyor.

Burada 2 akış var. çünkü son kullanıcı onay verdiğinde, artık client sürekli olarak yeni token alabilecek. Eğer böyle olmasaydı, örneğin her browser'ı kapatıp açtığımızda veya Android uygulamamızı silip geri yüklediğimizde son kullanıcıyı tekrar provider'ın sitesine yönlendirmek zorunda kalacaktık. __authorization_code__ bilgisini client, kendi DB'de kalıcı olarak tutmalıdır.

### 📌📌 state

user Oauth2 sunucusuna yönlendirildiğinde state parametresi isteğe bağlı atılır. bu parametreyi alan provider, bu değeri redirect URL'ye yönlendirildiğinde, tekrar client'e geri yollar. böylece client doğru akışta olup olmadığını eski değere bakarak doğrulayabilir olacaktır.

### 📌📌 flow çeşitleri (access token'ı elde etme yöntemleri)

"Grant" terimi ile akış kastediliyor. Her akış birden fazla alt akış içerebilir. O yüzden bazen terminolojik karmaşa olabiliyor. Örneğin burada:

<https://auth0.com/intro-to-iam/what-is-oauth-2>

7 farklı "Grant Types" olduğu yazıyor. Oysa refresh token ile access token alma sürecini de grant olarak yazmışlar. doğru yazmışlar. çünkü oauth dünyasında bu böyle adlandırılıyor.

Fakat Oauth 2.0'de, 3 farklı şekilde access token'ı client elde edebiliyor.

#### 📌📌📌 1 - Authorization Code

By akış 2 temel akıştan oluşuyor. ayrı başlıklarda anlatıldı.

1inci aşamanın sonunda, kullanıcı client'a redirect ediliyor. client tarafa, bu success redirection sırasında __authorization_code__ da gönderiliyor. Fakat bu kod ile, client taraf, artık resource-API'ye istek yapamaz. çünkü redirect sırasında araya biri girmiş olabilir. burada 2 çözüm var:

- `Oauth` `2.0` `RFC`'sinde (`RFC 6749`) yazan: __authorization_code__ ile auth-server'a istek yaparız ve __access_token__'ı alırız. __access_token__ ile artık resource-API'ye istek atabiliriz. Burada sorun şu: auth-server'a __authorization_code__ yanında client-secret-key'i de yollarız. Fakat bu secret key'i özellikle native-mobile ve JS-client'larda saklamak imkansız. De-compile işlemi ile mobile uygulamalardaki string'leri çok kolay şekilde hacker'lar bulabiliyor.
- `Oauth` `2.0` `RFC`'sinde (`RFC 6749`) yazmayan ek uzantı çözüm: __Proof Key for Code Exchange (PKCE)__. `RFC 7636` Bu çözüm `Oauth` `2.0` `RFC`'sinde yazan çözüm patlak verebileceğinden ortaya atılmış.

`PKCE`, `Oauth` `2.0` `RFC`'sinde (`RFC 6749`) yazan __authorization_code__ süreci ile aynı. Sadece istekler bazı ek parametreler içeriyor:

- the client first creates what is known as a "__code verifier__". This is a cryptographically random string using the characters A-Z, a-z, 0-9, and the punctuation characters -._~ (hyphen, period, underscore, and tilde), between 43 and 128 characters long.
- Client hash256 alabiliyor; __code verifier__'i, "BASE64-URL-encoded string of the SHA256" olarak çevirmelidir. Yoksa direk code verifier ile devam edebilir. hash alınsın alınmasın, artık (bu adımda), bu değere artık "__code challenge__" deniliyor.

Artık __Authorization Request__'e aşağıdakiler ek olarak eklenmelidir:

- __code_challenge__
- __code_challenge_method__ --> "plain" (eğer hash alınmadıysa) veya "S256" (hash alındıysa)

Daha sonra "Authorization Code Exchange" adımında ise tekrar "__code_verifier__" değerini yolluyoruz. Burada artık __code_challenge_method__ yollamıyoruz.

Auth-server her 2 istekte attığımız code_challenge değerlerini kendi tarafında valide ediyor. Bu da güvenliği arttırıyor.

#### 📌📌📌 2 - implicit flow (⟷ implicit grant)

- Sadece 1 adımda direk client'a access token döndürülüyor.
- yine son kullanıcı redirect ediliyor. Fakat client 2 kere provider'a gitmiyor.
- güvensizdir. Bu çok özel/istisna durumlarda kullanılmalıdır.

kaynak: RFC 6749 title: "4.2. Implicit Grant", subtitle: "4.2.1. Authorization Request".

#### 📌📌📌 3 - username password

- Bazı uygulamalar direk son kullanıcının şifre ve username'si ile access token'ı elde edebiliyor.
- güvensizdir. Bu çok özel/istisna durumlarda kullanılmalıdır.

kaynak: RFC 6749 title: "4.3. Resource Owner Password Credentials Grant".

#### 📌📌📌 Authorization Code ile süreç - Adım 1

bu kısım "__Authorization Request__" olarak adlandırılıyor. bunu sağlayan server endpoint'ine "__Authorization endpoint__" deniliyor.

web browser'dan user'ı provider'ın sitesine yönlendirirken şu parametrelerle yönlendirmek gerekli:

- __client_id__

- __response_type__: bu istekte spesifik olarak "code" değerini vermek gerekli. "code", response'ta __authorization_code__ istediğini belirten bir value'dur.

- __redirect_url__: opsiyonel. provider'a kaydolurken de bu URL veriliyor. isterse bu adımda o değer override edilebilir. redirect URL http:// değilde app:// gibi de olabilir. bu şekilde native uygulamadan webview açarak login ettiğimiz kullanıcı tekrar uygulamamıza yönlendirilebilir.

- __scope__: provider'ın belirlediği string'lerdir. bu string'ler hangi yetkileri istediğini belirtiyor. arada boşluk olacak şekilde formatlı olmalılar.

'scope' uygulamanın kullanacağı izinler olarak düşünülebilir.

- __state__: (yukarıda açıklandı.) buraya verilen string'ler, aynen geri client'a yollanıyor. burada ek güvenlik sağlamak amaçlı, her istekte her seferinde random data yollamak şiddetle önerilir.

yukarıdaki istek provider'ın authorization URL'sine yapılmalıdır.

Bu request'in dönüşünde __authorization_code__ ve __state__ dönmektedir.

#### 📌📌📌 Authorization Code ile süreç - Adım 2 (Authorization Code Exchange step)

authorization_code'u access_token'a çevirme isteğidir/aşamasıdır.

istek attığımız API'ye, "__token endpoint__" deniliyor.

login olma aşaması bittiğinde, client tarafa __authorization_code__ gönderildi. bu kodu __access_token__'a çevirmek için provider'ın token URL'sine post isteği aşağıdaki parametrelerle yapılmalıdır:

- __grant_type__: bu istekte spesifik olarak "authorization_code" vermek gerekli.

- __code__: bize bir önceki response'ta gelen __authorization_code__.

- __redirect_uri__: success olma durumunda buraya bir istek yapılacak

- __client_id__

### 📌📌 API calls

__access_token__'ı alan `client`, artık API'ye istek yapabilir. Oauth2 bu istek hakkında standart içermiyor. fakat genelde header'daki `Authorization` değerine `Bearer 123456` şeklinde yollanıyor. `resource server` gelen istekteki __access_token__'ı alıyor ve __auth_server__ server'a gönderiyor. __access_token__'ın geçerli olup olmadığının cevabını __auth_server__'dan alıyor. her istekte bu işlem tekrarlanıyor. eğer gelen istekteki access_token bir `JWT` ise; o zaman `authentication server`'a gitmeye gerek kalmaz. çünkü `JWT`'de, `resource server`'ın kendisi, token'ı `private key` ile açıp kontrol edebilir.

### 📌📌 refresh token vs access token

refresh token JWT'deki aynı mantıkta kullanılmaktadır.

## 📌 openid connect vs oauth 2.0

- Scope: OpenID Connect akışında openid scope'u yazması gerekir. OAuth 2.0 de bu tarz özel bir scope yoktur.

- ID Token: OpenID Connect akışında, erişim token'ı ile birlikte bir "__id_token__" döndürülür. Bu token, __Resource server__ ve/veya client tarafından kullanıcıyı onaylamak için kullanılır. Bu token'ı oauth 2.0 dönmez.

- Kullanıcı Bilgileri: __id_token__ Bir `JWT` token'ıdır. Bu `JWT` içerisinde, client'ın belirttiği scope'ta ki bilgiler (email, photo which is `base64`, phone) gibi bilgiler direk __authentication server__ tarafından döndürülür. Böyle bir durum `oauth` `2.0`'da yoktur.
