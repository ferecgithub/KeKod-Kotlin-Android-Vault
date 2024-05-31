#### by ile delegation
* Sınırlandırılmış hiyerarşiler (enum class, sealed class gibi) kullandığımızda, her yeni case için when yapısını doldurmamız gerekir. Ancak bu işi daha otomatize edebilmek de mümkündür. Bunun için `by` anahtar kelimesi ile delegation yani işi iteleme yapabiliriz.
* Kısacası delegation,  kullandığımız sınıfların ve arayüzlerin (interfacelerin) işlerini yapabilecek bir sınıf sağlamış olur.
* Manuel şekilde delegation yapmak delegate ettiğimiz sınıf veya interface büyüdükçe, deletagete yaptığımız fonksiyonun da şişmesine neden olur. O yüzden `by` anahtar kelimesi ise delegate yapmak geliştiriciyi bu dertten kurtarır. Ancak arka planda IDE kendisi bu işi üstlenir.
* Interfaceleri veya herhangi bir süper tipi implemente etmiş somut sınıfları delegate edersek, o somut sınıflarda yazdığımız ekstra fonksiyonlara ulaşamayız delegation ile.
* Eğer **property seviyesinde delegation** yapmak istersek, bir class yazıp orada `getValue` ve `setValue` operator fonksiyonlarını **overload** edersen (çünkü her property'nin kendi get ve set operatörleri vardır normalde) ve içerisindeki kodu kendi amaçlarına göre güncellersen (örn. get içinde cache'ten oku, set içinde cache'e yaz gibi), `by` anahtar kelimesi ile istenilen property'i bu class'a delegate edebiliriz. Örneğin:
```kotlin
class DatabasDelegate(private val db: Database, private val key: String) {
/***
thisRef ile delegate eden property'e ulaşabiliyoruz. Örneğin name veya email.
KProperty bize o property'nin özelliklerini gösteriyor.
*/
	operator fun getValue(thisRef: Any?, property: KProperty<*>): String {
		return db.loadData(key)
	}

	operator fun setValue(thisRef: Any?, property: KProperty<*>, value: String) {
		db.saveData(key, value)
	}
}

class User(db: Database) {
	var name: String by DatabaseDelegate(db, "name")
	var email: String by DatabaseDelegate(db, "email")

}
```
* Üstteki örnekte görüldüğü gibi bir property delegation işlemini özellikle içinde aynı görevi yapan get ve set operatörleri olan sınıflar gördüğümüzde uygulayabiliriz.
* Delegation'ın bir avantajı da implementasyonları soyutlamasıdır. Yani property'ler implementasyon ile ilgilenmiyorlar ve o implementasyon delegation sınıfından değiştirildiğinde delegation yaptığımız property'lerin hepsi bu değişikliği kabul etmiş oluyor.

#### by lazy ile delegation
* Bir nesnenin veya property'nin hafızada yer kaplamaya başlaması, o nesne veya property'nin oluşturulması ile başlar. 
* Ağır bir nesne oluşturma süreci, kullanıcı deneyimini etkiler. Hem de hafızayı gereksiz olarak doldurabiliriz. 
* `by lazy` ile delegation avantajları
	* **Değerlendirme (evaluation)**: `by lazy` ile tanımlanan bir değişken, ilk erişildiğinde değerlendirilir ve değeri hesaplanır. Sonraki erişimlerde ise hesaplanan değer doğrudan döndürülür. Bu, özellikle hesaplaması maliyetli veya zaman alan işlemler için performans avantajı sağlar.
	* **Null safety**: `by lazy` ile tanımlanan değişkenler nullable yapılabilir. Bu nedenle kullanmadan önce null olup olmadığına bakmamız gerekebilir.
	* **Thread safety**: `by lazy` varsayılan olarak thread-safe'dir. Yani aynı değişkene birden fazla thread aynı anda eriştiğinde, doğru değer güvenli bir şekilde döndürülür.
	* **Kullanım durumu**: Genellikle, hesaplaması maliyetli olan, ancak her zaman kullanılmayabilecek değerler için tercih edilir. Örneğin, bir konfigurasyon dosyasını okumak veya bir ağ isteği yapmak gibi işlemler için idealdir.
* `by lazy`'nin tembel başlatma modları:
	* **SYNCHRONIZED**: lazy değerine ilk erişimde tüm thread'leri bloklar ve value alanının oluşturulmasını tek bir thread'e sınıflar. Varsayılan mod budur.
	* **PUBLICATION**: value alanının oluşturulmasında tek bir thread'e sınırlama getirmez. Birden fazla thread, value alanını aynı anda oluşturabilir, ancak oluşturma işlemi tamamlandıktan sonra sadece bir thread'e value alanına erişim izni verilir. Diğer thread'ler oluşturma işlemi bitinceye kadar bekler.
	* **NONE**: thread güvenliği sağlamaz. Birden fazla thread, aynı anda value alanını oluşturabilir ve bu oluşturma işlemi sırasında herhangi bir senkronizasyon yapılmaz. Bu nedenle, bu mod, yalnızda tek thread'in erişimine uygun olan durumlarda kullanılmalıdır. (***NONE en performanslısı ancak tek bir thread kullanılacağından emin olmalıyız.***)
* Hangi modu kullanırsak kullanalım, bir kere değer atandıktan sonra o değer kullanılır. Çoklu thread erişimi işlemi değer atanmadan önce gerçekleşir.
* `by lazy` ile delagate eden property'ler `val` olmak zorundadırlar. Çünkü `Lazy<T>` interface'indeki `public val value: T` değerinin set `operatörü` yoktur. Bu yüzden `var` kullanmaya izin vermez.
* Kullanım örneği:
```kotlin
val lazyValue: Int by lazy(LazyThreadSafetyMode.SYNCHRONIZED)
```
#### lateinit ile delegation
* `by lazy` ile delegation avantajları
	* **Değerlendirme (evaluation)**: `lateinit` ile tanımlanan bir değişken, başlangıçta bir değer atanmadan tanımlanır. Değer, kodun ilerleyen bir noktasında atanmalıdır.
	* **Null safety**: `lateinit` ile tanımlanan değişkenler null olamaz (non-nullable). Bu, güvenli bir şekilde kullanılabileceklerini garanti eder.
	* **Thread safety**: `lateinit` varsayılan olarak thread-safe değildir. Eğer birden fazla thread aynı anda erişirse, beklenmedik sonuçlar ortaya çıkabilir.
	* **Kullanım durumu**: Genellikle, yaşam döngüsü boyunca değeri dışarından atanacak olan değişkenler için tercih edilir. Örneğin, bir dependency injection framework'ü tarafından atanacak olan veya bir aktivitenin onCreate metodunda atanacak olan değişkenler için idealdir.
* `lateinit`, `by lazy`'e göre daha risklidir. Çünkü `by lazy`'de ilk değerin ne olacağı bellidir ancak `lateinit`'te değeri biz atamalıyız. Bunu KProperty içindeki `isInitialized()` fonksiyonu ile kontrol edebiliriz. Örneğin:
```kotlin
fun printProperty() {
	if(::lateInitializedProperty.isInitialized) {
		println("Value of lateInitializedProperty: $lateInitializedProperty")
	} else {
		initializeProperty()
		printProperty()
	}
}
```
* `by lazy`'nin aldığı değer sonradan değiştirilemiyor ancak `lateinit`'in değeri sonradan değiştirilebilir. 