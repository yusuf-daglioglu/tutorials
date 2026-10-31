# SECURITY RANDOM NUMBERS

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Rassal değişken (⟷ Random variable)

rastgele oluşturulan değişkendir.

## 📌 sözde rastsal (⟷ pseudo random number)

bilgisayar ortamında rastgele değişken oluşturulamayacağından, sözde rastsal sayılar oluşturulabilmektedir.

## 📌 pseudorandom number generator (⟷ eng-kısaltma:PRNG ⟷ deterministic random bit generator ⟷ eng-kısaltma:DRBG)

sözde rastsal sayı üretebilmek için kullanılan yazılımlardır.

PRNG, initial value'ya bakar. Initial value aynı olursa sürekli aynı çıktıları üretir.

bahsi geçen bu initial value'ya __seed (⟷ tohum)__ denir.

## 📌 tr:entropi (⟷ eng:entropy)

kelime anlamı:

- Termodinami bilimindeki (fizikteki) tanımı: termal enerjinin faydalı işe çevrilemeyen kısmı olarak tanımlar (yani verimli kullanılamayan kısmına/oranına denir).

- İstatistik biliminde: measurement of how unpredictable something is.

  - entropi ne kadar yüksekse, bir olayın sonucu o kadar belirsizdir.
  
  - entropi düşükten yükseğe sırası ile örnekler:

    - bir zar atıyoruz: zarın 2 yüzü var. bi yüzü (tarafı) daha ağır ise (ağır olan tarafın gelmesi daha yüksek ihtimal olduğu için entropi düşük olur)
  
    - bir zar atıyoruz: zarın 2 yüzü var. 2 ihtimal var. entropi yüksektir.

    - bir zar atıyoruz. zarın 20 adet yüzü var. bu durumda entropi 2 yüzlü zarı atmaktan daha yüksektir.

## 📌 Hardware random number generator (⟷ eng-kısaltma:HRNG ⟷ true random number generators)

donanımlar aracılığı ile rastgele sayı üretilememektedir (yazılımda olduğu gibi). fakat donanımlar bazı sensörler ile çevreden bilgi alıp rastgele sayı üretmeye çalışabilirler. örneğin; ısı sensörü gibi. aslında burada da %100 rastgele sayı üretilmiş olmuyor. fakat önceden kestirilemeyen (unpredictability) sayı üretiliyor.

bilgisayarlar rastgele sayı üretebilmek için, her cihazda olan aşağıdaki 2 referanstan yararlanır. Her donanımda olduklarından portable algoritmalar için tercih edilirler.

- saat-tarih
- CPU çalışmaya başladığından itibaren kaç cycle geçmiş gösteren __Clock Sequence__

## 📌 cryptographically secure pseudorandom number generator (⟷ CSPRNG ⟷ cryptographic pseudorandom number generator ⟷ CPRNG)

%100 olmasa da belli standartları destekleyen güçlü seviyede random sayı üreten kütüphaneler mevcut. bunlara verilen özel isimdir. örnekler:

| Platform                               | CSPRNG                                                |
|----------------------------------------|-------------------------------------------------------|
| PHP                                    | mcrypt_create_iv, openssl_random_pseudo_bytes         |
| Java                                   | java.security.SecureRandom                            |
| Dot NET (C#, VB)                       | System.Security.Cryptography.RNGCryptoServiceProvider |
| Ruby                                   | SecureRandom                                          |
| Python                                 | os.urandom                                            |
| Perl                                   | Math::Random::Secure                                  |
| C/C++ (MS Windows API)          | CryptGenRandom                                        |
| Any language on unix-likes             | read from /dev/random or /dev/urandom                 |

Not: java.security.SecureRandom sınıfının constructor'ı parametre olarak Provider alıyor. Her provider farklı kaynaklardan data çekmeye yarıyor.

Bu tablodaki listede bulunan algoritmalar ek olarak donanımlardan da yararlanmaktadır. CPRNG'ın donanımdan yararlanması opsiyoneldir.

Yukarıdaki API'ler genelde `OS`'un sunduğu API'lerden yararlanır. `OS`'ların sunduğu API'ler, kaynaklardan (donanımlarda oluşan event'lerden) bilgi toplarlar. Fakat bu bilgilerin zamana oranla üretimi kısıtlıdır (entropi düşüktür). Dolayısı ile belli zaman diliminde belli sayıda rastgele data üretilebilir. Daha sık üretilmesi için özel cihazlara (HSM gibi) başvurulmalıdır.

Her `OS`'ta aşağıdakilerin davranışları farklı olabilir.

- "/dev/random" entropi düşükken, isteği bloklar (bekletir). ne zaman yeni data depolanır o zaman cevabı döndürür.
- "/dev/urandom" ise, eğer entropi düşükse, eski data'ları kullanarak cevap döndürür.

## 📌 software based random number generators

bazı yazılımlar entropi üretmektedirler. `OS`'ların default entropi üreten modüller (yazılımları) çok basittir. Özellikle GUI'siz bir ortamda kullanılması pek önerilmez.

Bu sebeple daha gelişmiş bazı yazılımlar kullanılır. Örnekler:

- Haveged (özel isimdir. içerisinde "HAVAGE" isimli algoritmayı kullandığından "havaged" olarak isimlendirilmiştir. HAVAGE algoritması bunun kısaltmasından üretilmiştir: HArdware Volatile Entropy Gathering and Expansion).
- EGD (⟷ Entropy Gathering Daemon)

## 📌 HAVAGE

HAVAGE algoritmasında, `OS`'un event'leri ve external event'ler HAVAGE generator'u sürekli reseed etmektedir.

HAVAGE algoritması CPU'da kod işletiyor ve bu kodun CPU'daki state'ini sürekli inceleyip, bu state'lerden entropi üretiyor.

## 📌 rastgele sayı üretimi hakkında

- Teorik olarak; sadece, hiç input almadan rastgele sayı üretebilen bir platform ancak bize gerçekten rastgele sayı üretebilir.

- random.org sitesi bir web servis üzerinden rastgele sayı servisi sağlıyor. bu servis hava durumundan yararlanıyor.
