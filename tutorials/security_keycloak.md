# SECURITY KEYCLOAK

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Keycloak

- Açık kaynaklı, user and security management tool'u olarak tanımlanabilir.
- Red Hat firması bu ürünü ticari olarak "Red Hat Single Sign-On" ismi ile pazarlıyor.
- Keycloak diğer servislerimizden tamamen bağımsız çalışan bir yazılımdır.
- Keycloak'ın user, client, key gibi bilgileri tuttuğu bir DB vardır.
- Keycloak, Keycloak'a login olmak için API ve HTML sunar. Bu API'ler OAuth 2, OpenID Connect gibi birçok yöntem ile login'i destekler.
- Keycloak'un eklenti altyapısı mevcuttur.

Client, Keycloak'a, Keycloak'un sunduğu API veya HTML aracılığı ile login olur. Token'ı client her istekte backend'e yollar.

- Eğer bu token JWT ise: her servisimiz bu token'ı kendi açar. Keycloak'a gitmeye gerek yoktur.
- Eğer bu token JWT değil ise: her defasında Keycloak'a backend istek atıp, user'ın role bilgilerini istemelidir.

## 📌 Web Admin UI

her realm'da şu sekmeler vardır:

- __User Federation__

  burada LDAP gibi seçenekler oluyor. Bu kısım, user DB'nin(kaynağının) nereden seçileceğini belirtebiliyoruz.

- __Identity Providers__

  "login with Google" gibi seçenek sunmak isteyebiliriz. Burada sunulan bazı seçenekler:

  - Google
  - Facebook
  - OpenID-connect (custom başka bir keycloak veya herhangi bir OpenID-connect destekleyen sunucu)

- __authentication__

  This menu includes the options about how the user will authenticate to keycloak. When we open this menu, it will list the "flow" table as seen here: https://www.keycloak.org/docs/latest/server_admin/keycloak-images/browser-flow.png

  The user has more than one alternatives flows to authenticate via keycloak. Each authentication flow can be seen in "flow" tab's table. There are default flows like "browser" which contains the flow where the user authenticate via web browser.

  Each flow can have multiple actions. The `Auth Type` column is the name of action that will be executed. Let's examine the browser-flow default actions:

  - Cookie

    When a user successfully logs in for the first time, a session cookie is set. If this cookie has already been set, then this authentication type is successful.

    It has set as "alternative". That means only 1 alternative (on the whole table(flow)) is enough for a successfully login. "Cookie" is one of those alternatives.

  - Kerberos

    The second execution of the flow looks at the Kerberos execution. This authenticator is disabled by default and will be skipped.

  - Identity Provider Redirector

    It can be configured through the Actions > Config link to automatically redirect to another IdP for identity brokering.

  - Forms

    this is a sub-flow. because it has sub-actions. Sub-actions are indented (they are on right column of parent-flow).

## 📌 Keycloak ile ilgili bir örnek login süreci işletelim

- keycloak'ı indirip ./bin/standalone.sh ile başlatalım
- web tarayıcısından <http://localhost:8080'e> gidelim
- admin username ve şifremizi belirlememizi isteyecek
- Administration Console'u seçelim
- admin şifremiz ile login olalım

Burada her "realm" birbirinden bağımsız keycloak ayarları kümesidir. hepsi ayrı ayrı export/import edilebilir.

- Yeni bir "realm1" isminde realm yaratalım.

- Bu ayarlarla yeni bir client yaratalım:
  - id: client1
  - client protocol: OpenID-connect
  - Access Type: confidential (bu seçilince artık her client secret-key (veya secret-JWT veya secret-X509-sertifika) ile access token alma işlemini yapabilecek. bu seçenek olmazsa secret-key'e gerek olmayacak.)

- username'i "user1" olan yeni bir user yaratalım. şifresini ise "user1password" olarak ayarlayalım.

  user'lar, client'lara login olacak user'lardır.

Basitçe tüm ayarlarımız hazır. Şimdi isteği yapıp login olalım (access token'ı alalım):

- GET isteğimizi atalım:

  ```text
  http://localhost:8080/auth/realms/realm1/protocol/openid-connect/auth
  ?client_id=client1
  &response_type=code
  &scope=openid
  &redirect_uri=http://localhost:8083/callback
  &state=state1
  ```

  "scope", permission kümesi olarak düşünülebilir. role ile benzer bir yapıdır. ilgili scope'lara izin verilip verilemeyeceğine son kullanıcıya karar verdirtebiliriz. örneğin facebook'a login iken, başka uygulamaya login olduğumuzda, facebook bize 3üncü parti uygulamanın "email,", "photo" gibi bilgilere erişeceğini gösterir. işte bunların her biri birer scope'tur. son kullanıcı isterse bunları reddedebilir.

  scope ile rol'ün şu açıdan temel farkı var: eğer client "photo" scope'u yok ise, o zaman user, "admin" rolünde olsa bile photo'ya erişemez. böylece ek güvenlik sağlanmış olur.

  Aslında scope'ta belirttiğimiz isterlere göre bize response'ta token (örnek: JWT) dönüyor.

  <http://localhost:8083/callback> fake bir adres. Tarayıcıdaki response'u görebilmek için yalan verdiğimiz adres.

  Yukarıdaki request bizi Keycloak'ın sunduğu login sayfasına yönlendirecek. "user1" ve "user1password" ile login'i devam ettirelim. Devam edince, web tarayıcı URL'inde şunu göreceğiz:

  ```text
  http://localhost:8083/callback
  ?state=state1
  &session_state=e9656216-5999-4327-aedb-b0a3a1272997
  &code=2aac650d-f0b6-436a-9627-7d6fe4ea0145.e9656216-5999-4327-aedb-b0a3a1272997.038379d8-626c-4af9-b5f6-faa2af2979ba
  ```

- 'code'u token'a çevirelim:

  POST isteği olacağından HTTP client kullanmakta fayda var:

  URL: <http://localhost:8080/auth/realms/realm1/protocol/openid-connect/token>

  Body:

  ```text
  grant_type:authorization_code
  client_id:client1
  client_secret:996c51c0-bd62-45d7-9ec2-496cf2d29eb8
  code:2aac650d-f0b6-436a-9627-7d6fe4ea0145.e9656216-5999-4327-aedb-b0a3a1272997.038379d8-626c-4af9-b5f6-faa2af2979ba
  redirect_uri:http://localhost:8083/callback
  state:state1
  ```

  Yukarıdaki client_secret Keycloak `Web UI` --> Administration Console --> realm1 --> client1 --> credentials --> içerisinde olan secret_key'dir.

  Response şöyle olacaktır:

  ```json
  {
    "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJ5clhVWFBSSXJDeGdJTmk4Ri1qVUotTlE4ZHN0c1NrMlBWWnpzMmFzYXFZIn0.eyJleHAiOjE2MTcyNjE2MzQsImlhdCI6MTYxNzI2MTMzNCwiYXV0aF90aW1lIjoxNjE3MjYwNjg0LCJqdGkiOiI2YWE0ZWM1OC0zZTAyLTQ0N2ItYTEyNy05MThkYTIyM2I4MTUiLCJpc3MiOiJodHRwOi8vbG9jYWxob3N0OjgwODAvYXV0aC9yZWFsbXMvcmVhbG0xIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjgwYjc3NTE4LTk3N2ItNDBhYS1hN2Y1LTQ3NmZkMjU2ZDMzYyIsInR5cCI6IkJlYXJlciIsImF6cCI6ImNsaWVudDEiLCJzZXNzaW9uX3N0YXRlIjoiZTk2NTYyMTYtNTk5OS00MzI3LWFlZGItYjBhM2ExMjcyOTk3IiwiYWNyIjoiMCIsInJlYWxtX2FjY2VzcyI6eyJyb2xlcyI6WyJvZmZsaW5lX2FjY2VzcyIsInVtYV9hdXRob3JpemF0aW9uIl19LCJyZXNvdXJjZV9hY2Nlc3MiOnsiYWNjb3VudCI6eyJyb2xlcyI6WyJtYW5hZ2UtYWNjb3VudCIsIm1hbmFnZS1hY2NvdW50LWxpbmtzIiwidmlldy1wcm9maWxlIl19fSwic2NvcGUiOiJvcGVuaWQgcHJvZmlsZSBlbWFpbCIsImVtYWlsX3ZlcmlmaWVkIjpmYWxzZSwicHJlZmVycmVkX3VzZXJuYW1lIjoidXNlcjEifQ.S0cUItrQnbhbP6Hm3q4P2P_-TOIA2RIFnWKBpxuACp3O1qI7rnyVww1Dv_nOZl4Ase2_174rRkBnUEPc9mn1Z7E-_XXXe3Xr94ZFVP0zk2qZ0EosQsyv--rMjFryWlZfE5sSeiKAoYGWmz7dctTvaldqQCcFTZpL6qCXMWOm1MaspYV3pce53Gxngrgk57_nqLUexa9mahxiwEnhhT4n5gg1ZWjiPiehnoOntistTVGTvj6uvlWi-GV6sa-DyvDEXXN26OfR94u22P7eIGIOdym7IJRa3IRKcit12cM1vgUPTAnHsD64pihGyAaVFxoBn91ioyw671ANWgXSxTf0jg",
    "expires_in": 300,
    "refresh_expires_in": 1800,
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJjZjc0YWZjOC1hY2IzLTQ5NjctOGM3ZS0yMjM1OGJmZWZhNWYifQ.eyJleHAiOjE2MTcyNjMxMzQsImlhdCI6MTYxNzI2MTMzNCwianRpIjoiZTkyNzQ3NDktOGUwYi00ZmU0LWFhMjgtMWU3MjUyMTU4YTljIiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL2F1dGgvcmVhbG1zL3JlYWxtMSIsImF1ZCI6Imh0dHA6Ly9sb2NhbGhvc3Q6ODA4MC9hdXRoL3JlYWxtcy9yZWFsbTEiLCJzdWIiOiI4MGI3NzUxOC05NzdiLTQwYWEtYTdmNS00NzZmZDI1NmQzM2MiLCJ0eXAiOiJSZWZyZXNoIiwiYXpwIjoiY2xpZW50MSIsInNlc3Npb25fc3RhdGUiOiJlOTY1NjIxNi01OTk5LTQzMjctYWVkYi1iMGEzYTEyNzI5OTciLCJzY29wZSI6Im9wZW5pZCBwcm9maWxlIGVtYWlsIn0.mlZJi39ksf2coHBuD2KJu6RAD2onibrLsTgJZnI_Sik",
    "token_type": "Bearer",
    "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJ5clhVWFBSSXJDeGdJTmk4Ri1qVUotTlE4ZHN0c1NrMlBWWnpzMmFzYXFZIn0.eyJleHAiOjE2MTcyNjE2MzQsImlhdCI6MTYxNzI2MTMzNCwiYXV0aF90aW1lIjoxNjE3MjYwNjg0LCJqdGkiOiJkMTllNDdmNy0wOGI2LTQ3ZGUtYjM1MS1mNzY0M2M2MDhkZmEiLCJpc3MiOiJodHRwOi8vbG9jYWxob3N0OjgwODAvYXV0aC9yZWFsbXMvcmVhbG0xIiwiYXVkIjoiY2xpZW50MSIsInN1YiI6IjgwYjc3NTE4LTk3N2ItNDBhYS1hN2Y1LTQ3NmZkMjU2ZDMzYyIsInR5cCI6IklEIiwiYXpwIjoiY2xpZW50MSIsInNlc3Npb25fc3RhdGUiOiJlOTY1NjIxNi01OTk5LTQzMjctYWVkYi1iMGEzYTEyNzI5OTciLCJhdF9oYXNoIjoieUpOdU1yeXBjQ0pIbFlEaXpscmw0dyIsImFjciI6IjAiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsInByZWZlcnJlZF91c2VybmFtZSI6InVzZXIxIn0.KJJeYeDPT4a04RmSz5Kphu2N-sgVm7d211A7YoPrAlXfxuvcdb3qCNvrC61Q8IWyPMay3-EoJzWVms2wDnICfaI2WTAeShDCwChfK1r3h2nRUGlqgaSpj29njjmKAK3RIUgpGqo6MZcHsElmt6c51RdciA3nsUYnRyhtR9TToqMkoQqWVDCPhl_9Oqg7_hjpyKsTe7DFX7KPt6nE7lafX4xT6T8sby-AFb0KRt2hSX3NlXlpA7nBzFdFjrad2FjqdE2IcjSF2R7kGk7bMIwVoZLAPypSlNNK5wSPC_UXmEkQ054LlRpnDQuogevetOEeK4DfCyRVS4e05LdM_Vwo1A",
    "not-before-policy": 0,
    "session_state": "e9656216-5999-4327-aedb-b0a3a1272997",
    "scope": "openid profile email"
    }
    ```

    Refresh token ile yeni access token alma işlemi keycloak üzerinden yapılır.

    Keycloak, JWT'ye ek parametreler geçebilmemizi de sağlar.

    Yukarıdaki JWT token ile artık herhangi bir resource-server'a istek yapabiliriz. İstek JWT olduğundan role, client gibi bilgiler zaten içinde mevcut. Resource server bu bilgilere dayanarak bize kapıları açacaktır.

    JTW'te gelen roller keycloak `Web UI` tarafından da belirlenmektedir. Fakat resource-server'ın bu rolleri ignore edip etmeyeceğini keycloak bilemez. Bazı sistemler keycloak'ta olan rolleri hiç kullanmayabilir. Böyle bir durumda; resource-server, JWT'de olan user-ID'yi kendi DB'lerinde aratıp role (permission) bilgilerini çekmektedirler.

    JWT decode edildiğinde role'lerin olduğu kısım şu şekilde olur:

    ```json
    "realm_access": {
        "roles": [
            "realm_role1",
            "offline_access",
            "uma_authorization"
        ]
    },
    "resource_access": {
        "client1": {
            "roles": [
                "client_role1"
            ]
        },
        "account": {
            "roles": [
                "manage-account",
                "manage-account-links",
                "view-profile"
            ]
        }
    }
    ```

    Yukarıdaki roller şurada gelir:

  - "realm_access" rolleri sadece Keycloak admin panelindeki "role" sekmesinden belirlenebilir ve her client'ın user'ı için JWT token'ında bu değer dönmektedir (user spesifik kapatılıp açılabilir).

  - "resource_access" sadece Keycloak admin panelindeki her client altından yeni role oluşturularak eklenebilir. bir client altında diğer client'larında rollerini ekleyebiliyoruz. Bu örnekte olduğu gibi resource_access altında 2 farklı client'ın rolleri var: "client1" ve "account".

## 📌 Form pages

Keycloak login, forgot password gibi sayfalar için kendi template'ini sunuyor. Her form sayfasının template'i Keycloak sunucusunun içerisinde bulunuyor. server side rendering ile form'lar önyüze gösteriliyor. Bu template dosyalarına alternatif template'ler sunucuya manuel kopyalanarak yüklenebilir.

Her sayfa load edilirken ilgili sayfanın template'i FreeMarkerLoginFormsProvider'dan da görülebileceği gibi bazı field'lar ile dolduruluyor.

örnek olarak login sayfasında server-side'da "realm.registrationAllowed" field'ına ulaşılabiliyor ve buna göre render yapılıyor.

## 📌 remember me

Login page'lerinde opsiyonel olarak "remember me" seçeneği sundurtulabiliyor. kullanıcı bunu seçip login olursa, artık browser kapatılıp açılsa bile, son kullanıcı login olmuş olarak web sayfasını kullanmaya devam edebiliyor.
