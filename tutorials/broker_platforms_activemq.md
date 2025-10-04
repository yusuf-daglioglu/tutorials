# BROKER PLATFORMS ACTIVEMQ

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Topic vs address

Bu konu burada çok net bir şekilde özet olarak anlatılmış: <https://activemq.apache.org/components/artemis/documentation/latest/address-model.html>

Piyasada şöyle bir durum var:

- JMS uses queues and topics
- STOMP uses generic destinations
- MQTT uses topics
- AMQP uses generic nodes

Bu sebepten ActiveMQ şu 3 konsepti kullanır:

- address

  producer buraya yazma işlemi yapar.

- queue

  consumer buradan okur.

  "queue" isimleri global olarak unique olması şart.

- routing type

  her adrese gelen mesajın, ona bağlanmış olan queue'lara nasıl dağıtılacağını belirleyen kuraldır.

  adresten queue'ya mesajı yönlendirirken,
  - eğer routing-type-config'i __anycast__ ise, gelen her mesaj, bu queue'lardan sadece 1'ine yollanır.
  - Oysa routing-type-config'i __multicast__ ise, gelen her mesaj, tüm queue'lara yollanır.

## 📌 server config

ActiveMQ server'ındaki config dosyalarının örnekleri:

active-mq'da address'e yazılıyor ve queue'dan okunuyor. bu sebeple active-mq'da şu ayar olmalıdır:

```xml
<addresses>
  <address name="address1">
    <anycast>
      <queue name="q1"/>
    </anycast>
  </address>
</addresses>
```

multicast örneği:

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

Yukarıdaki multicast örneği genelde bu şekilde implemente edilmez. Genelde aşağıdaki gibi implemente edilir. çünkü active-mq zaten her bağlanan consumer direk adresten okuma yapar. bu sebeple aşağıdaki tercih edilir:

```xml
<addresses>
  <address name="address1">
    <multicast/>
  </address>
</addresses>
```

Yukarıdaki son örnekte, her mesaj, her queue'ya klonlanır. bu tarz queue'lara piyasada __subscription queues__ denir.

## 📌 Fully Qualified Queue Name

JMS "address" kavramını bilmez. JSM'te şunlar vardır:

- Queue = mesajların sıralı gittiği hedef (sadece FIFO yapı)

- Topic = publish/subscribe mantığı

Bu sebeple active-mq'ya bağlanan örnek bir JMS kodu:

```java
// JMS API

// Create a reference of JMS-Queue locally (does not create a real Queue on remote/broker).
Queue jsm_q_1 = session.createQueue("active_mq_address_1::active_mq_queue_1"); // This string name is "Fully Qualified Queue Name".

// From now on, producer and consumer can use the same "jsm_q_1" instance:

MessageConsumer consumer = session.createConsumer(jsm_q_1); // consumer use only "active_mq_queue_1" behind the scenes.

MessageProducer producer = session.createProducer(jsm_q_1); // producer use only "active_mq_address_1" behind the scenes.
```
