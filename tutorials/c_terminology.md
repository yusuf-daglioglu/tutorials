# C TERMINOLOGY

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 C version history

`ANSI C (⟷ ISO C ⟷ Standard C)` aşağıdaki 2 kuruluş tarafından geliştirildi:

- `American National Standards Institute (⟷ ANSI)`
- `ISO`

Birçok sürümü vardır. spesifikasyon sürümleri:

| kısaltma         | spesifikasyon resmi ID   | resmi stabil yayım yılı | makro değeri `STDC_VERSION` |
|------------------|--------------------------|-------------------------|-----------------------------|
|                  | first edition            | 1978                    | not exist                   |
| `C89`            | `ANSI X3.159-1989`       | 1989                    | not exist                   |
| `C90`            | `ISO/IEC 9899:1990`      | 1990                    | not exist                   |
| `C95`            | `ISO/IEC 9899/AMD1:1995` | 1995                    | not exist                   |
| `C99`            | `ISO/IEC 9899:1999`      | 1999                    | `199901L`                   |
| `C11`            | `ISO/IEC 9899:2011`      | 2011                    | `201112L`                   |
| `C18` yada `C17` | `ISO/IEC 9899:2018`      | 2018                    | `201710L`                   |

## 📌 C standard library (⟷ libc ⟷ ISO C library)

`Libc`, aşağıdaki kuruluşlar tarafından geliştirilen, piyasada kabul görmüş temel birçok işlevleri bulunduran `C` kütüphaneleridir.

- `ANSI`
- `ISO`

`Libc`'ye alternatif birçok kütüphane vardır. bazıları:

- __C POSIX library__ kütüphanesi `Libc`'nin bir türevidir. çok büyük oranla `libc` ile uyumludur, ek olarak birçok farklı fonksiyon da içerir.
- __BSD Libc__, implementations distributed under `BSD` OSs
- __GNU C Library (Glibc)__, used in `GNU Hurd`, `GNU/kFreeBSD` and `Linux`
- __Microsoft C runtime library__, part of `Microsoft` `Visual C++`
- __Dietlibc__, an alternative small implementation of the `C` standard library (MMU-less)
- __μClibc__, a `C` standard library for embedded `μCLinux` systems (MMU-less)
- __Newlib__, a `C` standard library for embedded systems (MMU-less)
- __Klibc__, primarily for booting `Linux` systems
- __musl__, another lightweight `C` standard library implementation for `Linux` systems
- __Bionic__, originally developed by `Google` for the `Android` embedded system OS, derived from `BSD libc`

## 📌 C++

`C` is not a subset of `C++`. `C` programs will not compile as `C++` code without modification. `C++` was introduces as extension of `C`. But after a while `C++` introduced many features that are not available in `C`.

`C` ve `C++`'ın interoperability'si çok yüksek. birbirlerini direk olarak call edebiliyorlar. `kaynak: http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines title: "CPL.3: If you must use C for interfaces, use C++ in the calling code using such interfaces".`

## 📌 GCC (⟷ GNU Derleyici Koleksiyonu ⟷ GNU Compiler Collection)

Açık kaynaklıdır ve birçok programlama dilleri için (`C`, `C++`, `Objective-C`, `Fortran`, `Java`, `Ada`, `Go`) derleyici içerir.

İlk versiyonu sadece `C` derleyicisi içeriyordu. Bu sebeple eski versiyonlarının ismi yine `GCC` idi, fakat açılımı `GNU C Compiler` idi.

## 📌 g++

`GCC`'nin içindeki bir komut satırı uygulamasıdır. Sadece `C++` Compiler'ıdır. `gcc` komutu ile yapılabileceklerin aynısını sağlar. fakat sadece `C++` derlediği için, komutların `C++` için ayarlanmış olması büyük kolaylıklar sağlamaktadır.

## 📌 Bloodshed Dev-C++

`Bloodshed` firması tarafından geliştirilen sadece `MS Windows` üzerinde çalışan `C` ve `C++` için IDE'dir. compiler olarak `gcc` veya onun türevlerini kullanmaktadır.

## 📌 turbo c

`Borland` firması tarafından geliştirilen IDE ve `C` ve `C++` derleyicisidir. Adı daha sonra `Turbo C++` olmuştur. fakat proje sonlanmıştır.

## 📌 Objective-C

bu şekilde de kullanılır: __ObjC__ yada __Objective C__ yada __Obj-C__.

`Apple` ürünlerinde kullanılan fakat platform bağımsız bir dildir. object-oriented'tır.

`C` dilinden türetilmiştir. `C` diline ek olarak nesne oluşturma syntax'ları eklenmiştir. `kaynak: <https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Introduction/Introduction.html> Fakat superset/subset olayından hiç bahsedilmemiş.` Yani; object-oriented olması için yeni syntax eklenmiştir fakat var olanlar değiştirildi mi diye özel bir bilgi yok. Fakat buralarda `C`'nin `superset`'i olduğu belirtilmiş:

- https://developer.apple.com/library/archive/referencelibrary/GettingStarted/RoadMapOSX/books/WriteObjective-CCode/WriteObjective-CCode/WriteObjective-CCode.html
- https://www.informit.com/articles/article.aspx?p=1765122
- https://www.drdobbs.com/examining-objective-c/184402055

## 📌 C Sharp

`C` veya `C++`'ın `superset`'i veya `subset`'i değildir.
