# SECURITY CRYPTO

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 terminology

algoritma isimlerini gruplar halinde listeleyen bir kaynak: https://docs.oracle.com/en/java/javase/11/docs/specs/security/standard-names.html

## 📌 Simetrik Şifreleme Algoritması (⟷ gizli anahtarlı şifreleme ⟷ secret key encryption ⟷ symmetric-key algorithm)

bazı kaynaklarda şöyle isimlendiriliyor: __single-key yada shared-key yada one-key__

Veriyi şifrelemek ve geri çözmek için aynı anahtar kullanılıyor.

## 📌 Asimetrik şifreleme Algoritması (⟷ asymmetric algorithm ⟷ açık anahtarlı şifreleme algoritması)

- `public key` ve `private key` yapısına dayanır.

- `public key` ile şifrelenmiş bir data'yı, ancak (o `public key` ile birlikte oluşturulan) `private key` ile düzgün şekilde açabiliriz.

- Aynı durum tersi için (`private key` ile şifrelenip), `public key` ile bu şifre açılabilir mi bilmiyorum. çünkü:

  - Bu konuda yazılar var. fakat yazılar spesifik şifreleme algoritmalarına göre yazıldığı için, tüm asimetriklerde bu kural geçerli mi kesin bilmiyorum.
  - Üstelik RSA (asimetrik şifreleme) gibi algoritmalar bunu desteklese bile, padding, ECB gibi extension'lar bu kuralı bozuyor olabilir.

## 📌 anahtar (⟷ key) ve şifrelerin çözülme hızı

- public ve `private key`'ler birer şifredir. dosya olmalarının sebebi, büyük boyutta olmalarıdır. örnek 512 karakterli şifre.

- 2017'de ortalama bir bilgisayar, 128'lik bir SHA ile şifrelenmiş bir veriyi 2 trilyon yılda çözebiliyor.

- Aynı anahtar uzunluğuna sahip simetrik algoritma, asimetriklere göre daha uzun sürede çözülebiliyor. örnekler:
  - 1024-bit RSA keys are equivalent in strength to 80-bit symmetric keys
  - 2048-bit RSA keys to 112-bit symmetric keys
  - 3072-bit RSA keys to 128-bit symmetric keys
  - 15360-bit RSA keys to 256-bit symmetric keys

  Yukarıdaki değerler için kaynak: https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r4.pdf "Series/Number: NIST Special Publication 800-57 Part 1 Revision 4", Title: "Recommendation for Key Management, Part 1: General".

## 📌 passphrase

pass (geçiş) ve phrase (ifade) kelimelerinin birleşimidir. parola ile aynı anlamda kullanılır. fakat passphrase daha çok son boşluk içeren kelimelerin birleşiminden olabileceğini ifade etmek için kullanılır. örnek passphrase: "Hello I am 1st developer$$"

## 📌 SSH-keygen komutunda istenen passphrase

`SSH-keygen` gibi komut satırı uygulamaları ile `private key` ve `public key` oluşturulmaktadır. bu `private key`'ler oluşturulduğunda bizden passphrase istenebilir. bu opsiyoneldir. eğer passphrase istenirse oluşturduğumuz key, bu passphrase ile şifrelenir. aynı şekilde `public key`'de buna istinaden oluşturulur. dolayısı ile `private key` dosyamıza ulaşan hacker, şifreli dosyamıza ulaşmış olur.

## 📌 elektronik imza (⟷ electronic signature)

genel bir terimdir. "dijital imza", "parmak izi" gibi birçok çeşit imza çeşidini kapsar.

## 📌 sayısal imza (⟷ digital signature)

karşı tarafa mesaj atan taraf şunu yapar:
- mesajın hash'i mesaja ekler
- mesajı komple `private key` ile şifreler.

mesajı alan taraf:
- mesajı `public key` ile açar (bu `public key` mesajın içinde değildir. farklı bir kanaldan alınmış olmalıdır)
- mesajın kimden geldiği kesinleşmiş olur.
- mesajın hash'ini de kontrol ettiği için bütünlüğünden (değiştirilmemiş olduğunun) emin olur. 

## 📌 RSA

Geliştiricilerinin soy isimlerinin baş harflerinden isim oluşturulmuştur.

asimetrik şifreleme algoritmasıdır.

## 📌 Shamir's Secret Sharing

Shamir soy isminde biri tarafından geliştirilmiştir.

Bir çeşit __Secret sharing (⟷ secret splitting)__'dir.

5 adet anahtar oluşturuyoruz. Bunlardan 3'ü elimizde olunca, master-key'i elde etmiş gibi işlem yetkisine sahip oluyoruz. (5 ve 3 sayıları bu yazıda rastgele verilmiştir. 5'e 5 te olabilirdi.)

Bu şekilde "secret zero" problemine (kısmen) çözüm getirilir.

## 📌 cipher (⟷ cypher)

şifrelemek için kullanılan algoritmayı temsilen kullanılan terimdir.

__ciphertext__ terimi "şifreli" anlamına gelmektedir. bunun tersine ise "__plaintext__" denir ve Türkçe'de "__açık metin yada düzmetin__" olarak çevrilir.

## 📌 encrypting vs coding (⟷ encoding) | decrypting vs decoding

__coder (⟷ kodlayıcı)__ ile __cypher__ farkı şu.
- "cypher"; key ve data'yı input alır fakat belli bir algoritmaya göre çalışır ve output üretir.
- "coder"; sadece data'yı input olarak alır ve data'daki her birime karşılık farklı çıktılar üretir. yani bir nevi syntax/format değişikliği yapar.

"morse" code sistemi, code'a bir örnektir. oysa RSA, cypher'a bir örnektir.

__codebook__ terimi ise; code'un çalışması sırasında, her birimi çevirmek için, her birime karşılık gelen bilgileri okuduğu DB'dir.

__code__ terimi 2 farklı anlamda kullanılıyor:
- kodlayıcının kullandığı metoda referans ediyor.
- __codebook__'taki en ufak birime verilen isim.

## 📌 cryptogram

bilgisayar kullanmadan, elle çözülebilecek kadar basit, human-readable karakterlerden oluşan şifrelenmiş metne denir. bu şifrelenmiş metin, bilgisayarsız ortamda da çözülebilsin diye ayarlanmıştır. genelde şu amaçlarla kullanılır:

- Bulmacalar ve zeka oyunları
- Kriptografi dersleri
- Gizli mesajlaşmalar. (insanlar arasında birebir anlaşarak kullanılır. akademik ortamda ve public protocol'lerde kullanılmaz.)

## 📌 base64

- çift yönlü bir algoritmadır.
- anahtarsız çalışır.
- geri çevrilme de anahtarsız yapılır.
- bu sebeple şifreleme algoritması değildir.
- çıktı kümesinin çoğunda işaret karakterleri olmadığından, dijital ortamdalarda sıkça kullanılır.

input olarak verilen binary data; 6 bit olarak okunur. `2^6=64` olduğundan; 64 farklı karakter çıktı kümesi vardır. çıktı kümesine `Base64 Alphabet` deniliyor. çıktılar şu şekilde:

- `000000` çıktısı: `A`
- `000001` çıktısı: `B`
- `000002` çıktısı: `C`
- diğer değerler (bunları yazmaya gerek yok)
- `000025` çıktısı: `Z`
- `000026` çıktısı: `a`
- `000027` çıktısı: `b`
- diğer değerler (bunları yazmaya gerek yok)
- `000051` çıktısı: `z`
- `000052` çıktısı: `0`
- `000053` çıktısı: `1`
- diğer değerler (bunları yazmaya gerek yok)
- `000061` çıktısı: `9`
- `000062` çıktısı: `+`
- `000063` çıktısı: `/`

`base64`'ün birçok implementasyonu var:

- `RFC 3548` or `RFC 4648`

  İki `RFC` arasındaki fark neredeyse yok. `kaynak: https://www.rfc-editor.org/rfc/rfc4648.html#page-15`

  standart `base64`. Bu genel kabul gören ve özel bir amaç için tasarlanmayan `base64`.

- `RFC 2045`

  `BASE64` for `MIME`.

- ve daha birçoğu: https://en.wikipedia.org/wiki/Base64 "Variants summary table" başlığı.`

eşitlik işareti bazı implementasyonlarda padding karakteri olarak kullanılmaktadır.

## 📌 Java'da base64

`Java`'da birçok `base64` encode ve decode kütüphanesi var.

- `Java` 6 ile

```java
javax.xml.bind.DatatypeConverter.printBase64Binary("hello".getBytes());

DatatypeConverter.parseBase64Binary("aGVsbG8gd29ybGQ=");
```

- Java 8'de

daha esnekleştirilerek birçok base64 destekleyecek şekilde yeni embedded kütüphane gelmiştir. direk örnek üzerinden incelersek:

```java
// standart base64 (RFC 3548 or RFC 4648)
Base64.Encoder standartEncoder = java.util.Base64.getEncoder(); 

// URL safe base64.
// RFC 4648'ı içinde yazan özel bir türevdir. standart base64 ile aynıdır. tek farkı şudur:
// + yerine -
// / yerine _ 
// kullanır.
// RFC 4648 içinde buna 'base64url' olarak isimlendirme önerilmiştir.
Base64.Encoder urlEncoder = Base64.getUrlEncoder();

// mime base64
Base64.Encoder mimeEncoder = Base64.getMimeEncoder(); 

// run encoding
enc.encode(data.getBytes()); 
```

## 📌 hash algoritmaları (⟷ özet fonksiyonları)

hash Türkçe anlamı: bir malzemenin işlemden geçirilerek yeniden sunulan malzeme anlamına gelir. örneğin; kıyılan etin tekrar sunulması gibi.

- çıktı ile, girdiyi bulmak mümkün değildir. yani algoritma tek yönlü çalışır. şifreleme algoritmalarında ise bunun tersi şeklinde; çıktıdan girdiyi (bir anahtar aracılığı ile) elde edebilirsiniz.

- tüm hash algoritmaları gibi; birden fazla girdi, aynı çıktıyı üretebilir. aynı büyüklükteki string'ler bile, aynı hash sonucunu verebilir. çünkü her 1 girdi'ye karşılık, 1 çıktı olması şart olduğundan, birden fazla girdi aynı çıktıyı vermek zorunda kalacaktır. bu duruma __hash collision (⟷ hash clash ⟷ hash çakışması ⟷ hash çarpışması)__ denir.

## 📌 non-cryptographic hash function

bazen hash'lerken geriye dönülememesini önemsemeyiz. bu tarz algoritmalara verilen isimdir.

geriye dönülememezlik özelliğinden feragat ettiğimiz için başka özelliklerden kazanırız. örneğin; performans gibi.

### 📌📌 Real implementation

"__Murmur__"; non-cryptographic hash algoritma ailesidir. birçok farklı türevi ve versiyonu vardır.

### 📌📌 çıktı kümesi

Çoğu zaman çıktı kümesinin aşağıdaki 2 kurala uyması beklenir:

çıktı kümesi sadece sayısal olması gereklidir. böylece:
- bu değer array-index'i olarak kullanılabilir.
- hash'ler arasında sıralama yaparak aramak aşırı hızlı olur.

çıktı boyutunun sabit olması gereklidir. böylece:
- hashcode'lar bucket'lara dengeli gruplanabilir (modulo işlemine tabi tutulması için sayı olmalıdır. bucket başka başlıkta anlatılıyor)
- kullanacağımız primitive tipi (long, int) bilmezsek BigDecimal gibi bir yapı kullanmak durumunda kalırız. Bu da aramalarda yavaşlığa sebep olur.

### 📌📌 real world use case

- Veri Yapıları (Hash Table, HashMap, Dictionary)

- Bloom Filter

- server load balancing

  örneğin: 5 sunucu varsa, gelen request'i IP'sinin hash'i alınır böylece "modulo 5" işlemine tabi tutulabilir. hangi sunucuya gideceğini bi DB kullanmadan yönlendirebilmiş oluruz.

- NoSQL sharding

  Hash fonksiyonları, hangi verinin hangi parçaya (shard'a) ait olduğunu belirlemek için kullanılır.

- Olasılık/istatistik (hata payını kabul edebileceğimiz algoritmalar)

  hash alarak data'yı birbiri ile aynı kabul edebiliriz. Kısmen güvenilmez olur, ama hızlı olur.

  örnekler:
  - sistemde en fazla hangi hata log'u atılmış diye dashboard'umuz olabilir.
  - anonim user bilgileri toplarken, aynı hash'e sahip user'ları, aynı user'mış gibi kabul edebiliriz.

## 📌 cryptographic hash functions

güçlü hash metotları kesinlikle yeterli bir çözüm değildir. çünkü:

- elektronik cihazlar güçlendikçe ve maliyeti düştükçe brute force atak yapmanın maliyeti ve hızı da düşmektedir.
- son kullanıcılar genelde belli kelimeleri birleştirerek şifre oluşturabilmektedirler. bu durumlara karşı sözlüklerdeki kelimeler üzerinden kombinasyonlar ile hash'leri deneyen brute force yazılımları mevcut.
- her string'in hash karşılığının tutulduğu DB'leri mevcut. bu DB'lerden basit bir `SQL` sorgu ile hash karşılığı bulunabilir.

Bazı cryptographic hash fonksiyonları anahtar ile bazıları anahtarsız çalışabilir. Anahtarlı olanlar genelde HMAC için kullanılıyor.

## 📌 Salt

kelime anlamı: tuz

- hash'leme algoritmaları ne kadar güçlü olsa da güvenliği arttırma amaçlı hash'lemeden önce her şifrenin(input'un) sonuna veya başına bir string eklenmesi önerilir. bu string'e salt denir.
- md5 ve sha1 salt olsa bile güvensiz olarak kabul edilmektedir.
- Salt'ın çok gizli tutulmasına çok takılmamak gerekli.
- Salt her user şifresi için ayrı üretilmelidir ve salt üretimi için "Cryptographically Secure Pseudo-Random Number Generator"lar kullanılmalıdır.

Farklı user'lar, aynı şifreyi kullanarak sistemimize register oldu. Bu durumda ikisinin aynı hash değeri DB'de kayıtlı olur. bu bir risk'tir. DB'yi okuyan kişi bunu görebilir. Fakat her şifre için farklı salt kullanırsak bu durumu aşmış oluruz. (Rainbow table attack'tan bizi korumaktadır.)

## 📌 Pepper

kelime anlamı: biber

Salt ile tamamen aynı mantıkta kullanılır. Fakat salt genelde DB'de hash'lenmiş çıktı ile aynı fiziksel yerde saklanırken, pepper farklı bir yerde saklanır (örneğin OS environment değerlerinde). pepper tüm şifreler için aynıdır/sabittir. pepper sadece ek koruma sağlamak için kullanılır. Hacker tüm sistemi değilde, sadece DB'yi ele geçirmişse, pepper'ı bilmeyeceği için riskleri azaltmış olacağız.

## 📌 Sözlük Saldırısı (⟷ Dictionary Attack)

brute-force attack'ın bir çeşididir. Sık kullanılan şifrelerden oluşmuş bir DB'deki her şifre ile, şifrelenmiş data açılmaya çalışılır.

## 📌 HashTable (for Dictionary Attack)

Şifre ve ona karşılık hash'lerin bulunduğu bir DB'deki bilgiler sayesinde hash'ten input'u elde edebiliriz. Önceden tüm hash'ler hesaplanmış olduğu için CPU'ya ihtiyacımız daha azdır.

Bu yöntemin en büyük dezavantajı, tüm şifrelerin olduğu bir DB terabayt'lar almaktadır.

## 📌 Gökkuşağı Tablosu Saldırısı (⟷ Rainbow Table Attack)

Hacker'ler birçok string'in MD5 hesaplanmış halini tuttukları DB'leri var. bu DB'lere "__rainbow table__" denir.

standart bir lookup table mantığında düşünürsek, bu DB'yi çok büyük olur. buna kısmen de olsa bir trick ile çözüm bulmuşlar (bu trick güvenlik konusunun dışında olduğu için bunu anlatmıyorum).

bu DB'leri kullanarak hacker'lar, disk'ten kar ediyorlar ama CPU'dan zarar ediyorlar.

bu DB'yi kullanarak yapılan herhangi bir saldırıya "Rainbow Table Attack" denir.

## 📌 şifre üretiminde kullanılması önerilen algoritmalar

şifrelerin bunlarla üretilmesi önerilir: Argon2, bcrypt, scrypt, PBKDF2

## 📌 key stretching (⟷ anahtar germe)

decrypt ve encrypt işlemlerinin süresini arttıracak algoritmaların kullanılmasıdır (bazı algoritmalar bunu bilerek yapar).

bu sadece brute-force atakların önüne geçirir.

bunun için ideal algoritmalar: PBKDF2, bcrypt

Bu konu sanal paralardaki ASIC olayı ile benzerdir ("Equihash" başlığı). bu kaynakta: https://crackstation.net/hashing-security.htm#properhashing bu tarz algoritmalar "CPU-intensive hash function" olarak nitelendirilmiş. Bu URL, "Sam Newman" "Building microservices" kitabında 180inci sayfadaki "Go with the Well Known" başlığında, 3üncü paragrafta önerilmiştir.

## 📌 hash kodları (⟷ hash toplamları ⟷ hash sums ⟷ hash digest ⟷ hash özeti ⟷ digests ⟷ hashes ⟷ message digest)

hash fonksiyonlarından dönen değere (çıktı değerine) denir.

## 📌 kontrol toplamları (⟷ checksums)

teknik olarak "hash sums" ile aynıdır ama amaç farklıdır.

dosyaların bütünlüğünü kontrol etmek için "hash sums" kullanılır. dosyalar büyük olduğu için checksum için çok basit ve hızlı algoritmalar tercih edilir.

## 📌 HMAC (⟷ keyed-hash message authentication code ⟷ hash-based message authentication code)

MAC işlemini HASH algoritması kullanarak yapan algoritmalardır.

hash algoritması için kurallar:
- simetrik anahtar gereklidir
- cryptographic hash function olması gerekir.

Bu şekilde, sadece anahtara sahip olan kişi

- data'nın saf hali
- data'nın HMAC alınmış hali

yukarıdaki 2 data'yı karşılaştırabilir. Böylece saf data'nın değiştirip değiştirilmediğini bilebilir.

HMAC tek başına anlam ifade etmiyor. HMAC'i implemente ederken hangi algoritma ile implemente ettiğimizi de belirtmemiz gerekiyor. Örneğin "HMAC with SHA256" gibi.

### 📌📌 HMAC vs digital imza

|                   | HMAC                                                 | Dijital İmza                                               |
|-------------------|------------------------------------------------------|------------------------------------------------------------|
| Anahtar Tipi      | simetrik                                             | asimetrik                                                  |
| Kullanım Amacı    | Mesajın bütünlüğü & authentication                   | Mesajın bütünlüğü & authentication & inkar edilemezlik     |
| İnkar Edilemezlik | Yok (herkes aynı anahtarı bildiği için bunu yapamaz) | Var (çünkü sadece özel anahtara sahip taraf imzalanabilir) |
| hız               | hızlı                                                | yavaş                                                      |

### 📌📌 HMAC vs digital imza (real world use case)

- HMAC
  - JWT'de, web-browser sadece mesajı HMAC ile doğrular. (JWT'de farklı senaryolarda var. Bu sadece sıkça kullanılan bir senaryo örneği)
  - VPN'de, karşıya yollanıp alınan data paketleri, HMAC ile doğrulanır. sebebi:
    - vpn ilk başlangıçta bir kere "digital imza" ile kişiyi onaylar zaten.
    - ama sonrasında, vpn oturum süreci yıllar sürebilir. bu sebeple eğer dijital imza kullanılırsa:
      - bu performansı olumsuz etkiler.
      - VPN oturumu boyunca (belki yıllarca) `private key`'in (hem sever hem de client'ta) RAM'de tutulması gerekirdi. Şu andaki VPN mimarilerinde oturum bazlı geçici bir key ile HMAC kullanılıyor.

- digital imza
  - Blockchain işlemlerinde (Bitcoin, Ethereum) cüzdana sahip kişi `private key` ile para transferini imzalar, `public key` ile dünyadaki herkes işlemi görür.
  - Dijital belgeleri onaylama
  - e-imza ile login olma
  - SSL de web browser ile web sitenin haberleşmesi

## 📌 merkle tree (⟷ Hash tree)

özel bir tree veri yapısıdır. ağacın her yaprağı altındaki ona bağlı yaprakların hash'ini tutmaktadır. en üstteki ağacın kök yaprağına da "hash sum" adı verilir.

## 📌 Stream cipher

Block şifrelemedeki gibi blok blok değilde, stream'den gelen her bilgiyi direk şifreleyen algoritmalara denir. Stream cipher'da genelde, en ufak birim bit veya byte olarak baz alınır. Stream cypher, block şifrelemeye alternatiftir. Block şifrelemede en ufak birim block'tur.

## 📌 Blok Şifreleme (⟷ Block Cipher)

Blok şifreleme kullanan algoritmalarda, şifrelenecek tüm mesaj 'block size' kadar bölünür. Her bölünen parça ayrı ayrı şifrelenerek, şifrelenmiş halleri yan yana koyulur.

Block şifrelemede, __block size__ ne kadar büyükse, şifrelemede o kadar güvenlik sağlanmış olur. Zira, blok uzunluğu büyükse, her bloğu ayrı ayrı tahminleme ile inceleyen hacker'a daha uzun bir çıktı kümesi vermiş oluruz.

Block size ile, şifrelemede kullanılacak anahtarın boyutunun bir ilişkisi olmak zorunda değildir. (yani; anahtar boyutu, blok boyutundan küçük olmalı gibi kurallar yoktur)

Blok şifreleme kullanan birçok algoritma vardır. Fakat her blok şifrelemenin de birçok çeşidi (tarzı) vardır. Bunlar "__çalışma kipi (⟷ mode of operation)__" olarak ta adlandırılırlar. örnek çalışma kipleri:

### 📌📌 Elektronik kod defteri (⟷ Electronic Codebook ⟷ ECB)
  
Blok şifrelemenin en basitidir. Her blok bağımsızca şifrelenir ve sonrada yan yana dizilerek birleştirilir.

### 📌📌 Şifre Blok Zincirlemesi (⟷ Cipher Block Chaining ⟷ CBC)

her blok bağımsız şifrelenir. fakat her blok şifrelenmeden önce, bir önceki blokun şifrelenmiş halini ile XOR'lanır (ve daha sonra şifrelenir).

İlk bloğun öncesinde hiçbir blok olmadığı için, ilk blok manuel olarak verilecek "Initialization Vector" ile işleme tabi tutulur.

CBC mode'nun dezavantajları (Aşağıdaki cümlelerin tersi, ECB'nin avantajlarıdır):

- CPU her bloğu paralel şekilde şifreleme yapamaz. yani; şifrelerken yavaştır.

  Not: Hem CBC hem de ECB, decrypt işlemini her bloğu paralel şekilde işleterek yapabilir.

- Şifrelenmiş mesajda herhangi bir bloğu kaybedersek, mesajın kaybolan kısmından sonrasını açamayız.

CBC mode'nun avantajları (Aşağıdaki cümlelerin tersi, ECB'nin dezavantajlarıdır):

- Herhangi bir blok, farklı bir bloktaki ile aynı açık metni şifreliyor olsun (böyle denk gelmiş olsun). Buna rağmen farklı şifrelenmiş çıktı verecektir. Çünkü her block bir önceki blok ile de matematiksel işleme tabi tutulduğu için aynı çıktıyı vermez.

### 📌📌 Yayılımlı Şifre Blok Zincirlemesi (⟷ Propagating Cipher Block Chaining ⟷ PCBC ⟷ plaintext cipher-block chaining)
  
  CBC ile aynı mantıktadır. CBC; sadece bir önceki bloğun şifreli halini kullanırken, PCBC buna ek olarak; bir önceki bloğun açık halinden (şifrelenmemiş halinden) de yararlanır.

### 📌📌 Şifre Geri Beslemeli (⟷ Cipher FeedBack ⟷ CFB)

### 📌📌 Çıktı Geri Beslemeli (⟷ Output FeedBack ⟷ OFB)

### 📌📌 Sayıcı Modunda Şifreleme (⟷ Counter Mode Encryption ⟷ CTR)
  
Bazı kaynaklarda sadece 'CTR mode'un kısaltması olduğundan "__CM__" olarak da kullanılmaktadır. Bazı kaynaklarda ise; "__integer counter mode (⟷ ICM) and segmented integer counter (⟷ SIC)__" olarak kullanılır.

## 📌 Java Cipher

"algorithm/mode/padding" formatında "transformation" adı verilen string'i alır.

```java
Cipher c = javax.crypto.Cipher.getInstance("AES/CBC/PKCS5Padding");
```

Eğer getInstance'a sadece "AES" verilirse, bu durumda provider'ın sunduğu default değerler kullanılır.

## 📌 ilklendirme vektörü (⟷ starting variable ⟷ SV ⟷ Initialization Vector ⟷ IV ⟷ İV ⟷ ilklendirme değişkeni ⟷ başlangıç vektörü ⟷ BV)

IV, "salt" ile karıştırılmamalıdır. Farklı şeylerdir fakat aynı konsepte hizmet ederler.

'Salt'ta da olduğu gibi her şifrelemede farklı kullanılmalıdır ve data açılmak istendiği zaman kullanmak üzere DB'de saklanmalıdır. 'Salt' standart algoritmaların dökümantasyonunda belirtilen bir terim değil iken, IV direk algoritmanın kendisinde olan bir terimdir.

IV yerine key kullanılamaz mıydı? Eğer key IV yerine kullanılırsa, o zaman aynı plan-text'ler aynı şifrelenmiş bilgiyi üretir. Bu da güvenlik açığı meydana getirebilir.

Eğer şifreleme işleminde IV kullanılmazsa, CBC tabanlı algoritmalarının yapısı gereği, aynı veri şifrelendiğinde, şifrelenmiş verinin sadece ilk bloğu, aynı çıktıyı verir. kaynak: https://richardwarrender.com/2016/04/encrypt-data-using-aes-and-256-bit-keys/ "IV – the bit twister" başlığı.

örnek:

openssl komutu ile bir mesajı şifreleyelim:

```sh
$ openssl enc -aes128 -k "secretpassword" -a -p -nosalt -iv 00
key="2034F6E32958647FDFF75D265B455EBF"
Hello Mr Warrender, I have some terrible news

VVklkPrL5fczxmu4vZ93BnfBBpU8BWK1IQhHF6JRKSNZJ7PvpcaE8K/Mkbx1xgHa
```

```sh
openssl enc -aes128 -k "secretpassword" -a -p -nosalt -iv 00
key="2034F6E32958647FDFF75D265B455EBF"
Hello Mr Warrender, This is good news

VVklkPrL5fczxmu4vZ93BkAsVA64MLd5uah+zTVzr0XlOONVgDEd7ZunyIIzhpAo
```

Farklı bir IV kullanırsam, ilk bloğu farklı bir çıktı alırım:

```sh
openssl enc -aes256 -k "secretpassword" -a -p -nosalt
key="2034F6E32958647FDFF75D265B455EBF40C80E6D597092B3A802B3E5863F878C"
iv =AD0ACC568C88C116D57B273D98FB92C0
Hello Mr Warrender, This is good news

9/0FGE21YYBl8NvlCp1Ft8j1V7BiIpCIlNa/zbYwL5LWyemd/7QEu0tkVz9/f0JG
```

CBC tabanlı algoritmalarının yapısı gereği hacker'ın elinde IV yok ise, key ile sadece ilk bloğu (mesajın ilk kısmını) açamaz. Diğer blokların hepsini açabilir. kaynak: https://richardwarrender.com/2016/04/encrypt-data-using-aes-and-256-bit-keys/ "Wait... The IV is not a key!" başlığı.

Yukarıdaki cümleyi şöyle örneklendirerek anlatalım:

Şifrelerken her bloğu sırası ile şifreliyoruz:

```java
String fullPlainText = plainText1 + plainText2 + plainText3;

// Phase 1. We are creating the first block of encrypted text in this phase.
encryptedText1 = encryptFunction( xorFunction( plainText1, IV), KEY);

// Phase 2:
encryptedText2 = encryptFunction( xorFunction( plainText2, encryptedText1), KEY);

// Phase 3:
encryptedText3 = encryptFunction( xorFunction( plainText3, encryptedText2), KEY);

result = encryptedText1 + encryptedText2 + encryptedText3;
```

Şimdi ise şifrelenmiş veriyi, "KEY" kullanarak decrypt edelim:

```java
// Aşağıdaki blokların açılması işlemleri paralel yapılabilir.
// Sırası ile olmak zorunda değil. Fakat şifrelerken sırası ile yapılmak zorunda.

// Burası okunmadan önce XOR işleminin kurallarına bakılması gerekiyor şart (Başka dökümanda bu konu anlatıldı).

tempData3 = decryptFunction(cipherText3, KEY);
plainText3 = xorFunction(tempData3, cipherText2);

tempData2 = decryptFunction(cipherText2, KEY);
plainText2 = xorFunction(tempData2, cipherText1);

tempData1 = decryptFunction(cipherText1, KEY);
plainText1 = xorFunction(tempData1, IV); // In this line we need IV. Only "key" is not enough for this block.
```

## 📌 çığ etkisi (⟷ avalanche effect)

X string'in hash'i Y olsun. X string'inin bir karakterini değiştirdiğimizde ve tekrar hash aldığımızda Y ye benzer bir sonuç bekleriz, fakat çok farklı bir sonuç alırız. hash algoritmaları bunu bilerek yaparlar ki, değişiklikler daha belirgin olsun. buna çığ etkisi ismi verilmiştir.

## 📌 end-to-end encryption (⟷ E2EE)

arada farklı aracı makineler olsa da, şifrelenmiş data taşındığından ve sadece gönderen ve alıcının çözebildiği şifreleme sistemleridir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 padding

padding işlemi açık mesajın kendisine yapılır. padding Belli bir boyuta input bekleyen, cipher'lar içinde kullanılmaktadır. Örneğin blok şifrelemede; algoritma her bloğu eşit parçalara bölerek şifreler. bu durumda padding, input'un boyutuna göre şart olabilir. Blok şifrelemede açık metnin blok uzunluğunun katı olmak zorunda.

Her zaman padding kullanmak zorunda değiliz. Ama padding kullanmayacaksak; mutlaka gelen bilginin boyutunu bilmemiz gerekiyor. örneğin haberleşme protocol'lerindeki bilgilerin boyutu bellidir.

Kaç byte'ın veya bit'in padding yapıldığı bilgisi cypher tarafından dönülmez. Dolayısı ile bazı padding yöntemlerini data'mızın tipine göre seçmek gerekebilir. örneğin; binary olan data'da null karakteri olabileceğinden, aşağıdaki listede belirtilen "Pad with zero (null) characters" ile şifrelersek, bilgiyi tekrar düzgün açamayabiliriz.

padding için birçok farklı yöntem var. bazıları:

- PKCS#5

  3 adet padding varsa, "3" sayısı her padding karakteri olarak basılır.

  PKCS#5 sadece padding standartı değildir. PKCS#5 dökümanında padding'in yanında bir sürü daha standart vardır. Aynı şekilde PKCS#7'de de bu durum geçerlidir.

- PKCS#7

  PKCS#5 ile aynıdır (çok ufak bir farklılık var sadece).

- Pad with zero (null - 0x00) characters

- Pad with zero-space (0x20) characters

- OneAndZeroes

  padding'in ilk karakteri 1 (0x80), diğer tüm karakterleri 0 olarak kullanılır.

- Pad with zeroes except make the last byte equal to the number of padding bytes

## 📌 deterministic çıktı sorunu

bazı padding algoritmaları çıktının deterministik olmamasına sebebiyet vermektedir. bu konu burada tartışılmış durumda: <https://crypto.stackexchange.com/questions/66521/why-does-adding-pkcs1-v1-5-padding-make-rsa-encryption-non-deterministic>

Dolayısı ile şifre kontrollerinde padding algoritması önemli. şifre hiçbir zaman decrypt edilmeyip, sadece encrypt edip eşitliği kontrol ettiğinden dolayı, bu tarz padding'ler hataya sebep olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Java KeyStore (⟷ JKS)

- `Java`'daki `KeyStore` sınıfı veya
- `JRE` içinde gelen `keytool` komut satırı uygulaması

ile:

- `.jsk` uzantılı dosyaya (`Java` formatı)
- `.pkcs12` uzantılı dosyaya (public format. `Java`'dan bağımsız format.)
- ve daha birçoğu.

okuyup, yazabiliriz.

Bu dosyalar, private ve `public key`'lerin, sertifikaların bir arada tutulabildiği dosyalardır ve bilişim dünyasında `keyring` olarak isimlendirilirler.

## 📌 TrustStore

içerisinde `CA`'ların sertifikaları bulunduran dosyalara `Java` dünyasında verilen isimdir.

## 📌 OS keyrings

Her masaüstü uygulaması buraya `IPC` üzerinden şifre veya sertifika kaydedip, okuyabilir. Bu şifreler, için (genelde default olarak) user-login şifresi kullanılır. Bu login şifresini, `OS` kendi native popup'ında son kullanıcıya sürekli sorar.

Bazı kütüphaneler, aşağıdaki tüm keyring'leri, ortak bir API üzerinden yönetilmesini sağlıyor. Bunun için 2 farklı çözüm var:

- `Freedesktop Secret Service` standard

  `DBUS`'tan çağrılması gereken API'leri standratlaştırmış. Böylece desktop app, hangi `Linux` desktop manager'i için kod yazacağının önemi kalmıyor.

  Örnek `DBUS` path: `/org/freedesktop/secrets`

  `Gnome Keyring` ve `KeePassXC` uygulaması bu standartı implemente eden `DBUS` API'leri var. Aynı `OS`'ta her ikisini açmaya kalktığımızda hata alıyoruz. Çünkü `DBUS` path'leri aynı.

- daemon of each keyring

  Her keyring'in kendi daemon'ı var:
  
  - `KWallet`: `kwalletd`
  
  - `Gnome Keyring`: `gnome-keyring-daemon`
  
  Kütüphane bunlara ayrı ayrı istek atıyor. Hangisi ayakta olup olmadığını manuel kontrol ediyor. Aslında `OS`'ların sunduğu keyring UI'ları da, bu daemon'lara bağlanıp işlem yapıyor.

Bazı keyring'ler:

| Name                                              | Platform                                |
|---------------------------------------------------|-----------------------------------------|
| `Gnome Keyring`                                   | `gnome` and many other desktop managers |
| `KDE Wallet Manager (⟷ KWallet ⟷ KWalletManager)` | `KDE` and many other desktop managers   |
| `Keychain`                                        | `MacOS`                                 |
| `Credential Manager`                              | `MS Windows`                            |

## 📌 KDBX file format

`KDBX` dosya formatı açık kaynaklı. şifre saklamak için geliştirildi.

`KeePass`, `keepassdx`, gibi `GUI` yazılımları bu dosyaya okuyup yazıyor.

## 📌 X.509

Dosya formatı değildir. Sertifikasyon standartıdır. Şunları içerir:

- Sertifikaların neler içereceği bilgiler (sertifika imzalama algoritması, sertifika `public key`'i, versiyon, algoritma ID, seri numarası, konu, geçerlilik süresi...)

- root sertifikaları nedir?

## 📌 File formats

`X.509` standardına uyan sertifikaları saklamak için farklı formatlar kullanılabilir. Bunlardan bazıları aşağıdaki gibidir.

## 📌 File formats (additional RFCs)

Bu dökümanda anlatılan bilgiler saf format bilgisini anlatmaktadır. Fakat ilgili format için farklı/bağımsız standartlar ek özellikler sunabilir. Bu sebeple;
- `private key` saklanmayan formatta `private key` saklayabilir.
- binary olan format, base64 halinde kullanılabilir.
- şifre desteklemeyen bir formattaki dosya şifrelenebilir.

### 📌📌 file size vs key size

Aşağıdakiler farklıdır:

- `private key` veya `public key` dosyasının diskteki boyutu
- Key size (⟷ key length)

Bunun birkaç sebebi var:

- key'in saf hali haricinde, şifreleme hakkında meta-bilgilerde (version, prime numbers gibi) tutulur.
- `-----BEGIN PRIVATE KEY-----` gibi delimiter'lar olabilir.
- `private key`'in saf hali (ve varsa diğer meta-bilgiler) base64 veya farklı bir formata çevrilmiş olabilir.
- dosya, ek güvenlik olsun diye şifrelenmiş olabilir.

### 📌📌 key size vs max/min data size

Key size ile şifrelenecek data'nın boyutu arasında bir max-min ilişkisi olabilir veya olmayabilir. Bu algoritmadan algoritmaya değişmektedir.

## 📌 .PFX vs .P12

`PFX`, `Microsoft`'un formatı. `P12` ise `PKCS#12`'nin formatı. `P12`, `PFX`'in fork'u. Fakat daha sonra format üzerinde değişiklikler yapıldı mı bu konuda kaynak bulamadım. İnternette 2 formatın apaynı olduğu yazıyor. Fakat resmi kaynak bulamadım.

## 📌 Personal Information Exchange Format

Supports storage only for:

- `private key`s
- `public key`s
- certificates

## 📌 .CER vs .CRT

Supports storage only for:

- single certificate

## 📌 .p8

Standard of `PKCS#8`.

Supports storage only for:

- `private key`s

## 📌 .CRL

__Certificate Revocation List__'in kısaltmasıdır.

Designates a certificate that has been revoked.

## 📌 .CSR

__Certificate Signing Request__'in kısaltmasıdır.

This file type is issued by applications to submit requests to a `CA`.

## 📌 .DER

Supports storage only for:

- single certificate

## 📌 .spc vs .p7a vs .p7b vs .p7c

__Cryptographic Message Syntax Standard__'ıdır.

Standart of PKCS#7. Standartta 3 farklı versiyonu var: "a", "b" ve "c". Dosya isimleri ona göre oluyor.

Supports storage only for:

- certificates

## 📌 .PEM

__Privacy-Enhanced Mail__ kısaltmasıdır.

Supports storage only for:

- `private key`s
- `public key`s
- certificates

Base64 formatındadır. Bu sebeple human-readable'dır.

It use below delimiters for each information:

```text
-----BEGIN PRIVATE KEY-----
PRIVATE_KEY_IS_HERE
-----END PRIVATE KEY-----
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 PKCS

`RSA` kuruluşu tarafından yayımlanan güvenlik standartları ailesidir.

`public key cryptography standards` kısaltmasıdır.

15 adet standart içerir (yıl 2019). her sürümde farklı konular ele alınıyor. örneğin;

- PKCS#12

  bir dosya formatı standartıdır. Bu dosya formatında birçok sertifika saklanabiliyor ve bu dosyanın şifrelenmesi için de standart anlatılıyor. dosya uzantısı

  ".p12" olarak kullanılıyor. ".pfx" uzantılı olarak da saklanıyorlar fakat pfx farklı bir standart. sanırım standart yeni bir isimle çatallandı ve/veya çok az değiştirildi. bu konuda resmi bir kaynak bulamadım.

- PKCS#11

  'Cryptoki' olarak isimlendiriliyor. Bu standart sadece bir API. Token'lar üzerinde yapılabilecek işlemler için generic API sunulmuş.

Bazı sürümler:

```text
ismi       version
PKCS#1    2.2
PKCS#3    1.4
PKCS#7    1.5
```

Tüm sürümler farklı zamanlarda çıkarılmıştır. Fakat sırası ile çıkmamıştır. Bu sebeple yeni sürümler daha iyi olarak düşünülmemelidir. dikkat edilirse PKCS sürümü ile isminde bulunan numara aynı değil.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 DES (⟷ Data Encryption Standart)

simetrik bir şifreleme algoritmasıdır.

## 📌 AES (⟷ Advanced Encryption Standard ⟷ Gelişmiş Şifreleme Standardı)

Rijndael tarafından geliştirilmiştir. AES, Rijndael'ın proposal'ından ayrı olarak farklı isimde (AES ismi ile) resmileştirilmiştir.

DES artık eski kaldığı için pek tercih edilmemektedir. bu algoritma onun yerini alması için geliştirilmiştir. simetriktir.

"Key size (⟷ key length)" genelde kripto dünyasında bit bazında birime sahiptir. Bu sebeple AES-128 (⟷ AES128), 128 bitlik anahtar istemektedir (başka boyutta anahtar kesinlikle olmaz).

Wikipedia sayfasında bu şekilde notlar düşülmüş (fakat buna karşılık resmi bir kaynak bulamadım):

- Key sizes of 128, 160, 192, 224, and 256 bits are supported by the Rijndael algorithm, but only the 128, 192, and 256-bit key sizes are specified in the AES standard.
- Block sizes of 128, 160, 192, 224, and 256 bits are supported by the Rijndael algorithm for each key size, but only the 128-bit block size is specified in the AES standard.

## 📌 MD (⟷ Message Digest)

İsmi hash fonksiyonlarının çıktıları ile aynı anlama geliyor. Bu sebeple anlam karmaşası yaratabiliyor. Aslında hash fonksiyonu ailesidir. kaynak: https://docs.oracle.com/en/java/javase/11/docs/specs/security/standard-names.html title: "MessageDigest Algorithms", contains MD and SHA on the same table.

Birçok sürümü vardır.

- md2
- md4
- md5: 128 bitlik bir çıktı verir. piyasada en sık kullanılandır.
- md6

## 📌 SHA (⟷ Secure Hash Algorithm)

bir çeşit hash algoritmasıdır.

- __sha (⟷ SHA-0)__ - 160 bit (40 karakter) lik hash (çıktı) üretir.

- __SHA-1__ - 160 bit (40 karakter) lik hash (çıktı) üretir.

- __SHA-2__ - altında bir çok şifreleme barındırır. her şifreleme ürettiği hash'in boyutuna göre isimlendirilir. örnek:

  - 256'lık çıktı veren algoritma birçok isimde gösterilir: __SHA-256 (⟷ SHA 256 ⟷ SHA256 ⟷ SHA-256 bit ⟷ SHA2 256 ⟷ SHA-2 256 bit)__

  - __SHA-224__, __SHA-384__, __SHA-512__ gibi birçok örnek verilebilir.

  - Sayı ne kadar yüksekse o kadar güvenlik arttırılmış olur, çünkü kırılması o kadar zordur. çünkü; çıktının uzunluğu büyük olduğundan kombinasyonlar çok daha fazla olmaktadır.

- __SHA-3__

  sha2 gibi altında birçok şifreleme barındırır. sh2 gibi aynı şekilde; her şifreleme ürettiği hash'in boyutuna göre isimlendirilir.

  sha3, sha2 ile aynı boyutta hash çıktı boyutlarını destekler. sha3, sh2'den daha sonra çıktığından bu şekilde isimlendirmeler mevcuttur:

  - __SHA-224__ ---> SHA-2 224 bit
  - __SHA3-224__ ---> SHA-3 224 bit

## 📌 Neden hala SHA-512 yerine SHA-256 kullanılması öneriliyor?

Teorikte SHA-512 kullanılması önerilir. fakat pratikte 256'yı çözecek bir kudret olmadığından, 256 şu sebeplerden tercih ediliyor:

- 512'nin daha çok network'te bandwidth, memory, CPU vb harcıyor olması

- 256'nın yazılımlar tarafından daha çok desteklendiği için (eski olduğu için) daha az hatalı olması.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 doğum günü saldırısı (⟷ birthday attack)

öncelikle olasılık bilimindeki `doğum günü problemi (⟷ birthday problem)` konusundan bahsetmek gerekli.

bir grup insanın aynı doğum günü olma olasılığı aşağıdaki gibidir:

```text
gruptaki kişi sayısı  --> aynı doğum günü olma olasılığı
5 --> 2.7%
10 --> 11.7%
20 --> 41.1%
23 --> 50.7%
30 --> 70.6%
40 --> 89.1%
50 --> 97.0%
60 --> 99.4%
70 --> 99.9%
75 --> 99.97%
100 --> 99.99997%
```

Bu oranlar ilk bakışta insanlara tuhaf gelmektedir. Bu sebeple bu problem sıkça makalelerde söz konusu olmaktadır.

Kriptografide de, hash alarak bazı işlemler yapılabilmektedir. Örneğin, Eğer SHA-1 gibi zayıf bir hash fonksiyonu kullanılıyorsa, saldırgan 2 farklı mesajın aynı hash değerini üretmesini sağlayarak, dijital imzalarla sahtekarlık yapabilir.

birthday attack terimi de burada ortaya çıkıyor. hash çakışması olması için brute force attack yapılıyor. bu dışarıdan bakıldığında zor olasılık gözükebilir, fakat birthday problem'deki gibi yüksek olasılıkla çakışma da meydana gelebilir (eğer hash algoritması zayıf ise). işte bu benzetme buradan gelmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 JCE (⟷ Java Cryptography Extension) vs JCA (⟷ Java Cryptography Architecture)

JCE eskiden JRE dışında olan bir kütüphane grubuydu. Bağımsız geliştiriliyordu. Aynı zamanda JCA, JRE içerisinde geliştiriliyordu. İkisi benzer işleri yapabiliyordu.

JCE, JRE 1.4 ile birlikte JCA ile birleştirildi ve JRE altında geliştirilmeye devam edildi.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 "double encryption" is more secure and efficient?

Double encryption her zaman ek güvenlik sağlamayabilir. Bunun bazı sebepleri:

- 1

  bazı şifreleme algoritmalarında; bir bilgiyi:
  - K1 ve K2 key'leri ile şifreleme
  - K1 ve K2 şifrelerini yan yana koyarak şifreleme

  Yukarıdaki 2 durumda da şifrelenmiş veri aynı hızda geri çözülebilmektedir.

  Bazı algoritmalarda ise; 10 ve 5 haneli şifre kullanarak ayrı ayrı şifrelemek yerine, 11 haneli şifre kullanıp 1 kere şifrelemek daha zor kırılmasını sağlamaktadır. Örnek durum: <https://crypto.stackexchange.com/questions/6345/why-is-triple-des-using-three-different-keys-vulnerable-to-a-meet-in-the-middle>

- 2

  Şifrelerimizi (2 adet farklı şifremiz var) birbirinden bağımsız ortamlarda tutamazsak, 2 kere şifrelememiz neredeyse hiçbir şey ifade etmeyecektir.

- 3

  Kendini kanıtlamış ve stabil algoritmalar zaten matematiksel olarak 1 işlemden oluşmuyorlar. eğer yeteri derecede güçlü olduğu düşünülmüyorsa, işlem sayısı uzatılıyor (veya ona göre algoritmada ek geliştirme yapılıyor). yani; örneğin X algoritması, arka planda A ve B algoritmalarına benzer bir mantık ile üst üste şifreleme yapıyor olabilir. Dolayısı ile; hem A hem de B ile 2 kere şifreleme yapmanın, X algoritmasından daha güvenli olacağını (veya tersini) söylemek çok aşırı detaylı bir çalışma ister.

  Nihayetinde stabil ve kendini kanıtlamış algoritmalar geliştirildiğinde, piyasada var olan diğer algoritmalar da incelenip, geliştirilmektedir. Yeni algoritma dünyadaki bir çok bağımsız otorite tarafından onaylanıyor ise, çift şifrelemeye gerek duyan eksik bir geliştirmiş olma ihtimali çok düşüktür.

- 4

  Eğer 2 kere şifreleyeceksek, 2 farklı algoritma kullanmak mantıklı olacaktır. Tabi bu algoritmaların altında yatan logic'lerinde birbirinden çok farklı olduğunu da araştırmış olmamızda yarar var. Benzer temele dayanan algoritmalarla şifrelemek yukarıda yazan diğer maddelere takılmamıza sebep olacaktır.

  2 kere şifreleme ek proje geliştirme süreçleri doğuyor:

  - algoritma temellerini araştıracağımız vakit
  - 2inci şifrelerin farklı ortamlarda tutmak mantıklı demiştik. bu sebeple bu ortamların belirlenmesi, orada şifrelerin nasıl saklanacağı, ve algoritmayı çalıştıran sisteme nasıl taşınacağının araştırılması vakti (ve tabi ki ilerdeki zamanlarda bu geliştirmelerin bakım maliyeti olacak).

  Yukarıda harcanan vakit yerine; 1 kere şifreleyip, güvenlik sistemimizi best-practice'lere göre daha uygun yapabilmek daha kısa sürebilir ve hatta daha güvenli kalabilmemizi sağlayabilir.

### 📌📌 Ek not

iki kere aynı bilgiyi şifrelemek performansı da olumsuz etkilemektedir. Özellikle Whatsapp gibi end-to-end encryption kullanan uygulamalarda, anlık/canlı kameralı arama yapma gibi işlemlerde yavaşlık yaratabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
