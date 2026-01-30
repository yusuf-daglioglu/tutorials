# REGEX

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 regular expression (⟷ regex ⟷ reg-ex ⟷ regexp ⟷ rational expression)

arama yapabilmek için karakterlerden oluşan özel bir ifade dilidir.

birçok implementasyonu vardır. örnek:

- `IEEE` `POSIX`'ta belirtilen: `BRE (⟷ Basic Regular Expressions)`
- `IEEE` `POSIX`'ta belirtilen: `ERE (⟷ Extended Regular Expressions)`
- `IEEE` `POSIX`'ta belirtilen: `SRE (⟷ Simple Regular Expressions)`
- `Perl Compatible Regular Expressions (⟷ PCRE)` `kaynak: https://www.pcre.org/ 1inci paragraf`

Her regex motorunun, yukarıdaki specification'lara ekstra farklı syntax/formatı özellikleri de vardır. Bazı farklılıkların listesi buradadır: http://www.greenend.org.uk/rjk/tech/regexp.html

## 📌 full unicode support

`Unicode` DB'de her karakterin meta-bilgileri mevcut. Eğer kullandığımız regex kütüphanesi bu bilgileri saklıyor ise; o zaman aşağıdaki gibi kontroller ile de regex yazabiliriz:

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

`kaynak: https://docs.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html "Unicode support" başlığı`

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 regex kullanımı

(aşağıdaki syntax bilgileri hangi notasyon standardına göre yazıldı bilmiyorum. aşağıdaki bilgiler farklı kaynaklardan toplama olduğundan farklı standartlara ait bile olabilirler.)

| regex                                 | what it returns                                                                                                                                                                                                                                                                      |
|---------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ^merhaba                              | starts with "merhaba"                                                                                                                                                                                                                                                                |
| merhaba yusuf$                        | ends with "merhaba yusuf"                                                                                                                                                                                                                                                            |
| merhaba                               | contains "merhaba"                                                                                                                                                                                                                                                                   |
| ab*                                   | a, ac, abc, abb, abbc döndürebilir. a kesin var. b'den istenildiği kadar olabilir. *karakteri sadece bir önceki karakter için bir kuralı kapsar. eğer bir önceki karakter birden fazla karakter olacak ise; a(bc)* olarak parantez içinde gruplandırılır.                            |
| a(bc)*                                | a, abc, abcbc, abce döndürebilir.                                                                                                                                                                                                                                                    |
| ab+                                   | * karakteri ile aynıdır. tek farkı en az 1 adet b olmalıdır. ab, abb, abc                                                                                                                                                                                                            |
| ab?                                   | * karakteri ile aynıdır. tek farkı en fazla 1 adet b olmalıdır. a, ab, ac                                                                                                                                                                                                            |
| a?b+$                                 | ab, b, bb, abb, abbb döndürebilir. en sonda "dolar" olduğundan,  en sağa c gibi farklı karakterler gelemez                                                                                                                                                                           |
| ab{2}                                 | {} * gibi sadece bir önceki karakter için geçerli bir kural belirtir. {2} 2 tane b olması gerektiğini belirtiyor. abb, abbc, abbc                                                                                                                                                    |
| ab{2,}                                | en az 2 adet b olmalıdır. abbb, abb, abbc                                                                                                                                                                                                                                            |
| ab{3,5}                               | en az 3 en fazla 5 adet b olabilir. abbbc, abbb                                                                                                                                                                                                                                      |
| hi∣hello                              | hi veya hello içerenler döner.                                                                                                                                                                                                                                                       |
| (b∣cd)ef                              | bef yada cdef içeren text'ler döner                                                                                                                                                                                                                                                  |
| [ab]                                  | [] sadece bir karakteri temsil ediyor. (a∣b) ile aynı şeydir.                                                                                                                                                                                                                        |
| [a-d]                                 | [abcd] ve (a∣b∣c∣d) ile aynı şey.                                                                                                                                                                                                                                                    |
| [a-zA-Z]                              | regex aksi belirtilmedikçe case sensitive'dir. bu sebeple büyük küçük tüm alfabeyi kapsar.                                                                                                                                                                                           |
| [0-9]                                 | sadece rakam olduğunu belirtir.                                                                                                                                                                                                                                                      |
| [^0-9]                                | rakam dışında her şey olabilir                                                                                                                                                                                                                                                       |
| [a-zA-E0-8]                           | ufak karakterler tüm alfabe, büyük karakterlerde a'dan e'ye kadar olan karakterler, sayılarda ise 0'dan 8'e kadar olan karakterler                                                                                                                                                   |
| [0-9-[3-4]]                           | 0-9 arası karakterler fakat 3 ile 4 arası olan karakterler dahil değil (Subtraction)                                                                                                                                                                                                 |
| [abc-().^+]                           | [] içerisinde hiçbir karakteri escape yapmamıza gerek yok. ( ] karakteri hariç)                                                                                                                                                                                                      |
| [\p{L}-[\p{IsBasicLatin}\p{IsGreek}]] | BasicLatin ve yunanca karakterleri hariç herhangi bir karakter olabilir.  \p{L} yerine \p{Letter} aynı şeye referans eder.                                                                                                                                                           |
| \d                                    | digit. \D ise tersidir (yani non-digit)                                                                                                                                                                                                                                              |
| \s                                    | any space ⟷ \t ⟷ \f ⟷ \v ⟷ \n . \S ise bunun tersidir (yani non-space)                                                                                                                                                                                                           |
| \w                                    | wordly character. yani 1 karakteri temsil eder. Non-Latin letters (like cyrillic ⟷ hindi) do not belong to \w . \W ise bunun tersidir (yani non-wordly character)                                                                                                                   |
| \bJava\b                              | boundary. \ tag'leri arasında kalan kelimeden önce veya sonra ya i işaret yada boşluk yada cümle sonu olmalıdır. herhangi bir text editor'de search yaptığımızda "whole word" seçeneği ile aynı işlevi görmektedir. Boundary özelliği non-latin karakterler için işe yaramamaktadır. |
| a.b                                   | . bir karakteri temsil ediyor. acb, abb                                                                                                                                                                                                                                              |
| ..                                    | en az 2 karakter olan değerler döner                                                                                                                                                                                                                                                 |
| ^..$                                  | sadece 2 karakter olan değerler döner                                                                                                                                                                                                                                                |

## 📌 backslash

özel karakterler kullanılacak ise öncesinde `backslash` kullanılmalıdır. örneğin; `nokta` özel bir karakter. `nokta`yı özel karakter olarak istemiyorsak önüne `\` koymalıyız.

`\r`, `\n`, `\v`, `\t` gibi non-printable karakterler de `\` ile kullanılır.

## 📌 lookahead

2 çeşittir:

- `negative lookahead`
- `positive lookahead` (sadece `lookahead` de aynı anlama gelir)

### 📌📌 Positive lookahead

`X` var ise, sağında `Y` varsa `X`'i de match et:

```text
X(?=Y)
```

Örneğin; `30€` şu şekilde bulunabilir:

```text
\d+(?=€)
```

Aşağıdaki örnekte ve işlemi yapılıyor. `X`'in yanında `Y` varsa, onunda yanında `Z` varsa `X`'i match et.

```text
X(?=Y)(?=Z)
```

### 📌📌 negative lookahead

`X`'in yanında `Y` yok ise, `X`'i match et.

```text
X(?!Y)
```

## 📌 Lookbehind

2 çeşittir:

- `negative lookbehind`
- `positive lookbehind` (sadece `lookbehind` de aynı anlama gelir)

| Pattern | type                | matches                |
|---------|---------------------|------------------------|
| X(?=Y)  | Positive lookahead  | X if followed by Y     |
| X(?!Y)  | Negative lookahead  | X if not followed by Y |
| (?<=Y)X | Positive lookbehind | X if after Y           |
| (?<!Y)X | Negative lookbehind | X if not after Y       |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 class \p{...}

`Unicode`'daki `class`'ları referans alarak kategorilemektedir. listenin bir kısmı aşağıdaki gibidir. bu listeye `Unicode`'un resmi sitesinden de erişilebilir.

```text
Letter L
    lowercase Ll
    modifier Lm
    titlecase Lt
    uppercase Lu
    other Lo
Number N
    decimal digit Nd
    letter number Nl
    other No
Punctuation P
    connector Pc
    dash Pd
    initial quote Pi
    final quote Pf
    open Ps
    close Pe
    other Po
Mark M (accents etc)
    spacing combining Mc
    enclosing Me
    non-spacing Mn
Symbol S
    currency Sc
    modifier Sk
    math Sm
    other So
Separator Z
    line Zl
    paragraph Zp
    space Zs
Other C
    control Cc
    format Cf
    not assigned Cn
    private use Co
    surrogate Cs
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## 📌 JS'te regex

```js
var myRegexObject = /hello world/i; // literal notation
// or
var myRegexObject = new RegExp('hello world', 'i'); // first param is 'pattern' and the second param is 'modifiers'. some sources name the second parameter as 'flags'.
// or
var myRegexObject = new RegExp(/hello world/, 'i');
// or
var myRegexObject = /hello world/i;

var sonuc = "hello world coder".search(myRegexObject);
```

### 📌📌 functions of RegExp object

- `compile()`

Deprecated. Compiles a `regular expression`.

- `exec(str)`

Tests for a match in a `string`. Returns the first match.

```js
regex.exec("hello"); // returns the found string.
```

- `test(str)`

Tests for a match in a `string`. Returns true or false.

```js
regex.test("hello"); // returns true or false.
```

`test` and `exec` functions are always moving the `pointer`. Therefore `regex.lastIndex` will increase every time we call test or exec.

- `toString()`

Returns the `string` value of the `regular expression`.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

### 📌📌 Modifiers

- /i

Perform case-insensitive matching.

- /g

Perform a global match (find all matches rather than stopping after the first match)

- /m

Multiline mode: Eğer multiline mode olmazsa kaynak metin tümüyle tek bir string olarak algılanır. oysa multiline mode'da her satır ayrı ayrı kaynaklarmış gibi algılanır. "Multiline mode" ^ ve $ işaretlerinin davranışlarını etkiler.

- /u

JS motorunda olan bir modifier'dır. JS'te Unicode karakterlerinin bazı özel durumlardan ötürü sorunları var. fakat bu flag geçildiğinde, full Unicode support edilebiliyor.

Buna duruma örnek case aşağıdaki erilebilir:

```js
let str = "A ბ ㄱ";

alert( str.match(/\p{L}/gu) ); // A,ბ,ㄱ
alert( str.match(/\p{L}/g) ); // null (no matches, \p doesn't work without the flag "u")
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
