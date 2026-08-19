# LITERAL LENGTH

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 primitive

Türkçe kelime anlamı: ilkel

primitive programlama diline gömülü olan data tipleridir (data type). örnek: int, char, byte gibi.

## 📌 literal

bilişimde kullanılan literal kelimesinin Türkçesi mevcut değildir.

literal kelimesinin Türkçe anlamı: gerçekçi olan, kelime kelimesine aynı olan.

literal'ler primitive'lere atanabilecek değerlere verilen isimdir. örnek true, false, 'C', 123 gibi. bunlar "new" operatörü kullanılmadan oluşturup atanabilirler.

## 📌 primitive vs literal

int count = 9; //bu satırda int bir primitive'dir. 9 ise literal'dir.

char first = 'C' //c bir literal; char ise bir primitive'dir.

çoğu makalede primitive ve literal kelimesi birbiri yerine kullanılmaktadır. fakat farklı kavramlardır. buradaki anlatımlarda alt kısımda da yanlış kullanılmış kısımlar olabilir. üst tarafta kesin doğru anlatım mevcuttur.

## 📌 Java'da primitive'ler

byte, short, char, long, boolean, float, double, int.

String, Float gibi kavramlar büyük harfle başladıklarından birer sınıfı temsil ederler. literal değillerdir. kendi içlerinde literal barındırır ve onlar için hazır metotlar bulundururlar. isteğe bağlı kullanılabilirler ve birbirlerine her zaman cast edilebilecek şekilde tasarlanmışlardır.

## 📌 integer (⟷ tam sayı)

tam sayıya verilecek değeri 3 farklı şekilde set edebiliriz:

- `binary (⟷ ikili düzen)`: `int myVal = 0b101; //b harfi büyükte olabilir`

- `Decimal (⟷ tr:desimal ⟷ onluk düzen)`: `int myVal = 5;`

- `Oktal (⟷ sekizlik düzen)`: `int myVal = 010; //önünde sıfır olduğundan otomatik 8 lik tabanda algıladı`

- `Hexadecimal (⟷ onaltılık düzen)`: `int myVal = 0x1A; //önünde 0x olduğundan otomatik algıladı`

Yukarıdaki `0x`, `0`, `0b` standartları genelde tüm programlama dillerinde (ve matematiksel/bilimsel ifadelerde) de kullanılmaktadır.

`Java` 7 ile birlikte sayı tiplerinde değerlerin arasına alt çizgi koyulabilir. okunaklığı arttırmak için:

```java
int a = 100_300_500;
```

gibi.

## 📌 long

long a = 150L; veya long a = 150l; şeklinde set edilebilir. eğer long a = 150; yazılırsa 150 değeri int olduğu için otomatik olarak long'a cast edilir.

## 📌 Kayan Noktalı Literal'ler (⟷ floating point literal)

double a = 2e-5;  //küçük e yada büyük E kullanılabilir.  2*(10^(-5)) anlamına geliyor. e karakteri 10 sayısını temsil ediyor. ve sol tarafındaki sayı ile çarpılıyor.

double b = 3e+1;  // yada '3e1' = 30 yapıyor

double b = 3*e+1; // hata verir. e sayısı normal sayı gibi kullanılamaz. özel bir gösterimdir. e karakteri 'Exponential (⟷ üstel)' matematik ifadesinin baş harfidir.

Yukarıda 'e' karakteri hexadecimal bir sayıda 14 rakamına denk gelmektedir. Bu sebeple hexadecimal bir sayı yazıldığında e harfi yanlış anlaşılma yaratıyor. örnek:

0x11e+3 // bu (ondalık)3 ile (onaltılık)11e sayısının toplamı gibi duruyor. dolayısı ile standart belirlenmesi için hexadecimal işlemlerde 'e' yerine 'p' karakteri kullanılır. 'p' karakteri 10 sayısına değil, 2 sayısına denk gelmektedir. yani;

double c = 0x3p4; // 2*(2^4)

## 📌 float

Java'da  float a = 42.8F; yazıldığında 42.8 int olamayacağı için double kabul edilir. double'yi float'a çevirmek için suffix eklenir.

## 📌 char

char x = ‘\141'; // Oktal düzende 141 = 97'dir ve ASCII tablosunda 97=a karakteri olduğuna göre; x'e "a" değeri atanır. \ işareti oktal düzeni ve ASCII tablosunu temsil eder.

char y = ‘\u0061'; // \u onaltılık düzeni ve UNICODE tablosunu temsil eder.

## 📌 cast işlemleri

cast edilen taraf, dönüştürdüğümüz tipin değerinden büyük ise, ondalık kısmını kaybedebilir, yada dönüştürdüğümüz tipin maksimum değerini alır. örneğin (byte)150; değeri işlemi sonucu bize 127 verecektir. (int)50.9 değeri bize 50 verecektir.

## 📌 Standart büyüklükler

OS ve donanımlar yazılımların en az byte tipinde veriyi işletmesini sağlarlar. Tabi bazı istisnalar da var: bazı cihazlar en düşük birimi 8 bit kullanmıyor. fakat bunlar çok özel amaçlı makinelerdir.

CPU'lar ek olarak integer, floating point gibi bazı hazır tiplerde sunarlar. SUndukları bu tipler üzerinde toplama, çıkarma gibi birçok işlem de sunarlar.

Derleyiciler/Programlama dillerinin sundukları tipler 2 farklı şekilde olabilir:

- diller kendi içlerinde tipler de yaratırlar. bu tipler byte'lar üzerine kuruludur.
- bazı tipler direk CPU'nun native destek verdiği tip'lere referans ederler. örneğin C'deki int, CPU'nun sunduğu native tam sayıya referans eder. bu sebeple boyutu dinamiktir.

Bazı tipler IEEE gibi kuruluşlar tarafından standartlara bağlanmaya çalışılsa da, her dilin yada CPU'nun bu standartları kullanacağı kesin değildir. hatta bir dil IEEE standartlarını kullanıyor olsa da, tüm kuralları %100 aynı şekilde kullanmaz. IEEE üzerine ufak düzenlemeler/değişiklikler yapabilir.

Aslında dilden ziyade, compiler büyüklüklere karar verir. compile sırasında makine ve OS tipine göre karar verir ve büyüklükler dinamik olabilir. bu sebeple kod yazılırken C dilindeki gibi "sizeOf" fonksiyonları ile işlem yapmak gereklidir.

`Oracle`'ın `Java` standartlarında 32-bit ve 64-bit makinalarda tiplerin boyutları sabittir. bunlar:

byte: 1 byte

char: 2 byte

short: 2 byte

int: 4 byte (virgülsüz)

long: 8 byte (virgülsüz)

float: 4 byte (virgüllü)

double: 8 byte (virgüllü)

## 📌 word

bu terim bilgisayarlarda CPU'nun desteklediği bir birimdir. CPU aynı zamanda bu desteklediği word üzerinde işlemler (toplama, çıkarma...) yapılabilecek arayüzler sunar. CPU'larda işlem yapılabildiğinden dolayı, word'ler CPU'nun register'larında da saklanabilmelidir. bu sebeple "word" tanımı bazıları tarafından register'ların desteklediği boyutlar anlamında kullanılmaktadır. Word tanımı çok net değildir. çünkü çoğu CPU birden fazla tip sunarlar: integer, floating point types... Bu sebeple hangisinin word olacağını söylemek çok net değildir.

örneğin bazı CPU'lar için word 8 bit, bazıları için 64-bit olabilir.

## 📌 32-bit vs 64-bit vs others

bu ayrımlar CPU'ların mimarisine dayanmaktadır. eğer bir CPU 32-bit destekli ise register'larındaki 32-bitlik data'yı tek instruction cycle'da okuyabiliyor anlamına gelmektedir. okunan data CPU'nun __datapath__'lerinden taşınmak zorunda olduğu için "datapath widths" 32-bit olmalıdır.

dolayısı ile; CPU'daki register'lardan 64-bitlik data'nın okunması gerektiğini düşünelim. 32-bitlik bir CPU 2 kere döngüye girecektir. 64-bitlik makine ise işlemi tek bir hamlede tamamlayacaktır. performans farkı buradan gelmektedir.

CPU'nun register'larında RAM'in adresleri __Memory Address Register (MAR)__'larda saklanmaktadır. çünkü CPU işlem için RAm'in neresinden data çekeceği, işlem bitince sonucu nereye yazacağını kendi içinde tutmak zorundadır. dolayısı ile 32-bitlik makine en fazla 2^32=4 GB'lik RAM'e işaret edebilir. oysa 64-bitlik makine 2^64'lük adrese işaret edebilir. fakat 32-bitlik OS'ları bazı workaround yöntemlerle 32-bitlik makinalarda 8 GB'nin üstünde RAM destekletebilmektedirler.

## 📌 signed number

- sayısal olan her değerin (int, long, double) signed ve unsigned türevleri olabilir.

- pozitif/negatif bilgisini tutmak için birçok yöntem var. bazıları aşağıda listelenmiştir. Farklı represenation'ların olmasının sebebi; bazı matematiksel işlemleri (toplama, çıkarma gibi) tarafında kolaylık sağlamalarıdır. her yöntemin (represenation'ın) avantajları/dezavantajları vardır.

  - Sign and magnitude

    unsigned 0011=3 ise, signed int 1011=-3 tür. En başta olan bit değeri negatif olup olmadığını belirler. bu bit'in basamağı "most significant bit (⟷ MSB)" __sign bit__ denir.

    Bu representation'da olan 2 sayıyı toplama işlemine sokalım:

    ```text
      0011 (3)
    + 1011 (-3)
    -----------
      1110  (-5) ? yanlış değer çıkıyor. Bu sebeple "ones' complement" yöntemi düşünülmüştür.
    ```

  - Ones' complement

    "Sign and magnitude" olduğu gibi MSB kullanılıyor. Ek kural olarak; sadece negatif sayılarda, MSB hariç her basamağın tersi alınıyor (1 ise 0 a çevriliyor).

    Bu representation'da olan 2 sayıyı toplama işlemine sokalım:

    ```text
      0011 (3)
    + 1100 (-3)
    -----------
      1111  (-0) ? eksi sıfır olması mantıklı olmadığından. Bu sebeple "two's complement" yöntemi düşünülmüştür.
    ```

  - Two's complement

    "Ones' complement" ile aynı şekilde hesaplanır. sadece ek olarak şu kural var: sayımız negatif ise; sayı 1 arttırılır.

    ```text
      0011 (3)
    + 1101 (-3)
    -----------
      0000 (0) beklenen/doğru
    ```

  - Offset binary (⟷ excess-K ⟷ excess-N ⟷ excess-e ⟷ excess code ⟷ biased representation)

    bunun bir standardı yok çünkü sabit bir sayı belirleniyor ve bu sayı üzerinden implementasyon oluşturuluyor.

- bir değeri bit bazından okurken/anlamlandırırken unsigned mi yoksa signed mi olduğu mutlaka bilinmelidir ve yukarıdaki represenation'ların hangisi kullanıldığı mutlaka bilinmelidir. yoksa o değer okunur fakat yanlış okunur.
- C dilinde her int signed'dir. unsigned veya signed keyword'leri C'de olan fakat Java'da olmayan keyword'lerdir. Çünkü Java'da unsigned tipte primitive yoktur.
- herhangi bir programlama dilinde singed değerlerimiz two's complementer yada farklı bir yöntem ile tutuluyor olsun. biz bu dilde bitwise (bit seviyesinde) işlemler yapıyor olalım. Dilde kod yazan geliştiriciler sola kaydırma yada sağa kaydırma gibi şeylerde o sayının (ilgili bilginin) nasıl tutulduğu ile ilgilenmeyiz. Bunu CPU bizim için otomatik çevirir. biz ikilik sistemde sayıyı sola yada sağa kaydırmış gibi işlem yaparız. nasıl tutulduğu bizi ilgilendirmemektedir.

C dilinde aşağıdaki kod, yorum satırında belirtilen çıktıları verir.

```c
int x = 0xFFFFFFFF;
unsigned int y = 0xFFFFFFFF;
printf("%d, %d, %u, %u", x, y, x, y);
// -1, -1, 4294967295, 4294967295
```

Yukarıda x ve y bit bazında aynı değerlere sahipler. printf metodu ekrana basarken, nasıl basılacağına karar verirken ("%d, %d, %u, %u" yazan yer), hem x hem de y için aynı. Bu sebeple aynı değerleri ekrana basarlar.

## 📌 BigInteger vs BigDecimal

Java'da sadece sınıf olarak kullanılama açıktırlar (primitive değildir). BigInteger sadece tam sayı (noktasız), BigDecimal noktalı sayıları tutar. teorikte sınır yoktur. fakat pratikte limit mevcuttur.

## 📌 matematik aritmetik işlemler hatalı olabilmesi

bunun 2 temel sebebi var:

### 📌📌 değerin kaybolması

programlama dillerinde aritmetik işlemler hatalı sonuç verebiliyor. örneğin; int 10 / int 3 = int 3 vermektedir. sayısal değerler kayba uğramıştır. aynı şekilde noktalı sayılarda da limit olduğu için hassas işlemlerde kayıp yaşanabiliyor. bu yüzden özel matematik kütüphaneleri kullanılmalıdır.

bir hesap makinesi diğer hesap makinesi ile farklı sonuç da verebiliyor. çünkü hesap makinası de bir bilgisayar.

### 📌📌 floating point number format (⟷ kayan noktalı sayı formatı)

Piyasada bunu yapmak için birçok format/standart var.

basit olarak kendimiz bir format yazmaya kalksak şu şekilde yapabilirdik:

420000 = 1 x 4.2 x 10^5

Burada

- 1 --> sign (⟷ işaret)
- 4.2 --> fraction (⟷ tr:fraksiyon ⟷ tr:kesir) . Bu parçaya, "önemli" anlamına gelen "significant" terimi de kullanılmaktadır.
- 10 --> base (⟷ taban)
- 5 --> exponent (⟷ üs)

Diğer bir örnek:

-0.0042 = -1 x 4.2 x 10^(-3)

Şimdi ise gerçek standartlardaki bazı örneklere bakalım. IEEE 754 şu şekilde 3 farklı format tanımlamıştır:

| Floating-point format | Total bits | Sign bits | Exponent bits | Fraction bits | Base |
|-----------------------|------------|-----------|---------------|---------------|------|
| Half-precision        | 16         | 1         | 5             | 10            | 2    |
| Single-precision      | 32         | 1         | 8             | 23            | 2    |
| Double-precision      | 64         | 1         | 11            | 52            | 2    |

Yukarıdaki tabloda olan bilgiler boyut bilgileridir. Bu değerler bit bazında yan yana tutulur. Fakat gerçek sayı hesaplanmak istendiğinde bazı formatlarda "logaritma", "ortalama" gibi fonksiyonlara ihtiyaç vardır (yani sadece üs alma ve çarpma işlemi ile hesaplama yetmeyebilir).

### 📌📌 precision of floating point number format errors (⟷ kayan noktalı sayı formatının hassasiyeti hataları)

Float ve double literal'leri kendi içlerinde belli notasyonlarda (formatlarda) tutuluyorlar. Fakat yazılım geliştirici bundan habersizdir. bu format 2'lik düzende olmak zorundadır. örneğin; bir programlama dilinde primitive double değeri yarattık. double'ı belli bir formatta 64-bitte tutalım. ilk 1 biti negatif/pozitif bilgisini tutsun. diğer 31 bit üs, 32-bit bit değeri olsun. bu tarz durumlarda 0.3 sayısını tam olarak tutturamadığımızı görürüz. ekrana bastırmak istediğimizde bu sayıyı yuvarlamak durumunda kalabiliriz. bu tarz durumlar her dilde meydana gelmektedir. dil'in o sayıyı nasıl tuttuğuna bağlıdır.

JS'te meydana gelen bazı hatalar:

- 0.1 + 0.2 = 0.3 but computer says 0.30000000000000004
- 6 * 0.1 = 0.6 but computer says 0.6000000000000001
- 0.11 + 0.12 = 0.23 but again computer says 0.22999999999999998
- 0.1 + 0.7 = 0.7999999999999999
- 0.3 + 0.6 = 0.8999999999999999

Bu sayılar çok düşük 10^(-17) gibi basamaklarda hatalar olduğundan yazılımcılar tarafından fark edilememektedir. Fakat asıl sorun eşitliklerde ( if(x==y) gibi) çıkmaktadır.

### 📌📌 çözüm

çözüm olarak primitive değil sınıf bazlı matematik kütüphanelerinden yararlanmalıyız. örneğin; BigInteger, BigDecimal bu sorunları ortadan kaldırıyor.

BigDecimal gibi değerleri initialize ederken, string bazında etmek daha doğru sonuçlar verecektir. örnek:

```java
double d = 0.3;

BigDecimal bd = new BigDecimal(Double.valueOf(d));

System.out.println(bd); //prints 0.29999

string kullanarak initialize edilirse:

BigDecimal bd = new BigDecimal("0.3");

System.out.println(bd); //prints 0.3
```
