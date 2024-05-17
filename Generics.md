* Genericler, tipini bilmediğimiz ancak yine de bir hiyerarşi oluşturmak istediğimiz bir değişken verme yöntemidir. İlgili tür, arayüz (interface) dediğimiz `<>` karakterleri arasına belirtilir.
* Genericler, sınıfları ve fonksiyonları daha genel ve yeniden kullanılabilir hale getirir. Bir sınıfı başka bir sınıftan miras almaktansa, generics kullanarak gereken davranışları ve özellikleri parametrize edebilirsiniz, bu da kodun daha az bağımlı ve daha kolayca değiştirilebilir olmasını sağlar.
* Genericlere tip parametreleri (type parameters) de denir.
* Java ve Kotlin'de Genericler değişmezdirler (**invariant**). Yani `List<String>` beklenen bir yere `List<Object>` veremeyiz. Yani subtype verip supertype kullanamayız. Eğer List'ler invariant olmasalardı, Java'daki array'lerden farkları kalmazdı.
* Java'da wildcard denilen yapı vardır. Ancak Kotlin'de wildcard yoktur. Java'deki wildcard'a örnek olarak:
```java
interface Collection<E> {
	void addAll(Collection<E> items); // Invariance, E ne ise onu kabul eder. Ne üst ne de alt sınıfını kabul etmeyecektir.
}
interface Collection2<E> {
	void addAll(Collection2<? extends E> items); // Co-variance, E sınıfı ve onun alt sınıflarını kabul eder.
}
interface Collection3<E> {
	void addAll(Collection3<? super E> items); // Contra-variance, E sınıfı ve onun üst sınıflarını kabul eder.
}
```
* Üstteki örnekte `?` simgesi bir wildcard'dır. `? extends E` ifadesi, E sınıfının kendisi veya bir alt sınıfını ifade eder. Burada wildcard'ların extends-bound (upper bound) kullanımı, tipi "**co-variant**" yapıyor. Sınıfın kendisini veya alt sınıflarını kullanan yapılara "**co-variance**" deniyor.
* Java'da eğer `? super E` gibi bir wildcard kullanımı görürsek, yani sınıfın kendisini veya üst sınıflarını kullanan yapılara "**contra-variance**" deniyor.
* Genericlerin yerine `Any` kullanırsak, bize tip güvenliği (type-safety) sağlamazdı. Bu durumda çeşitli sorunlarla (örneğin operatör sorunları) karşılaşabilirdik.
* Genericler ile tipler konusunda oluşabilecek bir hatayı çalışma zamanı (run-time) yerine derleme zamanında (compile-time) alırız. Bu şekilde oluşabilecek hatalardan daha erken haberdar oluruz.
* Kotlin'de wildcard desteklenmez. Onun yerine decleration-site variance dediğimiz, generic tipin belirtildiği yere gelen `in` veya `out` anahtar kelimeleri ile Java'daki co-variance ve contra-variance durumları sağlanır.
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
* `Any` her sınıfın en üst sınıfıdır. `Nothing` ise her sınıfın en alt sınıfıdır. Dolayısı ile type projection yaptığımızda (`in` veya `out` kullanımında) eğer en alt sınıfı kastedip onun üst sınıflarının yazılabilmesini istersek `<in Nothing>` diyebiliriz. Eğer en üst sınıfı kastedip onun alt sınıflarının yazılabilmesini istersek  `<out Any>` diyebiliriz.
#### Star projection
* **Star projection** ile generic tip konusunda bilgimiz olmasa bile, güvenli bir şekilde generic tip atayabilmemizi sağlar. Bunun için generic tip belirlediğimiz yere `*` işareti koyarız. Okuma (read) için `<out Any?>` ve yazma (write) için `<in Nothing>` şeklinde çevrilir.
* `Function <*, String>` yapısında ilk yapı girdi (input) ikinci yapı çıktıyı (output) ifade eder. Kotlin'de bu yapı şu şekilde çevrilir: `Function <in Nothing, String>`. Yani girdiye verilecek sınıf `Nothing`'in üst sınıfı olmalıdır ve her sınıf zaten `Nothing`'in üst sınıfıdır.
* `Function <Int, *>` yapısı Kotlin'de bu yapı şu şekilde çevrilir: `Function <Int, out Any?>`. Yani döndürülecek sınıf `Any`'nin alt sınıfı olmalıdır ve her sınıf zaten `Any`'nin alt sınıfıdır.
* `Function <*, *>` yapısı Kotlin'de bu yapı şu şekilde çevrilir: `Function <in Nothing, out Any?>`. 