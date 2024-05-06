* Constructor'u olmadığı için nesnesi oluşturulamıyor. 
* Ancak `object` ile expression kullanımı şekilde yazdığımızda, arka planda interface'in bir class gibi davranıyor. Bir nesnesi oluşturuluyor. 
* Eğer bir interface'in fonksiyonuna bir body verirsek o fonksiyon opsiyonel hale gelir. Override edilmesi zorunlu olmaz. Çünkü arka planda statik bir sınıf içinde statik bir fonksiyonu oluşturulur. 
* Bir interface, başka bir interface'ten miras alabilir. SOLID'deki Interface Segregation'ı böyle sağlayabiliriz. Interface'ler şişmesin, ihtiyaç olduğu kadar bölmeliyiz.
* API'ları implemente edeceğimiz zaman, ilk önce bir interface'e implement edip sonrasında 
* Bir class birden fazla interface'i implement edebilir ancak birden fazla class'ı implement edemez. Bir interface başka bir class'ı implement edemez.
* Abstract sınıf mı interface mi kullanmalıyız konusunda, komple engellemek için abstract class kullanabiliriz çünkü abstract classlarda `final`kullanıp yeniden implement edilmesini engelleyebiliriz. Ancak interfacelerde komple engelleme yapılamıyor.
* Interface'ler state tutmazlar. Backing field oluşturulmaz içerideki değişkenler için. Ancak companion object içinde state tutabiliriz yani backing field olan değişken oluşturabiliriz. Yapısı gereği statik değişken oluşuyor ancak bu yöntemi kullanmamalıyız (edge-caselerde kullanılabilir). 

### Geri dön: [[Buradan Başla - Indeks]]