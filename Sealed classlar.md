* Sınıf hiyerarşisini geliştiricinin keyfinden alıp, IDE'nin eline verdiğimiz kısıtlanmış hiyerarşi oluşturabileceğimiz sınıflardır.  Child classlar derleme aşamasında belli olur. `sealed class`'lar miras verilemeyeceği için başka bir geliştiricinin bizim yaptığımız `sealed class`'ı miras alarak yeni türler oluşturmasını önlemiş oluruz.
* Boş bir sealed class oluşturursak, herhangi bir sınıfa miras olarak verebiliriz. Bu durumda kapalı kutu durumu ortadan kalkar.
* Visibility modifier'lar kullanılabilir.
* `open` veya `final` yapılamazlar.
* Sealed classlar diğer sınıfları miras alabilir.
* Primary constructoru varsayılan `protected`'dır. Sadece sınıf scope'u içinde bir hiyerarşi kurulursa constructor `private` yapılabilir ancak `protected`olması, dışarıda da bu sealed class'ı miras alan bir sınıf olabilmesini sağlar. Kütüphanelerde constructor `private`yapılır ki kütüphanedeki sealed class içinde belirlenen türlerden başka türler türetilemesin.
* Enum class'lardaki enum sabitlerinin arka planda bulunduğu gibi, sealed class'ın child classları da **static final class** olarak bulunurlar.
* Enum class'ların sabitleri hafızada sadece bir tane bulunur (nesnesi oluşturulamaz). Ancak sealed class'ların childları birden fazla nesnesi oluşturabilir.
* Sealed class içinde `object` kullanımı ve enum classlar'daki enum sabitleri arka planda aynı şekilde oluşturulur. İkisi de static class olarak oluşturulur.
* Sealed class'lar bize modül seviyesinde bir kısıtlama getirir. Örneğin BaseFragment tiplerini gruplayacağımız bir sealed class'ı core modülüne koyup, o şekilde paylaştırabiliriz.
* Sealed classlar'ın direct subclass'ların (bodysinde bulunan child classlar) primary constructor'ındaki parametre, sealed class'ın primary constructor'ındaki parametre ile aynı olmak zorunda değildir (enum classlar'ın aksine).
* Ortak olarak gruplayabildiğimiz yapılarda, tüm sabitler sadece ortak özelliklere sahip olsun dersek enum class ancak her sabit kendi içlerinde farklı özelliklere de sahip olabilsin (örneğin interface implemente ederek) dersek sealed class kullanabiliriz.
* Direct class'lar (sealed class'ın bodysi içindeki direkt child classları), `open`, `abstract`, `data` anahtar kelimelerini alabilirler. 
* Sealed classlar'ın direct child'ları derlenme zamanında (compile-time) belirlenir. Bu yüzden `when` ile kullanımında normal inheritance'dan farklanır.
* **Sealed Interface**'ler de aynı mantığın interfaceler için uygulanmış hâlidir. Interface'leri gruplamak için kullanılır.

### Geri dön: [[Buradan Başla - Indeks]]