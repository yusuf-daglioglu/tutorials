# DB ELASTICSEARCH

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 ElasticSearch

`elastic`, yazılımı geliştiren firmanın adı. firmanın eski ismi `ElasticSearch`'tü.

`ElasticSearch` document based (`MongoDB` gibi) data storage'tır. JSON formatında saklar. fakat dökümanın her parçasını index'ler.

bu index'leme tekniği birçok arama motoru kullanır ve buna `inverted Index` denir. örneğin;

3 adet kaydımız olsun:

```text
döküman numarası - içeriğindeki kelimeler
1- a b c
2- x y z
3- a x
```

`Elasticsearch` şu şekilde index tutar:

```text
a -> 1, 3
b -> 1
c -> 1
x -> 2, 3
y -> 2
z -> 2
```

`x` terimini arattığımızda direk 2 ve 3üncü `document`'lerde olduğunu bulmamızı sağlar.

`inverted index` teriminin tersi `Forward Index` terimidir. yani her döküman sadece `tokenizing` edilir. index'lenmiş olur. performansa tek katkısı zaten `tokenizing` edilmiş olmasıdır. parse edilmesine gerek yoktur.

`inverted Index` kabaca yukarıdaki gibi bir matris tutar. spesifik implementasyonları bakarsak farklı bilgiler de içerebilir. örnekler:

- her kelimenin dökümanda kaç kere yer aldığı bilgisi
- kelimenin dökümandaki pozisyonu

`ElasticSearch` dışarıya `REST` `API` sunar, bu dilden bağımsız sorgu atabilmemizi sağlar.

### 📌📌 shard

kelime anlamı: parça

`Elasticsearch`'in tuttuğu her index tek bir makinede tutulmayabilir. onu partition'lara ayırabiliriz. ayırdığımız her partition'a shard denir.

1 `node`'a, `shard` dememek gerekli. çünkü 1 `node`, 1'den fazla `shard` içerebilir ve aynı zamanda `replica` görevi de görebilir.

### 📌📌 replica

her `shard`'ın tutulacağı `replika` `node`'lara verilen isimdir.

### 📌📌 index

`RDBMS`'deki tablo'ya denk gelen kavramdır. en üst seviye bilgi grubudur. her index'e bir alias atılmak zorunda değildir. fakat atılması mutlaka önerilir. bir sistemde min 1 index olmalıdır.

### 📌📌 Indices (⟷ indexes)

`index` teriminin çoğul hali olduğundan, `RDBMS` server'larındaki, DB'ye denk geliyor diyebiliriz. çünkü; birden fazla index yani tablo bir DB'i tanımlar.

### 📌📌 Mapping

`RDBMS`'deki şemaya denk gelir. istenirse tipler önceden mapping ile belirtilebiliyor.

fakat ElasticSearch yeni bir kayıt eklendiğinde verinin yapısını zaten tespit ediyor.

### 📌📌 Document

`RDBMS`'deki tablo içindeki kayda (row) denk gelir.

### 📌📌 Type

Document'in tipini belirtir. user, tweet tipinde farklı document'ler olabilir. `RDBMS`'deki tablo'ya denk gelir.

### 📌📌 field

`RDBMS`'deki her tablo içinde olan bir sütuna denk gelir. `Elasticsearch`, `NoSQL` olduğu için `JSON` tutar ve her field bir JSON property'dir.

### 📌📌 kayıt eklemek

`ElasticSearch` önceden şema istemediğinden attığımız yeni kaydı direk kabul eder ve bundan arama yapılabilmesini kendisi sağlar. yeni kaydımızı Rest API'si ile ekleyelim:

> PUT ELASTIC_SEARCH_URL/customer/_doc/1

```json
{
  "name": "John Doe"
}
```

dönüşü:

```json
{
  "_index" : "customer",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 2,
    "failed" : 0
  },
  "_seq_no" : 26,
  "_primary_term" : 4
}
```

şimdi kaydımızı çekelim:

```
GET /customer/_doc/1
```

dönüşü:

```json
{
  "_index" : "customer",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "_seq_no" : 26,
  "_primary_term" : 4,
  "found" : true,
  "_source" : {
    "name": "John Doe"
  }
}
```

Birçok döküman index'lenecek ise, tek tek yerine performans açısından `_bulk` `API`'si tercih edilmelidir.

Birçok kayıt index'lemiş olalım. Şimdi bu kayıtlarımızı arayalım:

```
GET /bank/_search
```

```json
{
  "query": { "match_all": {} },
  "sort": [
    { "account_number": "asc" }
  ]
}
```

dönüşü:

```json
{
  "took" : 63, //how long it took ElasticSearch to run the query, in milliseconds
  "timed_out" : false, // request timeout or not.
  "_shards" : { // how many shards were searched and a breakdown of how many shards succeeded, failed, or were skipped
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
        "value": 1000,
        "relation": "eq"
    },
    "max_score" : null,
    "hits" : [ {
      "_index" : "bank",
      "_type" : "_doc",
      "_id" : "0",
      "sort": [0],
      "_score" : null,
      "_source" : {"account_number":0,"balance":16623,"firstname":"Bradshaw","lastname":"Mckenzie","age":29,"gender":"F","address":"244 Columbus Place","employer":"Euron","email":"bradshawmckenzie@euron.com","city":"Hobucken","state":"CO"}
    }, {
      "_index" : "bank",
      "_type" : "_doc",
      "_id" : "1",
      "sort": [1],
      "_score" : null,
      "_source" : {"account_number":1,"balance":39225,"firstname":"Amber","lastname":"Duke","age":32,"gender":"M","address":"880 Holmes Lane","employer":"Pyrami","email":"amberduke@pyrami.com","city":"Brogan","state":"IL"}
    },
    //...
    ]
  }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Nested objects

`ElasticSearch` stores:

- the inner (nested) `JSON` objects as different (separate) documents. The data type of these documents are nested type. The standard document type is `object`.

```json
PUT my-index-000001
{
  "mappings": {
    "properties": {
      "user": {
        "type": "nested"
      }
    }
  }
}

PUT my-index-000001/_doc/1
{
  "group" : "fans",
  "user" : [
    {
      "first" : "John",
      "last" :  "Smith"
    },
    {
      "first" : "Alice",
      "last" :  "White"
    }
  ]
}

GET my-index-000001/_search
{
  "query": {
    "nested": {
      "path": "user",
      "query": {
        "bool": {
          "must": [
            { "match": { "user.first": "Alice" }},
            { "match": { "user.last":  "Smith" }}
          ]
        }
      }
    }
  }
}
```

- the arrays as a flattened format.

```json
PUT my-index-000001/_doc/1
{
  "group" : "fans",
  "user" : [
    {
      "first" : "John",
      "last" :  "Smith"
    },
    {
      "first" : "Alice",
      "last" :  "White"
    }
  ]
}
```

It stores internally as:

```json
{
  "group" :        "fans",
  "user.first" : [ "alice", "john" ],
  "user.last" :  [ "smith", "white" ]
}
```

We can query using:

```json
GET my-index-000001/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "user.first": "Alice" }},
        { "match": { "user.last":  "Smith" }}
      ]
    }
  }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 ElasticSearch vs Cassandra vs MongoDB

`Elasticsearch` bir search ve analytics engine'i iken, diğerleri `NoSQL` olarak geçmektedir. 3 alternatif yazılım data'ları `NoSQL` olarak diske kaydedip, bunları client'a sunsalar da, tutuş biçimleri performans önceliklerine göre değişmektedir.

`MongoDB`, generic-purpose bir `NoSQL` `DB` olarak kullanılır. istediğimiz sütunları index'leriz. `Cassandra`'da ise, sorgularımız baştan belli olmalıdır ve daha incremental data'ların kaydı için kullanılır. `ElasticSearch` ise tüm sütunları (field'ları) ve field'ların içindeki data'ları sürekli index'ler. bu şekilde tüm data'larda hızlı arama yapılabilmesini sağlar. fakat bu durum insert işlemlerini yavaşlatır, çünkü her insert işleminde index'leme çalışması şarttır (ve tüm field'lar index'lenmek zorundadır).

`ElasticSearch` `document store` olarak kayıt tutmaktadır.

`ElasticSearch` ek olarak `full-text search` destekleyen bir storage'dir. `full-text search` teriminin kapsamı keskin çizgilerle çizilmemiştir. `SQL`'deki `like` keyword'ü de text search özelliğidir, fakat `full-text search` birçok kompleks özelliği kapsar. bunlardan bazıları:

- `stop word`'lerin otomatik çıkarılması (`and`, `I` gibi kelimeler...)
- kelime köklerinin algılanmasını sağlamak. örnek: `git` arandığında, `gidiyorum` kelimesi içeren sonuçlarda bulunur.
- petabyte'lar üzerinden arama yapabilmek.
- birden fazla sütunu birleştirerek arama yapabilmek ve hatta bu sütunların bazıları farklı tipte olabilirken (örnek: `char`, `varchar`...).
- tüm bu özelliklerin opsiyonel olabilmesi.

## 📌 ElasticSearch vs Loki

`Loki` data'nın kendisini structured tutmaz. Direk saf byte array olarak tutar. dolayısı ile okuyan client'ın bunu parse etmesi gerekecek.

`Loki`'de data insert etmek isteyen client, label bilgisi de yollamak zorunda `Loki`'ye. `Loki` her label kümesi için data'ları birlikte tutar. Yani adsıl fark; `Loki` `full-text search` ve hiçbir şey index'lemez.sunamaz.

`ElasticSearch`'te index, `Loki`'deki index'e benzetebiliriz. Ek olarak; `ElasticSearch`'te hiçbirşeyin indexlenmemesini ayarlatabiliriz. Ama buna rağmen sistem eşit olmuyor `Loki` ile. Çünkü `ElasticSearch` buna rağmen field parsing gibi işlemleri yapmaktadır.