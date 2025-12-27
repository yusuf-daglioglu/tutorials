# C STANDART LIBRARIES

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 standart kütüphaneler

bu kısım özellikle bir implementasyona göre yazılmadı.

- `assert.h`

  if `assert` function fails, it will throw exception. but we can change this behavior via compiler configuration.

  ```c
  scanf("%d", &a);

  assert(a >= 10);

  printf("Integer entered is correct");
  ```

- `ctype.h`
  - `int isalnum(int c)` is alphanumeric kontrolü
  - `int isdigit(int c)`
  - `int toupper(int c)`

- `errno.h`

  Bazı fonksiyonlar hata döndüklerinde genelde `-1` dönerler. ek olarak `errno` değerine de hata numarasını set ederler. bu integer değer `errno.h` içerisindedir.

  `errno` değeri `thread-local`'dir. bu sebeple multi-thread uygulamalarda her thread için farklı bir instance tutulur. bu sebeple exception kontrollerimizi buradan sorunsuzca yapabiliriz.

  `errno` `C` standardında `thread-local` değildir. Fakat çoğu modern `POSIX` `thread-local` olarak şekilde ayarlamıştır.

- `limits.h`

  aşağıdaki sabit değerleri barındırıyor
  - `CHAR_BIT`
  - `LONG_MAX`
  - `LONG_MIN`

- `stdio.h`

  dosya veya diğer stream'ler üzerine okuma yazma işlemleri yapabilmemizi sağlayan fonksiyonlar içerir
  - `int fclose(FILE *stream)`
  - `int printf(const char *format, ...)`

- `stdlib.h`

  Memory management, array sort, search gibi utility olarak gruplandırabileceğimiz fonksiyonlar içerir

  - `void *calloc(size_t nitems, size_t size)` - memory management fonksiyon
  - `void exit(int status)` - terminate the application
  - `void *bsearch(const void*key, const void *base, size_t nitems, size_t size, int (*compar)(const void *, const void*))` - binary search
  - `void qsort(void *base, size_t nitems, size_t size, int (*compar)(const void *, const void*))` - quicksort

- `math.h`

  - `double tanh(double x)`
  - `double log(double x)`

- `complex.h`

  matematikteki karmaşık sayılar üzerinde işlem yapmamızı sağlayan fonksiyonlar içerir

- `stdint.h`

  fixed-size variable'lar barındırır.

- `inttypes.h`

  `stdint.h`'i içinde zaten barındırıyor. bu kütüphane stdint içinde gelen fixed-size variable'ların printf, `scanf` gibi standart metotlarla çalışabilmeleri için formatter'ları barındırıyor. örnek:

  ```c
  PRId64 = "ld"
  ```

  sabiti `inttypes.h` içerisindedir. bu değer ile standart `c` kütüphane metodu olan printf ile şunu yapabiliriz:

  ```c
  printf("%+"PRId64"\n", myInt64Var);
  ```

  `C`'de string birleştirmeyi derleyici bizim için yapıyor. dolayısı ile yukarıdaki printf metodu şuna tekabül ediyor:

  ```c
  printf("%+ld\n", myInt64Var);
  ```

- `sysexits.h`

  `exit status` tam sayılarını barındırıyor. konu başka başlıkta anlatılıyor.

- `signal.h`

  Sinyal handler fonksiyonunu ve sinyallerin integer değerlerini barındırıyor. konu başka başlıkta anlatılıyor.

- `stdint.h`

sabit boyutlar tam sayılar barındıran kütüphanedir. boyutlar aşağıdaki tablodadır:

| Değişken | İşareti  | Bits | Bytes | Minimum Değer                      | Maksimum Değer                        |
|----------|----------|------|-------|------------------------------------|---------------------------------------|
| int8_t   | Signed   | 8    | 1     | −2^7 = −128                        | 2^7 − 1 = 127                         |
| uint8_t  | Unsigned | 8    | 1     | 0                                  | 2^8 − 1 = 255                         |
| int16_t  | Signed   | 16   | 2     | −2^15 = −32,768                    | 2^15 − 1 = 32,767                     |
| uint16_t | Unsigned | 16   | 2     | 0                                  | 2^16 − 1 = 65,535                     |
| int32_t  | Signed   | 32   | 4     | −2^31 = −2,147,483,648             | 2^31 − 1 = 2,147,483,647              |
| uint32_t | Unsigned | 32   | 4     | 0                                  | 2^32 − 1 = 4,294,967,295              |
| int64_t  | Signed   | 64   | 8     | −2^63 = −9,223,372,036,854,775,808 | 2^63 − 1 = 9,223,372,036,854,775,807  |
| uint64_t | Unsigned | 64   | 8     | 0                                  | 2^64 − 1 = 18,446,744,073,709,551,615 |
