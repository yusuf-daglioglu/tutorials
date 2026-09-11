# BROKER MQ KAFKA

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Apache Kafka

`Kafka` is open source and developing by `Apache`. But `Confluent`'s `Kafka` is a downstream (fork) of `Apache` `Kafka` which has additional features.

## 📌 Zookeeper

`Kafka` requires `Zookeeper` for the following reasons:

- store leader and follower information
- which nodes are available in cluster
- list of `topic`s
- which `topic`s are storing in which nodes and `partition` lists
- Quotas - how much data is each client allowed to read and write
- `ACL`s - who is allowed to read and write to which `topic` (old high level consumer) - Which consumer groups exist, who are their members and what is the latest offset each group got from each `partition`.

## 📌 Command line app

In the `/kafka-server/bin/` directory there are command line scripts which can easily allow developer to call the APIs of `Kafka` server. Examples:

```sh
## 📌 first we start the Kafka server
/bin/kafka-server-start.sh "config/server.properties"

/bin/kafka-topics.sh --Zookeeper "localhost:2181" --delete --topic "DEMO"

/bin/kafka-console-producer.sh --broker-list "localhost:9092" --topic "sampleTopic"

/bin/kafka-topics.sh --create --Zookeeper "localhost:2181" --replication-factor "1" --partitions "1" --topic "sampleTopic"
```

As it can be seen on above example, operations like creating or removing `topic`s requires `Zookeeper`'s socket information. Because this requests are not sending to `Kafka`, they sending directly to `Zookeeper`. But the commands like consuming or publishing messages are sending directly to `Kafka` server.

`Kafka` can send requests to `Zookeeper` to create or delete `topic`s. Those APIs are considered as admin operations. `https://github.com/obsidiandynamics/kafdrop title: "Running from JAR", paragraph: 2 + https://kafka.apache.org/23/javadoc/org/apache/kafka/clients/admin/KafkaAdminClient.html`

## 📌 Consumer reads via partition and topic

Consumer'lar sadece spesifik `partition`'lar ve `topic`'ten mesaj çekebilir:

```java
// example using spring-boot
@KafkaListener(topicPartitions = @TopicPartition(topic = "topicName", partitions = { "0", "1" }))
```

Fakat `partition` bilgisi vermezse sadece `topic` bilgisi verirse, o zaman `Kafka` server, o client'a kendi bir `partition` atar. `Kafka` yükü tüm client'lara dengeli dağıtır.

## 📌 CustomPartitioner class

`Java`'da producer tarafta `CustomPartitioner` class'ı ile, giden mesajın hangi `partition`'a gideceğini belirleyebiliriz. Mesajın içeriğinden bu ID'yi elde edecek metodu yazmamız şart.

`Partition` ID, interger olmak zorunda.

Fakat eğer `CustomPartitioner` kullanmazsak client şu şekilde mesajı yollar:

- Eğer mesaj key'imiz `null` ise, `topic` yaratılırken oluşturulmuş tüm `partition`'lara `round-robin` ile mesajları yükler.

- Eğer mesaj key'imiz `null` değil ise, onun `hash`'ini alır ve `modulo` işlemine tabi tutar.

## 📌📌 manuel ACK

`spring`'de `ENABLE_AUTO_COMMIT_CONFIG` ayarını false yaparsak, `Java` tarafında manuel onaylama yapmamız gerekir:

```java
@KafkaListener(topics = "your-topic")
public void consumeMessage(String message, Acknowledgment ack) {
    
    // do some business here.
    
    // at the end of the business:
    ack.acknowledge(); 
    // this block also should be also inside "exception catch" blocks. Otherwise kafka will not receive "commited" signal, therefor Kafka will send same message again and again.
}
```

## 📌 record (⟷ message)

- The each message which send by client to `Kafka` server.
- Each record is key-value.
- `Kafka` guarantees the consumption of messages in order (The order is based on `partition` only - not `topic`!).

## 📌 partition

It is the same thing with the `partition` term that we use on other `NoSQL`. In many `NoSQL`, we distribute the data depending on the `partition` ID.

In most `NoSQL`, as default the `partition` ID is the `hash` of the data (on `Kafka` the "`record`"). On `kafka` we can implement `CustomPartitioner` class to choose the `partition` of each record.

Some rules:

- Each `topic` must store at least in one `partition`.
- 1 `partition` must have only 1 `topic`.

`Partition` ID and `topic` are both unique together. In other words, `user-created-topic` has `partition` ID `1`, but `warnings-topic` may also have `partition` ID `1`. Both these `partition`s are completely independent.

`Topic`s are logical concepts, `partition`s are physically divided portions.

`Partition`s are like queues. We can consume a record from `partition` using `partition` id and `topic` name.

To understand clearly we can give real examples:

- `Topic`: `completed-payment-transactions`, `Partition`: `Store ID`
- `Topic`: `order-started`, `Partition`: `Store ID`

Each store should have is equivalent `partition` ID. Examples:

- store id: 100 ---> `partition` id: 1
- store id: 200 ---> `partition` id: 2
- store id: 400 ---> `partition` id: 4

The `partition` ID's on `Kafka` start from zero. So we should find a mathematical operation to find the equivalent `partition` ID from store ID.

`https://kafka-python.readthedocs.io/en/master/ title: "KafkaConsumer", 3rd code example is written on Python.` But the same object exist also on `Java`. `TopicPartition` instance is creating using `topic` name and the `partition` ID. `https://kafka.apache.org/23/javadoc/org/apache/kafka/common/TopicPartition.html`:

```java
TopicPartition(String topic, int partition)
```

## 📌 consumer group

`Kafka` does not store which consumer has read which data. Therefore each client should remember it's own `offset` (`topic` `offset` and `partition` ID) he read last. But `kafka` has another optional feature: `Kafka` stores the last read record (`offset`) for each `consumer group`. Therefore, as a client, if you don't have a specific reason, you don't have to manage `offset`s.

- birden fazla consumer, 1 adet `consumer group` altında olabilir.
- Kafka 1 `topic`'teki `partition`'ları `consumer group`'taki her client'a dengeli dağıtır. örnekler:
  - 4 `partition` varsa, 2 client varsa, ilk 2 `partition`'ı ilk client'a, 2inci ve 3üncü `partition`'ları 2inci client'a dağıtır.
  - 4 `partition` varsa, 5 client varsa, 1 client boşta bekler.
- 1 client 1'den fazla `consumer group`'un mensubu olabilir.
- 1 `topic`'ten birden fazla `consumer group` birbirlerinden bağımsız şekilde, bağımsız `offset`'lerle, aynı `topic`'ten mesaj okuyabilir.
- kafka `consumer group`' bilgisini server'da tutarken şu şekilde tutar:

  - `consumer group`:user-service, `topic`:X, `partition`:1, last-read-offset:99
  - `consumer group`:payment-service, `topic`:X, `partition`:2, last-read-offset:88
  - `consumer group`:order-service, `topic`:Y, `partition`:1, last-read-offset:77

## 📌 offset

`offset` meaning in IT: the distance of an element (point) to the reference (generally: position zero) element (point).

Each record has an `offset` in a specific `partition`. This value never changes.

Yeni `partition`'lar, 0 offset'i ile yaratılır. Fakat yeni mesajlar geldikten sonra, o `partition`'daki eski tüm mesajlar silinirse bile, o `partition`'un first offset'i 0 olmaz. silinen mesajların sayısı kadar olur. 30 mesaj silindiyse, first offset 30 olur.

## 📌 replication factor

her `partition`'un kaç adet broker'da klonlanacağını belirtebiliyoruz.

client mesajı sadece 1 node'a yollar, mesajı alan node, mesajı leader node'a, leader ise diğer follower node'lara yollar.

her `partition` için __leader__ ve __follower (⟷ in-sync replicas ⟷ ISR)__ broker'lar vardır. follower'lar (ilgili `partition` için) sadece data'nın klonlanmasında görevlidirler. aynı zamanda leader çökerse follower'lardan biri leader olarak devreye girmektedir.

## 📌 Add new partitions to existing topics

Bu çok kolay şekilde yapılabiliyor:

```sh
bin/kafka-topics.sh --zookeeper zk_host:port/chroot --alter --topic my_topic_name --partitions 9 
```

Fakat `Kafka` arka planda eski data'ları ilgili `partition`'lara taşımaz.

`./bin/kafka-reassign-partitions.sh` script'i ile var olan `partition`'ları farklı broker'lara taşıma tarzı işlemler yapılabilmektedir.

## 📌 Kafdrop

`Kafka` için açık kaynaklı `Web UI` monitoring uygulaması.

Sadece temel özellikleri içeriyor.

## 📌 MirrorMaker

`Kafka` default'ta (tek başına) multi data center hizmeti mevcut değil. Bunun için `Kafka` sunucusundan bağımsız yazılımlardan yararlanmak gerekiyor. Bunlardan biri de `MirrorMaker`.

`MirrorMaker` `Kafka`'lardan bağımsız olarak ayağa kaldırılıyor. Ayağa kaldırırken tüm data center'lardaki her `Kafka`'nın IP ve port bilgileri config ile `MirrorMaker`'e veriliyor. Aynı zamanda `MirrorMaker`'a, hangi `Kafka`'lardaki `topic`'lerin hangi `Kafka`'lara sync olacağı gibi bilgiler veriliyor. Bu bilgiler doğrultusunda `MirrorMaker` consumer ve producer olarak davranıp, data center'lar arasında bu `Kafka`'ları sync etmeye çalışıyor.

`Confluent` firmasının geliştirdiği `Replicator`, `MirrorMaker` alternatifidir.

## 📌 Kafka connect

`Kafka`'dan bağımsız çalışan bir instance'dır. 2 farklı connector'ü vardır:

- __Source connector__

  bağımsız sistemlerden (örneğin DB'den), data okur ve bunları `Kafka`'da `topic`'lere kaydeder.

- __Sink connector__

  `Kafka` `topic`'lerden okuduğu data'ları bağımsız bir sisteme (örnek: BD'ye) yazar.

## 📌 Kafka Streams

Hem consumer hem de producer için API'dir. Bu API, `Java`'daki stream altyapısına uygun metotlar sunar.
