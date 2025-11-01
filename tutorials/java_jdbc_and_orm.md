# JAVA JDBC AND ORM

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 ORM (⟷ Object to Relational Mapping)

`ORM` programlama dilinden bağımsız, DB objelerinin programlamada haritalanmasının (mapping) genel ismidir.

## 📌 Persistence (⟷ süreklilik)

Bilişim dünyasında `persistence`, bir işlemin sürekliliğinin sağlanması kastedilir. işlemin sürekliliği olabilmesi için state'in (data'nın) bir yere kaydedilmesi gerekir.

## 📌 JPA (⟷ Java Persistence API)

`Java` için `ORM` spesifikasyonudur.

## 📌 Eclipselink (⟷ old-name:Toplink) vs Hibernate vs openJPA

Birer `JPA` implementasyonudur.

## 📌 @javax.persistence.Entity vs @org.hibernate.annotations.Entity

`javax` kullanırsak runtime sırasında implementasyon arar ve onu çağırır. `Hibernate` olan direk kendini çağırır. `Hibernate` ne kadar `javax` ile aynı görevi görse de, `javax`'ta olmayan ekstra property ve fonksiyonlar sunuyor.

## 📌 Exception sınıfları

`org.hibernate.StaleObjectStateException` hatasını `Hibernate` fırlatırken, `JPA` `javax.persistence.OptimisticLockException` fırlatır. Bunlar birbirini implemente etmez. Benzer şekilde `Spring-Data`'da farklı exception sınıfları kullanıyor olabilir. Tamamen aynı hataya tekabül edebilirler.

Ne zaman hangi hatanın fırlatılacağı kimin metodunu çağırdığımıza bağlı değişir. Direk `Hibernate`'in `session` sınıfı üzerinden `SQL` işlettiysek onun özel hatasını alırız. Direk `JPA`'nın `EntityManager` sınıfını çağırırsak, `JPA`'nın hatasını alırız. Fakat `Hibernate` implementasyonunu kullanıyorsak, `JPA`'nın `stack trace`'ini incelerken, her 2 hatayı görmemiz gerekir. Çünkü `Hibernate` hata fırlatır `JPA` onu wrap eder. Fakat istisna olarak; `JPA` hatayı wrap ederken exception'ı log'a basmamış (yutmuş) olabilir. Bu durumda sadece `JPA`'nın hatasını görürüz.

## 📌 Entity Framework

`Microsoft`'un `ORM` framework'ü'nün özel ismidir. İsim karmaşasına yol açmaktadır, çünkü çok genel bir terimdir.

`.NET` uyumlu tüm dillerden kullanılabiliyor.

## 📌 JDBC (⟷ Java DB Connectivity) vs JPA

`JDBC`, `Java` için DB spec'idir. (`driver` olmadan bir işe yaramaz.) `JPA` kendi içinde `JDBC` kullanır. `JDBC` DB'ye bağlanan ve üzerinde işlem yapmaya yarayan `API`'dir. Her DB bunun implementasyonlarını (`driver`'larını) yapar (`MySQL`, `Oracle DB` gibi). Bunun üzerine `ORM` yeteneğini katarak `JPA` ortaya çıkmıştır.

## 📌 OJDBC

`Oracle DB`'nin `JDBC` `driver`'ıdır.

## 📌 ODM (⟷ Object Document Mapping)

`ORM`, `RDBMS`'ler için mapping yaparken, `ODM` sadece `NoSQL`'ler (relation olmayan DB'ler) içindir.

## 📌 DB Dialect vs DB Driver

`dialect` `Türkçe` anlamı: lehçe

`Driver` bir DB'ye giderken soket üzerinden kullandığı erişimi sağlayan yazılımdır (`Java` class implementation kodları). Bu kodlar, DB server'ı iletişime geçen network protocol'ü üzerinden haberleşme sağlatır. Farklı bir `driver` da aynı protocol ile haberleşmemizi sağlayabilir.

`Dialect` ise `SQL` türevi (ve o türevin versiyonudur). `SQL` dilinin birçok türevi ve versiyonu çıkmıştır. bazı DB'ler birçok farklı `SQL` türevine ve versiyonuna destek verir. işte bu durumlar için `Dialect` özelliğini kullanırız.

bir `Spring` uygulamasını `JPA` ile ayağa kaldırırken, hem `dialect` hem de `driver` vermemiz gerekir. eğer vermezsek default `dialect`'i ve `driver`'i kullanacaktır.

## 📌 SQL2o

açık kaynaklı `Java` `ORM` kütüphanesidir.

```java
Connection con = sql2o.open();

final String query =
    "SELECT id, name, address " +
    "FROM customers WHERE id = :customerId";

List<Customer> customers = con
    .createQuery(query)
    .addParameter("customerId", customerId)
    .executeAndFetch(Customer.class);
```

## 📌 Mybatis (⟷ old-name:IBatis)

- açık kaynaklı `Java` `ORM` kütüphanesidir.
- `JPA` implementasyonu değildir.
- `Mybatis` çok gelişmiştir:
  - `lazy load` desteği var
  - `annotation` desteği var
  - mapping desteği var (`ORM`)
  - `XML` ile önceden query'ler tanımlanıp çağrıldığı gibi, runtime'da da query'ler dinamik yaratılabilmektedir.

```java
Reader reader = Resources.getResourceAsReader("SqlMapConfig.xml");
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(reader);
SqlSession session = sqlSessionFactory.openSession();

session.delete("Student.deleteById", 2); // "Student.deleteById" query is declared on xml
session.commit();
session.close();
```

## 📌 JDBC driver types

`JDBC`'nin çalışma mantığını belirlemektedir. client tarafta URL girilirken `type` belirtilir. örnek: `Oracle DB`, `JDBC` kullanırken `URL` girerken şunu gireriz:

> JDBC:oracle:oci:@myHost:1521:inst1

burada `oci`, `Oracle DB`'nin belirlediği bir keyword'dür. `oci`, `type-2`'ye referans etmektedir.

### 📌📌 JDBC driver type numbers

- `Type 1 driver`

  `JDBC`-`ODBC (⟷ Open DB Connectivity)` bridge'i kullanılır.

  `ODBC`, `JDBC`'ye alternatif bir `API`.

- `Type 2 driver`

  OS'te yüklü olan bir `dll` kullanılır.

- `Type 3 driver`

  Network-Protocol driver (middleware driver).

  bizim Java uygulamamız direk DB'e bağlanmaz. Onun yerine arada bir proxy görevi gören bir servis vardır.

- `Type 4 driver`

  Database-Protocol driver/Thin Driver (Pure `Java` driver)

  `OJDBC`'de `thin` olarak geçmektedir.
  
  bu tipte bağlantılar %100 `Java` kodu kullanır. DB sunucusu ile `Java` soketi açıp haberleşirler. %100 `Java` olduklarından her tarafta kullanılabilirler.

## 📌 Oracle DB version history

`i`, `g`, `c` keyword'leri tamamen rastgele verilmiştir. teknik anlamda hiçbir önemi yok.

| full name and version         | initial version of series | latest version of series |
|-------------------------------|---------------------------|--------------------------|
| Oracle 7.2                    | 7.2.0                     |                          |
| Oracle 7.3                    | 7.3.0                     | 7.3.4                    |
| Oracle8 DB              | 8.0.3                     | 8.0.6                    |
| Oracle8i DB             | 8.1.5.0                   | 8.1.7.4                  |
| Oracle9i DB             | 9.0.1.0                   | 9.0.1.5                  |
| Oracle9i DB Release 2   | 9.2.0.1                   | 9.2.0.8                  |
| Oracle DB 10g Release 1 | 10.1.0.2                  | 10.1.0.5                 |
| Oracle DB 10g Release 2 | 10.2.0.1                  | 10.2.0.5                 |
| Oracle DB 11g Release 1 | 11.1.0.6                  | 11.1.0.7                 |
| Oracle DB 11g Release 2 | 11.2.0.1                  | 11.2.0.4                 |
| Oracle DB 12c Release 1 | 12.1.0.1                  | 12.1.0.2                 |
| Oracle DB 12c Release 2 | 12.2.0.1                  |                          |
| Oracle DB 18c           | 18.1.0                    |                          |
| Oracle DB 19c           | 19.0.0                    |                          |

`Oracle DB`'nin, `OJDBC`, `JDK`, `JDBC` spesifikasyonu sürümleri uyumu için burada resmi compatibility matrix table'lar mevcut:

(source-id: 400)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 DB Connection vs TCP socket

1 DB bağlantısı, sadece 1 `TCP` soketi veya `IPC` üzerinden yapılır.

Fakat bu durumun 2 istisnası var:

- `Distributed transaction`:

  - <https://www.ibm.com/docs/en/i/7.5?topic=transactions-example-using-connection-multiple>

    This is an example of how to use a single connection with multiple `transaction`s.

  - <https://www.ibm.com/docs/en/i/7.5?topic=transactions-example-multiple-connections-that-work-transaction>

    This is an example of how to use multiple connections working on a single `transaction`.

- `Connection Pool` proxy/manager kullanınca: `PgBouncer`, `Oracle Net`, `ProxySQL`.

## 📌 Transaction vs connection vs session

1 connection fiziksel `TCP` bağlantısıdır.

- birden fazla `session` içerebilir (ama aynı anda olamaz). (`ORACLE-İSTİSNA-NOT-1` başlığını okunmalı)
- birden fazla `transaction` içerebilir (ama aynı anda olamaz).

## 📌 Multi session in 1 connection (only for Oracle DB) (ORACLE-İSTİSNA-NOT-1)

`Note-1`: Bu durumun istisnası `Oracle DB`'de var. Bu diğer DB'lerde böyle mi bilmiyorum. Ben sadece `Oracle DB` için okudum. `Kaynak: <https://books.google.com.tr/books?id=NG4RpD8aLEIC&pg=PA170&redir_esc=y#v=onepage&q&f=false> page:171, title: "connections vs sessions", Expert Oracle Database Architecture: Oracle Database 9i, 10g, and 11g, Author: Thomas Kyte.`

Yukarıdaki kitapta şu metin yazıyor:

```text
AUTOTRACE command and discover that we have two sessions! Over a single connection, using a single process, we'll establish two sessions. Here is the first:
```

Fakat 2 `session` olduğu zaman, diğeri pasif konumda olduğunu gösteriyor kitapta. Yani aslında, yine ikisi birlikte aynı anda işlenmiş olmuyor.

`Oracle DB`'de `Alter session` `SQL`'i var. Var olan `session`'nın user'ı değiştiriliyor. <https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/ALTER-SESSION.html> Bu dökümanda şu yazıyor:

> Use the ALTER SESSION statement to set ⟷ modify any of the conditions or parameters that affect your connection to the database. The statement stays in effect until you disconnect from the database.

Bu cümleden anlaşıldığı gibi bu `session` değişimi `connection`'ın sonuna kadar kalıyor. Yani aynı anda 2 `session` olma gibi bir durum olsaydı, `until you disconnect from the database` yerine `until you close the session` gibi bir terim kullanılması gerekirdi.

## 📌 session

`Session` sanal bir kavramdır.

1 `connection`'da aynı anda ya 1 `session` olur yada hiçbir `session` olmaz. (`ORACLE-İSTİSNA-NOT-1` başlığını oku)

Bazı özellikler aynı `session` içinde tamamlanmak zorundadır. Örneğin; 1 `transaction`, 2 farklı `session` üzerinden tamamlanamaz. Bu durumu şuradan anlıyoruz: <https://www.pgbouncer.org/features.html> uygulaması 3 farklı mod sunuyor:

- `Session pooling`

  client her `connection` için, `pgbouncer`'daki ayrı 1 DB `connection`'ını kullanır.

- `Transaction pooling`

  client her `transaction` için, `pgbouncer`'daki ayrı 1 DB `connection`'ını kullanır.

- `Statement pooling`

  client her `SQL statement` için, pgbouncer'daki ayrı 1 DB `connection`'ını kullanır.

`pgbouncer`'ın `features` sayfasında, `Statement pooling` için şu not düşülmüştür:

> Multi-statement transactions are disallowed.

Yani bu bilgiden; 1 `session`'ın, 1 `transaction`'ı tümüyle tamamlanması gerektiğinin sonucuna varabiliriz.

Benzer şekilde aynı dökümanda `SQL feature map for pooling modes` başlığında, `transaction pooling` yönteminin bile bazı özellikleri tamamiyle destekleyemediği yazıyor.

`Connection` ve `session` terimleri birbiri yerine sıkça kullanılır. Oysa farklı kavramlar. Fakat apaynı amaç ve birebir ilişki olduğu için DB'ı yazan geliştirici için önemli iken, `backend` yazılımcısı için bu fark önemsizdir.

Bu ayrım yazılımcı için önemli olsaydı, `JDBC` hem `session`'ı hem de `connection`'ı tek bir nesne üzerinden yönettirmezdi. <https://docs.oracle.com/javase/8/docs/api/java/sql/Connection.html> `JDBC`'nin `Connection` sınıfı dökümanında şu yazıyor:

```text
A connection (session) with a specific database.
```

## 📌 connection pool per user

1 `connection pool` içerisindeki her `connection` aynı user'a ait olmak zorunda değil. `kaynak: <https://commons.apache.org/proper/commons-dbcp/> 1st paragraph.`

## 📌 session ⟷ connection per user

- 1 `session` max 1 user ile ilişkilidir. (`ORACLE-İSTİSNA-NOT-1` başlığı okunmalı)
- 1 `connection`'daki her `session` farklı user kullanabilir.
- Aslında `connection` direk user ile yapılmaz. `Session`'ın user'ı vardır.

Fakat `ORM`'ler veya `JDBC` gibi kütüphaneler bu kuralları katılaştırabilir. Örneğin; `JPA`'da her `connection` 1 user ile açılabiliyor. Fakat `Oracle DB`'de aynı `session` bile farklı user'a çevrilebiliyor (`ORACLE-İSTİSNA-NOT-1` başlığı okunmalı).

## 📌 HikariCP

Alternatifleri:

- `Tomcat DBCP`
- `DBCP2`.

`JDBC` için `connection pool` kütüphanesidir. Buraya bakılırsa: `(source-id: 397) "What does correctness and reliability mean?" başlığı`, `Connection pool` yönetiminin ne gibi yetenekleri olduğu görülmektedir.

Örneğin `HikariCP` şu özelliği ek olarak sunuyor: `Response time Guarantee`. Bu özelliğe `accurate timeouts` da denir. `HikariCP` asenkron olarak `connection` açar, böylece `connection` açılma süreleri önceden bellidir.

## 📌 connection pool size

`HDD`'lerde cihaz yavaşlığından kaynaklı bloklanma vardı. Fakat `SSD`'lerde block'lanma daha az çünkü `SSD` hızlı cevap dönüyor. `SSD`'ler hızlı olduğundan yazılımcılar direk olarak güçlü makine psikolojisi ile `pool` sayısını arttırıyor, oysa; bloklanma az olduğundan `CPU`'nun ve diğer tüm cihazların `thread` switch maliyetini düşürmenin avantajlı olduğu görülmektedir.

ortalama `connection pool` sayısı DB çalıştıran makinanın şu özelliklerinden hesaplanır:

`connections = (CPU_core_count * 2) + effective_spindle_count`

`Hyper-threading` aktif ise bile sanal `CPU` sayısı ile değil, gerçek fiziksel core `CPU` sayısı referans alınmalıdır.

yukarıdaki tüm bilgiler için kaynak: `(source-id: 398)`

`spindle`; DB'yi okuduğumuz diskten aynı anda kaç `IO` işlemi yapabileceğimizi temsil eder. 1 diskten 1 `IO` işlemi yapılabiliyor diye varsayılır. Ama `RAID` kullanılan sistemlerde paralel okuma yapılabiliyor. Ne kadar çok paralel yapılabiliyorsa, `effective_spindle_count` bu sayıya eşittir.

Yukarıda 1 DB sunucusundan bahsedildi. 10 `pool` sunucu tarafından ayarlanmış olsun. Eğer sadece 1 instance uygulama bu sunucuya istek yapıyor ise; client taraftaki ayara max ve min `pool` count 10 verilebilir.

yukarıdaki bahsi geçen `pool` sayısı sadece 1 DB ve 1 app bu DB'de işlem yaparsa geçerlidir. diğer durumlarda farklı `connection pool` sayısı ile çalışmak daha verimli olabilir. `kaynak: (source-id: 399) "brettwooldridge" comment on November 20, 2017.`

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 JDBC URL form examples

## 📌 Definitions

Her DB instance, database, `şema` kavramları farklıdır. Örneğin;

- `MongoDB`'de `şema` kavramı opsiyoneldir.
- `Oracle DB`'de `şema` kavramı user kavramı ile aynıdır. Aslında `şema` tanımı yoktur. User'ların neleri görebileceğini tanımlarız. Yani; bir `şema` tanımı yapıp altına hangi DB'lerin ve diğer hangi objelerin olacağını tanımlayacağımıza, user tanımlayıp, onun nelere erişip erişmediğini tanımlamaktayız.
- `MSSQL` ve `PostgreSQL`'de `şema` tanımı vardır.
- `MySQL`'de `şema` ve DB kavramları aynı şeydir.

## 📌 Multi DB on same DB instance

- `Oracle DB` supports one DB per instance unless you are working with `12c` and above and using `Oracle DB` `Multitenant`.
- `MongoDB`, `MSSQL`, `PostgreSQL`, `MySQL` supports multiple databases per instance.

### 📌📌 JDBC URL password

with username pass:

```text
JDBC:oracle:thin:ahmet/12345@//myHost:1111/myInstance1
```

without username pass:

```text
JDBC:oracle:oci:@myHost:1111:myInstance1
```

## 📌 System Identifier (⟷ SI) (For Oracle DB)

An `Oracle DB` server must have at least 1 `SID` per instance.

using `SID`:

```text
jdbc:oracle:thin:@myOracle.db.server:1111:MY_SERVICE_ID
```

using service name: (service name is the label of `SID`)

```text
jdbc:oracle:thin:@myOracle.db.server:1111/MY_SERVICE_ID
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 JDBC

```java
PreparedStatement pstmt = null;
try {
   String SQL = "Update Employees SET age = ? WHERE id = ?";
   pstmt = conn.prepareStatement(SQL);
   // other code here
}
catch (SQLException e) {
   // other code here
}
finally {
   pstmt.close();
}
```

Her açılan `JDBC` `connection`'ı için birçok `statement` tanımlanabilir. `statement`'lerin birçok türevi vardır. `statement` objelerimizi bir kere oluşturduktan sonra dilediğimiz kadar üst üste aynı `connection`'da çağırabiliriz. `statement`'ler (tipine göre) gerektiğinde database-side caching'de kullanmaktadır. `statement` tipleri:

- `Statement`

```java
Statement stmt = con.createStatement();

stmt.executeUpdate("CREATE TABLE STUDENT(ID NUMBER NOT NULL, NAME VARCHAR)");
```

- `PreparedStatement`

```java
PreparedStatement pstmt = con.prepareStatement("update STUDENT set NAME = ? where ID = ?");
pstmt.setString(1, "ahmet");
pstmt.setInt(2, 111);
pstmt.executeUpdate();
```

- `CallableStatement`

`store prosedür`leri çağırmak için kullanılır.

```java
CallableStatement statement = con.prepareCall("{call anyProcedure(?, ?, ?)}");
// Use statement.setter() methods to pass IN parameters
statement.execute();
```

## 📌 Cancel statement

`Statement` instance'ındaki query çalıştırma metotları sync çalışıyor. Bu sebeple `cancel` etmek için `statement` instance'ını başka `thread`'ten çağırıp `cancel` etmek gerekiyor.

```java
if(statement != null && !statement.isClosed()) {
    statement.cancel();
}
```

`<https://docs.oracle.com/javase/7/docs/api/java/sql/Statement.html#cancel()> writes:`

```text
Cancels this Statement object if both the DBMS and driver support aborting an SQL statement
```