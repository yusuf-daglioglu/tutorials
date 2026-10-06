# IT MANAGEMENT - ISO/IEC 20000

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

`ISO/IEC 20000-1` dökümanı, `Part 1: Service management system requirements` başlığını kapsıyor.

Bazı terimler:

## 📌 service

örnek: e-posta servisi.

## 📌 service catalogue (⟷ SC)

örnek: 

```
e-posta servisi sadec ekurum içi çalışanlara sunulan hizmettir.

SLA %99'dır.
```

## 📌 service catalogue (⟷ SC)

örnek: e-posta servisinin çalışması için bağımlı olduğu diğer servisler:

- `DB`
- mail server

## 📌 SLA (⟷ Service Level Agreement)

türkçe karşılığı: `Hizmet Düzeyi Sözleşmesi (⟷ Hizmet Seviyesi Sözleşmesi)`

hizmetin son kullanıcı ile arasında sözleşmedir. `SLA` müşterinin ne alacağını belirtir ve servis sağlayıcısından ne beklendiğini açıklığa kavuşturur.

## 📌 OLA (⟷ Operational Level Agreement)

`SLA`, müşteriye verilen söz iken, `OLA` o sözü tutabilmek için şirket içindeki ekiplerin birbirlerine verdikleri sözütemsil eder.

## 📌 event

servislerde meydana gelen veya tespit edilen durumdur. sorun olmak zorunda değildir.

örnekler:
- sunucunun `CPU` kullanımı %80'lere çıktı.
- backup başarıyla alındı.

## 📌 incident

hizmette kesinti veya kalite düşüklüğü oluşturan durum.

## 📌 problem

bir veya birden fazla `incident`'in sebebi (kök nedeni).

## 📌 change

servislerde yapılan herhangi bir dğeişiklik veya güncelleme.

## 📌 demand

müşteri veya şirket içi birimlerden gelen değişiklik istekleri.

## 📌 nonconformity

belirlenmiş bir gereksinimin karşılanmaması demektir.

örnek: prosedür "her ay rapor hazırlanır". nonconformity, "rapor hazırlanamadı" durumu olur.

## 📌 roller ve sorumluluklar

her servisin birçok farklı rol'de kişiye bağlantısı olabilir. bunların sayısı çok.

piyasada bazı şirketler bazı rolleri birleştirir. örneğin piyasada `rezilyasn yöneticisi` sıkça kullanılır. bu terim, `ISO/IEC 20000`'deki 2 fakrlı rolü birlikte kapsamaktadır.