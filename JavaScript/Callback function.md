
Callback Function bir fonksiyon içinde bir fonksiyon olmasıdır. asenkron işlemlerde kullanılır.


```js
const getTodos=(callback)=>{
    const request=new XMLHttpRequest();

request.open('GET','https://jsonplaceholder.typicode.com/todos');

request.onload=()=>{
    if(request.readyState===4 && request.status==200){
        console.log(request.responseText);
        callback();
    }
}
/* Buradaki onload fonksiyonu istek bittiğinde başarlı çalışırsa kendi kendiligine tetiklenecek
içindeki readState ise şaun requestin hangi aşamada olduğunu beliretiyor 4 demek en son aşama oldugunda ekrana bas 2 filan yazamadık
mantıkken çünkü 2 de istek atıyor filan derken 4 verilerin çekildgi aşama 
responseText ile de ekrana bastırdık 

request.status==200 ise statuc code bildiğimiz net core'dan eğer istek attığımzıda 200 döenrse her şey yolunda demesi */

request.send();
}

const callback=()=>{
    console.log('dandjnadjnajdnjadnjandjadnjandjadnj')
}

getTodos(callback);
```

Mantıkk şu burada ilk dıştaki fonksiyon çalışır ve içteki fonksiyon sırası geldiginde paremetreler nasış tetikleniyorsa bu da öyle olur aslında mantık burada fonksyion içinde fonksiyona paremetre olarak fonksyion verilmesi c# da bunu yapmaya kalkamassın bile.

Hazır callback function metotlar örnekleri :

setTimeout,setİnterval mesela içine arrow function yazıyoruz yani git ister içine yaz fonksiyonun arrow functtion olarak yada git arrow function boş biryerde yaz paremetre olarak ver js de yöntem bitmez.


 ```js
 fetch('Example/item.json')

.then(response=>response.json())

.then(data=>{
    console.log(data);
})
.catch((err)=>{
    console.log(err);
})
```


Şimdi kafa karışıklığını ortadan kaldıralım önce neden thenden sorna { } diye düşündüm aslında arrow function var orada ben farketmedim tamam  yani fonksyion içinde fonksyion callback function var orada. yani önce then istek atacak sonra istekten sonra içerdeki fonksiyon gidecek ver ilgi yerlerde kullanılıp bir sonuca varacağız.