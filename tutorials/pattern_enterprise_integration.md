# PATTERN ENTERPRISE INTEGRATION

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

Aşağıdaki her pattern'in ayrı ayrı, `Apache` `Camel` projesinde nasıl uygulandığını görmek istersek buradaki dökümantasyonu inceleyebiliriz:

<https://camel.apache.org/components/3.18.x/eips/message-endpoint.html>

Benzer şekilde `Spring Integration` modülü dökümanı'da `EIP`'leri implemente ettiği için okunabilir:

<https://docs.spring.io/spring-integration/docs/4.2.0.M1/reference/html/spring-integration-introduction.html>

## 📌 Integration Styles

### 📌📌 File Transfer

Birden fazla servis birbirleri arasında data transferi yapacağına, file kaydedip aynı yerden diğer servisin okuması sağlanabilir.

### 📌📌 Shared Database

Farklı servisler ortak DB kullanarak da bilgi alışverişi sağlamış olabilirler.

### 📌📌 Remote Procedure Invocation

Direk olarak istek atıp, birbirlerini çağırabilirler. senkron bir haberleşme yöntemidir.

### 📌📌 Messaging

asenkron mesajlaşma biçimidir. Not: `Mesaj` terimi, tüm bu kitapta, kuyruk yolu ile yollanan asenkron giden mesajları kapsamaktadır.

## 📌 Messaging Systems

`Messaging system (⟷ message-oriented middleware ⟷ MOM)`: `ActiveMQ` gibi sistemler.

### 📌📌 Message Channel

`Message Channel`: Topic ve kuyruk konseptlerini temsil eden bir terimdir (bu 2 terimin süper class'ı gibi düşünebiliriz).

#### 📌📌📌 Örnek 1
orders Kafka topic’i order event’leri için kanaldır.

#### 📌📌📌 Örnek 2
RabbitMQ’da orders.exchange -> orders.queue bağı bu iş akışının kanalıdır.

#### 📌📌📌 Kafka
- `Side:` publisher -> broker topic -> consumer.
- `Support:` native.
- `Implementation:` Topic name kanalın kendisidir. Producer yazar, broker saklar, consumer okur.

#### 📌📌📌 ActiveMQ
- `Side:` publisher -> broker destination -> consumer.
- `Support:` native.
- `Implementation:` Queue veya Topic destination kanal görevi görür.

#### 📌📌📌 RabbitMQ
- `Side:` publisher -> exchange -> queue -> consumer.
- `Support:` native.
- `Implementation:` Exchange + binding + queue birlikte kanal davranışı üretir.

#### 📌📌📌 Apache Camel
- `Side:` framework endpoint.
- `Support:` native.
- `Implementation:` jms:queue:orders veya kafka:orders endpoint URI kanal olur.

#### 📌📌📌 JMS
- `Side:` publisher/consumer via provider.
- `Support:` native.
- `Implementation:` Destination nesnesi kanalın API karşılığıdır.

### 📌📌 Message

`Message Channel` üzerinden yollanan data.

### 📌📌 Pipes and Filters

`Filters`: `HTTP` `Servlet`'lerdeki her filter konsepti ile aynı. Asenkron mesajlaşmada da aynı pattern yapılabilir.

`Pipe`: Tüm filter'lardan ilerleyen akışı temsil eden bir terim. "Boru hattı" benzetmesinden geliyor.

`port`: Filter ve pipe arasındaki bağlantıya denir.

#### 📌📌📌 Kafka
- `Side:` consumer or publisher
- `Support:` manual
- `Implementation:` manual coding

#### 📌📌📌 ActiveMQ
- `Side:` consumer or publisher
- `Support:` manual
- `Implementation:` manual coding

#### 📌📌📌 RabbitMQ
- `Side:` consumer or publisher
- `Support:` manual
- `Implementation:` manual coding
- Ek not: sadece header bazlı routing broker tarafından native destekleniyor.

#### 📌📌📌 Apache Camel
- `Side:` consumer or publisher
- `Support:` native
- `Implementation:` Camel tek route içinde validate, transform, filter, recipient list gibi adımları zincir halinde çalıştırabilir.

#### 📌📌📌 JMS
- `Side:` consumer or publisher
- `Support:` manual
- `Implementation:` manual coding

### 📌📌 Message Router

mesajı belirlediğimiz condition'lara göre farklı kuyruklara produce eder.

mesajda manipülasyon yapma bu pattern içinde değildir.

#### 📌📌📌 Kafka
- `Side:` consumer or publisher
- `Support:` manual
- `Implementation:` manual coding

#### 📌📌📌 ActiveMQ
- `Side:` consumer or publisher
- `Support:` manual
- `Implementation:` manual coding

#### 📌📌📌 RabbitMQ
- `Side:` broker
- `Support:` native.
- `Implementation:` sadece header bazlı karar broker'da native yapılır.

#### 📌📌📌 Apache Camel
- `Side:` framework route.
- `Support:` native.
- `Implementation:` Camel `choice()` ile body ve header'a bakar ve doğru endpoint’e yollar.

#### 📌📌📌 JMS
- `Side:` consumer
- `Support:` partly native.
- `Implementation:` sadece header bazlı karar consumer tarafta native yapılır.

### 📌📌 Message Translator

Data transformasyonu gerektiren durumlarda kullanılan bir filter'dır.

Bu `adapter pattern` kullanır. Çünkü bir mesajı farklı bir sisteme adapte etmemizi sağlamaktadır.

#### 📌📌📌 Kafka
- `Side:` consumer or publisher
- `Support:` manual
- `Implementation:` manual coding

#### 📌📌📌 ActiveMQ
- `Side:` consumer or publisher
- `Support:` manual
- `Implementation:` manual coding

#### 📌📌📌 RabbitMQ
- `Side:` consumer or publisher
- `Support:` manual
- `Implementation:` manual coding

#### 📌📌📌 Apache Camel
- `Side:` framework route.
- `Support:` native.
- `Implementation:` Camel transform, marshal, unmarshal, bean mapper ile çeviri yapar.

#### 📌📌📌 JMS
- `Side:` consumer or publisher
- `Support:` manual
- `Implementation:` manual coding

### 📌📌 Message Endpoint

client taraftaki (hem alıcı hem vericide aynı modül vardır), `MOM`'a istek yapacak olan modülün ismidir.

`örneğin:` `@KafkaConsumer` annotation'u bir `Message Endpoint`'tir.

## 📌 Messaging Channels

### 📌📌 Point-to-Point Channel

bu teknikte birden fazla receiver vardır. fakat 1 mesajı sadece 1 tane receiver okuyabilir.

#### 📌📌📌 Kafka
- `Side:` consumer configuration
- `Support:` native
- `Implementation:` consumer group verilirse, sadece 1 consumer mesajı okuyabilir.

#### 📌📌📌 ActiveMQ
- `Side:` broker queue dispatches.
- `Support:` native.
- `Implementation:` Queue bir mesajı tek consumer’a yollar.

#### 📌📌📌 RabbitMQ
- `Side:` broker queue dispatches.
- `Support:` native.
- `Implementation:` Queue worker model ile mesaj bir consumer’a gider.

#### 📌📌📌 Apache Camel
- `Side:` framework route over queue endpoint.
- `Support:` native-friendly.
- `Implementation:` Camel queue endpoint’ten okur; underlying provider tek consumer dağıtımı yapar.

#### 📌📌📌 JMS
- `Side:` provider queue + consumers.
- `Support:` native.
- `Implementation:` Queue destination point-to-point davranışı verir.

### 📌📌 Publish-Subscribe Channel

the sender broadcast an event to all interested receivers.

#### 📌📌📌 Kafka
- `Side:` publisher writes once, many consumer groups read.
- `Support:` native.
- `Implementation:` Aynı topic’i farklı consumer group’lar bağımsız okur.

#### 📌📌📌 ActiveMQ
- `Side:` broker topic distributes.
- `Support:` native.
- `Implementation:` Topic subscriber’lara mesajı fan-out eder.

#### 📌📌📌 RabbitMQ
- `Side:` broker exchange distributes.
- `Support:` native.
- `Implementation:` Fanout exchange veya topic exchange bağlı queue’lara kopyalar.

#### 📌📌📌 Apache Camel
- `Side:` framework route.
- `Support:` native-friendly.
- `Implementation:` Multicast veya publish-subscribe route kullanılabilir.

#### 📌📌📌 JMS
- `Side:` provider topic + subscribers.
- `Support:` native.
- `Implementation:` Topic ile birçok subscriber aynı mesajı alır.

### 📌📌 Datatype Channel

Different message types use different channels.

tüm sistemler bunu destekler. örneğin; `Kafka`'da `topic` mantığı.

### 📌📌 Invalid Message Channel

Consumer bir mesajı aldı fakat bu mesajı invalid olduğu için farklı kuyruğa atması.

### 📌📌 Dead Letter Channel (⟷ DLC ⟷ dead message queue ⟷ DMQ ⟷ Dead Letter queue ⟷ DLQ)

`MOM` bir mesajı aldı fakat bu mesajı invalid olduğu için farklı kuyruğa atması.

### 📌📌 Guaranteed Delivery

Her `MOM`'un kendi kalıcı DB'si vardır. Dolayısı ile crash olduklarında bilgi kaybı yaşanmaz.

### 📌📌 Channel Adapter

Producer veya Consumer taraftaki mesaj yollama ve alma modülüdür. Burası direk MOM'a networkten bağlantı kuran kodlardır.

`Message Endpoint` ise sadec bir API'dir. `Message Endpoint` bizim için `Channel Adapter`'ı çağırır.

### 📌📌 Messaging Bridge

bir `MOM`'dan başka bir `MOM`'a bilgi atma işidir. Bunu broker tarafta plugin'lerle yapabiliriz. Veya manuel bir app yazarız, `X` topic'ten okur başka bir `MOM`'daki `Y` topic'ine mesajı yollar.

### 📌📌 Message Bus

`MOM` üzerinde paylaşılan topic ve ona bağlı tüm diğer süreçler (filter, routing gibi) hepsi bir `Message Bus`'tır.

## 📌 Message Construction

### 📌📌 Command Message

Command pattern'in message üzerinden tetiklenerek uygulanmasıdır. sanal bir kavramdır.

### 📌📌 Document Message

"File Transfer" veya "Shared Database" kullanmak yerine bilgiyi mesaj ile yollama tekniğidir. sanal bir kavramdır.

### 📌📌 Event Message

bir event'in gerçekleştiğini bildirmek için kullanılan mesajlar. sanal bir kavramdır.

### 📌📌 Request-Reply

Başka dosyada anlatılıyor.

### 📌📌 Return Address

Request-Reply kullanırken, cevabın nereye döneceğini, request'imizde yollarız.

### 📌📌 Correlation Identifier

mesajın header'ına bir Correlation Identifier koyulur. bu şekilde reply-yapıldığında bu ID'yi gören servis hangi mesajın response'u olduğunu anlayabilir.

### 📌📌 Message Sequence

bazı mesajların boyutu büyük olabiliyor. bu gibi durumlarda mesajı parçalayıp yollayabilir. her parçaya ise Message Sequence koymalıyız. Her mesajda bu sequence bilgisi ile ilgili şu 3 bilgi mutlaka olmalıdır:

- sequence identifier (bu mesajın hangi sequence'a ait olduğunun bilgisi)
- position (ilgili sequence içerisinde kaçıncı position'da olduğu)
- Size or End indicator (tüm mesajın kaç adet mesajdan oluştuğu. Eğer bu bilgiyi yollamaz ise; o zaman sonuncu parçada "sonuncu" olduğu gösteren bir bilgi yollamalıyız.)

### 📌📌 Message Expiration

Her mesajda mesajın son işlenme tarihi koyulabilir. İstenirse Dead Letter Channel'a da bu expire etmiş mesajlar yollanabilir.

time-to-live desteği olan broker'larda bu özellik native desteklenir.

### 📌📌 Format Indicator

her mesajda mesajın formatı da yollanmalıdır. bu şekilde ileride payload'ın formatı değiştiğinde, mesajı alan taraf kolaylıkla (belki de hiç değişiklik yapmadan) uyum sağlayabilir.

header yapısı çoğu `MOM` tarafından desteklenir. Bu sayede bu özellik native desteklenmiş olur.

## 📌 Message Routing

### 📌📌 Content-Based Router

mesajın içeriğine göre farklı bir yere routing yapılmasıdır.

### 📌📌 Message Filter

mesajın içeriğine göre mesajın hiç işlenmemesi sağlanabilir. genelde gereksiz işlemlerde bu olur. kampanya servisinin 1000$ altındaki siparişlerde işlemi ignore etmesi gibi...

### 📌📌 Dynamic Router

runtime'da topic'in belirlenebilmesidir. bu tüm `MOM`'lar tarafından desteklenir. zaten client ayağa kalkerken nerden mesaj alacağını ve vereceğini kendi belirler.

### 📌📌 Recipient List

birden fazla topic'e aynı mesajı yollama yeteneğidir.

### 📌📌 Splitter

consumer tarafta olan bir özelliktir.

mesaj process edilmeden önce parçalara bölünür. her parça ayrı ayrı yönetilebilir. örneğin; bir siparişteki, her kalemi ayrı ayrı işlemek ve tedarikçilere yollamak isteyebiliriz.

`Apache` `Camel` bunu şöyle destekliyor:

```java
from("activemq:my.queue")
    .split(xpath("//foo/bar"))
        .to("file:some/directory")
```

Yukarıda consume edilen mesaj'daki `XML`, `Xpath` ile parçalara bölünüyor.

### 📌📌 Aggregator

splitter'ın tersi görevi görür. birçok kalem'i toplarız ve kampanya servisine toplu atmak isteyebiliriz. çünkü kampanya, sadece toplam tutar üzerinden hesaplanacaktır.

### 📌📌 Resequencer

consumer tarafta olan bir yetenektir.

bazı durumlarda birden fazla mesaj alırız ve bu mesajların farklı bir sıra ile işlenmesini isteyebiliriz. Bu durum genelde; bizim sistemimiz dışından gelen mesajların olduğu sistemlerde görülür. böyle bir durumda bu mesajları işlenecek sıraya sokan modül kullanmamız gerekir. bu modül Resequencer'dır.

### 📌📌 Composed Message Processor

Splitter ve Aggregator karışımıdır.

### 📌📌 Scatter-Gather

birden farklı topic'e mesaj atma, ve sonrasında bunlardan sebep oluşan event'leri toplama işlemi.

bu süreç, genelde birkaç fonksiyonalitenin birlikte kullanılması ile yapılabilir.

### 📌📌 Routing Slip

The message carries the list of next steps. consumers must read the slip and call the actions.

burada Routing Slip, topic listesi değildir. alınacak business aksiyonlarının listesidir.

### 📌📌 Process Manager

Bu aslında `MOM` konusundan bağımsız.

Mesajı ilk `Process Manager` yazılımı okur. Artık buradan sonra `saga` veya transactional olarak çalışabilir. sequential or parallel olarak çalışabilir.

bu farklı bağımsız servislere istek atıp cevap alır.

`Apache` `Camel` bu pattern'i `Dynamic Router` ile çözdüğünü belirtiyor.

### 📌📌 Message Broker

`MOM`'un kendisidir.

## 📌 Message Transformation

### 📌📌 Envelope Wrapper

Mesaj payload'ı header ile wrap edilmelidir.

header yapısını tüm `MOM`'lar destekliyor.

### 📌📌 Content Enricher

örnek: Consumer customerId=77 içeren mesajı alır, DB’den müşteri segmentini okur, mesaja segment=gold ekler.

### 📌📌 Content Filter

Bazen mesajları yollarken filtreden geçirmek isteyebiliriz. Bunun bazı sebepleri:

- güvenlik gereği her bilginini karşıya gitmesini önlemek.
- tree yapısında olan bir mesajı, flatten hale getirmek.
- tamamen gereksiz olduğunu düşündüğümüz kısımların silinmesi.
- network'ü daha az yormak.
- gibi.

### 📌📌 Claim Check

Bir mesajı trace ettiğimizde, ilgili mesaj sonrası farklı mesajlarla business'ın devam ettiğini görebiliriz. Bu gibi durumlarda, bazı data'lar tüm ara servislerde kullanılmıyor olabilir. böyle bir durumda bu data'yı DB'ye kaydederiz. Bunun karşılığında bir key'i mesaja ekleriz. Bu şekilde network'ü yormamış oluruz.

### 📌📌 Normalizer

Farklı formatlarda gelen fakat aynı anlamı taşıyan mesajları tek bir formata çeviren katmandır.

kelime anlamınca; input mesajı, şirket içindeki `norm`'lara uygun hale çevrilmiş olur.

### 📌📌 Canonical Data Model

canonical kelime anlamı: kurallara uygun, geleneklere uygun.

Tüm servisler ortak bir model kullanması durumudur.

`Normalizer` dönüştürücü iken ve sadece bir consumer tarafında olan bir kod parçası iken, `Canonical Data Model` birçok yazılım için ortak bir modeldir.

## 📌 Messaging Endpoints

### 📌📌 Messaging Gateway

mesaj atacak kod, mesajlaşma sisteminden bi haber olmalıdır. bu sebeple `orderCanceled(order)` şeklinde metotların olduğu sınıf olmalıdır. bu sınıf kendi içinde mesajlaşma sistemini direk çağıran kodlar içermelidir. İşte bu (`orderCanceled` metodunun olduğu) sınıf `Messaging Gateway` olarak adlandırılır.

### 📌📌 Messaging Mapper

mesaj objesi genelde domain obje'sini wrap eder ve bazı ufak farklılıklar içerir. bu iki obje arasında mapping yapan sınıflara `Messaging Mapper` denir.

### 📌📌 Transactional Client

producer `API`'si ve `MOM`'un desteklemesi gerek bir özleliktir. Şöyle bir `API` sunulabiliyorsa bu pattern destekleniyordur:

```java
producer.send(message);
producer.commit();
```

Böylece herhangi bir üçüncü parti transaction manager, producer tarafta farklı kaynakları (bu kaynaklardan biride `MOM`) aynı transaction'daymış gibi yönetebilir.

### 📌📌 Polling Consumer

consumer, istediği zaman mesaj almak istediğini mesaj atan tarafa iletir.

Bu tekniğe `synchronous receiver` da denir. çünkü receiver'daki thread, mesaj gelene kadar bloklanır.

### 📌📌 Event-Driven Consumer

`polling`'in aksine, mesajı atan taraf yeni bir thread tetikler.

bu tekniğe `asynchronous receiver`'da denir.

### 📌📌 Competing Consumers

`Competing` kelime anlamı: rekabet.

Farklı servisler (aynı servisin farklı instance'ları dahil) aynı mesajı okumak isteyebilir. `Competing Consumers` rolü burada consumer'lardadır. çünkü birbirleri arasında mesajı okumak için rekabet etmektedirler.

bu gibi durumlarda mesajın sadece 1 kere iletildiğinden emin olunmalıdır.

### 📌📌 Message Dispatcher

consumer 2 modülden oluşur:
- `Message Dispatcher`
- `performer`

mesajı ilk `Message Dispatcher` karşılar. onu `performer`'lara dağıtır. `performer` kendi başına 1 thread veya process olabilir.

### 📌📌 Selective Consumer

Consumer tarafta bir filter yazarız. Bu filter'da filtreleme yapar ve ona göre mesajın işlenip işlenmeyeceğine karar verebiliriz. Bu şekilde gereksiz mesajlar işlenmez. Örneğin; kampanya servisi 1000$ altındaki hiçbir şey için kampanya sunmayacak ise, sipariş-updated event'lerindeki toplam tutara göre filtreleme yapabiliriz.

### 📌📌 Durable Subscriber

consumer ayakta değilken, `MOM` onun okuyacağı mesajları saklar ve aktif olduğunda ona gönderir.

### 📌📌 Idempotent Receiver

mesajı alan servis, tekrar aynı mesajı okuduğunda hiçbir aksiyon almamalıdır.

### 📌📌 Service Activator

servislerimiz dinamik olarak aktif edilip kapatılabilir olmasını isteyebiliriz. bu gibi durumlarda, dinamik olan sevislerimizin önüne bir "servis activator" katmanı koyarız. activator, reflection ile veya hardcode yöntemlerle dinamik olan servisler, aktif edilmiş ise (classpath'te var ise, veya true/false config ile) ilgili servisi çağırır.

## 📌 System Management

### 📌📌 Control Bus

merkezi bir modüldür. tüm component'leri control eder ve monitoring'lerini sağlar. `System Management` başlığı altındaki tüm pattern'ler `control bus` tarafından yönetilir.

`Control bus` aracılığı ile her component'e config bilgisini de yollayabiliriz. örnekler: log seviyesi, IP/address değişiklikleri, timeout süreleri gibi.

`Control bus` her component ile haberleşmesini bağımsız bir kanal üzerinden yapar. bu sebeple, tüm sistemde 2 bağımsız kanal vardır:

- `Application Message Flow`
- `Control Bus`

### 📌📌 Detour

Detour kelime anlamı: Yol Değişikliği

bazı mesajlar ek validation gibi ekstra işlemere tabi tutulur.

### 📌📌 Wire Tap

wire tap kelime anlamı: hattı (örnek telefon) dinlemek.

gelen mesajı 2 yere yollar. 1 tanesi asıl yollamak istediğimiz destination'dır. diğer'i ise monitoring, logging, test, debug gibi custom amaçlarla kullanacağımız yerdir.

### 📌📌 Message History

her mesaj okuyan servis, header'a bilgi atarsa, geçmiş takip edilebilir olacaktır.

### 📌📌 Message Store

her attığımız mesajı bağımsız bir storage'ye kaydedebiliriz.

### 📌📌 Smart Proxy

`Producer <---> smart proxy <---> MOM`

Her producer instance'ı, kendi için unique isimde bir topic oluşturuyor. sonra producer, smart proxy'ye request-reply mesajı atıyor. bu mesajda, producer'ın unique topic ismi de var. smart proxy bu `MOM`'dan gelen reply'i okuyor, ve unique topic'e atıyor. böylece sadece isteği yapan instance, kendi cevabını okumuş oluyor.

### 📌📌 Test Message

`MOM`'a test mesajı bırakabilir.

### 📌📌 Channel Purger

developer'ın belirleyeceği koşullara göre bir topic'teki mesajları siler.
