# JAVA LOGGERS

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 log

log kelimesi `Türkçe`'de `kayıtların bütünü` anlamına geliyor. (ek olarak çok farklı anlamları da var.)

## 📌 java log libraries

| name                                                                         | wrapper for other libraries | reads config from                   | fork of   |
|------------------------------------------------------------------------------|-----------------------------|-------------------------------------|-----------|
| `log4j`                                                                      | no                          | `Log4j.properties` or `log4j.xml`   | -         |
| `log4j` 2                                                                    | no                          | `log4j2.properties` or `log4j2.xml` | `log4j` 1 |
| `Logback`                                                                    | no                          |                                     | `log4j` 1 |
| `Apache Commons logging` (`old-name:Jakarta Commons Logging ⟷ old-name:JCL)` | yes                         |                                     | -         |
| `SLF4J (⟷ Simple Logging Facade for Java)`                                   | yes                         |                                     | -         |
| `JUL` (`java.util.logging`)                                                  | no                          | `logging.properties`                | -         |

- `log4j` 2'nin hiçbir zaman 1inci versiyonu olmadı. Fakat proje sıfırdan yazıldı. yeni repo ve yeni web sitesi ile yayımlandı.

- `Logback` tamamen sıfırdan yazıldı. Fakat aynı geliştiriciler tarafından yazıldığı için fork'u gibi yansıtılıyor.

- `Apache Commons logging`, `Apache commons` ailesi grubu altındadır.

- Eski `JCL` makalelerinde, `Commons logging` diye kullanılıyordu. Bu sebeple `Apache Commons logging` ile karıştırılabiliyor. Oysa vakfa aktarılınca isim değişkliği oldu.

- `SLF4`, `Apache Commons logging` eksiklerini gidermek için çıktı. Ama her 2 proje de devam ediyor.

## 📌 Apache Commons logging vs SL4J

`Apache Commons logging`, `classpath`'te olan bir kütüphaneyi bulur ve log'ları ona yönlendirir. yani kullanılacak logger kütüphanesi `Apache Commons logging` implementasyonu olmak zorunda değildir.

## 📌 some dependencies

`SL4J` ilk başladığında bir implementasyon arar. eğer yok ise; default olarak kendi içinde gelen `no-operation (⟷ NOP ⟷ NOPLogger)` implementasyonunu kullanır.

- `SL4J-simple`: çok basit bir `SL4J` implementasyonudur.

- `SL4J-jdk14`: bir `SL4J` implementasyonudur. `Java.util.logging` kullanmamızı sağlar.

- `SL4J-API`: `SL4J` core kütüphanesinin paket adıdır.

- `SL4J binding`: bu terim `SL4J` implşementasyonunu kullanılması anlamında kullanılır.

- `Log4j-SL4J-impl`: `SL4J`'nin `Log4j` implementasyonu.

- `Log4j-core`: `log4j`'nin çekirdek kodları. `API` ile dışarıdan çağrılması için `Log4j-API` şarttır.

## 📌 Slf4net

`Slf`, facade olduğundan ötürü, birçok dilde aynı `API`'yi destekliyor. bu paket `.Net` için geliştirilmiştir.

## 📌 Log4j12

`Log4j`'nin `1.2`'inci sürümünün paket adıdır. artık geliştirilmemektedir.

## 📌 Logback profile and config example

aşağıdaki dosya opsiyonel olarak farklı profil dosyalarına referans ettirebiliriz:

```xml
<configuration debug="true">
    <springProfile name="prod">
        <include resource="logback-prod.xml"/>
    </springProfile>
    <springProfile name="test">
        <include resource="logback-test.xml"/>
    </springProfile>
    <springProfile name="default,development">
        <include resource="org/springframework/boot/logging/logback/base.xml"/>
    </springProfile>
</configuration>
```

`logback-prod.xml` dosyası örneği:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<included>
    <!-- include ile Spring'in içerisinde gelen default tanımları import ediyoruz-->
    <include resource="org/springframework/boot/logging/logback/base.xml"/>

    <!-- property variable'ı kullanıyoruz. burası Spring properties'ten variable çekmemizi sağlıyor.-->
    <springProperty scope="context" name="application_name" source="Spring.application.name"/>

    <!--console'a basmamızı sağlayan appender. file-appender, yada logstash-tcp-appender gibi alternatifler vardır.-->
    <appender name="flatConsoleAppender" class="ch.qos.logback.core.ConsoleAppender">

        <!--encoder'lar içerisine yazılan bilgileri formatlı yazar. Logstash içerisinde gelen bu encoder ilk Logstash'in kolayca parse edebileceği bir formata çeviriyordu data'ları fakat daha sonrada generic bir encoder halini aldı. -->
        <encoder class="net.logstash.logback.encoder.LoggingEventCompositeJsonEncoder">,

            <!-- providers içerisindeki her data direk output'a gönderilecek anlamına gelir.-->
            <providers>
                <timestamp>
                    <timeZone>UTC</timeZone>
                </timestamp>
                <version/>
                <logLevel/>
                <message/>
                <threadName/>
                <context/>
                <pattern>
                    <pattern>
                        {
                        "my_custom_env": "${my_custom_env}",
                        "java_class": "%C",
                        "trace": {
                            "trace_id": "%mdc{X-B3-TraceId}",
                            "span_id": "%mdc{X-B3-SpanId}",
                            "parent_span_id": "%mdc{X-B3-ParentSpanId}"
                            }
                        }
                    </pattern>
                </pattern>
                <stackTrace/>
            </providers>
        </encoder>
    </appender>

    <!--log 7seviyesi info olan log'ları içerisindeki appender'a yazar.-->
    <root level="info">
      <!--yukarıdaki appender'ımızın name'i-->
        <appender-ref ref="flatConsoleAppender"/>
    </root>
</included>
```

yukarıdaki config ile her satırın (log'un) örnek çıktısı:

```json
{
    "@timestamp": "2020-10-16T10:19:47.741Z",
    "@version": "1",
    "level": "WARN",
    "message": "log message from log.info( message ) ",
    "thread_name": "org.springframework.Kafka.KafkaListenerEndpointContainer#0-0-C-1",
    "application_name": "user-service",
    "hostname": "hostname_of_machine",
    "java_class": "org.Apache.Kafka.clients.NetworkClient",
    "trace": {
        "trace_id": "43344532",
        "span_id": "436546574",
        "parent_span_id": "34645364"
    }
}
```

## 📌 bridge vs migrator

`bridge` paketleri `JUL-to-SL4J` gibi arada `to` olacak şekilde isimlendirilir. `migrator` paketlerinde ise arada `over` keyword'ü yer alır. örnekler:

```text
JCL-over-SL4J

Log4j-over-SL4J

JUL-to-SL4J
```

- `migrator` paketleri `classpath`'te var olan diğer log kütüphanesinin kodlarını (`API`'lerini) ezer ve log'ları `SL4J`'ye aktarır. örnek: `jcl-over-SL4J`: `classpath`'te `JCL` olan kütüphanenin kodlarını ezer ve `SL4J`'ye aktarır.

- `bridge` paketlerinin de amacı aynıdır. fakat farklı şekilde çalışır. örnek üzerinden gidersek: `JUL-to-SL4J` paketi, `classpath`'teki `JUL` `API`'lerine Handler ekler ve log'ları `SL4J`'ye aktarır.

`JUL`'un `migrator` paketi olamaz. çünkü `JUL`, `java.*` paketleri altındadır. bu paketler ezilemez.

## 📌 isteğe bağlı handler'lar

`handler` atanmazsa her zaman default `handler` olur.

```java
val h = new ConsoleHandler

h.setLevel(Level.ALL) // we output ALL the logs to the console

val f = new FileHandler("warn.log", true)

f.setLevel(Level.WARNING) // we also send all WARNING's and more critical's to a file

f.setFormatter(new SimpleFormatter)

l.addHandler(h)

l.addHandler(f)
```

## 📌 Log4j FileAppender vs RollingFileAppender vs DailyRollingFileAppender

`RollingFileAppender` `FileAppender`'dan extend eder ve dosya boyutu maximum boyuta geldiğinde yeni bir dosya oluşturur eskisini backup alır.

`DailyRollingFileAppender` `RollingFileAppender`'ı extend eder ve dosya boyutu maximum boyuta gelince değil, istenilen gün sayısı sonrasında yeni dosyaya geçiş yapar.

## 📌 log levels

`SL4J` ve `Log4j` için:

| Seviye  | Açıklama                                                                                                      |
|---------|---------------------------------------------------------------------------------------------------------------|
| `Trace` | developer'ın hangi satırda olduğunu bildiğinde anlam çıkaracağı kodlar için kullanılır.                       |
| `Debug` | developer'ın satırdan bağımsız log'ları için kullanılır.                                                      |
| `Info`  | developer haricindeki insanların anlaması için log'lar.                                                       |
| `Warn`  | hata değildir. Çok sayıda aynı log varsa, hata anlamına gelecek durumlardır.                                  |
| `Error` | hata durumu.                                                                                                  |
| `Fatal` | Sistemin durması gerektiği veya zarar gördüğü veya sistemin hizmet vermesini anlamsızlaştıracak durumlar için |
| `All`   | Bu bir seviye değildir. Tüm log'ların basılacağını belirtmek için kullanılır.                                 |

Yukarıdaki level'lar kütüphaneden kütüphaneye değişebiliyor. bazılarında bazı level'lar olmuyor, bazılarında yukarıdakilerden daha fazla çeşit level sunuluyor. bazı diğer leveller:

| Level     | Karşılık Gelen Seviye | Level'ın `Türkçe` kelime anlamı |
|-----------|-----------------------|---------------------------------|
| `SEVERE`  | `Error`               | Şiddetli                        |
| `WARNING` | `Warn`                |                                 |
| `CONFIG`  | `Info`                |                                 |
| `FINE`    | `Debug`               |                                 |
| `FINER`   | `Debug`               |                                 |
| `FINEST`  | `Debug` veya `Trace`  |                                 |
| `VERBOSE` | `Trace`               | Gereksiz sözler                 |

## 📌 audit trail (⟷ audit log)

`audit` kelime anlamı: denetim

`trail` kelime anlamı: iz

`audit` log'lar sadece sistemdeki tüm kullanıcıları takip etmek amaçlıdır.

- transaction is created
- user is performing an action

gibi.

## 📌 best practices

- `SLF4` kullanırken;

```java
log.debug("Found " + records);
```

yerine bunu kullanmak daha iyi:

```java
log.debug("Found {}", records);
```

`Log4j`'de, `SL4j` gibi bir metot yok. bu sebeple ilk satırdaki yöntem kullanılmak zorundadır.

- `if(logger.isDebugEnabled)` gibi kodlar kullanılmamalı

- hatalar sadece bu şekilde log'lanmalıdır:

```java
log.error("Error reading configuration file", e);
```

- `log.debug("the name is");`

  `log.debug(name);`

gibi iki satıra kesinlikle yayılmamalı. paralelden gelen log'lar bu ikisi arasına girebilir.

- her sınıfın `toString`'i olmalı ki log'lar düzgünce basılabilsin. fakat gizli bilgiler log'a basılmayacağı için onlara ekstra dikkat edilmeli. gerekirse `toString`'den gizli bilgiler çıkarılmalı veya hash'lenerek print edilmeli.

- `log.info("Processing ", request.size());` request'in `null` gelme durumunda bu kod satırı patlayacaktır. request `null` kontrolü yapılması istenmiyor ise; request'in `toString`'i olmalı ve `size()` metodu kullanılmamalıdır. zaten `logger` `toString`'i çağırdığında, log'ları okuyan kişi dizinini boyutunu da çıkarabilecektir. eğer bu şekilde istenmiyor ise; `null` kontrolü yapılmalıdır.

## 📌 Spring logging

`Spring`'de `Java` sınıflarının bulunduğu paket isimleri ile log level'ı belirleyebiliriz. örnek:

```yaml
logging:
  level:
    ROOT: ERROR  # tüm sistem için genel seviyeyi temsil ediyor
    org.springframework: ERROR
    org.springframework.security: DEBUG
    org.springframework.security.web.FilterChainProxy: ERROR
    com.myapppackages: INFO # kendi sınıflarımız
    org.MongoDB: ERROR   # Üçüncü parti dependency olsa bile bunu yapabiliriz
  file: /Users/myapp/application.log
```

`Spring` kendi içinde `Apache Commons logging` kullanıyor. Fakat default'ta bir implementasyon sunmaz.

Fakat `spring-boot`, default olarak `Apache Commons logging`'i `Logback`'e yönlendirilmektedir ve default olarak `logback-Spring.xml` dosyasından config'leri okur.

## 📌 MDC (⟷ Mapped Diagnostic Context)

`ThreadLocal` olarak çalışır.

`MDC`'ye her log kütüphanesi farklı bir isim verebilir.

`MDC`, log'a basılması için her thread için bilgi içerir. Burada genelde `transaction-id`, `user-id` gibi bilgiler tutulur.

```java
import org.apache.log4j.MDC; // LOG4J 1
import org.apache.logging.log4j.ThreadContext; // LOG4J 2
import org.slf4j.MDC; // SLF4J

MDC.put("transaction.id", tx.getTransactionId());
MDC.put("transaction.owner", tx.getOwner());
// Any business code here
MDC.clearAll(); // clear MDC
```