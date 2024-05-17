* Kotlin'in içinde varsayılan olarak tanımlı refrection özellikleri yok. O yüzden Jetbrains'in `kotlin-reflect` kütüphanesini kullanıyoruz. ("org.jetbrains.kotlin:kotlin-reflect:1.6.0")
* Bu kütüphaneyi kullanmadan da reflection kullanabiliriz. Bu sefer Java üzerinden reflection kullanmış oluruz. Sadece araya `.java` çağrımı koyarız. Örneğin: `Account::class.java.constructors`. Burada Kotlin stiline göre daha farklı reflection fonksiyonları kullanırız. Örneğin `declaredFields(), declaredMethods()`.
* Reflection sayesinde `private` olan sınıf üyelerine erişebiliyoruz. Örneğin bir sınıfın `private` primary constructor'una şu şekilde erişebiliriz:
```kotlin
class Account(val branchName: String, val accountNumber: Int) {
	private var balance: Long = 2311235213
	private var accountName: String = "Küçük hesap"

	private fun sendMoneyFromBalance(sentMoney: Long) {
		balance = balance - sentMoney
	}
}

fun main() {
	val constructor = Account::class.primaryConstructor
	constructor?.isAccessible = true
	val account: Account = constructor?.call("Maltepe", 34001) as Account
	println(account.branchName)
}
```
* 3. parti bir kütüphanenin bir `private` bir fonksiyonuna reflection ile ulaşmadan önce kütüphane sahibine danışmak daha iyi bir fikir olabilir. O fonksiyona ulaşmanın yolunu kapatmışsa bir bildiği vardır diye düşünebiliriz.
* Eğer üstteki `Account` sınıfının üyelerine erişmek istersek şu şekilde yapabiliriz:
```kotlin
fun main() {
	Account::class.memberProperties.forEach {
		println(it.name) // bize üye değişkenlerin adlarını yazdırır. (balance, accountName, branchName, accountNumber)
	}
	
	Account::class.members.forEach {
		println(it.name) // bize fonksiyonları, miras aldığı fonksiyonları ve üye değişkenleri gösterir.
	}
}
```
* Eğer bir sınıf üyesine erişmek istersek şu şekilde erişebiliriz:
```kotlin
fun main() {
	val constructor = Account::class.primaryConstructor
	constructor?.isAccessible = true
	val account: Account = constructor?.call("Maltepe", 34001) as Account

	for(property in Account::class.memberProperties) {
		// Eğer özellik private ise ve KMutableProperty'nin alt sıfınına aitse
		if(property.visibility == KVisibility.PRIVATE && property is kotlin.reflect.KMutableProperty) {
			// Erişilebilir hâle getir
			property.isAccessible = true

			// Alanın değerini oku veya değiştir
			if(property.name == "balance") {
				val privateFieldValue = property.getter.call(account)
				println("privateField değeri: $privateFieldValue)
			}
		}	 
	}
}
```

* Reflection maliyetli bir işlemdir. O yüzden zorunlu kalmadıkça kullanmamamız gerekir.
* Android projelerde minify ile kod muğlaklaştırma (code obfuscation) yaparsak, sınıfların veya üyelerin isimleri değişeceği ve String yapıların sabit kalması sebebiyle reflection kodu hata verir. Bu hatayı almamak için reflection yaptığımız kısmı ProGuard'ın dışında tutmamız gerekir.
* 3. parti bir kütüphane kullanıyorsak, onun reflection yapıp yapmadığını ve buna özel ProGuard kuralları olup olmadığına bakmalıyız. Eğer test modunda çalışan ancak release modda çalışmayan bir uygulamamız varsa, reflection kurallarının eksik olması konusu muhtemeldir.