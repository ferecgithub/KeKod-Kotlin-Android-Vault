#### Encapsulation (Kapsüllemek)
* Encapsulation kavramı, bir sınıf içindeki üyeleri `private`olarak tutmamızı ve gerektiği takdirde ve gerektiği kadarını dışarıya açmamızı önerir. Çünkü sınıflar birbirlerinden haberdar olmazsa, katmanlı bir yapı oluşturabiliriz.
* Encapsulation'ı uygularken, her bir sınıfı, fonksiyonu ve özelliği kimin kullanacağını bilerek visibility modifier atamalıyız. Örneğin o sınıf sadece child sınıflar tarafından kullanılacaksa `protected`, eğer sadece o modül içinde kullanılacaksa `internal` kullanabiliriz.
* Kotlin'de bir değişkene `private`verirsek, aslında o değişkenin arka planda setter ve getter fonksiyonları oluşturulmaz. Dolayısı ile Kotlin'de değişkenler arka planda property olarak tutulduğu için onlara erişme veya değiştirme yapılamaz. Backing field'ı her zaman `private`'tır.
* Fonksiyon içinde lokal **olmayan** bir değişkene atama işlemi yapmadığımız sürece arka planda setter fonksiyonu oluşturulmaz.

#### Inheritance (Miras almak)
* Bir varlığın temel özelliklerini ve işlevlerini belirlediğimiz işleme miras alma diyoruz. Örneğin bir rol yapma oyunu oynadığımızı düşünelim. Burada bize "5 tane ayı postu getir." görevi verilmiş olsun. Oyunda boz ayı, kutup ayısı ve siyah ayı türlerinde ayılar var ve bunların hepsi aslında ayı sınıfından miras alan varlıklar. Ancak her birinin kendi adaptasyonları çerçevesinde implementasyonları değişiklik gösterir. Kutup ayısının postu beyaz ve yediği yiyecek fok balığı iken, boz ayının postu kahve rengi ve yediği besin nehirde avladığı somon balığıdır. Verilen görev "ayı postu" getirmek olduğu için tüm bu türlerin postu kabul olacaktır.
* Java'da `extends` ile miras alma işlemi yapılırken, Kotlin'de `:` ile aynı işlem yapılır.
* Kotlin'de Java'nın aksine sınıflar varsayılan olarak `final`'dırlar (property ve fonksiyonlar da keza öyle. Bu yüzden override'ı önler). Ancak `open` anahtar kelimesi ile sınıf, fonksiyon veya özellik miras alınabilir ve `final` ile miras alınamaz hâle getirebiliriz.
* `open`ve `final` anahtar kelimeleri Kotlin için accessibility modifier (erişim düzenleyicisi)'dır. Bu yapılar ile child sınıflarda herhangi bir sınıf, fonksiyon veya özellik miras alınabilir veya miras alınması engellenebilir.
* `final` anahtar kelimesi backing field'ı olmayan top level property veya delagete'lerde kullanılamaz.

#### Polymorphism (Çok şekillilik)
* Polymorphism override ve overload ile ilgilidir. 2 tip polymorphism vardır. **Statik** ve **dinamik** polymorphism. Dinamik polymorphism override, statik polymorphism overload ile ilgilidir. Statik denmesinin sebebi bilginin belli olmasıdır. Dinamik olmasının sebebi de bilginin belli olmamasıdır. Üst classta statik polymorphism, alt classlarda override ederek dinamik polymorphism yaparız.
  ChatGPT açıklaması:
  Polimorfizm ayrıca dinamik (runtime) ve statik (compile-time) olmak üzere iki ana kategoride de olabilir:

1. **Statik Polimorfizm (Static Polymorphism)**: Bu tür polimorfizm, derleme zamanında belirlenir ve aşırı yükleme (overloading) ile ilişkilidir. Fonksiyonlar arasındaki doğru sürümün, fonksiyon çağrısının derleme zamanındaki bağlamına göre belirlendiği durumlarda ortaya çıkar.
    
2. **Dinamik Polimorfizm (Dynamic Polymorphism)**: Bu tür polimorfizm, çalışma zamanında belirlenir ve aşırı yazma (overriding) ile ilişkilidir. Alt sınıflardaki bir yöntemin, üst sınıflardaki aynı isimli yöntemi geçersiz kılması ve çağrıldığında alt sınıftaki sürümünün çalıştırılması durumunda ortaya çıkar.

#### Abstraction (Soyutlamak)
* Bir varlığın detay vermeden, genel özellikleri üzerinden sınırlarının belirlenmesidir. Bunu [[Soyut Sınıflar (Abstract Classes)]] veya [[Interfaceler]] ile gerçekleştirebiliriz.

### Geri dön: [[Buradan Başla - Indeks]]