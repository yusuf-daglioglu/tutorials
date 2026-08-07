# TOGAF

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 TOGAF (⟷ The Open Group Architecture Framework)

## 📌 sertifika programları

`togaf` `open grup` tarafından geliştirliyor. farklı sürümleri var.

her sürümün 2 farklı sınavı var. level 1 ve 2. level 2 daha kapsamlı.

`Certification` tablosu:

| `Sertifika`                                  | `Açıklama`             | `Hedef`               |
|----------------------------------------------|------------------------|-----------------------|
| `TOGAF 9 Foundation`                         | `TOGAF` `9.2`          | Level 1               |
| `TOGAF 9 Certified`                          | `TOGAF` `9.2`          | Level 2               |
| `TOGAF Enterprise Architecture Foundation`   | `TOGAF` 10             | Level 1               |
| `TOGAF Enterprise Architecture Practitioner` | `TOGAF` 10             | Level 2               |
| `TOGAF Business Architecture Foundation`     | Ayrı bir uzmanlık yolu | Business Architecture |

`TOGAF`, `Credential` (ek uzmanlık rozeti) sunan standartlar da sunuyor. bunlar opsiyonel ek sertifikalar. `Business Architecture Foundation`, `Credential` grubu altında değilde, `Certification` grubu içerisinde tanımlanmış, çünkü başlıca bir disiplin gibi görülüyor. oysa `Credential`'lar ek bilgi olarak görülüyor.

`togaf` sınav dışında da genel olarak kitaplar yayınlıyor. o bilgilerin hespi sınavda sorulmuyor. sınavın kapsamı küçük (`open group`'un yayımladığı kitaplar ekstra bilgiler içeriyor).

## 📌 organizasyon şeması

- C levels

  - `CEA (⟷ Chief Enterprise Architect)`

    - `arcihtecture team`

      - `Lead architect` (who works as a `Sponsor` for each Project)

      - `Business Architect`

      - `Data Architect`

      - `Application Architect`

      - `Technology Architect`

    - `Ar-ge`

    - `PMO (⟷ Project Management Office)`

  - `Architecture Governance Board`

  - `HR`

  - Sales

  - `IT`

`pmo`: sadece proje yönetimi süreci takibi yapar. projenin sadece işletilebilir sürece gelene kadarki süreç takibi. karar alma bu ekipte değil.

`pmo` `arcihtecture team`'e istek geldiği anda takibe başlar.

`ar-ge team`: organizasyonda olmayan konuların şirkete öneri sunulması.

`arcihtecture team`: günlük operasyonel işlerden bağımsız çalışmalı. ayrı birim. zaman kaybetmemeli ufak detaylarla. bu sebeple `HR`, `IT` takımlarından bağımsız bir kutuya çizildi.

`arcihtecture team` içinde her domain (`HR`, `IT`) için ayrı olmalı. yani her domain'in `arcihtecture team`'i olabilir.

`Chief Enterprise Architect`: 1 kişidir. birimin başındadır.

`Lead Architect` ise 1 adet projenin başındadır.

## 📌 mimarinin 4 architecture domain'leri

`arcihtecture team`, şirket çalışan sayısının %2-4 arasında (önerilen rakam bu) ve en tecrübelilerin olduğu ekip olursa, tüm şirket her önemli kararları buraya sorabilir.

- `business architech`

  Organizasyon yapısı, Stratejik hedefler, iş akışlarını bilen kişiler - şirket içi ve son kullanıcı süreçlerini bilen.

- `data architech`

  şirketteki veritabanı veri modellerini bilir.

- `application architech`

  örnek mobil uygulama, entegrasyonlar gibi application'ların hangi süreçleri kapsadığını bilenler.

- `technology architech`

  `IT` architecture'ını bilenler.

### 📌📌 business architech vs application architech

`business architech`, Business Capability ile ilgilenirken (neyin yapılıp neyin yapılamayacağını bilirken), `application architech` bunun implementasyonlarını bilir. örneğin; `business architech` ödemenin aldığını bilir, `application architech` bunun payment `vpos` kullanılarak web'den yapıldığını bilir. bunun tam teknik sürecini ise `technology architech` bilir.

## 📌 Knowledge Levels

### 📌📌 Seviye 1 - Awareness (Farkındalık)

Konuyu duymuş, temel kavramları biliyor.

### 📌📌 Seviye 2 - Foundation (Temel)

Kavramları ve ana yapıyı anlıyor; temel soruları cevaplayabilir.

### 📌📌 Seviye 3 - Practitioner (Uygulayıcı)

Bilgiyi gerçek projelerde kullanabilir, yöntemleri uygulayabilir.

### 📌📌 Seviye 4 - Expert (Uzman)

Derin bilgiye sahip; tasarım yapabilir, yönlendirebilir, karar verebilir.

## 📌 proje vs program

program birden fazla projeyi kapsar.

## 📌 enterprise continium

repository'daki her dökümanın sınıflandırılır. bu sınıflar `togaf` tarafından sabit belirlenmiştir. bu sınıf modeline `enterprise continium` adı verilir. çok yüzeysel olarak:

- `Foundation`

  bu sınıftaki örnek dökümanlar: `TCP/IP`, `HTTP`

- `Common Systems`
  
  bu sınıftaki örnek dökümanlar: Kimlik doğrulama (`SSO`) mimarisi

- `Industry`

  bu sınıftaki örnek dökümanlar: Bankacılık referans mimarisi

- `Organization-Specific`

  bu sınıftaki örnek dökümanlar: `ABC` Bankası'nın kendi hedef mimarisi

istersek kendi sub-label'larımızı yaratabiliriz.

## 📌 building blocks

şirkette mimarileri gösteren modellemelerdir. bunlar şirkette tekrar kullanılabilen modellerdir. bunlar her dökümanda linklenerek gösterilmelidir. yoksa dublikasyonlar olur.

İki türü vardır:

- `ABB (⟷ Architecture Building Block)`

  Ne gerektiğini tanımlar.

  Örnek: Kimlik Doğrulama Servisi, Müşteri Verisi, Ödeme Süreci.

- `SBB (⟷ Solution Building Block)`

  Nasıl gerçekleştirileceğini tanımlar.

  Örnek: `Keycloak`, `PostgreSQL` Customer `DB`.