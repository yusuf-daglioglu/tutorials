# DB LOCK WITH REDIS OR JAVA

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Distributed lock

- Bağımsız microservice'ler bir data'yı lock ediyor ise, `Distributed lock` yapmış oluruz.
- Lock metod'ları, lock ettiğimiz data'nın ne olduğu ile ilgilenmez.
- `Distributed transaction` ile `Distributed lock` farklı kavramlardır.

## 📌 lock'a sahip client'ın çökme durumu

Lock'layan application veya thread aniden çöker ise, lock olan data'yı evict edemeyeceği 2 farklı çözüm uygularız:

- timeout

  timeout süresi atanır. timeout sonrası her daim lock merkezi sunucu tarafından kaldırılır.

- watchdog

  lock yapan application'da watchdog olur. bu bağımsız bir thread'dir. lock eden thread ayakta oldukça lock süresini uzatır. Thread çökerse lock'ı serbest bırakır. Ama program tümüyle çökerse bu çözüm bir işe yaramaz.

### 📌📌 Sınıflar

Java, Standart olması açısından Lock için bir interface ortaya atmıştır: "__java.util.concurrent.locks.Lock__". Örnek implementasyonlar:

- Redisson'ın sunduğu __org.redisson.api.RLock__.
- Java içinde __java.util.concurrent.locks.ReentrantLock__ gelir.

### 📌📌 Sadece lock'a sahip thread, o lock'ı kaldırabilir

Bazı implementasyonlar (Java standartı olan "__java.util.concurrent.locks.Lock__" bunu zorunlu tutmuyor); lock'a sahip olan (acquire eden) Java thread'i dışında bir thread, lock'ı serbest bırakmaya kalkarsa, IllegalStateException gibi bir hata alır.

Eğer bunu istemiyorsak "__java.util.concurrent.Semaphore__" kullanılmalıdır.

### 📌📌 Redisson Lock implementasyonları

Redisson'un lock için sunduğu implementasyonların bazılarına bakalım.

Aşağıdaki lock mekanizmalarının desteklenmesi için Redis (sunucu) tarafında birçok metadata'nın tutulması gerekebilir.

- __Lock__

Klasik lock işlemi yapar.

```java
// redissonClient1 tüm lock'ların listesini (instance'larını) tutuyor. bu sebeple; getLock metodu ile aynı instance'ı istediğimiz yerden çağırabiliriz.
RLock lock = redissonClient1.getLock("myLock");

lock.lock();
// or acquire lock and automatically unlock it after 10 seconds
lock.lock(10, TimeUnit.SECONDS);

// or wait for lock acquisition up to 100 seconds
// and automatically unlock it after 10 seconds
boolean lockResult = lock.tryLock(100, 10, TimeUnit.SECONDS);

// do anything you want
if (lockResult) {
  try {
    // do anything here. Redis does not care about what writes here..
  } finally {
    lock.unlock();
  }
}
```

- __Fair Lock__

```java
RLock lock = redissonClient1.getFairLock("myLock");
```

"Redisson" (connection) instance'ındaki tüm thread'ler hangi sıra ile lock isteği yolluyorsa, o sıra ile lock'ları acquire edebilecekleri şekilde ayarlanmış lock implementasyonudur. aynı thread'in içindeki lock sırası değil, tüm thread'lerdeki lock sıralaması göz önünde bulundurulur.

- __MultiLock__

birden fazla lock'ı grup içine alarak tek bir lock nesnesi üzerinden işlemlere devam edebiliriz.

```java
RLock lock1 = redissonClient1.getLock("lock1");
RLock lock2 = redissonClient2.getLock("lock2");
RLock lock3 = redissonClient3.getLock("lock3");

RLock multiLock = redissonClient1.getMultiLock(lock1, lock2, lock3);
```

- __RedLock__

Redis'in ortaya attığı bir algoritmayı baz alarak çalışır.

- __ReadWriteLock__

Lock mekanizmasının genel ismi bu olarak geçse de; interface olarak buna denk gelir: __org.redisson.API.RReadWriteLock__. Bu interface ise Java'daki __java.util.concurrent.locks.ReadWriteLock__ arayüzünden türer. Sınıf olarak da __org.redisson.RedissonReadWriteLock__ kullanılır.

Read-Write-Lock'lar genel olarak hem read hem de write için ayrı ayrı seçenek sunan Lock'lardır. Örneğin read'i birden çok instance yapabilirken, write'ı sadece 1 instance'a izin verilir...
