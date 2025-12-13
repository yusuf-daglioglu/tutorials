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
                    .                      .   (other memory maps do occur here, such
                    .                      .    as dynamic libraries, and different memory
                    :                      :    allocate)
                    |           ^          |
                    |           |          |
                    | - - - - - - - - - - -|   Dynamic memory is declared on the heap
                    |          HEAP        |
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

  `memory allocation` (`alloc`, `realloc` gibi) işlemleri ile buradan yer ayırtılır. buradaki değerler fonksiyonlardan bağımsızdır.

- `BSS (⟷ Uninitialized data segment)`

  static yada global olan fakat initialize olmamış değerler burada tutulur. (aslında initialize doğru değil. çünkü tanımlama yapılınca zaten örneğin integer'lara 0 değeri atanmış oluyor. onlarda burada tutuluyor.)

- `DS (⟷ Initialized data segment)`

  static veya global olan ve initialize olmuş değerler burada tutulur.

- `Text`

  kodumuzun binary hali burada tutulur. bu kısım read-only'dir. böylece yanlışlıkla buraya yazma gibi bir durum söz konusu olamaz.

## 📌 segmentation fault (⟷ segfault)

eğer memory'de erişmeye yetkimiz olmayan veya geçersiz bir adrese erişmek istediğimizde bu hatayı alırız.
