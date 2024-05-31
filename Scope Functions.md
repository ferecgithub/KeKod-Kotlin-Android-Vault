* Scope fonksiyonlar temelde 5 tanedir. Bunlar:
	* let
	* apply
	* run
	* also
	* with
* scope fonksiyonlarla null checkleri daha kolay yapabiliriz, ekstra fonksiyon yazmadan nesnelere uygulayacağımız mantığı bir yerde toplayabiliriz.
* ***Hatırlatma:*** Higher-order fonksiyonların bloklarındaki son satır o higher-fonksiyonun return değeridir.

![[Screenshot 2024-05-28 at 10.37.50.png]]

#### let
* Önüne gelen değişken = T
* Son satır = R
* Lambra parametresi = T
* `let` fonksiyonunun arka planda imzası şöyledir: 
```kotlin
public inline fun <T, R> T.let(block: (T) -> R): R
```
Bu durumda T'yi extend eden let extension fonksiyonu yine T'yi parametre olarak alacak (bloktaki **it** tipi T olacak) ve tüm fonksiyon R tipini dönecek.
```kotlin
val returnValueLet = name?.let { name: String ->
	println("let $name") // son satır Unit olduğu için returnValueLet Unit'tir.
}
```
#### run 
##### 1. kullanım
* Önüne gelen değişken = T
* Son satır = R
* Lambra parametresi = yok, this = T
* 1. kullanım için `run` fonksiyonunun arka planda imzası şöyledir: 
```kotlin
public inline fun <T, R> T.run(block: T.() -> R): R
```
`run` parametre olarak bir extension function aldığı için artık parametre yerine `this` kelimesi ile erişiyoruz T'ye. 
```kotlin
val returnValueRun = name?.run {
	println("run $this") // son satır Unit olduğu için returnValueRun Unit'tir.
}
```
##### 2. kullanım
* T = yok, extension işlemi yok.
* Son satır = R
* Lambra parametresi = yok, this = yok çünkü extension yok.
* 2. kullanım için `run` fonksiyonunun arka planda imzası şöyledir: 
```kotlin
public inline fun <R> run(block: () -> R): R
```
Burada son satırda yazılan değer `returnValueRun`'ın değeri olacak.
```kotlin
val returnValueRun = run {
	println("run $this") // son satır Unit olduğu için returnValueRun Unit'tir.
}
```
#### with
* 1. parametre (fonksiyon olmayan değişken) = T, 
* Son satır = R
* Lambra parametresi = yok, this = T
* `let` fonksiyonunun arka planda imzası şöyledir: 
```kotlin
public inline fun <T, R> with(receiver: T, block: T.() -> R): R
```
* `with` ile `repeat` benzeşirler yapı bakımından. Sadece repeat'in ikinci parametresi bir extension function değildir, Int bir parametre alır.
```kotlin
val returnValueWith = name?.with {
	println("with $this") // son satır Unit olduğu için returnValueRun Unit'tir.
}
```
#### apply
* Önüne gelen değişken = T
* R = yok (tüm fonksiyonun dönüş değeri T)
* Lambra parametresi = yok, this = T
* `let` fonksiyonunun arka planda imzası şöyledir: 
```kotlin
public inline fun <T> T.apply(block: T.() -> Unit): T
```
* Örneğin:
```kotlin
val returnValueApply = name?.apply {
	println("apply $this") // son satır Unit olduğu için returnValueRun Unit'tir.
}
```
#### also
* Önüne gelen değişken = T
* R = yok (tüm fonksiyonun dönüş değeri T)
* Lambra parametresi = T, this yok çünkü extension yok.
* `let` fonksiyonunun arka planda imzası şöyledir: 
```kotlin
public inline fun <T> T.also(block: (T) -> Unit): T
```
* Örneğin:
```kotlin
val returnValueAlso = name?.also { name: String ->
	println("also $name") // son satır Unit olduğu için returnValueRun Unit'tir.
}
```
* **Bu fonksiyonun geri dönüş değeri olarak lambda'nın son satırını mı almak istiyorum yoksa ana değişkenin kendisini mi almak istiyorum?** (Eğer lambda'nın son satırını alacaksak apply ve also'yu kullanamayız.) **Değişkenin değerini değiştirebilecek bir işlem yapacaksak blok içinde apply veya also kullanmak daha iyi olabilir çünkü değişkenin kendisini geri almamız gerekiyor.** Değişkenin değerini değiştirmeyecek birşey yazıyorsak bloğa o zaman let, run veya with kullanabiliriz.
* Düz bir class'tan bir nesne oluştururken genelde `apply` scope fonksiyonunu kullanırız.
* Bir data class'tan bir nesne oluşturduktan sonra `also` fonksiyonunu kullanabiliriz. Aslında `apply`da kullanılabilir ancak anlamsal olarak `also` daha çok uyar.
* Örneğin bize bir nesneyi parametre olarak alan bir fonksiyon verilsin. Anlamsal olarak nesneyi oluştururken kullandığımız `apply` ve `also` scope fonksiyonlarını burada kullanmak pek anlamlı değildir. Bu yüzden diğer scope fonksiyonlarını ihtiyacımıza göre tercih edebiliriz.
```kotlin
fun someUserFun(user: User?) {
// apply ve also'yu nesne oluşturulurken kullanmak daha mantıklıdır. Burada hazır bir nesne paslandığı için tercih etmiyoruz. Çünkü bu nesneye tekrar ihtiyaç duymuyoruz geri dönüş değeri olarak.
	user.also {} 
	user.apply {}

// user nullable olduğu için with scope fonksiyonu kullanmamalıyız. nullable olmasaydı o zaman with kullanırdık.
	with(user) {}
// nullable değerler için let veya run scope fonksiyonu kullanabiliriz.
	user?.let {}
	user?.run {}
}
```
* `with`, `let` ve `run` fonksiyonları lambda'nın son satırını dönüş değeri olarak aldıkları için null dönme durumunda `?:` operatöründen sonraki `run` bloğu çalışır. Dolayısı ile eğer nesnenin kendisinin null kontrolünü yapmak istiyorsak `also` fonksiyonunu kullanmalıyız. Örneğin:
```kotlin
fun someUserFun(user: User?) {
// Aşağıdaki durumda let fonksiyonu son satırı kayde aldığı için run bloğu da çalışır. foo'nun değeri 4 olur.
	val foo = user?.let {
		5
		null
	} ?: run {
		4
	}

// Eğer user'ın null olup olmamasına göre blokların çalışmasını istersek also kullanabiliriz. Aşağıda boo'nun değeri User nesnesi olur.
	val boo = user?.also {
		5
		null
	} ?: run {
		4
	}
}
```
* `let` scope fonksiyonu lambdanın son satırını değer olarak döndüreceği için elvis (`?:`) operatörü sayesinde `run` bloğu çalıştırılacaktır. Eğer burada amacımız user nesnesinin null kontrolü üzerinde blokları çalıştırmak ise `let` yerine `also` kullanabiliriz. Bu sayede lambdanın son değeri null bile olsa geri dönüş değeri nesnenin kendisi olacaktır.
* Eğer değer atama yapacaksak `also` veya `apply` kullanamayız. Örneğin üssteki `boo` değeri eğer user null değilse user, eğer null ise 4 alacaktır. Yani User veya Int değeri alabilir (IDE tipi Any olarak belirler). Bu da sağlıklı bir atama değildir.

#### Özetle
1. Eğer nesne oluşturacaksak ve onu bir değişkene atayacaksak `apply` veya `also` kullanmalıyız.
	1. Eğer bir data class ise `also` kullanırız çünkü zaten constructor'da gereken parametleri verip üretmiş oluruz. Hemde mânâsına gelen `also` burada anlamsal olarak daha çok uyuyor.
	2. Eğer normal bir class ise `apply` kullanırız çünkü gereken veriler bloğun içinde verilecek.
2. Eğer nesne hazır ise ve **onu bir değişkene atayacaksak** o hâlde `let`, `run` veya `with` scope fonksiyonlarından birini kullanabiliriz.
	1. Eğer nesne nullable bir nesne ise o hâlde `let` veya `run` kullanmalıyız çünkü `with` ile yeniden bir null kontrolü yapmak gerekecektir.
	2. Eğer nesne nullable bir nesne değilse o zaman `with` ile kullanabiliriz.
3. Eğer `let` ile `run` scope fonksiyonlarını `?:` operatörü ile birlikte kullanıyorsak ve bir değişkene atama yapacaksak, son satırı ile birlikte elvis operatörü kontrol edilir. Son satırda null bir değer varsa `run` bloğu da `let` bloğundan sonra çalışacaktır (nesne null olmasa bile). 
4. Eğer nesne hazır ise ve **onu bir değişkene atama yapmayacaksak**, yani expression kullanımı yapmayacaksak `let` ve `run` yerine `also` ve `run` bloklarını birlikte kullanmak daha mantıklıdır. Çünkü `also` ve `run` birlikte kullanıldığında eğer bir değişkene atama yaparsak, atadığımız değişkenin tipi `Any` olacaktır. Eğer ana değişken null değilse tip ana değişkenin tipi olacaktır, eğer null ise `run` bloğunun son satırının tipi olacaktır.
5. `run`'ın extension olmayan halini ise kodu gruplara ayırmak ancak fonksiyonlara ayırmak istemediğimiz zaman kullanabiliriz.

![[kotlin_scope_functions.gif]]
Resim kaynağı: https://www.linkedin.com/feed/update/urn:li:activity:7200835881341972480/