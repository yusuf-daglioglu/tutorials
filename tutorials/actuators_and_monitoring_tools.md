# ACTUATORS AND MONITORING TOOLS

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

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

## 📌 Micrometer

`JVM` uygulamaları için facade kütüphanesidir. Bu kütüphane `JVM` based uygulamamız dışarı `Spring-Actuator` gibi bilgi vermektedir. `Spring-Actuator` bilgileri dışarı açarken belli bir formatta/standartta sunmaz. oysa `Micrometer` birçok `monitoring tool`'a destek verecek şekilde bunları dışarı açar.

`Micrometer`'ın support ettiği `monitoring tool`'lar:

- `Dynatrace`
- `Elastic`
- `Prometheus`

`Micrometer`, `JVM` uygulamalarında `Spring-Actuator` ile de entegreli çalışabilir. eğer istenirse `Spring-Actuator`'sız, saf `Java` projesinde de çalışabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Grafana

- birçok kaynaktan verileri çekerek, bunları kendisinin sunduğu web arayüzünden visualize eden bir yazılımdır. Topladığı data'ları anlık olarak gösterir. Anlık olarak data'ları çektiği için kendi DB yoktur. `Grafana` herhangi bir veriyi toplayabilir. örnekler:
  - `Java` spring uygulamasından sadece metric okuyabilir: `http://localhost:8080/metrics`
  - `PostgreSQL` gibi `RDBMS`'lerde sadece belirlediğimiz `SQL` sorguları çalıştırır. örnek son 1 dakikadaki toplam siparişler: 
  
    ```sql
    SELECT count from Order WHERE date>(now-1m)
    ```

  - `Prometheus`'tan data okur (aynı `PostgreSQL`'den okuduğu gibi).
  
`Grafana` özellikle time series data yönetimini çok iyi yaptığından dolayı `Prometheus` gibi `data source`'lardan ve `time series DB (⟷ TSDB)` konularından çok bahsedilir.

- `Grafana` sadece şu zamanlarda kaynaktan data okur:
  - manuel query çalıştırdığımızda gider ve o `data source`'tan bilgiyi alır.
  - dashboard'lar arkada query çalıştırır. ne sıklıkla query'nin execute edileceği, yine dashboard'un config'inden ayarlanır.

- `Grafana` üzerinden `API` `key` oluşturulabilir. Bu `key`'ler ile, `Java` gibi client'larımız aracılığı ile `Grafana`'ya query execute ettirebiliriz.

- `API` `key` yerine, `Grafana` `Web UI` üzerinde de query atabiliriz. Bu query'ler zaten dashboard oluşturmak için kullanılan query'lerle aynıdır.

- `Query Editor`, `Web UI`'daki `explore` sekmesindeki query oluşturmamıza yarayan, auto-complete seçenekleri sunan HTML component'lerdir.

  `Query editor` bir query generate eder. bu query her `data source` için ayrı bir formatta ve mantıktadır. örnek:

  `PostgreSQL` için query:

  ```text
  SELECT hostname FROM host  WHERE region IN($region)
  ```

  `PromQL` için query:

  ```text
  query_result(max_over_time(<metric>[${__range_s}s]) != <state>)
  ```

  Bu sebeple `query editor`, `data source` değiştirildiğinde (seçildiğinde) çok farklı bir component halini alabilir.

- `Grafana` kendi configurasyon'larını (aşağıdaki liste) kaydetmek için `RDBMS` (default'ta `SQLite`) kullanır:
  - user list to login `Grafana` `Web UI`
  - `data source` connection info (`ip`, `port`, username, password...)
  - user generated dashboards

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Prometheus

`Grafana`'nın alternatifidir. `Grafana` ile en temel farkı `Grafana` DB'yi kendi tutmazken, `Prometheus` kendi DB'si mevcuttur (Bu DB time series DB'dir). Fakat `Prometheus`'un visualizing modülü `Grafana` gibi başarılı değildir.

`Prometheus` bağımsız bir servis olarak ayağa kalkar. önceden her servisin health (örneğin Spring'deki `Actuator` endpoint'leri) URL'si `Prometheus`'a tanıtılır. `Prometheus` bu URL'lere giderek düzenli olarak data çeker ve kendi DB'sine kaydeder.

### 📌📌 exporter

`Prometheus` dünyasında `exporter`; okuduğu data'yı `Prometheus`'un anlayacağı formata çeviren yazılımdır. Piyasada neredeyse her yazılım `Prometheus`'a metric'lerini atabilmek için kendi `exporter`'ını geliştirmiştir. örnekler:

- https://github.com/danielqsj/kafka_exporter
- https://github.com/infinityworks/docker-hub-exporter

Bazı yazılımlar ise default'ta direk `Prometheus`'un anlayacağı formatta metric sunmaktadır. örnekler:

- `K8s`
- `RabbitMQ`
- `Zipkin`

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
