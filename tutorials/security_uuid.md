# SECURITY UUID

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 UUID (⟷ Universally unique identifier ⟷ GUID Globally Unique IDentifier)

`UUID`, `rfc 4122`'da `(source-id: 302)` belirtilen standarttır.

## 📌 versions

versiyonlar olgunluk değildir. çeşittir.

### 📌📌 version 1 (Time Based)

cihazdaki `MAC` adresi ve data-time (en ufak birimi nanosecond olacak şekilde) bilgisi ile UUID üretir. aynı bilgilerle aynı UUID tekrar üretilebileceğinden bazı durumlarda takip edilebilirliği sağlamak amaçlı bu kullanılmaktadır. Bu log gibi basit sistemler için güzel, ama güvenlik/gizlilik gerektiren yerlerde kullanılmamalı. Çünkü: bu string parse edilebiliyor. Yani version-1 UUID'yi alıp parse edersek içinde şunlar var:

- UUID genel metadata'ları (version bilgisi gibi)
- mac adresi
- date-time
- ufak bir random sayı (aynı mac adresleri cihazdan aynı anda UUID üretilirse çakışmayı engellemek için).

Bu sebeple UUID date-time değerine göre sıralanabilme avantajı sunuyor (örneğin, DB'de index'lenebiliyor).

mac adresi olmayan durumlarda (cihazlarda/platformlarda) rastgele sayı kullanılabileceği RFC standartlarında belirtilmiştir.

### 📌📌 version 2 (DCE Security)

### 📌📌 version 3 (Name Based)

UUID oluşturmak için bir çeşit namespace (örneğin: URL, DNS, SSL Sertifikası) ve name'in MD5'i alınır, sonra bu MD5'ten UUID üretilir. örnek kod:

```java
String source = namespace + name;
byte[] bytes = source.getBytes("UTF-8");
UUID uuid = UUID.nameUUIDFromBytes(bytes);// nameUUIDFromBytes fonksiyonu, UUID üretmeden önce MD5 hash alıyor.
```

Bazı öntanımlı namespace'ler var. örnekler:

- DNS: 6ba7b810-9dad-11d1-80b4-00c04fd430c8
- URL: 6ba7b811-9dad-11d1-80b4-00c04fd430c8
- X.500: 6ba7b814-9dad-11d1-80b4-00c04fd430c8

Bunu kullanırken şöyle olacak:

Web sitemiz için UUID version 3 üretecek isek:

- namespace: 6ba7b811-9dad-11d1-80b4-00c04fd430c8 (yukarıdaki öntanımlı URL değeri)
- name: my-web-site-domain.com

Oluşturulan UUID, version 1'deki gibi parse edilebilir değil.

OLuşturulan UUID'nin içinde hiçbir random değer yoktur. Bu sebeple aynı namespace ve name değerleri sürekli aynı UUID çıktısını verir.

Gerçek hayat kullanımı: Her user'ın email adresinden bir UUID üretebiliriz. Bunları DB'de UUID formatında saklayabiliriz.

### 📌📌 version 4 (Random)

sistem random number generate ederken, önceden belirli data'lardan yararlanmaz. bu sebeple genelde bu tercih edilir.

### 📌📌 version 5 (Name Based)

Version 3 ile aynı. tek fark: MD5 değil, SHA-1 istiyor.

SHA-1, MD5'e göre daha kırılamaz gibi görünse de, günümüzde SHA-1 artık güvenli kategorisinde değildir.

## 📌 variant

UUID'nin ilk 3 bitidir.

UUID'nin layout'udur. hangi bitlerde, hangi bilginin yer aldığı standartıdır.

`RFC 4122`'de sadece bir varyant anlatılıyor.

("x" önemi olmayan bit anlamına geliyor)

| bit-0 | bit-1 | bit-2 | explanation                                            |
|-------|-------|-------|--------------------------------------------------------|
| 0     | x     | x     | Reserved, `NCS` backward compatibility                   |
| 1     | 0     | x     | `RFC 4122`'de anlatılan formattır                         |
| 1     | 1     | 0     | Reserved, `Microsoft` backward compatibility |
| 1     | 1     | 1     | Reserved for future definition.                        |

`Leach-Salz` (`RFC 4122` yazarlarının soyadları) `UUID`'nin 2'inci varyantının (digit olarak bakıldığında varyant değeri "1") takma adı olarak birçok makalede kullanılmaktadır.

UUID ilk olarak NCS'de kullanılıyordu. Daha sonra Microsoft firması kullanıldı. Daha sonra RFC yayımlandı. Bu sebeple NCS ve Microsoft'un kendi layout'larına geriye uyumluluk için RFC'de yer verildi.

Microsoft kendi dökümanlarında RFC yayımlanana kadar GUUID terimini kullanmıştır. Bu sebeple GUUID'nin Microsoft'un standardı olduğu yanılgısı vardır. Oysa ikisi aynıdır. Aşağıda belirttiğim istisna durum için ise hiçbir resmi yayım bulamadım.

Ek not: `Microsoft` resmi sitesinde` <https://docs.microsoft.com/en-us/windows/win32/msi/guid> şu 2 cümleyi yazmaktadır:`

- `The valid format for a GUID is {XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX} where X is a hex digit (0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F).`
- `Note that utilities such as GUIDGEN can generate GUIDs containing lowercase letters. These must all be changed to uppercase letters before the GUID can be used by the installer as a valid product code, package code, or component code`

Oysa bu 2 madde `RFC`'de yazan bilgiler ile çakışıyor. Çünkü versiyon ve variant kısmında her karakter olamaz. Ve büyük harfe çevirme zorunluluğu vardır. Buda `Microsoft` tarafında üretilen bir UUID'nin kendi sistemimizde parse edilememesine yol açabilir. Fakat bu döküman çok yüzeysel görünüyor, muhtemelen de `Microsoft` tarafındaki kod implementasyonlarında `RFC` dikkate alınmıştır diye düşünebiliriz fakat burada bu konunun hataya sebep olduğu ve `Microsoft`'un `RFC` dışında farklı data ürettiği görülmektedir: (`source-id: 478`) Bu bug içerisinde `Microsoft` teknolojileri ile üretilmiş UUID'nin bir kütüphane tarafından parse edilemediği ve bu konunun çözülmediği görülmüştür.

`Java` code of version vs variant

```java
UUID uuid = UUID.randomUUID();
int variant = uuid.variant();
int version = uuid.version();
```

## 📌 çakışma

`UUID` kümesi çok büyük olduğundan, aynı UUID'nin sadece aynı uygulama içinde veya farklı bir sistemdeki bağımsız uygulamada üretilmesi matematiksel olarak çok azdır. bu sebeple standart olarak kullanılır.

version 4 UUID'den ortalama `5*(10^17)` adet vardır. Bu da ortalama 80 sene boyunca, her saniye 1 milyar `UUID` üretilmesi durumunda, %50 ihtimalle 1 kere çakışma yaşanacağı oranı yaratmaktadır.

## 📌 real world use case

- fiziksel network cihazlarının `MAC` adresleri `UUID`'dir. (tabi `VM`'lerde de aynı şekilde böyle)
- dosya sistemlerinin partition-ID'leri `UUID`'dir.
- DB'lerde auto generated primary key'ler `UUID` olabilir.
- `UUID`'nin aynı olma olasılığı olmadığından dolayı, client tarafta daha insert(persist) edilmemiş DB entity'sine Client'ın kendisi de `UUID` verip, daha sonra DB'ye yollayabilir.

## 📌 notation and format

- gösterirken sadece küçük veya büyük harf olması önemli değildir.

- Tekil bir string'dir. 128 bittir = 32 karakterdir = 16 byte'dir.

- Alabildiği karakterler: 0-9 ve a,b,c,d,e,f (Aslında hex digit'i temsil ettiği için bu karakterleri alabiliyor. kendisi sadece 16-tabanında bir sayı.)

- 36 karakterdir ve arada tire koyularak (tireler sadece notasyondur, 128 bite dahil değildir) 5 grup olarak gösterilir:

  > 123e4567-e89b-12d3-a456-426655440000

## 📌 Uniform Resource Name (⟷ URN) notation

> urn:uuid:123e4567-e89b-12d3-a456-426655440000

## 📌 Nil UUID

tümü sıfır olan UUID'dir.
