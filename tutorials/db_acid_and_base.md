# DB ACID AND BASE

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 BASE

ACID'e alternatif bir karakteristik listesidir. Genelde NoSQL sistemlerde bu karakteristikler gözlemlenir. 3 maddenin kısaltmasıdır:

- __Basically Available__

  BASE'deki `B` ve `A` buradan geliyor.

  sistem sürekli cevap verir (tutarsız data dönse bile).

- __Soft state__

  Sistemdeki veriler yeni input olmasa bile, değişebilir (çünkü diğer DB'leri birbirlerine arka planda sync oluyor).

- __Eventual consistency__

  sistem bir süre sonra tutarlı (ilgili query için aynı bilgiyi) bilgi döndürecek hale gelecektir.

BASE kullanan bir sisteme örnek verirsek: arama motorları. Elasticsearch gibi NoSQL'e kayıt atar. Atılan kayıt hemen okunamaz. index'lenmesi uzun sürer. Burada dikkat edilirse BASE bir sistem vardır.

## 📌 Atomicity vs Transactional

Bu terim birbiri yerine sıkça kullanılır. Fakat tamamen farklı kavramlardır. Atomicity, Transaction'ın sadece 1 karakteristiğidir. Oysa bir transaction'ın 4 farklı karakteristiği var. Bunlar ACID'dir.

İlk olarak bir akademik makalede A, C, D ortaya atıldı. Bu makalede bunların bütününün bir "transaction" olduğu belirtildiği. Daha sonra farklı bir makalede isolation karakteristiği eklendi. Dolayısı ile "transaction" terimi ile "ACID transaction" terimi tümüyle aynı anlama gelmektedir.

## 📌 ACID

DB transaction karakteristikleridir (Transaction Properties):

- __Atomicity__: Ya hep ya hiç demektir. Bir transaction içinde bütün işlemler yapılır, ya da biri dahi yapılamıyorsa hiçbiri yapılmaz. rollback özelliğidir. Eğer transaction içinde bir hata olursa sistem en başa dönebilmelidir (yaptıklarını geri alabilmelidir).

- __Consistency (⟷ tutarlılık)__

  Yapılan işlemler sonucunda oluşan çıktıların tutarlılığı anlamına gelir. yani; primary-key, foreign-key, not-null gibi tablo kurallarının bozulmadığı garanti edilir.

- __Isolation (⟷ izolasyon ⟷ yalıtım)__

  Transaction gerçekleşirken dışarıdan müdahaleye izin verilmemelidir. izolasyon seviyelerine göre değişiklik gösteren bir özelliktir. örneğin; gerekiyorsa/isteniyorsa kullanılan tablolar ⟷ satırlar dışarıdan erişim için bekletilmelidir (lock'lanmalıdır). yada gerekiyorsa/isteniyorsa; dışarıdakiler transaction'ın yazdıklarını okuyabilir, fakat değişiklik yapamaz. izolasyon seviyeleri başka başlıkta detaylı anlatılıyor.

- __Durability (⟷ dayanıklılık)__

  işlem client tarafa tamamlandı denildiğinde (commit edildiği onayı geldiğinde), verinin sorunsuzca kaydedildiğini garanti etme özelliğidir. herhangi bir elektrik kesintisinde dahi sistemin kayıt işlemini garanti etmesi gerekir.
