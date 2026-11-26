# BROKER MQ ACTIVEMQ RABBITMQ

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 RabbitMQ

Producer `exchange`'e mesaj atar. `routing key` bilgisini opsiyonel atar.

Server taraf, binding kurallarına göre (`routing key` wildcard, `routing key` boş ise gibi) ilgili mesajı `queue`'lere yönlendirir.

Bu sebeple producer tarfata `JSM` `API` kullanırken hangi `Destination` (interface) implementasyonu kullandığımızı bir önemi kalmıyor.

`Consumer` taraf sadece `queue`'te okur.

şu kavramlar yok:
- `address`
- `topic`
- `routing type`

## 📌 ActiveMQ Artemis

- `address`

  `producer` buraya yazma işlemi yapar.

- `queue`

  `consumer` buradan okur.

  `queue` isimleri global olarak unique olması şart.

- `routing type`

  her `address`'e gelen mesajın, ona bağlanmış olan `queue`'lara nasıl dağıtılacağını belirleyen kuraldır.

  Bu kuralı client belirler. Ama eğer server'da tersi ayar için zorla ayar yapılmış ise, server hata fırlatır.

  `address`'ten, `queue`'ya mesajı yönlendirirken;
  - eğer `routing type config`'i `anycast` ise, gelen her mesaj, bu `queue`'lardan sadece 1'ine yollanır.
  - Oysa `routing type config`'i `multicast` ise, gelen her mesaj, tüm `queue`'lara yollanır.

## 📌 JMS ve Apache ActiveMQ (Classic)

`JMS` ve `Apache ActiveMQ (Classic)` için mimari aynı.

`Destination` terimi var sadece. Bunun iki çeşidi var: `queue` ve `topic`.

şu kavramlar yok:

- `address`

  address kavramı olmadığı için, `ActiveMQ Artemis`'teki gibi `routing type` diye özel bir terim kullanılmıyor. ama aynı işlev, producer taraftan server'a zaten `Destination`'ın tipi ile gitmiş oluyor. `ActiveMQ Artemis` ile `JMS`'in yapısı farklı.

## 📌 Fully Qualified Queue Name (FQQN)

Bu pek tavsiye edilmiyor ama istersen producer veya consumer FQQN ile sadece spesifik 1 kuyruğa yazabilir veya okuyabilir.

```java
// JMS API

// Create a reference of JMS-Queue locally (does not create a real Queue on remote/broker).
Queue jsm_q_1 = session.createQueue("active_mq_address_1::active_mq_queue_1"); // This string name is FQQN.

// From now on, producer or consumer can use the same "jsm_q_1" instance.
```

## 📌 server config

`ActiveMQ` server'ındaki config dosyalarının örnekleri:

`activemq`'da `address`'e yazılıyor ve `queue`'dan okunuyor. bu sebeple `activemq`'da şu ayar olmalıdır:

```xml
<addresses>
  <address name="address1">
    <anycast>
      <queue name="q1"/>
    </anycast>
  </address>
</addresses>
```

`multicast` örneği:

```xml
<addresses>
  <address name="address1">
    <multicast>
      <queue name="q1"/>
      <queue name="q2"/>
    </multicast>
  </address>
</addresses>
```

Yukarıdaki `multicast` örneği genelde bu şekilde implemente edilmez. Genelde aşağıdaki gibi implemente edilir. çünkü `activemq` zaten her bağlanan consumer direk `address`'ten okuma yapar. bu sebeple aşağıdaki tercih edilir:

```xml
<addresses>
  <address name="address1">
    <multicast/>
  </address>
</addresses>
```

Yukarıdaki son örnekte, her mesaj, her `queue`'ya klonlanır. bu tarz `queue`'lara piyasada `subscription queues` denir.

## 📌 kuyruk isimlerinde noktalı syntax (Kafka vs JMS)

`Kafka`'da noktalı topic ismi örnekleri görmeyiz. Oysa diğerlerinde noktalıdır. Bu bi zorunluluk değil. Fakat `JMS` bazlı broker'larda sunucu tarafta, mesajın doğru kuyruğa yönlendirmeleri yapılırken wildcard özlelikleri de kullanılabiliyor. Bu sebeple noktalı olunca daha okunabilir olur. örnek: `common.order*`.