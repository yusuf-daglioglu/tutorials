# DB LOCK AND ISOLATION

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

Aşağıdaki dökümandaki kısaltmalar:

- TX = Transaction
- TX1 = Transaction No 1 (runs same time with TX2)
- TX2 = Transaction No 2

## 📌 Isolation'ın olmaması yada olması durumunda çıkabilecek problemler (transactional anomaly)

Aşağıdaki 3 madde "__ANSI/ISO SQL__" standartında "ISO/IEC 9075:1992" dökümanında ortaya atılmıştır. Piyasada "__Read phenomena__" olarak adlandırılırlar. Çünkü bu 3'ü sadece okunurkenki anormallikleri ele almış.

phenomena kelime anlamı: olay.

- `Dirty Read` (`P1`)

  Başka bir TX'nin henüz commit etmediği data'ların okunması. Buna "__read uncommitted__" veya "__uncommitted dependency__" de denir.

- `Non-Repeatable Read (⟷ Fuzzy Read)` (`P2`)

  - TX1, D1 satırını okudu.
  - TX2, D1'i update etti.
  - TX1 tekrar D1'i okuduğunda farklı data okuyacaktır.

- `Phantom Read` (`P3`)

  phantom kelime anlamı: hayalet.
  
  - TX1, TABLE1'i komple okudu.
  - TX2 TABLE1'e yeni satır ekledi.
  - TX1 tekrar TABLE1'i çektiği zaman hayalet satırlar gelecektir.

ANSI'de olmayan fakat piyasada sıkça kullanılan phenomena:

- `Lost Update`

  Bu "__Read phenomena__" değildir. Çünkü okurken olan bir durum değil.

  TX1 ve TX2 aynı satırlarda işlem yaparsa, erken biten TX'nin data'ları kaybolmuş olur.

## 📌 Standart karmaşası

İzolasyon seviyeleri ve "Read phenomena" kavramları, "ANSI ve ISO SQL" de belirlenmiş olsa da bunlar çok net ve her durumu kapsayacak şekilde tanımlanmamıştır. Burada bununla ilgili bir makale yayınlamıştır: <https://www.microsoft.com/en-us/research/publication/a-critique-of-ansi-sql-isolation-levels/>, Makale buraya yönlenmektedir: <https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/tr-95-51.pdf>

Bu projede: <https://github.com/ept/hermitage> bu durum açıklanmış ve ek olarak DB'leri arasındaki farklar özel bir tabloda toplanmıştır.

<https://github.com/ept/hermitage> projesinde 'Read phenomena'ların sayısı, 'ANSI ve ISO SQL'de yetersiz olduğu için arttırılmış. Yeni makaledeki bazı yeni read phenomena'lar:

- G0: Write Cycles (dirty writes)
- G1a: Aborted Reads (dirty reads, cascaded aborts)
- G1b: Intermediate Reads (dirty reads)
- P4: Lost Update

Buna rağmen projede şu uyarı düşülmüş:

```text
The summary table above only describes safety properties, i.e. whether the DB allows a certain race condition to occur. It doesn't describe how the anomaly is prevented (usually by blocking or aborting some of the transactions). In practice, how much transactions need to be blocked or aborted makes a big performance difference. For example, although PostgreSQL's serializable and MySQL's serializable have the same isolation guarantees, they have totally different implementations and very different performance characteristics.
```

Fakat tablodaki MySQL ve PostgreSQL için ayrılan "serializable" izolasyon satırındaki tüm bilgiler aynı. Buna rağmen farklılıklar göstermektedirler.

## 📌 isolation seviyeleri (⟷ isolation levels)

ANSI ve ISO SQL'de 4 seviye var. Fakat her DB farklı seviyeler sunabilir. Örneğin; MSSQL, ANSI ve ISO SQL'e ek olarak "SNAPSHOT" isimli bir izolasyon seviyesi daha sunmaktadır.

Burada sade bir anlatım mevcut: (source-id: 111)

JPA'da aşağıdaki gibi her TX işlemi için izolasyon seviyesi verebiliriz. Eğer hiç izolasyon seviyesi kullanmazsak, JPA default olarak DB'in izolasyon seviyesini kullanmaktadır.

```java
@Transactional(isolation = Isolation.REPEATABLE_READ)
```

## 📌 İzolasyon seviyesi hangi transaction'ı etkiler
TX1'in isolation seviyesi şunları belirler:

- TX1, kendi kullandığı satırlara TX2 erişmesin diye lock atıp atmayacağı.

- TX1, TX2'nin lock'lı satırlarına erişecek mi? 

  Bu madde ANSI'de yok. Yani ANSI'ye göre, isolation seviyesi ne olursa olsun, bir satır üzerinde lock varsa, diğer TX oraya erişemez. Ama pratikte, bazı DB'ler izin veriyor. Bunun bir örneği bu dökümanda da var.

- TX1, TX2'nin uncommited satırlarına (lock olmadığı sürece) erişecek mi?

## 📌 İzolasyon Seviyelerinin engellediği phenomena'lar (olaylar)

| isolation seviyesi ve adı           | Dirty Read | Non-repeatable Read | Lost Update | Phantom Read |
|-------------------------------------|------------|---------------------|-------------|--------------|
| 0 (Read Uncommitted)                | Olur       | Olur                | Olur        | Olur         |
| 1 (Read committed) (mostly default) | Olmaz      | Olur                | Olur        | Olur         |
| 2 (Repeatable Read)                 | Olmaz      | Olmaz               | Olmaz       | Olur         |
| 3 (Serializable)                    | Olmaz      | Olmaz               | Olmaz       | Olmaz        |

## 📌 read lock vs write lock

### 📌📌 read lock (⟷ shared lock)

eğer bir satır (yada tablo) "read lock" olarak flag'lendiyse, o satır okunabilir fakat yazılamaz(update edilemez).

"shared" denmesinin sebebi: birden fazla TX aynı zaman diliminde, aynı satırlara read-lock koyabilmesidir.

### 📌📌 write lock (⟷ exclusive lock)

exclusive kelime anlamı: herkese açık olmayan. ayrıcalıklı.

eğer bir satır (yada tablo) "write lock" olarak flag'lendiyse, o satır okunamaz veya yazılamaz(update edilemez).

"exclusive" denmesinin sebebi: birden fazla TX aynı zaman diliminde, aynı satırlara write-lock koyamamasıdır.

## 📌 isolation levels

### 📌📌 TX1, TX2'deki data'yı okurken

| level of TX1 | TX2'deki uncommited data'yı | TX2'deki read-lock'lı data'yı | TX2'deki write-lock'lı data'yı |
|--------------|-----------------------------|-------------------------------|--------------------------------|
| 0            | okur                        | okur                          | okumaz (Note-1)                |
| 1            | okumaz                      | okur, yazmaz                  | okumaz, yazmaz                 |
| 2            | okumaz                      | okur, yazmaz                  | okumaz, yazmaz                 |
| 3            | okumaz                      | okur, yazmaz                  | okumaz, yazmaz                 |

"Okumaz" veya "yazmaz" durumlarında, diğer TX'ın lock'ı serbest bırakmasını bekler.

Yukarıdaki tabloya göre read-uncommited sadece şu durumda oluyor: TX1 ve TX2 ikisi de level-0'day ise.

- Note 1:

  Bu case'de sadece MSSQL kendi içinde istisna yapmış ve okumasına izin vermiş. `(source-id: 112) "Arguments" başlığı, "READ UNCOMMITTED" altbaşlığı, ikinci paragraf, ikinci cümle.`

### 📌📌 Diğer transaction'ların data okumasını engellemek için kendi okuduğu data'lara lock atar

| Level | okudugu data'larda yaptığı lock                   | yazdığı data'larda yaptığı lock               |
|-------|---------------------------------------------------|-----------------------------------------------|
| 0     | yok                                               | yok                                           |
| 1     | satır bazlı read-lock atar (statement bazlı) (K1) | satır bazlı write-lock atar (TX sonuna kadar) |
| 2     | satır bazlı read-lock atar (TX sonuna kadar)      | satır bazlı write-lock atar (TX sonuna kadar) |
| 3     | range bazlı read-lock atar (TX sonuna kadar) (K2) | range bazlı write-lock atar (TX sonuna kadar) |

Kaynaklar:

- K1: (source-id: 112) "Arguments" başlığı, "REPEATABLE READ" altbaşlığı, ikinci paragraf.

- K2:

  - (source-id: 112) "Arguments" başlığı, "SERIALIZABLE" altbaşlığı, 2'inci paragraf ilk cümle.
  - (source-id: 112) "Arguments" başlığı, "SERIALIZABLE" altbaşlığı, 3üncü madde ve 2inci paragraf 2inci madde.

### 📌📌 range lock örnekleri

Örnek 1:

```sql
SELECT * FROM Order
```

|                  | select anında dönen 10 satırı başka TX | başka TX order'a yeni kayıt |
|------------------|----------------------------------------|-----------------------------|
| range lock varsa | update edemez                          | edemez                      |
| row lock varsa   | update edemez                          | ekler                       |

Örnek 2:

```sql
SELECT * FROM Order WHERE status = "DISABLED"
```

|                  | select anında dönen 10 satırı başka TX | başka TX order'a yeni kayıt |
|------------------|----------------------------------------|-----------------------------|
| range lock varsa | update edemez                          | "DISABLED" olarak edemez    |
| row lock varsa   | update edemez                          | ekler                       |

### 📌📌 Snapshot isolation

- ANSI/ISO SQL'de yazılmamıştır. Fakat bazı DB'ler bunu sunar.
- TX başladığı anda snapshot alınır. İlgili TX sürekli bu snapshot üzerinde bağımsızca çalışır.
- lock'lama ihtiyacı yoktur. bu sebeple hızlıdır.
- pratikte, en sonda hata alması yüksektir. çünkü hiç lock atmadı. bu sebeple diğer TX'lara lock atmasını sağlayan opsiyonel ek ayar sunulur.

Terminoloji karmaşası: Bazı DB'ler "serializable" izolasyon seviyesini "snapshot" seviyesi olarak kullanır. Bazı makalelerde ise "Serializable Snapshot Isolation" olarak adlandırılır.

## 📌 Lock of Tables

Tüm izolasyon seviyelerine bakıldığında tabloların tümüyle çok nadiren lock'landığını görebiliriz. Fakat buna rağmen pratikte, update veya delete işlemlerinde tabloların bazen lock'lanabilir. Bunun 2 sebebi var:

1- DB sunucuları kendi içlerinde data'ları basit yöntemlerle ve tek 1 dosyada tutmazlar. Biz ufak bir data'yı update ettiğimizde, bazen internal olarak bu fiziksel dosyaları farklı yerlere taşıyabilir, parçalayabilir, birleştirebilir veya içerisinde büyük bir işleme tabi tutabilir. Bunu kendisi optimizasyon için veya farklı sebeplerden yapıyor olabilir. Dolayısı ile bu durumda değişiklik yaptığımız tablo lock'lanıyor olabilir. Bizde bunu izolasyon seviyesinden kaynaklı sanıyor olabiliriz.

2- Genelde range-lock'lar her durumda developer tarafından yanlış algılanabiliyor. Örneğin şu denemeyi `MSSQL`'da kendim birçok kere denemiştim ve bu şekilde sonuç verdiğini görmüştüm:

```sql
DELETE FROM USER_TABLE
WHERE ID BETWEEN 1000 AND 9999
```

Yukarıdaki TX çalışırken, aşağıdaki TX cevap vermiyordu (yukarıdakinin bitmesini bekliyordu):

```sql
SELECT * FROM USER
```

Fakat aşağıdaki paralel çalıştığında cevap veriyordu:

```sql
SELECT * FROM USER
WHERE ID BETWEEN 1 AND 9
```

Nedenini muhtemelen cevap vermeyen SQL'in tüm tablo üzerinden işlem yapmaya çalışmasıdır. Cevap veren SQL ise ID ile spesifik gittiği için range lock'a takılmıyor. Detaylı düşünüldüğünde okuyan ve yazanın ne işlemler yaptığı önemli.

## 📌 Manuel lock table

Tüm tabloyu veya bir view'ı manuel lock'layabiliriz:

```sql
LOCK TABLES table_name READ
```

Yukarıdaki SQL'de;

- READ diğer TX'lerin tablodan okuyabilmesini fakat yazamamalarını sağlar.
- READ yerine WRITE kullanırsak; ilgili tablo okumaya dahi kapatılır. Diğer okuyacak TX'ler kuyrukta bekletilir (exception almazlar).

## 📌 locking mechanism (locking types)

## 📌 optimistic vs pessimistic
  
pessimistic kelime anlamı: karamsar.

|             | support level            | how it works                                 | works                                          |
|-------------|--------------------------|----------------------------------------------|------------------------------------------------|
| optimistic  | app (manually or by ORM) | Custom version ⟷ timestamp column           | works only if other TX read our uncommited row |
| pessimistic | db must support          | Lock via SQL: SELECT * FROM USERS FOR UPDATE | with or without isolation level                |

| optimistic via JPA                                      | okuduğu satırın versiyonunu                          |
|---------------------------------------------------------|------------------------------------------------------|
| __OPTIMISTIC__ (or same Enum __READ__)                  | aynı bırakır                                         |
| __OPTIMISTIC_FORCE_INCREMENT__ (or same Enum __WRITE__) | arttırır. böylece başkaları tekrar okumadan yazamaz. |

| pessimistic via JPA             | okuduğu satırı başkaları | yazdığı satırları başkaları |
|---------------------------------|--------------------------|-----------------------------|
| __PESSIMISTIC_READ__            | okur, yazamaz            | okuyamaz, yazamaz           |
| __PESSIMISTIC_WRITE__           | okuyamaz, yazamaz        | okuyamaz, yazamaz           |
| __PESSIMISTIC_FORCE_INCREMENT__ | okur, yazamaz            | okuyamaz, yazamaz           |

__PESSIMISTIC_READ__ ve __PESSIMISTIC_FORCE_INCREMENT__ kullanan açısından aynı. Sadece __PESSIMISTIC_FORCE_INCREMENT__ arka planda ek olarak "version" isimli sutunu okuduğu her satır için o anda arttırıyor. Yani arka planda çift koruma birden çalıştırıyor.

"okuyamaz" veya "yazamaz" durumunda API'den direk hata fırlatılır. Oysa Isolation seviyesinin lock'larında API beklemeye giriyordu.

## 📌 semantic lock

semantic kelime anlamı: anlamsal

DB seviyesinde değil, uygulama seviyesinde manuel yönetilir.

Özel bir "status" isimli sutun açarız ve business ile bu sutuna bakıp manuel lock'larız.

## 📌 usage in JPA

```java
// entityManager Find
entityManager.find(Student.class, studentId, LockModeType.OPTIMISTIC); // LockModeType has also Pessimistic Enums.

// Query
Query query = entityManager.createQuery("from Student where id = :id");
query.setParameter("id", studentId);
query.setLockMode(LockModeType.OPTIMISTIC_INCREMENT);
query.getResultList();

// explicit Locking
Student student = entityManager.find(Student.class, id);
entityManager.lock(student, LockModeType.OPTIMISTIC);

// refresh (updates the existing entity)
Student student = entityManager.find(Student.class, id);
entityManager.refresh(student, LockModeType.READ);

// NamedQuery
@NamedQuery(name="optimisticLock",
  query="SELECT s FROM Student s WHERE s.id LIKE :id",
  lockMode = WRITE)

// lock scope
Map<String, Object> properties = new HashMap<>();
// properties to lock relations like @JoinTable, @OneToOne.
map.put("javax.persistence.lock.scope", PessimisticLockScope.EXTENDED); // PessimisticLockScope.NORMAL: does not lock relations.
// optional:
map.put("javax.persistence.lock.timeout", 1000L);

entityManager.find(Student.class, 1L, LockModeType.PESSIMISTIC_WRITE, properties);
```
