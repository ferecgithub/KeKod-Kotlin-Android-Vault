* Java ve Kotlin'de sınıflarda değişen ilk şey, sınıfın nesnesini oluştururken artık `new`anahtar kelimesini kullanmıyoruz.
```java
public static void main() {
	Human human = new Human("Ferec", "Hamitbeyli")
}
```
```kotlin
fun main() {
	val human = Human("Ferec", "Hamitbeyli")
}
```
* Java ve Kotlin'de sınıflarda değişen ikinci şey, constructor yapısıdır. Java'da parametrelere varsayılan değer veremediğimiz için constructorları overload etmemiz gerekir. Kotlin'de bu overload işlemi arka planda yapılır ve bize varsayılan değer girmemize izin verir. Bu sayede daha temiz bir kod elde ederiz. 
```java
class Human {
	private String name;
	private String surname;

	public Human() {
		name = "Ferec"
		surname = "Hamitbeyli"
	}

	public Human(String name) {
		this.name = name
	}

	public Human(String name, String surname) {
		this.name = name
		this.surname = surname
	}

	//Gereksinimlere göre getter ve setter fonksiyonları yazılır...
}
```
```kotlin
class Human(name: String, surname: String) // üye değişken olarak davranmaz ve getter/setter fonksiyonları üretilmez.
class Human(val name: String, val surname: String) // üye değişken olarak davranır sadece getter fonksiyonları üretilir.
class Human(var name: String, var surname: String) // üye değişken olarak davranır getter/setter fonksiyonları üretilir.
```
* Kotlin'de primary constructor ve secondary constructor kavramları vardır. Primary constructor sınıf adının hemen yanına parametre parantezleri açarak yazılır. Secondary contructor(lar) `constructor`anahtar kelimesi ile bir blok açılarak yazılır.
```kotlin
class Human(var name: String, var surname: String) // Primary constructor {  
    init { // Primary constructor'un body'si
        name = "Ahmet"  
        surname = "Mehmet"  
    }  
      
    constructor(name: String, surname: String, age: Int) : this(name, surname) {  // Secondary constructor
        this.name = name  
        this.surname = surname  
        println("I'm $age years old.")  
    }  
}
```
* Kotlin'de sınıflardaki `init`bloğu aslında constructor'ın body'sidir. Sınıf başlatıldığında ilk olarak çalıştırılacak bloktur. Ancak sınıf içerisindeki üye değişkenler init bloğundan daha yukarıda yazılmış ise, ilk önce o kodlar çalıştırılırlar. O yüzden `init`bloğu içinde çağıracağımız üye değişkenleri `init` bloğundan önce yazmalıyız.
* Her zaman önce primary constructor body'si çalıştırılır sonra secondary constructor body'si çalıştırılır. Java'da bir constructor diğer bir constructor'ı işaret etmediği için sadece o constructor'ın body'si çalışır.
* Secondary constructor, primary constructor'ın varsayılan değer içermeyen tüm değerlerini içermek zorundadır.
* Primary constructor'da parametredeki değişkenlere `val`veya `var`anahtar kelimelerini verirsek, artık bunlar sınıfın üye değişkenleri gibi davranırlar. Sınıf içindeki üye fonksiyonlardan da ulaşılabilirler.
* Kotlin'de kullanabileceğimiz 4 farklı visibility modifier var. Bunlar `public`, `private`, `protected`, `internal`'dır.
* Kotlin'de değişkenler arka planda aslında fonksiyon gibi davranırlar. Hafızadaki değeri yani backing field, her zaman `private`'tır. Onlara eriştiğimiz getter ve setter fonksiyonları `public`olabilir.
* Kotlin'de encapsulation yapmıyormuş gibi gözükse de aslında Java'nın aksine encapsulation'ı değiştirmemize izin bile verilmez.
* Eğer bir `val` değere ilk değer ataması yapmayıp, get fonksiyonu ile bir değer vermeye çalışırsak, bu değişkenin backing field'ı oluşmaz. `val` olduğu için set fonksiyonu da oluşturulmaz. Örneğin:
```kotlin
val size = 35
val A.isEmpty: Boolean // ilk değer ataması yapılmadığı için backing field oluşmaz
	get() = size == 0
val isEmpty: Boolean // bu da üstteki gibi davranır. Backing field'ı yoktur.
	get() = size == 0
val A.isEmpty2: Boolean = true // Extention function ile backing field oluşturamayız, o yüzden hata verecektir bu ilk değer atama.
	get() = size == 0

```
* Bir sınıfın üstünde bir top level değişkeni `const val`olarak tanımlarsak, o değer derlenme zamanı (compile-time) doldurulur. Diğer değişken tipleri çalışma zamanı (run-time) doldurulur.

### Kotlin'de obje yönelimli programlama (object oriented programming)
#### Encapsulation (Kapsüllemek)
* Encapsulation kavramı, bir sınıf içindeki üyeleri `private`olarak tutmamızı ve gerektiği takdirde ve gerektiği kadarını dışarıya açmamızı önerir. Çünkü sınıflar birbirlerinden haberdar olmazsa, katmanlı bir yapı oluşturabiliriz.
* Encapsulation'ı uygularken, her bir sınıfı, fonksiyonu ve özelliği kimin kullanacağını bilerek visibility modifier atamalıyız. Örneğin o sınıf sadece child sınıflar tarafından kullanılacaksa `protected`, eğer sadece o modül içinde kullanılacaksa `internal` kullanabiliriz.
* Kotlin'de bir değişkene `private`verirsek, aslında o değişkenin arka planda setter ve getter fonksiyonları oluşturulmaz. Dolayısı ile Kotlin'de değişkenler arka planda property olarak tutulduğu için onlara erişme veya değiştirme yapılamaz. Backing field'ı her zaman `private`'tır.
* Fonksiyon içinde lokal **olmayan** bir değişkene atama işlemi yapmadığımız sürece arka planda setter fonksiyonu oluşturulmaz.

#### Inheritance (Miras almak)
* Bir varlığın temel özelliklerini ve işlevlerini belirlediğimiz işleme miras alma diyoruz. Örneğin bir rol yapma oyunu oynadığımızı düşünelim. Burada bize "5 tane ayı postu getir." görevi verilmiş olsun. Oyunda boz ayı, kutup ayısı ve siyah ayı türlerinde ayılar var ve bunların hepsi aslında ayı sınıfından miras alan varlıklar. Ancak her birinin kendi adaptasyonları çerçevesinde implementasyonları değişiklik gösterir. Kutup ayısının postu beyaz ve yediği yiyecek fok balığı iken, boz ayının postu kahve rengi ve yediği besin nehirde avladığı somon balığıdır. Verilen görev "ayı postu" getirmek olduğu için tüm bu türlerin postu kabul olacaktır.
* Java'da `extends` ile miras alma işlemi yapılırken, Kotlin'de `:` ile aynı işlem yapılır.
* Kotlin'de Java'nın aksine sınıflar varsayılan olarak `final`'dırlar (property ve fonksiyonlar da keza öyle. Bu yüzden override'ı önler). Ancak `open` anahtar kelimesi ile sınıf, fonksiyon veya özellik miras alınabilir ve `final` ile miras alınamaz hâle getirebiliriz.
* `open`ve `final` anahtar kelimeleri Kotlin için accessibility modifier (erişim düzenleyicisi)'dır. Bu yapılar ile child sınıflarda herhangi bir sınıf, fonksiyon veya özellik miras alınabilir veya miras alınması engellenebilir.
* `final` anahtar kelimesi backing field'ı olmayan top level property veya delagete'lerde kullanılamaz.

#### Polymorphism (Çok şekillilik)
* Polymorphism override ve overload ile ilgilidir. 2 tip polymorphism vardır. **Statik** ve **dinamik** polymorphism. Dinamik polymorphism override, statik polymorphism overload ile ilgilidir. Statik denmesinin sebebi bilginin belli olmasıdır. Dinamik olmasının sebebi de bilginin belli olmamasıdır. Üst classta statik polymorphism, alt classlarda override ederek dinamik polymorphism yaparız.
  ChatGPT açıklaması:
  Polimorfizm ayrıca dinamik (runtime) ve statik (compile-time) olmak üzere iki ana kategoride de olabilir:

1. **Statik Polimorfizm (Static Polymorphism)**: Bu tür polimorfizm, derleme zamanında belirlenir ve aşırı yükleme (overloading) ile ilişkilidir. Fonksiyonlar arasındaki doğru sürümün, fonksiyon çağrısının derleme zamanındaki bağlamına göre belirlendiği durumlarda ortaya çıkar.
    
2. **Dinamik Polimorfizm (Dynamic Polymorphism)**: Bu tür polimorfizm, çalışma zamanında belirlenir ve aşırı yazma (overriding) ile ilişkilidir. Alt sınıflardaki bir yöntemin, üst sınıflardaki aynı isimli yöntemi geçersiz kılması ve çağrıldığında alt sınıftaki sürümünün çalıştırılması durumunda ortaya çıkar.
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
