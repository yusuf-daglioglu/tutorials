
# PATTERN TYPES

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 pattern types

- Creational Pattern

  bir veya birden fazla objenin init edilmesi ile ilgili konuları ele alır.

- structural pattern

  Sınıfların birbirleri arasındaki ilişki yapılarını ve birbirlerini nasıl kullandıkları ile ilgili konuları ele alır.

- Behavioral pattern

  Objelerin birbiri ile olan ilişkilerinden ziyade, birbirleri arasında haberleşmelerini veya birbirlerini ne zaman kullanmaları gerektiği (rolleri/görevleri) konuları ele alır.

- Concurrency pattern

  Multi-thread uygulamalar için geliştirilen pattern'lerdir.

- Architectural pattern

  bunun çalışma alanı keskin çizgilerle ayrılmış değildir.

  fakat daha çok üst seviyeli, yani;
  
  - sınıf ve fonksiyon seviyesinde değil,
  - sınıf grupları (paketleri), yazılımlar, modüller seviyesinde
  
  olan pattern'lerdir.

- Database pattern

- Anti-Pattern

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 System design (or sistem dizaynı) ve Architectural design (or mimari dizayn)

System design son kullanıcı ve API use case'lerine göre ihtiyaçların belirlendiği dizayndır. Oysa mimari dizayn teknolojilere göre nelerin kullanılacağı ve nasıl kullanılacağını belirler.

Örnek 1:

- sistem design: Veritabanının isim sutununda max 50 karakter olacağı
- mimari dizayn: Veritabanının postresql olacağı.

Örnek 2:

- sistem design: "/user" PATH servisi user bilgilerini JSON dönecek.
- mimari dizayn: "/user" PATH servisi Sping-MVC olacak ve Jackson JSON parser kullanılacak.

Örnek 3:

- sistem design: sistemden aynı anda 10 sipariş aynı anda işlenebilecek.
- mimari dizayn: sistemden aynı anda 10 sipariş işletebilmek için CQRS ve event-sourcing kullanılacak.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

