# ELASTICSEARCH KIBANA LOGSTASH

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

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

- Köşeli parantez içinde kalan kısım custom alanlardır. İstediğimiz oraya ekleyebiliriz. Bunlar `RFC`'de opsiyoneldir.

- Tire karakteri opsiyonel data olmadığını gösterir.

## 📌 syslog protocol

Network protocol tarafında bu log `string`'i karşı tarafa taşınmaktadır. `RFC`'ler okunduğunda protocol'lerin çok saf olduğu görülür. Yani `TCP` soket açıp log'ları yollamamız bile yeterlidir. Ek bir metadata yoktur. Sadece her log satırı sonunda satır sonu karakterini eklememiz yeterli.

Bu sebeple; `Logstash` output plugin'i ile `syslog` server'ına log atmak istediğimiz zaman, saf `TCP` output plugin'ini kullanmamız yeterli olmaktadır. Burada sadece dikkat etmemiz gereken nokta: her log sonrası satır sonu karakteri eklemeliyiz. Bunu da `TCP` output plugin'inin `json_lines` codec'ini kullanmamız gerekiyor. Bu codec'in `JSON Lines` ile alakası yok. Bambaşka konulardır.

## 📌 Syslog (Input plugin) for Logstash or Fluentd 

`Logstash` veya `Fluentd` ayarlarında input plugin olarak `Syslog` aktif edilirse, `Logstash` veya `Fluentd`, 514 `UDP` portundan dinleme yapmaya başlar. aynı bir `Syslog` daemon'u gibi log'ları dinler ve bunları bir sonraki aşamaya yollar.

`Linux`'ta çalışan bazı `syslog` daemon implementasyonları hem kendileri mesajları okuyor ve daha sonra ona gelen mesajları başka porta yönlendirebiliyor. Böyle olunca hem `Linux`, hemde `Fluentd` mesajları okuyabiliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 ELK (⟷ Elastic Stack)

`Elasticsearch`, `Logstash` ve `Kibana` üçlüsü çoğu zaman birlikte kullanılıyor. bu sebeple bu üçlü pakete verilen kısaltma.

`Logstash`'e her uygulama log'u yollar, `Logstash` tüm veriyi `Elasticsearch`'e aktarır. `Kibana` ise `Elasticsearch`'ten okuduğu data'ları son kullanıcıya web arayüzünden gösterir.

Bu mimaride `Logstash` çok yorulur. Çünkü herkes sadece `Logstash`'e data (log) yollar. Burada yükü dağıtmak için `Beats` isimli uygulama sıkça kullanılır. Tüm projemizdeki her uygulama için ayrı ayrı `Beats` ayağa kaldırabilir yada n-adet uygulama için sadece 1 adet `Beats` kaldırabiliriz. `Beats` sadece aracılık yapar. `Logstash`'e bilgileri yollarken, `backpressure` yeteneği de olduğundan yoğunluk sırasında sistemin dengesini korur. `Beats` aynı zamanda isterse direk olarak `Elasticsearch`'e de aldığı bilgileri atabilir. Hatta bazı ek modüller sayesinde basit filtreleme işlemleri de yapabilir.

`Beats`'ler tipine göre farklı executable dosyaları indirme gerektiriyor. aşağıdaki tüm örnek `Beats` uygulamaları, o anda çalıştıkları OS'tan toplandıkları data'ları Logstash veya `ElasticSearch`'e yollarlar.

- `Filebeat`

  istediğimiz dosyalardan okuma yapar.

- `Winlogbeat`

  `MS Windows` log sisteminden log'ları takip eder.

- `Auditbeat`

  `Linux Audit Framework` sisteminden topladığı audit bilgilerini toplar.
  
  `Linux Audit Framework`, `Linux`'ta zaten varolan bir yapı. son kullanıcının yaptığı işlemleri tutmaktadır.

- `packetbeat`

  Çalıştığı `OS`'taki tüm diğer process'lerin ne kadar `HTTP`, `DNS` isteği yaptığı gibi bilgileri toplar. Bunları yaparken network'ü sniff eder. ROot çalışmaıs gerekir ama sadece network yetkisi verilerekte çalışabilir.

- `Metricbeat`

  desteklediği birçok yazılımdan (`PostgreSQL`, `NGINX` gibi) metric toplar. Örneğin; `Metricbeat`'e `PostgreSQL`'in `DB` şifresi ve `IP`'si verilir, `Metricbeat` gibip bağlanır ve `SQL` atarak bazı istatistikler toplar. Örneğin; kaç satır eklenmiş tablolara gibi bilgiler  `PostgreSQL` `pg_stat_database` tablosunda bu tazr bilgileri tutuyor çünkü.

`Logstash` config dosyasının kendine has bir syntax'ı var.

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

`ElasticSearch`'te farklı bir index'e (`RDBMS`'teki tabloya denk gelen yapıya) kaydeder.

`Kibana` `Web UI`'ındaki sekmeler:

- `discover`: arama yapılmasını sağlıyor.
  `discover` sekmesindeki alt widget'lar:
  - bir widget'ta `Selected Fields` ve `Available Fields`. bunlar arama sonuçları ekranında gösterilebilecek sütunları belirtmektedir. örnek: `Available Fields` listesinde, log'larda akan tüm alanlar(sutunlar) mevcuttur. örnek: appName, IP, timestamp, message. `Selected Fields` listesine, `Available Fields` listesinden seçim yaparak alan ekleriz(taşırız). böylece sadece `Selected Fields` listesindeki alanlarımız, sonuç ekranında görünmüş olur. eğer `Selected Fields` listesini boş bırakırsak sonuç ekranında `_source` sütunu olur ve bu sütunda tüm log bilgileri(alanları) bir `JSON` şeklinde gösterilir.
  - bir widget'ta standart olan `Apache Lucene Query Syntax` veya `KQL` ile arama yapabilmemizi sağlayan textbox mevcuttur. `Lucene` arama örneği: `status:200 AND appName:user-service`. (`Lucene Query Syntax` başka başlıkta detaylı anlatılıyor)
  - bir widget'ta filter'lar mevcut. bunlar `Lucene` stili aramasına ek filtreler eklememizi sağlıyor. `Lucene` ile de yapılabilecek bir şey fakat filtreler kolay enable disable edilebiliyor. oysa `Lucene`'de bir filtreyi yorum satırı yapmazsınız. silip tekrar yazmamız gerekir.

  Ekranda yaptığımız her ayar `URL`'ye yansır. `URL`'yi başka tarayıcıya yapıştırdığımızda bu arama aynen açılır.

- `dev tools`: `JSON` query'leri ile arama yapılmasını sağlıyor. örnek:

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

index `Elasticsearch`'te `RDBMS`'teki tabloya denk gelen bir terimdir.

`index pattern` tell `Kibana` which `ElasticSearch` `indices` you want to explore. An `index pattern` can match the name of a single `index`, or include a wildcard (`*`) to match multiple `indices`.

For example, `Logstash` typically creates a series of `indices` in the format `Logstash-YYYY.MMM.DD`.

## 📌 index terimi karmaşası

`index pattern`'i kibana kullanıyor.

buradaki `index`, data'daki her field'ın `index`'lenip `index`'lenmemesi ile alakalı bir durum değil. bağımsız konular.

`elasticsearch`'te `index` bazen timestamp kullanılıyor. bu otomatik olan bir durum değildir. manuel developer bu şekilde ayarlar. tablo ismini manuel timestamp vermişisiz anlamına gelir.

## 📌 Kibana Query Language (⟷ KQL)

`Lucene syntax`'ına benzemektedir. `Lucene syntax`'ının sunmadığı ek özellikler sunmaktadır.

## 📌 ElasticSearch Query DSL

`API` üzerinden sunulan query sorgulama JSON syntax'ıdır. Tüm `ElasticSearch` `API`'si `Query DSL` değildir. `Query DSL` sadece `query` field'ı içindeki syntax'a verilen isimdir.

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

`Graylog`; `Logstash` ve `Kibana` ikilisene birlikte alternatif, `ELK`'ya göre daha community based bir projedir.

`Graylog` genelde `ElasticSearch` ve `MongoDB` ile kullanılıyor. `MongoDB`'de genel `graylog`'un config'leri saklanırken, `ElasticSearch`'te log'ların kendileri saklanıyor.

`Graylog` sunucusuna direk mesajları atabiliriz. Fakat `Beats` veya `Logstash` tarafında ek modüllerle support'u mevcut.

## 📌 Comparison of collectors and agents

| Name                             | Written with Language | resource usage           | main purpose       | detail                             |
|----------------------------------|-----------------------|--------------------------|--------------------|------------------------------------|
| `Logstash`                       | `Java` & `Ruby`       | slower then `Fluentd`    | aggregator         |                                    |
| `Fluentd`                        | `Ruby`                | slower then `Vector`     | aggregator         |                                    |
| `Vector`                         | `Rust`                | slower then `Fluent Bit` | agent & aggregator |                                    |
| `Fluent Bit`                     | `C`                   |                          | agent              |                                    |
| `OpenTelemetry Collector`        | `Golang`              |                          | agent & aggregator |                                    |
| `Splunk OpenTelemetry Collector` | `Golang`              |                          | agent & aggregator | fork of `OpenTelemetry Collector`. |
| `Elastic Agent`                  | `Golang`              |                          | agent              |                                    |
| `Beats`                          | `Golang`              |                          | agent              |                                    |

`OpenTelemetry Collector` ve `Logstash` aynı yapıdadır. Ama `Logstash` gelen data'nın içeriği hakkında pek ayar sunmaz, çünkü ilgilenmez. Ama `OpenTelemetry Collector` 3 türde data'yı (metric, log, trace) farkındadır ve onlara göre ayarlar sunar. Örneğin; hata oranı %1 in altında olan metric'leri toplama ayarını çok kolayca sunabilir.

## 📌 Resume after exit

Bazıları default'ta bazıları ise opsiyonel olarak dosyaya şu gibi bilgileri yazabilir:

- gelen data'yı
- eğer bir dosyadan okuyorlarsa, okdukları son satırın index'ini

Böylece, kill olurlarsa kaldıkları yerden devam edebilirler.

## 📌 Splunk forwarders

`Universal Forwarder (⟷ UF)` ve `Heavy Forwarder (⟷ HF)` aslında birer full Splunk binary'sidir. Fakat birçok özelliği disable edilerek çalıştırılır. sadece log datasını forward etmek ve kurulduğu `OS`'ten collect edebilmek için gerekli özellikleri barındırır.

`UF` parsing, filtering gibi işlemler yapamazken, `HF` yapabilir.

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
