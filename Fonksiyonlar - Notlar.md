* Kotlin'de fonksiyonlara varsayılan değer vererek fonksiyon overload'ı gerçekleştirebiliyoruz.  Java tarafında 2 farklı fonksiyon oluşturuluyor. Birisi varsayılan değer alan hâl, diğeri varsayılan değer almayan hâl.
* Bir fonksiyonun, ismi aynı kalarak birden fazla kere parametre sayısı veya parametlerin tipleri değiştirilerek yeniden yazmaya fonksiyon aşırı yükleme (fonksiyon overloading) denir.
* Matematiksel operatörlerin, tip dönüşümlerinin (type conversions), aralık (range) kullanımının, `infix` methodlara karşı işlem önceliği vardır. 
* `infix` methodların da mantık operatörlerine göre işlem önceliği vardır.
* `infix` fonksiyonların performansa katkısı **yoktur**. Sadece kodu daha okunabilir kılar.
* Bir fonksiyon eğer herhangi bir sınıf içinde değil de, bir dosyanın içinde boşlukta tanımlanıyorsa buna `top level function` adı verilir.
* Bir fonksiyon bir sınıfın dışına da yazılabiliyorsa (yani `top level function` olarak), buna ***fonksiyon*** denir. Eğer yazılamıyorsa buna ***method*** denir.
* Başka bir fonksiyonun scope'u içinde tanımlanan fonksiyonlara, ***lokal fonksiyonlar*** denir. Bunu yaparak fonksiyonların yazılmış olduğu sınıfın içinden bile çağrılabilmesini engelleyebiliriz. Hatta reflection ile bile çağrılamazlar. Kesinlikle dışarıdan fonksiyona ulaşmaya ve değiştirmeye izin vermemek amacıyla kullanılır.
* Üzerinde değişiklik yapamadığımız (read-only) veya yapmak istemediğimiz kadar büyük sınıflara ekstra fonksiyonalite ekleyebilmemizi sağlayan fonksiyonlara `extension functions` denir. Ancak arka planda o sınıfa bir ekleme yapılmaz. Bu fonksiyonlar, üzerinde extension fonksiyon yazdığımız `receiver` olarak tanımladığımız sınıflara ihtiyaç duyar. Örneğin aşağıdaki extension fonksiyonda receiver tipimiz `String`'dir.

```kotlin
fun String.extPrint(handsomeValue: HandsomeOne): Unit {

}
```

* Extension fonksiyonlar eğer `open` bir sınıfa yazıldıysa, ondan miras alan sınıflar da bunu override edebilirler.
* Nullable extension fonksiyonlar da yazılabilir.
* Companion object'lere de extension fonksiyonlar yazılabilir.
* Higher-order fonksiyonlar, parametre olarak en az bir fonksiyon alan veya geri dönüş tipi bir fonksiyon olan fonksiyonlardır. High-order fonksiyonlar geriye dönük değer göndermek için kullanılan yapılardır. Eskiden bunun için interface'ler kullanılırdı. 
* Higher-order fonksiyonlarda Java tarafında bir fonksiyon objesi oluşturulur ve parametre olarak verilir.
* Kotlin'de fonksiyonların `first class citizen` olmasının sebebi higher-order fonksiyonların parametre olarak verilebilmesi ve dönüş değeri gibi döndürülebilmesidir. Bir değişkene değer olarak verilebilme konusu zaten normal fonksiyonlar için de geçerli bir durum.
* Extension fonksiyonları lambda veya isimsiz fonksiyon olarak 2 şekilde gösterebiliriz. Örneğin:
  
```kotlin
val sumFunction = { numberOne: Int, numberTwo: Int ->
	numberOne + numberTwo
}

val extractionFunction = fun(numberOne: Int, numberTwo: Int): Int {
	return numberOne - numberTwo
}
```

* Kotlin'de `::` operatörü "referans oluşturma operatörü" olarak adlandırılır. Bu operatör, bir fonksiyonu veya bir sınıfı referans olarak almanıza olanak tanır. Higher-order fonksiyonları, fonksiyonun dönüş tipi aynı olması koşuluyla eğer aynı sayıda, aynı tipte parametreler verilmişse `::` operatörü ile çağırabiliriz. Başka bir deyişle, o fonksiyonun bodysini veriyoruz `::` ile o parametreye veya değere.  Örneğin:
  
```kotlin
val calculateAndPrint(numberOne:Int, numberTwo:Int, operation:(numberOne: Int, numberTwo: Int) -> Int)): Unit { // normalde higher-order fonksiyonlar bir fonksiyona parametre olarak verildiğinde, higher-oprder fonksiyon içindeki parametre isimleri verilmez. Sadece öğrenirken daha kolay görülmesi için verdi hoca. Normalde şöyle yazılır: operation:(Int,Int) -> Int)
	val result = operation(numberOne, numberTwo) // bunun yerine operation.invoke(numberOne, numberTwo) kullanabilirdik. invoke'un avantajı nullable değerleri de karşılayabilmesi. Örn: operation?.invoke(numberOne, numberTwo)
	println("Result: $result")
}

fun divFunction(numberOne:Int, numberTwo:Int): Int {
	return numberOne / numberTwo
}

fun main() {
	val numberOne = 100
	val numberTwo = 25
	calculateAndPrint(numberOne, numberTwo, ::divFunction)
}
```

* Eğer bir extension fonksiyon bir fonksiyonun tek veya son parametresi ile Kotlin'deki `trailing lambda` özelliği sayesinde parametre parantezleri dışında bir süslü parantez bloğu açarak kullanılabilir.
* Higher-order fonksiyonları extension fonksiyon olarak da yazabiliriz. Örneğin:
  
```kotlin
val calculateAndPrint(
	numberOne:Int = 3, 
	numberTwo:Int = 4, 
	operation: String.(Int, Int) -> Int
) { 
	val result = operation("", numberOne, numberTwo) 
	println("Result: $result")
}

fun main() {
	val numberOne = 100
	val numberTwo = 25
	calculateAndPrint(numberOne, numberTwo, { numberOne, numberTwo -> 
		println("$this: $numberOne, $numberTwo") // buradaki `this` ifadesi String'i ifade ediyor.
		35
	})
}
```

* Higher-order fonksiyonlar ile ilgili 3 anahtar kelime önem taşır. Bunlar: `inline`, `noinline` ve `crossinline`'dır.
* Parametre olarak en az bir fonksiyon almayan (higher-order olmayan) bir fonksiyonu `inline` yapmamızın bir anlamı yoktur. Bir higher-order fonksiyona `inline` **yazmadığımızda** ve byte kodu Java tarafında derlendiğinde parametresindeki çağrılan her fonksiyon için bir obje üretiliyor.
* Bir higher-order fonksiyonu `inline`olarak yazdığımızda, hem o fonksiyonun içindekiler, hem de fonksiyonun çağrıldığı yerdeki lambdası içindeki herşey `inline`yapılmış olur. Arka planda objeleri oluşturulmaz.
* Eğer `inline`'ı her yerde kullanırsak, byte kod büyüklüğünü arttıracaktır. Ancak performans yönünden kazanç sağlayacağız. (Zaman karmaşıklığı azalıyor, yer karmaşıklığı artıyor)
* `noinline` ve `crossinline` anahtar kelimeleri, fonksiyon `inline` olmadan anlamsızdır.
* Bir `inline` higher-order fonksiyonun parametresindeki bir fonksiyonu, body içinde başka bir `inline` olmayan higher-order fonksiyona parametre olarak verirsek, IDE bize hata gösterecektir çünkü obje üretmesi gerekir. Bu durumda parametre içindeki bu fonksiyonun önüne `noinline` getirmeliyiz. Java koduna derlendiği zaman, önünde `noinline` olan fonksiyonların nesnelerinin oluşturulduğunu görürüz. Önünde `noinline` olmayan fonksiyonların body'si ise direkt olarak bu fonksiyonun body'sine yapıştırılır.
* `inline` ve tek bir fonksiyonu parametre olarak almış bir fonksiyonda parametreyi `noline`yapmak gereksiz olur. Çünkü `inline`ile almak istediğimiz performans artışını `noinline` ile  baltalamış oluruz. Normal bir higher-order fonksiyondan bir farkı kalmaz.
* `inline`fonksiyonlarda `return`anahtar kelimesinin kullanımına izin verilir. Bir `inline` higher-order fonksiyon içinde `return` kullanırsak, bunun çıkış yeri o higher-order fonksiyonun dışındaki fonksiyon olur. Dolayısı ile o higher-order fonksiyonda `return`'un altındaki değerler çalışmaz. Higher-order fonksiyonlarda dönüş değerini `return` ile değil de, bloğun en sonuna yazdığımız değer ile belirliyoruz. `inline` bir fonksiyonun içine yazdığımız `return`'e `non-local return` diyoruz. Label kullanarak higher-order içindeki `return`'un `continue`gibi çalışmasını da sağlayabiliriz. **Higher-order fonksiyon eğer `inline` değilse, `non-local return` kullanamayız. Dolayısı ile `inline` bir fonksiyonun içinde `inline`olmayan bir fonksiyon çağırırsak, IDE bizden `return`çağırmayacağımızı garanti etmemizi isteyecektir ve hata verecektir.** 
* `non-local return`çok çekirdekli (multi-threaded) işlemlerde tehlike oluşturabilir. Bir threadin işlemi bitmeden diğer thread return etmiş olabilir.
* Eğer `inline` fonksiyonun parametresindeki fonksiyon başka bir higher-order fonksiyona parametre olarak verilecekse, `non-local return` konusundan dolayı izin verilmez. Bu durumda parametredeki fonksiyon için `crossinline`kullanırız. Bu şekilde artık parametredeki fonksiyon `non-local return` ihtimaline karşı hata almayız **ve `return` keywordüne izin verilmez.** Diğer bir deyişle `crossinline` zaten `non-local return`kullanmadığımızı taahhüt eden bir keyworddür. IDE'de emin oluyor bu şekilde. Ayrıca `crossinline`keywordü `inline`özelliğini korur yani parametredeki fonksiyonun nesnesi oluşturulmaz. 
* `inline`ile fonksiyon return edebilir hâle gelir. Ancak body içinde `inline`olmayan başka bir fonksiyon içinde return çağırılabileceği için IDE hata verir ve `crossinline`yazmamız istenir.

![[inline_modifiers.png]]
Resim kaynağı: https://www.youtube.com/watch?v=T9sAlxqYFYc