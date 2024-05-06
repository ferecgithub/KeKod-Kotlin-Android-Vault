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

### [[Kotlin'de obje yönelimli programlama (object oriented programming)]]
### [[Soyut Sınıflar (Abstract Classes)]]

### [[Interfaceler]]

### [[Data classlar]]

### [[Enum classlar]]

### [[Sealed classlar]]


### Geri dön: [[Buradan Başla - Indeks]]
