# C MEMORY STRUCTURE

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 memory structure

```text
High Addresses ---> .----------------------.
                    |      Environment     |  CommandLine Argument + Environment variables
                    |----------------------|
                    |                      |   variable are declared on the stack
                    |         STACK        |
stack pointer ->    | - - - - - - - - - - -|
                    |           |          |
                    |           v          |
                    :                      :
                    .                      .   The stack grows down into unused space
                    .         Empty        .   while the heap grows up.
                    .                      .
                    .                      . 
                    .                      .
                    :                      :
                    |           ^          |
                    |           |          |
                    | - - - - - - - - - - -|
                    |          HEAP        |   Dynamic memory is declared on the heap
                    |                      |
                    |----------------------|
                    |          BSS         |   Uninitialized data (BSS)
                    |----------------------|
                    |          Data        |   Initialized data (DS)
                    |----------------------|
                    |          Text        |   Binary code
Low Addresses ----> '----------------------'
```

`C` ile derlenmiş bir uygulama çalışırken, memory'deki görüntüsü yukarıdaki gibidir. bu parçaları incelersek:

- `stack`

  static olmayan değerler burada tutulur.

  her fonksiyonun bir stack bloğu vardır.

  `LIFO` mantığında çalışır. bu sebeple `stack pointer` listenin tepesinin adresini tutar.

- `heap`

  `memory allocation` (`malloc`, `realloc` gibi) işlemleri ile buradan yer ayırtılır. buradaki değerler fonksiyonlardan bağımsızdır.

- `BSS (⟷ Uninitialized data segment)`

  static yada global olan fakat initialize olmamış değerler burada tutulur.

  initialize olmayan değerler sıfırdır. fakat yinede bu bölgede tutulurlar.

- `DS (⟷ Initialized data segment)`

  static veya global olan ve initialize olmuş değerler burada tutulur.

- `Text`

  kodumuzun binary hali burada tutulur. bütün executable burada olmak zorunda değil. `OS` ihtiyacı kadarını çekiyor.
  
  bu kısım read-only'dir. böylece yanlışlıkla buraya yazma gibi bir durum söz konusu olamaz.

## 📌 segmentation fault (⟷ segfault)

eğer memory'de erişmeye yetkimiz olmayan veya geçersiz bir adrese erişmek istediğimizde bu hatayı alırız.
