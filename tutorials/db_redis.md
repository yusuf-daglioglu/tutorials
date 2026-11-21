# DB REDIS LOCK

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Redis

- belirli aralıklarla (isteğe bağlı olarak) verileri dosyaya kaydedebilir.
- tümüyle `C` ile yazılmıştır. tek thread çalışır.

`Redis` client'ları:

- __Lettuce__

  reactive API, `thread-safe`.

  Sadece native `Redis`'in sunduğu özellikleri full destekleyen API'ler sunar.

- __Redisson__

  `Redis` aslında "lock" gibi özellikleri native desteklemez. Fakat `redisson` bazı tricky yöntemlerle daha gelişmiş özelliklerin kullanılmasını sağlar:

  `Queue`, `Scheduler`, `RPC`, `Lock`

- __Jedis__

  çok basit ve sadece sync API sunuyor.

- __Spring-data-Redis__

  wrapper'dır. arka planda `Jedis` yada farklı bir implementasyonu kullanır.

  `Spring cache` modülündeki `redis` provider'ından tamamen farklı bir pakettir.

Terimler hakkında:

- `Redis Lab`; `Redis`'in enterprise versiyonunu geliştiren firmanın ismi.
- `Redis lab`'in kendi AWS'lerinde bize hazır sunduğu `Redis`'ler var. bu hizmetin özel adı `Redis Enterprise Essentials` dır.
- `Redis Enterprise Pro` ise `Redis lab`'ın sunduğu AWS entegrasyonudur. Kendi AWS hesabımızda kullanacağımız `Redis`'lerdir.

## 📌 Redis Sentinel

ayrı bir instance'tır. client kütüphanesinin sentinel desteği olmak zorundadır.

proxy gibi çalışmaz. hangi `Redis` instance'ları ayakta diye, sentinel sürekli monitoring yapar ve bunu client'lara bildirir. client'lar, sentinel'e sadece hangi `Redis`'ler ayakta diye sorar.

## 📌 data types of Redis

`Redis`'te her bilgi bir key'e karşılık tutulur. Örneğin bir liste tutacak isek bunun bir key'i vardır. o key aracılığı ile bu listeye erişebiliriz. 1 adet key bazen sadece 1 adet string tutmak için kullanılır.

`Redis` aşağıdaki tiplerden daha fazlasını destekler. Aynı zamanda aşağıdaki tipler içerisinde data'yı bit bazında okuma gibi birçok ek özellik sunar.

### 📌📌 String

bir key karşılığında bir string saklar. Örnek `Redis` komut satırı ile bir key oluşturabiliriz:

```text
> SET mykey "10"
```

var olan string'e append, eğer sayı ise 1 arttırma gibi özel komutlarda sunar.

### 📌📌 List

`Redis` komut satırları örnekler:

```text
> LPUSH mylist a   # now the list is "a"
> LPUSH mylist b   # now the list is "b","a" # append to left side - left push
> RPUSH mylist c   # now the list is "b","a","c" # append to right side - right push
```

### 📌📌 Set

1 key'e karşılık tüm set'i barındırıyoruz.

```text
> sadd myset 1 2 3
(integer) 3

> smembers myset
1. 3
2. 1
3. 2
```

### 📌📌 Hashes

1 key'e karşılık bir `hashmap` barındırıyoruz.

`Redis` 4 ile birlikte, `HMSET` komutu, `HSET` olarak değiştirildi.

```text
> HSET myhash field1 "Hello"
(integer) 1

> HGET myhash field1
"Hello"
```
