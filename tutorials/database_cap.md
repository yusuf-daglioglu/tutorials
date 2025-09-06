# DATABASE CAP

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 CAP theorem

Eric Brewer tarafından ortaya atıldığı için "__Brewer's theorem__" denir.

dağıtık bir sistemde veri üzerinden sunulan hizmet için aynı anda 3 özelliğin sağlanamayacağıdır. En fazla 2 tanesinin sağlanabildiğidir. Bu 3 özellik nedir:

- __Consistency (or Tutarlılık)__

  "Consistency" terminini ucu açıktır. Çünkü neyin tutarlı olacağına göre değişir. Örneğin; ACID'deki Consistency, data'nın primary-key, not-null gibi kurallara uyduğunu belirtmek için kullanılır. Oysa CAP teoremindeki Consistency, bilgisayar bilimlerinde "__linearizable (or doğrusallaştırılabilir)__" olarak kullanılır.

  dağıtık sunuculardan birine X bilgisini kaydettik. diğer herhangi bir sunucudan o veriyi okumak istediğimizde X okuruz. (bu eventual-consistency'nin zıttıdır diyebiliriz - zıttı demek yanlış alternatifi demek daha daha doğru olabilir)

- __Availability (or Erişebilirlik)__

  istediğimiz zaman sunuculara erişim açık olacaktır (sunucular erişilebilir durumda olacaklardır). Bu madde "high-availability"'den farklıdır. "high-availability" yük dağıtmak ve hata durumunda diğer sunucuları hazır tutmak amaçlı iken, sadece "Availability" terimi sunucuların cevap vermeyi reddedip reddetmemesi karakteristiğidir.

- __Partition Tolerance (or Bölünebilme Toleransı)__

  sistemdeki data'lar tüm node'larda değil, parçalanmış durumda olması durumudur.

Örneğin; ağ arasındaki haberleşme giderse (P); C'nin sağlanması mümkün olamaz. Fakat A sağlanabilir.

Örneğin; Eğer data partitioning yaptıysak (P) ve node'lar arasında network sorunu olduğunda şu iki çözümden birini uygulamamız şart:

- client'tan gelen request'leri geri çevir --> bu durumda; sistemin tutarlılığı sağlanmış olur (C), fakat availability'den (A) feragat etmiş oluruz
- client'lardan gelen request'lere, eldeki en sonki güncel data ile cevap ver --> bu durumda; sistemin tutarlılığı sağlanmamış olur (C), fakat availability (A) sağlanır.

(yukarıdaki örnek için kaynak: (source-id: 87) "Peki bu Partition Tolerance ne işe yarıyor ? Hala tam anlayamadım diyorsanız biraz daha konunun üzerine gidelim." maddesi.)

Facebook gibi sosyal medya yazılımlarında, "like" sayıları anlık olarak herkese doğru gösterilmeyebilir. "Like" gibi tölerans gösterilebilen birçok data tipi vardır. Bunlar için uygun teknoloji seçimi yapmak gereklidir. CAP teoremi yazılarında, hangi ikili istendiğinde hangi veritabanı kullanılabileceği de yazmaktadır.

```text
                                 CONSISTENCY (All client get the same data)
                                           x
                                        x     x              
          All RDBMS                x             x           Zookeeper
                              x                      x       Etcd
                          x                             x    
                     x                                     x
                 x                                             x
            x  x  x   x   x   x   x  x   x  x  x  x  x  x  x  x  x
AVAILABILITY                                                       PARTITION
(All clients can                   Cassandra                 (The system is not effected
always read and write)             ElasticSearch              by network partitioning)
                                   
                                   
```

Yukarıdaki şekilde DB isimleri farklı köşelerde de olabilir. Çünkü, DB sistemleri yapılacak config'lere göre davranışı değişebilir. Örneğin:

- Hazelcast gibi birçok in-memory DB'ler yapılacak ayarlara göre AP veya CP çalışabilir.
- MongoDB yapılacak ayarlara göre AP veya CP çalışabilir.
- Cassandra'yı 1 node ve 1 cluster ile kullanıp, sadece "lightweight transaction" ile yazma okuma işlemi yaparsak CP yönüne kaydığını görürüz.

## 📌 PACELC

pronounced "pass-elk".

Teorinin başlığı orjinal dökümanındaki bu cümledeki karakterlerin birleşiminden gelmektedir: "if there is a partition (P), how does the system trade off availability and consistency (A and C); else (E), when the system is running normally in the absence of partitions, how does the system trade off latency (L) and consistency (C)?"

Burada CAP'e ek olarak __latency (L)__ de işin içine katılmaktadır.

CAP teoreminin bir uzantısı olarak ortaya atılmıştır. Yukarıdaki cümle şunu diyor:

- P varsa, A ve C arasında seçim yapılması gerekir (Bunu zaten CAP söylüyor).
- P yoksa, C ve L arasında da seçim yapılması gerekir (işte bunu sadece PACEL söylüyor).

Normalde CAP, P yoksa, A ve C arasında seçim yapmamızı söylüyor. Oysa P yoksa, A ve C arasında seçim yapmamızdan ziyade, C ve L arasında seçim yapmamızın daha düşünülmesi doğru bir kriter olduğunu dile getiriyor. Çünkü P yoksa, zaten C ve A her türlü sağlanacak, fakat asıl mesele A'nın ne kadar Latency ile sağlandığı düşünülmelidir.

## 📌 kısaltmalar

bir sistemimiz/yazılımımız olsun. Bu yazılım PACEL'deki hangi şemaya göre çalışıyor ise ona göre bir isim verebiliyoruz.

Aşağıda "yok", "umursanmıyor" anlamında kullanılıyor.

Örnekler:

- __PA/EL__ system:

  Partition varsa, A da var (ama C yok). Eğer partition'suz çalıştırırsak, L var ama C yok.

Yani format şu şekilde:

> PX/EY

P varken, X var. P yokken (else), Y var.
