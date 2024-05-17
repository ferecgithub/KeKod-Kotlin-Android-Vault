* Bir veritabanı veya retrofit instance'ını birden fazla oluşturursak, ihtiyacımız olan verilere her yerde ulaşamayız ve tutarlı veriye sahip olduğumuzu da bilemeyiz. Bu yüzden bu tip yapıların tek bir instance'ını oluştururuz (singleton).
* Singleton oluşturmamızın bir diğer sebebi de, nesneyi oluşturmanın yüksek maliyeti olduğu durumdur. Bu nesneyi başlat ve sil yerine bir tane nesne oluşturup birden fazla yerde kullanabiliriz. Hafızada yer kaplar ancak o nesneye erişim maliyeti bu durumda bizim için daha önemlidir.
* Yapıyı singleton formunda oluşturursak, yapı statik olarak hafızada korunur. Garbage collector'a bu nesneye dokunma diyoruz. Bu nesnenin kullanım ömrü ve yaşam döngüsü (lifecycle) benim elimde diyoruz. Ancak eğer çok fazla sayıda singleton oluşturursak, hafıza gereksiz yere dolacaktır.
* Java tarafında bir Singleton objesi oluşturmak için private bir constructorı olan bir sınıf kullanıyoruz. Instance objesi `volatile static` olur. Buradaki `volatile` çoklu thread ile çalışıldığında threadlere değerin değiştiğini haber vermek için kullanılır. Singleton obje üretme fonksiyonunu da `synchronized static`yaparız. Buradaki `synchronized` da çoklu thread ile çalışıldığı durumda fonksiyon bloğuna birden fazla thread'in girmemesini sağlamak içindir. Singleton bir Retrofit objesi üretmek için Java kodu aşağıdaki gibidir:
```java
class Retrofit {
	private volatile static Retrofit newInstance;

	private String baseUrl = "www.google.com";

	private Retrofit() {
	
	}

	private String getBaseUrl() {
		return baseUrl;
	}

	public void setBaseUrl(String baseUrl) {
		this.baseUrl = baseUrl;
	}

	public synchronized static Retrofit getNewInstance() {
		if(newInstance == null) {
			newInstance = Retrofit();
		}
		return newInstance;
	}
}
```
* Singleton yapısında garbage collector nesneye karışmadığı için, nesnenin kullanımı bittikten sonra bizim manuel şekilde nesneyi yok etmemiz gerekir.
* Aynı işlemi Kotlin'de sadece `object` anahtar kelimesi ile gerçekleştirebiliyoruz ve boilerplate koddan kurtulmuş oluyoruz. Kotlin'de üstteki Retrofit singleton objesini üretmek için kod aşağıdaki gibidir:
```kotlin
object Retrofit {
	var baseUrl = "www.google.com"
}

fun main() {
	Retrofit.baseUrl = "www.ferec.com"
}
```

* Arka planda oluşturulan kod Java'daki gibi thread-safe anahtar kelimeleri ile yazılmamış görünse de **dökümantasyonda `object` yapısının thread-safe olduğu belirtiliyor**.
* Object'in mirasını alamayız. Ancak `object`'e bir class veya interface'ten miras alabiliriz.
* Object Kotlin'de declaration, companion ve expression kullanımı olarak üç tipte kullanılabilir.
	*  Object'i decleration kullanımı `object` anahtar kelimesi ile başka bir değişkene atamadan veya başka bir fonksiyona parametre olarak verilmeden yapılan kullanımdır. Örneğin:
	```kotlin
object Retrofit {
	var baseUrl = "www.google.com"
}
	```
	* Object'i expression kullandığımızda, onu bir değişkene atayabiliriz. Bunlara "kullan at" yapılar da denir. Bu yapı genelde interface ve abstract class'lar ile yapılır. Onların implement edilmesi gereken fonksiyonları block içinde implemente edilir. Burada aslında ilgili interface veya abstract class'ı miras alan isimsiz bir class oluşturmuş oluyoruz. Java'da buna anonymous class deniyor. Object kullanımında static bir yapı oluşmaz arka tarafta. Örneğin:
	```kotlin
fun demo() {
	val textWatcher = object : TextWatcherClass() { // Gereksiz fonksiyonları override etmek yerine, kendi sınıfımıza/interface'imize override edip o sınıfı object olarak veriyoruz. Bu sayede daha temiz bir kod yazıyoruz.
		override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {
			println("$name, $age, $surname")
		}
	}
}
	```
	* **Normalde Kotlin ve Java'da multiple inheritance desteklenmiyor. Ancak Object'in expression kullanımı sayesinde multiple inheritance yapabiliriz.**

* `data object` veya `data class` arasındaki fark, `data object`'te arka planda class yerine singleton yapısı, equal() ve hashCode() fonksiyonları var, toString(), componentN fonksiyonları ve copy() fonksiyonu yok. Yani `data object` singleton formunda (static) olan ve yarı data yapısında bulunan bir sınıftır. Sade `object` ise singleton formunda (static) bir classtır.

####  Companion object
* Bir pattern olarak `companion object`, nesne üretmede kullanılabiliyor. `companion object`'in bodysine yeni instance oluşturacak fonksiyon yazılıp, parametre olarak o nesneyi üretmek için gereken tüm verileri verilir. Bu sayede nesneyi oluşturmak için gereken öğeler normal kullanıma göre daha açık bir şekilde görülebilir.
* `companion object`'e aynı `object`'e olduğu gibi bir class veya interface'ten miras alabiliriz.
* Arka planda `static` bir `Companion` objesi oluşturulur. Bu yüzden fonksiyonun body'sine static bir fonksiyon yazmasak bile bu obje üzerinden erişebiliriz. Yani biz `HomeFragment.newInstance()` yazsak da, aslında IDE arka planda bunu şöyle görür: `HomeFragment.Companion.newInstance()`. Ancak eğer projede Java sınıfları da varsa, bu durumda `Companion`'ı da yazmalıyız o fonksiyonu çağırırken. İstenirse `companion object`'in yanına isim verilip, `Companion`'un adı değiştirilebilir. Örneğin: `companion object MyCompanion {}`.
* Top-level sabit tanımlama ile `companion object` içinde sabit tanımlama arasındaki fark: top-level kullanımda sınıf yok edilse bile bu top-level sabit hafızada yer kaplamaya devam eder. Ancak bir sınıfın içindeki `companion object` içinde tanımlanan bir sabit, o sınıf yok edildiğinde onla birlikte yok edilir. Bu da küçük bir miktar performans artışı sağlar.