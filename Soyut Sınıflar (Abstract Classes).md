  * Abstract sınıflar ile detay vermiyoruz ancak sınırlarını net olarak belirliyoruz. Örneğin insan sınıfını abstract class olarak tanımlayabiliriz ancak Adanalı sınıfını open class olarak belirleriz. Çünkü Adanalı'dan Sivaslı oluşturamayız. İkisinin de kendine özgü implementasyonu var.
  * Nesnesini oluşturacağımız bir sınıf open olmalıdır ancak nesnesini kesin olarak oluşturmayacağımız bir sınıfı abstract yapabiliriz. En az bir tane abstract methodu olması gerekli.
  * Abstract class içinde solid bir sınıfın instance'ının olmaması gerekli (genellikle). Bu yüzden o nesne dependency injection ile parametreden vermemiz gerekli. Abstract classların birden fazla constructorunun olabilmesinin temel sebeplerinden biri budur.
  * Abstract classlar, başka abstract classları miras alabiliyor. Bu durumda üst sınıftaki abstract property ve abstract fonksiyonların implemente edilmesi zorunluluğu olmaz alt abstract sınıf için. 
  * Open sınıflarda abstract yapılar bulunduramayız. Abstract keywordunu sadece abstract sınıflar içinde kullanabiliriz.
  * Bir class abstract ise bir interface'i implement ettikten sonra o interface'in fonksiyonlarını override etme zorunluluğu kalkar.

### Geri dön: [[Buradan Başla - Indeks]]