
```js
setInterval(function, delay);
```
Bu kullanım function adında bir fonksiyon tanımlanmış yani ilk paremetre fonksiyon 
ikinci paremetre ise kaç saniyede bir tekrarlanağı değer.


```js
setInterval(() => {
  console.log("Merhaba dünya");
}, 1000); // Her 1 saniyede bir çalışır
```

bu da yukardaki kodun önceden fonksyion tanımlanmadan set Interval fonksiyonunun içinde oluşturulmuş fonksiyon örneğidir.
