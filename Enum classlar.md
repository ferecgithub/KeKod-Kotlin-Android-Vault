* Gruplayabildiğimiz, aynı tipe sahip öğelerin bulunduğu sınıflardır. Bu kısıtlama sayesinde yazılım geliştiricisinin yapabileceği hata azaltılmış olur.
* Visibility modifier'ları alabilirler.
* Başka sınıflardan miras alamaz ancak interface implement edilebilir. Çünkü enum sınıflar değerler (value) ile ilgilidir, sınıflar ile ilgili değildir.
* Primary constructor `private`olduğu için nesnesi oluşturulamaz.
* Eğer bir `enum class`'ın içine `abstract` bir fonksiyon yazarsak veya bir interface implement edersek, bu durumda enum sabitleri de bu fonksiyonları override etmeli veya `enum class` bodysi içinde tanımlamak zorundayız.
* Eğer primary constructor'da val veya var ile property yazarsak, bu property'e dışarıdan erişilebilir. `val` ise arka planda get fonksiyonu oluşturulur, `var` ise get/set fonksiyonları oluşturulur.
* Enum class'a tanımlı bazı fonksiyonlar:
```kotlin
enum class Team(val starCount: Int) {
	FENERBAHCE(5),
	GALATASARAY(4)
	BESIKTAS(3)
}

fun main() {
	val besiktasStars = Team.BESIKTAS.starCount
	val galatasarayStars = Team.valueOf("GALATASARAY").starCount
	val teams1: Array<Team> = Team.values() // önerilen entries kullanımıdır
	val teams2: Array<Team> = Team.entries

	val besiktasName = Team.BESIKTAS.name
	val besiktasOrdinal = Team.BESIKTAS.ordinal // array'deki indeksini verir.
}
```

* Any sınıfından gelen equals, hashCode ve toString fonksiyonlarını override edebiliriz.
* Enum sabitlerinin `name` ve `ordinal` olmak üzere 2 ayrı property'e sahiptirler. `name` özelliği toString'den bağımsızdır. O enum sabitinin birebir ismini döner. Eğer bir format kaygımız varsa toString'i override etmek daha faydalı olabilir.
* Enum sabitleri arka planda statik sınıf olarak oluşturulurlar ve üzerindeki enum sınıfını miras alırlar.
* Enum sınıflarının içindeki düz fonksiyonlara, statik olmadıkları için dışarıdan erişilemez.

### Geri dön: [[Buradan Başla - Indeks]]