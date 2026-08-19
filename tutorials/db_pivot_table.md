# DB PIVOT TABLE

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 pivot table

pivot Türkçe anlamları: en önemli nokta, püf nokta, çark etmek, eksen üzerinde dönmek.

pivot olarak belirlediğimiz sütuna ait satırları, sütun haline çevirip oluşturduğumuz tabloya pivot tablosu denir.

en basit örnekle ilerleyelim:

- bir sipariş tablomuz var.

- Bir müşterinin verdiği her bir sipariş bir satırda gösteriliyor.

- Her müşteri için son 6 aylık sipariş bilgilerini görmek istiyoruz.

> ||| SiparisID ||| MusteriAdSoyad ||| UrunAdi ||| Tutar ||| Donem |||

tablomuz olsun.

pivot tablomuzu oluşturursak;

```sql
SELECT *

FROM (

      SELECT

       MusteriAdSoyad

      ,Donem

      ,sum(Tutar) as ToplamTutar

      FROM Siparis

      group by MusteriAdSoyad ,Donem

     ) as gTablo

PIVOT

(

  SUM(ToplamTutar)

  FOR Donem IN ([200911],[200912],[201001])

)

AS p
```

Tablomuz şöyle olacaktır:

> ||| MusteriAdSoyad ||| 200911 ||| 200912 |||

MS Excel gibi uygulamalarda da pivot tablosu yapabilmek için fonksiyonlar hazır sunulmaktadır.
