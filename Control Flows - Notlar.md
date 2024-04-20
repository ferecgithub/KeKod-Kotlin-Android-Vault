* Nullable bir değerde kontrol operatörleri ile hata vermeden kontrol etmek için bu kontrol yapısından önce o değer null ise `return` yap gibi bir kontrol ifadesi yazarsak, derleyici bizim için akıllı tip dönüşümü (smart casting) yapacak ve aşağıdaki kontrol operatörleri değerin null olmayacağını bildiği için artık hata vermeyecektir.
* Bir sınıf için farklı operatörleri çalıştırabilmek için, o sınıf içine operator overloading yapmak gerekir. Bunu `operator` anahtar kelimesi ile başlayan fonksiyonlar yaratarak yapabiliriz. Çıkarma için `minus` operator fonksiyonunu veya toplama için `plus` operator fonksiyonunu overload edebiliriz. Örneğin:
  
```kotlin
data class PairNumber(val numberOne: Int, val numberTwo: Int) {
	operator fun minus(pairNumber: PairNumber): PairNumber {
		val localNumberOne = numberOne - pairNumber.numberOne // eğer overload yazarken operator anahtar kelimesini kullanmasaydık, operatorün kendisi çalışmayacaktı.
		val localNumberTwo = numberTwo - pairNumber.numberTwo

		val resultPairObject = PairNumber(localNumberOne, localNumberTwo)

		return resultPairObject
	}
}

fun main() {
	val pairNumber1 = PairNumber(5, 7)
	val pairNumber2 = PairNumber(7, 9)

	val extractedPairNumbers1 = pairNumber1 - pairNumber2
	val extractedPairNumbers2 = pairNumber1.minus(pairNumber2)
}
```

* Bir if/else kontrol yapısını bir ifadeye atarsak buna expression kullanımı, eğer atamaz ve sade şekilde yazarsak buna state kullanımı diyoruz. Expression kullanımında her bloğun sonundaki değer ifadenin alacağı değer olacaktır ve aynı tipten olmalıdır (veya tip belirtmeyebiliriz ve tip çıkarımı yapılmasını sağlayabiliriz).
  
* Performans olarak if/else if/else yazmak, alt alta sadece if blokları yazmaktan daha performanslıdır. Çünkü if/else if/else bloklarında koşul tuttuğunda aşağıyı kontrol etmezken, alt alta if koşulları yazarsak tüm if koşulları kontrol edilecektir.
  
* when yapısında aynı kod bloğunu çalıştıracak koşulların arasına `,` koyulabilir. Örneğin:
    
```kotlin
when(countryCode) {
	"tr", "az" -> println("Türk vatandaşı")
	"us" -> println("Amerikan vatandaşı")
	"fr" -> println("Fransız vatandaşı")
}
```

* when yapısına parametre girmeden kullanmamak gerekli genellikle. Buna istisna olarak, eğer mantık operatörleri kullanacaksak bu şekilde kullanılabilir. Yoksa if/else if/else yapısından farkı kalmıyor. Örneğin:
  
```kotlin
when {
	(name == "Ferec").and(surname == "Hamitbeyli) -> println("Bu benim")
	(name == "Herec").and(surname == "Famitbeyli) -> println("Bu ben değilim")
	else -> println("Bu hiçbiri değilim")
}
```

* when yapısı içinde `is` veya `!is` anahtar kelimelerini bir sınıfın referansı olup olmadığını kontrol etmek için kullanacağız.

* when yapısı içinde `in` veya `!in` anahtar kelimelerini bir aralıkta olup olmadığını kontrol etmek için kullanacağız.
  
