# JAVA SPRING DATA

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Spring-Data

Bu ailenin içindeki bazı Jar'lar:

- Spring-Data-JPA
- Spring-Data-MongoDB
- Spring-Data-JDBC
- Spring-Data-LDAP
- Spring-Data-Redis
- Spring-Data-for-Apache-Cassandra

## 📌 Spring-Data-JPA over JPA

- JPA'ya ek özellikler sunar (Repository gibi)
- Spring'in diğer modülleri ile entegreli çalışır.
- Wrapper olduğu için "Spring-Data-MongoDB" modülü ile çok benzer isimde metotlarla çalışır.
- Wrapper olduğu için ileride JPA yerine başka bir RDBMS modülüne geçişimiz kolaylaşır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

### 📌📌 Spring JTA

Java EE'nin kullandığı `Java Transaction API (⟷ JTA)` metodlarını çağırarak çalışır. Yine, `Spring-data` da olduğu gibi, Spring önüne wrapper atarak bir API sunar. Dolayısı ile bir `JTA` implementasyonuna ihtiyacımız var:

- `Atomikos`
- `Narayana`
- `Java Open Transaction Manager (⟷ JOTM)`
- `JBoss TS` (`Java EE` sunucusundan bağımsız çalışabilmektedir)
- `Bitronix Transaction Manager (⟷ BTM)`

Yada `Java EE` managed bir `Transaction Manager` kullanabiliriz. Bunun için uygulamamızın `Java EE` sunucu üzerinde kaldırmak zorundayız. Spring, embedded web server ile geldiğinde de, embedded Java web server'ın sunduğu `JTA`'yı kullanabiliriz.

`JTA` birden fazla DB'nin de transactional çalışmasını sağlamaktadır. yani bir işlem diğer DB'ye kaydedilemediyse, ilk DB'ye de kaydetmeyecektir.

`JTA` 2 amaçla kullanılabilir:

- `JMS`: Broker'a mesaj gitmesi, onun karşı taraftan okunduğunu ve karşı tarafta çalıştırıldığı metodun başarılı şekilde tamamlandığından emin olunması.
- `DB`: DB bilgilerinin gerektiğinden bir den fazla DB'ye tümüyle kaydedilebilmesinden emin olunması.

kaynak: https://spring.io/blog/2011/08/15/configuring-spring-and-jta-without-full-java-ee 2 inci paragraf.

Spring, `PlatformTransactionManager` sınıfı ile transaction yapısını desteklemektedir. `@Transactional` annotation'unu bir metodun tepesine yazdığımızda, Spring `application-context` içerisinde `PlatformTransactionManager` implementasyonu (`Bean`) arayacaktır. Eğer manuel `Bean` tanımlatmak istiyorsak şunu yapmalıyız:

```java
@Bean
 public PlatformTransactionManager platformTransactionManager(){
    return new JtaTransactionManager();
}
```

`PlatformTransactionManager` class hiyerarşisi:

- `PlatformTransactionManager`
  - `HibernateTransactionManager`
  - `JTATransactionManager`
  - `JMSTransactionManager`
  - `JPATransactionManager`
  - `WebLogicJtaTransactionManager`

kaynak: https://assets.spring.io/wp/wp-content/uploads/2011/08/TransactionClassHierarchy.png

### 📌📌 distributed transaction

`global transaction` terimi `XA` protocol'ü dökümanında `distributed transaction` olarak geçer. `global transaction` her yerde kullanılan bir terim değildir. `XA` dökümanında, karışıklığı engellemek için bu şekilde kullanıldığı belirtilmiş. kaynak: https://pubs.opengroup.org/onlinepubs/009249599/toc.pdf "Distributed Transaction Processing:Reference Model", Version 3, title: "2.1 Transaction Definitions", subtitle: "Global Transaction" and "Distributed Transaction".

`local transaction` terimi `XA` protocol'ü dökümanında hiç kullanılmamıştır.

`global transaction` birden fazla bağımsız kaynak üzerinde çalışırken transaction sağlayan yapılardır. `global transaction` için üçüncü parti bir servisin kaynakları kontrol ediyor olması gerekmektedir. ancak `global transaction` ile `2PC` yapılabilmektedir. `2PC` nin olması için üçüncü parti servisin DB gibi kaynaklarla iletişimde olması gerekiyor. Bu iletişim için kullanılan protocol'lerden bir tanesi `XA` protocol'üdür. kaynak: https://spring.io/blog/2011/08/15/configuring-spring-and-jta-without-full-java-ee 3 üncü paragraf.

__X/Open (⟷ Open Group for Unix Systems)__ birçok kuruluş tarafından desteklenen ortak bir camiadır. `XA` protocol'ü, `X/Open` tarafından geliştirilmiştir.

Dökümandaki bazı tanımlar:

- __Application Program__: transaction'ın adımlarını tanımlayan program.
- __Resource Manager__: DB, `file system`, printer service gibi paylaşılacak kaynağı paylaşan yazılımdır.
- __Transaction Manager__: transaction'ın ne durumda olduğunu monitör eden, gerektiğinde rollback alması için ilgili RM'lere protocol üzerinden haber veren yazılımdır.

"Transaction"; aynı resource-manager'de yapılan, rollback edilebilir atomik işlemi kapsamaktadır. Oysa "Distributed transaction"; farklı resource manager'lerında olduğu data'lar üzerinde yapılan, rollback edilebilir atomik işlemleri kapsamaktadır.

Dökümanda 11.inci sayfada "Interface Overview" başlığında belirtildiği gibi XA, DB ile transaction manager arasındaki protocol'dür. Bu sebeple DB tarafından desteklenmesi zorunludur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 EntityManager/EntityManagerFactory vs Session/SessionFactory

`Session`, `Hibernate` specific sınıftır. `EntityManager` ise `JPA` spesifiktir. `EntityManager` arka planda `Hibernate` yapısının `Session` sınıfını zaten kullanmaktadır.

`EntityManager`'dan `Session` instance'ımızı elde edebiliriz:

```java
Session session = entityManager.unwrap(Session.class);
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Spring-Data "delete" performance

`delete` metodu `entityManager` veya `repository` tarafından çağrılırsa, JPA şunları yapar:

- tüm silmesi gereken sub entity'leri de silmesi gerekecektir. (cascade işlemler)
- cache varsa güncellemesi gerekecektir
- bir entity'ye (buna sub-entity'lerde dahil) event atanmış ise, o event'lerin (`@PreRemove`, `@PostRemove`...) tetiklenmesi gerekecektir. ("JPA Entity Lifecycle Events" konusu.)
- managed entity'lerin güncellenmesi gerekecektir.

Saf SQL, JPA repository'den execute edilirse bunların hiçbiri yapılmaz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Repository

Türkçe kelime anlamı: depo, ambar

Bazı süper class'lar:

- `Repository`

  hiçbir metot içermiyor.

- `CrudRepository`

  count, delete, existById, findAll, save gibi metotlar içeriyor

- `PagingAndSortingRepository`

  pagination için metotlar içeriyor.

## 📌 @NoRepositoryBean

ilgili sınıfın instance'ının repository olmadığını işaretler. örnek kullanımlar:

- şirketimiz içinde geliştirdiğimiz common-jar'da olan BaseRepository sınıfımızda bu annotation'ı kullanabiliriz.
- `PagingAndSortingRepository` bu annotation'ı içeriyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 @Modifying

repository'lerde her eğer manuel bir query yazıyor isek ve DB'yi güncelliyorsak, o zaman bu annotation'u o metodumuzun tepesine eklememiz şarttır. bunsuz da çalışır kod, fakat sağlıklı olmaz. JPA buna göre güncelleme olup olmadığını biliyor ve buna göre arka plandaki davranışlarını belirliyor (örnek cache güncelleme gibi...)

```java
@Query("UPDATE my_table SET name = ahmet WHERE id = :id")
@Modifying
int increaseValue(@Param("id") Long id);
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 relations

(önce güçlü zayıf tablo başlığı okunmalı)

customer-order tablosunda ilişkilere bakalım. customer güçlü, order güçsüz tablodur. çünkü order'da customer-ID olmak zorundadır. yoksa o order bir anlam ifade etmez (diğer bir değiş ile sistemimiz için bir geçerliliği olmaz. diğer bir değiş ile business'a göre bir mantığı olmaz).

"CustomerEntity.java" içerisinde bu olabilir:

```java
@OneToMany(mappedBy="customer") //order class'ının private 'customer' property'sidir (Java field'ının ismi burada yer almalı, DB sütun ismi değil!)
private List<Order> order;
```

"OrderEntity.java" içerisinde bu olabilir:

```java
@ManyToOne
@JoinColumn(name="customer_id") //order tablosundaki 'customer_id' sütununun adı.
// @JoinColumn(name="customer_id", referencedColumnName = "customer_national_identity_number") // Eğer böyle olursa order tablosundaki 'customer_id', Customer tablosundaki customer_national_identity_number sütununa denk geliyor anlamına gelir. Eğer referencedColumnName'i yazmazsak zaten DB ayarlarında olan foreign-key ayarı devreye girecektir. Fakat eğer DB'de foreign-key ayarı yok ise, referencedColumnName'e mutlaka ihtiyaç vardır. (eğer bu ayar girilmezse JPA default olarak Customer'ın default primary key'ini referans ediyor)
private Customer customer;
```

## 📌 JoinColumn vs mappedBy

JoinColumn ile JPA'ya, ilişki sütununun (foreign key'in) olduğunu belirtiyoruz. "mappedby" belirtildiğinde ilgili tablomuzda foreign key olmamasına rağmen, Java objesinde sanal olarak varmış gibi davranabilmemiz sağlanabiliyor.

örneğin yukarıdaki örnekte;

- `customer` içinde herhangi bir foreign key olmamasına rağmen `getOrder` yapabileceğiz.
- oysa `JoinColumn` ile `order` tablosunun `customer_id` diye DB'de bir sütun içerdiğini belirtiyoruz.

## 📌 crosstable (⟷ jointable)

sadece tablo ilişkilerinin tutulduğu tabloya verilen isimdir. `one-to-one` yada `many-to-one` ilişkilerde pek tercih edilmiyor. çünkü yeni tablo yaratmak yerine, var olan tablolara bir foreign key ekleniyor. fakat `many-to-many` ilişkilerde kullanılması şart. ismi genelde ilişkilendirilen iki tablonun birleşimidir. örnek: `customer_order`.

## 📌 @jointable

kullanımı:

```java
@Entity
public class A {

    private Long id;

    @ManyToOne
    @JoinTable(
       name = "A_B",
       joinColumns = @JoinColumn(name = "B_ID"),
       inverseJoinColumns = @JoinColumn(name = "A_ID")
    )
    private B b;
}
```

## 📌 FetchType

```java
@ManyToOne(fetch = FetchType.LAZY)
```

şeklinde kullanılır.

`Lazy` sadece `get` metodu çağrılınca DB'den nesneyi çağırır.

`EAGER` ise her zaman direk DB'den okunmuş halde objeyi getirir.

`one-to-one` ve `many-to-one` ilişki default olarak `EAGER` iken, `many-to-many` ilişki default'ta `LAZY`'dir.

## 📌 Basic

bu annotation temelde hiçbir şey yapmaz. İlgili alana, sadece `FetchType` parametresini geçebilmek için yaratılmış boş bir annotation'dur. eğer zaten bir JPA annotation'u field üstünde varsa `FetchType`'ı o annotation'a parametre geçeriz. fakat hiçbir JPA annotation'u yoksa `FetchType`'ı `@Basic` ile geçeriz.

## 📌 CascadeType

```java
@OneToMany(cascade=CascadeType.ALL)
```

şeklinde kullanılır. örneğin `order` yaratıyoruz, fakat `customer_id` foreign key'inin `null` olmaması gerekiyor. dolayısı ile önce `customer` yaratılmış olmalı. bu sebeple iki tabloya birden kayıt atarken önce `customer` daha sonra `order`'a yazmasını istiyoruz JPA'dan. örnek:

```java
Student student = new Student();
student.setName("Ahmet");
student.setHomeAddress( new Address("Istanbul") );
entityManager.persist(student);
```

Yukarısı `CascadeType` belirtmediysek hata verecektir. çünkü `adres` objesi `student` içinde tanımlı değil. bunu şu şekilde çözebiliriz:

```java
@OneToOne(cascade = CascadeType.PERSIST)
private Address homeAddress;
```

`CascadeType` seçenekleri:

(aşağıdaki her örnek `Order` ve `Order` içinde field olan `OrderItem` üzerinden anlatılmıştır):

- __PERSIST__: `Order` persist edilirse, `OrderItem` da persist edilir.

- __MERGE__: `Order` merge edilirse, `OrderItem` da merge edilir ("persist vs merge" farkı başka başlıkta anlatılıyor).

- __REMOVE__: `Order` silinirse, `OrderItem` da silinir.

- __REFRESH__: `EntityManager.refresh(Order)` metodu çağrılırsa, `EntityManager.refresh(OrderItem)` da çağrılır.

  EK not: `EntityManager.refresh(entity)` metodu DB'deki bilgilerin tümünü RAM'deki instance'a kaydediyor.

- __ALL__: Yukarıdaki tüm özellikleri birlikte aktif edilmesini sağlıyor.

Multiple kullanım da mevcuttur:

```java
@OneToOne(cascade = { CascadeType.PERSIST, CascadeType.MERGE })
```

## 📌 orphanRemoval

orphan kelime anlamı: öksüz.

Kullanımı:

```java
@OneToMany(orphanRemoval = true, cascade = CascadeType.PERSIST, mappedBy = "orderRequest")
private List<LineItem> lineItems;
```

`orphanRemoval` ve `CascadeType.REMOVE` farklıdır.

`CascadeType.REMOVE`, `Order` silinirse, silinen `order`'a ait `orderItem`'ların tümünün silinmesini sağlar. Fakat `orphanRemoval`, `order`'ın silinme durumunda devreye girmez. `orderItem-list`'ten 1 eleman silindiğinde, ve `repository.save(order)` yapıldığında, sadece RAM'den silinen `order-item`'ın DB'den de silinmesini sağlar. farkı örneklerle gösterirsek:

```java
Order order = new Order();
order.removeOldItems();
entityManager.persist(order); // in this line, orphanRemoval will remove old items.
```

```java
Order order = new Order();
entityManager.remove(order); // in this line, CascadeType.REMOVE will work all "items" of Order.
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Entity manager methods: merge vs persist

persist edilen entity üzerinde farklı bir manipülasyon (sınıf alanı güncellemesi) yapılırsa, transaction sonunda o manipülasyonlar da DB'ye otomatik yansıyacaktır. merge işleminde aynı durum söz konusu değildir.

Örnek:

```java
MyEntity e = new MyEntity();

// scenario 1
// transaction starts
em.persist(e);
e.setSomeField(someValue);
// transaction ends, and the row for someField is updated in the database

// scenario 2
// transaction starts
e = new MyEntity();
em.merge(e);
e.setSomeField(anotherValue);
// transaction ends but the row for someField is not updated in the database
// (you made the changes *after* merging)

// scenario 3
// transaction starts
e = new MyEntity();
MyEntity e2 = em.merge(e);
e2.setSomeField(anotherValue);
// transaction ends and the row for someField is updated
// (the changes were made to e2, not e)
```

## 📌 managed entity vs detached entity

persist metoduna yollanan entity'ler `managed entity` durumuna gelir. merge metodundan dönen instance, `managed` iken, merge metoduna parametre geçilen instance ise hala `detached` kalmaya devam etmektedir.

## 📌 bi-directional (⟷ bidirectional) vs in-directional (⟷ indirectional)

`one-to-many` yada `many-to-many` yada `one-to-one`, tek yönlü yada çift yönlü olabilir. örneğin; `one-to-one` ilişki yaptık, fakat 2 objenin sadece 1 tanesine `foreign key` koyduk. Java objesinden (entity içinde) de tanımlama yapmadık. dolayısı ile runtime sırasında sadece birinden diğerine gidiş olabilecek. dolaylı yoldan erişim yapılır fakat `bidirectional` ve `in-directional` terimleri direk erişim olup olmadığı ile ilgili bir durumdur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 transaction

bir metodun tepesine `@Transactional` annotation'u atılırsa o metodun içindeki işlemlerde yapılan DB işlemleri atomic olur. eğer hata çıkarsa otomatik olarak SQL'ler geri alınır. aslında geri alınmasından ziyade yapılan DB işlemleri DB'de işlenmiyor (`commit` vs `flush` başlığında daha detaylı anlatılıyor).

`@Transactional` annotation'ı, transaction'ı rollback etmekle ve denetlemekle yükümlü bir aspect. `@Transactional` attığımız fonksiyondan exception fırlatılırsa, `@Transactional` bu exception'ı değiştirmez.

`@Transaction` opsiyonel olarak şu parametreleri alabilir:

```java
@Transactional(rollbackFor = MyException.class,
               propagation = Propagation.REQUIRED,
               timeout = 35)
```

- `rollbackFor`

  sadece belirtilen exceptionlar için rollback yapılacağıdır. default'ta herhangi bir Java exception'ında SQL'ler rollback edilir.

  eğer bu property'de belirtilenlerden farklı bir exception alınırsa, oraya kadar ki SQL'ler commit edilir.

- `timeout`

  eğer bu süre geçerse rollback edilir

- `readOnly`

  ilgili transaction bloğunun sadece read-only işlem yaptığının bilgisini transaction manager'a yollar.

  transaction manager bunu runtime'da optimizasyon için kullanabilir.

  eğer "true" verilmişse; transaction manager fail vermek zorunda değil. bu transaction manager'a kalmış bir durum.

- `propagation`

  Türkçe kelime anlamı: yayılma

  `@transactional` bir metot içinde farklı bir ` @transactional` Bean'in metodu çağrılabilir. o zaman bu yeni metot'daki işlemlerin bağımsız mı yoksa, bir önceki transaction'a bağlı mı olacağına propagation özelliği ile karar veriliyor. propagation'ın alabileceği bazı değerler aşağıdaki gibidir.

  not: eğer çağrılan metot içerisinde exception alınırsa, exception muhtemelen(genelde) catch edilmeyecektir. çünkü exception'ı `@Transactional` annotation'u algılasın isteyeceğiz ki revert işlemini yapabilsin. Tabi bu durumda; parent metot'ta bu exception'u manuel olarak catch etmez isek, o zaman parent metot'da exception fırlatır ve artık parent metottaki `@Transactional` annotation'u çalışır. İşte burada bunun yönetimi yazılımcıya kalmıştır. Yani catch'i manuel tutmalı ve akışı business'a göre kendi manuel yönetmelidir.

  - `REQUIRED`

    Spring'de default değerdir. Aktif bir Transaction yoksa yeni bir transaction açar. Aktif varsa buna katılır.

  - `REQUIRES_NEW`

    Aktif bir transaction işlemi varsa bunu bekletir (Suspend) Yeni bir tane açarak kendi işini hallettikten sonra bu transaction işlemini kaldığı yerden devam ettirir. 2 transaction birbirinden bağımsızdır.

  - `NOT_SUPPORTED`

    Transaction varsa suspend edilir service metodu transaction'sız çalıştırılır.

  - `NEVER`

    Transaction varsa Exception fırlatır.

  - `MANDATORY`

    Transaction yoksa exception fırlatır

  - `SUPPORTS`

    önceden açılmış transaction varsa; onu kullanır. yoksa transaction'sız işlem yapar.

  - `NESTED`

    Aktif bir Transaction yoksa yeni bir transaction açar. Aktif varsa buna katılır. Buraya kadar ki kısım `REQUIRED` ile aynıdır. fakat ek olarak metot başlangıcında bir `savepoint` yaratır. ve eğer hata alınırsa bu `savepoint` noktasına dönülür. Transaction'lar arka planda DB tarafında yönetildiği için, buradaki `savepoint` noktaları, DB'deki `SAVE TRANSACTION save-point-name` SQL'ine denk gelmektedir.

`@Transactional` declarative çözüm. Programatik çözüm için `PlatformTransactionManager` veya `TransactionTemplate` sınıflarından yararlanılır. örnek:

```java
@Autowired
PlatformTransactionManager txManager;

myFunction(){
  DefaultTransactionDefinition def = new DefaultTransactionDefinition();
  // explicitly setting the transaction name is something that can only be done programmatically
  def.setName("SomeTxName");
  def.setPropagationBehavior(TransactionDefinition.PROPAGATION_REQUIRED);
  def.setTimeout(10);
  TransactionStatus status = txManager.getTransaction(def);
  try {
    // execute your business logic here
    txManager.commit(status);
  }
  catch (MyException ex) {
    txManager.rollback(status);
    throw ex;
  }
  
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 flush vs commit

`commit`, transaction'ı sonlandırmak için kullanılır. `transaction-begin` var fakat `end` yoktur. `end` metodu, `commit` olarak isimlendirilir.

`flush` ise transaction'ı yöneten framework'ün sunduğu fonksiyondur. transaction'lar arka planda aslında DB tarafında yönetilir. fakat framework, RAM'de tuttuğu fakat henüz DB'ye atmadığı SQL sorguları olabilir. Bu durum sadece "transaction" var olduğu durumlarda olur. Bu RAM'deki SQL'leri şu durumlarda framework DB'e fiziksel olarak yollar:

- her query hazırlandığı zaman otomatik framework `flush` eder
- manuel `flush` edebiliriz (`flush` fonksiyonu ile)
- framework kendi belli aralıklarla da `flush` edebilir (buradaki zaman aralığı konfigüratif olabilir)
- framework commit sırasında otomatik `flush` eder (bu kesin tüm entity-framework'ler tarafından yapılmak zorundadır)

Örneğin `Hibernate`'in `Flush` mode'ları burada yazmaktadır: <https://docs.jboss.org/hibernate/orm/3.5/javadocs/org/hibernate/FlushMode.html>

## 📌 Flush'lamanın yan etkileri

- `read-uncommited` isolation seviyesi DB'de açık ise, `flush` fonksiyonunu çağırırsak başka transaction'lar, `flush` yaptığımız transaction'ın `flush`'ladığı data'ları görebilir.
- Constraint'lerin validation'ları işleme alınabilir.

## 📌 Deferrable SQL Constraints

Deferrable kelime anlamı: ertelenebilir.

Transactional işlemlerde table constraints'lerin (kuralların NOT-NULL, FOREIGN-KEY gibi) ne zaman yapılacağı konusu biraz karmaşık. Her DB bunu farklı manage edebiliyor. Öncelikle data'nın flush edilmesi şart. Bu her DB için geçerli. Flush edilmeden, DB tarafında data'lar onaylanamaz/valide edilemez. Yani "entity framework" (örnek JPA) bu validasyonu kendi yapamaz. Fakat flush edilse bile anında kontrol sağlanmıyor olabilir. İşte burası karışık olan kısım.

Flush edilmiş data'da bazı constraint'ler o anda kontrol edilirken, bazıları sadece transaction'ın sonunda kontrol ediliyor/valide ediliyor. İşte hangilerinin ne zaman kontrol edileceğini belirlemek için PostgreSQL'de her constraint için farklı mode'lar sunuyor. Tablo oluştururken her sütuna için mod vermek zorundayız (aşağıda örneği var). Eğer mode verilmezse varsayılan mod kullanılır. Her mode'un kuralları aşağıdaki tablodaki gibidir (kaynak: https://begriffs.com/posts/2017-08-27-deferrable-sql-constraints.html):

Mode tanımlamak için SQL syntax'ı şu formatta olmalıdır:

```text
ALTER CONSTRAINT constraint_name [ DEFERRABLE | NOT DEFERRABLE ] [ INITIALLY DEFERRED | INITIALLY IMMEDIATE ]
```

Örnek mod tanımlama:

```java
ALTER TABLE foo
  ADD CONSTRAINT foo_bar_fk
  FOREIGN KEY (bar_id) REFERENCES bar (id)
  DEFERRABLE INITIALLY IMMEDIATE;
```

There are only 3 modes on PostgreSQL: source: https://www.postgresql.org/docs/current/sql-set-constraints.html title: "Description", 2th paragraph.

| Mode                             | Constraints are validated  | this setting can change per (by) transaction. |
|----------------------------------|----------------------------|-----------------------------------------------|
| `NOT DEFERRABLE` (note-1)          | immediately(note-2)        | not allowed                                   |
| `DEFERRABLE` + `INITIALLY IMMEDIATE` | immediately(note-2)        | allowed(note-3)                               |
| `DEFERRABLE` + `INITIALLY DEFERRED`  | at transaction commit time | allowed(note-3)                               |

`note-1`: `NOT DEFERRABLE` is always `IMMEDIATE`. source:https://www.postgresql.org/docs/current/sql-set-constraints.html title: "Description", 2th paragraph.

`note-2`: immediately after each row is updated individually.

`note-3`: Any transaction can override the rule with below SQL syntax:

```sql
BEGIN;
  -- bu satır ile artık erteleme mode'unu sadece "foo_bar_fk" için açmış oluyoruz.
  SET CONSTRAINTS foo_bar_fk DEFERRED;
  -- ...
  -- now we can do whatever we want to bar_id
  -- ...
COMMIT;
```

Bazı constraint'lere her mode'u set edemeyebiliyoruz. source: https://www.postgresql.org/docs/current/sql-set-constraints.html title: "Description", 5th paragraph.

(Not: JPA tarafında bunun yönetimini sağlayan bir yapı bulamadım)

## 📌 Use cases

Some usa cases for those modes

- https://emmer.dev/blog/deferrable-constraints-in-postgresql/ title: "Use cases".
- https://begriffs.com/posts/2017-08-27-deferrable-sql-constraints.html title: "Why Defer?".

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 ddl-auto

Spring'in JPA modülünde bu property: `spring.jpa.hibernate.ddl-auto`, sadece Hibernate kullanıldığında buna denk gelmektedir: `hibernate.hbm2ddl.auto`.

Sadece JPA init olduğunda aksiyon alır. alabileceği değerler:

- `create`

  drop everything, create new.

- `create-drop`

  same as "create". in addition it drop whole db when JPA is closed.

- `validate`

  only validate if existing DB scheme is compatible with Java code.

- `update`

  yeni sütun ekleme, index oluşturma gibi işlemler yapar fakat hiçbir zaman DB'deki bir şeyi silmez.

- `none`

  does nothing.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 envers

`Hibernate` modülüdür ve hibernate'in kendi repository'sinin içinde geliştirilir.

Her tablo veya field değişikliğinde audit tablolarına otomatik olarak değişiklikleri yazar. Hangi tablo veya field'ın audit'ini istiyorsak onların üstüne envers'in annotation'larını atmamız yeterlidir.

`Envers`, `Criteria` sınıfları ile yapılan bazı işlemlerini handle edemiyor. Bunu resmi dökümantasyondan okudum. muhtemelen native query'leri de destekleyemiyordur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Level 1 Cache

sadece 1 `EntityManager` instance'ında etkili olan cache'dir.

Default olarak aktiftir.

```java
EntityManager em = entityManagerFactory.createEntityManager();
em.getTransaction().begin();

User user1 = em.find(User.class, 1L); // DB'den alınır.
User user2 = em.find(User.class, 1L); // Önbellekten alınır, DB'ye gitmez.

em.getTransaction().commit();
em.close();
```

aynı `spring-boot` uygulamasındaki tüm `EntityManager` imnstance'lerini kapsayan bir cache'tir.

```java
EntityManager em1 = emf.createEntityManager();
User user1 = em1.find(User.class, 1L); // DB'den alınır ve L2 cache'e konur
em1.close();

EntityManager em2 = emf.createEntityManager();
User user2 = em2.find(User.class, 1L); // L2 cache'ten alınır, DB'ye gitmez
em2.close();
```

Her 2 seviye cache yazılımsal tarafta `JPA` tarafında olan bir tekniktir. DB'nin desteklemesini gerektiren bir durum yoktur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
