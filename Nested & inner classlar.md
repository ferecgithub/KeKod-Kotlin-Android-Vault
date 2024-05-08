* İçiçe sınıfların (nested classes) ve `inner` classların temel farkı, içiçe sınıflarda içteki sınıf `public static final` olarak belirlenir. `inner`class'larda ise `public final` olarak belirlenir.
* Nested class'larda dışarıdaki bir üyeye erişilemezken, `inner` class'larda dışarıdaki üyelere erişilebilir. Bunun sebebi `inner`class'lara erişebilmek için dış sınıfın nesnesinin üretilmesi gerekmesidir. Nesne oluştuğu için dış sınıfın (outer class) herhangi bir üye değişkenine erişebiliriz.
* `inner` class'ın ömrü en az outer class kadar olursa `inner` class kullanmak mantıklıdır. Yoksa `inner` class kullanacağız diye boşuna outer class'ı da hafızada tutmuş oluruz.

```kotlin
class RecyclerView {
	val outerName: String = "RecyclerView"

	class ViewHolder {
		val nestedName: String = "ViewHolder"

		fun getOuterName(): String = outerName // Hata verir, erişemez.
	}
}

class RecyclerView2 {
	val outerName: String = "RecyclerView2"

	inner class ViewHolder {
		val nestedName: String = "ViewHolder2"

		fun getOuterName(): String = outerName // Erişime izin verilir.
	}
}

fun main() {
	val recyclerView = RecyclerView()
	val viewHolder = RecyclerView.ViewHolder() // nested class yapısında static final class oluştuğu için ViewHolder sınıfına ulaşırken nesne üretmeye ihtiyaç duymaz.

	val recyclerView2 = RecyclerView2()
	val viewHolder2 = RecyclerView2().ViewHolder() // inner class yapısında outer class'ın nesnesini üretmemiz gerekir ViewHolder'a ulaşabilmek için.
}

```
