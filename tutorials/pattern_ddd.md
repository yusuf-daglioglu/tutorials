
# PATTERN DOMAIN DRIVEN DESIGN

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Kitap

`DDD` ilk olarak `Eric Evans` tarafından `Domain-Driven Design: Tackling Complexity in the Heart of Software` kitabında, 2003 yılında ortaya atılmıştır. Bu kitaba piyasada `The Big Blue Book` da denilmektedir. Daha sonra, 2014'te `Eric Evans`, tanımların referanslarını kısaca yazdığı `Domain-Driven Design Reference: Definitions and Pattern Summaries` isimli kitapta özetlemiştir ve eski kitabına çok ufak eklemeler yapmıştır. Bu yeni eklemeler özellikle kitabın title index'inde her başlıkta yıldızlı şekilde belirtilmiştir. Bu yeni eklemeler şunlardır:

- `Domain Events`
- `Partnership (from "Context Mapping")`
- `Big Ball of Mud (from "Context Mapping")`

`DDD` 2003 kitabında birçok öneri ve pattern'den bahsedilmektedir. Bunun yanında;

- continuous integration
- Refactoring
- Clean&Readable code/architecture (not specific implementation examples - but more architectural like how to separate each class...)
- Factory sınıfları
- takım çalışmasının önemi
- side effect free functions

gibi konuların önemlerine bir kez daha bu kitapta özellikle değiniliyor. zaten çoğu bilgi kitap ilk yazıldığında piyasada bilinmiyordu. Bu konular `DDD` çalışması içinde temel oluşturduğu için bu konularda kitapta yer almıştır.

## 📌 ne zaman kullanılmalı

Business logic'in fazla olduğunda tercih edilir.

## 📌 ubiquitous language

`ubiquitous` kelime anlamı: yaygın

Domain expert'leri ve yazılımcılar ortak isimlendirme kullanmalıdır. isimlendirmeler teknik değil; domain uzmanlarının verdiği isimler olmalıdır. bu isimlendirmeye `ubiquitous language` adı verilmiştir.

## 📌 Domain-driven design (⟷ DDD) vs data-centric design (⟷ database-centric design ⟷ data-driven design)

`DDD`, `domain-centric design` mantığına dayalıdır.

Karşılaştırma:

- `DDD`'de, business tarafında `DB`'den hiç habersiz (`persistence ignorance`) geliştirme başlanmalıdır ve devam ettirilmelidir. Oysa `data-centric design`'te bunun tersi önerilir.
- `DDD`'de, domain ve persistence `DB`'deki modellerimiz farklı olabilir. Oysa `data-centric design`'te bunun tersini önerilir.

Yukarıdaki maddeleri şuradan çıkarabiliriz: `book: Patterns, Principles, And Practices Of Domain-Driven Design", authors: "Scott Millett" and "Nick Tune", title: "Domain Model Implementation Patterns", subtitle: "domain model", 63 page, 1st paragraph.`

Bazı domain'lerde (örneğin; metric, istatistik, yapay zeka sistemlerinde) `DDD` yerine, `data-centric design` kullanmak daha verimli olabiliyor.

## 📌 business katmanı VS repository implementasyonu (persistence katmanı)

`DDD` kitabında `repository`'lerin sadece `aggregate root`'ları üzerinde işlem yapması gerektiğinden bahsedilmiş. Bazen bazı kod bloklarının `repository` implementasyonlarımızda mı, yoksa business logic'lerin olduğu servislerde mi olacağı karışmaktadır. Örneğin bir data'yı silmeye kalktığımızda, önce onunla bağlantılı data'ları silmemiz gerekmektedir. İşte bu kod bloğu `repository` tarafında olmalıdır. Burada şöyle düşünmek lazım: Eğer ki; `DB` tarafında çok farklı bir yapı kullanıyor olsaydık, bu kod bloğu değişecek miydi? `DB` değiştiğinde, domain'in bundan haberi olmaması gerektiği için, bu kod bloğunu `repository` tarafına çekmeliyiz. Gerçek bir örnekten gidersek; `RDBMS` kullandığımızı varsayalım. Fakat bunun yerine ileride 1 entity'ye ait tüm ilişkileri `Cassandra`'da tek 1 tablo içerisinde tutabiliriz. Bu durumda domain'in değişmemesi lazım. Bu sebeple, bahsedilen kod bloğu `repository` altında olmalıdır. Tabi burada ince bir nokta daha var: Domain servisleri, `repository` metotlarını çağırırken, ilişkili data'ları ayrı ayrı silineceğini bildirmez, sadece `aggregate root`'un son halini yollar ve bunun silineceğini belirtir:

```java
// pseudo code örneği

// compatible with DDD:
repository.save( orderInstanceDomainObject );

// "Repository" implementations may have inside anything like JPA-Entities.
```

## 📌 4 Layers of DDD

`DDD`'de bir sistem 4 temel katman bulunmalıdır (`kaynak: "Eric Evans", "DDD", "Layered Architecture" başlığı.`):

- `User Interface (⟷ Presentation Layer)`

  `web` servislerinin bulunduğu, önyüzlerin bulunduğu kodlar.

- `Application`

  bu kısım use case'leri içerir. bu kısımda business logic yoktur. `Controller`'larımızın gittikleri ilk servisler burasıdır diyebiliriz.

  Görevlerine bazı örnekler;
  - farklı client'lar için yapılan transformation ve validasyon işlemleri yapılabilir.
  - `UnitOfWork`, `transactional` gibi yönetimlerin tümünü bu katman yapar.
  - Dış dünyaya data döneceği zaman, domain-objelerini dönmez. Bunları `DTO`'ya çevirir. Dışarıya `DTO` döner.
  - Domain grubu içinde olan kodları çağırır. bu kısım high-level'dır.

- `Domain`

  bu kısım business logic'lerin olduğu kısımdır.

- `Infrastructure`

  diğer hizmetlerin (mail, database...) bulunduğu servislerdir. Bu kısma aynı zamanda `ORM` mapper'larda dahildir.

Yukarıdaki her maddenin ayrımı için kaynak: `"Eric Evans", "DDD", şekil: "Figure 4.1. Objects carry out responsibilities consistent with their layer and are more coupled to other objects in their layer.".`

## 📌 bounded context

Gerçek hayattaki aynı fiziksel obje, farklı domain'lerde herkes için farklı görünebilir:
- farklı ismi olabilir
- ve/veya field isimleri farklıdır
- ve/veya veya bir domainde bazı filed'lar hiç kullanılmazken, diğer domainde bazı filed'lar kullanılabilir.

`örnek-1`; `USB` ile satılan bir `Java` yazılımı; aynı şirkette;

- satış departmanı için satılacak `ürün`
- lojistik departmanında `kargo`
- yazılımcı içinse bir `jar` dosyası'dır.

`örnek-2`; her `user`; aynı şirkette;

- security manager için `client user`
- order-management için `customer`
- customer için ise, farklı bir customer (müşterinin arkadaşı), referans edebileceği kişi'dir (loyalty kapsamında).

yani; her model, bir domain içerisinde bound edilmiştir (sınırlandırılmıştır). bu sınıflara `bounded context` denir.

Eğer bir model başka bir birime yollanacak ise, map'lenmelidir. Bu konu `context mapping` başlığında anlatılmaktadır.

## 📌 example grouping of all services

- `e-commerce` -> `domain`

  - `checkout` -> `subdomain`

    `checkout` sadece logical bir gruplamadır. fiziksel hiçbir etkisi yoktur.

    - basket -> `subdomain`

      bu kodların içinde `Aggregate root`, `Basket`'tır. İçinde `ProductId` de var.

    - delivery -> `subdomain`
    
      kendi `aggregate root`'u var. kendi `bounded context`'i var.

      - delivery harita rota optimizasyonu -> `supporting domain`

    - payment -> `subdomain`

      kendi `aggregate root`'u var. kendi `bounded context`'i var.

- `user ve yetkilendirme` -> `generic domain`
- `product-search` -> `supporting domain`

## 📌 defining the bounded context

her domainin model yapısını belirlerken (yani başka bir ifade ile: defining the `bounded context` işlemi yaparken) nelere dikkat etmeliyiz?

bu işlemin resmi bir standardı yoktur. bu iş biraz da sezgisel (heuristic) yapılır. dikkat edilebilecek bazı noktalar:

- uygulama geliştirme süreci ilerledikçe aynı isimler farklı field ve/veya model'lerde karşımıza çıkmaya başlar. Bu durumda acaba bunlar farklı `bounded context`'lerde mi olmalı diye düşünmeliyiz.
- Modellerin fiziksel tutuluşuna göre veya yazılımın teknik kararlarına göre değil, iş mantığına göre belirlenmelidir.
- transaction süreci göz önünde bulundurulabilir. bir değişken değişip, diğer modeli de senkron değiştiriyorsa, o tüm modeller aynı `bounded context`'te mi olmalıdır diye düşünmek lazım.

## 📌 context map/mapping

`bounded context`'ler arası model ilişkilerinin nasıl yönetileceği için disiplinleri belirler. Bu disiplinlerin bazıları aşağıda listelenmiştir.

`Context mapping` farklı `bounded context`'ler arası modellerin ilişkilerini gösteren bir dökümandır. `https://matfrs2.github.io/RS2/predavanja/literatura/Avram%20A,%20Marinescu%20F.%20-%20Domain%20Driven%20Design%20Quickly.pdf book:"Domain Driven Design Quickly", authors:"Abel Avram", "Floyd Marinescu", page:73, writes:`

`A Context Map is a document which outlines the different Bounded Contexts and the relationships between them. A Context Map can be a diagram like the one below, or it can be any written document. The level of detail may vary. What it is important is that everyone working on the project shares and understands it.`

- `Partnership`

  iki context'i geliştiren ekip birbirinin projeleri için uygun (uyumlu) şekilde ortak kararlarla modellerini günceller.

- `Shared kernel`

  farklı `bounded context`'ler ortak model direk kod olarak paylaşabilir. bu JAR/DLL paylaşarak olabilir... örneğin sadece "customer" modeli 2 bounded context için ortak ise bu yol izlenebilir.

- `Customer/Supplier Development Teams`

  `bounded context`'deki takımlardan biri customer biri supplier rolündedir. Bir takım diğer takımdaki değişiklikleri izler ve planlayarak kendine alır. Burada direk modeli "hemen kabul etme" ilişkisinden ziyade, iki müşteri-tedarikçi rollerinde bir çalışma modeli söz konusudur.

- `Conformist`

  iki servisteki ekipler birbiri ile ortak çalışamıyor ise, 1 ekip diğerinin yaptığı her değişikliği almak zorunda olduğu durumdur. 1 ekip diğer ekibi hiç düşünmeden modellerini günceller.

- `Anticorruption Layer`

  bir context diğer context'teki ile uyuşmazlıklar yaşayabilir. bu durumda en iyi çözüm; herkesin tamamen kendi modelini oluşturması ve olabildiğince birbirinin field'larına bağımlı kalmayacak şekilde servis istekleri hazırlanmalıdır. Bunun için adapter (integration layer) yazılması gerekmektedir.

- `Open Host Service`

  Daha çok public `API` açan hizmetlerdeki gibi bir çalışma modeli söz konusudur.

- `Published Language`

  calendar syntax, contact sync, authentication standards (örnek: `OAuth`) gibi sistemler buna güzel birer örnektir. Belli dökümantasyona ve standartlara uyan modellerle çalışıldığında, `API`'lere de zaten dolaylı yoldan uymuş olacağınız sistemlerdir.

- `Separate Ways`

  Ortak bir çalışmanın mümkün olmadığı takımlarda artık tamamen farklı yollara gidilebilir. Hiçbir şekilde ortam model paylaşımı yapılmaz.

- `Big ball of mud`

  kelime anlamı: büyük çamur topu.

  Burada hiçbir pattern uygulanmamaktadır. Takımlar birbirlerine haber vererek doğaçlama bir çalışma tekniği izlerler. Önerilen bir yöntem değildir.

## 📌 microservice vs bounded context

Genişten küçüğe doğru sıralarsak:

Domain >= `subdomain` >= `bounded context` >= `aggregate root` >= entity

Yukarıdaki sıralama sanaldır. Bunu fiziksel olarak ihtiyaçlarımıza göre bölebiliriz. `DDD` mimarisinde, microservice ile ilgili bir bilgi yoktur. DDD, `modular monolithic` bir yazılımda da kullanılabilir.

- Bazı durumlarda 1 microservice tek başına bir bounded context'i temsil edebilirken, birden fazla microservice birlikte 1 bounded context'i temsil edebilir. `kaynak: https://suadev.gitbook.io/turkish-microservices-book/ddd-ve-microservice-mimari "Bir Bounded Context == Bir Mikroservis ?" başlığı. + https://medium.com/trendyol-tech/ddd-ve-mikroservis-kavramlar%C4%B1-%C3%BCzerine-bounded-context-mikroservis-i%CC%87li%C5%9Fkisi-ve-tutarl%C4%B1l%C4%B1k-9e7ac4a3532f 1inci paragrafın sonu.`

- 1 domain, birden fazla microservice'ten meydana gelebilir. Veya tam tersi de olabilir.

## 📌 Aggregate vs Aggregate Root

`Aggregate` bir şeylerin bütün olarak kullanılmasını temsil eden genel bir terimdir. Biz `DDD`'de bunu pratiğe döktüğümüzde, karşımıza `aggregate root` kavramı çıkar.

birden fazla entity'nin transaction'larda birlikte uyumlu şekilde iş akışını tamamlamaları gerekir. birden fazla entity'nin birlikte kullanılması durumu `aggregate` olarak ifade edilmektedir. yani `aggregate`, entity'ler grubudur.

`aggregate root` ise; direk örnek üzerinden gidersek: `sipariş` bir `aggregate root`'tur. ancak `sipariş`e bağlı `ödeme bilgisi`, `kargo bilgisi` gibi entity'ler, `aggregate root`'a bağlı entity olarak nitelenirler.

`Sipariş`e bağlı `ödeme bilgileri`, `kargo bilgisi` bazı durumlarda (bazı domain'lerde) aynı `aggregate root`'a bağlı olmayabilir. Fakat `sipariş kalemi` (yani bir siparişteki her item), `sipariş` `aggregate root`'unun altında olması bu konuya daha net bir örnek olarak verilebilir.

İnternetteki kaynaklarda bazen, `Aggregate` ve `Aggregate root` birbiri yerine kullanılmaktadır. Çünkü biri soyut, diğeri `DDD`'ye özel bir implementasyondur.

`aggregate root`; `repository` tarafından yönetilir ve client bir data load ettiğinde ancak `aggregate root`'a erişebilir. `kaynak: https://stackoverflow.com/questions/1958621/whats-an-aggregate-root answer of Jeff Sternal at Dec 24 '09 at 15:33.` Fakat pratik'te böyle kod yazılmaz. genelde client'a sadece ihtiyacı olan data kısmı yollanır. bu durum anti-pattern değildir. client için gerektikçe optimizasyon yapılabilir.

```java
public class Order { // this is the "aggregate root" which has all information about "aggregate".

  private UUID orderId;

  private long orderNumberForEndUser;

  private boolean disabled;

  private List<OrderItem> orderItemList; // this is "domain object/domain entity".
  // all other domain objects (like orderItem) should be added here as an object, not ID!

  // Customer is another aggregate root. Therefore customer should not be added as object. It should be ID.
  private long customerId;

  // Customer is another "aggregate root". Therefore we should not use the below fields on this class:
  // String customerName; // Which is inside Customer.
  // Customer customer; // this is another "aggregate root".
  // Address customerAddress; // this is another domain object which is inside Customer.

  // "getter" and "setter" should be here...
  // "setters" ve "getter" fonksiyonları dışarıya zorunlu olmadıkça açılmamalı. Daha doğrusu açılabilir, fakat "getter" ile private field'ı
  // çekip başkası o private field içerisinde işlem yapamamalı. Aggregate root'u, Object-oriented programlamada olduğu gibi
  // sadece public metotları ile işlem yapmalıyız. Yani Order içerisindeki domain objeleri üzerinde manipülasyon sadece Aggregate Root'un
  // metotları aracılığı ile olmalıdır. Örneğin item eklemek isteyen client aşağıdaki metodu tercih etmeli.

  public boolean addItem(OrderItem orderItem){

      if(disabled){
        return false;
      }

      if(orderItemList == null){
        orderItemList = new ArrayList<>();
      }

      if(orderItemList.size() > 999){
         throw new OrderException("You can not add more than 999 items to the order!");
      }

      orderItemList.add(orderItem);

      // event of "event sourcing". it is sync.
      appendChange( new OrderItemAdded(orderId, orderItem) );

      // "domain event".
      // "domain event" must be publish transactionally (atomically).
      // but the consumer may read it sync or async.
      eventToPublish( new OrderItemAddedEvent(orderItem) );

      return true;
  }

  // All other business logic specific to Order should be here...

  // All the data and method of this class should be "persistence agnostic".

  // Only "Order" can be sent to "repository" to save/update...
  // OrderItem should not be sent to "repository". There should not be "OrderItemRepository" on the whole application code.
  // (exceptional cases may happen for performance issues).

  // repository.save(order) method should be called from services or other layers.

  // Order should be saved transactionally (via repository). Therefore the events (like OrderItemAddedEvent) should never be published before the transaction will succeeds. (To achieve this gap, if we use microservices we can prefer outbox pattern.)
}
```

## 📌 Domain Services vs Application Service

`Service` kavramı çok geneldir. 2'ye bölünür:

- `Domain Service`, `aggregate`'lerin doğasında olmayan metotların toplandığı süreçleri kapsayan sınıflar olmalıdır.

  `Service` sınıfların çok olması ve/veya `Aggregate`'lerin içindeki metotların servislerde bulunması iyi bir durumun göstergesi değildir.

  `Domain service`'lerin state'i olmamasına gayret edilmelidir.

- `application service`, `domain service` ile farkını `4 Layers of DDD` başlığından çıkarabiliriz.

## 📌 events

| event type                | sınır (kimler bu event'i yakalayabilir) | kim fırlatır           |
|---------------------------|-----------------------------------------|------------------------|
| event of `event sourcing` | aynı `aggregate root`                   | aynı `aggregate root`  |
| `domain event`            | aynı `bounded context`                  | `Domain Layer`         |
| `integration event`       | anyone                                  | `application layer`    |

| event type                | opsiyonel mi                                                                               | sync          |
|---------------------------|--------------------------------------------------------------------------------------------|---------------|
| event of `event sourcing` | eğer ilgili `aggregate root`'ta `event sourcing` var ise, kesinlikle her aşamada yapılmalı | sync          |
| `domain event`            | opsiyonel ama `DDD` uygulayan yazılımda neredeyse kesin beklenen bir durum                 | sync or async |
| `integration event`       | entegrasyon varsa kesin olmalı                                                             | async         |

## 📌 DDD kitabında geçen bazı terimler

Aşağıdaki bazı terimler piyasada birbirlerinin yerine kullanılıyorlar.<>

## 📌 domain

domain is the field for which a system is built. examples: Airport management, insurance sales, coffee shops.

## 📌 model

gerçek dünyadaki bir nesnenin, dijital dünyadaki yansımasıdır. `Customer.java` `class`'ı gibi.

`modelling (⟷ modelleme)` terimi de geçrek dünyadaki bir şeyin, dijital dünyada nasıl tutulup yansıtılacağını belirleme işlemidir.

## 📌 domain model

tüm domain içindeki obje ve bunların davranışları, business logic'leri kapsar.

## 📌 domain object vs DB object

`domain object`, `domain model` içerisinde olan bir bileşendir (parçadır). `Customer.java` `class`'ı gibi.

`domain object` ile `DB object` farklı olabilir. çünkü `DB` admin denormalizasyon gibi şeyler yapmış olabilir.

## 📌 entity

`kaynak: "Eric Evans", "DDD", "Entities (a.k.a. Reference Objects)" başlığı, 13'üncü paragraf.`

`domain object`'nin `ID`'si var ise, ve eşitlik kontrolü bu `ID`'ye göre yapılabiliyorsa bu `domain object` bir `entity`'dir.

`DDD` `entity` ile `JPA` `entity` tamamen farklı şeylerdir.

## 📌 value object

- property'leri eşit olduğunda, birbirleri yerine kullanılabilen objeler'dir.
- immutable olmalıdırlar.

- Aşağıda bazı örnekler listelendi. Fakat bu objeler domain'den domain'e (business logic'e göre) entity veya value obje olabilirler. Yani aşağıdakiler bu şekilde olacak diye bir kaide yoktur. Aşağıdakiler Sadece örnek amaçlı yazılmıştır.

örnek-1:

X ve Y DB objelerimiz olsun. ikisinin tüm property'lerinin (isim, soy isim, yaş...) value'ları aynı olsun. buna rağmen bu objeler birbirlerine eşit değildir. çünkü ID'leri farklıdır. bu sebeple bu objeler "value object" değildir.

örnek-2:

Koordinat düzlemindeki noktayı tutan bir point1 ve point2'miz olsun. bu objelerin property'leri (x, y, z) eğer aynı ise bu 2 obje birbirlerine eşit demektedir. bu sebeple "point" objesi bir "value object" sınıfına girer.

örnek-3:

İki farklı kişi aynı adres'te oturuyor olabilir. Böyle bir durumda her iki Person objesinin hangi adrese işaret ettiğinin önemi yoktur. Bu sebeple, adresin value object olmasını bekleriz. Fakat bazı business'larda adres objelerinin birbirinin aynısı olup olmayacağını tanımlayan bir garanti olmayabiliyor. Çünkü adresler genelde manuel giriş yapılıyor. Son kullanıcı tarafından otomatik tamamlamadan seçilerek tanımlanmıyor. Uzunca bir string oluyorlar. Bu durum genelde e-ticaret sitelerinde oluyor. Bu sebeple e-ticaret sitelerinde adres bir entity'dir.

Oysa bir harita sisteminde (örnek: google maps) bir adresin neresi olduğu tüm field'ları ile ayrılmış şekilde sistematik olarak tutulur. Bu sebeple google-maps için adres value objedir.

Not: E-ticaret platformlarındaki adres objesinin entity olmasının sebebi; son kullanıcı tarafından update edilebilmesi (yani immutable kuralı bozması) değildir. Çünkü e-ticaret sitelerinde update edilen adres olduğunda, yeni bir adres objesi yaratılır ve eskisi disable edilir/edilebilir.

örnek-4:

renkler birer value object'tir. kaynak: kaynak: "Eric Evans", "DDD", "Is "Address" a VALUE OBJECT? Who's Asking?" başlığının hemen sonrasındaki ilk cümle.

diğer örnekler:

money, range, date birer value objedir.

## 📌 strategic design vs tactical design

`DDD` kitabı 2 temel kısımdan oluşuyor. İçerdiği bazı konular aşağıdaki gibidir:

- `tactical design` (bu terim kitapta geçmiyor. Piyasada kitabın bu kısmına hitap edenler bu terimi kullanıyor.)

  Kitabın `Part IV` bölümünden önceki tüm bölümlerini kapsıyor.

  Daha çok kod (`class`) seviyesinde pattern'ler içeriyor.

  - `aggregate`
  - `value object`
  - `layered architecture`
  - `factories`
  - `repository`

- `strategic design` (name of the "`Part IV`" of the book)

  Biraz daha büyük resimden bakmamızı sağlayan, çok aşırı detaylar belirtilmediği bir bölüm burası. Daha çok domain'in nasıl ayrılacağını anlatmaya çalışılıyor.

  - `Bounded Context`
  - `Context Map`
  - `domain types` (`core domain`, `subdomain` gibi)

Aslında kitabı bu şekilde net olarak 2'ye bölmek mümkün değil. Birbirine bağlı çok konu var. Örneğin `Ubiquitous Language` konusu `tactical`'da anlatılıyor. Fakat `strategic`'in altında olması daha doğru gibi. Zaten birindeki ibareler, diğerlerinin altyapısını hazırlıyor...

Strateji ve taktik kelimelerinin kelime anlamları için buradaki cevaplar okunabilir: <https://english.stackexchange.com/questions/29415/whats-the-difference-between-the-adjectives-strategic-and-tactical>
