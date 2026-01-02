# PATTERN ENTERPRISE INTEGRATION

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

Aşağıdaki her pattern'in ayrı ayrı, Apache Camel projesinde nasıl uygulandığını görmek istersek buradaki dökümantasyonu inceleyebiliriz: <https://camel.apache.org/components/3.18.x/eips/message-endpoint.html>

Benzer şekilde `Spring Integration` modülü dökümanı'da `EIP`'leri implemente ettiği için okunabilir: <https://docs.spring.io/spring-integration/docs/4.2.0.M1/reference/html/spring-integration-introduction.html>

Aşağıda kitapta anlatılan birçok pattern'ler açıklanmıştır. Piyasadaki yazılımlar bu pattern'leri aynı isimde implemente etmiyorlar. Bu sebeple kitapta birbirlerine denk gelen özellikleri gösterebilmek için şöyle bir tablo bulunmaktadır:

kaynak: `authors:Gregor Hohpe & Bobby Woolf, book name:EIP, title:Introduction, sub-title:"Commercial Messaging Systems".`

| `EIP` (Book)              | `JMS`                            | `Microsoft` `MSMQ` | `WebSphere MQ` | TIBCO                | WebMethods           | SeeBeyond            | Vitria                    |
|---------------------------|----------------------------------|--------------------|----------------|----------------------|----------------------|----------------------|---------------------------|
| Message Channel           | Destination                      | MessageQueue       | Queue          | Topic                |                      | Intelligent Queue    | Channel                   |
| Point-to-Point Channel    | Queue                            | MessageQueue       | Queue          | Distributed Queue    |                      | Intelligent Queue    | Channel                   |
| Publish-Subscribe Channel | Topic                            |                    |                | Subject              |                      | Intelligent Queue    | publish-subscribe Channel |
| Message                   | Message                          | Message            | Message        | Message              | Document             | Event                | Event                     |
| Message Endpoint          | MessageProducer, MessageConsumer |                    |                | Publisher,Subscriber | Publisher,Subscriber | Publisher,Subscriber | Publisher,Subscriber      |

## 📌 Integration Styles

### 📌📌 File Transfer

Birden fazla servis birbirleri arasında data transferi yapacağına, file kaydedip aynı yerden diğer servisin okuması sağlanabilir.

### 📌📌 Shared Database

Farklı servisler ortak DB kullanarak da bilgi alışverişi sağlamış olabilirler.

### 📌📌 Remote Procedure Invocation

Direk olarak istek atıp, birbirlerini çağırabilirler. senkron bir haberleşme yöntemidir.

### 📌📌 Messaging

asenkron mesajlaşma biçimidir. Not: "Mesaj" terimi, tüm bu kitapta, kuyruk yolu ile yollanan asenkron giden mesajları kapsamaktadır.

## 📌 Messaging Systems

### 📌📌 Message Channel

__Messaging system (⟷ message-oriented middleware ⟷ MOM)__: ActiveMQ gibi sistemler.

__Message Channel__: Topic ve kuyruk konseptlerini temsil eden bir terimdir (bu 2 terimin süper class'ı gibi düşünebiliriz).

### 📌📌 Message

Message Channel üzerinden yollanan mesaj.

### 📌📌 Pipes and Filters

__Filters__: HTTP Servlet'lerdeki her filter konsepti ile aynı. Asenkron mesajlaşmada da aynı pattern öneriliyor.

__Pipe__: Tüm filter'lardan ilerleyen akışı temsil eden bir terim. "Boru hattı" benzetmesinden geliyor.

__port__: Filter ve pipe arasındaki bağlantıya denir.

### 📌📌 Message Router

mesajı consume eder ve belirlediğimiz condition'lara göre farklı kuyruklara produce eder.

mesajda manipülasyon yapmaz.

### 📌📌 Message Translator

Data transformasyonu gerektiren durumlarda kullanılan bir filter'dır.

Bu "adapter pattern" kullanır. Çünkü bir mesajı farklı bir sisteme adapte etmemizi sağlamaktadır.

### 📌📌 Message Endpoint

client taraftaki (hem alıcı hem vericide aynı modül vardır), MOM'a istek yapacak olan modülün ismidir.

## 📌 Messaging Channels

### 📌📌 Point-to-Point Channel

bu teknikte birden fazla receiver vardır. fakat 1 mesajı sadece 1 tane receiver okuyabilir.

### 📌📌 Publish-Subscribe Channel

the sender broadcast an event to all interested receivers.

### 📌📌 Datatype Channel

Kafka'daki 'topic' terimine denk gelmektedir.

### 📌📌 Invalid Message Channel

Receiver bir mesajı aldı fakat bu mesaj invalid. Bu mesajı, receiver'ın kendisi, Invalid Message Channel'a (burası da bir kuyruk) atmalı.

### 📌📌 Dead Letter Channel (⟷ DLC ⟷ dead message queue ⟷ DMQ⟷ Dead Letter queue ⟷ DLQ)

`Messaging system`'ın invalid olarak algıladığı mesajları `DLC`'ye yollar.

`Microsoft`'un sitesindeki doc'ta bu pattern aynı amaçla kullanılıyor. fakat ek olarak, receiver'larda eğer bu mesajı kabul etmezse yine bu kuyruğa yolluyor.

### 📌📌 Guaranteed Delivery

Her `messaging system`'ın kendi DB'si vardır. Dolayısı ile crash olduklarında bilgi kaybı yaşanmaz.

### 📌📌 Channel Adapter

Client taraftaki mesaj yollama modülüdür.

### 📌📌 Messaging Bridge

Message-system'dan message-system'a mesaj aktaran modüldür. Apache Camel'da bu şekilde yapan bir Java uygulaması mesajı direk diğer message-system'e forward etmektedir:

```java
from("mq:queue:foo")
.to("activemq:queue:foo")
```

### 📌📌 Message Bus

ActiveMQ'ya denk geliyor.

## 📌 Message Construction

### 📌📌 Command Message

Command pattern'in message üzerinden tetiklenerek uygulanmasıdır.

### 📌📌 Document Message

"File Transfer" veya "Shared Database" kullanmak yerine bilgiyi mesaj ile yollama tekniğidir.

### 📌📌 Event Message

Document Message veya Command Message olmayan ve bir olayı temsil eden mesajlardır.

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

### 📌📌 Format Indicator

her mesajda mesajın formatı da yollanmalıdır. bu şekilde ileride payload'ın formatı değiştiğinde, mesajı alan taraf kolaylıkla (belki de hiç değişiklik yapmadan) uyum sağlayabilir.

## 📌 Message Routing

### 📌📌 Content-Based Router

mesajın içeriğine göre farklı bir yere routing yapılmasıdır.

### 📌📌 Message Filter

mesajın içeriğine göre mesajın hiç işlenmemesi sağlanabilir. genelde gereksiz işlemlerde bu olur. kampanya servisinin 1000$ altındaki siparişlerde işlemi ignore etmesi gibi...

### 📌📌 Dynamic Router

her ayağa kalkan receiver, ayrı ayrı `Messaging system`'a bilgi gönderir. bu bilgide hangi mesajları receive etmek istedikleri yer alır. bu bilgiler "__control channel__" üzerinden `Messaging system`'a yollanır.

### 📌📌 Recipient List

birden fazla receiver'a aynı mesajı yollama durumudur.

### 📌📌 Splitter

mesaj işlenmeden önce parçalara bölünür. her parça ayrı ayrı yönetilebilir. örneğin; bir siparişteki, her kalemi ayrı ayrı tedarikçilere yollamak isteyebiliriz.

### 📌📌 Aggregator

splitter'ın tersi görevi görür. birçok kalem'i toplarız ve kampanya servisine toplu atmak isteyebiliriz. çünkü kampanya, sadece toplam tutar üzerinden hesaplanacaktır.

### 📌📌 Resequencer

bazı durumlarda birden fazla mesaj alırız ve bu mesajların farklı bir sıra ile işlenmesini isteyebiliriz. Bu durum genelde; bizim sistemimiz dışından gelen mesajların olduğu sistemlerde görülür. böyle bir durumda bu mesajları işlenecek sıraya sokan modül kullanmamız gerekir. bu modül Resequencer'dır.

### 📌📌 Composed Message Processor

### 📌📌 Scatter-Gather

### 📌📌 Routing Slip

### 📌📌 Process Manager

### 📌📌 Message Broker

broker kullandığımız messaging system'a denk geliyor. 1 broker içerisinde, birçok queue olabilir.

## 📌 Message Transformation

### 📌📌 Envelope Wrapper

Mesaj payload'ı header ile wrap edilmelidir.

### 📌📌 Content Enricher

Bazı mesajlarda bazı kısımlar bazen istediğimiz bilgileri içermeyebiliyor. Örneğin; şehir kodunu yolluyor fakat bu mesajı okuyacak olan birçok servis mutlaka şehir ismine de ihtiyacı oluyor. Bu gibi durumlarda şehir kodundan şehir ismini DB'den fetch eden bir "Content Enricher" katmanı kullanabiliriz.

### 📌📌 Content Filter

Bazen mesajları yollarken filtreden geçirmek isteyebiliriz. Bunun bazı sebepleri:

- güvenlik gereği her bilginini karşıya gitmesini önlemek.
- tree yapısında olan bir mesajı, flatten hale getirmek.
- tamamen gereksiz olduğunu düşündüğümüz kısımların silinmesi.
- network'ü daha az yormak.
- gibi...

### 📌📌 Claim Check

Bir mesajı trace ettiğimizde, ilgili mesaj sonrası farklı mesajlarla business'ın devam ettiğini görebiliriz. Bu gibi durumlarda, bazı data'lar tüm ara servislerde kullanılmıyor olabilir. böyle bir durumda bu data'yı DB'ye kaydederiz. Bunun karşılığında bir key'i mesaja ekleriz. Bu şekilde network'ü yormamış oluruz.

### 📌📌 Normalizer

Farklı formatlarda gelen fakat aynı anlamı taşıyan mesajları tek bir formata çeviren katmandır.

### 📌📌 Canonical Data Model

canonical kelime anlamı: kurallara uygun, geleneklere uygun.

Birçok servis kendi formatını kullanıyor olabilir. Bunlar kendi içlerinde haberleşiyor olabilir. Bu gibi durumda ortak bir data model oluşturulur. Herkes bu model'i birbirine yollar ama her servis bir mesaj okuduğunda kendi internal formatına çevirir.

## 📌 Messaging Endpoints

### 📌📌 Messaging Gateway

mesaj atacak kod, mesajlaşma sisteminden bi haber olmalıdır. bu sebeple orderCanceled(order) şeklinde metotların olduğu sınıf olmalıdır. bu sınıf kendi içinde mesajlaşma sistemini direk çağıran kodlar içermelidir. İşte bu (orderCanceled metodunun olduğu) sınıf Messaging Gateway olarak adlandırılır.

### 📌📌 Messaging Mapper

mesaj objesi genelde domain obje'sini wrap eder ve bazı ufak farklılıklar içerir. bu iki obje arasında mapping yapan sınıflara "Messaging Mapper" denir.

### 📌📌 Transactional Client

mesajı yollayan servis, aynı business sürecinde (aynı thread'de) başka kaynaklarda da değişiklik yapıyor olabilir. bu durumda:

- saga
- global transaction manager

kullanıp bu durumu çözebiliriz. eğer bu şekilde çözüyorsak "Transactional Client" pattern'ini uyguluyoruzdur.

### 📌📌 Polling Consumer

receiver, istediği zaman mesaj almak istediğini mesaj atan tarafa iletir.

Bu tekniğe __synchronous receiver__ da denir. çünkü receiver'daki thread, mesaj gelene kadar bloklanır.

### 📌📌 Event-Driven Consumer

polling'in aksine, mesajı atan taraf yeni bir thread tetikler.

bu tekniğe __asynchronous receiver__ de denir.

### 📌📌 Competing Consumers

Competing kelime anlamı: rekabet.

Farklı servisler (aynı servisin farklı instance'ları dahil) aynı mesajı okumak isteyebilir. "Competing Consumers" rolü burada consumer'lardadır. çünkü birbirleri arasında mesajı okumak için rekabet etmektedirler.

bu gibi durumlarda mesajın sadece 1 kere iletildiğinden emin olunmalıdır.

### 📌📌 Message Dispatcher

Channel'a sadece 1 receiver bağlarız. Bu receiver'a __Message Dispatcher__ denir. Message Dispatcher, artık gelen mesajı __performer__'lara dağıtır.

### 📌📌 Selective Consumer

Consumer tarafta bir filter yazarız. Bu filter'da filtreleme yapar ve ona göre mesajın işlenip işlenmeyeceğine karar verebiliriz. Bu şekilde gereksiz mesajlar işlenmez. Örneğin; kampanya servisi 1000$ altındaki hiçbir şey için kampanya sunmayacak ise, sipariş-updated event'lerindeki toplam tutara göre filtreleme yapabiliriz.

### 📌📌 Durable Subscriber

receiver ayakta değilken, farklı bir servis onun okuyacağı mesajları saklar.

Bu senaryoda; __Durable Subscriber__ sadece asıl receiver ayakta değilken mesajları saklayan servistir. çökmüş olan servis ise __non-durable Subscriber__ rolündedir.

`messaging system`'deki çalışma modelimiz disconnected olan subscriber'lara sonradan, almadığı mesajları iletiyorsa "Durable Subscriber" tekniğine ihtiyacımız yoktur.

### 📌📌 Idempotent Receiver

mesajı alan servis, tekrar aynı mesajı okuduğunda hiçbir aksiyon almamalıdır.

### 📌📌 Service Activator

servislerimiz dinamik olarak aktif edilip kapatılabilir olmasını isteyebiliriz. bu gibi durumlarda, dinamik olan sevislerimizin önüne bir "servis activator" katmanı koyarız. activator, reflection ile veya hardcode yöntemlerle dinamik olan servisler, aktif edilmiş ise (classpath'te var ise, veya true/false config ile) ilgili servisi çağırır.

## 📌 System Management

### 📌📌 Control Bus

merkezi bir modüldür. tüm component'leri control eder ve monitoring'lerini sağlar. bu başlık altındaki tüm pattern'ler control bus tarafından yönetilir.

Control bus aracılığı ile her component'e config bilgisini de yollayabiliriz (örnek: log seviyesi, IP/address değişiklikleri, timeout süreleri...).

Control bus her component ile haberleşmesini bağımsız bir bağlantı/kanal üzerinden yapar. bu sebeple, tüm sistemde 2 bağımsız kanal vardır:

- __Application Message Flow__
- __Control Bus__

### 📌📌 Detour

özel bir "router" çeşididir.

gelen mesajı 2 yere yollar. 1 tanesi asıl yollamak istediğimiz destination'dır. diğer'i ise monitoring, logging, test, debug gibi custom amaçlarla kullanacağımız yerdir.

### 📌📌 Wire Tap

wire tap kelime anlamı: hattı (örnek telefon) dinlemek.

### 📌📌 Message History

message based sistemlerin temeldeki felsefesini değerlendiremeye alınca, mesajı okuyan servisin, mesajı atan servisin hangisi olduğundan bihaber olmalıdır. tabi bu durum debug ve trace işini zorlaştırmaktadır.

bu sebeple; her mesajın header'ında, sürecin en başından beri hangi servislerden geçildiğinin listesi eklenmelidir.

### 📌📌 Message Store

her attığımız mesajı bağımsız bir storage'ye kaydedebiliriz. bunu MOM'un kendisi de yapabilir, veya her client kendi de yapabilir.

### 📌📌 Smart Proxy

### 📌📌 Test Message

### 📌📌 Channel Purger
