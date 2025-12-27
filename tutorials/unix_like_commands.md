# UNIX LIKE COMMANDS

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 bazı komut satırı uygulamaları ve shell builtin'ler

### 📌📌 echo

`-e` parametresi aşağıdaki karakterlern özel karakter olarak algılanmasını sağlar.

```text
\a      alert (bell)
\b      backspace
\c      suppress further output
\e      escape character
\f      form feed
\n      new line
\r      carriage return
\t      horizontal tab
\v      vertical tab
\\      backslash
\0nnn   the character whose ASCII code is NNN (octal).  NNN can be 0 to 3 octal digits
\xHH    the eight-bit character whose value is HH (hexadecimal).  HH can be one or two hex digits
```

`-E` yukarıdakilerin özel karakter olarak algılanmamasını sağlar.

`-n` çıktınınn sonuna satır sonu karakteri ekler.

`echo` çoğu sistemde built-in komut'tur. `-n` ve `-e` parametreleri çoğu komut satırı interpreter'ında default olarak zaten `echo` komutuna yollanır.

### 📌📌 printf

`print formatted`'ın kısaltmasıdır.

kullandığı format, `C` dilindeki `printf` ile aynı.

### 📌📌 VIM

birçok mode'u var:

- insert mode
- normal mode
- visual mode

`VIM`'in hangi mode'da olduğunu en aşağıdaki panelde yazmaktadır. `VIM` ilk açıldığında insert mode'da başlar. insert mode'da klavyeden girilenler, dosyayı düzenlemek için kullanılır.

`ESC` tuşu ile normal moda geçiş yapılır.

Normal mode'dayken de sağa sola gidilebilir. bunun için sağa sol yerine, `h` `j` `k` `l` tuşları (veya farklı tuşlar konfigüre edilebilir) kullanılabilir.

normal mode'da, 4 karakter sağa gidilecek ise; 4 ardından da `l` karakterine basılır.

normal mode'da sırası ile `c`,`4`,`l` tuşlarına basıldığında ise 4 karakter sağa gider, ardından da otomatik insert moda geçeriz. `c` karakteri bunu yapar.

normal mode'dayken, insert mode'a geçiş yapmak için `i` tuşuna basmalıyız. eğer `a` tuşuna basarsak bizi insert tuşuna alır fakat imleci bulunan karakterin sağına atar.

normal mode'dayken komut girebiliriz. bunun için normal mode'dayken yada insert mode'dayken `:` tuşuna basmalıyız. ardından da komutumuzu gireriz. örnek:

```
:q
```

`VIM`'i kapatır.

```
:q!
```

değişiklikleri kaydetmeden kapatır

```
:wq
```

değişiklikleri kaydederek çıkar.

```
:w
```

sadece değişiklikleri kaydeder. `VIM`'i kapatmaz.

visual mode'a geçiş yapabilmek için v tuşuna basılmalıdır. v tuşuna basıldıktan sonra karakter grubu seçimi yapabiliriz. burada hem mouse support'u var, hem de yine `ghjk` karakterleri ile sağa sola gidip seçim yapabiliyoruz. 4 ardından da `l` tuşuna basarsak, 4 karakter sağa doğru seçim yapılmış olur gibi.

### 📌📌 Emacs

bu terim; genişletilebilir komut satırı text editörlerine verilene genel bir isimdir. tek başına bir uygulama değildir. mutlaka kendini Emacs olarak belirten bir türevini kurmak gereklidir:

- `GNU Emacs`
- `XEmacs (⟷ old-name:Lucid EMacs)`
- `remacs`

komut satırından `emacs` diye çağrıldığında sistemde yüklü olan bir `Emacs` editörü (yukarıda yazanlardan biri) çağrılır.

`Emacs`, `VIM`'e göre çok daha gelişmiş bir yapıya sahiptir. fakat `VIM`'e göre konfigürasyonu daha zordur.

### 📌📌 ln

- kısa yol oluşturur.
- `ln -s`, `symbolic link (⟷ soft link)` oluşturur.
- `-s` parametresi geçilmezse `hard link` oluşturur.
- `POSIX`'teki dosya sistemleri her 2 türdeki link çeşidini de destekler.
- `soft link`, dosyanın path'ine işaret ediyor. dolayısı ile; link ettiğimiz dosyanın adı veya path'i değişirse, linkimiz artık çalışmaz hale gelir.
- `hard link` ise; dosyanın içeriğine direk link ediyor. dosya ismi değişirse, linkimiz hala doğru yere referans etmeye devam eder.

  (Yani; dosyanın içeriği ile dosyanın adresi farklı göstergelerdir)

### 📌📌 man

`man printf`

aldığı ilk parametre farklı bir programdır. ilgili programın manuel page'sini gösteriyor.

`POSIX`'lerde her uygulama kurulduğunda manuel dosyalarını `/usr/share/man/tr/i386` veya `/usr/local/man` gibi dizinlere atar. bu dizinler `OS`'tan `OS`'a değişiklik göstermektedir.

`man` programı bu dizinleri kurcalar ve bize ilgili programın manuel'ini gösterir.

her manuel'de `SYNOPSIS` (`Türkçe` kelime anlamı: özet) başlığı vardır. Bu başlıkta komut satırının parametrelerinin nasıl yollanacağı açıklanır.

`SYNOPSIS`'in %100 bir standartı yok. birçok firma kendi standardını resmi sitesinden yayınlıyor. fakat her uygulama bunlara uymuyor ve firmalar arası ufak farklar mevcut. örnek bir `SYNOPSIS` standartları standardı: https://www.ibm.com/support/knowledgecenter/en/STAV45/com.ibm.sonas.doc/sonas_manpages_syntax.html

örnek `SYNOPSIS` ve anlamları:

```
command [options] <file1> [<dir1>]... param {a|b|c}
```

- `options` başlığındaki parametrelerden birini alacak anlamına gelir.
- `options`'taki `[]` işareti, bu alanın opsiyonel olduğunu gösterir. yani yollanması zorunlu değildir.
- `file1` zorunlu bir alandır. çünkü `[]` içinde değil.
- `file1` `<>` içine alınmış çünkü komutu çağracak kişi istediği değeri buraya yazabilir.
- `dir1` opsiyoneldir ve `...` birden fazla parametre yan yana olabilir anlamını kazandırır.
- `param` `<>` içine alınmamış. çünkü hardcode bir string. yani son kullanıcı buraya sadece `param` yollayabilir.
- `{a|b|c}`, `a` veya `b` veya `c` parametresi alır. bazı standartlarda `{}` yerine `()` kullanılır. `c`'nin altı çizili olsaydı, `c` default değer (yani son kullanıcı o parametreyi yollamazsa) alınacak değer anlamına gelir.

- kalın yazılmış kelimeler:
  - `<>` alınmayan (sabit, yani hardcode string'leri)
  - komutun kendisi

- italik yazılan kelimeler:
  - `<>` içerisine alınan değerlerdir

#### 📌📌📌 section

`man` sadece komutların doc'larını değil, `system call` gibi birçok fonksiyonunda doc'unu göstermektedir. bunlar bazı kategorilere ayrılmış durumda. kategori listesi bu şekilde:

- 1 - User commands (Programs)

Those commands that can be executed by the user from within a shell.

- 2 - `System call`s

Those functions which wrap operations performed by the kernel.

- 3 - Library calls

All library functions excluding the `system call` wrappers (Most of the libc functions).

- 4 - Special files (devices)

Files found in /dev which allow to access to devices through the kernel.

- 5 - File formats and configuration files

Describes various human-readable file formats and configuration files.

- 6 - Games

Games and funny little programs available on the system.

- 7 - Overview, conventions, and miscellaneous

Overviews or descriptions of various topics, conventions and protocols, character set  standards, the standard `file system` layout, and miscellaneous other things.

- 8 - System management commands

Commands like `mount`, many of which only `root` can execute.

örnek:

`man 1 read` komutu read komutunun doc'unu gösterecektir. fakat `man 2 read` read `system call`'unun doc'unu gösterecektir.

örnek2:

https://linux.die.net/man/2/read sayfası bize read'in `system call`'unun doc'unu gösteriyor.

### 📌📌 httpie

`wget` ve `curl`'a modern bir alternatiftir.

komut satırından `http` olarak çağrılır.

### 📌📌 http-prompt

arkada `httpie` kullanan, fakat komut satırında otomatik tamamlama sunan bir komut satırı uygulamasıdır. interaktif çalışır.

komut satırına `http-prompt` yazıldığında interaktif moda geçer.

### 📌📌 update-alternatives

`/etc/alternatives/` ve `/var/lib/alternatives/` dizinlerini düzenler.

Aslında hepsi birer link dosyasıdır:

- `/usr/bin/java` (aşağıdakine link eder)
  - `/etc/alternatives/java` (aşağıdakine link eder)
    - `/usr/lib/jvm/java-17-openjdk-amd64/bin/java`

### 📌📌 7z

- `7-zip`

`MS Windows` için dağıtılan official paketin ismidir.

- `7-Zip for Linux/MacOS`

sadece `7-zip` yazan bir makalede `MS Windows` build kastediliyor. Oysa `7-Zip for Linux/MacOS`, resmi `7-zip` geliştiricilerinin `MacOS` ve `Linux` için geliştirdikleri `7-zip` paketidir.

`7z` ilk çıktığında sadece `MS Windows` için çıkmıştı. Bu sebeple, `p7zip` projesi `Linux` için fork olarak çıkarıldı. Ama artık `7-Zip` resmi olarak `Linux` ve `MacOS` desteklediği için `p7zip` güncelleme almamaktadır.

- `p7zip`

`7-Zip` geliştiricilerinden bağımsız sunulan ve sadece `POSIX`'ler için geliştirilen paketin ismidir.

`p7zip`, `7-Zip`'nin resmi sitesinde ayrı bir başlıkta referans olarak gösterilmektedir.

- `p7zip-full` vs `p7zip`

İki pakette independent developer'ların çıkardığı fork'un `linux` installer isimleridir. Artık bu paketler güncelleme almamaktadır.

<https://packages.debian.org/stable/p7zip-full>

<https://packages.debian.org/stable/p7zip>

Yukarıdaki kaynaklardan alıntılar:

```text
p7zip, provides 7zr, a light version of 7za, and p7zip, a gzip-like wrapper around 7zr.
```

```text
p7zip-full, provides 7z and 7za which support more compression formats.
```

- lisans

Orjinal `7zip` paketi un-free olarak sınıflandırılır. çünkü `unRAR` kodu içermektedir.

Resmi bir kaynak bulamadım ama bi yerde `Debian` paketinin free olabilmesi için `rar` kodunun silindiği yazmaktadır. Bu sebeple `Debian`'deki bazı paketler `free` sınıfında olabilir.

- `7-Zip`'in içerisinde birçok binary vardır:
  
  - `7z`

    (binary/executable dosya) kendisiyle aynı dizinde olması beklenen MS Windows'ta 7z.dll'i (Linux'ta 7z.so'yu) kullanılır.

    __7z.dll (Linux'ta 7z.so)__ asıl işi (7z engine) yaparken, __7z__ binary'si sadece komut satırı API'si sunar.

    7zip'in GUI'si, 7z'yi değil, 7z.dll (Linux'ta: 7z.so)'i kullanır.

  - __7zG__ ve __7zFM__
  
    binary'leri sadece MS Windows için GUI sunmaktadır.

    __7zFM__ file-manager olarak açılan GUI kodlarını içeriyor.

    __7zG__ yine komut satırı 7Z gibi aynı parametreler ile çağrılıyor fakat console output yerine GUI açarak işlemleri gösteriyor (örneğin progress bar).
  
  - __7z.sfx__
  
    __self-extracting archive (⟷ SFX ⟷ SEA)__ module only for MS Windows.
  
  - __7zCon.sfx__
  
    console version on SFX module.
  
  - __7za.exe__
  
    a: alone

    standalone console version of 7-Zip. sadece console API'si sunmaktadır.
  
    __7za__ does not support same features as __7z__.

  - __7za.dll__
  
    same logic as __7za.exe__ but it is DLL.
  
  - __7zxa.dll__
  
    a: alone

    x: extract

    library only for extracting archives (no other dependency needed for this executable)

- __7-Zip Extra__

  paketi, __7-Zip__ geliştiricilerinin dağıttığı özel bir pakettir.
  
  GUI içermez ve sadece MS Windows için çalışır.

  Hem DLL hem binary barındırır. Ek olarak 3üncü parti yazılım eklentilerini de barındırır.

### 📌📌 awk

girdi olarak multiline bir string alır ve her satırını ekrana nasıl basacağını parametre olarak alır.

örnek:

İlgili dosyanın her satırını ekrana basar:

```sh
awk '{print $0}' myfile
```

- `$0` for the whole line.
- `$1` for the first field.
- `$2` for the second field.

Her satır; `RS` yani; "input-record separator" ile birbirinden ayrılıyor. Her satır içindeki field'lar ise; "FS" yani;"the input-field separator" birbirinden ayrılıyor. Bu ayraçları override edebiliriz.

örnek:

```sh
ss --all --processes --tcp --udp | awk -F ' ' '{print $1"\t"$2"\t"$5"\t"$6"\t"$7}'
```

### 📌📌 pushd ve popd

`cd`'ye alternatif komut satırı uygulamalarıdır. cd'ye göre akışta ince detaylar fark etmektedir. fakat `cd` built-in oluğundan bu davranışları kesin olarak standarda dökmek yanlıştır.

```sh
cd directory1
    # change directory to directory1
    # save the original directory in OLDPWD
    # set PWD="directory1"
    # replace top element of the directory stack (as shown by dirs) with directory1 # (the number of elements on the stack does not change).
cd -
    # change directory to $OLDPWD
    # swap values of PWD and OLDPWD
    # modify the top element of the directory stack to reflect (the new) PWD
pushd directory1
    # change directory to directory1
    # save original directory in OLDPWD
    # set PWD="directory1"
    # push directory1 onto the directory stack (extending it by one element)
popd
    # save original directory in OLDPWD
    # remove first element of the directory stack
    # change directory to the new top element of the directory stack
    # set PWD to the new top element of the directory stack
```

### 📌📌 alias

komut satırı dünyasında alias bir komutun yerine kullanılan keyword'dür. örneğin;

```sh
alias clr="clear"
```

komutu ile artık istediğimiz zaman "clr" komutu ile "clear" komutunu çalıştırmış oluruz.

```sh
unalias clr
```

komutu ile alias silinir.

## 📌 script

"script" komutu çağrıldıktan sonraki komutların ve çıktıların hepsini "exit" komutu çağrılana kadar kaydeder ve bir dosyaya aktarır. örnek:

```sh
script "$HOME/records1.txt"
echo "hello"
echo "world"
exit
```

Artık "$HOME/records1.txt" u text editor ile açıp okuyabiliriz. Bu dosya direk kolay okunabilir olmayabiliyor. Bunun birçok sebebi var. Bazıları:

- Komut satırını renklendirmek için gerekli kod karakterleri varsa onlarda burada oluyor.
- Komut satırında her yeni satırda $ gibi karakterler de oluyor. Onlarda bu dosyaya yazılıyor.

## 📌 locale

bu komut bize OS'un default olarak hangi dil ve coğrafi özelliklerine göre hizmet verdiğini göstermektedir.

"locale" komutun çıktısı aşağıdaki gibidir:

```sh
LANG="en_US.UTF-8" # Eğer bir değişkenin değeri yok ise (örnek LC_MONETARY boş ise), "LANG" değeri ise default değer olarak kabul edilir.

LANGUAGE=

LC_CTYPE="en_US.UTF-8" # using for character classification and case conversion. örneğin hangi karakter, 1 letter veya işarete tekabül ediyor gibi... Küçük büyük harf çevrimlerinde kullanılıyor. Örneğin; İngilizce'de I küçük harfe çevrilirken i diye çevriliyor, oysa Türkçe'de i diye çevriliyor. Dolayısı ile bunun nasıl yapılacağına LC_CTYPE karar veriyor.

LC_NUMERIC="tr_TR.UTF-8" # Specifies the decimal delimiter (or radix character), the thousands separator, and the grouping.

LC_TIME="tr_TR.UTF-8" # date formatting

LC_COLLATE="en_US.UTF-8" # Bu değer character/string "sorting/ordering" gibi algoritmalarda kullanılıyor.

LC_MONETARY="tr_TR.UTF-8" # monetary formats, including the currency symbol for the locale, thousands separator, sign position, the number of fractional digits, and so forth.

LC_MESSAGES="en_US.UTF-8" # Specifies the language in which the localized messages are written, and affirmative and negative responses of the locale (yes and no strings and expressions).

LC_PAPER="tr_TR.UTF-8" # her coğrafyada ve dilde 1 adet sayfanın boyutları değişebiliyor. örneğin Türkiye'de sıkça A4 kullanılıyor. dolayısı ile tr_TR.UTF-8 kuralları içerisinde A4 yazmaktadır.

LC_NAME="tr_TR.UTF-8" # How names are represented (surname first or last, etc.).

LC_RESPONSE="tr_TR.UTF-8" # Determines how responses (such as Yes and No) appear in the local language. LC_MESSAGES ile çok benzer. Bazı OS'larda iri olup diğeri olmayabiliyor.

LC_ADDRESS="tr_TR.UTF-8" # LC_ADDRESS Convention used for formatting of street or postal addresses. How addresses are formatted (country first or last, where Zip code goes etc.).

LC_TELEPHONE="tr_TR.UTF-8" # telephone number format (sadece nasıl gösterileceği, +90'dan sonra boşluk mu, yoksa parantez mi var gibi)

LC_MEASUREMENT="tr_TR.UTF-8" # What units of measurement are used (feet, meters, pounds, kilos etc.).

LC_IDENTIFICATION="tr_TR.UTF-8" # Metadata about the locale information.

LC_ALL=""  # şu örnekte boş. LC_ALL diğer tüm "LC" parametrelerini override etmek için kullanılmaktadır.
```

- Yukarıda tanımlanmış bu değerler shell'e ve diğer uygulamalara environment parametresi olarak geçilmektedir.

- "LC_MONETARY=tr_TR.UTF-8" değerini inceleyelim: Para birimleri için ayracın, nokta mı, virgül mü olacağına dil karar veriyor. yani "tr_TR". Fakat bu ayracın hangi encoding ile tanımlandığı UTF-8 ile belirlenmiş.

- "locale -a" komutunun çıktısı tüm alabilecek(available) local kombinasyonları listelemektedir. Her satırı açıklaması ile görmek için bu komut çalıştırılmalıdır: "locale -v -a". Daha detaylı olarak çıktıya baktığımızda her lokalin rule'larının belli bir dosyaya gömülü olduğunu görürüz. Bu dosyalar programlar tarafından parse edilebilirdir. Örnek dosyalar:

  - locale: C.UTF-8 kuralları bu dosyalardadır: /usr/lib/locale/C.UTF-8/LC_ADDRESS, /usr/lib/locale/C.UTF-8/LC_TIME...

  - locale: tr_TR.utf8 nin kurallarının tümü (LC_ADDRESS + LC_TIME ...) bu dosyadadır: /usr/lib/locale/locale-archive

- Bazı durumlarda "tr_TR.UTF-8" yerine "POSIX" görebiliriz. Bu POSIX standartlarında belirtilen default değer anlamına gelmektedir. POSIX standartlarında ise "C" lokaline denk gelmektedir.

- "en_US.utf8" Amerika bölgesindeki İngilizce ve UTF-8 encoding'i ile birlikte kullanımını temsil ederken sadece "en_US" yazanlar farklı encoding ile oluyorlar. Hangi encoding olduğunu görmek için "locale -v -a" komutu ile incelenmelidir.
