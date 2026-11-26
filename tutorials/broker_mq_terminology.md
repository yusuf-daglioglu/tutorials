# BROKER MQ TERMINOLOGY

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Messaging system

`Messaging system`'a aracılık eden tool'a __messaging broker (⟷ message broker)__ denir.

## 📌 JMS (⟷ Java Message Service)

- It is only API for `Java`.
- It does not define a standard data transfer format.
- It is part of `Java EE`.
- API allow developers to use both `queue` and `topic` messaging patterns.

`source for all above: <https://activemq.apache.org/components/artemis/documentation/2.0.0/messaging-concepts.html> title: "Java Message Service (JMS)".`

- `JMS` dünyasında `message broker`'lara `JMS provider` deniliyor.
- `Apache ActiveMQ Artemis` is fully compatible with:
  - `JMS` `1.1` and `2.0`
  - `Jakarta Messaging` `2.0` and `3.0`
  
  `source: https://activemq.apache.org/`

## 📌 message queuing vs publish-subscribe (⟷ P/S ⟷ pub/sub)

| messaging model                          | mesaj nerden yollanır | mesajı kimler okur? | `request-reply` (sync messaging) | `fire-and-forget` (async messaging) |
|------------------------------------------|-----------------------|---------------------|----------------------------------|-------------------------------------|
| `publish-subscribe`                      | `topic`               | tüm consumer        | impossible                       | must                                |
| `queuing (⟷ Point-to-Point ⟷ P2P ⟷ PTP)` | `queue`               | 1 adet consumer     | possible                         | possible                            |

`kaynak: https://kafka.apache.org/intro "Kafka as a Messaging System" başlığı ilk paragraf.`

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

## 📌 protocols

- `OpenWire`, `AMQP (⟷ Advanced Message Queuing Protocol)`'ye çok benzer. `OpenWire` sadece `ActiveMQ` projeleri tarafından destekleniyor.

- `AMQP` vs `STOMP`

  `STOMP` text based haberleşme sağlar ve `AMQP`'a göre yavaştır. `AMQP` ise binary haberleşme sağlar ve `STOMP`'a göre hızlıdır.

## 📌 event bus için yazılımlar (broker'lar)

- `Apache Kafka`

- `Apache Artemis`

- `Apache ActiveMQ`

  `Artemis` projesi ilk başladığında `ActiveMQ` çatısı altında geliştiriliyordu ve ismi `ActiveMQ`'ti. Daha sonra ayrıldılar. Bu sebeple `Artemis` projesi ilk başladığı dönemlerde, `Apache ActiveMQ` yazılımına `classic` suffix'i veriliyordu.

  `AMQP` protocol'ü ile çalışır. Fakat `MQTT`, `OpenWire`, `Stomp` protocol'leri ile de çalışabilir.

- `Open Message Queue (⟷ OpenMQ ⟷ Open MQ)`

  `Oracle` tarafından geliştirilen bir `MQ`.
  
  `GlassFish` içerisinde default olarak geliyor. İstenirse standalone da kullanılabiliyor.

- `OpenJMS` (not developing)

- `Oracle DB Advanced Queuing (⟷ AQ)`

  `Oracle DB` içinde çalışan bir feature'dir. Verileri tablolarda saklayarak işlem yapar.

- `IBM MQ`

- `RabbitMQ`

  `Pivotal` firması tarafından geliştiriliyor. `AMQP` protocol'ü ile çalışır.

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

#### 📌📌📌 Producer delivery

`Kafka` bunların hepsini destekliyor.

Aşağıdaki listede aşağıya gittikçe transaction maliyeti artıyor.

- `at most once` client isteği `Kafka`'ya atar. isteğin bir broker'a ulaşıp ulaşmadığı ile hiç ilgilenmez.
- `at least once`: client sadece `kafka` leader'ın isteği aldığından emin olana kadar bekler.
- `exactly once`: o `partition` için tüm diğer in-sync replica `kafka` node'larının bu record'u aldığından emin olana kadar bekler.

#### 📌📌📌 Consumer receipt

- `At most once`: consumer mesajı aldığı gibi onayı `broker`'a yollar. hata alırsa, mesaj tekrar işlenmez.
- `At least once`: consumer mesajı işledikten sonra onayı verir.

### 📌📌 ORDERING GUARANTEE

- `no-ordering`

  bazı `broker`'lar mesajların sırasını garanti edemeyebilir. performans açısından büyük avantaj sağlar.

- `partition ordering` or `queue ordering`

  bazı `broker`'lar mesaj sırası garanti edebilir ancak sadece belli kısımları için edebilir. örneğin `Kafka` sadece partition bazında garanti verebiliyor.

- `global ordering`

  performans açısından önemli derecede yavaşlığa sebep olabilir. bu seviyede tüm partition'lar arası sürekli senkronizasyon gerekmektedir.
