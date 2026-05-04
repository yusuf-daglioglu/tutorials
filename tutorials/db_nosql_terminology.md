# DB NOSQL TERMINOLOGY

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 NoSQL (⟷ Not Only SQL ⟷ non-relational SQL ⟷ non-SQL) vs Relational DB Management System (⟷ RDBMS ⟷ İlişkisel veri tabanı yönetim sistemi)

ilişki olmamasından yola çıkılarak optimize edilmiş `DB`'lerdir. fakat bazı `DB`'ler ilişki destekler. örnekler:

- `MongoDB`'deki `DBRef` özelliği.
- tüm graph DB'ler (örnek `neo4j`).

Dolayısı ile aslında olayın çıkış noktası ilişki olmaması değildir. olsa da, ilişki kurup, daha farklı özelliklerden feragat ederek optimize edilen `DB`'leri da `NoSQL` grubunun içindedir.

piyasada `NoSQL`'lerin scalable olduğu söylenir. Oysa `RDBMS`'lerde scalable'dır. Fakat `RDBMS`'in scalable olmasını zorlaştıran etkenler çok daha fazladır. en önemli sebeplerden biri, distributed transaction lock ve foreign key özellikleridir. Çünkü bu özellikler tüm cluster'daki tüm node'lara aynı anda bilgi iletimi gerektirir. Dolayısı ile scalable sistemde yeni node ekleme çıkarma gibi işlemlerde, runtime sırasında binlerce farklı satırda lock olduğunu düşünürsek işimizi zorlaştığını göreceğiz. Aslında:

- `RDBMS`'lerde `Horizontal Scaling (⟷ Yatay Ölçeklendirme)` `NoSQL`'e göre zordur (imkansız değil).

- `Vertical Scaling (⟷ Dikey Ölçeklendirme)` ise hem `RDBMS` hem de `NoSQL`'lerde aynı komplekslikte olduğunu söyleyebiliriz. Çünkü sadece tüm sütunları okumak gerektiğinde hem `NoSQL` hem de `RDBMS` bu 2 node'a erişmek durumundadır. Eğer sadece 1 node'daki tüm sütunlar okunacak ise yine 2 DB'de sadece 1 node'a ihtiyaç duyacaktır.

## 📌 NoSQL tipleri

## 📌 Document-Oriented store

her bir nesneyi, ayrı birer fiziksel döküman olarak saklar. `kaynak: https://docs.microsoft.com/en-us/azure/architecture/data-guide/big-data/non-relational-data title: "Document data stores", 2th paragraph.`

örnek sistemler: `MongoDB`.

## 📌 Key-Value store

Bilgiler bir anahtar ve karşılığında bir değer şeklinde saklanıyor. genelde çok fazla işlem ama ufak verileri saklamak için kullanılan sistemlerde kullanılıyor (örneğin; caching).

Key'e karşılık gelen data, `Document-Oriented store` gibi aynı fiziksel dosyada değildir.

En kritik noktalardan biri sadece key ile value çekilebilmesidir. Bu sebeple `SQL`'deki `WHERE` koşulu imkanı sunulmaz. Sunulsa da çok yavaş arama yapacaktır. `kaynak: https://docs.microsoft.com/en-us/azure/architecture/data-guide/big-data/non-relational-data title: "Key/value data stores", 5th paragraph.`

örnek sistemler: `Redis`.

## 📌 Graph store

`RDBMS`'le karşılaştırarak gidersek aşağıdaki temel farkları görürüz:

- `Graph DB`'lerde, join-table ihtiyacı olmamaktadır. Çoklu ilişkiler, direk olarak asıl entity'nin kendisinde tanımlanabilmektedir.
- `Graph DB`'lerde, `RDBMS`'den farklı olarak, foreign-key'lere özel nitelikler atanabilmektedir. Örneğin; Öğrencinin öğrenim gördüğü okulları referans eden foreign-key'de, `yıl` da yer almaktadır. Bunu `RDBMS`'te yapmaya kalksak şu sütunlarla tablo yaratmamız gerekmektedir:
  
  `student_id`, `student_name`, `school_2010`, `school_2020`

  Oysa `graph DB`'lerin doğasında her foreign-key için bir nitelik belirtme imkanı mevcut.

- Aşağıdaki 2 sebepten ötürü `graph DB`'ler kendi query syntax'larını sunarlar:

  - `Graph DB`'lerde join-table ihtiyacı olmadığı için query'ler çok kısalmaktadır ve farklılaşmaktadır.
  - `Graph DB`'ler çok fazla join yapılacak query'ler üzerinde çalışmalara göre optimize edildiği için, `SQL`'de olmayan özel (kolaylaştırılmış) hazır query'ler sunar. örnek: Oda tablosu olduğunu düşünelim. Bu odaların bazıları birbirlerine bağlı (kapı girişleri birbirlerine bakıyor). Biz hem `SQL`'de hem de graph query'leri ile birbirine 1inci dereceden bağlı odaları kolayca bulabiliriz. Fakat ek olarak; `graph DB`'lerin query'leri, 2inci veya n'inci dereceden odaları da kolayca döndüren özellikler sunabilmektedir. Aynı zamanda eğer döngüsel bir durum olduğunda hata vermeyip, sonsuz döngüye sokmadan response dönebilmektedir gibi.

örnek sistemler: `Neo4J`, `Giraph`.

## 📌 others which includes "column" word

bu gruba giren örnek sistemler: `HBase`, `Cassandra`.

burada terminoloji karmaşası söz konusudur. Bu kaynakta yorumları da dahil olmak üzere çok verimli bilgiler mevcut: http://dbmsmusings.blogspot.com/2010/03/distinguishing-two-major-types-of_29.html

`column` terimi içeren `NoSQL` terimleri aşağıda açıklanmıştır. Bu terimler piyasada sıkça birbirleri yerine kullanılmaktadır. Fakat genel olarak `column` terimi içeren `DB` sistemleri her sütuna (veya sütun grubuna) ayrı ayrı (tüm diğer sütunları okumadan) erişebilmemizi sağlar.

## 📌 column-oriented

bu terim `RDBMS` dünyasında kullanılan bir terimdir. Fakat çoğu makalede bu terim `NoSQL`'ler için de kullanıldığından karışıklığa sebep olmaktadır.

## 📌 column-store

`column-store` bir sütun'a ait (belli range'lerdeki veya tüm) bilgilerin fiziksel olarak ard arda kayıtlı olduğu sistemlerdir. daha doğrusu; her hücrenin hangi satıra ait olduğu bilgisinin direk olarak store'a yazılmayan sistemlerdir. bu tarz sistemlerde bir hücrenin hangi satır'a ait olduğunu farklı hesaplamalarla bulabiliriz. `column-store` fiziksel olarak data'yı bu şekilde kaydeder:

```text
(ID) 1, 2, 3, 4, 5, 6
(First Name) Joe, Jack, Jill, James, Jamie, Justin
(Last Name) Smith, Williams, Davis, Miller, Wilson, Taylor
(Phone) 555-1234, 555-5668, 555-5432, NULL, 555-6527, 555-8247
(Email) jsmith@gmail.com, jwilliams@gmail.com, NULL, jmiller@yahoo.com, NULL, jtaylor@aol.com
```

`column-store`'lar her sütunu ve içindeki bilgileri farklı dosyalarda saklar. Dolayısı ile sadece tek formatta (`String`, `int` gibi) veri tuttuğu için compression gibi birçok optimizasyon sunar.

Yukarıdaki şekli bir tablo gibi düşünürsek, artık ID'ler bizim için sütun, `first name` ve `last name` bizim için satır gibi olmuş oluyor. Bu sebeple `column-store` `DB` denir. Çünkü `RDBMS` sistemler `row-store` olarak ifade edilebilir.

Örneğin `Cassandra`, `column-store` değildir. zaten resmi `Github` sayfasında `partitioned row-store` olduğu yazar. çünkü eğer bir hücreye erişmek istiyorsak; `Cassandra`, önce row-key'i buluyor, sonra o satırdan istediğimiz column-name'i olup, ilgili sütunun değerini buluyor. Zaten `Cassandra` her sütunu ayrı dosyada tutmuyor. 1 tabloyu tümüyle aynı dosyada tutuyor (bu durum `column family` başlığında anlatılıyor).

## 📌 column family (⟷ columnar data store)

`columnar` türkçe kelime anlamı: `sütunsal`, `sütun biçiminde`, `sütunlu`.

`ar` suffix'i, `tabular` kelimesindeki gibi kullanılıyor. `tablular`, `table` kelime kökünden geliyor ve `tablosal (veya tablo biçiminde)` anlamına geliyor.

`column family` `DB`'de, `RDBMS`'teki tablo, `column family`'ye denk gelmektedir. `column family` yapısında developer'ın sorgudan döndürmek istediği her sütun için farklı tablo yapması beklenir. işte `one table per query pattern` buradan gelir. bu sebeple `tablo` yerine `column family` terimi kullanılır. yani 1 tablo, bir column grubuna denk gelir. `family` suffix'i buradan gelir. family suffix'i; bir nevi `column group` terimine denk gelir.

Bu kısmı farklı bir dille açıklamak gerekirse: `user_by_name` ve `user_by_id` tablomuz olsun. `RDBMS`'te bunları tek 1 tabloda tutardık. `Column family` yapısında ayrı ayrı tutuyoruz. İşte bu sebeple sütunları family'lere ayrılmış varsayıyoruz. terim buradan geliyor.

`Column family` yapısında fiziksel olarak, her `column family` data tek bir dosyada tutulur. Tabi bu dosyada tüm satırlar değil, belli aralıktaki satırlar tutulur. Yani birçok dosya birbirini tamamlar. Fakat bunun avantajı şudur: Bir data çekmeye çalıştığınızda, sadece ilgili sütunların olduğu dosyayı okumak yeterli olmaktadır. Yani sadece `column family`'deki sütunları okumaktayız. `kaynak: https://docs.microsoft.com/en-us/azure/architecture/data-guide/big-data/non-relational-data title: "Columnar data stores", 5th paragraph`

dosyanın logical yapısı bu şekildedir:

```text
              column-A      column-B
row-key-1     value-1       value-2


              column-A      column-C
row-key-2     value-3       value-4
```

`table` ile `column family` karşılaştırıldığında, fiziksel olarak data tutuş biçiminin fark etmemesini bekleriz. fakat ikisi arasında şu farklar söz konusu:

- kavramsal ve logical farklılık (birinde denormalized ve duplicated data olabilirken, diğerinde bu olmamalı)
- veriye erişimde, genelde farklı öncelikler tercih edildiği için, data'nın tutuş biçimi structural olarak farklı optimize ediliyor. örneğin `column family` mantığında (genelde) data fiziksel olarak sorted oluyor. tablo ile karşılaştırıldığında, sorted olması gibi optimizasyonlar/farklılıklar söz konusu olabiliyor. RDBMS'te de 1 sütuna göre index'leme yapabiliyoruz fakat RDBMS'te data fiziksel sorted olmuyor.

Cassandra dünyasında eskiden `column family` terimi kullanılırdı. fakat daha `CQL`'e geçiş ile `tablo` terimi kullanılmaya başlandı. fakat Cassandra hala `column family` bir `DB`'dir.

## 📌 wide-column store (⟷ extensible record store)

`column family`'nin türevidir diyebiliriz.

`wide-column` sistem; her cell'in (hücrenin) value'su için farklı sürüm değerleri saklar. Boyutlar şunlardır: Satır, sütun ve zaman. Bu 3'ü ile ancak data'ya erişebiliriz. Oysa 2 boyutlu sistemlerde data'ya erişmek için satı ve sütun bilgisi yeterlidir.

Bu sebeple;

- `wide-column` store'lar `three-dimensional` (tabi aynı zamanda `multi-dimensional`) olarak adlandırılır.
- `document-based` store'lar ise `two-dimensional` olarak nitelendirilirler. çünkü `document-store`'da her column'un farklı sürümlerini saklayamayız (`JSON` formatını düşünürsek bunu hayal edebiliriz - Aslında bu yapılabilir fakat JSON'da bunu yapmak çok verimsiz olacaktır).
- `RDBMS`'ler `two-dimensional`'dır. (yukarıda yazan sebepten)

`Wide-column` bir store'u görselleştirmeye çalışırsak:

```text
              column-A    column-B@01-01-2001    column-B@02-02-2002    column-Y
row-key-1     value-1     value-2                value-3                value-4


              column-A    column-B@01-01-2001    column-B@03-03-2003    column-X
row-key-2     value-5     value-6                value-7                value-8
```

## 📌 Time series data store

primary-key'lerin time olduğu DB'lerdir. yüksek hızda append-only'dirler. update olabilir ama verimli değildir.

`Time series data store`'da metric datası tutumak çok verimli.

Log datası tutmak verimli değil, çünkü `full-text search` olması da gerekir.

trace datası graph store'da tutmak mantıklı.