# BROKER MQ TERMINOLOGY

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Messaging system

`Messaging system`'a aracılık eden tool'a __messaging broker (⟷ message broker)__ denir.

## 📌 JMS (⟷ Java Message Service)

- It is only API for `Java`.
- It does not define a standard wire (data transfer) format.
- It is part of `J2EE`.
- API allow developers to use both `queue` and `topic` messaging patterns.

`source for all above: <https://activemq.apache.org/components/artemis/documentation/2.0.0/messaging-concepts.html> title: "Java Message Service (JMS)".`

- `JMS` dünyasında `message broker`'lara `JMS provider` deniliyor.
- `Apache ActiveMQ Artemis` is fully compatible with:
  - `JMS` `1.1` and `2.0`
  - `Jakarta Messaging` `2.0` and `3.0`
  
  `source: https://activemq.apache.org/`

## 📌 AMQP (⟷ Advanced Message Queuing Protocol)

`AMQP`'nun farklı versiyonları var.

__wire-level protocol__ network üzerinden atılan data'nın formatının belirlendiği standartlardır. Örneğin; `JMS` bir API'dir, fakat implementasyonun network'ten hangi data ile nasıl haberleşeceğine karışmaz. Fakat AMQP __wire-level protocol__'dür. Data'nın kendisine karışır. `source: https://www.amqp.org/resources/developer-faqs`

__wire-level protocol__ (kelime kökeni bunu ifade etmese de) fiziksel network katmanındaki formatlarla ilgilenmez, onun yerine API-level'daki (daha üst katmanların) yollayacağı data formatını ifade etmek için kullanılır. `TCP` ve `UDP` katmanının wired-level'a girip girmediği bir tartışma konusudur.

## 📌 message queuing vs publish-subscribe (⟷ P/S ⟷ pub/sub)

| messaging model                          | mesaj nerden yollanır | mesajı kimler okur? | `request-reply` (sync messaging) | `fire-and-forget` (async messaging) |
|------------------------------------------|-----------------------|---------------------|----------------------------------|-------------------------------------|
| `publish-subscribe`                      | `topic`               | tüm consumer        | impossible                       | must                                |
| `queuing (⟷ Point-to-Point ⟷ P2P ⟷ PTP)` | `queue`               | 1 adet consumer     | possible                         | possible                            |

`kaynak: (source-id: 388) "Kafka as a Messaging System" başlığı ilk paragraf.`

- `Kafka` yukarıda 2 messaging pattern'i sadece `topic` üzerinden destekliyor. Bu bazen makalelerde terimlerin yanlış anlaşılmasına sebep olabiliyor.

- `Kafka` yukarıda yazan özelliklere ek olarak şu özelliği sunuyor: streaming. Yani offset'i bilen client'lar eski bilgileri istediği okuyabiliyor, yani stream edebiliyor.

## 📌 Kafka vs RabbitMQ

| `message broker` | stream support | `consumer-centric` model?                           | smart dump model               |
|------------------|----------------|-----------------------------------------------------|--------------------------------|
| `kafka`          | yes            | yes. because consumer can fetch any data via index. | `smart-consumer`/`dumb-broker` |
| `RabbitMQ`       | no             | no. it based on `broker-centric` model.             | `smart-broker`/`dumb-consumer` |

`RabbitMQ` mesajları persist edebilir fakat bunların çekilmiş client'lar tarafından tekrar çekilebilmesini sağlayamaz.

`RabbitMQ` mesajların consumer'lara ulaşıp ulaşmadığını garanti edebilir (retry yapabilir).

## 📌 queue vs bus

`bus` genel bir terimdir. `queue` onun özel bir türevidir. 

`queue` mutlaka `FIFO` olmalıdır. fakat `bus`'ta böyle bir zorunluluk söz konusu değildir.

## 📌 event bus için yazılımlar (broker'lar)

- __Apache Kafka__

- __Apache ActiveMQ__

  "`Artemis`" kod adı ile `ActiveMQ` projesi paralelden geliştiriliyor. `Artemis` stabil olduğunda eski proje sonlandırılacak. `Artemis`'in başlaması ile artık eski proje `Classic` olarak adlandırılıyor.

  __Advanced Message Queue Protocol (⟷ AMQP)__ protocol'ü ile çalışır. Fakat `MQTT`, `OpenWire`, `Stomp` protocol'leri ile de çalışabilir.

  - __OpenWire__: __AMQP__'ye çok benzer bir protocol'dür.
  - __OpenWire__ vs __STOMP__: STOMP text based haberleşme sağlar ve OpenWire'a göre yavaştır. OpenWire ise binary haberleşme sağlar ve STOMP'a göre hızlıdır.

- __Open Message Queue (⟷ OpenMQ ⟷ Open MQ)__

  `Oracle` tarafından geliştirilen bir `MQ`.
  
  `GlassFish` içerisinde default olarak geliyor. İstenirse standalone da kullanılabiliyor.

- __OpenJMS__ (not developing)

- __Oracle DB Advanced Queuing (⟷ AQ)__

  `Oracle DB` içinde çalışan bir feature'dir. Verileri tablolarda saklayarak işlem yapar.

- __IBM MQ__

- __RabbitMQ__

  `Pivotal` firması tarafından geliştiriliyor. __AMQP__ protocol'ü ile çalışır.

## 📌 BROKER FEATURES

`Broker` olan sistemlerde şu özelliklerden bahsedebiliriz:

### 📌📌 CONSUMPTION MODE

Terminoloji karmaşası var bu mode isimlerinde. çünkü sdece consumer veya sadece broker tarafından bakılarak isimlendirilme yapılmamış. Ama piyasada bu şekilde kullanılıyor.

2 mode var:

- `push`

  `broker`'ın `push` ettiği model.

- `pull`

  `consumer`'ın `pull` aldığı model.

Client'ın `pull` alması, client'ın kendine fazladan yük bindirmemesi konusunda avantaj sağlamaktadır. Çünkü `broker` sürekli `push` yaparsa sıkıntı olabilir.

### 📌📌 DELIVERY SEMANTICS

- __Producer delivery__

  `Kafka` bunların hepsini destekliyor.

  Aşağıdaki listede aşağıya gittikçe transaction maliyeti artıyor.

  - __at-most-once__: leader mesajı aldığına dair onayı verince, producer'ın işi tamamlanır.
  - __at-least-once__ `Kafka`'da default'tur. Consumer'lardan en az biri mesajı aldığına dair onay verince producer'ın işi biter.
  - __exactly-once__: Consumer'lardan biri mesajı aldığına dair onay verince producer'ın işi biter.

- __Consumer receipt__

  - __At most once__: consumer mesajı aldığı gibi onayı `broker`'a yollar. hata alırsa, mesaj tekrar işlenmez.
  - __At least once__: consumer mesajı işledikten sonra onayı verir.

### 📌📌 ORDERING GUARANTEE

- `no-ordering`

  bazı `broker`'lar mesajların sırasını garanti edemeyebilir. performans açısından büyük avantaj sağlar.

- `partition ordering` or `queue ordering`

  bazı `broker`'lar mesaj sırası garanti edebilir ancak sadece belli kısımları için edebilir. örneğin `Kafka` sadece partition bazında garanti verebiliyor.

- `global ordering`

  performans açısından önemli derecede yavaşlığa sebep olabilir. bu seviyede tüm partition'lar arası sürekli senkronizasyon gerekmektedir.
