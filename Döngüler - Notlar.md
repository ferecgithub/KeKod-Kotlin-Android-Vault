* `break` anahtar kelimesi döngüyü bitirir. `continue` anahtar kelimesi ise döngü adımını atlar. 
* İç içe döngülerde eğer üstteki döngüyü bitirme veya adım atlama yapmak istersen label koymalıyız. Örneğin:
  
```kotlin
returnLabel@ for (value in 1..50) {
	for (value2 in 0..10) {
		if (value2 == 5) {
			break@returnLabel // bu şekilde üstteki label verdiğimiz dış döngü bitirilecek.
		}
	}
}

returnLabel@ for (value in 1..50) {
	for (value2 in 0..10) {
		if (value2 == 5) {
			continue@returnLabel // bu şekilde üstteki label verdiğimiz dış döngünün geçerli adımı geçilecek.
		}
	}
}
```

* `break` ve `continue` anahtar kelimeleri döngü ile ilgilidir. Ancak `return` direkt fonksiyondan çık komutunu verir.