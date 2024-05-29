* Stringler her karakteri bellekte 2 byte yer kaplayan ve UTF-16 encoding kullanan yapılardır. 
* Stringler immutable'dır. Bir String ilk değer atamasından sonra değerini değiştiremez ya da yeni değer atanamaz. String üzerinde yapılan işlemler sonucunda bize yeni bir String döner. String'in ilk hâli değişmeyecektir. Sadece çöp toplayıcı (garbage collector) mekanizması kullanılmayan ilk hâli bellekten silecektir.
* Stringler değişkenin ismini Stack'te tutar. Ancak değeri Heap'te tutar. Bu değerin yerine geçecek başka bir değer yazarsak, bu değer de Heap'te yer alır ve Stack'teki değişken ismi Heap'teki yeni değeri işaret eder. Garbage collector, Heap'te işaret edilmeyen bir değer görürse onu siler.
* String tüyosu:
```kotlin
val numbersValue: String = "value" + (4 + 2 + 8) // value14
val numbersValue2: String = (4 + 2 + 8) + "value" // çalışmaz. Değer Int olarak belirlenir burada + operatörü çalışmaz.
```

* Primitive array ve object-type array olarak 2 tip array'den söz edebiliriz. Primitive arrayler ByteArray(byte[]), ShortArray(short[]), IntArray(int[]), LongArray(long[]), DoubleArray(double[]), FloatArray(float[]), BooleanArray(boolean[]), CharArray(char[]) şeklindedir (String'in primitive array yapısı yoktur). Object-type arrayler ise Array sınıfından miras aldığımız arraylerdir. İkisi arasında performans farkı vardır.  Primitive tipli arraylerde değerin adı ve değerin kendisi Stack'te tutuluyor. Boxed tipli referans değerlerde ise değerin adı Stack'te, değer ise Heap'te tutuluyordu. Dolayısı ise primitive tipler daha hızlı çalışırlar.
* Arraylerde eğer elemanlar üzerinde işlem yapmamız gerekliyse contructor'lı versiyonunu kullanırız. Eğer işlem yapmamız gerekmiyorsa fonksiyon olarak kullanabiliriz. Örneğin:

```kotlin
val someCities = arrayOf("İstabul", "Hatay", "Kars") // fonksiyon versiyonu
val someCities2 = Array<String>(5) { // constructor versiyonu
	// pseudo code ...
	3.14 * it // return değeri
}
```

* Arraylerin "invariant" olması, bir tipteki arrayin, üst tipteki array yerine atanamamasıdır. Örneğin:
  
```kotlin
val arrayOfString: Array<String> = arrayOf("Strawberry", "Banana")
val arrayOfAny: Array<Any> = arrayOfString // hata verir!
val arrayOfAny: Array<Any> = arrayOf("Strawberry", "Banana") // çalışır
```

* Array'lerde `vararg`' a denk gelen bir yapı kullanmak istersek spread (`*`) operatörünü kullanabiliriz. Bu operatör basitçe önüne geldiği dizinin elemanlarını açık bir şekilde ifade eder. Örneğin:
  
```kotlin
val fruitsArray = arrayOf("Strawberry", "Banana")
printAllStrings("Melon", "Kiwi", *fruitsArray, "Mango") 
// *fruitsArray yapısının yaptığı işlemle aslında aşağıdaki gibi bir kod yazılmış oluyor arka tarafta
printAllStrings("Melon", "Kiwi", "Strawberry", "Banana", "Mango") 

fun printAllStrings(vararg strings: String) {
	for (string in strings){
		print(string)
	}
}
```

* Array'lerde `==` operatörü farklı çalışır. Bu operatör direkt referans karşılaştırması yapar yani bellekteki yeri karşılaştırır. Dolayısı ile aynı yeri işaret etmeyen array'ler için `false` değeri döner.
  
* Değer aralığı (range) operatörleri ile sonlu sayılı bir liste oluşturabiliriz. Küçükten büyüğe bir liste oluşturmak için:
	* `..` operatörünü veya `rangeTo()` fonksiyonunu
	* `..<` operatörünü veya `rangeUntil()` fonksiyonunu kullanabiliriz. Örneğin:

```kotlin
val numberRange = 1..100 // [1,100]
val numberRange2 = 1.rangeTo(100) // [1,100]

val numberRange3 = 1..<100 // [1,100)
val numberRange4 = 1.rangeUntil(100) // [1,100)

val lettersRange = 'A'..'Z'
```

Büyükten küçüğe bir liste oluşturmak için:
 * `..` operatörünü KULLANAMAYIZ. Boş elemanlı bir aralık üretir.
 * `downTo()` fonksiyonunu kullanabiliriz. Bu fonksiyon infix olarak da kullanılabilir. Örneğin:
   
```kotlin
val numberRange = 100..1 // Çalışmaz. Boş aralık döner
val reversedNumbers = 100.downTo(1) // [100,1]
val reversedNumbers2 = 100 downTo 1 // [100,1]

val numberRange3 = 1..<100 // [1,100)
val numberRange4 = 1.rangeUntil(100) // [1,100)

val lettersRange = 'A'..'Z'
```

Adım sayısını belirlemek için `step` infix fonksiyonunu kullanabiliriz. Örneğin:

```kotlin
val steppedNumbers = 1..100 step 2
val reversedSteppedNumbers = 100.downTo(1) step 3
```

* Değer aralıklarında Progression sınıfları ile işlemler yapabiliriz. 3 tipi vardır. CharProgression, IntProgression ve LongProgression. first, last, step ve count gibi ek bilgiler de alınabilir. `Iterable<N>` interface'ini implement etmişlerdir. O sebeple map, filter gibi fonksiyonları kullanabilirler. (Aslında üstteki range türleri de IntProgression'dan miras alan IntRange türündedirler. Aynı özellikler kullanılabilir.) Örneğin:

```kotlin
val numberList: IntRange = 10 until 90
numberList.first
numberList.last // last bilgisi step'e göre dönecektir
numberList.step // getter
numberList.step = 5 // setter
numberList.count()

val biggerThanFiftyCount = numberList.count { it > 50 } // 50'den büyükleri say
```
