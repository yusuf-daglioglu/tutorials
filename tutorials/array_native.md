# ARRAY NATIVE

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 Multidimensional Array (⟷ çok boyutlu dizi)

`C#` Example of: `two-dimensional (2d) array` of 4 rows and 2 columns.

```c#
string[,] array = new string[4, 2] {
                                      { "one", "two" },
                                      { "three", "four" },
                                      { "five", "six" }
                                    };

```

`C#` Example of: `three-dimensional array (⟷ 3d array)`:

```c#
int[,,] array = new int[2, 2, 3] {
                                   { { 1, 2, 3 }, { 4, 5, 6 } },
                                   { { 7, 8, 9 }, { 10, 11, 12 } }
                                 };
```

## 📌 jagged array (⟷ ragged array ⟷ array of array)

`jagged` kelime anlamı: sivri.

`ragged` kelime anlamı: eski püskü.

`multidimensional array` is something different. It should not be mixed.

This feature is supported by `Java` and `C#`.

The below `jagged array` example is syntax is valid for both `Java` and `C#`:

```java
int[][] c;
c = new int[2][]; // creates 2 rows
c[0] = new int[5]; // 5 columns for row 0
c[1] = new int[3]; // create 3 columns for row 1
```

### 📌📌 variable-length array (⟷ VLA ⟷ variable-sized array ⟷ runtime-sized array)

`Java` bu özelliği destekliyor.

`C` diline `C99` sürümü ile bu özellik gelmiştir. `C`'de daha öncesinde array ancak şu 2 şekilde oluşturulabiliyordu:

- array size'ı ancak define'da tanımlanan bir değer ile oluşturulabiliyordu
- `malloc` ile bellek alanı tahsis edilen bir array pointer'ı yaratılıyordu

`VLA` sayesinde, `C99` ile artık runtime'de oluşturulan bir integer değeri ile de dizi oluşturabiliyoruz. Örnek:

```c
int i = 5;
int array[i];
```

`C++` dilinde bu özellik yok (eski sürümlerinde yoktu. yeni sürümlerinde gelmiş olabilir). Fakat `gcc` derleyicisi bu özelliği destekleyecek şekilde bir yeteneği var. fakat resmi `C++` spesifikasyonlarında bu özellik desteklenmiyor.

`Flexible array member`, `C99` ile gelen `VLA`'dan farklı bir özelliktir. `VLA` ile karıştırılmamalı.

### 📌📌 dynamic array (⟷ growable array ⟷ resizable array)

`dynamic size array` veya `dynamic length array` isimlendirilebilir.

Bazı makalelerde `dynamic size array` veya `dynamic array`, `VLA` yerine kullanılıyor. Çünkü array'i boyutu runtime'da belirlenip array init edilebiliyor. Oysa bu terimler farklıdır.

`dynamic array`; size'ı belirlenmiş ve init edilmiş bir dizinin boyutunun değiştirilebilir olması özelliğidir.

`dynamic array`'i bir programlama dilin desteklemesi mümkün değil. Genelde bunu 2 dolaylı yöntem ile çözmüş gibi gösteriyorlar:

- arka planda yeni array oluşturan wrapper sınıflar bu altyapıyı destekler gibi gözükmektedir. Örnek: `Java`'da `ArrayList`.
- Pointer destekleyen dillerde (örnek: `C`), pointer'lar ile bunu yapabiliriz. Fakat aslında pointer'lar, native array olarak sayılmazlar.
