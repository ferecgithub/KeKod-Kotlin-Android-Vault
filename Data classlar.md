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

### Geri dön: [[Buradan Başla - Indeks]]