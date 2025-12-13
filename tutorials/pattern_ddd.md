
# PATTERN DOMAIN DRIVEN DESIGN

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Kitap

DDD ilk olarak Eric Evans tarafından "Domain-Driven Design: Tackling Complexity in the Heart of Software" kitabında, 2003 yılında ortaya atılmıştır. Bu kitaba piyasada "The Big Blue Book" da denilmektedir. Daha sonra, 2014'te Eric, tanımların referanslarını kısaca yazdığı "Domain-Driven Design Reference: Definitions and Pattern Summaries" isimli kitapta özetlemiştir ve eski kitabına çok ufak eklemeler yapmıştır. Bu yeni eklemeler özellikle kitabın title index'inde her başlıkta yıldızlı şekilde belirtilmiştir. Bu yeni eklemeler şunlardır:

- Domain Events
- Partnership (from "Context Mapping")
- Big Ball of Mud (from "Context Mapping")

DDD 2003 kitabında birçok öneri/pattern'den bahsedilmektedir. Bunun yanında;

- continuous integration
- Refactoring
- Clean&Readable code/architecture (not specific implementation examples - but more architectural like how to separate each class...)
- Factory sınıfları
- takım çalışmasının önemi
- side effect free functions

gibi konuların önemlerine bir kez daha bu kitapta özellikle değiniliyor. zaten çoğu bilgi kitap ilk yazıldığında piyasada bilinmiyordu. Bu konular DDD çalışması içinde temel oluşturduğu için bu konularda kitapta yer almıştır.

## 📌 Bu dökümandaki bilgilerin kaynağı

Bu dökümanda her bilgi sadece DDD'nin ilk kitabına dayanmıyor. Farklı kaynaklardan da yararlanılmıştır.

## 📌 ne zaman kullanılmalı

Business logic'in fazla olduğunda tercih edilir.

## 📌 ubiquitous language

ubiquitous kelime anlamı: yaygın

Domain expert'leri ve yazılımcılar ortak isimlendirme kullanmalıdır. isimlendirmeler teknik değil; domain uzmanlarının verdiği isimler olmalıdır. bu isimlendirmeye "ubiquitous language" adı verilmiştir.

## 📌 Domain-driven design (⟷ DDD) vs data-centric design (⟷ database-centric design ⟷ data-driven design)

DDD, __domain-centric design__ mantığına dayalıdır.

Karşılaştırma:

- DDD'de, business tarafında DB'den hiç habersiz (__persistence ignorance__) geliştirme başlanmalıdır ve devam ettirilmelidir. Oysa data-centric'te bunun tersi önerilir.
- DDD'de Domain ve persistence DB'deki modellerimiz farklı olabilir. Oysa data-centric'te bunun tersini önerilir.

Yukarıdaki maddeleri şuradan çıkarabiliriz: book: "Patterns, Principles, And Practices Of Domain-Driven Design", authors: "Scott Millett" and "Nick Tune", title: "Domain Model Implementation Patterns", subtitle: "domain model", 63 page, 1st paragraph.

Bazı domain'lerde (örneğin; metric/istatistik, yapay zeka sistemlerinde) DDD yerine, data-centric mimariler kullanmak daha verimli olabiliyor.

## 📌 business katmanı VS repository implementasyonu (persistence katmanı)

DDD kitabında repository'lerin sadece aggregate root'ları üzerinde işlem yapması gerektiğinden bahsedilmiş. Bazen bazı kod bloklarının repository implementasyonlarımızda mı, yoksa business logic'lerin olduğu servislerde mi olacağı karışmaktadır. Örneğin bir data'yı silmeye kalktığımızda, önce onunla bağlantılı data'ları silmemiz gerekmektedir. İşte bu kod bloğu repository tarafında olmalıdır. Burada şöyle düşünmek lazım: Eğer ki; DB tarafında çok farklı bir yapı kullanıyor olsaydık, bu kod bloğu değişecek miydi? DB değiştiğinde, domain'in bundan haberi olmaması gerektiği için, bu kod bloğunu repository tarafına çekmeliyiz. Gerçek bir örnekten gidersek; RDBMS kullandığımızı varsayalım. Fakat bunun yerine ileride 1 entity'ye ait tüm ilişkileri Cassandra'da tek 1 tablo içerisinde tutabiliriz. Bu durumda domain'in değişmemesi lazım. Bu sebeple, bahsedilen kod bloğu repository altında olmalıdır. Tabi burada ince bir nokta daha var: Domain servisleri, repository metotlarını çağırırken, ilişkili data'ları ayrı ayrı silineceğini bildirmez, sadece aggregate root'un son halini yollar ve bunun silineceğini belirtir:

```java
// pseudo code örneği

// compatible with DDD:
repository.save( orderInstanceDomainObject );

// "Repository" implementations may have inside anything like JPA-Entities.
```

## 📌 4 Layers of DDD

DDD'de bir sistem 4 temel katman bulunmalıdır (kaynak: "Eric Evans", "DDD", "Layered Architecture" başlığı.):

- __User Interface (⟷ Presentation Layer)__: web servislerinin bulunduğu, önyüzlerin bulunduğu kodlar.

- __Application__: bu kısım use case'leri içerir. bu kısımda business logic yoktur. Controller'larımızın gittikleri ilk servisler burasıdır diyebiliriz.

  Görevlerine bazı örnekler;
  - farklı client'lar için yapılan transformation ve validasyon işlemleri yapılabilir.
  - UnitOfWork, transactional gibi yönetimlerin tümünü bu katman yapar.
  - Dış dünyaya data döneceği zaman, domain-objelerini dönmez. Bunları DTO'ya çevirir. Dışarıya DTO döner.
  - Domain grubu içinde olan kodları çağırır. bu kısım high-level'dır.

- __Domain__: bu kısım business logic'lerin olduğu kısımdır.

- __Infrastructure__: diğer hizmetlerin (mail, database...) bulunduğu servisler/kodlar. Bu kısma aynı zamanda ORM mapper'larda dahildir.

Yukarıdaki her maddenin ayrımı için kaynak: "Eric Evans", "DDD", şekil: "Figure 4.1. Objects carry out responsibilities consistent with their layer and are more coupled to other objects in their layer.".

## 📌 Domain types

- __subdomain__

  Domain: e-commerce, sub-domain: checkout, order.

  bir domain'de birden fazla subdomain olabilir. bir sub-domain'de birden fazla bounded context olabilir.

  best-practice'lere göre 1 bounded-context, 1 sub-domain'e denk gelmelidir. Fakat bunu yapmak çok zordur.

- __core domain__

  bir doktor için yazılım düşünelim. Temelde 2 ihtiyaç var: 1-randevu sistemi, 2-hastaların bilgileri

  Burada core domain 2inci oluyor. Çünkü; hastaların bilgilerini tutma, tahlil sonuçlarını göstermek temel amaç. Bu sebeple "randevu" domain'i, __supporting domain__'dir.

- __generic subdomain__

  user-management, file-archive gibi core domain bilgisi hiç gerektirmeyen katmanlar, "generic domain" olarak adlandırılıyor. Bunlar bazen 3üncü parti servisler olabilir, dışarıdan satın alınabilir yazılımlar olabilir...

## 📌 bounded context

Bir model herkes için farklı görünebilir.

örnek-1; USB ile satılan bir Java yazılımı; aynı şirkette;

- satış departmanı için satılacak "ürün"
- lojistik departmanında "kargo"
- yazılımcı içinse bir "jar dosyası"dır

örnek-2; her kullanıcı; aynı şirkette;

- security-manager için user
- order-management için customer
- customer için bir diğer customer, referans edebileceği kişi'dir (loyalty kapsamında).

Yani; her birimde'te, aynı model, farklı field'larla temsil edilebilir. Çünkü her birimin tüm alanlara ihtiyacı olmayabilir veya bir alanı farklı tipte tutmak isteyebilir. yani; her model bir birim içerisinde bound edilmiştir (sınırlandırılmıştır).

Eğer bir model başka bir birime yollanacak ise, map'lenmelidir. Bu konu "context mapping" başlığında anlatılmaktadır.

## 📌 example grouping of all services

Aşağıdaki aggregate root'lar 1 microservice olsun dedik, çünkü aggregate-root atomic transaction'da yönetilmeli.

- checkout __(domain)__ (composition root - logical concepts)
  - basket __(domain)__ (aggregate root) (1 microservice)

    Bunun altındakiler moduler monolithic yapıda olabilir ve/veya hepsinin ayrı bounded-context'i de olabilir.

    - kampanyalar __(subdomain)__ (core domain)
    - gift    __(subdomain)__ (core domain)
    - kupon   __(subdomain)__ (core domain)
    - product __(aggregate)__ (core domain)
  - delivery __(domain)__ (aggregate root) (1 microservice)
  - payment __(domain)__ (aggregate root) (1 microservice)
  - order __(domain)__ (aggregate root) (1 microservice)
- user __(generic domain)__ (1 microservice)
- product-search __(core domain)__ (1 microservice)

## 📌 defining the bounded contexts

bounded context'lere ayırma işinin resmi bir standardı yoktur. bu iş biraz da sezgisel (heuristic) yapılır. Bounded context'lere ayırma işlemi yapılırken bazı dikkat edilecek noktalar şunlardır:

- uygulama geliştirme süreci ilerledikçe aynı isimler farklı field ve/veya model'lerde karşımıza çıkmaya başlar. Bu durumda acaba bunlar farklı bounded context'lerde mi olmalı diye düşünmeliyiz.
- Yapılacak işin özelliğine göre değil, iş mantığına göre belirlenmelidir.
- transaction süreci göz önünde bulundurulabilir. bir değişken değişip, diğer modeli de senkron değiştiriyorsa, o tüm modeller aynı bounded context'te mi olmalıdır diye düşünmek lazım.

## 📌 context map/mapping

bounded context'ler arası model ilişkilerinin nasıl yönetileceği için disiplinleri belirler. Bu disiplinlerin bazıları aşağıda listelenmiştir.

Context mapping farklı bounded context'ler arası modellerin ilişkilerini gösteren bir dökümandır. (source-id: 477) book:"Domain Driven Design Quickly", authors:"Abel Avram", "Floyd Marinescu", page:73, writes:

> A Context Map is a document which outlines the different Bounded Contexts and the relationships between them. A Context Map can be a diagram like the one below, or it can be any written document. The level of detail may vary. What it is important is that everyone working on the project shares and understands it.

- Partnership

  iki context'i geliştiren ekip birbirinin projeleri için uygun/uyumlu şekilde ortak kararlarla modellerini günceller.

- Shared kernel

  farklı bounded context'ler ortak model direk kod olarak paylaşabilir. bu JAR/DLL paylaşarak olabilir... örneğin sadece "customer" modeli 2 bounded context için ortak ise bu yol izlenebilir.

- Customer/Supplier Development Teams

  bounded context'deki takımlardan biri customer biri supplier rolündedir. Bir takım diğer takımdaki değişiklikleri izler ve planlayarak kendine alır. Burada direk modeli "hemen kabul etme" ilişkisinden ziyade, iki müşteri-tedarikçi rollerinde bir çalışma modeli söz konusudur.

- Conformist

  iki servisteki ekipler birbiri ile ortak çalışamıyor ise, 1 ekip diğerinin yaptığı her değişikliği almak zorunda olduğu durumdur. 1 ekip diğer ekibi hiç düşünmeden modellerini günceller.

- Anticorruption Layer

  bir context diğer context'teki ile uyuşmazlıklar yaşayabilir. bu durumda en iyi çözüm; herkesin tamamen kendi modelini oluşturması ve olabildiğince birbirinin field'larına bağımlı kalmayacak şekilde servis istekleri hazırlanmalıdır. Bunun için adapter (integration layer) yazılması gerekmektedir.

- Open Host Service

  Daha çok public API açan hizmetlerdeki gibi bir çalışma modeli söz konusudur.

- Published Language

  calendar syntax, contact sync, authentication standards (örnek: OAuth) gibi sistemler buna güzel birer örnektir. Belli dökümantasyona/standartlara uyan modellerle çalışıldığında, API'lere de zaten dolaylı yoldan uymuş olacağınız sistemlerdir.

- Separate Ways

  Ortak bir çalışmanın mümkün olmadığı takımlarda artık tamamen farklı yollara gidilebilir. Hiçbir şekilde ortam model paylaşımı yapılmaz.

- Big ball of mud

  kelime anlamı: büyük çamur topu.

  Burada hiçbir pattern uygulanmamaktadır. Takımlar birbirlerine haber vererek doğaçlama bir çalışma tekniği izlerler. Önerilen bir yöntem değildir.

## 📌 microservice vs bounded context

- DDD mimarisinde, microservice ile ilgili bir bilgi yoktur. DDD, `modular monolithic` bir yazılımda da kullanılabilir.
- Bazı durumlarda 1 microservice tek başına bir bounded context'i temsil edebilirken, birden fazla microservice birlikte 1 bounded context'i temsil edebilir. kaynak: (source-id: 231) "Bir Bounded Context == Bir Mikroservis ?" başlığı. + (source-id: 232) 1inci paragrafın sonu.
- 1 domain birden fazla microservice'ten meydana gelebilir. Veya tam tersi de olabilir.

Genelde beklenen şu yapıdır:

Domain > Bounded context > Microservice

1 microservice'te 1'den fazla domain veya bounded context olabilir ama pek tercih edilmez. Bu durum microservice'in temel bağımsızlık argümanını biraz kırar.

Eğer 1 bounded context 1 microservice'te olursa, o microservice'in ne yaptığı, sorumluluk ayrımı, transaction yönetimi çok netleşir.

## 📌 servisler

objelerin doğasında olmayan yazılımsal süreçler, "servis" olarak tasarlanmalıdır.

## 📌 Aggregate vs Aggregate Root

1 aggregate, 1 aggregate root içerir. "Aggregate" bir şeylerin bütün olarak kullanılmasını temsil eden genel bir terim. Biz DDD'de bunu pratiğe döktüğümüzde, karşımıza "aggregate root" kavramı çıkar.

Farklı bir deyiş ile; birden fazla entity'nin transaction'larda birlikte uyumlu şekilde iş akışını tamamlamaları gerekir. birden fazla entity'nin birlikte kullanılması durumu "__aggregate__" olarak ifade edilmektedir. yani aggregate, entity'ler grubudur.

"__aggregate root__" ise; direk örnek üzerinden gidersek: "sipariş" bir 'aggregate root'tur. ancak siparişe bağlı, 'ödeme bilgisi', 'kargo bilgisi' entity'ler aggregate root'a bağlı (normal) entity olarak nitelenirler.

Siparişe bağlı 'ödeme bilgileri', 'kargo bilgisi' bazı durumlarda (bazı domain'lerde) aynı aggregate root'a bağlı olmayabilir. Fakat "sipariş kalemi (yani bir siparişteki her item)", "sipariş" aggregate root'unun altında olması bu konuya daha net bir örnek olarak verilebilir.

İnternetteki kaynaklarda bazen, Aggregate ve Aggregate root birbiri yerine kullanılmaktadır. Çünkü biri soyut diğeri DDD'ye özel bir implementasyondur.

aggregate root; repository tarafından yönetilir ve client bir data load ettiğinde ancak aggregate root'a erişebilir. kaynak: (source-id: 233) answer of Jeff Sternal at Dec 24 '09 at 15:33. Fakat pratik'te böyle kod yazılmaz. genelde client'a sadece ihtiyacı olan data kısmı yollanır. bu durum anti-pattern değildir. client için gerektikçe optimizasyon yapılabilir.

```java
public class Order { // this is the "aggregate root" which has all information about "aggregate".

  UUID orderId;

  long orderNumberForEndUser;

  boolean disabled;

  List<OrderItem> orderItemList; // this is "domain object/domain entity".
  // all other domain objects (like orderItem) should be added here as an object, not ID!

  // Customer is another aggregate root. Therefore customer should not be added as object. It should be ID.
  long customerId;

  // Customer is another "root aggregate". Therefore we should not use the below fields on this class:
  // String customerName; // Which is inside Customer.
  // Customer customer; // this is another aggregate root.
  // Address customerAddress; // this is another domain object which is inside Customer.

  // "getter" and "setter" should be here...
  // "setters" ve "getter" fonksiyonları dışarıya zorunlu olmadıkça açılmamalı. Daha doğrusu açılabilir, fakat "getter" ile private field'ı
  // çekip başkası o private field içerisinde işlem yapamamalı. Aggregate root'u, Object-oriented programlamada olduğu gibi
  // sadece public metotları ile işlem yapmalıyız. Yani Order içerisindeki domain objeleri üzerinde manipülasyon sadece Aggregate Root'un
  // metotları aracılığı ile olmalıdır. Örneğin tem eklemek isteyen client aşağıdaki metodu tercih etmeli.

  boolean addItem(OrderItem orderItem){

      if(disabled == true){
        return false;
      }

      if(orderItemList == null){
        orderItemList = new ArrayList<>();
      }

      if(orderItemList.size() > 999){
         throw new OrderException("You can not add more then 999 items to basket!");
      }

      orderItemList.add(orderItem);

      // This is "domain event".
      // For event management Kafka can be used or Spring-event (application itself). By this way, other aggregates may effected by this operation.
      // It is optional to publish an event for each business method.
      // "eventToPublish" method does not publish the event directly. It adds the event to list. This event list should be publish directly after we save the Order Aggregate to DB. (This process should be transactional. "Outbox pattern" can be used.) (for the source check below note-1)
      eventToPublish( new OrderItemAddedEvent(orderItem) );

      return true;
  }

  // All other business logic specific to Order should be here...

  // All the data and method of this class should be "persistence agnostic".

  // Only "Order" can be sent to "repository" to save/update...
  // OrderItem should not sent to "repository". There should not be "OrderItemRepository" on the whole application code.
  // (exceptional cases may happen for performance issues).

  // repository.save(order) method should be called from services or another layers.

  // Order should be saved transactional (via repository). Therefore the events (like OrderItemAddedEvent) should never publish before the transaction will success. (To achieve this gap, if we use microservices we can prefer outbox pattern.)
}
```

source for above (note-1):

<https://github.com/Sairyss/domain-driven-hexagon> title:"Domain Events", sentence: "typeorm.repository.base.ts - repository publishes all domain events for execution when it persists changes to an aggregate.". The link "typeorm.repository.base.ts" redirects here: <https://github.com/Sairyss/domain-driven-hexagon/blob/master/src/libs/ddd/infrastructure/database/base-classes/typeorm.repository.base.ts> On this class we can see below code:

```typescript
async saveMultiple(entities: Entity[]): Promise<Entity[]> {
    const ormEntities = entities.map(entity => {
      entity.validate();
      return this.mapper.toOrmEntity(entity);
    });
    const result = await this.repository.save(ormEntities);
    await Promise.all(
      entities.map(entity =>
        DomainEvents.publishEvents(entity.id, this.logger, this.correlationId),
      ),
    );
    this.logger.debug(
      `[${entities}]: persisted ${entities.map(entity => entity.id)}`,
    );
    return result.map(entity => this.mapper.toDomainEntity(entity));
  }
```

## 📌 separating the business logics of Aggregate methods

bir örnek üzerinden gidelim:

```java
public class Order { // this is the "aggregate root" which has all information about "aggregate".

  public void disableOrder(){

    order.setDisabled(true);
    order.revertBonus();

    // optionally other business logic here...
    // optionally preparing domain events here...
  }

  private void setDisabled(boolean newStatus){
    this.disabled = newStatus;
  }

  public void revertBonus(){
    // revert the bonus (points) from user.
  }

  // other methods here...
```

Yukarıdaki Aggregate'i kullanan kod bloğu şu şekilde işlem yapmaması gerekiyor:

```java
order.setDisabled(true);
order.revertBonus();
```

Bu sebeple setDisabled metodu private yapıldı. Yani disableOrder metodu bir facade görevi görüyor. Fakat bazı durumlarda sadece revertBonus yapılabileceği için revertBonus public olarak bırakıldı.

Tabi bu durum tamamen business'a bağlı. Yani; bu kuralın kesin ve her zaman bu şekilde doğru olduğunu varsayarak yukarıdaki kodu yazdık: "Eğer bir sipariş iptal ediliyor ise mutlaka o siparişten kazanılan bonus'lar geri alınmalıdır.".

## 📌 Domain Services vs Application Service

Service kavramı çok geneldir. 2'ye bölünür:

- Domain Servisler, aggregate'lerin doğasında olmayan metotların toplandığı süreçleri kapsayan sınıflar olmalıdır.

  Servis sınıfların çok olması ve/veya Aggregate'lerin içindeki metotların servislerde bulunması iyi bir durumun göstergesi değildir.

  Domain servis'lerin state'i olmamasına gayret edilmelidir.

- application service, domain service ile farkını "4 Layers of DDD" başlığından çıkarabiliriz.

## 📌 Domain Events

Bu konuya event-sourcing başlığında da değinildi.

teknik isimde olmamalıdırlar. domain expert'leri de anlamalıdır.

Teknik anlamda konuşursak; domain event'leri, (farklı veya aynı "bounded context" içindeki) aggregate'lerin birbirleri ile haberleşmesi (bir aggregate'teki değişiklik, farklı bir aggregate'te de değişiklik gerektirdiğinde) durumunda fırlatılan event'lerdir.

### 📌📌 when to publish

Domain event'lerin tam olarak ne zaman fırlatılacağı Aggregate kod örneğinde belirtildi.

### 📌📌 who can handle

Bir domain event şunlar tarafından handle edilebilir:

- aynı bounded context içindeki bir aggregate-root tarafından.
- farklı bounded-context tarafından.
- farklı domain tarafından (önerilmez. source: <https://docs.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/domain-events-design-implementation> title:"What is a domain event?", paragraph: 1,8,9.)

### 📌📌 can be sync or async

Domain event'ler senkron veya asenkron olabilirler. Fakat "Integration event"leri kesinlikle asenkron olmak zorundadır. source: <https://docs.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/domain-events-design-implementation> title:"What is a domain event?", paragraph: 9.

## 📌 integration events

domain dışındaki yerlere yollanan event'lerdir. source: <https://docs.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/domain-events-design-implementation> title:"Domain events versus integration events", paragraph: 2.

## 📌 DDD kitabında geçen bazı terimler

Aşağıdaki bazı terimleri direk olarak kitap içerisinde açıklanmış.

## 📌 domain

domain is the field for which a system is built. Airport management, insurance sales, coffee shops...

## 📌 model

representation of a real world object. example: student class is not a real student object. student class includes only name, surname, age and some others representation on digital world.

Model objemiz her zaman fiziksel bir nesneye tekabül etmek zorunda değil. Bazen "para transferi" gibi bir modelimizde olabilir.

## 📌 domain model vs DB object

domain objeleri, DB objelerine benzer fakat DB objelerinden tamamiyle ayrı olabilir. ilk uygulama tasarlandığında, "modeller" tasarlanmalıdır. daha sonra database-admin bunları nasıl isterse o şekilde DB'ye kaydeder. DB'de hızlı arama olsun diye bazı şeyler denormalize edilebilir, sorgu bazlı tablolar yapılması için data'lar mükerrer edilebilir vs... fakat bu optimizasyonlar DB objelerinde yapılmalıdır.

dolayısı ile; bu ikisi arasında gerçek hayattaki nesneye/kavrama en benzer olan; domain objeleridir. çünkü DB yöneticileri gerçek hayata uzak değişiklikler yapabilir.

işte DDD kitabı bu 'ünün uygulamamızda birbirinden bağımsız kalmasını, bu şekilde DB yapısının, domain modellerini veya iş akışını etkilememesi konusunda yönlendirmektedir.

## 📌 domain model

domain model (class) is the model which is using by a domain.

## 📌 DTO (⟷ Data Transfer Object)

DTO is the class/structure which is using to store data. It can be using to send data from network protocols or passing arguments to functions. DTO can store model or models or any data...

gerçek bir uygulamadan örnek verirsek; repository'den dönen entity class'ı, önce model'e çevrilir, sonrada DTO'ya çevrilir ve client'a dönülür.

DTO'lar, modellerimiz'in içerikleri aynı olacakları anlamına gelmez. Farklı da olabilirler. Yada aynı olabilirler ama modelleri direk döndürmek yerine, mapping yaparak aynı field'lara sahip farklı objeler de kullanabiliriz. Çünkü DTO'lar sadece Data transfer objeleridir ve servisler arası haberleşme'de data transferi için kullanırlar. Remote servis çağrıları maliyetli olduğu için, tek bir istekte birçok data'yı birden taşımak isteriz. Bu sebeple DTO' pattern'i ortaya atılmıştır. kaynak: (source-id: 234) 1st paragraph.

Sun/Java community DTO'lara sadece "transfer obje" demektedir. Bu pattern'in yazılım dünyasında genel adı "Data Transfer Object (⟷ DTO)"tir. kaynak: (source-id: 234) son paragraf.

## 📌 entity

yazılım framework'lerinde "entity" terimi, DB'deki bir tablonun yazılım tarafındaki temsilidir (örnek JPA "entity bean"). fakat Eric'in DDD kitabında "entity" terimi farklı amaçla kullanılmaktadır. kaynak: "Eric Evans", "DDD", "Entities (a.k.a. Reference Objects)" başlığı, 13'üncü paragraf.

Eric'in kitapta bahsettiği entity kavramı; eşleştirilmeye kalktığımızda (bir eşitlik sorgusunda) field'ları ile değil, sadece ID'si ile kontrol etmemiz gereken domain modellerimizdir. kaynak: "Eric Evans", "DDD", "Entities (a.k.a. Reference Objects)" başlığı, 12'üncü paragrafın ilk cümlesi.

Bazı kaynaklarda DDD'den bahsedilsin veya bahsedilmesin, "entity" terimi direk olarak hem domain model, hem de DB nesnesinin yerine  kullanılmaktadır. Bunun 3 temel sebebi vardır:

1- domain model, BD'ye farklı kaydedilse de, aslında ikisi birbirinin yansımasıdır. Farklı bir değiş ile; bir modelin DB'ye farklı bir biçimde kaydedilmiş olması (yansıtılmış olması) onun gerçek değerinin kaybolmasına sebep olmaz. Örneğin; aynı modelin, "materialized view" ile NoSQL ve RDBMS'e ayrı ayrı kaydediliğini düşünelim. İlgili model, DB-1, DB-2 arası mapper fonksiyonlarımız olsun. Aslında 3'ü arasında git gel yapabiliyoruz. Bu durumda aslında 3 taraftaki data aslında aynı varlığa tekabül ediyor.

2- JPA direk olarak kendi içinde bu terimi DDD'den bağımsız şekilde kullanmaktadır. Sadece DB tarafından bakıldığında her tablo birer 'varlık'tır.

3- DDD uygulamadığımız sistemde, NoSQL tarzı sistemler kullanmıyorsak, genelde direk "entity bean" sınıflarını, model olarak düşünüp birebir şekilde kullanırız.

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

DDD kitabı 2 temel kısımdan oluşuyor. İçerdiği bazı konular aşağıdaki gibidir:

- __tactical design__ (bu terim kitapta geçmiyor. Piyasada kitabın bu kısmına hitap edenler bu terimi kullanıyor.)

  Kitabın "Part IV" bölümünden önceki tüm bölümlerini kapsıyor.

  Daha çok kod (class) seviyesinde pattern'ler içeriyor.

  - aggregate
  - value object
  - layered architecture
  - factories
  - repositories

- __strategic design__ (name of the "Part IV" of the book)

  Biraz daha büyük resimden bakmamızı sağlayan, çok aşırı detaylar belirtilmediği bir bölüm burası. Daha çok domain'in nasıl ayrılacağını anlatmaya çalışılıyor.

  - Bounded Context
  - Context Map
  - domain types (core-domain, sub-domain...)

Aslında kitabı bu şekilde net olarak 2'ye bölmek mümkün değil. Birbirine bağlı çok konu var. Örneğin "Ubiquitous Language" konusu tactical'da anlatılıyor. Fakat strategic'in altında olması daha doğru gibi. Zaten birindeki ibareler, diğerlerinin altyapısını hazırlıyor...

Strateji ve taktik kelimelerinin kelime anlamları için buradaki cevaplar okunabilir: <https://english.stackexchange.com/questions/29415/whats-the-difference-between-the-adjectives-strategic-and-tactical>
