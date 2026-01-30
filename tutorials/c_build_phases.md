# C BUILD PHASES

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 fazlar (⟷ phases)

- `preprocess`

  derleme öncesi çalışır. başka başlıkta anlatılan keyword'lerin (`ifdef` gibi) çalıştığı aşamadır. bu fazın çıktısı yine source koddur. fakat bir kısmı `ifdef` gibi tanımlar sebebi ile silinmiştir.

- `compile`

  kod, makinenin anlayacağı dile çevriliyor. bu faz sonucunda `object code (⟷ object file)` elde edilir.

  genelde `.o` uzantılıdır. `C`'den türemiş bazı dillerde __.obj__ olarak da kullanılır.

  `object code`'da diğer kütüphaneler henüz `link`'lenmediği için düzgün çalışamaz.

- `link`

  kütüphanelerin birleştirildiği/`link`'lendiği fazdır. artık `object code`'umuz executable olmuştur.

## 📌 file formats

arayüzler `.h` (`header file (üst bilgi dosyası)`) uzantılı, diğer tüm kodlar `.cpp` uzantılı text dosyalarında saklanması önerilir. bazı repo'larda `C++` header'ları `C` header'ları ile karışmasın diye `.hpp` uzantılı kaydedilir.

`C`'de de bu şekildedir. header dosyasında fonksiyon definition'larımız durur. fonksiyonların implementasyon kodları `.h` dosyasında olmamalıdır. fakat olursa yine çalışır. fakat best practice değildir.

`.h` dosyalarının implementasyonları `.c` dosyalarında olmalıdır. `.c` dosyası ile "`.h`" dosyası aynı isimde olma gibi bir zorunluluk yoktur.

## 📌 "iostream" vs "iostream.h"

farklı dosyalardır. `C++` ilk çıktığında `iostream.h` kullanılıyordu. fakat namespace verilmemişti. daha sonrada `std` namespace'i altına taşındıklarında eski kodlar etkilenmesin diye `iostream` isimli kütüphane (dosya) altında dağıtılmaya başlandı.

## 📌 DLL

`DLL` dosyaları sadece implementasyonları barındıran dosyalardır. Bu sebeple; `DLL` dosyasını `C` veya `C++`'tan kullanabilmek için, `DLL`'e tekabül eden (`C` veya `C++` için uygun formatta olan) header dosyasına ihtiyacımız vardır. Bu header dosyalarını build sırasında bulundurmak zorundayız, yoksa kodumuz compile etmez.

Eğer `DLL` dinamik ise (runtime sırasına belirlenebilir), `OS` tarafından bize verilmelidir.

Fakat `DLL` dosyaları portable olduklarından, onları `Exe` içerisine gömüp de kullanabiliriz.
