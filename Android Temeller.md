* `gradle.properties` dosyasına lokal bir dosya yolu koymamalıyız. Bu tür verileri `local.properties`'de tutmalıyız.
* Proje seviyesindeki `build.gradle` içinde dependency'lerin hangi repodan indirileceği ve dependencyResolutionManagement bloğu içinde dependency'ler ile ilgili çeşitli kısıtlamalara gidilebilir.
* Versiyon kataloğunda bir kütüphanenin versiyonu düşükse ancak başka bir kütüphaneden daha güncel versiyonu geliyorsa bu kütüphanenin, Android otomatik olarak daha güncel versiyonu kullancaktır.
* Kodlar üzerinde yaptığımız değişikliklerin tarihi `Local History`'de tutulur. 