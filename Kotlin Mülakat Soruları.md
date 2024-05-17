
---------------------
### [[Temel Tipler - Notlar]]

---------------------

1. **Bir val değer ile var değerin arasında performans farkı var mıdır? Hangisi daha performanslıdır?**
	val veya var değerlerin arasında ciddi bir performans farkı yoktur. Ancak çok büyük verilerle çalıştığımızda bu konu önem kazanabilir.
	Eğer uygulama çok-işparçacıklı (multi-threaded) bir uygulama değilse, var daha performanslıdır. Çünkü val'ın aksine var'da değer atanıp atanmadığını kontrol etme mekanizması yoktur. Ancak eğer uygulama çok-işparçacıklı (multi-threaded) bir uygulama ise, val daha performanslı olacaktır çünkü thread'ler de kendi aralarında değer sorgulama yapacakları için bu da bir performans kalemi olarak sayılacaktır.
	
2. **"Tip Çıkarımı" (Type inference) kavramını açıklayın. Hangi durumlarda tip belirtmek kesin olarak gereklidir?**
   Tip çıkarımı, değişkenlerin veri tipine göre otomatik olarak belirlenmesidir. Örneğin biz `var name = "Ferec"` yazdığımızda, IDE otomatik olarak `name` değişkeninin tipini String olarak atar. Eğer değişkenimizi bir class içerisinde tanımlıyorsak, bir değer ataması yapmamız şarttır.Fakat eğer lokal olarak bir fonksiyonun içerisinde bir değişken tanımlıyorsak, değişkeninin tipini belirttiğimiz takdirde başlangıç değeri vermemiz şart değildir.
   
```kotlin
fun runForward() {
	var velocity: Int // Burada başlangıç değeri vermemiz gerekmiyor.
}

class Runner() {
	val name: String? = null // Burada başlangıç değeri vermemiz şarttır.
}
```

3. **Eğer bir değişkene tip belirtimi yapılmaz ve bir değer atanırsa, Kotlin tip çıkarımını nasıl yapar?**
	Kotlin derleyicisi, atanan değere göre uygun veri tipini çıkarır. Örneğin atanan değer bir sayı ise bunun değer aralığına bakarak, değişkenin tipini belirler. Sayılar özelinde, küçük sayıları biz özellikle belirtmediğimiz taktirde `Int` olarak kabul edecektir. Sayı `Int` aralığından çıktığında `Long` tipini çıkaracaktır.

4. **Bir `var` değişkeni `val` gibi davranmasını nasıl sağlayabiliriz `val` kelimesini kullanmadan? Bunu neden yapmak isteriz? Örnek bir senaryo verin.**
	Bir `var` değişkeninin `val` gibi davranmasını yani yeniden atama yapılmamasını sağlamak için setter fonksiyonunu private yaparız. Kotlin'de bir sınıf içinde tanımladığımız değişkenler aslında Java tarafında property'lere denk geldiği için ve varsayılan olarak private ile enkapsüle edilmiş olduğu için setter fonksiyonuna müdahale ederek onun hiç değiştirememesini sağlayabiliriz. Örneğin:
	
```kotlin
class Cat() {
	var name: String = "Stray cat"

	var pawAmount: Int = 4
		private set

	fun setPawAmount(amount: Int) { // değişkeni bir fonksiyon yardımıyla daha güvenli bir şekilde değiştirmesini sağlayabiliriz.
		if(amount <4) {
			// hata ver
			println("Not enought paws, sorry!")
		} else {
			// kabul et
			pawAmount = amount
		}
	}
}

fun main() {
	val cat = Cat()

	cat.pawAmount = 3 // Değiştirmeye izin vermeyecek, hata verecektir.
	
	cat.setPawAmount(5) // Bu yöntemde değiştirmesine izin verebiliriz.
}
```

5. **"Değişmez" (Immutable) ve "Salt Okunur" (ReadOnly) kavramlarını açıklayın. `val` değişkenler neden aslında "değişmez" değil de "salt okunur" olarak açıklanmalıdır?**
	Değişmez (immutable) değerler oluşturulduktan sonra hiçbir şekilde değeri değişmeyen değerlerdir. Salt okunur (read-only) değerler ise bir kere atanabilen (set edilen) değerlerdir ve değerleri başka bir değere bağlı olması halinde değişebilecek değerlerdir. `val` değerler salt okunur değerlerdir çünkü sadece bir kere değer ataması yapılıyor ancak değerleri başka bir değere bağlı ise değişebilir. Örneğin:
	
```kotlin
fun main() {
    val police = Police()
    police.dispatchPolice()
}

class Police() {
    var officerName: String = "Officer Jack"
    var officerRank: Int = 4
    val officerBadge: String
        get() {
            return "Badge: $officerName, Rank: $officerRank"
        }

    fun dispatchPolice() {
        println(officerBadge)
        officerName = "Officer Malfoy"
        officerRank = 5
        println(officerBadge)
        officerName = "Officer Harry"
        officerRank = 3
        println(officerBadge)
    }
}
```

6. **Bir değişkene null değer atanır ve tip belirtilmezse Kotlin bu değişkeni nasıl yorumlar?**
	Bir değişkene null değeri atayıp, o değişkenin tipini belirtmezsek Kotlin bu değişkenin tipini `Nothing?` olarak belirler. `Nothing` o işlevin hiçbir zaman bir değer döndürmeyeceğini ifade eder. `Nothing?` sadece null değer alabilir ve başka hiçbir değeri temsil etmez.

7. **"Null Güvenliği" (Null Safety) kavramını açıklayın.**
	Kotlin'de bulunan null güvenliği konsepti, kod yazarken NullPointerException hatalarını almamızın önüne geçmeyi amaçlar. Bu sayede null gelmemesi gereken değerleri eğer değer null ise hata verebilecek işleme sokmayız ve hata ayıklama işlemlerimizi daha da karmaşıklaştırmamış oluruz. Kotlin'de null güvenliğini Java'da olan if/else yapısı yanında let scope fonksiyonu, elvis fonksiyonu ile sağlayabiliriz. Null olup olmadığını belirlemede `?` ve `!!` operatörlerini kullanırız. Tabii burada `!!` operatörünü kullanırken son derece dikkatli olmalıyız çünkü bu IDE'ye o değerin null olmadığını belirtir. `?` ve `!!` kullanımı aslında işlemimizin kritikliğine göre değişir. Eğer kullanıcıya göstermemiz gereken hesaplama çok kritikse o zaman uygulamanın çökmesini de göze alarak `!!` kullanırız. Ancak işlem çok kritik değilse o hâlde `?` kullanabiliriz.

8. **Sekizlik (Octal) değişkenler Java'da nasıl tanımlanır? Kotlin'de Sekizlik değişken tanımlanabilir mi?**
	Java'da sayıların başına 0 ön eki getirilerek sekizlik sayılar ifade edilir. Ancak Kotlin'de bu geçerli değildir.Kotlin'de doğrudan bir sayısal ifadeyi sekizlik olarak tanımlayamayız. Bunun için Integer sınıfından parseInt veya toInt fonksiyonlarını kullanabiliriz. Örneğin:

```kotlin
fun main() {
        val numberToParse = 120

        octalToDecimalUsingParseInt(numberToParse.toString()) // çıktı 80'dir.

        octalToDecimalUsingToInt(numberToParse.toString()) // çıktı 80'dir.
}

fun octalToDecimalUsingParseInt(octal: String): Int {
	return Integer.parseInt(octal, 8)
}

fun octalToDecimalUsingToInt(octal: String): Int {
	return octal.toInt(8)
}
```

9. **Kotlin'de boxed ve unboxed değerleri açıklayınız.**
	Kotlin'de ilkel (primitive) değişkenler sınıf gibi görünse de, özel optimizasyonlar sayesinde Java'daki ilkel karşılıklarına dönüştürülürler. Ancak eğer tip Kotlin'de nullable olarak ifade edilmişse, bu sefer Java'daki sınıf tipindeki yapılara dönüştürülür. Bunlar daha çok yer kaplar ve ilkel yapılara göre nispeten daha yavaş çalışırlar. İlkel yapılar daha az yetenekli iken, sınıf tipindeki karşılıkları daha yeteneklidirler. 
	Bir değişkenin obje referansı ile tutulmasına ***boxed*** değişken denir. 
	Bir değişkenin ilkel yapıda tutulmasına ***unboxed*** değişken denir.

10. **`==` ile neyi karşılaştırırız? `===` ile neyi karşılaştırırız?**
	`==` operatörü iki değeri karşılaştırır ve değerler eşit ise `true` döndürür. `===` operatörü iki referansı karşılaştırır ve iki nesne de bellekte aynı yeri işaret ediyorsa `true` döndürür.
	
11. **`===` operatörü ile karşılaştırma yaparken Byte değer aralığı neden önemlidir? Kotlin bu aralığa göre neden özel bir davranış sergiler?**
	`===` operatörü ile -128 ile 127 arasındaki byte değerler için bellekte tek bir bölüm oluşturulur. Bu performans optimizasyonu için yapılmıştır. Bu aralık dışındaki değerler için her seferinde yeni bir yer ayırımı yapılır. Bu durumda `===` operatörü ile -128 ile 127 değer aralığındaki değerleri karşılaştırırken bellekteki yer aynı olduğu için değer farklı bile olsa `true` döner. Bu aralık dışındaki değerler için hem değer hem de bellekteki adresin karşılaştırmasını yapar.

12. **Sayısal değişkenlerde örtük tip genişletme (implicit widening conversions) ne demektir? Kotlin'de bu neden yapılamaz?**
	Sayısal değişkenlerde örtük tip genişletme, bir veri türünün daha küçük bir türden daha büyük bir türe dönüştürülmesidir. Bu dönüşüm, genellikle otomatik olarak gerçekleşir ve programcının belirli bir dönüşüm işlemi gerçekleştirmesine gerek kalmaz. Örneğin, bir `byte` türünün bir `int` türüne otomatik olarak genişletilmesi gibi.
	Kotlin'de örtük (implicit) tip genişletme yapılamaz çünkü Kotlin, Java'dan farklı olarak daha sıkı bir tür sistemi sunar. Bu sıkı tür sistemi, tür uyumsuzluklarını belirlemekte daha hassas olduğundan, otomatik genişletme işlemleri mümkün değildir. Bu nedenle, Kotlin'de sayısal değerler arasında tür dönüşümleri açıkça belirtilmelidir, yani örtük genişletme desteklenmez. Bu, tür uyumluluğunun daha net ve daha güvenli bir şekilde kontrol edilmesini sağlar ve tür hatalarını önler. Örneğin:
	
```kotlin
val randomByte: Byte = 7

val randomNumber: Int = randomByte.toInt() // Byte tipindeki veriyi açık bir şekilde Int'e çevirdik.
```

**Not1:** Numbers sınıfından miras alan sınıflarda küçük değer aralığına sahip tipler, büyük değer aralığına sahip tiplere sorunsuz, açık olarak (explicit) dönüştürülebilirler. Ancak tersini yaparken dikkatli olmalıyız. Değerimizin daha düşük aralığa sahip tipin aralığında olduğundan emin olmamız gereklidir.

**Not2:** Bir işlemde sonuç değeri işleme giren elemanların tiplerinin dışına çıkarsa, Kotlin otomatik tip dönüştürme ***yapmaz***.

13. **İlkel bir değişkenin nullable olması ile null değer alamaması arasında bellek yönetimi açısından nasıl farklar vardır?**
	İlkel tipler Stack denilen hafıza yapısında tutulur. Obje tipleri ise değerleri Heap'te, adresleri ise Stack'te tutulur. Heap, Stack'e göre nispeted daha yavaş çalışır. Bir değerin nullable yapılması, onun bir obje tipine çevirdiği için ilkel bir tipe göre daha nispeten daha yavaş çalışmasını ve daha fazla bellek harcamasını sağlar.

14. **"Akıllı Dönüşüm" (Smart Cast) ne demektir? Farklı kod örnekleri ile açıklayın. Bu özelliğin sınırlamaları nelerdir?**
	Akıllı Dönüşüm (Smart Cast), bir nesnenin türünün kontrol edildiği bir akış kontrolü ifadesinde, `is` operatörü kullanıldıktan sonra, Kotlin'in otomatik olarak bu nesneyi belirtilen türe dönüştürmesi ve bu türe özgü işlemleri gerçekleştirmenizi sağlamasıdır. Örneğin:

```kotlin
fun findTheSuspect(clue: Any) {
	if (clue is String) {
		println("$clue is the murder letter. Its length is ${clue.length}")
	} else {
		if (clue is Date) {
			println("$clue is the murder date. The murder date is ${clue.time}")
		} else {
			println("$clue is useless!")
		}
	}
}
```

15. **Neden eğer iki `Int` değerinin toplamı veya çarpımı `Int` değer aralığını aşarsa Kotlin işlemin sonucunu `Long`'a çevirmez?**
	Çünkü nadir karşılaşılabilecek bir durumdan dolayı her işlem için ekstra bir koşul vermek performans kaybına neden olur. Defansiv programlama yapmak performans kaybına neden olabileceği için ihtiyacımızı analiz ederek ona göre bir çözüme gitmek daha mantıklı olacaktır. Örnek olarak:
	
```kotlin
val result: Int = Int.MAX_VALUE + Int.MAX_VALUE // Bu durumda hatalı bir sonuç alırız çünkü Kotlin otomatik olarak Long tipine çevirmeyecektir.
```
	
16.  **Bir `val charNumber: Char = '6'` değeri `Int`'e çevirilirse sonuç ne olur?**
	`Char` değerler   `Int`'e çevilirse, ASCII tablosundaki karşılığı döner. '6' değeri için 54 değeri dönecektir.

17.  **Kotlin'de Unicode Char nasıl yazılır? Bir Unicode simgenin rengi nasıl değiştirilir ve varsayılan renge döndürülür?**
	Kotlin'de Unicode karakterleri, `\u` önekini kullanarak yazılabilir. Örneğin:
	
```kotlin
val blackHeart = '\u2665' // ♥
val blackHeart2 = '\u2764' // ❤

val ANSI_RED = "\u001B[31m" // Rengi kırmızıya çevirir.
val ANSI_RESET = "\u001B[0m" // Rengi varsayılan renge döndürür.
```

18.  **Kotlin'de `&&` ve `||` operatörleri lazily çalışma mekanizmasına sahiptir. Bu ne demektir?**
	`&&` (ve) operatörünün solu `false` ise sağdaki değere bakmaya gerek duymadan `false` kabul eder.
	`||` (veya) operatörünün solu `true` ise sağdaki değere bakmaya gerek duymadan `true` kabul eder.

19.  **Bir bankacılık uygulamasında Amerika tarafındaki müşteri için ondalık ayracını `.` ve büyük sayıları ayırmak için `,`kullanmalıyız ve Türkiye tarafında bu işaretleri tam tersi olarak kullanmalıyız. Bunu uzun bir mantık yazmadan nasıl oluştururuz?**
	Bunu `String.format()` ile oluşturabiliriz. Örneğin:

```kotlin
val number = 1234567.89
val numberInUSFormat = String.format(Locale.US, "US formatında: %,.2f", number)
println(numberInUSFormat) // "US formatında: 1,234,567.89" yazdırır.

val localeTR = Locale("tr", "TR") // Türkçe için Locale nesnesi oluşturulur.
val numberInTRFormat = String.format(localeTR, "TR formatında: %,.2f", number)
println(numberInTRFormat) // "TR formatında: 1.234.567,89" yazdırır.
```

**Not:** `%,.2f` ifadesinde `,` karakteri varsayılan olarak US sayıldığı ve ayırma ifadesinin `,` olduğu için var. Locale olarak TR verirsek, arkaplanda `,` yerine `.` kullanılacak.

20.  **`val mixedArray = array<Any>(13, "Ahmet", 'V', true)` şeklindeki bir dizinin içindeki tipler primitive midir yoksa boxed tiplerini mi içerir? **
    Any tipindeki bir dizi içindeki veriler boxed hâllerini içerir. 
    
**Not:** `array<Any>` veri tipinde `<>` yapısı, arayüzü (interface) temsil eder. Burada bize soru sorulurken, "Any arayüzünde herhangi bir değişken alabilen dizi" de diyebiliriz.

21. **Array'ler (diziler) immutable mıdır yoksa mutable mıdır?**
Bu sorunu 3 farklı bakış açısından cevabı vadır:
	i. `var` tipindeki bir dizi mutable'dır çünkü yeni bir dizi atayabiliriz. `val` varsa immutable'dır diyebiliriz.
	ii. Array'in `var` veya `val` olmasından bağımsız şekilde, dizinin her indeksindeki değeri değiştirebildiğimiz için mutable'dır.
	iii. Array'ler de aynı String'ler gibi (bellek yönünden) immutabledır. Yani dizilere yeni bir eleman eklersek, eski diziyi işaret etmeyi bırakıp yeni diziyi işaret eder. Örneğin:

```kotlin
val someCities = arrayOf("İstabul", "Hatay", "Kars")
someCities += "Sivas" // üstteki array hafızada kalmaya devam eder ve Sivas'ın eklendiği yeni bir array oluşturulur. Bundan sonra someCities yeni arrayi işaret eder ve eski array garbage collector tarafından silinir.
```

22. **Arraylerde toCharArray() gibi tipe özel tanımlı fonksiyonlar ve toTypedArray() fonksiyonu ne işe yarar?**
`toCharArray()` gibi fonksiyonlar object-type bir array'i primitive bir array'e dönüştürür.
`toTypedArray()` gibi fonksiyonlar primitive bir array'i object-type bir array'e dönüştürür.

23. **Arraylerde contentEquals() ve contentDeepEquals() fonksiyonu ne işe yarar?**
`contentEquals()` tek boyutlu fonksiyonlarda elemanları karşılaştırır.
`contentDeepEquals()` çok boyutlu (multi-dimensional) fonksiyonlarda elemanları karşılaştırır.

---------------------
### [[Control Flows - Notlar]]

---------------------

24. **++ operatörünün değişkenin başına veya sonuna yazılmasının ne farkı vardır?**
`++` operatörünün değişkenden önce yazılması önce işlemi yap anlamına gelir ve eğer print edilecekse sonra print eder. `++` operatörünün değişkenden sonra yazılması ise önce print et sonra işlemi yap demek olur.

25. **compareTo() fonksiyonu nasıl çalışır?**
Soldaki sayı yani üzerinde compareTo() çağırdığımız sayı, sağdaki sayıdan küçükse -1 değerini döner. Büyükse +1 değerini döner. İki değer de eşitse 0 değerini döner.

26. **Aritmetik operatörleri ve onların fonksiyon hâllerini ne zaman kullanmalıyız?**
Sayılardan en az biri nullable ise aritmetik operatörleri kullanamayız. Onların fonksiyon hâllerini kullanmak daha mantıklı olacaktır. Örneğin, eğer en az bir değer nullable ise `+` yerine `plus()` kullanabiliriz. Eğer değerlerin ikisi de nullable değilse operatörün kendisini kullanmak, daha az karşılaştırma yapacağımız için daha performanslı olacaktır.

---------------------
### [[Döngüler - Notlar]]

---------------------

27. **`break` ve `continue` anahtar kelimeleri ne işe yarar? İç içe döngülerde nasıl istenilen bir dış döngüye bu kelimeler ile müdahale edilebilir?**
    `break` anahtar kelimesi döngüyü bitirir. `continue` anahtar kelimesi ise döngü adımını atlar. İç içe döngülerde eğer üstteki döngüyü bitirme veya adım atlama yapmak istersen label koymalıyız. Örneğin:
  
```kotlin
returnLabel@ for (value in 1..50) {
	for (value2 in 0..10) {
		if (value2 == 5) {
			break@returnLabel // bu şekilde üstteki label verdiğimiz dış döngü bitirilecek.
		}
	}
}

returnLabel@ for (value in 1..50) {
	for (value2 in 0..10) {
		if (value2 == 5) {
			continue@returnLabel // bu şekilde üstteki label verdiğimiz dış döngünün geçerli adımı geçilecek.
		}
	}
}
```


28. **`while` döngüsü ne zaman  for döngüsünün yerine kullanılır?**
    `while` döngüsü, "for döngüsünün içinde if" yapısında olduğu için bu durumda for yerine kullanılabilir.


---------------------
### [[Fonksiyonlar - Notlar]]

---------------------

29. **`Unit` ile `Nothing` arasında ne fark vardır?**
    `Unit` bir geri değer döndürmeyen işlevlerde kullanılır. Java'daki `void` yerine geçen bu yapı, genelde bir işlevin yan etkileri yürütmesi ama bir değer döndürmemesi gerektiğinde kullanılır.
    `Nothing` türü, işlevin hiçbir şekilde tamamlanamayacağını belirtmek için kullanılır. Yani, işlevin herhangi bir şekilde geri dönmesi veya tamamlanması mümkün değildir. Genellikle bir işlevin istisna fırlattığı veya tür belirtilmeyen ve değeri null olan değerlerde kullanılır.

30. **Yazılımda `Birinci sınıf vatandaş (First class citizen)` ne demektir?**
	Eğer bir yapı; 
		i. bir değişkene değer olarak verilebiliyorsa, 
		ii. bir fonksiyona parametre olarak verilebiliyorsa veya 
		iii. bir fonksiyonun geri dönüş değeri olarak verilebiliyorsa bu yapı `first class citizen`'dır. 
	Örneğin fonksiyonlar ve değişkenler Kotlin'de `first class citizen`'dır.

31. **Fonksiyon overload nedir? Ne zaman kullanırsınız? / Varsayılan değerli/named argumentli fonksiyon nedir? Neden kullanırsınız?**
     Bir fonksiyonun, ismi aynı kalarak birden fazla kere parametre sayısı veya parametlerin tipleri değiştirilerek yeniden yazmaya fonksiyon aşırı yükleme (fonksiyon overloading) denir. 
     Eğer fonksiyonda bazı parametreler opsiyonel olacaksa, bu değerlere varsayılan bir değer verebiliriz ve fonksiyonu varsayılan değer alan parametreler olmadan çağırabiliriz.
     
32. **`vararg` fonksiyonlarda ne zaman daha performanslı çalışır?**
     Bir fonksiyonun parametresinde `vararg` eğer tek başına veya son parametre olarak yazılırsa, Java tarafında `String...` gibi derleneceği için daha fazla performans verecektir. Eğer `vararg` birden fazla parametre arasında sona yazılmazsa, bu sefer Array'e dönüştürülecektir ve daha az performans verir.

**Not:** Kodun okunaklığı, bu küçük performans farkından daha önemlidir. Ancak eğer performans farkı gözle görülür hâlde ise üstteki şekilde kullanılmalıdır.

33. **Bir fonksiyonun `infix` fonksiyon olabilmesi için hangi şartları sağlamalıdır?**
     `infix` fonksiyonlar daha okunaklı bir kod yazmak amacıyla kullanılır ve . (nokta) kullanımını kaldırır.
     Bir fonksiyonu `infix` hâle getirmek için 5 şart vardır:
     1. `infix` anahtar kelimesi ile başlar.
     2. Fonksiyon bir üye fonksiyon yani bir sınıfa ait fonksiyon olmalıdır.
     3. Ya da bir extension fonksiyon olmalıdır.
     4. Sadece bir parametre almalıdır ve bu parametre `vararg` olamaz.
     5. `infix` fonksiyon parametresi varsayılan değer alamaz.
        
     Örneğin:
        
```kotlin
infix fun infixMethod(justOneParam: AwesomeParam): Whatever {}
```

34. **Extension fonksiyonları açıkla.**
     Extension fonksiyonlar salt okunur (read-only) bir sınıfı veya üzerinde direkt olarak ekleme yapmak istemediğimiz bir sınıfın üzerine bir fonksiyonu eklemiş gibi göstermemizi sağlar. Ancak arka planda o sınıfa bir ekleme yapmaz. Extension fonksiyonlar aslında çok yeni birşey değil. Java tarafında static utility fonksiyonu yazıp, o sınıfı ve diğer değerleri parametre olarak fonksiyona verip yazabiliyorduk. Ancak extension fonksiyonlar bu işlemi fonksiyonel programlamaya daha uygun bir yazımla yapmamızı sağladı. **Extension fonksiyonlar ancak bir `top level fonksiyon` ise genişletilen sınıflarda görünür hâle gelir. Eğer bir sınıf içinde ise gelmez.** Dolayısı ile extension fonksiyonlar üye bir fonksiyon (method) gibi durak ancak gerçekte üye fonksiyon olmayan fonksiyonlardır.

35. **Extension fonksiyonlar arka planda nasıl çalışırlar?**
     Extension fonksiyonlar Java tarafında static utility fonksiyonlarına karşılık gelir. Basitçe hem extension yazığımız değerin kendisini (bunu `this` ile görürüz) hem de extension fonksiyonun parametresindeki değeri parametre olarak alır ve üzerlerinde işlem yapar.

35. **Extension fonksiyonlar ile bir fonksiyonu genişletebiliriz. Biz ayrıca değişkenleri de genişletebiliyoruz. Bunu nasıl yaparız?**
     Kotlin'de değişkenler aslında Java'da property olarak tanımlanırlar. Yani değişkenler Java'da değişken her zaman `private`'tır ve sadece get ve set methodları dışarıya açıktır. Backing field dediğimiz değişkenin kendisi açık değildir. Dolayısı ile get ve set kullanarak bir değişkeni de bir sınıftan extend edebiliriz. Ancak extension property'lerin içinde `field` tanımlanamaz (backing fieldları yoktur). Dolayısı ile gerçek anlamda bir değişken extension edilemez. Arka planda değişken genişletme aslında fonksiyon genişletme ile aynı şekilde çalışır. Örneğin:
     
```kotlin
var Shape.type
	get() = "Rectangle"
	set(value) {
		type = value
	}

//Burada eğer aşağıdaki gibi birşey yapsaydık çalışmazdı çünkü backing field oluşturamayacaktı.
var Shape.type = "Rectangle" // çalışmaz!

fun main() {
	val shape = Shape()
	shape.type = "45"
}
```

36. **Kotlin'de birden fazla sınıftan miras almamızı sağlayan yapı nedir?**
     Context receiver'lardır. Kotlin'de varsayılan olarak multi-inheritance desteklenmiyor ancak Context receiver'lar ile extension fonksiyonlarla birden fazla sınıf miras alınabilir.
     https://blog.rockthejvm.com/kotlin-context-receivers/

37. **Higher-order fonksiyonların interface'e göre bir performans avantajı vardır. Bu avantajı nasıl elde ederiz?**
     `inline` anahtar kelime sayesinde, higher-order bir fonksiyonun parametresinde geçen fonksiyonları alıp, direkt bloğa yazmasını sağlayabiliriz. Bu şekilde arka planda fonksiyon için ekstra objeler üretilmeyeceği için performans avatantajı sağlarız.
     
38. **Hangi durumlarda `inline` kullanmamalıyız?**
     `inline` anahtar kelimesiyle build zamanı konusundan feragat ediyoruz (özellikle büyük body'li fonksiyonlar için). Eğer çok güçlü bir bilgisayarımız varsa ve build süremiz çok kısaysa, inline konusu problem olmayacaktır. Ancak ekibin bilgisayarları çok üst düzey değilse çok sık kullanılmayan ve scope'u nispeten büyük olan fonksiyonları `inline` yapmamamamız önerilir.
     Ancak çok çağrılan ve büyük body'li bir fonksiyon varsa, burada performans artışını seçip build zamanından feragat etmeliyiz yani `inline` kullanmalıyız. Kısacası, ***kullanıcı deneyimi > build zamanı***.
 39. **Hangi durumlarda `noinline` kullanmalıyız?**
     Bazı durumlarda bir higher-order fonksiyonun parametresindeki bir fonksiyonu `inline` yapmak istesek bile, inline yapılamaz. Bu durumlardan biri, parametredeki bir fonksiyonu, başka bir `inline` olmayan higher-order fonksiyona verdiğimizde gerçekleşir. Burada obje üretilme durumu olacağı için `noinline`kullanmalıyız. Veya parametredeki fonksiyonun nesnesini üretmek gerekiyorsa kullanmalıyız. Örneğin, aşağıdaki fonksiyonda run fonksiyonu `inline` olduğu için arka planda nesnesi üretilmez ancak logger `noinline` olduğu için arka planda nesnesi üretilir.:
```kotlin
inline fun runAndPrint(run: (String) -> Unit, noinline logger: (String) -> Unit) {
    log(logger) // noinline yapmazsak hata verir.
    run("Message")
    logger("End")
    }

	fun log(logger: (String) -> Unit) { // bunu inline yapsaydık üstteki logger'a noinline keywordu vermemiz gerekmezdi.
		logger("Start")
```
     
 40. **Bir higher-order fonksiyon, diğer bir higher-order fonksiyona parametre olarak veriliyorsa ve bu parametredeki fonksiyonda `crossinline`anahtar kelimesini kullanmayıp, `non-local return`'e izin verseydik ne gibi bir sorunla karşılaşırdık?**
     `inline`bir fonksiyonun body'si içine yazıldığı fonksiyona yapıştırılır. Eğer fonksiyonun içinde `return`kullanılıyorsa, bu fonksiyon normal bir fonksiyon da içerebileceği için (örn. değiştiremeyeceğimiz bir fonksiyon olan Button sınıfında tanımlı setOnClickListener gibi, IDE bunu bilemez) izin verilmez. `crossinline`ile biz bu sorunun oluşmayacağını taahhüt ediyoruz.
  41. **`non-local return` neden `inline` olmayan higher-order fonksiyonlarda kullanılmaz?**
	`inline`olmayan lambda'lar expression şekilde kullanılabilir (bir değişkene atanabilir) veya daha sonra çalıştırılabilir (istenirse başka bir thread üzerinde de çalıştırabilir). Bu lambda'lar herhangi bir bağlamda, herhangi bir zaman çalıştırabileceği için ve dönüşün en yakın kapsayıcı fonksiyon olmadığı için `non-local return`'lere izin verilmez. 

42. **`inline` bir fonksiyonu  `non-local return` konusunda hata vermesine rağmen `crossinline`kullanmadan nasıl hatayı giderebiliriz?**
	Kotlin'deki `contract`'ları kullanabiliriz parametreye `crossinline` vermek istemiyorsak. `contract` ile IDE'ye `non-local return` gelmeyeceğini taahhüt ediyoruz ve bu şekilde hata vermiyor.
	

---------------------
### [[Sınıflar - Notlar]]

---------------------
 43. **Kotlin'de bir değişkenin arka planda property olarak değil de, Java'daki değişken olarak çevrildiği durumu anlatın.**
	    Kotlin'de bir fonksiyonun içinde lokal bir değişken tanımlarsak, bu değişken arka planda Java'daki değişken gibi yazılır. Yani artık bu değişken property değil backing field'dır.
 44. **Arayüzler (interface) ve soyut sınıflar (abstract class) arasında ne fark vardır?**
    * İnterface'ler state tutmazlar yani değişkenler backing field tutmazlar. Ancak abstract classlar state tutabilirler.
    * Kotlin'de çoklu miras desteklenmiyor. Bu yüzden tek bir sınıftan miras alınabilir. Ancak interfaceler daha fazla esneklik verir çünkü birden fazla interface'ten miras alınabilir.
    * abstract fonksiyonları implement etmek zorundayız çünkü bodysi yoktur ancak interface'te fonksiyonları opsiyonel yapabiliriz. Bunu body yazarak (süslü parantez) yapabiliriz. Interface'te fonksiyonlara body açınca, o fonksiyon statik bir sınıfın bir fonksiyonu gibi çevrilir.
    * Interface'te state tutabiliriz ancak bu önerilmeyen bir yöntem. Companion object içinde tutulabilir. Ancak interface içinde state tutulmamalıdır.
    * Abstract class içindeki fonksiyonlara `final` anahtar kelimesi yazarak yeniden implementasyonunu engelleyebiliriz. Ancak interfaceler yapıları gereği `final` kelimesi kullanılamaz. Bu yüzden yeniden implementasyonu engelleme yapamayız.
45. **Soyut sınıflar (abstract class) ve açık sınıflar (open class) arasında ne fark vardır?**
    * Eğer soyut (abstract) bir bilgi gerekecekse yani miras alacak sınıflarda detaylar farklılaşacaksa bu yapıyı abstract class yapsak daha iyi bir pratiktir. Ancak gerekmeyecekse open class yaparız.
    * Aynı UI'a sahip iki ekran varsa ve detayları da aynı ise, bu yapıların ayrıntılarını içeren bir open class oluşturabiliriz. Bu open class da tüm fragmentlerin ortak özelliklerini miras alabileceği bir abstract sınıftan miras alabilir. 
    * Bir sınıfın kesin olarak alması gereken özellikleri zorunlu olarak (developer hatasına yer bırakmayacak şekilde) implement edilmesini sağlamak için abstract class'tan miras alırız. Eğer varsayılan değerleri kullanarak bir implementasyon yapacaksak open class kullanabiliriz.
46. **Data classlar ile normal classlar arasında ne fark vardır?**
    * İçlerinde salt veri tutmak için kullanılırlar. Mutlaka primary constructor'a sahip olmalı ve bu constructor içinde mutlaka en az bir val/var ile yazılmış parametre olması gerekli. val/var şekilde yazılmalarının sebebi, içerideki üye fonksiyonlar tarafından kullanılabilmeleri içindir.
    * Parametreler default değer alabilirler. Bu özellikle data classları JSON'a maplerken işe yarar. Gson, Moshi gibi kütüphaneler varsayılan olarak boş constructorlara ihtiyacı var. Eğer biz varsayılan değerler verirsek primary constructor'daki üyelere, nesne üretilirken boş constructor alabilir.
    * Body'li veya body'siz kullanılabilir.
    * Varsayılan olarak `final`'dır. `open`yapılamaz. Ancak başka bir `abstract`sınıf veya `open`sınıfı implement edebilir. 
    * Primary constructor içindeki üyeler için component fonksiyonları üretilir ancak sınıf içinde bir üye değişken yazılırsa onun için arka planda component fonksiyonu üretilmez. 
    * component fonksiyonları, primary constructor içindeki her üye için component1, component2.. şekilde oluştururuz. Bu yüzden componentN fonksiyonları da denir. Bu yapı bize, data class'ın primary constructor'ının içindeki üyeleri Pair ve Triple gibi içlerinde componentN şeklinde veri tutan yapılara ayırarak dönüştürmemizi sağlar. Buna **destructing declarations** denir.
    * Sınıfın içine yazılan üye değişkenler, copy, hashCode, equals ve toString fonksiyonları üretilirler kullanılmazlar.
47. **Kotlin'de visibility modifierları anlatın?**
    * Kotlin'de 4 tip visibility modifier vardır. Bunlar: `private`, `public`, `protected` ve `internal`'dır.
    * `public` kelimesi ile sınıf, fonksiyon veya özellik dışardan ulaşıma açılır.
    * `private` önüne geldiği değişken veya fonksiyona sadece aynı sınıf içinden ulaşılmasına izin verir. Bunu o değişkenin arka planda get ve set fonksiyonlarını silerek gerçekleştirir.
    * `protected`Java'da aynı modül dışına kapatırken, Kotlin'de tanımlandığı sınıf ve child sınıfların o fonksiyona veya özelliğe erişebilmesini sağlar. Kotlin'de `protected`sınıf için kullanılamaz.
    * `internal` kelimesi ile bir sınıfı içinde bulunduğu modülün dışında ulaşılmasını engelleriz. Dışarıda `private`, modül içinde `public`gibi davranır.
48. **Kotlin'de accessibility modifierları anlatın?**
	Kotlin'de `open`ve `final`olmak üzere 2 çeşit accessibility modifier vardır. `open` anahtar kelimesi ile sınıf, fonksiyon veya özellik child sınıflar tarafından miras alınabilmesi sağlanırken, `final` ile child sınıflar tarafından miras alınabilmesi engellenir.
49. **Sınıfları neden override edemiyoruz?**
		Override ederken yaptığımız şey varlığın özelliğini değiştirmek/yeni bir davranış kazandırmak. Biz sınıf oluştururken ortak özellikleri taşıyan en küçük yapıyı oluşturmaya çalışıyoruz. Bu yüzden sınıf override etmek mantıklı olmazdı.
50. **Bir sınıf aynı fonksiyon ismine, aynı fonksiyon parametlerine ve aynı geri dönüş tipine sahip bir fonksiyonu içeren bir interface'i ve bir üst sınıfı implement eder ve miras alırsa, child class'ta üst sınıfın ve interface'in bahsi geçen fonksiyonuna nasıl ulaşabiliriz?**
		Burada `super`anahtar kelimesinin yanına bir arayüz kullanırız (`<>` yapısına arayüz diyoruz bu örnek kapsamında.) Örneğin:
```kotlin
class Square(): Rectangle(), Polygon {

	override fun draw() {
		super<Rectangle>.draw() // Rectangle'ın draw fonksiyonuna ulaşır
		super<Polygon>.draw() // Polygon'un draw fonksiyonuna ulaşır
	}
}
```

51. **Aşağıdaki kod parçacığının sonucu ne olur?**
```kotlin
class A(){
	var counter = 54
		set(value) {
			counter = value
		}
}
```

Bu kod parçacığı **StackOverflow hatası** verecektir. Her seferinde özyineli şekilde set fonksiyonu çağrılacaktır. Bunu düzeltmek için `field` değerine atama yapılmalıdır.
```kotlin
class A(){
	var counter = 54
		set(value) {
			field = value
		}
}
```

52. **Sealed class'lar ile enum class'ları karşılaştırınız.**
* Sealed class'lar ve enum class'lar ortak özellikleri üzerine gruplayabileceğimiz yapılar için kullanılırlar. İkisini de `when` yapısı içinde kullandığımızda exhaustive olduğu için tüm ihtimalleri ele almamızı ister ve geliştiricinin hatasını önler. Bu yapılar hiyerarşik yapıyı geliştiricinin elinden alıp IDE'nin kontrolüne vererek daha sıkı bir kontrol sağlar.
* Sealed class'lar bir sınıf hiyerarşisi kurarken, enum class'lar bir değer (value) hiyerarşisi kurar. Eğer sınıf özelliklerini kullanacaksak, nesne üretmemiz gerekirse sealed class kullanmamız gerekir. Eğer tek bir instance olsun ve daha basit bir yapı gerekli ise enum class kullanabiliriz.
* Enum class'lardaki enum sabitlerinin arka planda bulunduğu gibi, sealed class'ın child classları da **static final class** olarak bulunurlar. Ancak miras aldıkları sınıf türü farklıdır. Enum class sabitlerinin miras aldığı `enum` iken, sealed class'ın direct subclass'larının miras aldığı bir sınıf yapısıdır.
* Enum class'ların sabitleri hafızada sadece bir tane bulunur (nesnesi oluşturulamaz). Ancak sealed class'ların childları birden fazla nesnesi oluşturabilir.
* Sealed class içinde `object` kullanımı ve enum classlar'daki enum sabitleri arka planda aynı şekilde oluşturulur. İkisi de static class olarak oluşturulur.
* Sealed classlar'ın direct subclass'ların (bodysinde bulunan child classlar) primary constructor'ındaki parametre, sealed class'ın primary constructor'ındaki parametre ile aynı olmak zorunda değildir (enum classlar'ın aksine).
* Enum class'lar miras alınamazlar dolayısı ile `open` veya `abstract` olamazlar. Ancak sealed class'larda bu kısıtlama yoktur.
---------------------
### [[Generics]]

---------------------
 53. **Generic yapılarda invariance, co-variance ve contra-variance nedir?**
	* Invariance değişmezlik demektir.  Invariance, bir süper sınıfın generic yapıda belirtilen alt sınıf yerine kullanılamayacağını ifade eder. Bu yapıyı daha esnetmek istiyoruz ve bunun için de `in` ve `out` anahtar kelimelerini kullanıyoruz.
	* Co-variance Kotlin'de `out` anahtar kelimesi ile yapılır. Co-variance, generic yapıda bir sınıfı ve onun alt sınıflarını kabul eder.
	* Contra-variance Kotlin'de `in` anahtar kelimesi ile yapılır. Contra-variance , generic yapıda bir sınıfı ve onun üst sınıflarını kabul eder. Örneğin:
	     
```kotlin
interface Collection<E> {
	fun addAll(items: Collection<E>) // Invariance, E ne ise onu kabul eder. Ne üst ne de alt sınıfını kabul etmeyecektir.
}
interface Collection2<out E> {
	fun addAll(items: Collection<E>) // Co-variance, E sınıfı ve onun alt sınıflarını kabul eder.
}
interface Collection3<in E> {
	fun addAll(items: Collection<E>) // Contra-variance, E sınıfı ve onun üst sınıflarını kabul eder.
}
``` 
54. **Neden `Any` yerine generic kullanmalıyız?**
	    `Any` kullandığımızda bir kısıtlamaya gidemeyiz. Örneğin sadece `Number`'dan miras alan sınıfları kullanabilmek gibi. Ayrıca derleme zamanında kontrol edilemeyeceği için hatalara daha açık bir yapı oluşacaktır. Ancak generic kullandığımızda hem kısıtlamaları doğru ayarlayabiliriz, hem de derleme zamanı hatalar daha açık şekilde gösterilir.
55. **Star projection nedir?**
	    Kotlin'de generic yapılarda kullanılacak tipi tam olarak bilmesek bile daha güvenli bir şekilde tip belirtebilmek için star projection kullanılır. Generic tip belirtilecek yere `*` işareti konulur. Bu işaret okuma (read) veya return değeri için `<out Any?>` ve yazma (write) veya input için `<in Nothing>` şeklinde çevrilir. Dönülecek sınıf Kotlin'deki en üst sınıf olan`Any` sınıfının bir alt sınıfı olmalı ve girdi olarak verilecek sınıf Kotlin'deki en alt sınıf olan `Nothing` sınıfının bir üst sınıfı olması istenir. 
 56. **`reified` neden `inline` ile kullanılıyor?**
	    Kotlin'de generic yapılar çalışma zamanında tipleri tutmuyorlar (type erasure). `inline` ile kodun çağrıldığı yer de fonksiyonun içine yapıştırıldığı için, `reified` anahtar kelimesi ile birlikte kullanıldığında bu sınıflara erişebiliyoruz.
57. **`inline` anahtar kelimesi Kotlin'de sadece higher-order fonksiyonlar ile mi kullanılıyor? Değil ise başka nerede kullanılır?**
	    `inline` anahtar kelimesi higher-order fonksiyonların dışında `reified` anahtar kelimesi kullanılan fonksiyonlarda da kullanılır.
	    
 58. **Kotlin'de generic bir tipi non-nullable olarak belirlemek için nasıl bir yol izlenmelidir?**
	    Kotlin'de generic bir tipi non-nullable yapabilmek için `& Any` yapısı kullanılır. Örneğin:
```kotlin
interface ArcadeGame<T1>: Game<T1> {
	override fun save(x: T1): T1

	// T1 non-nullable'dır.
	override fun load(x: T1 & Any): T1 & Any
}
```    
 59. **Kotlin'de tür silmesi (type erasure) ne demektir?**
		Kotlin'de generic yapılar için tip kontrolleri derleme zamanı (compile-time) yapılır. Compile-time zamanı bilinen tipler, çalışma zamanı (run-time) bilinmez. Kotlin'de tür silmesi, derleme sırasında generic tür bilgilerinin kaldırılmasıdır. Örneğin `Foo<Bar>` veya `Foo<Bar?>` yapıları `Foo<*>`'a çevrilir. Bu, çalışma zamanında tür bilgisinin eksik olmasına ve bazı durumlarda tür güvenliği ve dinamik tür kontrolüyle ilgili zorluklara neden olabilir. Bu nedenle, Kotlin'de `reified` tür parametreleri gibi çözümler kullanılarak bu sınırlamaların üstesinden gelinmeye çalışılır.
60. **Aşağıdaki kodda B parametresinin sadece Number'ın alt sınıflarından birini alabilmesi için nasıl kısıtlarız?**
```kotlin
data class GPair<A, B>( val first: A, val second: B ) : Serializable
```
Üstteki kodda B tipini `out` kelimesi ile birlikte Number olarak kısıtlarsak, artık sadece Number'ın alt sınıflarını alacaktır.
```kotlin
data class GPair<A, out B: Number>( val first: A, val second: B ) : Serializable
```
---------------------
### [[Objects]]

---------------------
61. **Singleton bir anti-pattern olarak da adlandırılır. Neden?**
	Gereğinden fazla singleton oluşturmak hafızanın gereksiz dolmasına sebep olacaktır. Ayrıca hız kazanmak için backend'e gitmeden client-side'da singleton tutup onu güncelliyorsak, senkronizasyonu iyi yaptığımızdan emin olmalıyız.
62. **Kotlin'de Object'in kaç farklı kullanımı vardır?**
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
	* `companion object` kullanımında ise bir sınıfa eşlik eden bir static yapı oluşturmuş oluruz. Bu yapının lifecycle'ı `object`'in aksine sadece içinde tanımlandığı sınıf kadardır.
63. **`object` ile `companion object` arasındaki farklar nelerdir?**
	* `object` arka planda kendisi `static`değildir ancak bodysinde bulunan instance'ı `static`'tir. Dolayısı ile içindeki instance'a erişip, oradan içindeki öğelere erişiriz. 
	* `companion object` bir sınıfın içinde bulunduğu için yani nested'lık özelliğinden dolayı `static`'tir ve kendi bloğu içinde kendi nesnesi bulunmaz. Kendi nesnesi bağlı olduğu sınıfın içinde yine `static final` olarak bulunur.
	* Bir `companion object` ile sınıfımıza eşlik eden bir singleton oluşturmuş oluruz. `companion object`'in lifecycle'ı içinde bulunduğu sınıf kadardır. Bir `object` ile genel bir singleton oluştururuz ve bu singleton'u tüm sınıflarda kullanabiliriz. `object`'in lifecycle'ını biz yönetiriz.
64. **Bir `companion object` içindeki `static` olmayan bir fonksiyona nasıl içinde bulunduğu sınıfın nesnesini oluşturmadan (constructor olmadan) erişebiliyoruz?**
	Arka planda `static` bir `Companion` objesi oluşturulur. Bu yüzden fonksiyonun body'sine static bir fonksiyon yazmasak bile bu obje üzerinden erişebiliriz. Yani biz `HomeFragment.newInstance()` yazsak da, aslında IDE arka planda bunu şöyle görür: `HomeFragment.Companion.newInstance()`. Ancak eğer projede Java sınıfları da varsa, bu durumda `Companion`'ı da yazmalıyız o fonksiyonu çağırırken.

---------------------
### [[Reflection]]

---------------------
65. **Reflection nedir?**
	Üyeleri veya constructor'ı `private` olan bir sınıfın `private` üyelerine erişebilmek için kullanabileceğimiz bir yöntemdir reflection.