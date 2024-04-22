* Java ve Kotlin'de sınıflarda değişen ilk şey, sınıfın nesnesini oluştururken artık `new`anahtar kelimesini kullanmıyoruz.
* Java ve Kotlin'de sınıflarda değişen ikinci şey, constructor yapısıdır. Java'da parametrelere varsayılan değer veremediğimiz için constructorları overload etmemiz gerekir. Kotlin'de bu overload işlemi arka planda yapılır ve bize varsayılan değer girmemize izin verir. Bu sayede daha temiz bir kod elde ederiz. 
* Kotlin'de primary constructor ve secondary constructor kavramları vardır. Primary constructor sınıf adının hemen yanına parametre parantezleri açarak yazılır. Secondary contructor(lar) `constructor`anahtar kelimesi ile bir blok açılarak yazılır.
* Kotlin'de sınıflardaki `init`bloğu aslında constructor'ın body'sidir. Sınıf başlatıldığında ilk olarak çalıştırılacak bloktur.
* Her zaman önce primary constructor body'si çalıştırılır sonra secondary constructor body'si çalıştırılır. Java'da bir constructor diğer bir constructor'ı işaret etmediği için sadece o constructor'ın body'si çalışır.
* Secondary constructor, primary constructor'ın varsayılan değer içermeyen tüm değerlerini içermek zorundadır.
* Primary constructor'da parametredeki değişkenlere `val`veya `var`anahtar kelimelerini verirsek, artık bunlar sınıfın üye değişkenleri gibi davranırlar. Sınıf içindeki üye fonksiyonlardan da ulaşılabilirler.
* Kotlin'de kullanabileceğimiz 4 farklı visibility modifier var. Bunlar `public`, `private`, `protected`, `internal`'dır.
* Kotlin'de değişkenler arka planda aslında fonksiyon gibi davranırlar. Hafızadaki değeri yani backing field, her zaman `private`'tır. Onlara eriştiğimiz getter ve setter fonksiyonları `public`olabilir.
* Kotlin'de encapsulation yapmıyormuş gibi gözükse de aslında Java'nın aksine encapsulation'ı değiştirmemize izin bile verilmez.
* Kotlin'de Java'nın aksine miras alınabilmesi veya alınamamasını belirlemek için özel anahtar kelimeler vardır. `open` ile sınıf miras alınabilir ve `final` ile sınıfı miras alınamaz hâle getirebiliriz. Varsayılan olarak Kotlin'de sınıflar `final`'dır.
  
* Sınıflarda'ki propertylere de `open` verilebilir. Bu o property'nin override edilebilir hâle getirir.
  
* Polymorphism override ve overload ile ilgilidir. 2 tip polymorphism vardır. Statik ve dinamik polymorphism. Dinamik polymorphism override, statik polymorphism overload ile ilgilidir. Statik denmesinin sebebi bilginin belli olmasıdır. Dinamik olmasının sebebi de bilginin belli olmamasıdır. Üst classta statik polymorphism, alt classlarda override ederek dinamik polymorphism yaparız.
  ChatGPT açıklaması:
  Polimorfizm ayrıca dinamik (runtime) ve statik (compile-time) olmak üzere iki ana kategoride de olabilir:

1. **Statik Polimorfizm (Static Polymorphism)**: Bu tür polimorfizm, derleme zamanında belirlenir ve aşırı yükleme (overloading) ile ilişkilidir. Fonksiyonlar arasındaki doğru sürümün, fonksiyon çağrısının derleme zamanındaki bağlamına göre belirlendiği durumlarda ortaya çıkar.
    
2. **Dinamik Polimorfizm (Dynamic Polymorphism)**: Bu tür polimorfizm, çalışma zamanında belirlenir ve aşırı yazma (overriding) ile ilişkilidir. Alt sınıflardaki bir yöntemin, üst sınıflardaki aynı isimli yöntemi geçersiz kılması ve çağrıldığında alt sınıftaki sürümünün çalıştırılması durumunda ortaya çıkar.
   
* Sd
  
  
  
  ------------
  ### Soyut Sınıflar (Abstract Classes)
  * Abstract sınıflar ile detay vermiyoruz ancak sınırlarını net olarak belirliyoruz. Örneğin insan sınıfını abstract class olarak tanımlayabiliriz ancak Adanalı sınıfını open class olarak belirleriz. Çünkü Adanalı'dan Sivaslı oluşturamayız. İkisinin de kendine özgü implementasyonu var.
  * Nesnesini oluşturacağımız bir sınıf open olmalıdır ancak nesnesini kesin olarak oluşturmayacağımız bir sınıfı abstract yapabiliriz. En az bir tane abstract methodu olması gerekli.
  * Abstract class içinde solid bir sınıfın instance'ının olmaması gerekli (genellikle). Bu yüzden o nesne dependency injection ile parametreden vermemiz gerekli. Abstract classların birden fazla constructorunun olabilmesinin temel sebeplerinden biri budur.
  * Abstract classlar, başka abstract classları miras alabiliyor. Bu durumda üst sınıftaki abstract property ve abstract fonksiyonların implemente edilmesi zorunluluğu olmaz alt abstract sınıf için. 
  * Open sınıflarda abstract yapılar bulunduramayız. Abstract keywordunu sadece abstract sınıflar içinde kullanabiliriz.
  * Bir class abstract ise bir interface'i implement ettikten sonra o interface'in fonksiyonlarını override etme zorunluluğu kalkar.

-----------
### Interfaceler
* Constructor'u olmadığı için nesnesi oluşturulamıyor. 
* Ancak `object` ile expression kullanımı şekilde yazdığımızda, arka planda interface'in bir class gibi davranıyor. Bir nesnesi oluşturuluyor. 
* Eğer bir interface'in fonksiyonuna bir body verirsek o fonksiyon opsiyonel hale gelir. Override edilmesi zorunlu olmaz. Çünkü arka planda statik bir sınıf içinde statik bir fonksiyonu oluşturulur. 
* Bir interface, başka bir interface'ten miras alabilir. SOLID'deki Interface Segregation'ı böyle sağlayabiliriz. Interface'ler şişmesin, ihtiyaç olduğu kadar bölmeliyiz.
* API'ları implemente edeceğimiz zaman, ilk önce bir interface'e implement edip sonrasında 
* Bir class birden fazla interface'i implement edebilir ancak birden fazla class'ı implement edemez. Bir interface başka bir class'ı implement edemez.
* Abstract sınıf mı interface mi kullanmalıyız konusunda, komple engellemek için abstract class kullanabiliriz çünkü abstract classlarda `final`kullanıp yeniden implement edilmesini engelleyebiliriz. Ancak interfacelerde komple engelleme yapılamıyor.
* Interface'ler state tutmazlar. Backing field oluşturulmaz içerideki değişkenler için. Ancak companion object içinde state tutabiliriz yani backing field olan değişken oluşturabiliriz. Yapısı gereği statik değişken oluşuyor ancak bu yöntemi kullanmamalıyız (edge-caselerde kullanılabilir). 

-----------
### Data classlar
* Mutlaka primary constructor'ı olan ve bu constructor'da en az bir property (val/var ile yazılmış) parametre içermesi gereken sınıflardır.  val/var şekilde yazılmalarının sebebi, içerideki üye fonksiyonlar tarafından kullanılabilmeleri içindir. İçlerinde salt veri tutmak için kullanılırlar.
* İkinci constructor içerebilirler.
* Member property tanımlanabilir.
* Visibility modifier'lar kullanılabilir.
* `open`kullanılamaz. Varsayılan olarak `final`'dır.
* `abstract`veya `open`sınıflardan miras alabilir. Interface'leri implement edebilir.
* `inner`yapılamaz ancak nested yapıda bulunabilir.
* `sealed`yapılamaz.
* Parametreler default değer alabilirler. Bu özellikle data classları JSON'a maplerken işe yarar. Gson, Moshi gibi kütüphaneler varsayılan olarak boş constructorlara ihtiyacı var. Eğer biz varsayılan değerler verirsek primary constructor'daki üyelere, nesne üretilirken boş constructor alabilir.
* Primary constructor içindeki üyeler için component fonksiyonları üretilir ancak sınıf içinde bir üye değişken yazılırsa onun için arka planda component fonksiyonu üretilmez. 
* component fonksiyonları, primary constructor içindeki her üye için component1, component2.. şekilde oluştururuz. Bu yüzden componentN fonksiyonları da denir. Bu yapı bize, data class'ın primary constructor'ının içindeki üyeleri Pair ve Triple gibi içlerinde componentN şeklinde veri tutan yapılara ayırarak dönüştürmemizi sağlar. Buna **destructing declarations** denir. Örneğin:
  
```kotlin
data class PokemonData(val name: String = "", val type: String = "", val isEvolved: Boolean = false, val age: Int = -1)

fun main() {
	val pokemonData = PokemonData("Pikachu", "Electric", false, 4)

	val pair: Pair<String, String> = Pair(pokemonData.name, pokemonData.type)
	val triple: Triple<String, String, Int> = Triple(pokemonData.name, pokemonData.type, pokemonData.age)
	
	val name = pair.first
	val type = pair.second
	
	// Bu yapıyı destructing declarations ile yazacak olursak:
	val(name2, type2) = pair
	val(name3, type3) = Pair(pokemonData.name, pokemonData.type)

	// Eğer direkt data class'ın componentN fonksiyonlarını destructing declarations ile ayırmak istersek (burada constructor'daki sıralama önemli):
	val(pokeName, pokeType, isPokeEvolved, pokeAge) = pokemonData
}
```
  
* Sınıfın içine yazılan üye değişkenler, copy, hashCode, equals ve toString fonksiyonları üretilirler kullanılmazlar.  hashCode, equals ve toString fonksiyonları Any sınıfından gelir ancak copy fonksiyonu data class içinde üretilir.
* copy fonksiyonu arka planda tüm primary constructor parametrelerini içeren bir şekilde oluşturulur.
* İdeal olarak data classlar salt veri içermeli ve mantık içermemelidir. UI için veri manipülasyonları mapperlar ile yapılıp, UI için özel oluşturulmuş data classlara çevrilmelidir.
* toString methodu dolu olduğu ve loglanabildiği için, gizliliği ihlal edilebilecek bilgileri data class yerine normal classlarda yazabiliriz. Bu durumda hackerların işini zorlaştırmış oluruz.
