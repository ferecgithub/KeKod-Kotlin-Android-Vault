* Bir değeri gözlemleyip herhangi bir değişim konusunda bilgi veren akışa **observer pattern** denir.
* Önceden bunun için interfaceler kullanılırdı. Burada bilgilendirme fonksiyonları kullanılarak istenilen işlemlerde, istenilen birimlere haber verilirdi.
* Observable pattern'da ister interface ister higher-order fonksiyonları kullanarak geriye dönük bilgi akışı sağlayabiliriz.
* Bunu Android'de Activity/Fragment ve ViewModel arasında kullanılır genelde.
* Kotlin içinde tanımlı olan ve işimizi daha da kolaylaştıran `Delegates.observable(VARSAYILAN_DEGER)` sayesinde observer patterni çok daha az kodla implement edebiliriz. Arka planda yine bir interface kullanıyor ve `getValue` ve `setValue` operatorlerini override ediyor.
* `Delegates.observable(VARSAYILAN_DEGER)` yanında bir de `Delegates.vetoable(VARSAYILAN_DEGER)` var. Burada değerin bir limiti/aralığı varsa onu belirtebiliyoruz ve ona göre atama yapmasını kontrol edebiliyoruz. Son satıra atama yapabilme koşulunu yazıyoruz. Eğer şart sağlanmazsa blok çalışmış olur ancak değişkene atama yapmaz. Örneğin:
```kotlin
var name: String by Delegates.observable("Ferec") { property, oldValue, newValue ->  
    println("$oldValue -> $newValue")  
}  
  
var age: Int by Delegates.vetoable(0) {  property, oldValue, newValue ->  
    println("$oldValue -> $newValue")  
    newValue >= 18  
}
```
