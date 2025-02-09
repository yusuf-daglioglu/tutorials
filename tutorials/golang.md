############################

############################
# GOLANG
############################

############################

# GO

__golang__ olarakta adlandırılmaktadır.

native derlenen bir dil. C'ye kıyasla daha basit özellikleri var. bu durum ona performans kazandırıyor.

concurrent uygulamaların az yer maliyetli şekilde çalışması için mimarisi uygun tasarlanmış. OS thread'i yerine, "goroutine" denilen kendi içinde sanal thread management'ı var. OS thread'i gibi çok bellek harcamıyor, çünkü temelde daha basit özellikler içeriyor. tabi programcının hiçbir şeyden haberi olmuyor.

# Syntax

```go
// "main" is the module name.
// the first module should be always "main".
package main

/*
multiline comment
*/

// contains functions for "formatting text".
// this is a default package so we don't have to add to go.mod file.
import (
	"fmt"

	// fmt2 is optional alias.
	fmt2 "fmt"
	"reflect"
)

// or optionally this syntax can be used:

// import "fmt"
// import fmt2 "fmt"

// package scope
const ok1 = true

func main() {

	/////////////////////////
	/// BLOCK SCOPE
	/////////////////////////
	// constants are read-only and un-changable.
	const ok2 = true

	/////////////////////////
	/// NEW VARIABLES
	/////////////////////////
	// if you dont use it, go compiler will give error.
	var b1 bool = true // typed declaration with initial value
	var b2 = true      // untyped declaration with initial value. but type is automatic assing on runtime.
	var b3 bool        // typed declaration without initial value
	b4 := true         // untyped declaration with initial value. iki nokta sadece "var" keyword'ünü yazmamamızı sağlıyor.

	// we print them so compiler will not give any error.
	fmt.Println("new variables: ", b1, b2, b3, b4)

	fmt.Println("type of: ", reflect.TypeOf(b2))

	// we define 2 values on same line.
	var firstName, lastName string = "name1", "surname1"

	fmt.Println(firstName, lastName)

	/////////////////////////
	/// NEW VARIABLES SYNTAX (another example)
	/////////////////////////

	// all below are equal:
	//
	// var (
	//   speed int
	//   velocity int
	// )
	//
	// or:
	//
	// var speed int
	// var velocity int
	//
	// or:
	//
	// var speed, velocity int

	/////////////////////////
	/// SCOPE BASED VALUES
	/////////////////////////
	// we re-define same name variable.
	// in this function ("main") this value will be overrided
	// but when "main" function ends, the value will be reverted.
	const ok1 = false

	/////////////////////////
	/// FUNCTION CALL
	/////////////////////////
    fmt.Println("hi" + "hello")
	fmt2.Println("hi")

	// function1 should be exported via "main" (same with this file module)
	// because we call it directly.
	function1()

	/////////////////////////
	/// SEMI COLON
	/////////////////////////
	// optionall semi-colon can be used:
	function1(); function1();

	/////////////////////////
	/// PARENTHESIS
	/////////////////////////
	
	// no need for paranhesis

	if 5 > 1 {
		fmt.Println("inside if")
	}

	for i:=0; i < 5; i++ {
		fmt.Println(i)
	}

	/////////////////////////
	/// TYPES
	/////////////////////////

	// integer types
	var i int // platform depended (32 or 64 bit machine has different max and min size)
	// all above integers have fixed size on all platforms.
	var i8 int8   // 8 bit (max min: -128 to 127)
	var i16 int16 // 16 bit (max min: -32768 to 32767)
	var i32 int32
	var i64 int64

	// Unsigned Integers
	// all above integers may have "u" as preffix.
	var var99 uint

	// float types
	var f32 float32
	var f64 float64

	// bool type
	var b bool

	// string types
	var s string
	var r rune  // type of numeric type
	var by byte // type of numeric type

	fmt.Println(
		i, i8, i16, i32, i64,
		var99,
		f32, f64,
		b, s, r, by,
	)


	/////////////////////////
	/// ARRAYS
	/////////////////////////
	// arrays are alwasy fixed size.

	var arr1 = [3]int{1,2,3}

	// in below line 3 (the length) is automatically set by compiler:
	var arr2 = [...]int{1,2,3}


	// we can set all or some variables:
	arr3 := [5]int{1,2} //partially set
  	arr4 := [5]int{1,2,3,4,5} //fully set

	// set only second and third variables:
	arr5 := [5]int{1:10,2:40}

	// set third element:
	arr5[2] = 50

	fmt.Println(arr1, arr2, arr3, arr4, arr5)

	/////////////////////////
	/// SLICE
	/////////////////////////

	// slice are similar with arrays but they are resizable 
	// and they don't need lenght when initialize on compile time.
	slice1 := []int{1,2,3} 

	fmt.Println(slice1)

	/////////////////////////
	/// FUNCTION CALL
	/////////////////////////

	result1, result2 := multiReturnV1(5, "XXX")

	result3, result4 := multiReturnV2(5, "XXX")
	
	fmt.Println(result1, result2, result3, result4)

	/////////////////////////
	/// blank identifier
	/////////////////////////

	// kullanmadığımız değerleri buna atıyoruz. böylece compiler hata fırlatmıyor.

	result5, _ := multiReturnV1(5, "XXX")
	
	fmt.Println(result5)
	///////////////

	var pers1 Person

	// Pers1 specification
	pers1.name = "Jack"
	pers1.age = 20

	fmt.Println("Name: ", pers1.name)
	
	var pers2 = Person{"Bob", 20}
	fmt.Println(pers2)
	
	var pers3 = Person{name: "Alice", age: 30}
	fmt.Println(pers3)

	var pers4 = Person{name: "Alice"} // age is 0.
	fmt.Println(pers4)

	// "new" is a built-in function of go.
	var pers5 = new(Person)
	// it is same with:
	var pers6 = Person{}

	// type of pers5 and 6 are also same.
	fmt.Println(pers5, pers6)

	/////////////////////////
	/// Receiver functions
	/////////////////////////
	
	receiverTest()

	/////////////////////////
	/// interface
	/////////////////////////

	interfaceTest()

	/////////////////////////
	/// pointers
	/////////////////////////

    pointerTest()
}

func sumV1(x int, y int) int {
	return x + y
}

func sumV2(x int, y int) (result int) {
	result = x + y
	return // or: return result
}

func multiReturnV1(x int, y string) (result1 int, result2 string) {
	result1 = x + 1
	result2 = y + "text"
	return
}

func multiReturnV2(x int, y string) (int, string) {
	var result1 = x + 1
	var result2 = y + "text"
	return result1, result2
}

// structure
type Person struct {
	name string
	age  int
}

/////////////////////////
/// Receiver functions
/////////////////////////

// "p Person" metoda geçilen bir parametre ile aynı görevi görür.

func (p Person) Print() {

	fmt.Println(p.name)
}

func receiverTest() {
	person := Person{name: "Jack2"}
	person.Print() // Burada person nesnesi Print metodunu çağırıyor. bu sebeple "receiver" deniliyor.
}

/////////////////////////
/// interface
/////////////////////////

type Hayvan interface {
	Ses() string
}

type Kedi struct{}

func (k Kedi) Ses() string {
	return "Miyav"
}

type Kopek struct{}

func (k Kopek) Ses() string {
	return "Hav"
}

func hayvanSesiniYazdir(h Hayvan) {
	fmt.Println(h.Ses())
}

func interfaceTest() {
	kedi := Kedi{}
	köpek := Kopek{}

	hayvanSesiniYazdir(kedi)  // Çıktı: Miyav
	hayvanSesiniYazdir(köpek) // Çıktı: Hav

	// Eğer "Hayvan" interface'inde "Run" fonksiyonu olsaydı,
	// o zaman Kedi ve Kopek, "Hayvan" olarak hayvanSesiniYazdir
	// fonksiyonuna parametre yollanamayacaktı (derleyici hata verecekti).
	// Go'da kalıtım olmadığı için, go-derleyicisi kalıtımı otomatik algılamaya çalışıyor.
	// Go yukarıdaki örnekte kalıtımı şuradan anlıyor: "Ses" fonksiyonu "Köpek" tarafından override edilmiş.
	// Demekki "Kopek", "Hayvan"'ın tüm metodlarını override etmiş. Demekki kalıtım yapılmak istenmiş.
	// Fakat "Run" fonksiyonu olsaydı, "Kopek" ve "Kedi" bunları override etmediğinden ötürü
	// kalıtım olmadığını düşünecekti.
}

/////////////////////////
/// pointers
/////////////////////////

// meaning of pointers and address (&, *) are same as C.

// we require pointers for sending parameters as reference (not their values).

func pointerTest() {

    var i = 1
    fmt.Println(i) // prints: 1

    changeIt(i)
    fmt.Println(i) // prints: 1

    changeItViaPointer(&i) 
    fmt.Println(i) // prints: 0

    fmt.Println(&i) // prints: an address



	// on C we can define directly a pointer.
	// but on go we need the value first:

	var val1 int = 99
	var po1 *int
	po1 = &val1

	fmt.Println("value", val1) // prints: 99
	fmt.Println("pointer's value", *po1) // prints: 99
	fmt.Println("pointer itself (which is an address)", po1, &val1)  // prints: same 2 addreses
	fmt.Println("&po1", &po1) // prints: address different than above

	// pointers can be nil:
	po1 = nil
	// but we can not assign nil to a value:
	// val1 = nil


	////////////
	// multiple star example (pointer of pointer)
	////////////
	var num int = 42

	var ptr1 *int = &num

	// create pointer of pointer
	var ptr2 **int = &ptr1

	// create pointer of pointer
	var ptr3 ***int = &ptr2

	// Değerleri yazdırıyoruz
	fmt.Println("num:", num)      // 42
	fmt.Println("ptr1:", *ptr1)    // 42
	fmt.Println("ptr2:", **ptr2)   // 42
	fmt.Println("ptr3:", ***ptr3) // 42

	***ptr3 = 100
	fmt.Println("num after change:", num) // prints: 100
}

// we pass integer by value, so if we change the value, 
// the parent/caller function will not see the update.
func changeIt(deger int) {
    deger = 0
}

// the sender/caller sends the integer as address,
// but changeItViaPointer function use it as pointer.
// if we change the value inside this function,
// the parent/caller function will see the change directly.
func changeItViaPointer(deger *int) {
    *deger = 0
}
```