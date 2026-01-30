# DATABASE

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 CouchDB

Document based bir `NoSQL`'dir.

## 📌 CouchDB vs Couchbase

__CouchDB__, `IBM`'de çalışan bir developer tarafından geliştiriliyordu. Daha sonra bu developer, kodu açık kaynaklı olarak yayımladı ve __Apache vakfı__ projeyi devam ettirdi ve "__Apache CouchDB__" ismi ile devam etti.

__CouchOne (⟷ old-name:CouchIO)__, "__Apache CouchDB__" yazılımını geliştiren şirketin ismidir.

__CouchOne__ firması, __Membase (⟷ old-name: NorthScale)__  firması ile birleşerek __Couchbase__ isimli şirket kuruldu.

__Membase__ şirketi, __Membase__ isimli product'ı geliştiriyordu. __Membase__'i geliştiren lead developer'lar __Memcached__ projesinde de çalışmıştı. __Memcached__ ürünü, __Memcached__ protocol'ünü kullanır.

__Couchbase__ firması, __Membase__ ve __Apache CouchDB__ yazılımlarını birleştirerek "__Couchbase Server__" isimli yazılımı yarattı. Yeni yazılım __Membase__'in yeni versiyonu olarak tanıtıldı. __CouchDB__ ile uyumluluk gibi bir durum söz konusu değildir.

Yıl 2022 itibariyle "__Couchbase Server__" ve "__Apache CouchDB__" olarak 2 ayrı proje birbirinden bağımsız şekilde geliştirilmektedir.

__Couchbase Server__'ın hem __Community__ hem de __Enterprise__ sürümü mevcut.

## 📌 N1QL

acronym of: `non-first normal form query language`.

pronounced: nickel.

`Couchbase`'in kullandığı `SQL` alternatifi dildir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 nesne-ilişkisel empedans uyumsuzluğu (⟷ object-relational impedance mismatch)

Object-oriented bir dilde, DB entity'lerimizde kullandığımız teknikler birbirlerinden farklıdır:

- `Granularity Problem`

  Customer içinde Adres olan bir nesnenin tek bir tabloda karşılığı olamaz. Bunun için her class'a istinaden bir tablo açmak gerekir.

- `inheritance`

  `RDBMS`'te inheritance yoktur.

- `identity`

  Object-oriented'da equals metodunda herhangi bir field ve/veya logic ile eşitlik kontrolü sağlanabilir. Oysa `RDBMS`'te sadece primary key ile bir satırın diğerine eşit olup olmadığını tanımlayabiliyoruz.

- `data navigation`

  Object-oriented'da bir nesneye erişim referans olan field'dan yapılabilir: `customer.getOrder().getAddress()`. Oysa `RDBMS`'te bu iş `SQL join` ile olmaktadır.

- `type mismatch`

  list gibi tipler `RDBMS`'te tutulduğunda bambaşka bir formatta tutulmaktadırlar. Diğer her tipi 1e1 karşılığı olmayabilmektedir.

Bu sorunları ortadan kaldırmak için `NoSQL`'e başvurulabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Lookup table

lookup kelime anlamı: arama.

Türkçe kaynakların bazılarında "__başvuru çizelgesi__" olarak kullanılıyor.

lookup table'ın sıradan bir tablodan hiçbir farkı yoktur. Sadece tablonun oluşturulma amacı bir şeylere hızlı veya daha simple bir API ile erişim sağlayabilmektedir.

lookup table'ın kaç sütunu olabileceği gibi standartlar yoktur. Lookup table çok genel bir terimdir.

Örnek durumlar:

- çok sık arama için kullandığımız sadece product-id ve product-name bilgilerini cache'te hızlı erişmek için tuttuğumuz tablo lookup table'dır. (Hızlı erişim için çözüm)
- Resim dosyalarında ekranda resmi gösterirken veya resmi edit'lerken, ekranda olan renkler hızlı erişim için editor tarafından gruplanarak tutuluyor. Bu gruplu bilgiler sadece RAM'de tutuluyor. Resim dosyasında bu bilgiler yok. Bu RAM'deki tabloya lookup table denir. (Kompleks API için basit API çözümü)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 scale and precision

matematikteki terim anlamları tamamen başka amaçla kullanılır.

```sql
CREATE TABLE table1
(
  -- precision: 5
  -- scale:2
  -- max value: 999,99
  -- 5-2=3 basamak var tam sayı kısmında.
  MyDecimalColumn DECIMAL(5,2)
);
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 DUAL

`Oracle DB` içerisinde gelen bir tablodur. Tablonun içerisinde sadece `DUMMY` isimli, `char` tipinde bir sütun vardır. İçerisinde de sadece 1 kayıt vardır. Onun içeriği de `X` `string`'i vardır.

`Dual` tablosu aşağıdaki tarz sorguları atabilmemizi sağlamaktadır:

```sql
SELECT 1+1
FROM dual;

SELECT 1
FROM dual;

SELECT USER
FROM dual;

SELECT SYSDATE
FROM dual;
```

`MSSQL`'de `dual` tablosuna ihtiyaç yoktu çünkü bu çalışmaktadır:

```sql
SELECT 1+1
```

Bazı DB'lerin `Oracle DB` ile uyumlu olmak için `dual` tablosu default'ta gelmektedir. Bazı DB'lerde ise aynı amaçla farklı tablo ismi kullanılmaktadır. Bazılarında ise hiç `dual` tablosu yoktur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 BSON (⟷ binary JSON)

`MongoDB`'nin data'ları `file system`'a kaydettiğinde kullandığı bir formattır.

örnek `JSON` dosyası:

```json
 {"hello": "world"}
```

buna denk gelen BSON dosyası:

```text
\x16\x00\x00\x00          // total document size
\x02                      // 0x02 = type String
hello\x00                 // field name
\x06\x00\x00\x00world\x00 // field value
\x00                      // 0x00 = type EOO ('end of object')
```

BSON dosyası, ona denk gelen JSON dosyası ile karşılaştırıldığında bazen daha büyük boyutlu veya bazen daha küçük boyutlu olabiliyor.

BSON, JSON'ın desteklemediği date, binary-data gibi formatları da destekliyor. Bunlara "BSON type" deniliyor ve her bir tipe bir numara verilmiş. örneğin; String 2, Date 9. kaynak: <https://www.mongodb.com/docs/manual/reference/bson-types/>

BSON dosyaları binary oldukları için human-readable değillerdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 cluster vs data center (⟷ data centre)

data center fiziksel bir kavramdır. data center aynı fiziksel lokasyonda olan (bu sebepten genelde aynı LAN'a bağlı olan) bilgisayarlar topluluğudur. Oysa cluster tamamen logically sınıflandırılmış/gruplandırılmış yazılımlar kümesi anlamına gelir.

Aynı yazılımın 1 cluster'ı içerisinde birden fazla data center olabilir. Veya 1 data center içerisinde, aynı yazılımın birden fazla cluster'ı olabilir.

1 cluster içerisindeki veriler sync olmak zorunda değil. parçalanmış olabilir, yada sadece bazı data'lar sync, bazı data'lar parçalanmış olabilir. Bu tamamen yazılımın nasıl çalıştığına bağlıdır.

Cluster'lar arası haberleşmede şu gibi durumlar olabilir:

- cluster çökme senaryoları için, bir cluster diğer bir cluster ayakta mı? eğer ayakta değilse devreye girmeye hazırlanması için bazen birbirleri arasında haberleşme sağlayabilirler.
- cluster'ın tümünün çökme senaryoları için bir cluster diğer bir cluster'ı sürekli kendine kopyalayabilir. Bu herhangi bir client'ın o cluster'dan data çekmesine benzerdir.
- ve dahası.

Ama asıl olarak cluster'lar arasında şu olmamalı: Bir cluster içindeki bir node, diğer bir cluster'ın içindeki node'un tamamlayıcısı olmamalı. Fakat maliyet ve hızlı geliştirme gibi sepeplerdne bunu yapmak durumunda kalabiliyoruz.

Yazılım sunucuları birçok node'da ayağa kaldırıldığında bu yazılımlara her cluster ve her data center'ı tanıtmak gerekir. Çünkü data center'lar arası data paylaşım politikası ile data center içerisindeki data paylaşım politikası farklı olabilir. Örneğin; Cassandra'da, aynı cluster içindeki farklı, data center'larda farklı politika izlemesini sağlayabiliyoruz. örnek:

```sql
CREATE KEYSPACE custom_keyspace
-- here column names...
WITH REPLICATION = { 'class' : 'NetworkTopologyStrategy',
                     'NewYork':5,
                     'Dubai':10
                   }
```

## 📌 availability domain

yoğun yük durumuna karşı çalışan data center'ların tümüdür.

## 📌 High-availability clusters (⟷ HA clusters ⟷ fail-over clusters)

fail-over kelime anlamı: yük devretme

Bu terim sıkça DB alanında kullanılsa da, bilişimde diğer birçok alan içinde geçerlidir. Herhangi bir server yazılım grubu için High-availability'den bahsedilebilir.

Her DB'nin genelde clone'u/replikası tutulur. Bu şekilde bir DB instance'ına zarar gelirse, yedeği olmuş olur ve aşığı yük karşı işlemler dağıtılmış olur. Bunu yapmak için birçok farklı teknik var:

- __Active/active__

  her node sync bir şekilde birbirinin her güncellemesini anlık olarak alır/sync olur. Bu yavaş bir tekniktir fakat en stabildir. Her instance/node sürekli dışarıya hizmet veriyor olur.

- __Active/passive__

  passive node'lar, active olan (dışarıya hizmete açık olan) DB node'larındaki tüm değişiklikleri kendine sürekli olarak alır. Fakat passive node'lar dışarıya hizmet vermezler. Eğer active bir node'a zarar gelirse, passive olan herhangi bir node active duruma geçer.

## 📌 Oracle DB's High Availability Architectures and Solutions

`Oracle DB` ürünü (default'ta) sadece tek başına tek 1 instance ve tek 1 cluster olarak çalışır. Diğer her feature ekstra çözüm uygulamak gerekir. Bunlar burada detaylı listelenmiştir: https://docs.oracle.com/cd/B28359_01/server.111/b28281/architectures.htm

Bu çözümlerden biri de 'Data Guard'dır. Bu çözümde "Primary database" e hem yazılıp hem okunabiliyor. Fakat farklı data center'larda olan birden çok `Oracle DB` instance'ı "Standby database" olarak çalışıyor. Primary'ya yapılan her işlem otomatik olarak her standby'a da yollanıyor. Standby sürekli olarak sadece read-only hizmet verebiliyor. Fakat eğer primary çökerse, bir standby primary olarak çalışmaya başlıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Consistency Models

RDBMS'ler "Immediate Consistency" sunarlar. fakat ayarlanır ise; bazı NoSQL DB'leri da "Immediate Consistency" sunabilir.

birçok çeşit Consistency Model'i var. Bu modeller burada anlatılmıştır: https://res.mdpi.com/d_attachment/futureinternet/futureinternet-11-00043/article_deploy/futureinternet-11-00043.pdf Yazar: Miguel Diogo, Bruno Cabral, Jorge Bernardino, "Consistency Models of NoSQL Databases", "Figure 2.Consistency Models based on Reference" görseli.

Consistency nereden bakıldığına göre (client ve sunucu açısından) değişir.

### 📌📌 User-centric consistency models

#### 📌📌📌 Consistency Guarantees

Bu başlık altındakiler birer model değildir. Fakat birer feature olarak değerlendirilirler.

##### 📌📌📌📌 read your own writes (⟷  read-your-own-writes ⟷ read-your-writes ⟷ read your writes ⟷ RYW)

bir client bir bilgiyi kaydettikten hemen sonra o bilgiyi okumak isterse, güncel kaydettiği bilgiyi okur.

##### 📌📌📌📌 Monotonic writes (⟷ MW)

bir client sırası ile istek attığında bunlar sırası ile işletilecektir.

##### 📌📌📌📌 Monotonic reads (⟷ MR)

client bir bilgi okumuş ise; o bilgi okunduktan sonra tekrar update edilmedi ise, o bilgi tekrar tekrar okunduğunda aynı dönecektir.

##### 📌📌📌📌 Writes Follow Reads (⟷ WFR)

#### 📌📌📌 Eventual consistency (⟷ nihai tutarlılık)

weak ile strong arasında kalan bir tutarlılık biçimidir.

weak'teki gibi; client'lar eski data'ları çekiyor olabilir. fakat nihai olarak, belli süre geçtikten sonra aynı bilgiyi çekecektir. burada Weak Consistency'den fark; işlemlerin sıralı olmasıdır. Weak Consistency'de sıra sağlanamadığından, herkes aynı zaman aralıklarında farklı data çekebilir.

Bu fark neden olur? Cassandra'yı ele alalım. Cassandra'da, update ile insert işlemi aynı kapıya çıkıyor. 2 farklı Cassandra sunucusuna istek geldiğini düşünelim, ikisi de insert işlemi(yada update) yapıyor olsun. ikisi aynı anda replica'lara dağıtılırken/sync olurken, o sıra data okumaya çalışan farklı client'lar var ise, farklı sunuculardan okurlarsa aynı anda farklı data çekmiş olacaklar.

Cassandra örneğimizde; en son yazılan data, (belli bir süre sonra - nihai olarak) tüm replica'lara klonlanacağı için, nihai tutarlılık sağlanmış olacak.

### 📌📌 Data-centric consistency models

#### 📌📌📌 Causal consistency

#### 📌📌📌 FIFO consistency (⟷ PRAM consistency)

#### 📌📌📌 Cache consistency

#### 📌📌📌 Processor consistency

#### 📌📌📌 Strong Consistency (⟷ immediate consistency ⟷ Linearization ⟷ strict consistency ⟷ atomic consistency)

client'a cevap ancak; tüm sunuculara iletildikten ve kaydedildikten sonra gerçekleşir. dolayısı ile en yüksek seviye consistency budur. tüm sistemde kayıtların/işlemlerin sırası tektir/sabittir.

#### 📌📌📌 Weak Consistency

client'a gönderilen cevap en güncel data'yı içermeyebilir. tüm sistemde kayıtların/işlemlerin sırası tutulmaz.

client'ın eski data alması durumuna "inconsistency window" adı verilmiştir.

#### 📌📌📌 Sequential Consistency

tüm client'lar aynı zaman aralıklarında aynı bilgiyi okurlar. örneğin; X güncellemesi, replica'lara sync ediliyor olsun. 10 replica'dan 5'i X güncellemesini alsın. Tam o sırada istek atan client'ların tümü X güncellemesini almalı yada herkes eski halini görmeli. Client'ların bir kısmı X ile bir kısmı eski halini görmemeli. Eğer bu durum sağlanabiliyorsa "Sequential Consistency" var demektir. kaynak: https://medium.com/cosmosgaminghub/consistency-e3e0fe41358d "2–1. Sequential Consistency" başlığı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Replication models

Bu başlıktaki tüm bilgiler için kaynak: `https://riunet.upv.es/bitstream/handle/10251/54786/TFMLeticiaPascual.pdf Yazar: Leticia Pascual Miret, Master's Degree in Parallel and Distributed Computing`

### 📌📌 Passive replication model (⟷ Primary backup)

tüm client'lar sadece "primary server" denilen sunucuya istek atar. bu sunucu diğer replica'lara klonlama işlemini gerçekleştirir. sync yada async yapıp yapmayacağı implementasyona göre değişir. eğer sync backup işlemi yapılıyorsa; client'a geç cevap dönülür. bu model'e __pessimistic__ adı verilir. Tersi durumda async backup alınır ve client'a hızlıca cevap dönülür. Bu model'e ise __optimistic (⟷ eager)__ adı verilir.

### 📌📌 Active replication model

Passive'de tek bir yükümlü sunucu varken (primary server), burada birden fazla olmaktadır. Dolayısı ile, tutarlılığı sağlamak daha zordur. Fakat bu model'in __fault tolerance (⟷ hata töleransı)__ vardır.

### 📌📌 Semi-passive and semi-active replication models

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 partitioning vs sharding

sharding kelime anlamı: parçalama

shard kelime anlamı: parça

partitioning kelime anlamı: bölümleme

"__partitioning__" bir tablonun satırlarının veya sütunlarının ayrı fiziksel lokasyonlara ayırmaktır. Eğer partitioning işlemini farklı makinelere yapıyor isek, bu işlem "__sharding__" olur. kaynak: https://hazelcast.com/glossary/sharding/ title: "What Is the Difference between Sharding and Partitioning?".

Ek not: DB programı farklı fiziksel makineler olup olmadığı anlayamaz. Burada, farklı DB instance'ı olup olmadığına bakmalıyız.

## 📌 Horizontal Partitioning (⟷ Yatay Bölümleme)

Aynı tablonun farklı satırları farklı fiziksel yerlerde olma durumu.

|              | physical location 1 | physical location 2 |
|--------------|---------------------|---------------------|
| row id range | 0-100               | 100-200             |

Bu fiziksel lokasyonlar farklı DB instance'ı ise; __Horizontal Sharding__ yapmış oluruz.

## 📌 Vertical Partitioning (⟷ Dikey Bölümleme)

Aynı tablonun sutunlarının farklı fiziksel yerlerde olma durumu. kaynak: https://subscription.packtpub.com/book/big-data-and-business-intelligence/9781788397186/5/ch05lvl1sec38/vertical-partitioning 1st sentence.

|         | physical location 1 | physical location 2 |
|---------|---------------------|---------------------|
| columns | Name, Surname       | City, Country       |

Bu fiziksel lokasyonlar farklı DB instance'ı ise; __Vertical Sharding__ yapmış oluruz.

Vertical partitioning pratikte uygulama seviyesinde yapılır. Örneğin customer tablosundaki sütunları 2 farklı tabloya bölerek ve farklı tablolara dağıtarak bu işlemi yapabiliriz. Eğer tüm data'yı çekmek gerekirse 2 data'yı ard arda çekmemiz gerekir.

## 📌 ne zaman hangisini kullanmak lazım?

Sadece bazı sütunları okuyor isek, sadece bazı node'lara erişeceğimiz durumlar için "Vertical Partitioning" daha verimli. Fakat tüm sütunları çekiyor, fakat belli satırları query ile çekiyorsak (örneğin; şu tarihte kayıt yaptırmış kullanıcılar) bu durumda sadece bazı node'ları kullanacağımız için "Horizontal Partitioning" daha verimli.

## 📌 Horizontal + Vertical Partitioning

Her ikisini de aynı tablo için uygulayabiliriz.

## 📌 terminoloji

"shard" veya "partition" terimleri DB'den DB'ye farklı anlamları olabiliyor. Buna 2 örnek:

- Dosyalara parçalarken oluşturulan yeni tablolara "shard" veya "partition" adı verilir. kaynak: https://hazelcast.com/glossary/sharding/ 1st paragraph.
- Dosyaların bölündüğü farklı DB instance'larının her birine "shard" denir. kaynak: https://docs.oracle.com/en/database/oracle/oracle-database/18/odpnt/featDataSharding.html 1st paragraph, 2nd sentence.

## 📌 Horizontal Scaling (⟷ Yatay Ölçeklendirme)

yeni makine (node) eklenerek güç arttırılması.

## 📌 Vertical Scaling (⟷ Diken Ölçeklendirme)

var olan makinelere `CPU`, memory, disk gibi kaynaklar eklenerek güç arttırılması.

Bunun en önemli 2 dezavantajı:

- Tüm bilgiler tek node'da toplanıyor (`SPOF`)
- Her `OS` veya her donanım veya her yazılım sonsuz sayıda CPU veya memory veya disk gibi kaynakların eklenmesini resmi olarak desteklemiyor olabilir.

## 📌 scaling verbs

- `scale up`: adding more resource (like RAM, CPU) to a machines/instance.
- `scale down`: removing resources (like RAM, CPU) from a machines/instance.
- `scale out`: adding more machines to cluster or system
- `scale in`: removing machines from cluster or system

## 📌 Network Partitioning (⟷ Split Brain Syndrome)

Bağımsız cluster'daki node'lar, diğer cluster çökmemesine rağmen çökmüş olarak algılama durumuna verilen isimdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Liquibase vs Flyway

programatik DB şema yönetimi sağlarlar.

Migration gibi işlemlerde de kullanılırlar.

Opsiyonel her şema değişikliği için ayrı ayrı rollback senaryoları tanımlayabilmemizi sağlarlar.

Şema değişiklikleri:
- Flyway'de SQL hazırlanır.
- Liquibase'de DB'den bağımsız tanımlamalar yaptırtır.

örnek bir "liquibase.xml" dosyası:

```xml
<?xml version="1.1" encoding="UTF-8" standalone="no"?>
<databaseChangeLog xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                   xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
                   xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.9.xsd">

    <!-- her changeSet sırası ile DB şemasına işlenir.  liquibase kendi tablosunu oluşturur.
    bu tabloda hangi changeSet'lerin işlendiği, author'un kim olduğu, hangi date-time'da DB'ye işlendiği
    gibi bilgileri tutar.-->

    <!-- changeSet'lerde her şey olabilir: DB'ye yeni kayıt (satır) ekleme,
         yeni tablo oluşturma, tablo silme, tablo şeması güncelleme... -->

    <changeSet author="ahmet" id="changeSetId1">
        <createTable tableName="users">

            <!-- 2 adet sütun birlikte (username ve last_name), bir primary key görevindeler. -->
            <column name="username" type="VARCHAR2(255 CHAR)">
                <constraints nullable="false" primaryKey="true" primaryKeyName="username_primary_key"/>
            </column>

            <column name="last_name" type="NUMBER(19, 0)">
                <constraints nullable="false" primaryKey="true" primaryKeyName="username_primary_key"/>
            </column>

            <column name="CREATED_BY" type="NUMBER(19, 0)"/>
            <column name="CREATED_DATE" type="TIMESTAMP(6)"/>
            <column name="LAST_MODIFIED_BY" type="NUMBER(19, 0)"/>
            <column name="LAST_MODIFIED_DATE" type="TIMESTAMP(6)"/>
        </createTable>
    </changeSet>

    <changeSet author="mehmet" id="changeSetId2">
        <createTable tableName="orders">

        </createTable>
    </changeSet>
</databaseChangeLog>
```

Yukarıdaki liquibase.xml dosyasını bu şekilde ilgili DB'de işlem görmesini sağlayabiliriz:

```sh
"/path/to/liquibase" --driver="oracle.JDBC.OracleDriver" --classpath="/path/to/ojdbc7-12.1.0.2.jar" --url='JDBC:oracle:thin:@www.custom-db.com:1521/MY_DB' --username="user" --password="password123" --changeLogFile="liquibase.xml" update
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 savepoint

Line 5 çalıştığında, DB'nin state'inin satır 3'teki duruma gelmesini sağlar. Ama SQL kodu line 6'dan devam eder.

örnek:

```sql
BEGIN TRANSACTION
   INSERT INTO TestTable( ID, Value ) VALUES  ( 1, 'hi')
   SAVE TRANSACTION FirstInsert -- line 3
   INSERT INTO TestTable( ID, Value ) VALUES  ( 2, 'hello')
   ROLLBACK TRANSACTION FirstInsert --line 5
COMMIT
```

## 📌 try ve catch

```sql
BEGIN TRY
    -- Hatalı olabilecek işlemler
    BEGIN TRANSACTION

    INSERT INTO TestTable(ID, Value) VALUES (1, 'test')
    -- Örneğin hatalı bir sorgu:
    DECLARE @x INT = 1 / 0

    COMMIT
END TRY
BEGIN CATCH
    -- Hata durumunda burası çalışır
    ROLLBACK
    PRINT 'Hata oluştu: ' + ERROR_MESSAGE()
END CATCH
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Oracle Exadata (⟷ Oracle Exadata DB Machine)

sadece `Oracle DB` kullanımı için optimize edilmiş, `Oracle` tarafından satılan fiziksel sunuculardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 DB2

IBM'in geliştirdiği DB.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 varchar vs char vs nvarchar vs nchar

"n" prefix'i, string'in UNICODE karakterlerinin tümünü desteklediği anlamına gelir. "n" olmayan string'lerde, UNICODE'un sadece en sık kullanılan "plane 0" karakterleri (başka başlıkta anlatılıyor) destekleniyor.

- CHAR is fixed-size. char(100), 100 karaktere store etmese bile 100 karakterlik yer harcar. sürekli bu alanı reserved tutar.
- VARCHAR is variable-size. hücre, sadece karakterler sayısı kadar yer kaplar.

char; varchar'a göre çok daha performans sağlar fakat daha çok yer kaplar.

## 📌 Oracle DB Data Types

`Oracle DB`'da internal ve external data tipler mevcut.

- `VARCHAR2`, `NVARCHAR`, `CHAR`, `NCHAR` internal datatype olarak geçiyor.

- `VARCHAR2` aynı zamanda external dataype olarak geçiyor.

## 📌 blob

`blob` kelime anlamı: damla

context'e göre farklı anlamları var:

- açık kaynaklı bir OS çekirdeğinde; proprietary device driver'ları (closed-source device driver'ları) için kullanılan bir terimdir. buradan yola çıkan bazı insanlar, daha sonra binary blob terimini açık kaynaklı olan bir yazılımın içindeki kapalı kod binary'leri içinde kullanmaya başlamışlardır.

- DB sistemlerinde; __Binary Large OBject (⟷ BLOB)__ olarak kullanılıyor. bu objeler media objeleri gibi herhangi bir binary dosyaları olabilir.

## 📌 Spring ile BLOB tanımlama

Database tarafında __CLOB (⟷ Character large object)__ olarak kullanılırken, __BLOB__ binary objeler tutar.

CLOB'u, BLOB gibi kullanırsak hata alırız. Çünkü CLOB her byte'ı kabul etmez. CLOB belli karakter setlerini kabul eder. Ama tüm byte'lara karşılık gelen bir karakter yoktur.

Spring bu ikisi için tek annotation sunar ve field'ın tipinden (String veya byte[]) __CLOB__ veya __BLOB__ olup olmadığını anlar.

```java
import javax.persistence.Lob;
import javax.persistence.Column;

@Lob
@Column
private String name;

@Lob
@Column
private byte[] photo;
```

## 📌 String tiplerin farklılıkları

varchar gibi tiplerin maximum büyüklükleri sınırlı olabiliyor. Oysa CBLOB kullanıldığında bir satırın büyüklüğü terabyte'lara kadar çıkabiliyor.

CBLOB, belli bir karakter seti kabul edeceği için, bazı karakterlerde hata fırlatabilir veya hata fırlatmaz fakat "cast" işlemine tabi tutulabilir. bu sebeple karakter setine dikkat edilmelidir. eğer karakter set'inini bilmiyorsak veya emin değilsek BLOB kullanmalıyız.

Bazı DB'lerde karakter setini baştan verebiliyor. Örneğin MYSQL'de aşağıdaki gibi tanımlama yapılabilir:

```sql
CREATE TABLE t
(
  c1   VARCHAR(20)    CHARACTER SET utf8, -- utf8 is encoding but it has supported character set (bu durum başka başlıkta anlatılıyor)
  c2   TEXT           CHARACTER SET latin1 COLLATE latin1_general_cs -- cs ==> case-sensitive
);
-- collate kelime anlamı: harmanlama.
```

Her DB birçok farklı data type destekliyor. Örneğin; "JSON" tipi destekleniyor olabilir. Böyle durumlarda JSON'un validasyonu da DB tarafında da yapılabilir. JSON manipülasyonları için özel SQL fonksiyonları da sunuluyor olabilir...

## 📌 comparison of data types between DB systems

`Migration from Oracle DB to MySQL` gibi terimlerle aratıldığında çıkan dökümanlarda data tiplerinin karşılaştırılması bulunabilir. Örnekler:

- <https://cloud.google.com/solutions/migrating-oracle-users-to-mysql-data-users-tables> title: "Oracle to MySQL data type conversion".
- <https://dev.mysql.com/doc/workbench/en/wb-migration-database-mssql-typemapping.html>
- <https://dev.mysql.com/doc/workbench/en/wb-migration-database-postgresql-typemapping.html>

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 code first vs DB first vs model first

birer DB yazılım ilişki kurma yaklaşımlarıdır.

### 📌📌 code first

önce kod yazılır, ardından entity framework yazılımı DB'yi oto oluşturur.

### 📌📌 DB first

önce SQL kodları ile DB oluşturulur, ardından yazılımcı buna denk gelen modelleri kodda yazar.

### 📌📌 model first

genelde IDE'lerde DB dizayn araçları ile yapılan bir işlemdir. DB'yi bu araçta hazırlarsınız, bu araç hem kod hem de DB tarafını otomatik oluşturur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 obje

DB dünyasında obje; tablo, view, index'lerin (ve daha fazlası fakat her şey değil!) grup ismidir. bu durumda; örneğin USER tablosu bir DB instance'sidir.

## 📌 Schema (⟷ şema)

1 DB'de birden fazla şema olabilir. her şema altında birden fazla tablo vardır. Şema aslında tabloların, ilişkilerin ve daha birçok DB objesinin tanımıdır.

A user'ı, Y şemasının altındaki tüm objelere erişebilir gibi ayarlamalar kolayca yapılabilir.

bir tablonun ismini yazarken şu şekilde format kullanırız: şema_ismi.tablo_ismi

Şema bazı DB'lerde yok bazılarında ise farklı anlamlarda kullanılabiliyor. DB'den DB'ye çok değişiyor (bu konu başka başlıkta anlatılıyor).

## 📌 Constraint

Türkçe kelime anlamı: kısıt

şemayı oluştururken/değiştirirken tanımlanan, tabloların yapısını kısıtlayan kurallardır. örnek constraint'ler:

- primary key
- unique
- not null
- foreign key

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 string vs number sort/order performance

DB'de number'lar çok daha hızlı sort'lanır. sort'lanınca da index'lenmesi daha hızlıdır ve index'te aramada daha hızlıdır.

fakat string'de arka planda binary olarak tutuluyor. bu durumda o da aynı performansı göstermesini beklemez miyiz?

evet beklerdik. fakat string ordering DB'lerde natural ordering yapar. bu da her karakter için, hangi karakterden önce geleceğinin kurallarının olduğu anlamına gelir. tabi bu da yavaşlık meydana getirir.

Fakat natural ordering ihtiyacı yok ise, ve sadece hızlı read amaçlı index'leme istiyorsak, bazı DB'ler byte bazında index'leme yapabiliyor (bunun için "COLLATE BINARY" konusu internette aratılabilir).

böylece string sıralama hızlanır ama yine de number sıralama gibi hızlı değildir. bunun bazı sebepleri:

- UTF-8: karakter'e göre 1-4 byte arsında her karakter uzayabilir (bu konu detaylı başka yerde anlatılıyor). Number gibi fixed-length olmadığı için, CPU hesaplamaları daha yavaş.
- string'de natural order olmasa bile, byte bazında bir karşılaştırma yapılacaktır. 101010 olan bir sayı karşılaştırılmaz. byte karşılaştırma, string'e göre hızlıdır, ama number'a göre daha yavaştır. Çünkü bir "sayı", CPU'da tümüyle tek bir hamlede karşılaştırılıyor. Yani 9876 sayısı CPU da 1 işlemde karşılaştırılabiliyor. Ama byte karşılaştırması, her byte için, CPU'da ayrı işleme tabi tutuluyor.
- byte'ların sonunda padding olma durumu olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 In-memory DB

## 📌 data grid vs data structure store vs key-value store

grid kelime anlamı: kafes, şebeke.

"__data grid__" terimi; '__grid computing__'den gelir.

"data grid" terimi front-end'de de kullanılır. Data'yı filtre, sort, paging gibi gelişmiş seçenekler sunan tablo component'idir.

__grid computing__: Büyük, yoğun ve karmaşık hesaplama görevlerini, tek bir güçlü bilgisayar yerine birçok bilgisayarın gücünü birleştirerek yapmak.

__data grid__; data'nın birçok node'a paylaştırılarak(distributed) yönetilmesini sağlayan sistemlere/mimarilere verilen genel ifadedir.

__in-memory data grid (⟷ IMDG)__ terimi ise isminden de anlaşıldığı gibi RAM'de yönetilen data-grid'lerdir.

__key-value DB__'leri, sadece bir key'e karşılık herhangi bir tipte veri tutarlar. Fakat __data structure__ DB'leri key-value ile kısıtlı değillerdir. list, set, array gibi daha karmaşık veri yapılarını da destekler. dolayısı ile __data structure__ DB'leri aynı zamanda key-value veri yapısını da desteklemiş olur

## 📌 local cache

`In-memory DB`'lerin önemli özelliklerinden biri de local-cache özelliğidir. Redis gibi data store'ları genelde kalıcı data için değilde, çok kısa vadeli data'ları tutmak için kullanılırız. Bu sebeple genelde kayıt atan uygulama aynı data'yı tekrar okur. Bu sebeple local cache özelliği vardır. local-cache okunan bilgiyi lokalde tutar ve her güncellemesini de sunucu ile sync tutar. Bu şekilde tekrar okuması gerekirse direk lokal'den okur. Local-cache policy'leri DB'den DB'ye çok fark edebiliyor.

## 📌 distributed cache

Cache, genelde;

- NoSQL'dir
- in-memory'dir,

fakat böyle bir zorunluluk yok.

örnek sistemler:

- CDN

- Redis, Hazelcast gibi yazılımların instance'larını distributed data tutması için multi-cluster ayarlayabiliriz.

  Hazelcast "Map" yapısında bir veri tuttuğunda bu data'ları kendi içinde partitioned olarak tutar. Bundan client'ın haberi yoktur. Hazelcast bunları aynı zamanda multi-instance/node üzerinde klonlayabilir (replika) vs...

- Her `Java` microservice'imiz kendi içinde API response'larını cache'leyebilir. Aynı parametrelerle istek atıldığında aynı cevabı döner.

## 📌 in-memory non-RDBMS

| name       | developer  | language | open source | distributed data                   |
|------------|------------|----------|-------------|------------------------------------|
| Infinispan |            | Java     | yes         | supported                          |
| Ignite     | Apache     | Java     | yes         | supported                          |
| Hazelcast  | Hazelcast  | Java     | partially   | fully supported easily             |
| Redis      | Redis labs | c        | partially   | limited (via redis-cluster module) |
| memcached  |            | c        | yes         | very limited                       |
| Ehcache    |            | Java     | yes         | not possible                       |

`distributed data` yönetimi kendi içinde birçok feature barındırabilir:

- auto rebalancing
- non-auto rebalancing
- sharding, partitioning, vertical scaling, horizontal scaling
- Network Partitioning durumunda nasıl davranacağı
- multi-cluster high availability (async and/or sync backup/replication)

`memcached`, `Redis`'e göre daha az özellik içerir.

Redis için çok fazla 3üncü parti eklenti bulunur. Redis çok gelişmiştir ve bu sebeple hakimiyet ister. gelişmiş özellikler kullanılmayacak ise memcached tercih edilmelidir.

Ignite, arkada RDBMS tarzı tablolar tutmuyor fakat;

- Kolaylık olsun diye SQL (hem manipülasyon, hem query) destekliyor.
- Bağımsız tablolarda JOIN dahi destekliyor.
- ACID destekliyor.

Ignite, infinity span'a göre çok daha gelişmiş.

## 📌 in-memory RDBMS

| name                 | open source | developer | language |
|----------------------|-------------|-----------|----------|
| h2                   | yes         |           | Java     |
| Derby                | yes         | Apache    | Java     |
| HSQLDB (⟷ HyperSQL) | yes         |           | Java     |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 RDBMS

### 📌📌 MySQL vs MariaDB

`MySQL`, `Oracle` tarafından geliştirilen open source bir DB'dir. ilk geliştirildiğinde `Oracle`'da değildi, fakat `Oracle` satın alınca community `MariaDB`'yi oluşturdu. `MariaDB` gerçek bir community ürünüdür ve geliştirme süreçleri bile takip edilebilir durumdadır. `Oracle`'ın kendi DB'si olduğundan, birçok proje, uzun vadede lisans veya farklı problemlerden kaçındığı için `MariaDB` kullanmaktadır.

`MariaDB`, resmi olarak `drop-in replacement of MySql` olarak sunulmaktadır.

yani `MySQL` yerine hemen geçilebilir durumdadır. Fakat son yıllarda geçiş iş yükü (farklılıklar) gitgide artmaktadır. `MySQL` client'ları `MariaDB`'ye de bağlanabilir, SQL syntax'ları aynıdır, desteklediği veri tipleri aynıdır gibi.

### 📌📌 PostgreSQL (⟷ postgres)

performance açısından MariaDB ve MySQL'e göre övülen açık kaynaklı bir DB.

### 📌📌 Microsoft SQL server (⟷ MSSQL) vs Oracle DB

- genel olarak `Oracle DB` daha gelişmiş özellikler içeriyor fakat yönetimi daha zor.
- `Oracle DB`, OS bağımsız çalışırken, `MSSQL` sadece MS Windows gibi limitli birkaç OS'ta çalışabiliyor.
- `MSSQL`, `Microsoft`'un diğer ürünleri ile direk olarak destekleniyor. örneğin `Linq`'nın diğer DB'lere olan entegrasyonu daha sonradan eklendi.

### 📌📌 DB2

`IBM` tarafından geliştirilen ilişkisel DB. güncel sürümlerinde `NoSQL` tarzı özelliklerde sunmaya başlamıştır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 file based

### 📌📌 SQLite

sadece bir dosya içinde saklanabilen ilişkisel DB altyapısıdır. sunucu runtime yazılımına ihtiyaç duymaz. çünkü sadece 1 tek dosyadan ibarettir. bu dosyaya erişenler dosyayı okuyup DB'den neler olduğunu görebilir. aslında sadece bir dosya formatıdır. yazılım veya bir servis değildir.

### 📌📌 Microsoft Access

### 📌📌 LibreOffice Base

### 📌📌 comma separated values (cvs)

## 📌 embedded db

client tarafta executable'ın içerisinde de açılabilen DB'lerdir.

her file based db, embedded kategorisindedir.

embedded-db kategorisinde ek olarak şunlar var:

### 📌📌 H2

### 📌📌 Apache derby

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 strong entity vs weak entity

Bir tablo (entity) sadece kendi başına bir objeyi ifade edebiliyor (kendi başına yaşayabiliyor / anlam ifade ediyor ise) o tablo güçlüdür. tersi durumda zayıftır. örneğin; "oda" zayıf bir nesnedir. çünkü bina olmadan, oda hiçbir şey ifade etmez. fakat bina tek başına yeterlidir.

Aslında tam olarak strong mu olup olmadığını tespit etmek için resmi bir yöntem var. şu örnekten gidelim:

Customer(customerId, name, surname) --> dışarıdan tablo referans edilmesine gerek yok. kendi başına yeterli. güçlü.

Address(addressId, addressName, customerId) --> customerId ile customer verilmesi lazım. Address güçsüz bir varlık.

Adres bir müşteriye ait olmasa; güçlü olacaktı. yani; DB'ye göre gerçek hayattaki nesneler güçlü yada güçsüz olabilir. gerçek hayattan örnek vererek tespit doğru değildir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 aday anahtar (⟷ candidate key)

join yada benzeri bir sorgu sonucu dönen sonuç tablosunda; bir yada birden fazla sütun, o sonuç tablosu için `private key` oluyorsa; bu sütunlar aday anahtar demek oluyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 data domain

DB'lerde data domain; bir sütunun alabileceği değerler kümesini belirtmektedir. örneğin; tc, 10000000 ile 99999999 arasında değer alabilir. is_enabled, true yada false alabilir gibi...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 norm

Türkçe ve İngilizce'de aynı yazılır.

Türkçe'de eş anlamlısı: düzgü.

Norm'un sözlük anlamı: sosyal veya bilimsel bir olay için, yazılı olan veya olmayan standart/pattern/ölçüt/yöntemdir.

Matematikte norm, bir vektörün "büyüklüğünü" veya "uzunluğunu" ölçen bir fonksiyondur. Yani bir vektörün ne kadar "uzakta" olduğunu tanımlar. örneğin; (x,y) noktasının sıfır noktasına olan uzaklığı "L2 normu" ile hesaplanır. Matematikte birçok norm vardır.

normalization bilişimde; belli bir standarta uyarlama anlamına gelir. Örnekler:

- DB table normalization (1NF, 2NF)
- bir tablodaki/listedeki verileri standart bir sayı aralığına çevirip, son kullanıcıya gösterme (bunun örneği başka başlıkta anlatılıyor)

## 📌 Table Normalizasyon

tekrarlanmayı engellemek için best practice'lere uygun DB modeli uygulamak için yapılan işlemlerdir.

## 📌 Denormalized

1NF'in dahi karşılanmadığı DB dizaynıdır.

Aşağıdaki örnek DB'de Adres bilgileri çalıştığı ofisin adres bilgileridir. sütun ismi uzun olacağından, anlaşılır bir sütun isimi yazılmadı.

```text
||| AdresId ||| Name ||| Surname ||| Adres1_İlçe ||| Adres2_İlçe ||| Calistigi_Ofis |||
```

## 📌 1NF (⟷ 1st normal form)

normal form: model/tip anlamına gelen norm ve form kelimelerinin birleşimidir.

- You can only have one value in a column (virgüllerle aynı sütunda değer olmamalı)
 -You should not create multiple columns for a one to many relationship

```text
||| AdresId ||| Name ||| Surname ||| Adres ||| İlçe ||| Calistigi_Ofis |||
```

Adres2_İl'i eskiden dolu olan satırlar 2 satır olarak ayrı ayrı yazılmak durumunda kalacaklardır.

## 📌 2NF

Her NF, derecesinden düşük NF'leri zaten destekliyor olması gerekiyor.

- (primary key birden fazla sütundan oluşuyor olabilir. bunu gö önünde bulundurarak;) her non-primary sütun; primary sütuna bağlı değilde, sadece 1 parçasına(sütununa) bağlıysa, o parçasına bağlı olduğu sütun ile birlikte farklı bir tabloya alınmalıdır.

Yukarıdaki örnekte primary-key: Name+Surname+Calistigi_Ofis birliktedir. Adres bilgileri sadece Calistigi_Ofis'ya aittir. bu sebeple farklı tabloya taşınırlar.

```text
||| Name ||| Surname ||| Calistigi_Ofis |||

||| AdresId ||| Adres ||| İlçe ||| Calistigi_Ofis |||
```

## 📌 3NF

- her non-primary sütun direk olarak sadece primary sütuna bağlı olmalıdır.

Örnekte ilk tablo olmuş 2NF'e uymuş. ikinci tabloda 2NF'e uymuş. bu durumda 3NF'i kontrol etmek gerekmekte. ilk tablo 3NF'e uymuş. fakat ikinci tablo 3NF'e uymamış. çünkü; ilçe sütunu adresId'ye tam bağımlı değil. zaten bu sebepten tekrarlanabilir veri oluşturuyor. örneğin; ilçe 10 kere tekrar edilebilir o sütunda. ilçe farklı bir tabloya atılmalı.

```text
||| Name ||| Surname ||| Calistigi_Ofis |||

||| AdresId ||| Adres ||| İlçeId ||| Calistigi_Ofis |||

||| İlçeId ||| İlçe |||
```

## 📌 Diğer formlar

- `Boyce–Codd normal form (⟷ BCNF ⟷ 3.5NF)`

- `4NF`

Fakat en çok ilk üçünden bahsedilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Grafana connection pool metrics

### 📌📌 "Connections" panel

- Active: The connection is using by a thread.
- Idle: The connection is inside the pool and waiting for a thread to be used.
- Pending threads: The thread count which are waiting for a connection from the pool.

### 📌📌 "Connections time" panel

- Usage: When a connection is active, that means, it is being used by a thread. This metric shows how much time the connection was used by the thread.
- Creation: A connection takes time to be created. This metrics show how much it took to create them.
- Acquire: This metrics shows the time a thread needed to acquire a idle connection and move it to active connection.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 function vs stored procedure (⟷ saklı yordam)

|                       | function       | stored procedure |
|-----------------------|----------------|------------------|
| input parameter       | optionally yes | optionally yes   |
| return result         | must return    | optionally yes   |
| manipulate DB         | no (note-1)    | optionally yes   |
| read from DB (SELECT) | optionally yes | optionally yes   |
| can be transactional  | no (note-2)    | optionally yes   |

Note-1: functions can use INSERT, UPDATE features only for function local variables/tables.

Note-2: No need for transaction because it can not manipulate the real DB data.

"Functions" are designed to call them from inline SQL queries like:

```sql
SELECT column1, column1, myFunction(column3) FROM UserTable
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Unique Key

unique sütunlar null değerler alabilir.

## 📌 surrogate key vs natural key

surrogate kelime anlamı: vekil.

her tablodaki primary key bir bilgiye tekabül etmemesi (surrogate key) best practice'dir . örneğin tc (natural key) primary key olmamalıdır. bunun temel 2 sebebi:

- primary key update yapmak zordur (diğer tablolarda aynı anda update gerekir vs) (örneğin kişinin TC'si değişirse, update işlemi zor olabilir)

- primary key'in ömür boyu unique olmayacağı veya hiçbir durumda null olmayacağının garantisini vermek zordur. bu garanti bugün verilse de yarın bu karar business gereği değişebilir. fakat eğer incremental bir ID kullanılsa, business değişikliklerinde primary key'lerde update gerekmeyecek.

Bunun bir dezavantajları:

- SQL query'lerinin sürekli olarak primary key'e göre yazılmasını gerektirmektedir. bu sadece manuel yazılan query'lerde değil, programatik query'leri de yoracak. yani performance'ı olumsuz etkileyecek.

- tc örneği üzerinden gidelim. çoğu projede PK index'lenir. PK, sadece tc olsaydı, sadece TC'yi index'leyecektik. fakat surrogate key varsa onunda index'lenmesi gerekecek. bu da DB sistemine maliyeti arttıracak.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 primary key generation strategy

performans için önemli bir konu.

```java
@javax.persistence.Id // PK olduğunu JPA'ya bildirir.
@javax.persistence.GeneratedValue(strategy = GenerationType.AUTO) // GeneratedValue, bu field'ın oto-generated value olduğunu tanımlar.
Integer id;
```

GenerationType sınıfının içini incelersek:

```java
package javax.persistence;
/**
  * Defines the types of primary key generation strategies.
  *
  */
public enum GenerationType {
    /**
      * Indicates that the persistence provider must assign
      * primary keys for the entity using an underlying
      * database table to ensure uniqueness.
      */
    TABLE,
    /**
      * Indicates that the persistence provider must assign
      * primary keys for the entity using a database sequence.
      */
    SEQUENCE,
    /**
      * Indicates that the persistence provider must assign
      * primary keys for the entity using a database identity column.
      */
    IDENTITY,
    /**
      * Indicates that the persistence provider should pick an
      * appropriate strategy for the particular database. The
      * <code>AUTO</code> generation strategy may expect a database
      * resource to exist, or it may attempt to create one. A vendor
      * may provide documentation on how to create such resources
      * in the event that it does not support schema generation
      * or cannot create the schema resource at runtime.
      */
    AUTO
}
```

- AUTO

  GeneratedValue için default "strategy" değerdir.

  burada JPA kendisi, hangi GenerationType'ın kullanılacağına karar veriyor. Genelde SEQUENCE seçiliyor.

- IDENTITY

  ID değeri entity kayıt edildiğinde DB tarafından oluşturuluyor. bu sebeple aynı entity'den batch işlemlerinde performance konusunda sıkıntı yaşatıyor, çünkü bir işlem bitmeden, diğer işlemi JPA üst üste yollayamıyor. Burada dikkat: 10 adet liste tek JPA query'sinde DB ye yollanırsa sorun yok. Asıl performans sorunu üst üste farklı JPA insert query'si yapılırsa oluyor.

- SEQUENCE

  DB'de sequence kavramı var. JPA sequence'den ilgili entity'nin ID değerini çekiyor. bu IDENTITY'ye göre daha hızlı. JPA'nın bir kere DB'den sequence'i çekmesi yeterli. Çektiği zaman belli bir dilimi allocate edebiliyor. Bunu da bu şekilde set ediyoruz:

  ```java
  @Id
  @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "sequence1")
  @SequenceGenerator(name = "sequence1", allocationSize = 50)
  private Integer id;
  ```

- TABLE

  JPA'nın kendisi bir tablo oluşturuyor ve bu tabloda sequence'yi tutuyor. Performance için pek tercih edilen yöntem değil. Çünkü JPA'nın Her zaman bu tabloya query atması gerekiyor. Hangi tabloda tuttuğunu entity tarafında belirtmiyoruz. (JPA config'lerinde belirtiliyor olabilir). Yani biz yine field'ı bu şekilde tanımlıyoruz:

  ```java
  @javax.persistence.Id
  @javax.persistence.GeneratedValue(strategy = GenerationType.TABLE)
  Integer id;
  ```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 problems of NoSQL

## 📌 celebrity problem (⟷ hotspot key problem)

Bazı node'daki verilerin bir kısmının, diğer node'daki data'lara göre çok daha sık kullanılması problemidir. Örneğin, korona döneminde, "maske" çok
satışı çok olduğu için e-ticaret sitelerinde bu ürünlerin aramaları aşırı derecede artış gösterdi. Bu gibi durumlarda bu ürünleri özel node'lara taşımak ve sadece onları scale etmek gerekebilir.

## 📌 resharding data

bazen, fiziksel olarak aynı node'da duran data'ların farklı node'lara taşınması gerekebilir. bu işlem bolca kaynak tüketen bir işlemdir.

## 📌 join of de-normalized data problem

denormalize olmuş tablolardan bilgi çekmek için join yapmak gerekmektedir. bu gibi durumlarda bu data'ları direk tek tabloda bulundurmak yararlı olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 sütun index'lemesi

bir sütun index'lendiğinde artık üzerinde daha hızlı query çalıştırılabilir hale gelir. fakat her insert veya update edilişi yavaşlar çünkü index'in de update edilmesi gerekecektir.

primary key'ler otomatik olarak index'lenir.

index kullanımı sunucuda memory kullanımını da arttırabilirken, sunucu açılma süresini de yavaşlatabilir.

```sql
CREATE INDEX my_index_name -- Creates "secondary index"
ON Persons (LastName)
```

index'i kaldırma işlemi:

```sql
DROP INDEX index_name ON table_name
```

## 📌 primary index vs secondary index

primary index'ler sadece unique olan sütunlara atanabilir. oysa secondary index'ler unique olmayan sütunlara da atanabilir.

## 📌 clustered index vs non-clustered index

sıralı (clustered) olması istenen sütunlar clustered-index olması daha uygundur. ilgili tabloyu fiziksel olarak clustered-index yapılan sütuna göre sort'lar. dolayısı ile en fazla 1 adet clustered index'imizi olabilir.

primary key + incremental + integer olan sütunlar otomatik olarak "clustered index" olarak ayarlanabilir.

non-clustered index'ler sırasız şekilde sütunu ayrı bir fiziksel bellekte tutar.

## 📌 index strategy

index'lerin fiziksel olarak tutulduğu data-structure'ye denir. hangi strategy'nin kullanılacağını index oluştururken belirtebiliyoruz. eğer belirtmezsek default olan kullanılır. en sık kullanılan strategy'ler:

- __B-Tree__ (binary tree'den farklıdır)

  Veriler şu mantıkta tutulur:

  ```text
      [20, 50]
      /    |     \
  <20   20–50   >50
  ```

  B-tree dengelidir (flat). Bu sebeple aramalar hızlı olması sağlanır. Fakat ağacın dengesinin sağlanması gereklidir her insert sonrası, bu da ekleme/silmelerde yavaşlık getirir.

  String'ler de B-tree'de index'lenebilir (alfabetik sıra yapılır).

  Fakat JSON, Array gibi data tutan sütunlar B-tree'de index'lenemezler.

  Eşitlik (=), aralık (<, >, BETWEEN) sorguları için çok uygundur.

- __Hash Index__

  Sadece eşitlik sorguları (=) için hızlıdır, aralık sorguları desteklenmez.

  Data şu şekilde tutulur:

  - hash_Of( a ) mod 10 = 0
  - hash_Of( b ) mod 10 = 1
  - hash_Of( c ) mod 10 = 1
  - hash_Of( x ) mod 10 = 2
  - ...

  olduğunu varsayarsak; index şöyle tutulur:

  ```text
  Hash Bucket 0: [a]
  Hash Bucket 1: [b, c]
  Hash Bucket 2: [x, y, z]
  ```

  Not: Hash code'u trim'leyerek arama gibi çözümler var. "string yerine string'in hashcode ile arama" başlığına bakılabilir.

- __Full-Text__

  "Full-Text search" yapabilmek için gerekli index'lemedir. ("Full-Text search" başka yerde anlatılmıştır)

- __Bitmap__

  Düşük kardinaliteli sütunlar'da kullanılmalıdır. örneğin: Cinsiyet = Erkek/Kadın, Durum = Aktif/Pasif gibi.

  Sadece eşitlik sorguları (=) için hızlıdır, aralık sorguları desteklenmez.

  Data şöyle tutulur:

  DB Tablomuz bu olsun:

  ```text
  Satır   Cinsiyet
  1       Erkek
  2       Kadın
  3       Erkek
  4       Erkek
  5       Kadın
  6       Kadın
  ```

  Index şu şekilde tutulur:

  ```text
  Erkek için bit dizisi:
  Satır: 1 2 3 4 5 6
  Erkek: 1 0 1 1 0 0

  Kadın için bit dizisi:
  Satır: 1 2 3 4 5 6
  Kadın: 0 1 0 0 1 1
  ```

  Böylece artık "__WHERE Cinsiyet = Erkek__" sorgusu çalıştırıldığında, sadece __erkek-bit-dizisi__'ne bakılması yetecektir.

## 📌 cardinality (⟷ πληθάριθμος ⟷ πληθικός αριθμός)

Sözlükte Türkçe karşılığını bulamadım. Fakat piyasada "kardinalite" olarak kullanılıyor.

kümenin eleman sayısıdır. (kümede aynı eleman olmadığını unutmayalım).

DB dünyasında, bir sütunun cardinality'sinin yüksek veya az olduğundan bahsedilir. tekrarlanan veri varsa cardinality düşüktür. tekrarlanan veri az ise cardinality yüksektir. örnek olarak "cinsiyet" sütununun kardinalitesi düşüktür.

DB'de index alırken çok düşük kardinalitesi olan veya sütunun toplam elemen sayısı az olan tabloların index'inin alınması önerilmez.

## 📌 unique column

unique sütunlar index'lenebilir fakat bir sütunun unique olduğunu DB'ye belirtmek ona atılan bazı sorguları aşırı hızlandırır. çünkü unique olmayan bir sütunda arama yapıldığında, birden fazla satır dönebileceği için tüm tablo aranmak zorundadır. Oysa unique bir sütunda arama yapıldığında, ilk sonuç bulunduğunda query sonlandırılır. çünkü devam etmeye gerek yoktur.

## 📌 unique column with index vs non-unique column with unique index

DB'lerde "UNIQUE INDEX" oluşturabiliriz veya bir sütunu UNIQUE yapıp ona index atayabiliriz. Aralarında çok çok ince farklılıklar var. kaynak: https://docs.microsoft.com/en-us/sql/relational-databases/indexes/create-unique-indexes?view=sql-server-ver15 1st paragraph, 3th sentence.

## 📌 composite index

```sql
CREATE INDEX my_index ON users (username, age);
```

query'lerimizin where koşulunda sıkça username ve age sütunlarını görüyorsak o zaman bunları composite index yapmalıyız. Composite-index'ler şu şekilde tutulurlar:

| composite-index-key | primary-key-of-table |
|---------------------|----------------------|
| ahmet-30            | 99                   |
| ahmet-40            | 10                   |
| ahmet-50            | 26                   |
| cemal-20            | 190                  |
| cemal-60            | 100                  |
| zeynep-20           | 45                   |

Artık hem username hem de age sütununu where koşulunda bulunduğumuz sorgular hızlanacaktır.

Ek olarak where koşulunda sadece username olan sorgular da hızlanacaktır. Çünkü zaten username, composite-index-key'in ilk sırasında. Oysa sadece "age" olan bir where koşulu composite-key'imizden hiç verim alamayacaktır. Çünkü composite-index-key'in ikinci alanında arama yapmak ile gerçek tablonun tümünde aramanın hiçbir farkı yoktur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 SQL sorguları yaptıkları işlere göre gruplandırılırlar

DDL (Data Definition Language) --> şemalar üzerinde işlem yapan operatörler: alter, create table gibi.

DQL (data query language) --> SELECT, FROM, WHERE gibi.

DML (Data Manipulation Language) --> satırlar üzerinde işlem yapmaya yarayan operatörler: INSERT, UPDATE gibi.

DCL (Data Control Language) --> user'ların yetkilerini ayarlamak için kullanılan SQL operatörleri. grant, revoke gibi.

TCL (Transaction Control Language) --> Transaction işlemlerinde kullanılan SQL operatörleridir: COMMIT, ROLLBACK gibi.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 parsing

DB sunucusu, SQL sorgularını parse edip optimize eder ve execute eder. Bu işleme "__hard parsing__" denir. Oysa aynı sorgunun tekrar tüm bu işlemlere girmesine gerek yok. DB sunucusu, her SQL'in optimize edilmiş halini, bellekte tutar. Eğer aynısı gelirse bu sefer "__soft parsing__" yapar ve execute eder.

`Oracle DB`'de büyük küçük harf dahi (örneğin; Select, SELECT) sorgunun farklı olmasına sebep olmaktadır. kaynak: https://blogs.oracle.com/sql/improve-sql-query-performance-by-using-bind-variables "Why do bind variables matter for performance?" başlığı.

Buna çözüm olarak SQL'lerin elle yazılmaması önerilir. Bu şekilde case-sensitivity sorunu ortadan kalkar. Aynı zamanda 'statement'lara dynamic variable'ları bind ederek'te aynı SQL'in farklı değerlerle çalıştırıldığında, hard parsing yapılmamasını sağlamış oluruz. Çünkü; eğer binding yapmazsak; SQL aynı fakat içindeki herhangi bir integer farklı ise bile hard parsing yapılır. kaynak: https://blogs.oracle.com/sql/improve-sql-query-performance-by-using-bind-variables "Where do bind variables come into this?" başlığı.

Hard parsing ile soft parsing arasında performance olarak büyük fark var. bu kaynakta; https://blogs.oracle.com/sql/improve-sql-query-performance-by-using-bind-variables "What's the impact of hard parsing?" başlığındaki örnekte, `for loop` içerisinde aynı sorgu bind edilerek ve bind edilmeden yollanmış. Bir döngü 4 saniyede tamamlanırken, diğeri 1 buçuk dakikada tamamlanmış.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 TNS (⟷ Transparent Network Substrate)

`TNS`, `Oracle DB` client ile sunucusu arasındaki protocol'ün ismi.

`TNS Listener`: Bağlantı taleplerini dinler ve yönlendirir. Bağımsız bir servistir. Load balancer tarzı görevler üstlenir.

`TNS Service`: `Oracle DB`'yi temsil eder.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 MongoDB

`MongoDB`'de veri yapıları ve ona denk gelen `RDBMS` karşılıkları aşağıdaki gibidir:

- `collection`, `tablo`'a denk geliyor.
- `document`, `satır`'a denk geliyor.

İlişkiler `MondoDB`'de tutulmamaktadır. fakat bu ilişkiler yazılımsal olarak sağlanabilir.

`MongoDB` de veriler `JSON` formatında tutuluyor. `JSON` formatı dışında bir tipte değer tutmak gerekirse onu string'e cast edip tutabiliriz. yada DB dışında bir yerde tutup, onun `URL`'sini `JSON` `string`'i olarak saklayabiliriz.

`NoSQL`'de standart `SQL` query'ler olmayabilir. örneğin MongoDB'de şu şekilde sorular yazılır:

`db.users.find( { status: "A", age: { $lt: 30 } } )` gibi sorgu ile sadece tek tablo üzerinde sorgu yapılabilir. `$lt` değeri aldığı integer değerden daha küçük tüm satırları temsil etmek için kullanılır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 SQL (⟷ Structured Query Language)

`sekuel` diye de okunur (eski ismine benzerliği sebebi ile).

`SQL` bir çeşit `DSL`'dir.

tüm ilişkisel DB'leri için ortak dildir. Tüm sistemlerde çalışabileceği için az özellik içerir.

`ANSI` ve `ISO` kurumları __International Electrotechnical Commission (⟷ IEC)__'a bağlıdır.

Both `ANSI` and `ISO` and `IEC` have accepted `SQL` as the standard language for `RDBMS`s. When a new `SQL` standard is simultaneously published by these organizations, the names of the standards conform to conventions used by the organization, but the standards are technically identical.

## 📌 Standard SQL version history

| Year | Name     | Alias            | Comments                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|------|----------|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1986 | SQL-86   | SQL-87           | First formalized by ANSI                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| 1989 | SQL-89   | FIPS 127-1       | Minor revision that added integrity constraints, adopted as FIPS 127-1                                                                                                                                                                                                                                                                                                                                                                                               |
| 1992 | SQL-92   | SQL2, FIPS 127-2 | Major revision (ISO 9075), Entry Level SQL-92 adopted as FIPS 127-2                                                                                                                                                                                                                                                                                                                                                                                                  |
| 1999 | SQL:1999 | SQL3             | Added regular expression matching, recursive queries (e.g. transitive closure), triggers, support for procedural and control-of-flow statements, nonscalar types (arrays), and some object-oriented features (e.g. structured types), support for embedding SQL in Java (SQL/OLB) and vice versa (SQL/JRT)                                                                                                                                                           |
| 2003 | SQL:2003 |                  | Introduced XML-related features (SQL/XML), window functions, standardized sequences, and columns with autogenerated values (including identity columns)                                                                                                                                                                                                                                                                                                              |
| 2006 | SQL:2006 |                  | ISO/IEC 9075-14:2006 defines ways that SQL can be used with XML. It defines ways of importing and storing XML data in an SQL database, manipulating it within the database, and publishing both XML and conventional SQL-data in XML form. In addition, it lets applications integrate queries into their SQL code with XQuery, the XML Query Language published by the World Wide Web Consortium (W3C), to concurrently access ordinary SQL-data and XML documents. |
| 2008 | SQL:2008 |                  | Legalizes ORDER BY outside cursor definitions. Adds INSTEAD OF triggers, TRUNCATE statement, FETCH clause                                                                                                                                                                                                                                                                                                                                                            |
| 2011 | SQL:2011 |                  | Adds temporal data (PERIOD FOR) (more information at: Temporal database#History). Enhancements for window functions and FETCH clause.                                                                                                                                                                                                                                                                                                                                |
| 2016 | SQL:2016 |                  | Adds row pattern matching, polymorphic table functions, JSON                                                                                                                                                                                                                                                                                                                                                                                                         |
| 2019 | SQL:2019 |                  | Adds Part 15, multidimensional arrays (MDarray type and operators)                                                                                                                                                                                                                                                                                                                                                                                                   |

## 📌 PL/SQL (⟷ Procedural Language/SQL)

`Oracle DB`'ın `SQL` türevi dili.

## 📌 PL/pgSQL

`PostgreSQL`'ın `SQL` türevi dili.

## 📌 T-SQL (⟷ Transact-SQL)

`MSSQL`'ın `SQL` türevi dili.

## 📌 PL/SQL vs T-SQL

"Migration from `Oracle DB` to `MSSQL`" gibi terimlerle aratıldığında çıkan dökümanlarda data tiplerinin karşılaştırılması bulunabilir.

Bu konu için güzel bir kaynak: `http://www.bristle.com/Tips/SQL.htm title: "Differences Between Oracle and MS SQL Server".`

## 📌 Other extensions

Makalelerin çoğunda, PL/SQL, T-SQL gibi extension'ların "prosedürel" olduğu yazar. Bunun sebebi şu: Standard SQL daha çok data çekmek için tek satırlık işlemlere odaklanmış bir standart. Oysa PL/SQL, T-SQL ve diğer extension'lar transaction, fonksiyon, batch gibi bloklardan oluşan (yani prosedürel programlama metodolojisine uygun) SQL'lere daha çok önem veren özellikler içermesidir.

| Source               | Abbreviation    | Full name                                                                                 |
|----------------------|-----------------|-------------------------------------------------------------------------------------------|
| ANSI/ISO Standard    | SQL/PSM         | SQL/Persistent Stored Modules                                                             |
| Interbase / Firebird | PSQL            | Procedural SQL                                                                            |
| IBM DB2              | SQL PL          | SQL Procedural Language (implements SQL/PSM)                                              |
| IBM Informix         | SPL             | Stored Procedural Language                                                                |
| IBM Netezza          | NZPLSQL         | (based on Postgres PL/pgSQL)                                                              |
| Invantive            | PSQL            | Invantive Procedural SQL (implements SQL/PSM and PL/SQL)                                  |
| MariaDB              | SQL/PSM, PL/SQL | SQL/Persistent Stored Module (implements SQL/PSM), Procedural Language/SQL (based on Ada) |
| Microsoft / Sybase   | T-SQL           | Transact-SQL                                                                              |
| Mimer SQL            | SQL/PSM         | SQL/Persistent Stored Module (implements SQL/PSM)                                         |
| MySQL                | SQL/PSM         | SQL/Persistent Stored Module (implements SQL/PSM)                                         |
| MonetDB              | SQL/PSM         | SQL/Persistent Stored Module (implements SQL/PSM)                                         |
| NuoDB                | SSP             | Starkey Stored Procedures                                                                 |
| `Oracle DB`               | PL/SQL          | Procedural Language/SQL (based on Ada)                                                    |
| PostgreSQL           | PL/pgSQL        | Procedural Language/PostgreSQL Structured Query Language (based on reduced PL/SQL)        |
| SAP R/3              | ABAP            | Advanced Business Application Programming                                                 |
| SAP HANA             | SQLScript       | SQLScript                                                                                 |
| Sybase               | Watcom-SQL      | SQL Anywhere Watcom-SQL Dialect                                                           |
| Teradata             | SPL             | Stored Procedural Language                                                                |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 statement

SQL dilindeki her komut (select, update, create, commit ...) ve o komuta yollanan parametreler birlikte 1 statement'tır. Her statement noktalı virgül ile ayrılmaktadır. Fakat bazı platformlar noktalı virgül koyulmasını zorunlu kılmaz. yeni satıra geçmek yeterlidir.

1 transaction, sadece 1 statement'ten da oluşuyor olabilir.

sadece 1 statement yolladığımızda, arka planda bizim için 1 transaction oluşturuluyor. kaynak: https://docs.microsoft.com/en-us/sql/t-sql/language-elements/transactions-transact-sql?view=sql-server-ver15 "Autocommit transactions" başlığı altında " Each individual statement is a transaction." yazıyor.

## 📌 transaction

Türkçe'de kelime anlamı: işlem

multiple statement olmak zorunda değil. tüm SQL işlemleri birer transaction'dır ve DB Admin GUI'lere bakarsak "transaction log" (history)'de görürüz.

## 📌 batch (⟷ script)

multiple statement'lar kümesidir. batch, özellikle belirtilmediği sürece "transactional" (atomik) değildir.

## 📌 batch on Hibernate

spring-batch modülü ile karıştırılmamalıdır. tamamen farklı kavramlardır.

Hibernate ile Java uygulamasından attığımız SQL'ler normal koşullarda tek tek `TCP` bağlantısı üzerinden DB sunucusuna iletilmektedir. Fakat Hibernate, bunları gruplandırarak batch halinde atmamızı da sağlamaktadır.

Hibernate ayarlarında Batch=1 (hibernate.jdbc.batch_size=1) ise aşağıdaki for döngüsü, hemen aşağısındaki çıktıyı vermektedir.

```java
for (int i = 0; i < 10; i++) {
    School school = createSchool(i);
    entityManager.persist(school);
}
entityManager.flush();
```

```sql
-- pseudo SQL

INSERT INTO SCHOOL VALUES 1
INSERT INTO SCHOOL VALUES 2
-- others here
INSERT INTO SCHOOL VALUES 10
```

Batch=5 olsaydı aynı for döngüsü aşağıdaki gibi çıktı üretirdi:

```sql
-- pseudo SQL

INSERT INTO SCHOOL VALUES 1, 2, 3, 4, 5 -- same insert statement (this is only 1 insert statement)
INSERT INTO SCHOOL VALUES 6, 7, 8, 9, 10
```

Hibernate'te "batch" sayısını arttırdığımızda, "hibernate.order_inserts" vs "hibernate.order_update" config'lerine de dikkatli kullanmak gerekiyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
