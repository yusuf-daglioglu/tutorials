# UNICODE AND MULTI-LANGUAGE APPS

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Endianness

Bir veri başka bir yere taşınırken verinin taşınma sırasını temsil eder. __Big endian__'da değeri yüksek kısım (byte) (__most significant byte__) sağ tarafta olur. __little endian__'da ise (__least significant byte__) tersi geçerlidir.

Big Endian piyasada "__Motorola convention byte ordering__" yada "__Motorola style byte ordering__" olarak da geçmektedir. çünkü `Motorola` `CPU`'larda hep big endian kullanılmaktadır.

Bu sıralama bit bazında değildir. Bit bazında hangi sırada tutulduğu donanımın kendisi belirler. eğer bit bazındaki önceliklendirmeden bahsediyorsak; "__Bit endianness (⟷ bit-level endianness)__" terimini kullanmak durumundayız.

Fakat network dünyasında __endianness__ yerine "__network order__" terimi kullanılır ve bit bazındaki transfer yönünü belirler. network dünyasında; "__network byte order__" big endian'a denk gelmektedir.

## 📌 bit

__binary digit__'ten türemiştir.

## 📌 nibble

kelime anlamı: kemirmek.

`byte` `İngilizce`'deki ısırmak anlamına gelen `bite` ile eş seslidir. bu sebeple `nibble`'a da bu isim takılmıştır.

`Türkçe`'de bu terimin karşılığı yok. `yarım byte` olarak ifade edilir.

bazı kaynaklarda `nibble` yazmak yerine şu terimler tercih edilir:

- 'byte' ile yazımını benzetmek amaçlı __nybble ⟷ nyble__ olarak ta kullanılır.
- __half-byte ⟷ tetrade__ olarak kullanılır.
- network sistemlerinde genel olarak __semi-octet ⟷ quadbit ⟷ quartet__ kullanılır.

Ek not: __Octet__ 8 bit anlamına geliyor. __Byte__ terimi yerine, __octet__ kullanılıyor bazı makalelerde. Bunun sebebi __byte__ teriminin eskiden cihaz üreticileri tarafından farklı boyutta olabilmesiydi. Bu sebeple __octet__ kelimesi doğdu. __Octet__ daha da eski yıllarda __octad (⟷ octade)__ olarak ta kullanılıyordu.

## 📌 charset (⟷ karakter seti) vs character encoding (⟷ karakter kodlaması)

`charset`, hangi karakterleri kullanılıp kullanılmayacağını belirtir. Örneğin; ABC kümesi bir `charset` olabilir. Fakat bunların binary bazında nasıl kaydettiği bilgisini `charset` içermez. binary bazında nasıl kaydedileceğini (tutulacağını) `encoding` kuralları belirler. A harfi bir `encoding`'de 8 bit olarak tutulurken, farklı bir `encoding`'de yine A harfi 16-bit olarak tutulmak zorunda kalabilir. Her `encoding` `charset` uzayının boyutuna göre ayarlandığından (formatlandığından/boyutlandırıldığından), belli bir `charset`'i destekliyor olur. Dolayısı ile bir yerde `charset=UTF-8` gibi bir ibare görülebilir. Bu ibare; `UTF-8`'in desteklediği `charset`'ler anlamına gelmektedir.

## 📌 ASCII (⟷ American Standard Code for Information Interchange)

Bir encoding standardıdır. Sadece `İngilizce` ve en temel karakterleri içerir. 7 bittir. 1 bir ekstra olarak parity biti olarak kullanılır. dolayısı ile 8 bit yer kaplar.

## 📌 Extended ASCII (⟷ EASCII ⟷ high ASCII ⟷ Genişletilmiş ASCII)

8 bittir. parity biti içermez. ilk 7 biti `ASCII` ile aynıdır. diğer bitler ise farklı karakterler içerir. diğer bitler için bir standart yoktur. her OS ve kurulan dil paketine göre değişiklik gösterir.

## 📌 Windows code pages (⟷ old-name:ANSI code pages)

- `7+1` bittir. 
- `ASCII` yetersizliğinden ortaya çıkmıştır.
- `ASCII` ile ilk 7 biti aynı karakter setine referans ettiği için `ASCII` ile uyumludur.
- Diğer 128 bit `ISO 8859` standardının türevlerin değişiklik göstermektedir.
- Örneğin; `Windows-1254`; `ASCII` (ilk 127) ve `TR` (diğer 127) içermektedir.

## 📌 ISO 8859

- `ISO 8859` standartın adıdır.
- `7+1` bittir.
- `ASCII` yetersizliğinden ortaya çıkmıştır.
- `ASCII` ile ilk 7 biti aynı karakter setine referans ettiği için `ASCII` ile uyumludur.
- Diğer 128 bit `ISO 8859` standardının türevlerin değişiklik göstermektedir.
- Örneğin; `ISO 8859-9`; `ASCII` (ilk 127) ve `TR` (diğer 127) içermektedir.

## 📌 Unicode Transformation Format (⟷ UTF)

Bir encoding standardıdır. kendi içinde `UTF-8`, `UTF-16`, `UTF-32` gibi ayrımları vardır. Format sürekli yenilenmekte ve yeni versiyon çıkmaktadırlar.

`UTF` karakter boyutu çok büyük olduğundan boş atanmamış karakter noktaları/kodları da vardır.

- `UTF-8`: karakter'e göre 1-4 byte arsında her karakter uzayabilir. ilk 7 biti aynı karakter setine referans ettiği için ASCII ile uyumludur.

- `UTF-16`: karakter'e göre 2-4 byte arsında her karakter uzayabilir. ASCII ile uyumsuzdur.

- `UTF-32`: her karakter 32-bittir. her karakter ayrı ayrı ayrıştırılabilir. `ASCII` ile uyumsuzdur.

- `UTF-7`: birileri tarafından makalelerle yayınlanmıştır. fakat resmi bir UTF standardı değildir.

`UTF-8`, `UTF-16` ve `UTF-32` tüm karakterleri gösterebilme yeteneğine sahiptir (hatta boş alanları dahi vardır).

## 📌 code unit

bir karakteri `UTF-8`, `UTF-16` gibi `encoding`'lerle byte haline çevirdiğimizde ortaya çıkan byte bilgisine verilen isimdir. örneğin; `A` karakterinin code point'i `U+0031` olsun, code unit'i `UTF-8` için `0031`, `UTF-16` için `00010031` olabilir (değerler tamamen uydurmadır).

## 📌 Unicode

UTF-8, 16 ve 32 bir encoding formatı iken, bu encoding'lerin baz alındığı(desteklendiği) karakter-seti Unicode'dur. UTF-8 dahi tüm Unicode'u desteklemektedir.

"Unicode" kelimesi MS Windows'un bazı yazılımlarında yanlış kullanılmaktadır. bu sebeple karıştırılmamasına dikkat edilmeli.

## 📌 '€' karakterini UTF-8 ile kodlayalım

- € karakteri U+20AC kod noktasına tekabül eder

- 0010 0000 1010 1100 (binary) = 20AC (hexadecimal)

- U+20AC 2 byte'tır. fakat aralık Unicode standartlarında U+0800 ile U+FFFF arasında olduğu için 3 byte ile tutulmalıdır.

- 3 bayt olduğu için 3 tane 1 yan yana kullanılacaktır. 1'lerin bittiğini belirlemek için 1 adet sıfır sağ tarafa eklenecek. sonuç; 1110.

- 1110 4 karakter odluğu için 1 bayta tamamlanacak ve 0010 0000 1010 1100'nin en önemli 4 biti daha kullanılacaktır. Sonuç: 1110 0010

- daha sonraki kısmı da belirli algoritmalarla tamamlanıyor.

akışa bakıldığında; tüm metin için ortak bir yerde metadata saklanmadığı görülmektedir. yani her karakterin uzunluğu UTF-8 ve UTF-16 için karakterin başında bellidir. bu sebeple bir sonraki yada önceki karakterler ilgili karakterimizi boyutunu değiştirmemektedir.

## 📌 Unicode karakter özellikleri

Unicode standartlarında her karakterin bir meta bilgisi vardır. bu bilgi karakter kodundan değil, dökümantasyondan çıkartılmalıdır.

- `genel kategori (⟷ general category)`

  30'a yakın kategori vardır (yıl 2019)

  kategoriler virgülle ayrılmış iki kısımdan oluşmaktadır. ilk kısım major class, ikincisi ise subclass'ı temsil eder.

  örnek: "Number, decimal digits", "Number, letter", "Number, Others".

  bazı kategoriler:

  - Other, control

    kod: Cc

    açıklama: "start of text", "end of line" gibi karakterleri buradadır.

  - Letter, Lowercase

    kod: Ll

  - Letter, Uppercase

    kod: Lu

  Fakat her kategori %100 şekilde uymamaktadır. geriye uyumluluk için bazı istisnalara tölerans gösterilmiştir. (bazıları yeni sürümlerde düzeltiliyor)

- code point

  U+0001 şeklindeki gösterimin ismidir. burada U+ hiçbir anlamı yoktur. Unicode'un verdiği özel bir prefix'tir. Aslında asıl değer 0001'dir ve bu değer Unicode karakter listesindeki 2'inci elemana referans eder. 0001, hexadecimal formattadır.

- name

  büyük harflerle alfanumerik, boşluk ve tire işaretli içerebilir. tekildir fakat her karakter için set edilmiş bir değer olmayabilir.

- name alias

  Unicode version 2 ile birlikte

- script

  (başka başlıkta ne olduğu anlatılıyor)

  200'e yakın script vardır.

- Bidirectional class

  "Unicode Bidirectional (BIDI) Algorithm" için gerekli bilgileri verir. metnin sağdan sola mı soldan sağa mı, yoksa etkisi olup olmadığı gibi... Bu bilgiden 25'e yakın var (yıl 2019). bu sebeple algoritma karışık. aynı satırda birden fazla farklı dilden metin olursa ne yana yaslanacağı belli kurallara göre (algoritma ile) belirlenmektedir. Aynı satırda farklı yönlerde karakterlerin olması durumunda o text bloğuna Bi-directional text (⟷ BI-DI ⟷ BIDI) denir.

- Lowercase/uppercase Character

  aynı karakterin büyük yada küçük versiyonunun code-point'i.

- Mirrored

  karakterin mirror'u olup olmadığı bilgisi. bu bilgi sadece true/false şekilde tutuluyor. hangi karakter bu karakterin mirror'u olup olmadığı bilgisi tutulmamaktadır. örnek mirrored character'ler: < > ( )

- Decomposition

  bir karakter birden fazla karakterin birleşiminden oluşuyorsa, hangi karakterlerin birleşiminden oluştuğu bilgisi. örnek: vurgu işaretli e' karakteri; e ve vurgu karakterinden oluşuyor. dolayısı ile şapka yada tonlama işaretlerinden herhangi birini almış her karakterin Unicode'u farklıdır. örneğin yunancadaki omega işareti; ω, tonlama aldığı zaman (ώ) farklı codepoint'e sahip farklı karakter oluyorlar.

Bu gibi karakter özellikleri karakterlerin tipini algılamak durumunda olduğumuzda önemli olabiliyor. örneğin; Unicode'da her karakterin bilgisi saklanıyor. Eğer kullandığımız regex kütüphanesi bu bilgileri saklıyor ise; o zaman aşağıdaki gibi kontroller ile de regex yazabiliriz:

- Alphabetic
- Ideographic
- Letter
- Lowercase
- Uppercase
- Title case
- Punctuation
- Control
- White_Space
- Digit
- Hex_Digit
- Non-character Code Point
- Assigned

kaynak: https://docs.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html "Unicode support" başlığı.

## 📌 UTF mimarisinde karakterlerin sıralanması/gruplanması

- plane

  plane kelime anlamı: tabaka

  tüm karakterler sıra bazlı gruplandırılmışlardır. sıfıra yakın plane'deki karakterler, günlük hayatta daha çok kullanılıyor.

  örnek:

  - plane 0

    aralık: U+0000 - U+FFFF

    adı: BMP (⟷ Basic Multilingual Plane ⟷ 0 BPM ⟷ plane 0)

  - plane 1

    adı: 1 SMP (⟷ Supplementary Multilingual Plane)

  - plane 2

    adı: 2 SIP (⟷ Supplementary Ideographic Plane)

  - plane 3-13

  - plane 14

    adı: 14 SSP (⟷ Supplement­ary Special-purpose Plane)

  - plane 15-16

  Yukarıdan görüldüğü gibi; U+FFFF - U+FFFFFF aralığı için plane'ler "supplementary" prefix'ini alıyor.

- block

  tüm karakterler plane'den daha ufak seviyede de gruplandırılmıştır. kolay hesaplama olsun diye yapılmıştır. bazı block'lar:

  - U+0000 - U+007F

    ismi: Basic Latin

  - U+0080 - U+00FF

    ismi: Latin-1 Supplement

  - U+0100 - U+017F

    ismi: Latin Extended-A

  ...

  - U+0600 - U+06FF

    ismi: Arabic

  ...

  - U+0750 - U+077F

    ismi: Arabic Supplement

  ...

  - U+08A0 - U+08FF

    ismi: Arabic Extended-A

  ...

## 📌 BOM (⟷ Byte Order Mark)

UTF'lerde (little/big endian) önemlidir. Bu sebeple standartlara göre opsiyonel olarak text'in en başına byte order bilgisi atanır. Bu bilgi ile text'in hangi order'da olduğu belirtilir. Text editörler menülerde encoding'leri bu şekilde gösterirler: UTF-16-LE, UTF-32-BE olarak kısaltılırlar. UTF-16-with-BOM'da kullanılır fakat LE mi BE mi belli değildir. bu yüzden bu kullanım tavsiye edilmez.

BOM'un uzunluğu belirsizdir. çünkü birçok kombinasyonu vardır. UTF-8, 16, 32, 7 (ve daha fazlası) ve bunların LE BE kombinasyonları vardır.

## 📌 EOF (⟷ End Of File)

EOF dosyanın içinde olan bir karakter değildir. Çoğu programlama dilindeki dosya okuma yazma API'leri bunu karaktermiş gibi yansıtabilir. fakat karakter değildir. Böyle bir karaktere ihtiyaç yoktur, çünkü tüm dosya sistemlerinde dosyaların boyutu bellidir.

dosyanın sonuna gelindiğinde driver (API), bu durumu eof karakteri yaratarak ve bunu read() fonksiyonundan döndürerek çalışır. API'ler böyle çalışmak zorunda değillerdir. fakat piyasada neredeyse tüm API'ler bu şekilde çalışır. API/driver'ın döndüreceği değer API'nin sunduğu static bir EOF adında tam sayıdır. yani Unicode'da EOF karakteri yoktur.

End of Text, End of Transmission, End of Transmission Block, End of Medium, File Separator, gibi EOF ile çok benzer amaçta kullanılan karakterler vardır. Bunlar gerçek Unicode karakterleridir ve okunan kaynaktan (stream'den/source'den) döndürülürler ve gerçekten fiziksel olarak ilgili kaynakta bu karakter vardır.

## 📌 newline (⟷ line ending ⟷ end of line ⟷ EOL ⟷ line break ⟷ satır sonu)

birçok alternatifi vardır. OS de varsayılan olarak değişebilir:

- POSIX: LF (line feed) --> \n

- MS Windows: CR+LF (⟷ CRLF) --> \n\r

- bazı sistemlerde sadece CR kullanılmakta, bazılarında ise LF+CR kullanılmaktadır.

açılımları:

CR: carriage return

LF: line feed

Atom text editor'de "invisible character" olarak nitelendirilen karakterler: boşluk, eof ve tab'dır. Atom, LF ve CRLF'yi simge olarak göstermektedir.

Bunlar dışında karakter setlerinden birçok kontrol (özel) karakter bulunmaktadır. örnek:

- paragraf sonu

- backspace

- delete

- ctrl-z

Bu karakterler önyüzde bir şey göstermek için değil, belli işlemler yaptırmak için kullanılır. bu karakterler sadece dosyadan okununca işe yaraması için yapılmamıştır. örneğin CTRL-Z karakterini C gibi bir dilinde printf ile ekrana bastığınızda CTRL-Z olan "geriye al" işlemini yapar. yada klavyede basılan bir tuşu uzak makinada çalıştırmak için bu tarz kontrol karakterleri işe yarayabilir. Bu tarz karakterler "control" karakteri olarak Unicode'da da kullanılmaktadır.

## 📌 Unicode'daki bazı özel karakterler

Unicode'da özel karakterler diye bir grup yoktur. Bu başlıkta boşluk, satır sonu gibi karakterler hakkında açıklamalar vardır.

```text
LF:    Line Feed, U+000A

VT:    Vertical Tab, U+000B

FF:    Form Feed, U+000C (yeni bir sayfa olduğunu printer'a belirtmek için kullanılır)

CR:    Carriage Return, U+000D

CR+LF: CR (U+000D) followed by LF (U+000A)

NEL:   Next Line, U+0085 (bazı ibm platformları bunu kullanıyor.)

LS:    Line Separator, U+2028

PS:    Paragraph Separator, U+2029
```

yukarıdaki bazı karakterlere simge atayabilmek için farklı karakterler ile ikon seti belirlenmiştir:

```text
U+2424 (SYMBOL FOR NEWLINE, ␤)

U+23CE (RETURN SYMBOL, ⏎)

U+240D (SYMBOL FOR CARRIAGE RETURN, ␍)

U+240A (SYMBOL FOR LINE FEED, ␊)
```

Unicode'da 20'ye yakın farklı space vardır. bazıları farklı platformlarda kullanıldığı için, bazılarının ise uzunlukları farklı.

Unicode'un amacı yeni bir karakter standartı yaratmak değildir. var olan tüm karakterleri tutmaktır.

## 📌 invisible characters

UNICODE'da birçok invisible karakter vardır. Bunların bazıları yazılımlar tarafından input olarak alındığında hardcode olarak engellenmezken, bazıları engellenmektedir.

Örneğin bu projede: https://github.com/KuroLabs/stegcloak 

1- gizlenecek text
2- şifre
3- şifrelenmiş mesaj

input olarak programa veriliyor. "şifrelenmiş mesaj"ın en az 2 kelime olması zorunlu. şifrelenmiş mesajın boşluk kısmına (2 kelime arasına) gizli mesajın kendisi görünmez karakterler aracılığı ile gömülüyor. Aslında boşluk bir referans olarak kullanılıyor, yoksa gizlenen mesaj herhangi bir karakterin arasına da koyulabilir, çünkü zaten görünmüyor.

## 📌 URL encoding (⟷ percent encoding)

URL 'lerde boşluk ve bazı özel karakterler kabul edilemez. dolayısı ile bu karakterler için yeni bir encoding geliştirilmiştir. bazı karakterler % ve sonrasında 2 adet sayı olacak şekilde bir karakteri temsil etmektedirler.

## 📌 StringUtils

Apache-commons-lang kütüphanesinin içinde olan bir sınıf. Commons-lang içinde CharUtils, RandomStringUtils(random string üretiyor) sınıfları da mevcuttur.

## 📌 trim

Türkçe kelime anlamı: düzeltmek

bu metot birçok kütüphane içinde mevcuttur. birden fazla boşluk yan yana ise onları tek bir boşluk haline getirir. eğer boşluk en sağda yada en sonda varsa onları kaldırır.

## 📌 karakter grupları

karakterler son kullanıcı açısından gruplanmıştır. fakat bu grupların bir resmiyeti yoktur. örneğin; mobil uygulamalarda sanal klavyede i tuşuna basılı tutulduğunda i'nin türevleri çıkar. örnek: 'ı' gibi. bu karakter grupları o sanal klavyenin kendi ayarıdır. böyle bir gruplama UTF yada farklı karakter setlerinde tanımlanmamıştır.

İngilizce'deki küçük 'i' ve Türkçe'deki küçük 'i' ortak karakterdir. toLowerCase() metodu OS'tan default dili algılar ve ona göre uygun büyük harfe çevirir. toLowerCase() metodu parametre olarak Java Locale'i alır.

## 📌 accent (⟷ aksan ⟷ şive)

örnek: alman Türkçesi. ana dili Türkçe olmayan kişiler kullanır. bu terim "ağız" terimi ile çok benzer fakat farklı. ağız, ülke içinde kullanılan farklı tonlamalar gibi ufak değişiklikleri temsil ederken, aksan yurt dışındaki insanların kullandığı tonlamalar gibi ufak farklılıkları temsil eden terimdir.

## 📌 lehçe (⟷ diyalekt ⟷ Dialect)

örnek: Azerice (⟷ Azerbaycan Türkçesi ⟷ Azerbaycanca) bir Türkçe lehçesidir.

terminolojik olarak; resmi olarak lehçenin dil bilgisi farklıdır.

bazı kaynaklar dil bilgisindeki değişiklikler çok farklı olmadığında "lehçe" terimi yerine, "şive" terimi kullanılır. Eğer çok fazla fark var ise, "lehçe" yerine "dil" terimi kullanılır.

## 📌 ağız (⟷ subdialect)

örnek: karadeniz Türkçesi.

## 📌 aksan işareti (⟷ aksan imi ⟷ grave accent)

örnek: "ằ", "v̀" gibi hem sessiz, hem sesli harflerde birçok farklı şekilde bulunurlar.

StringUtils.stripAccents("abc"); metodu aldığı string'deki vurgu karakterlerini silip, yerine vurgusuz halini koymaktadır. örnek égğ --> egg

"strip" Türkçe kelime anlamı: soymak, üstünü çıkarmak.

(diğer başlıklarda anlatıldığı üzere) Unicode metadata'larında hangi karakterlerin üzerinde hangi aksan işareti olduğu bilgisi tutulmaktadır. yani; v̀ için, "v ve aksan işareti" olduğu Unicode DB'de mevcuttur.

## 📌 java.util.Locale

bu sınıf JRE içinde gelir. içinde static olarak en sık kullanılan değerler hazır bulunur:

```java
static public final Locale ENGLISH = createConstant("en", "");
static public final Locale UK = createConstant("en", "GB");
static public final Locale US = createConstant("en", "US");
```

Locale 3 parametre alır:

```java
Locale(String language, String country, String variant);
```

sağdaki iki parametre opsiyoneldir. eğer atılmazsa boş olarak kullanılır. yani "locale.getVariant()" boş döner.

Locale sınıfının aldığı üç parametre ağız/lehçe/aksan kombinasyonu değildir. dil, kullanıldığı ülke ve türevi olarak algılanır. örnek:

- "Azerbaycan Türkçesi", Türkçe'nin türevi değildir. kendisi bir dildir. ve ülkesi Azerbaycan'dır.

## 📌 alfabe (⟷ alphabet)

bazı alfabeler:

- `Latin` alfabesi (ABCDEF karakterleri)

- `Yunan (⟷ Greek)` alfabesi

- `Kiril (⟷ Cyrillic)` Alfabesi (`Rusya`, `Bulgaristan`, `Sırbistan` ve etrafında kullanılıyor)

- `Ermenice (⟷ Armenian)` alfabesi

- `Gürcü (⟷ Georgian)` alfabesi

- `Arap` alfabesi

- `İbrani alfabesi (⟷ Hebrew)`

- `Tibet` Alfabesi (`Tibetçe` için)

- `Thai` Alfabesi

Ek notlar:

Japonya, Çin ve Kore'de kullanılan alfabelerde birbirinden farklıdır.

Arapça temelli dillerde alfabe hep aynı. bazı karakterler bazı dillerde yok yada fazlası var fakat temelde aynı karakterler kullanılıyor. yani farklı ülkeye gittiğinizde kelimeleri okuyabiliyor fakat ne anlama geldiklerini bilemiyorsunuz.

Arapça temelli dillerde, bazı karakterler yanındaki karakterlere göre şeklini değiştirmektedir (yanındaki karakterlere bağlanmaktadırlar). bu sebeple font viewer'larda gördüğümüz font'lar ile cümle içinde gördüğümüz karakterler aynı olmayabilirler.

## 📌 Yazı sistemi (⟷ writing system ⟷ script)

yazı sistemi; bir dilin yazı stilidir. örnek: Arapça script'ini ele alalım. Bu script'in baz alındığı birçok dil vardır: Iraqi, Lebanese, Egyptian... Arapça baz alınan dillerin %100 Arapça yazı stili kurallarına uymayacağı zaten ortadadır. Zaten tamamen aynı olsalar, o zaman çatallanma meydana gelmezdi.

Bazı diller birden fazla yazım sistemini desteklemektedir. Örnek: `Korece (⟷ Korean)`, `Japonca (⟷ Japanese)`. Bu dillerde aynı cümleler farklı yazı sistemi ile yazılabilmektedir.

Daha spesifik örnek verirsek: Japonca'da aşağıdaki yazım sistemleri vardır:

- Hiragana: Japonca kökenli kelimeleri yazmak için kullanılır.
- Katakana: Yabancı kökenli sözcüklerin yazmak için kullanılır. Örneğin "otel" kelimesinin karşılığı sadece Katakana'da vardır. Hiragana'da böyle bir kelime yoktur.
- Kanji: Çince'den geçen kelimeler için kullanılır. (Kanji kelime anlamı: Çin Harfi (Çince yazı karakterlerine Japoncada verilen isim)).

Japonca'da aynı cümle içerisinde dahi birden fazla yazım stili olabilir.

## 📌 Romanization (⟷ romanizasyon ⟷ romantizasyon)

Japonca, Yunanca gibi dillerin Latin karakterlerle yazılmasıdır.

## 📌 Romanji

Japonca'nın romanizasyon ismidir.

Romanji'de, Japon yazı sistemindeki her karaktere ve heceye karşılık gelen latin karakterleri vardır. Japonca'da yazılacak cümle, aynı karakterlere denk gelen latin karakterleri ile yazılır. Romanji'ninde kendi içinde türevleri vardır.

## 📌 Traditional vs Simplified Chinese

Çince zor dil olduğundan her karakter daha basit bir görüntü haline getirilip, ortaya daha basit okunabilen karakterler yaratılmıştır. Bu yeni sisteme Simplified Chinese ismi verilmiştir. Simplified Chinese'de her karakter değişmemiştir.

Japonca da benzer durum yok, çünkü Japonca Simplified Chinese'e göre dahi daha kolay anlaşılır bir görüntüsü var.

## 📌 numbers

### 📌📌 digit

Türkçe'de matematik'teki "basamak", "hane" olarak çevrilir.

13'lük sayı siteminden rastgele bir sayı alalım: 111A

en sonda olan A (10 sayısına denk gelmektedir) tek başına bir basamak hanedir.

### 📌📌 Unicode terminolojisi

Unicode karışıklığa sebep olmaması bazı terimleri şu şekilde kullanmaktadır:

- digit: only numerical digits

- Arabic digits yada Arabic-Indic Digits: Arabic script'e ait rakamlar

- Western digits yada Latin digits yada European Digits: 0-9 rakamları

- Indic digits: hint temelli dillerde kullanılan rakamlardır.

### 📌📌  Doğu Arap rakamları (⟷ Eastern Arabic numerals ⟷ Arabic–Indic numerals ⟷ Arabic–Hindu numerals ⟷ Arabic Eastern numerals ⟷ Indo-Persian numerals)

Arapça tabanlı kullanılan bir sayı sistemidir. Arap ülkeleri arasında da farklılık gösteriyor.

Arapça yazılım geliştirirken Arapça sayı girişi olan input'larda Arap ülkelerindeki kullanıcılar sadece `Doğu Arap rakamları` değil aynı anda klavye aracılığı ile `Arap rakamları` karakterlerine (0-9) geçiş yapabiliyor. bu durumda yazılımcıya ek iş yükü binmiş oluyor.

### 📌📌 Arap rakamları (⟷ Hindu–Arabic numerals ⟷ Indo-Arabic numerals ⟷ Arabic numerals ⟷ Western Arabic numerals ⟷ Western numerals)

0-9 kullanan rakam karakterleridir.

### 📌📌 Roma rakamları (⟷ Roman numerals ⟷ Romen rakamları)

Güncel zamanlarda dijital ortamlarda kullanım alanları:

- Kitapların başlangıçlarındaki sunuş ve ön söz gibi bölümlerdeki başlıklarda
- makalelerin sayfalandırılmasında
- dizi ve oyun gibi seri yayımlanan isimlerde: Rocky II, Grand Theft Auto V...
- Yüzyıl adlarında: XIX. yüzyıl gibi.
- Aynı adlı padişah ve kralların ayrılmasında: II. Ahmet, XVI. Louis gibi...
- Tarihi olaylarda: II. Viyana Kuşatması, I. Dünya Savaşı gibi.. (burada lokalizasyon gerekmez. fakat roma rakamları static string olarak tutulur fakat yine de not olarak belirttim)
- apartman numaralarında (bazen sadece bir kısmında)
- OCR ile okunan kitapların anlamlandırılmasında

Temel kurallar:

| Symbol | I | V | X  | L  | C   | D   | M    |
|--------|---|---|----|----|-----|-----|------|
| Value  | 1 | 5 | 10 | 50 | 100 | 500 | 1000 |

sıfır ve basamak kavramı yoktur.

büyük sayının sağına eklenenler toplanır, sola eklenenler çıkarılır. örnek:

- XIV = 14
- XXIV = 24

karakterler üzerlerine düz çizgi de alabilirler. bu durumda o sayının 1000 katı temsil edilir.

Roma sayılarında aynı sayı farklı ifadelerle de gösterilebilir (denk gelmektedir).

### 📌📌 Çin ve Arapça'da sayılar

Arapçada sayıların görünümü farklı fakat sistem olarak "Arap rakamları (0-9)" ile aynı. yani her basamak bir karaktere denk geliyor.

Çince temelli dillerde de sayılar bölgeden bölgeye farklılıklar gösterir.

Çince'de ise yazım sistemi de farklı. örnek üzerinden gidersek; "Mandarin Çincesi"nde her karakter bir sayıyı ifade etmiyor. örnek:

- 四十二 42 iken
- 九十 90 'ı temsil ediyor. görüldüğü gibi sayı daha büyük olmasına rağmen daha az karakterle temsil ediliyor.

Özellikle programatik olarak __sıralama__ istendiğinde çok zor durumlarla karşılaşılmaktadır.

## 📌 CJK

Chinese, Japanese ve Korean dilleri için ortak terimdir. Bazı dil bilimciler Vietnamese (⟷ Vietnamca) dilini de bu gruba katarak, __CJKV__ kısaltmasını kullanırlar.

CJK karmaşıklığını gidermek için (mapping'lerin daha düzenli olması için) Unicode projesinde ayrı gruplar çalışmaktadır. bu çalışmalar "__Han Unification__" olarak isimlendirilmektedir.

'Çin yazı karakterleri'ne '__Hanzi (⟷ Han characters)__' deniliyor.

## 📌 Çin

Çin'de birçok farklı türemiş dil mevcuttur. Ülke resmi olarak __"Mandarin"__ isimli dili kullanmaktadır. "Mandarin" Çin'de en fazla kullanılan dildir. Cantonese ve Wu dilleri diğer en çok kullanılan diller arasındadır.

Sadece "Çince" denildiğinde "Mandarin" kastediliyordur.

Mandarin, Cantonese arasında farklar: bazı karakterler, bazı gramer yapıları, bazı tonlamalar, bazı kelime hazinesi. Mandarin metni okunduğunda, sadece Cantonese bilen biri bu cümleleri anlayamıyor.

Çin'de yüzlerce dialect vardır.

__Classical Chinese (⟷ Literary Chinese)__ hala kullanılan ve İsa zamanına dayanan bir tarihi vardır.

Kore ve Japonya'da böyle bir durum söz konusu değildir, çünkü tek bir dil var fakat çok çok ufak bölgesel farklılıklar görülebilir.

## 📌 Arapça

__Klasik Arapça (⟷ Classical Arabic ⟷ Quranic Arabic)__ 7 ve 19 uncu yüzyıllar arası kullanılan Arapçadır. Şu anda daha çok dinsel eğitimlerde ve ibadetlerde kullanılmaktadır.

__Old Arabic (⟷ eski Arapça)__ klasik Arapça'dan önce kullanılması ile ölen bir dildir.

__Modern Standard Arabic (⟷ MSA ⟷ Standard Arabic ⟷ Modern Written Arabic ⟷ MWA ⟷ tr:Fasih Arapça)__ birçok ülke tarafından resmen tanınan bir Arapça çeşididir. Günümüz sistemlerine uygun şekilde tasarlanmaktadır. "Klasik Arapça" temelli bir dildir. Arapçada bu dile "El-Arabiyye el-fusha" denir.

__tr:Ammi Arapçası (⟷ Ammiya Arabic)__ halkın kendi içinde konuştuğu Arapça'dır ve her Arap ülkesine göre değişiklikler gösterir. Halk Ammiya dilini konuşurken, yazılı mecralarda ve medyada MSA kullanılır.

"Ammiya", Arapça bir kelimedir. İngilizce'ye ve Türkçe'ye direk okunuşu ile çevrilmiştir. Ammiya'nın kelime anlamı: çoğunluğun dili.

Ammi Arapça'sına örnekler:

- Eastern Arabic - East bölgelerinde kullanılan Arapça.
- Mashriqi Arabic (⟷ Mashriqi Ammiya) - Mashriqi bölgelerinde kullanılan Arapça.
- Lebanese Arabic - Sadece Lübnan'da kullanılan Arapça.
- Syrian Arabic - Sadece Suriye'de kullanılan Arapça.

__Literary Arabic (⟷ Edebi Arapça)__ terimi biraz genel bir terim olduğundan kesin referans ettiği bir karşılığı yoktur. Herkes farklı anlamda kullanabilir.

Tüm Arapça dillerinin alfabeleri aynıdır.

## 📌 alphanumeric (⟷ Alfanümerik)

Apache-commons-lang StringUtils sınıfına göre; sadece isAlpha():

- işaret ve boşluk karakteri içermemeli. yani sadece "letter" içermeli. letter bilgisi Unicode'un "general category" bilgisinden gelmektedir.

- sadece Unicode karakteri içermeli

- sayı içermemeli

isAlphanumeric(); isAlpha'nın sayı kuralını ignore eder.

## 📌 Tipoğrafik İşaretler

resmi dilin noktalama işaretleri dışında kalan karakterlerdir. aslında birer anlama gelen şekillerdir. @, &, # gibi karakterlerdir.

bir dilde tipoğrafik olan karakter başka dilde tipoğrafik olmayabilir.

## 📌 font

her font belli bir karakter setini desteklemektedir. Eğer ekranda gösterilmesi gereken karakter seti o sıra kullandığımız font'ta tanımlı değil ise; web tarayıcıda isek; web tarayıcının kendisi, native bir uygulamada isek; OS'un kendisi karar verip, görüntülenemeyen karakter için şunu yapacaktır:

- default bildiği font, yada farklı bir font ile o karakteri gösterecektir

- sadece o karakteri hiç göstermeyecektir (boşluk yada hiç)

- hata fırlatacaktır (bu hiç tercih edilmeyen yöntem. neredeyse hiçbir program böyle bir şey yapmıyor)

- fallback karakterini gösterecektir.

- böyle bir karakter gösterecektir: (bir dikdörtgen içinde sayılar ve harfler oluyor)

  ```text
   _____
  | 1 8 |
  | 2 f |
   ¯¯¯¯¯
  ```

   Bunun anlamı bu karakter: 16'lık gösterimde 182f'dir.

## 📌 glyph (⟷ tr:glif)

işaret, karakter, sembol, sayı, emoji veya herhangi bir amaç için çizilebilen/yazılabilen her ifade ayrı ayrı birer glif olarak kabul edilir. Unicode'da boşluk hariç her şey ayrı ayrı birer glif'tir.

"a" karakterini birçok farklı biçimde yazabiliriz/çizebilir: köşesi daha kalın, daha olan... Bu çizeceğimiz her kombinasyon ayrı ayrı birer glif'tir.

## 📌 replacement character (⟷ missing glyph ⟷ replacement glyph ⟷ notdef glyph)

- � (içinde soru işareti olan altıgen) karakteri simgesindedir.
- bu karakter UTF kodlamasında hata alındığında, hata alınan karakter yerine bu karakterin konulması istenmektedir.
- code point'i U+FFFD'dir.
- Bu Unicode'un bir standartıdır. bu sebeple her font'un fallback karakteri bu codepoint'e tekabül etmez. fakat her font dosyasında bir fallback karakteri olması beklenir.
- Unicode'un "unsupported character" dökümanına bakıldığında: http://unicode.org/faq/unsup_char.html her fallback karakteri için "replacement character" kullanılmaması önerilmektedir. örneğin;
  - White_Space property içeren fakat tanımlanmamış karakterler yerine boşluk gösterilmeli
  - default-ignorable character'ler tanımlanmamışsa yerine hiçbir şey gösterilmemeli

## 📌 Tofu

kelime anlamı: soya peyniri

replacement character yazılımlar tarafından her zaman Unicode'da belirtilen şekilde olmamaktadır. genelde boş kare yada içi noktalarla dolu kare işareti gösterilmektedir. işte bu işarete yazılımcılar Tofu demektedir. kelime anlamından gelmesi ona benzemesindendir.

## 📌 NOTO

NO-Tofu'dan üretilen bir kelimedir.

Google'ın geliştirdiği font ailesinin ismidir. Unicode'da olan dillerin tümünün alfabesini karşılamak için tasarlanmıştır. alfabeler dışında ekstra karakterlerde vardır, fakat öncelik dilin alfabeleridir.

## 📌 glyph

kelime anlamı: kabartma/oyma.

bir karakterin yada herhangi bir olgunun, simgelenmiş halidir (representation of character or thing).

bazen bazı karakterlerin birden fazla glyph karşılığı olabilir. çünkü karakterler, kültürden kültüre (digital ortamda font'tan font'a) farklı görünebilirler. bu sebeple; bir font dosyasındaki her karakter, birer glyph'tir. fakat her "UNICODE karakteri" bir glyph değildir. UNICODE nasıl görüntüleneceği ile ilgilenmez. görüntü glyph'tir. bir karakter, görüntüsü olmadan bir şey ifade etmez. "karakter" tek başına sadece bir tanımdır.

## 📌 eng:grapheme (⟷ tr:grafem)

dil biliminde; bir yazı sisteminin en küçük birimidir. Alfabedeki karakterler buna örnek verilebilir.

## 📌 yüzde işareti (⟷ percent sign)

% işareti ile temsil edilir. İngilizce'de yüzde 5; 5% ile yazılırken, Türkçe'de %5 ile yazılır.

‰ işareti "per mile" olarak isimlendiriliyor. fakat 1000'i temsil ediyor. "per mil" karakterindeki "mil" kelimesi uzunluk birimi olan mil'den (1 mil = 1 km değil! ve kara ve denizde farklı değeri vardır) gelmemektedir.

45% = 0.45 = 45/100 eşitliğini göstermek için kullanılır. + - işaretleri gibi her istenilen yere konulamaz. özel bir gösterim karakteri olduğu için bazı özel durumlarda kullanılır. örnek:

- 45% = 0.45 // tek başına kullanıldığında bu anlama geliyor.

- 200 − 10% = 180 //burada 10% ile; 200'ün yüzde 10'u alınmıştır. yani; 200-20=180.

- 200 − 10% + 3 //hatalı kullanım.

## 📌 dosyayı byte array olarak ekranda okuma

Linux komut satırı uygulamaları ile yapılabilir. örnek olarak içeriği aşağıdaki gibi olan bir dosya için;

123456789123456789

- 16'lık tabanda görebilmek için:

```sh
hexdump "myfile.txt"
```

output:

```text
0000000 3231 3433 3635 3837 3139 3332 3534 3736

0000010 3938 3231 3433 3635 3837 3139 3332 3534

0000020 3736 3938 3231 3433 3635 3837 0a39

000002e
```

- 8'lik tabanda görebilmek için:

```text
0000000 031061 032063 033065 034067 030471 031462 032464 033466

0000020 034470 031061 032063 033065 034067 030471 031462 032464

0000040 033466 034470 031061 032063 033065 034067 005071

0000056
```

-- yukarıdaki çıktılarda ilk sütun aralıklardır. sadece output'u gruplandırmak için komut satırı uygulamasının kullandığı gruplama tekniğidir.

-- bir satırda sadece # karakteri var ise; bunun anlamı üstteki satır ile birebir aynı olduğudur. -v parametresi ile bu durum iptal edilebilir.

-- 8'lik tabanda output'ta gösterilen bir değer max 7 değerini alabilir. 7 için 3 bit'e ihtiyacımız var. 1 byte 8 bit olduğundan, 8lik tabanda 1 byte'ı göstermek için 3 karaktere ihtiyacımız var. ve en yüksek öncelikli karakterde gereksiz boş yer kalmaktadır. örnek: octal(177) = hex(7F) = binary(1 111 111). örnekte gözüktüğü gibi output'u en erimli görebilmek için 4 bite denk gelen 16'lık taban kullanılmalıdır. octal gösterimde 2'inci byte, örnekte 177'yi en az 1 arttırarak 200'a çıkaracaktır.

-- bazı text editörler satır sonu karakterlerini (örnek 0a) otomatik atarlar ve ekranda (son kullanıcıya - GUI'de) satır sonunu göstermeyebilirler.

-- bazı editörler BOM karakterini dosyanın başına otomatik atarlar.

## 📌 text dosyasında encoding'i otomatik algılatma

text editörler dosyayı açtıklarında deneme yanılma yöntemi ile doğru formatı bulmaya çalışırlar. örnek UTF-16 mı diye sorguladıklarında, her karakter için bir karaktere ulaşılabiliyor ise o formatı geçerli kılarlar. fakat bu her zaman %100 doğru encoding'i bulabilme durumu söz konusu değildir.

## 📌 Java char

Java'da "char" 2 byte uzunluğundadır. unsigned 16-bit sayı olarak tutulur. bu sebeple Unicode'daki U+FFFF üstü karakterler Java'nın char tipi tarafından desteklenememektedir.

tanımlamak için: char myChar = '\u0003'; yazılır. Bu sebeple int'e cast işlemi sonucu "3" değerini alırız.

Character sınıfı char tipini constructor'dan alır ve wrap eder. Character sınıfı da char'ı wrap ettiği için yine  U+FFFF üstü karakterleri destekleyemez.

Aşağıdaki gibi tanımlama yapabiliriz:

```java
char myChar = '\u0003';

Character myCharClass = new Character(myChar);

myCharClass.hashCode(); // int 3 değerini döndürür

myCharClass.compareTo(anotherCharacter); //eşitlerse 1 değillerse 0'ı döndürür. eşitlik Unicode code-point'i ile yapılır.
```

Character sınıfının metotları Unicode'un her karakter için belirlediği property'leri de döndürmektedir. örnek: lowercase, unicodeblock... metot isimleri direk anlaşılmaktadır. tek istisna getType() metodudur ki bu "genel kategori" bilgisini döndürmektedir.

Char karakteri 16-bit olduğundan tüm Unicode desteklenememektedir. daha sonraları char boyutu yükseltilmek istenmiş fakat bu köklü değişiklikler gerektirdiğinden string alternatifi farklı sınıflar sunulmuş, ve Character sınıfına bazı destekleyici metotlar eklenmiştir. örnek:

charCount(int codePoint) metodu: codepoint'imizi int olarak gönderiyoruz. eğer U+FFFF 'ten büyükse (yani 16-bit'e sığmayacaksa) 2 dönüyor. diğer durumlarda 1 dönüyor.

boolean isSupplementaryCodePoint(int codePoint) metodu: charCount metodu ile ynı şeyi belirtmektedir fakat boolean ile dönüş yapar.

Character sınıfı içindeki birçok aynı isimde metot hem int (codepoint) yada char alacak şekilde ayarlanmıştır. char alan metotlar eskiden vardı, int alanlar ise sonradan geliştirildi. int almalarının sebebi codepoint'i U+FFFF üstü değerleri int olarak verebilmemizi sağlamaktadır.

Character.toCodePoint(char high, char low) --> char array gönderiyoruz. elimizdeki 2 elemanlı bir char[]'in ilk ve ikinci elemanını buraya yolluyoruz. char[2] 32-bittir. buraya yolladığımız karakter bir code-unit'tir. yani (int) char[2] bize codeunit vermelidir. bu metot ise bize onun code point'ini döndürmektedir.

Character.toChars(int codePoint) --> codepoint'e karşılık char[2] yada char[1] döndürmektedir. döndürülen bu char[]; (int) char[2] bize codeunit verir.

Character.isSurrogatePair(char high, char low) --> toCodePoint metodu ile aynı parametreleri alıyor. toCodePoint'in iki elemanlı dizi dönüp dönmeyeceği bilgisini veriyor.

## 📌 Java String

String kelime anlamı: sicim (bir çeşit iplik)

String kendi içinde char[] tutar. her "char" kendi içinde Unicode codepoint'i ile tutulur. String'in mimarisini constructor'larına bakarak kavrayabiliriz:

String constructor'u eğer String alıyorsa o zaman charset parametresi istemez. örnek: new String("abc"); çünkü yolladığımız string'i karakterlere böler ve char array'e atar. zaten her karakter artık bellidir. bu karakterlerin Unicode'da tanımlı olması şarttır. zaten yolladığımız string'de bir Java string'idir. bu işlem sonucu kopyalama işlemi gerçekleşir.

"new String(byteArray, "UTF-8");" ile byte-Array'ı char-array'e çevirmesi için karakter seti bilgisini yollamamız gerekir. eğer yollamazsak default OS'un karakter-encoding'ini kullanacaktır. burada byteArray aslında UTF-8 encoding'inde olduğunu belirtiyoruz, ve constructor'a bize Java String'i dönmesini istiyoruz. Java string'inin ne tuttuğu önemli değil.

String constructor'u eğer unmappable karakter okursa bunu default/özel bir karakter ile replace eder ve bu şekilde char[] oluşturur.

String'in toString metodu dışarıya string'i verirken kendini return eder. bu string'i bir yere basmak için bir stream'e yollamak lazım. bu stream bunu alırken nasıl okuduğu önemlidir. stream, isterse karakter karakter okur, isterse byte byte okur.

"text".getBytes("UTF-8"); ile istediğimiz çeşit encoding'de byte array olarak text'i döndürür.

yukarıdan görüldüğü gibi; bizim için charset ancak şu durumlarda gerekiyor:

- string'den dışarı byte-array çıkarmak istediğimizde
- string'i byte-array ile initialize etmek istediğimizde

bunun dışında, Java kendi içinde nasıl tuttuğu önemli değil. her sürümde farklı şekilde tutuyor olabilir. emin olduğumuz bir şey var: `JVM` bizim için her karaktere tekabül eden codepoint'ler ile string'imizi tutuyor.

- örnek 1:

  yukarıda anlatıldığı gibi;
  - Java char'larda codepoint tutuyor
  - string ise char array tutuyor.

  şimdi şöyle bir örnek üzerinden gidelim:

  sunucumuza CP1250 formatında request gelsin.

  HTTPServletRequest.getParameter("name") bize beklenmedik karakterler döndürüyor olsun. önce bu string'in bize gelmeden hangi aşamalardan geçtiğini trace edelim:

  - web server'a gelen istek servlet tarafından karşılanıyor.
  - servlet'e gelen istek byte array olarak karşılanır
  - arada filter'larımız olabilir
  - daha sonra getParameter("name") bize string olarak (ilgili alanı, sadece "name" kısmını) döndürür.

  getParameter bize hatalı döndü. çünkü Java byte-array'i string'e çevirirken, byte-array'in CP1250 olduğunu bilmiyor ve farklı bir encoding'de okuyor. Doğal olarak; String'e yanlış çevriliyor. Örneğin; en başında belirttiğimiz kurallar gereği artık string'imiz: byte-array barındırmıyor, Code-point barındırıyor. Dolayısı elimizde ile artık yanlış code-point'li karakterler var. Artık string'imizi CP1250'e çevirme gibi bir durumumuz yok. Bu sebeple şöyle bir çözüme gidilebilir:

  Filter oluşturulur:

  ```java
  public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {

    if (request.getCharacterEncoding() == null) { //Güncel HTTP standartlarında encoding bilgisi de gönderilebiliyor.

        request.setCharacterEncoding("cp1250");
    }
    chain.doFilter(request, response);
  }
  ```

  Filter'da encoding set edilince artık, HTTPServletRequest.getParameter("name") metodu byte-array'den bize string döndürürken CP1250 encoding'ini kullanacaktır.

  HTTPServletRequest.getParameter("name", "cp1250") gibi bir metot sunmuyor. bu sebeple yukarıdaki yöntemi kullanmak gerekli.

- örnek 2

  Console'dan karakter okuduğumuz zaman OS'un default encoding'i ile karakterler okunur. işlemi trace edersek:

  - console uygulaması input olarak karakteri byte'a çevirir.
  - console uygulaması byte'a çevirirken (özel ayar belirtilmemişse) OS'un default encoding'i ile karakterleri byte'a çevirir.
  - bu byte'ları array olarak programımıza yollar.
  - programımız stream'den (özel ayar belirtilmemişse) OS'un default encoding'i ile byte'ları string'e çevirir.

  Kod içerisinde (örneğin java) stream'in encoding'ini değiştirmek istersek şu kodu kullanabiliriz:

  ```java
  Scanner console = new Scanner(new InputStreamReader(System.in, "cp1250"));
  ```

## 📌 Modified UTF-8 (⟷ MUTF-8)

`JVM`'in kendi içindeki sınıflarda kullandığı `UTF-8` türevi encoding'dir. Dışarıya çıktı/girdi alan sınıflarda kullanılmaz. Dolayısı ile uç durumlar haricinde yazılımcıyı ilgilendiren bir durum değildir.

`MUTF-8` encoding'inin kullanıldığı yerler:

- `DataInput` sınıfı içindeki `String`, `readUTF()` `MUTF-8` okur ve bize `String` döndürür.

- `JVM` serialize/deserialize yaparken `MUTF-8` kullanır.

## 📌 Console'a basarken yaşanan karakter problemleri

Bir log attığımızda Eclipse console'u veya aynı text'i dosyaya attığımızda farklı biçimlerde ekranda görünebilir. bu tarz sorunların sebebi her Reader'ın (console, text editor...) okuduğunda encoding'i bilmemesinden kendi default varsaydığı encoding'de olduğunu varsayıp okuyup ekranda göstermesidir.

## 📌 code page

Unicode çıkmadan önce her OS belli karakter kümelerini gruplayarak isimlendirmekteydi. bu gruplara code page deniliyor.

code page bir encoding değil, sadece karakter setidir. örneğin; sistem hangi karakter seti destekliyor dendiğinde, X code page'ini destekliyor denilirdi.

bir code page içerisinde, her karaktere tekabül eden bir sayı vardır. bu sayıya "code point" denir. GUI aracılığı ile bir textbox'a metin girerken, ALT+123 şeklinde bir tuşlama yazdığımızda, OS'un varsayılan code point'i üzerindeki 123'üncü sayıyı çağırmış oluyoruz. ALT+123'a "alt code" adı verilir.

çoğu zaman codepoint'ler 256 karakter uzunluğunda tutulur. bu şekilde ilk karakter ve son karakter, bir sayı ile temsil edilmek istendiğinde hepsinin byte aralığı sabit olur (1 byte). aslında bu bilgiyi byte olarak dosyaya yazdığımızda encoding görevi görmüş olacaktır. fakat code page encoding standartı değildir, çünkü code point'in nasıl bellekte tutulacağının bilgisini içermez. byte olarak kaydetmek en mantıklısıdır ve çoğu text editor, bir dosyayı bir code page standartlarında kaydettiği zaman, byte olarak kaydeder. fakat bunu BCD'de kaydedebilir, yada farklı yöntemlerde tercih edebilir. bu yazılımın kendisine kalmıştır.

MS Windows kendi API'lerinde code page'lerin tümünü bu şekilde referanslıyor:

aşağıdaki tabloda UTF-8'i de görmekteyiz. bunun sebebi; o code page'in barındırdığı tüm karakterler, UTF-8 'in desteklediği tüm karakterlerdir.

aşağıdaki linkte "mac" keyword'ü içeren satırlar mevcut. bunun sebebi; MacOS'te kaydedilmiş dosyaları, MS Windows tarafında okuyabilmektir.

https://docs.microsoft.com/en-us/windows/desktop/Intl/code-page-identifiers

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 GILT

Globalization, Internationalization, Localization and Translation'ın kısaltmasıdır.

yazılım geliştirme sürecinde dil desteği için kullanılan terimlerdir.

### 📌📌 Translation (⟷ T9N)

bir yazılımın text'lerinin başka bir dile çevrilme işlemidir.

### 📌📌 Localization (⟷ l10n)

yazılım geliştirme sürecinde, içeriklerin ve bilgisayar yazılımlarının belirli bir coğrafyaya ya da etnik topluluğa özgü pazar ya da coğrafi bölgede geçerli yerel dilsel ve kültürel özelliklere uyarlanmasıdır. dolayısı ile; T9N'u ve daha fazlasını kapsar.

### 📌📌 Internationalization (i18n)

bir yazılımın birden fazla dili göre destekleme işlemidir.

### 📌📌 Globalization (⟷ g11n)

i18n'i ve daha fazlasını kapsar. i18n olaya sadece metin bazında bakarken, g11n; kültürel açıdan da coğrafyalara uyumlu olabilmesini kapsar.

## 📌 a11y

erişilebilirlik özelliğini kazandırma işlemidir. GILT grubuna dahil değildir.

## 📌 locale

Sadece metni çevirmek yetmiyor, sayfa boyutu (A4 gibi) bile ülkeden ülkeye değişebiliyor. Bu konunun detayları için buraya bakılmalı: [file:"unix_likes_commands.md" - title:"locale"](./unix_likes_commands.md#locale)

## 📌 i18n kütüphaneleri

"JQuery.i18n" kütüphanesi çoktandır güncellenmiyor. JS için "i18next" kütüphanesi inceleyelim.

ilgili dildeki string karşılığını döndürüyor:

### 📌📌 Plural (⟷ çoğul)

İngilizce'de 1 farklı plural form varken, Arapça'da 5 farklı form var. Örneğin:

- 1 pencil
- 2 pencils
- 3 pencils

Oysa Arapça'da:

- 0
- 1
- 2
- 3-10 arası
- 11-100 arası
- 100 ve sonrası

için ayrı ayrı formlar mevcut. Yani bu sayılara göre "pencil" kelimesinin formu değişebiliyor.

Dolayısı ile aşağıdaki gibi placeholder tekniği ile dil desteği sağlayamayız:

```js
var message = "$1 pencil";
$.i18n(message, '3');
```

Her dildeki plural form bilgisini framework kendi içinde tutuyor ve böylece i18next bu metodu sunuyor:

```js
i18next.t('key', {count: 100});
```

Dolayısı ile language dosyamız aşağıdaki gibi olmalıdır:

```json
{
  "key_zero": "zero", // only for: i18next.t('key', {count: 0});
  "key_one": "singular", // only for:  i18next.t('key', {count: 1});
  "key_two": "two", // only for:  i18next.t('key', {count: 2});
  "key_few": "few", // i18next.t('key', {count: 3}); or i18next.t('key', {count: 10});
  "key_many": "many", // i18next.t('key', {count: 11}); or i18next.t('key', {count: 12});
  "key_other": "other" // i18next.t('key', {count: 101}); or i18next.t('key', {count: 999});
}
```

Yukarıdaki "few" ve "many" anahtar kelimeleri framework'ün belirlediği kelimelerdir. Başka framework'lerde farklı anahtar kelimeler görebiliriz.

__Common Locale Data Repository (⟷ CLDR)__, Unicode ekibinin public olarak yayınladığı çoğul kelimelerin sayılara göre nasıl değiştiğini açıklayan kurallar dökümantasyonudur. Hem dökümantasyon hem de parse edilebilmesi için XML olarak yayımlanır. Bu dökümantasyona göre parse eden program kütüphaneleri mevcuttur.

CLDR plural form bilgileri gibi birçok bilgi tutmaktadır. örnek:

- Locale-specific patterns for formatting and parsing: dates, times, timezones, numbers and currency values, measurement units...
- Translations of names: languages, scripts, countries and regions, currencies, eras, months, weekdays, day periods, time zones, cities, and time units, emoji characters and sequences (and search keywords),...
- Language & script information: characters used; plural cases; gender of lists; capitalization; rules for sorting & searching; writing direction; transliteration rules; rules for spelling out numbers; rules for segmenting text into graphemes, words, and sentences; keyboard layouts...
- Country information: language usage, currency information, calendar preference, week conventions,...
- Validity: Definitions, aliases, and validity information for Unicode locales, languages, scripts, regions, and extensions, ...

### 📌📌 Gender (⟷ cinsiyet)

"`His name`", "`her name`" şeklindeki ifadeler.

### 📌📌 Ordinal numbers

"`1st`", "`2nd`", "`3rd`" şeklindeki ifadeler.

## 📌 Message documentation for Internationalization frameworks

`en.json`, `tr.json` gibi dosyalar ile aynı dizinde `information.json` dosyası bulunması önerilir. bu dosya `runtime` sırasında okunmaz/kullanılmaz. translator'ların okuması için bir dosyadır. key'ler yetersiz olduğunda translator bu dosyaya bakıp ne yazması gerektiğini çıkarır. örnek "`information.json`" dosyası:

```json
{
 "appname-title": "Application name",
 "appname-sub-title": "explanation of the application",
 "appname-about": "About this application text"
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Bir sayıyı metne çevirme veya metinden sayıya çevirme

örnek:

- 105 --> yüz beş

- 1208 --> bin iki yüz sekiz

bu işlemler i18n gibi kütüphanelerin görevi değildir. bu sebeple ekstra kütüphane kullanmak gerekiyor. 2018 yılı itibari ile hala bu işi yapan düzgün ve multi-language kütüphane mevcut değil.

bunu yapan kütüphaneleri genelde "number to word" terimleri ile arama yapmak gerekiyor. Genelde her dil için ayrı kütüphane kullanmak gerekiyor. Bazı dillerde yine kompleks durumlar söz konusu olabiliyor. Örneğin; Fransızca'da sayıların bazıları yazılırken veya okunurken, Türkçe ve İngilizce'deki gibi direk sayı değerleri ile ifade edilmeyip, başka sayıların toplamı olarak ifade edilmektedir. Örnek: "90", "2 40 10" olarak yazılıyor ve söyleniyor (2*40+10 olduğu için).

## 📌 number and counting system

Bazı dillerde sayıları listelemek yerine bir objeyi saymak için sayı kullandığımızda, saydığımız obje farklı bir kelimeye veya kelimelere bürünmektedirler. Bu konu için İrlandaca (⟷ Irish) örnek verilebilir. İrlandaca'da:

- bir sayıyı bir şeyi saymak için kullanıyor veya telefon numarası verirken kullanıyor isek, sayıların önüne farklı bir kelime gelmektedir.
- "Bir kalem" yerine "tek kalem" gibi ibareler şarttır.
- "üç kalem" gibi ibarelerde, "kalem" kelimesi bazen değişebilmektedir ve bu sadece bazı sayılar için olmaktadır (değiştiği zaman her sayıda değişmemektedir.)
- Bu durumlardan çok daha fazlası mevcut. Örnek: <https://www.bitesize.irish/blog/counting/>
- Başka dillerde farklı kompleks durumlar olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

### 📌📌 Type Family

karakterlerinin yapısını belirleyen kuralları içerir. örnek: Helvetica

### 📌📌 Typeface (⟷ typeface family ⟷ font family)

bir 'type family'ye ek olarak; kalın, italik olup olmaması bilgisini birlikte içerir. örnek: Helvetica Bold.

### 📌📌 Font

Typeface'e ek olarak; boyut bilgilerini içerir. örnek: Helvetica Bold 12px

### 📌📌 font-face

tipografi'de geçen bir terim değildir. sadece CSS için font keyword'üdür.

### 📌📌 Serif

her karakterin uç kısımlarına ufak bir çizgiler koyulur. bu çizgiler Serif'tir. Sans-Serif ise bu çizgiler olmadan ki karakterleri temsil ediyor.

### 📌📌 OpenType vs TrueType vs PostScript

alternatif font dosya saklama formatlarıdır.

### 📌📌 ClearType

Microsoft'un ekranlarda tüm fontları daha ince göstermek için geliştirdiği teknoloji. tüm fontları farklı bir biçime sokuyor. fontlar tasarlanırken özellikle ClearType'a uygun şekilde tasarlanırsa, ClearType daha başarılı oluyor. ClearType genel olarak LCD ekranlarda daha smooth bir görüntü yaratıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 NLP (⟷ Natural Language Processing ⟷ Doğal Dil İşleme)

"doğal dil" terimi doğal süreçlerle oluşmuş dilleri temsil etmek için kullanılır. Örneğin İngilizce, Türkçe birer doğal dildir. Bunun tersi olarak __constructed language (⟷ conlang)__ terimi kullanılır. Bazı kaynaklarda __conlang__ yerine, __artificial (⟷ yapay) language ⟷ planned language ⟷ invented language ⟷ fictional language__ terimleri de kullanıldığı le karşılaşabiliriz. Örneğin yazılım programlama dilleri birer __conlang__'dir.

NLP yapay zeka ve dil bilim alt kategorisidir. bu dal ile her türlü dili sözlü veya yazılı olarak anlama işlevleri yerine getirilmeye çalışılır. bunun için yazılımlar geliştirilmiştir. bu yazılımların örnek bazı görevleri ve YSA'ya bunun neresinde ihtiyaç duyulduğu:

- sesi algılayıp yazıya çevirme

  sesteki kelimeler tam olarak algılanamadığında YSA ile cümlenin akışına göre tahminleme de yapılabilir

- metni okuma

  bir metin okunduğunda birçok kişi farklı tonlarda okuyor. bu tonların daha düzgün olması için YSA'dan yararlanılıyor. eğer YSA olmadan metin okutulursa robotun okuduğu çok belli oluyor.

- dil bilgisi hata denetimi

  sadece hata denetimi için YSA işlevlerinden yararlanılmaz, fakat yanlış karakterler yerine düzgün kelime önerilerinde YSA'dan yararlanılabilir

- cümleyi veririz, bize her kelimenin kökünü ve detayını verir. örnek: "ben eve gidiyorum" dönüş olarak: [ {kok: ev, aldığı-ek:"e", tip: nesne, adet: tekil, sıra: 2, verb:false ... } ... ]

- farklı dile çeviri işlemleri

  YSA ile çevrilecek konular daha net bir şekilde algılanıp diğer dile çevrilmesi kolay olabilir

- OCR

  YSA bu metinlerin tam okunamadığı zamanlar tahminlemede kullanılıyor

- bir cümleyi kategorileme: örnek banka uygulamamız olsun. müşteri istediği işlemi söyleyecek/yada yazacak bizde o işlemin ne olduğunu anlayıp işlemi gerçekleştireceğiz. Müşterinin verdiği cümle, bizim işlem uzayımızdaki hangi cümle ile aynı kategoride olduğunu tespit/tahmin edebiliyor. örneğin; eğer tahmin oranı sonucu %90 oranla A işlemi çıktıysa, direk müşteriyi A işlemi sayfasına yönlendiririz. oysa %70 oranla A çıktıysa, müşteriye tekrar ne yapmak istediğini  şekilde açıklamasını isteriz.

  burada YSA tahminleme oranlamasında kullanılıyor

genelde her dil için ayrı NLP yazılımları geliştiriliyor, fakat bu ortak yazılımların olmayacağı/olamayacağı anlamına gelmiyor.

## 📌 zemberek

özellikle Türkçe olmak üzere Türk dilleri için geliştirilmiş açık kaynak kodlu bir doğal dil işleme Java kütüphanesidir. openoffice ve libreoffice için Türkçe yazım denetimi eklentisi bu kütüphaneyi kullanır ve eklentide aynı ismi taşımaktadır.

## 📌 Optical character recognition (⟷ optical character reader ⟷ OCR)

resim'den metin algılama işlemidir.

Tesseract komut satırı uygulaması (açık kaynaklı) bu işi yapmaktadır. Motor olarak Libtesseract'ı kullanmaktadır.

## 📌 open source NLP yazılımları

açık kaynaklı kütüphaneler mevcut. fakat henüz (2018 yılı) son kullanıcıya sunulacak derecede başarılı değiller. hiçbir NLP konusunda ticari olmayan NLP özelliği mevcut değil.

## 📌 Google speech Api

Google'ın speech API'si client olan her taraftan çağrılabilmektedir.

Google API'si sesi gerçek input-stream'ler ile gerçek zamanlı da alabiliyor. yani baştan sonra tek dosya halinde ses atılmak zorunda değil.

Google sesi sunucularına atıyor. kişi sesini gereksiz seslerden arındırma yeteneğine sahip. sesi sadece metine çevirmekle kalmıyor, aynı zamanda her kelimenin yüzde kaç doğrulukla metne çevrildiği de tespit ediliyor ve yapay sinir ağı tüm cümleyi devrik olmayacak şekilde doğrulamaya çalışıyor. yapay sinir ağı olduğundan; yüzde yüz bir sonuç olmuyor. Google API'sinin result'ı bir string listesi oluyor. her elemanında kişinin kurduğu tüm cümle oluyor (baştan sona konuşması).

listesinin ilk elemanı Google'ın en doğru düşündüğü cümle. ikinci elemanı daha az ihtimalle doğru olan cümle. listenin elemanı sıfıra yaklaştıkça daha yanlış sonuçlara gidilmektedir. her string elemanının bir de güvenilirlik oranı mevcut. böylece yazılımcı eğer ilk eleman en az %90 güvenilir değilse, uygulamaya işlem yapma gibisinden davranış sergiletebilir.

Google-translate'ten daha başarısız oranla dili de otomatik algılayabiliyor. daha başarısız çünkü alfabetik harfleri göremiyor. sadece okunuşuyla değerlendiriyor. bu sebeple otomatik dil tanıma özelliği sunulmuyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 tr:Ortografi (⟷ eng:orthography)

Bir dilin yazım kurallarını ifade eder. Yani, kelimelerin nasıl yazılacağı, büyük harf kullanımı, noktalama işaretleri gibi konuları kapsar. Ortografi, bir dilin yazılı formunun standartlaşmasını sağlar.

## 📌 Dil bilgisi (⟷ grammar)

Bir dilin yapısını ve kurallarını inceleyen bir alandır. Dil bilgisi, cümle yapıları, zamanlar, fiil çekimleri, isimlerin kullanımı gibi dilin gramer kurallarını içerir. Dil bilgisi, bir dilin doğru ve anlamlı bir şekilde kullanılmasını sağlar.

## 📌 tr:tipografi (⟷ tr:tipografya ⟷ eng:typography)

Türkçe sözlükteki anlamı: Kabartma biçimlerle ilgili baskı yöntemi.

Artık digital ortamlarda karakterlerin farklı temalarla yazılmasını da kapsıyor. Aslında tematik yazılan fiziksel veya dijital her karakter tipoğrafi disiplini çatısı altındadır diyebiliriz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 Input Form validasyonları

## 📌 Posta kodu (⟷ postal code)

Her ülkede farklı terminoloji kullanılmaktadır. bazı örnekler:

- Amerika da ZIP Code
- Hindistan da PIN Code (Postal Index Number'ın kısaltması)
- Brezilya'da CEP
- İrlanda'da Eircode

posta kodunun formatı ülkeden ülkeye değişiklik göstermektedir (bazı ülkelerde harf boşluk ve tire işareti dahi mevcuttur). bu sebeple; son kullanıcıya önce ülke seçimi yaptırılmalı ve posta kodu validasyonu devreye girmelidir.

Türkiye'de posta kodu mahalle'yi temsil etmektedir. fakat bazı ülkelerde, şehir/semt/mahalle kavramlarının seviyeleri farklı olduğundan, posta kodu farklı bir seviyeye denk gelebilir.

Türkiye'deki seviyeler (büyükten küçüğe):

- il
- ilçe
- semt
- mahalle veya köy (aynı seviyedeler)

Bazı ülkeler, bazı posta kodlarını spesifik kurumların adresine veriyor. bunlar genelde yoğun posta alan devlet daireleri gibi kurumlar, yada büyük kampüsler/yerleşkeler oluyor.

Genelde e-ticaret firmaları ülke bazlı kontrol yapıyor yada sadece karakter sayısını kontrol ediyor. adresin yanlış girilmesi sorumluluğu son kullanıcıya bırakılıyor.

## 📌 Administrative division (⟷ idari bölünme)

her ülke kendi içinde birçok alt bölge ayrım içindedir. son kullanıcıya adres formu doldururken, eğer site uluslararası bir site ise (tüm dünyaya yada geniş bir alana hizmet veriyor ise) sadece ülke ve şehir bilgisi otomatik selectbox'tan tamamlanır. bir de her zaman son kullanıcıya adresi kendisi girmesi için textbox bırakılır. adres kullanıcının sorumluluğundadır. çünkü her ülkenin tüm seviyelerini otomatik selectbox'tan tamamlanması çok zordur. her ülkenin bilgisi dönem dönemde (zaman zaman) değişebiliyor. sürekli bu bilgileri güncel tutmak büyük maliyet. bazı kaynaklar bunları web servisi yada offline olarak sağlıyor, fakat tüm detayların doğru olduğundan hiçbir zaman %100 emin olamıyorsunuz. Google'ın "Places" servisi otomatik tamamlamayı ücretli şekilde sunuyor.

eğer birkaç yada tek bir ülkeye hizmet veren bir yazılım geliştiriliyorsa, tüm seviyelerin kaynaklarını bulmak kolay oluyor. fakat tüm dünya için bu detaylar çok maliyetli.

tüm dünyanın DB olsa dahi, her seviyenin isminin hangi dilde son kullanıcıya gösterileceği de önemli. Türkçe olarak sitede gezip form dolduran bir son kullanıcı, adres formunda Amerika'yı seçtiğinizde her seviyenin Türkçe çıkması gerekecektir. bunların kombinasyonu çok fazla.

Bazı seviyeler bazı durumlarda atlanabiliyor (boş geçilebilmesi gerekiyor). Örneğin Amerika'da Washington DC, herhangi bir eyalete bağlı değil.

sadece şehir/ülke kombinasyonu yeterlidir. Amazon gibi büyük firmalar bu şekilde yapıyorlar (yıl 2018).

PHP'de kullanılan https://github.com/commerceguys/addressing kütüphanesine baktığımızda, DB adres modelinin şu olduğu görülüyor:

- Country code
- Administrative area
- Locality (City)
- Dependent Locality
- Postal code
- Sorting code
- Address line 1
- Address line 2
- Organization
- Given name (First name)
- Additional name (Middle name / Patronymic)
- Family name (Last name)

Yukarıdaki modelin OASIS eXtensible Address Language (xAL) standard'ten alındığı belirtilmiş. Kütüphanenin github root readme dosyasında şu not düşülmüş:

`The dataset is stored locally in JSON format https://github.com/commerceguys/addressing/tree/master/resources. Countries are generated from CLDR v37  http://cldr.unicode.org/. Address formats and subdivisions are generated from Google's Address Data Service https://chromium-i18n.appspot.com/ssl-address.`

`Google`'s `Address Data Service`'inden `TR/İstanbul`'u seçtiğimizde bu sayfa karşımıza çıkmaktadır: https://chromium-i18n.appspot.com/ssl-address/data/TR/İstanbul 

içerdiği bilgi şudur:

```json
{"id":"data/TR/İstanbul","key":"İstanbul","lang":"tr","zip":"34","isoid":"34"}
```

## 📌 isim soy isim (full name)

- her ülkede farklı standartlar vardır.
- bazen nüfusa yanlış yazılmış, fakat değiştirilmemiş karakterlerde çıkabiliyor. bu sebeple ülke bazlı dahi düzgün bir validasyon/regex mevcut değildir.
- yunan gibi alfabeler de hesaba katıldığında validasyon bulmak çok zor. bu sebeple bazı siteler validasyon yaparken sadece bazı karakterleri exclude ediyor. çünkü sadece kabul edilen karakterler çok fazla.
- tüm karakterler kabul edildiğinde bu sefer işaret karakterleri de kabul edilmiş oluyor. bu durum yunanca gibi diller işin içine girince daha da zorlaşıyor.
- sayı(lar) içeren isimlerde mevcut.
- tek kural şu denebilir: isim ve soy isim en az birer karakterden oluşmak zorundadır ve bu karakterler Unicode'da control karakteri property'sine sahip olmamalıdır. (Tabi yine daha fazla detaya girilirse; Unicode'da farklı "boşluk" karakterleri var. Onları da exclude etmek gerekebilir.)
- standart olarak her zamanki gibi, XSS saldırıları için text'i denetlemek gerekir.

## 📌 yaş

- minimum yıl seçeneği çok eskiye dayanmalıdır. resmi olarak kayıtlarda ülkeye ölü bilgisi gelmeden o kişi ölü sayılamaz. ve sistemde kaydı açılması gerekiyor ise; bu kişinin doğum yılı çok eski tarih olabilir. e-devlet gibi sistemler düşünüldüğünde yaş validasyonunu yüksek tutmakta yarar var.

Google, Yahoo, Apple gibi sitelerin kayıt sayfalarında 1865'ten itibaren doğum yılı validasyonları kabul ediliyor (yıl 2019).

## 📌 telefon mobil numaraları

her ülke için değişmektedir.

<https://github.com/Google/libphonenumber> projesi C++, Java ve JS için ortak repo altında kütüphane sunmaktadır. bu kütüphane ülke bazında validasyon sağlamaktadır. aynı standartları uygulayan diğer diller için "Third-party Ports" başlığında listelenmektedir: <https://github.com/Google/libphonenumber#third-party-ports>

## 📌 email ve URL

- Emailin kendi validasyon kuralları var. Fakat ek olarak tüm URL kurallarını kapsıyor. çünkü emailin @ işaretinden sonraki kısmı bir URL'dir.
- domain'in uzantısı en az 2 karakter olmalı.
- domain'in uzantısı istenildiği kadar uzun olabilir ve sayı içerebilir.
- domain'in uzantısı sayı ile bitemez.
- bazı URL bilgilerinde IP girilebiliyorsa, regex çok daha karışık olmaktadır (IP4 ve IP6 da unutulmamalı).
- emaillerde <john@server.department.company.com> gibi subdomain'ler olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
