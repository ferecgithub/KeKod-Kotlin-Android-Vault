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
* Mutlaka primary constructor'ı olan ve bu constructor'da en az bir property (val/var ile yazılmış) parametre içermesi gereken sınıflardır.
