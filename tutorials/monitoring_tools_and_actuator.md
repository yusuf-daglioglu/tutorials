# MONITORING TOOLS AND ACTUATORS

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Log standartları

- `OpenTelemetry Protocol (⟷ OTLP)`

  SABİT ALANLAR: VAR

  OPSİYONEL ALANLAR: VAR

  NETWORK'TEN NASIL İLETİLECEĞİNE: KARIŞIR (`gRPC` veya `HTTP` zorunludur)

  PRINT FORMATINA: KARIŞMAZ

- `Extended Log File (⟷ ELF)`

  log dosyasının ilk satırında, her satırın ne anlama geldiği yazıyor.

  SABİT ALANLAR: VAR

  OPSİYONEL ALANLAR: VAR

  NETWORK'TEN NASIL İLETİLECEĞİNE: KARIŞMAZ

  PRINT FORMATINA: KARIŞIR

- `Common Log Format (⟷ CLF)`

  web server'ların access log'ları için uygundur.

  SABİT ALANLAR: VAR

  OPSİYONEL ALANLAR: YOK

  NETWORK'TEN NASIL İLETİLECEĞİNE: KARIŞMAZ

  PRINT FORMATINA: KARIŞIR

- `Combined Log Format`

  `CLF` ile aynı sadece ek olarak 2 field daha alıyor.

- `syslog`

  detayları başka başlıkta anlatıldı.

  network'ten nasıl yollanacağı ve format farklı `RFC`'lerde yayımlanmıştır.

Bazı log formatları: https://docs.lnav.org/en/v0.13.1/formats.html#log-formats

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 spring-boot Actuator

`Actuator` kelime anlamı: 1. işletici, çalıştırıcı 2. uyarıcı

`Spring`'e `dependency` olarak eklendiğinde, o `Spring` uygulaması içerisinde `REST` `endpoint`'leri açıyor ve bu `endpoint`'ler üzerinden `health check`, disk usage, heap dump gibi bilgiler dışarıya veriyor.

`http://localhost:8080/actuator` `URL`'si bize `Actuator`'ın açtığı tüm metric gruplarının `URL` listesini döner. Liste aşağıdaki gibidir. Her `URL`, kendi içerisinde istediği bilgileri dışarıya açabilir. Bazı temel örnekler:

- `http://localhost:8080/actuator/metrics`

  - current thread count
  - current memory usage
  - system update
  - all `Java` classes and package names

- `http://localhost:8080/actuator/beans`

  all `Spring beans`
  
- `http://localhost:8080/actuator/env`

  all environment variables (her prop `app.yaml`, `bootstrap yaml`, `K8s`'ten mi gelip gelmediği detayı ile birlikte yazıyor)

- `http://localhost:8080/actuator/info`

  information about `spring-boot`

- `http://localhost:8080/actuator/trace`

  all `REST` `endpoints`

Örneğin; `K8s Java client`; `Pod` (`container`) name, `namespace` gibi bilgileri `/actuator/info` `path`'inden döndürüyor. `K8s` isterse farklı bir URL'den bunları sunabilirdi. ama "`K8s Java client`" geliştiricileri bu `URL`'den döndürecek şekilde geliştirme yapmış (standart olması açısından).

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Dynatrace

`APM` ve `profiling` sunuyor.

Server tarafında `Web UI`, `DB`, backend bir arada.

## 📌 Dynatrace OneAgent

her `OS` için ayrı binary sunuluyor. Bu yazılım `OS`'ten bilgileri toplayıp server'a yolluyor.

admin yetkisi ile çalışır. çünkü, örneğin, `Java` processlerini otomatik yakalar ve kendini startup'larında inject eder.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Grafana

- herhangi bir kaynaktan verileri çekerek, bunları kendisinin sunduğu web arayüzünden visualize eden bir yazılımdır. Topladığı data'ları anlık olarak gösterir. Anlık olarak data'ları çektiği için kendi `DB`'i yoktur.

- `Grafana` herhangi bir veriyi çekebilir. Verinin tipine (modeline, formatına bakmaz).

- `Grafana` sadece şu zamanlarda kaynaklardan data okur:
  - manuel query çalıştırdığımızda gider ve o `data source`'tan bilgiyi alır.
  - dashboard'lar arkada query çalıştırır. ne sıklıkla query'nin execute edileceği, yine dashboard'un config'inden ayarlanır.

- `Query Editor`, `Web UI`'daki `explore` sekmesindeki query oluşturmamıza yarayan, auto-complete seçenekleri sunan `HTML` component'lerdir.

  `Query editor` her `data source` için karşı tarafın kendi native dilini kullanır.

  Örneğin; `PostgreSQL`'den data çekmek için `SQL` kullanır.

  ```text
  SELECT hostname FROM host  WHERE region IN($region)
  ```

  Bu sebeple `query editor`, `data source` değiştirildiğinde (seçildiğinde) çok farklı bir component halini alabilir.

  `Garafa` karşıdan data çekmeyi kolaylaştırmak için şunları yapabilir:
  - Hangi dil olursa olsun (connector'den bağımsız) template variable'ları sunuyor.
  - Dil bazlı (connector spesifik) (örneğin sadece `PostgreSQL` için) macro (built-in helper) sunabilir.

  Bu sebeple bazı query'ler native görünmeyebilir.

- `Grafana` kendi configurasyon'larını (aşağıdaki liste) kaydetmek için `RDBMS` (default'ta `SQLite`) kullanır:
  - user list to login `Grafana` `Web UI`
  - `data source` connection info (like `ip`, `port`, username, password)
  - user generated dashboards

### 📌📌 Prometheus

`Prometheus` her türlü data'yı çekebilir. Data'nın içeriğine karışmaz ama çekeceği data kendi belirlediği syntax'ta olmalıdır. Eğer kendi formatında değilse, `Prometheus` içindeki connector data'yı çektiğinde convert edebilir. Örnek format bu şekilde:

```text
# TYPE http_requests_total counter
http_requests_total{status="200",env="prod"} 150
http_requests_total{status="500",env="prod"} 2
```

Örneğin; `string` kısımların içinde hiç boşluk olmamalı. Bu gibi kurallar da olduğundan, herhangi bir data'yı geçici wrap edemediğimiz için `Prometheus`'a yollayamayız.

### 📌📌 exporter

`Prometheus` dünyasında `exporter`; okuduğu data'yı `Prometheus`'un anlayacağı formata çeviren yazılımdır.

## 📌 Comparison of monitoring tools

| name            | Metric | Trace | Log | `Web UI`        | `profiling` | `Web UI` `APM` | `DB`                              | supported protocols                                   |
|-----------------|--------|-------|-----|-----------------|-------------|----------------|-----------------------------------|-------------------------------------------------------|
| `Pyroscope`     | yes    | yes   | no  | advanced        | yes         | no             | yes. data is distributed.         | `OTLP`, it's own format                               |
| `Mimir`         | yes    | no    | no  | no              | no          | no             | yes. data is distributed.         |                                                       |
| `Prometheus`    | yes    | no    | no  | yes but simple. | no          | no             | yes. data can not be distributed. | `OTLP`, `OpenMetrics`, `Prometheus` Exposition Format |
| `Grafana Tempo` | no     | yes   | no  | no              | no          | no             | yes.                              | `OTLP`, `Jaeger`, `Zipkin`                            |
| `Zipkin`        | no     | yes   | no  | yes but simple. | no          | no             | yes. any `DB`.                    | `Zipkin`                                              |
| `Dynatrace`     | yes    | yes   | yes | advanced        | yes         | yes            | yes.                              | `OTLP`, it's own proprietary                          |
| `Signoz`        | yes    | yes   | yes | advanced        | no          | yes            | yes.                              | `OTLP`, `Jaeger`, `Zipkin`                            |
| `Jaeger`        | no     | yes   | no  | yes             | no          | no             | yes.                              | `OTLP`, `Jaeger`, `Zipkin`                            |
| `Splunk`        | yes    | yes   | yes | yes             | yes         | yes            | yes.                              | `HTTP Event Collector (⟷ HEC)`                        |

`Pyroscope`, has also UI plugin for `Grafana`.

`Signalfx`, monitoring yazılımı. `Splunk` firması tarafından satın alındı ve `Splunk`'ın monitorng sunan ürünün içine gömüldü.

## 📌 Pushgateway

tool'un özel ismidir.

`Prometheus` sadece pull mantığında çalışır. yani data'yı gidip bir `API`'den okur. Ama agent'ların push yapmsı zorunlu olan durumlarda (örneğin; sürekli çalışmayan batch uygulamaları gibi) araya `Pushgateway` isimli yazılım konulur. agent'lar `Pushgateway`'e data insert eder, `Prometheus` `Pushgateway`'den data'yı pull eder.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Micrometer

`JVM` uygulamaları için facade kütüphanesidir. Bu kütüphane `JVM` based uygulamamız dışarı `Spring-Actuator` gibi bilgi vermektedir. `Spring-Actuator` bilgileri dışarı açarken belli bir formatta (standartta) sunmaz. oysa `Micrometer` birçok `monitoring tool`'a destek verecek şekilde bunları dışarı açar.

`Micrometer`'ın support ettiği `monitoring tool`'lar:

- `Dynatrace` (Proprietary format)
- `OTLP`
- ve dahası

`Micrometer`, `JVM` uygulamalarında `Spring-Actuator` ile de entegreli çalışabilir. eğer istenirse `Spring-Actuator`'sız, saf `Java` projesinde de çalışabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 application performance monitoring (⟷ APM)

uygulamanın dışarıdan performansı ve hata oranlarını görebilmemizi sağlar.

örnekler:

- payment-service son 5 dakikada %10 error rate
- DB timeout artmış

## 📌 profiling

uygulamanın içindeki her türlü detayları görebilmemizi sağlar. Nerdeyse debug seviyesindedir.

örnekler:

- fonksiyon ve sınıf bazında bellek ve `CPU` kullanımı
- thread beklemelerini görebilme
- `Java` `Garbage collector` çalışma zamanlarını

## 📌 3 pillars of observability (⟷ core observability pillars)

`monitoring` terimi, `IT` sektöründe `Observability` olarak kullanılıyor.

- metric
- log
- trace

`profiling` data'sını veritabanında saklamak istersek, yukarıdaki 3 model ile alakası yoktur. Genelde açık kaynaklı `pprof` formatı kullanılır. `protobuf` tabanlı olduğu içim böyle isimlendirilmiş. Bu format birçok programlama dilini destekliyor.

`APM` data'sını veritabanında metric ve trace data'sını ilişkilendirerek dutabiliriz. Yani `UI` tarafının metric ve trace data'sını ilişkilendirerek araması ve gösterebilmesi yeterlidir.

## 📌 Bazı UI component'leri

### 📌📌 Flame graph

alev şeklindedir. her fonskiyonun ne kadar çalıştığı gösteriyor. alevlerin ucuna gittikte, `stack trace`'deki son çağrılan fonskiyonları görürürüz.

### 📌📌 Top N (hot paths)

belli aralıktaki en pahalı endpoint veya fonksiyon listesi.

### 📌📌 Waterfall (trace timeline)

sadece trace için kullanılır. Her span bir çubuktur. Bir sonraki span'a paralel'den yönlendiği zaman, ilgili çubuğun aşağısına başka çubuk gelir.

### 📌📌 Heatmap

1 `Heatmap`, sadece 1 fonksiyone, endpoint veya label ile gruplanmış endpoint'ler için hazırlanmış olabilir.

Tablodur. X ekseni zaman aralıklarını gösterir: 10:00, 10:15 gibi. Y ekseni ise, geçikme sürelerini gösteririr. Her hücre ise o zaman aralığında, ne kadar hızlı dönüş yapan request sayısı gösterir.

### 📌📌 SLI (⟷ Service Level Indicator)

Sistemin ölçülen performans metric'leridir. örnekler:

- error rate %2
- %90 istek 3 saniyenin altında hızlıca cevaplandı.

### 📌📌 KPI (⟷ Key Performance Indicator)

İş hedefleri başarı göstergeleri. Daha çok business hedeflerini temel alır. örnekler:

- müşteri memnuniyeti %90
- payment service uptime %99

### 📌📌 Abdex (⟷ Application Performance Index)

Kullanıcı deneyimini 0 ile 1 arasında bir skor ile göstermek.

bazı varyasyonları olsada asıl formülü:

```
Abdex = ( Satisfied + 0,5 * tolerating ) / total
```

örnek:

- 200 ms önceden şirket çinde belirlediğimiz memnuniyet treshold'u. bundan daha yavaş veren servislerin memnuniyetsizlik yarattığı kabul edilir.
- 100 adet istek örneği alınır:
  - 70 istek, 200ms'nin altında (Satisfied)
  - 20 istek, 200ms'nin biraz üstünde (tolerating)
  - 10 istek, 200ms'nin çok üstünde (frustrated)

```
Abdex = ( Satisfied + 0,5 * tolerating ) / total = ( 70 + 0,5 * 20 ) / 100 = 0.8
```

Sonuç 1 ise, mükemmel müşteri deneyimi anlamına gelir. Bu sayı düştükçe daha kötü deneyim olmuş olur.

## 📌 Percentile-based Monitoring (⟷ Yüzdelik Tabanlı İzleme)

Belli zaman aralığındaki tüm isteklerin reponse time'ı alınır.

UI'da şöyle bir şey okuyabiliriz:

- P50 = 60ms
- P95 = 200ms
- P99 = 400ms

Bu şu demek:

- client'ların %50'si 60ms'den aşağıda bir sürede cevap aldı.
- client'ların %95'i 200ms'den aşağıda bir sürede cevap aldı.
- client'ların %99'si 400ms'den aşağıda bir sürede cevap aldı.

## 📌 Synthetic monitoring

`Synthetic` kelime anlamı: `sentetik` (yapay).

otomasyon test user'ları ile yapılan testlerdeki `APM`'dir.

## 📌 RUM (⟷ Real User Monitoring)

Gerçek kullanıcıların uygulamanı tarayıcıda veya mobilde kullanırken yaşadığı deneyimi ölçer.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 eBPF (⟷ extended BPF)

`Linux`'un (`API` aracılığı ile kullanılabilen) sunduğu bir mekanizmadır.

`Linux`'ta run edilen yazılım user seviyesinde çalışıp `eBPF` `API`'lerini çağırabilir. Ama bu process'in ekstra yetki alması şarttır.

`Linux`'ta run edilen yazılım, `Linux` modülü olmak zorunda değildir.

`Linux`'ta run edilen yazılım, runtime'da `OS`'un event'lerine hook'lar ekler. Bu hook'lar `OS`'un event'lerinde devreye girer. Mesela, herhangi bir yazılım `TCP`'den bir şey okuduğunda veya attığında, hook'ta istediğimiz kodu çalıştırabilmemizi sağlar.

Bu inject edilen hook'lar, `API`'yi kullanan process kapatıldığında yok olurlar. Yani kalıcı hook'lar değildir. Böylece `kernel`'da kalıcı etki yaratmazlar ve fiziksel olarak `kernel` dosyalarında değişiklik olmaz.

## 📌 Beyla

`eBPF` `API`'sini kullanarak `OS`'tan bilgi toplayan yazılımdır. topladığı bilgileri `OTel` ile  monitoring tool'a yollar.

sadece metric ve trace bilgilerini toplar.

## 📌 Berkeley Packet Filter (⟷ BPF ⟷ BSD Packet Filter ⟷ classic BPF ⟷ cBPF)
Cross platform, (`API` aracılığı ile kullanılabilen) bir mekanizmadır.

`eBPF`'ye göre daha basit ve eskidir. `cBPF` sadece network event'leri için hook yapısı sunar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 OpenTracing vs OpenCensus vs OpenTelemetry (⟷ OTel)

`OpenTracing` ve `OpenCensus` projeleri, `OpenTelemetry` isimli projede birleşti.

`OpenTelemetry` projesi tüm bunları içeriyor:

### 📌📌 OTLP

`IT` dünyasında bunlara genel olarak `export protocol (⟷ telemetry protocol)` deniyor.

aşağıdaki bilgilerinin monitoring tool'a nasıl yollanacağı (network protocol):

- `trace`

  `span` aslında buna denk geliyor.

- `log`

- `metric`

- `profile`

  Örnek: her 2 saniyede bir kere `CPU` ve `RAM` kullanımı, hangi fonskiyonlar kullanılıyor (`stack trace` aracılığı ile) gibi bilgiler. Bunlar batch olarak veya anlık monitoring tool'a atılabilir.

  `OTLP`'nin profiling formatı `pprof` değildir. Daha çok programlama diline destek verecek farklı bir formatı temel alır.

#### 📌📌📌 example request body JSON

https://github.com/open-telemetry/opentelemetry-proto/tree/main/examples

Example Log request from above link:

```json
{
  "resourceLogs": [
    {
      "resource": {
        "attributes": [
          {
            "key": "service.name",
            "value": {
              "stringValue": "my.service"
            }
          }
        ]
      },
      "scopeLogs": [
        {
          "scope": {
            "name": "my.library",
            "version": "1.0.0",
            "attributes": [
              {
                "key": "my.scope.attribute",
                "value": {
                  "stringValue": "some scope attribute"
                }
              }
            ]
          },
          "logRecords": [
            {
              "timeUnixNano": "1544712660300000000",
              "observedTimeUnixNano": "1544712660300000000",
              "severityNumber": 10,
              "severityText": "Information",
              "traceId": "5B8EFFF798038103D269B633813FC60C",
              "spanId": "EEE19B7EC3C1B174",
              "body": {
                "stringValue": "Example log record"
              },
              "attributes": [
                {
                  "key": "string.attribute",
                  "value": {
                    "stringValue": "some string"
                  }
                },
                {
                  "key": "map.attribute",
                  "value": {
                    "kvlistValue": {
                      "values": [
                        {
                          "key": "some.map.key",
                          "value": {
                            "stringValue": "some value"
                          }
                        }
                      ]
                    }
                  }
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

`JSON`'da görüldüğü gibi birden fazla log aynı request'te de yollanabiliyor. Bu tamamen client tarafa kalmış. Client isterse batch mantığında çalışabiliyor yani.

`JSON`'daki bazı field'lar zorunlu veya opsiyonel veya tamamen custom olabilir.

### 📌📌 trace context propagation format (⟷ propagation format)

her servis sadece monitroing tool'una değil, aynı zamanda iş akışını devrettiği servise de mesaj atıyor. böylece `trace-id`'yi, `parent-id`'yi, işlemi devam ettirecek servis bilsin. buradaki mesajın formatıda bir standart olmalı. bu bilgiler her zaman header'da gider.

Piyasadaki `trace context propagation format` alternatifleri:

- `B3 propagation`

  `HTTP` `header`'da `X-B3-` ile başlayan `string`'lerdir.

  `Zipkin`, `Spring Cloud Sleuth`, bunu kullanır. Bu formatı ilk `Zipkin` ortaya attığı için `Zipkin B3` diye piyasada adlandırılabiliyor.

  `Spring Cloud Sleuth` daha sonrasında `W3C Trace Context`'e geçti.

- `W3C Trace Context`

  `OpenTelemetry` ve `Dynatrace` bunu kullanır.

- `jaeger propagation`

  `jaeger` isimli monitoring tool'u tarafından öneriliyor.

## 📌 OTel gloassary

`signal` aşağıdaki hepsine verilen genel bir isim:

- `trace`
- `log`
- `metric`
- `baggage`: tüm akış boyunca taşınan key value data. distributed context gibi düşünülebilir. bu özel olarak monitoring tool'a export edilmez.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Elastic Observability

`Elastic` firmasının sunduğu, `APM` için sunduğu tüm hizmetlere verilen genel isim:

- `ElasticSearch` data tiers (`ElasticSearch` core feature)

  Her tier ayrı fiziksel alanlarda tutulabilmesini sağlıyor. Örneğin sık kullanılan index'ler (hot tier) SSD'de, az kullanılanlar (cold tier) bulut sistemlerde depolanabilmesi ayarlanabiliyor.

- `ECS (⟷ Elastic Commons Schema)`

  `APM` datasını veritabanında tutarkenki `DB` şema (tablo ve sutun isimleri, hangi sutun hangi bilgiyi tutuyor) standartı.

- `Kibana` `APM` datasını daha verimli şekilde sunabilmek için özel bir menüsü var (özel dashboard'lar içeriyor).

- `Kibana` Alerting (core feature of `Kibana`)

- `Fleet`, `Kibana`'nın bir core feature'sinin ismi. Bunun sayesinde `Kibana` arayüzünden `Elastic Agent`'ların configurasyonlarını uzaktan yönetebiliyor. `Elastic Agent`'lar `filebeat` alternatifidir ve tüm `beats`'lerin özelliklerini içerir ve bu sebeple binary boyutu daha büyüktür.

- ve dahası.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Zipkin

Her servisimizde `Zipkin` ile haberleşmesi için bir kütüphane olmalı. bu kütüphanelere `instrumentation library` deniliyor. her microservice'e `reporter` veya `instrumented app` deniliyor.

sadece microservice'ler değil, aynı zamanda isteği yapan frontend'ta bu sürecin içine dahil edilebilir. client tarafta isteği yolladığı anda bu bilgiyi `Zipkin` sunucusuna bildirebilir.

`Zipkin`, her microservice'ten bilgi alabilmek için onlara bir `API` sunar. Bu `API`'ye `collector` denir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Sleuth dependencies

`Spring cloud`'un `Sleuth` için dependency'leri:

- `Spring-cloud-starter-zipkin` -> `BOM`
- `Spring-cloud-sleuth-stream` -> deprecated oldu
- `Spring-cloud-sleuth-zipkin` -> `Zipkin` server'a bilgi yollayan kütüphane.
- `Spring-cloud-sleuth` -> `Zipkin`'e bilgi yollamaz. bir sonraki service bilgileri taşır. `kaynak: https://github.com/spring-cloud/spring-cloud-sleuth#only-sleuth-log-correlation`

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 span

`Türkçe` kelime anlamı: karış.

bir iş akışı süreci birçok iş akışı parçacığından oluşabilir. bu parçacıkların ne olacağı tamamen bize kalmış.

Spring kütüphaneleri default'ta, her microservice arasındaki geçişler birer `span` olarak kabul eder.

## 📌 trace

trace-ID tüm istek için baştan sona tekildir.

### 📌 parent-ID

Buna `parent-span-id`'de denildiği olur.

isteğin hangi `span-ID`'den sonra yapıldığı bilgisidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
