# ELASTICSEARCH KIBANA LOGSTASH

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 ELK (⟷ Elastic Stack)

`Elasticsearch`, `Logstash` ve `Kibana` üçlüsü çoğu zaman birlikte kullanılıyor. bu sebeple bu üçlü pakete verilen kısaltma.

`Logstash`'e her uygulama log'u yollar, `Logstash` tüm veriyi `Elasticsearch`'e aktarır. `Kibana` ise `Elasticsearch`'ten okuduğu data'ları son kullanıcıya web arayüzünden gösterir.

Bu mimaride `Logstash` çok yorulur. Çünkü herkes sadece `Logstash`'e data (log) yollar. Burada yükü dağıtmak için `Beats` isimli uygulama sıkça kullanılır. Tüm projemizdeki her uygulama için ayrı ayrı `Beats` ayağa kaldırabilir yada n-adet uygulama için sadece 1 adet `Beats` kaldırabiliriz. `Beats` sadece aracılık yapar. `Logstash`'e bilgileri yollarken, `backpressure` yeteneği de olduğundan yoğunluk sırasında sistemin dengesini korur. `Beats` aynı zamanda isterse direk olarak `Elasticsearch`'e de aldığı bilgileri atabilir. Hatta bazı ek modüller sayesinde basit filtreleme işlemleri de yapabilir.

`Beats`'ler tipine göre farklı executable dosyaları indirme gerektiriyor. aşağıdaki tüm örnek `Beats` uygulamaları, o anda çalıştıkları OS'tan toplandıkları data'ları Logstash veya `ElasticSearch`'e yollarlar.

- __Filebeat__

  istediğimiz dosyalardan okuma yapar.

- __Winlogbeat__

  MS Windows log sisteminden log'ları takip eder.

- __Auditbeat__

  `Linux Audit Framework` sisteminden topladığı audit bilgilerini toplar.
  
  `Linux Audit Framework`, `Linux`'ta zaten varolan bir yapı. son kullanıcının yaptığı işlemleri tutmaktadır.

- __packetbeat__

  Çalıştığı `OS`'taki tüm diğer process'lerin ne kadar `HTTP`, `DNS` isteği yaptığı gibi bilgileri toplar.

- __Metricbeat__

  desteklediği birçok yazılımdan (`MongoDB`, `Redis`, `Zookeeper` gibi) metric toplar.

Logstash config dosyasının kendine has bir syntax'ı var.

## 📌 Syslog

özel isimdir.

`Syslog` servisi, default'ta 514 portundan dinleme yapar. isteyen uygulamalar buraya log'larını yollar.

`Syslog` ekosistemi 3 parçaya bölebiliriz:

- `Syslog` mesaj formatı

  log mesajında hangi bilgilerin olacağı modelidir.

  2 sürümü var: 1-`RFC 3164` ve 2-`RFC 5424`.

- `Syslog` protocol

  servisinin network üzerinden nasıl mesajı alacağı protocol'ü (örnek `TCP` ve `UDP` var) farklı `RFC`'lerde yazmaktadır.

- `Syslog` servisi (server)

  yukarıdaki 2 maddeyi dökümante etmiş olmalıdır ki, `Syslog` server'a mesaj atacak olan yazılım uyumlu çalışsın.

## 📌 apps for syslog

- `rsyslog` ve `syslog-ng`, `syslog` server'larıdır.

- `systemd-journald`

  piyasada buna sadece `journald` dendiği de olur.
  
  `systemd` projesinin bir parçasıdır ve onunla çalışmak için tasarlanmıştır.

  `systemd-journald` yazılımı birçok farklı yerden manuel olarak log'ları toplar ve bir yerde birleştirir.

  `libc`'nin `syslog` fonskiyonu çağrıldığında, fonskiyon bir `UNIX domain socket`'e log'ları `syslog` formatında yollar. `libc`, bu log'ları kimin okuyacağına karışmaz. Örneğin; bu `UNIX domain socket`'ten `systemd-journald` okuma yapar.

## 📌 syslog format

`syslog` formatında, sadece hangi alanlar (field'lar) olacağı bellidir.

2 adet `syslog` formatında log örneklersek:

```text
<24>1 2025-01-12T11:22:33Z myHostName myService1 1111 2222 - merhaba1
<24>1 2025-01-12T11:22:33Z myHostName myService1 1111 3333 [example@32473 user="john" ip="10.0.0.5"] merhaba2
```

- 24: mail system olduğunu ve log level'ın bir sabit sayı ile çarpımıdır (hesaplama formülü var `RFC`'de)

- 1: versiyonu temsil eder. Şu ana dek sadece 1 versiyon yayımlanmıştır.

- Zaman formatı `ISO 8601` olmalıdır.

- `1111` burada process ID'dir.

- `2222` ve `3333` mesajın tekil ID'sidir.

- Köşeli parantez içinde kalan kısım custom alanlardır. İstediğimiz oraya ekleyebiliriz. Bunlar RFC'de opsiyoneldir.

- Tire karakteri opsiyonel data olmadığını gösterir.

## 📌 syslog protocol

Network protocol tarafında bu log `string`'i karşı tarafa taşınmaktadır. RFC'ler okunduğunda protocol'lerin çok saf olduğu görülür. Yani `TCP` soket açıp log'ları yollamamız bile yeterlidir. Ek bir metadata yoktur. Sadece her log satırı sonunda satır sonu karakterini eklememiz yeterli.

Bu sebeple; `Logstash` output plugin'i ile `syslog` server'ına log atmak istediğimiz zaman, saf `TCP` output plugin'ini kullanmamız yeterli olmaktadır. Burada sadece dikkat etmemiz gereken nokta: her log sonrası satır sonu karakteri eklemeliyiz. Bunu da `TCP` output plugin'inin `json_lines` codec'ini kullanmamız gerekiyor. Bu codec'in `JSON Lines` ile alakası yok. Bambaşka konulardır.

## 📌 Syslog (Input plugin) for Logstash or Fluentd 

`Logstash` veya `Fluentd` ayarlarında input plugin olarak `Syslog` aktif edilirse, `Logstash` veya `Fluentd`, 514 UDP portundan dinleme yapmaya başlar. aynı bir `Syslog` daemon'u gibi log'ları dinler ve bunları `Elasticsearch`'e yollar.

`Linux`'ta çalışan bazı `syslog` daemon implementasyonları hem kendileri mesajları okuyor ve daha sonra ona gelen mesajları başka porta yönlendirebiliyor. Böyle olunca hem `Linux`, hemde `Fluentd` mesajları okuyabiliyor.

## 📌 OpenSearch

`Open Distro`, `OpenSearch` olarak ismi değişti.

`OpenSearch` sadece aşağıdaki projeleri yeni isimlerle daha özgür lisans ile fork'ladı:

- `Kibana` - `OpenSearch Dashboards`
- `ElasticSearch` - `OpenSearch`

`Logstash` ve `Beats` projeleri fork'lamadı. Çünkü zaten onlarla uyumlu çalışmaktadır.

## 📌 Kibana

`ElasticSearch`'teki data'ları son kullanıcıya göstermek için web arayüzü sunan bir yazılım.

`Logstash` kendi internal'da kullandığı config'lerini:

- index patterns
- saved searches
- saved visualizations
- ve dahası...

`ElasticSearch`'te farklı bir index'e (RDBMS'teki tabloya denk gelen yapıya) kaydeder.

`Kibana` `Web UI`'ındaki sekmeler:

- `discover`: arama yapılmasını sağlıyor.
  `discover` sekmesindeki alt widget'lar:
  - bir widget'ta `Selected Fields` ve `Available Fields`. bunlar arama sonuçları ekranında gösterilebilecek sütunları belirtmektedir. örnek: "Available Fields" listesinde, log'larda akan tüm alanlar(sutunlar) mevcuttur. örnek: appName, IP, timestamp, message. `Selected Fields` listesine, "Available Fields" listesinden seçim yaparak alan ekleriz(taşırız). böylece sadece `Selected Fields` listesindeki alanlarımız, sonuç ekranında görünmüş olur. eğer `Selected Fields` listesini boş bırakırsak sonuç ekranında `\_source` sütunu olur ve bu sütunda tüm log bilgileri(alanları) bir JSON şeklinde gösterilir.
  - bir widget'ta standart olan "__Apache Lucene Query Syntax__" veya "__Kibana Query Language (⟷ KQL)__" ile arama yapabilmemizi sağlayan textbox mevcuttur. `Lucene` arama örneği: `status:200 AND appName:user-service`. (`Lucene Query Syntax` başka başlıkta detaylı anlatılıyor)
  - bir widget'ta filter'lar mevcut. bunlar `Lucene` stili aramasına ek filtreler eklememizi sağlıyor. `Lucene` ile de yapılabilecek bir şey fakat filtreler kolay enable disable edilebiliyor. oysa `Lucene`'de bir filtreyi yorum satırı yapmazsınız. silip tekrar yazmamız gerekir.

  Ekranda yaptığımız her ayar URL'ye yansır. URL'yi başka tarayıcıya yapıştırdığımızda bu arama aynen açılır.

- `dev tools`: JSON query'leri ile arama yapılmasını sağlıyor. örnek:

```text
GET _search
{
  "query": {
    "match_all": {}
  }
}
```

- `visualize`: grafikler hazırlamamızı sağlıyor. buradan grafik oluşturuyoruz. fakat bu grafikleri direk `Web UI`'ı açan user'a gösteremiyoruz. bu grafikleri dashboard'a bağlamak zorundayız.

- `dashboard`: bir filtre yazıyoruz ve visualize'da hazırladığımız her görsel'i buradaki filtreye bağlıyoruz. örneğin filtremiz `status:200` olsun. altında da buna bağlı 3 tane grafik bağlamış oluruz. bu `dashboard`'a "başarılı istekler" adını verelim. birçok `dashboard`'umuz olabilir. artık `dashboard` sekmesine girip, listeden "başarılı istekler" `dashboard`'ını seçen user, karşısında 3 tane grafik görecektir.

## 📌 Kibana index pattern

index `Elasticsearch`'te RDBMS'teki tabloya denk gelen bir terimdir.

`index pattern` tell `Kibana` which `ElasticSearch` `indices` you want to explore. An `index pattern` can match the name of a single `index`, or include a wildcard (`*`) to match multiple `indices`.

For example, `Logstash` typically creates a series of `indices` in the format `Logstash-YYYY.MMM.DD`.

## 📌 index terimi karmaşası

`index pattern`'i kibana kullanıyor.

buradaki `index`, data'daki her field'ın `index`'lenip `index`'lenmemesi ile alakalı bir durum değil. bağımsız konular.

`elasticsearch`'te `index` bazen timestamp kullanılıyor. bu otomatik olan bir durum değildir. manuel developer bu şekilde ayarlar. tablo ismini manuel timestamp vermişisiz anlamına gelir.

## 📌 Kibana Query Language (KQL)

`Lucene syntax`'ına benzemektedir. `Lucene syntax`'ının sunmadığı ek özellikler sunmaktadır.

## 📌 ElasticSearch Query DSL

API üzerinden sunulan query sorgulama JSON syntax'ıdır. Tüm `ElasticSearch` API'si `Query DSL` değildir. `Query DSL` sadece `query` field'ı içindeki syntax'a verilen isimdir.

```text
GET /_search
```

```json
{
    "query": {
        "match" : {
            "message" : {
                "query" : "this is a test"
            }
        }
    }
}
```

## 📌 Graylog

`Graylog`; `Logstash` ve `Kibana`'ya alternatif, `ELK`'ya göre daha community based bir projedir.

`Graylog` genelde `ElasticSearch` ve `MongoDB` ile kullanılıyor. `MongoDB`'de genel `graylog`'un config'leri saklanırken, `ElasticSearch`'te log'ların kendileri saklanıyor.

`Graylog` sunucusuna direk mesajları atabiliriz. Fakat `Beats` veya `Logstash` ile de ek modüllerle support'u mevcut.

## 📌 Fluentd

ayrı bir process olarak çalışır.

2 görevi var diyebiliriz:
- `Logstash`'e alternatiftir.
- `beats`'e alternatiftir. Fakat FLuentd burada kullanmak gereksiz olur. Burada `Fluentbit` önerilir. çünkü daha hafiftir ve `Fluentbit` sadece agent modu için tasarlanmıştır.

- sadece `Beats` yerine de koyabiliriz
- sadece `logstash` yerine de koyabiliriz.
- `Logstash`'e de direk log yollayabilir, `elasticsearch`'e de direk yollayabilir. Yani sadece kendi başına da işi baştan sona tamamlayabiliyor.

`JDK` bağımlılığı yok.

## 📌 Vector

`Fluentd` ve `Fluentbit` ikilisine birlikte alternatiftir. ikisini görevini görür.

## 📌 Splunk

Ticari lisansla dağıtılan merkezi bir log yöntim sistemidir. Tüm sistemi baştan sona içinde barındırır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Log driver of Docker

`Docker` runtime sırasında sysout ve syserr çıktılarını container dışında herhangi bir yere yönlendirebilir. driver seçeneğini container başlatınca verebiliriz. örnekler:

- `none`

  hiç log basmaz.

- `json-file`

  container dışında (`docker`'ı run eden `OS` içinde) bir `JSON` dosyaya kaydeder.

- `local`

  container dışında (`docker`'ı run eden `OS` içinde) bir dosyaya kaydeder.

- `syslog`

  `docker`'ı run eden `OS`'un syslog servisine log'ları yollar.

- `fluentd`

  `fluentd` servisine log'ları yollar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Solr vs ElasticSearch

Ikisi de açık kaynaklı birbirine alternatif arama index'leyici ve query yazılımlardır.

`Solr`'da field isimleri ve field type'lar baştan belirtilmek zorundadır. Yeni field eklemeye kalktığınızda, bütün kayıtları tekrar `Solr`'a aktarmanız gerekiyor. `Elasticsearch`'te böyle bir durum yok. Çünkü `Elasticsearch`, scheme barındırmıyor. `NoSQL` `DB` gibi.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

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

Elasticsearch bir search ve analytics engine'i iken, diğerleri NoSQL olarak geçmektedir. 3 alternatif yazılım data'ları NoSQL olarak hard-drive'a kaydedip, bunları client'a sunsalar da, tutuş biçimleri performans önceliklerine göre değişmektedir.

MongoDB generic-purpose bir NoSQL DB olarak kullanılır. istediğimiz sütunları index'leriz. Cassandra'da ise, sorgularımız baştan belli olmalıdır ve daha incremental data'ların kaydı için kullanılır. ElasticSearch ise tüm sütunları (field'ları) ve field'ların içindeki data'ları sürekli index'ler. bu şekilde tüm data'larda hızlı arama yapılabilmesini sağlar. fakat bu durum insert işlemlerini yavaşlatır, çünkü her insert işleminde index'leme çalışması şarttır (ve tüm field'lar index'lenmek zorundadır).

ElasticSearch "document store" olarak kayıt tutmaktadır.

ElasticSearch ek olarak "__full-text search__" destekleyen bir storage'dir. "full-text search" teriminin kapsamı keskin çizgilerle çizilmemiştir. SQL'deki "like" keyword'ü de text search özelliğidir, fakat "full-text search" birçok kompleks özelliği kapsar. bunlardan bazıları:

- __stop word__'lerin otomatik çıkarılması ("and", "I" gibi kelimeler...)
- kelime köklerinin algılanmasını sağlamak. örnek: "git" arandığında, "gidiyorum" kelimesi içeren sonuçlarda bulunur.
- petabyte'lar üzerinden arama yapabilmek
- birden fazla sütunu birleştirerek arama yapabilmek (ve hatta bu sütunların bazıları farklı tipte olabilirken (örnek: char, varchar...))
- tüm bu özelliklerin opsiyonel olabilmesi
