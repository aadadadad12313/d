
Local Stogare tarayıcıda saklanır ve sayfa yenilense, tarayıcı kapansa bile kaybolmaz.

```js
localStorage.setItem('name','Serkan');
localStorage.setItem('Age',20); //ekleme yapar

let ad= localStorage.getItem('name') //listeleme yapar
let yas=localStorage.getItem('Age')

localStorage.setItem('name','serkan ve mızık');
//lcoal stogare'de güncelleme yapmak için var ola key yazılır varsa o key güncellenir yeni değerle yoksa yeni oluşturulur bu anahtarla
console.log(ad)
console.log(yas)

localStorage.removeItem('name'); //name sahip datayı siler
localStorage.clear(); //tüm datayı local stogareden siler
```


```js
const variable=[
    {hobby:'swimming',person:'can'},
    {hobby:'playing card',person:'can'},
    {hobby:'take a trip',person:'can'}
]

localStorage.setItem('todos',JSON.stringify(variable));

const storedData=localStorage.getItem('todos');
```

Local stogare string türünde ister veriyi onun için local Stogareye bir obje vereceksen bunun bir string formatta olması gerkir bunun içinde json'a dönüştürme yapılması gerekir.