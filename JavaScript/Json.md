
Json.Parse

json objesini javascript objesine çevirir

```js
const jsonString = '{"isim":"Ahmet","yas":25,"aktif":true}';

const jsObjesi = JSON.parse(jsonString);

console.log(jsObjesi.isim);  // "Ahmet"
console.log(jsObjesi.yas);    // 25
console.log(jsObjesi.aktif);  // true
```

JSON.stringify Nedir?


Javascript objesini json objesine çevirir.

```js
const obj = {
  isim: "Ahmet",
  yas: 25,
  aktif: true
};

const jsonString = JSON.stringify(obj);

console.log(jsonString);
// Çıktı: '{"isim":"Ahmet","yas":25,"aktif":true}'
```


Json ile Obje arasındaki küçük fark

```json
[
    { "Name": "serkan","Age":20},
    { "Name": "mehmet","Age":25},
    { "Name": "selin","Age":30},
    { "Name": "ahmet","Age":18},
]
```

bu bir json'dır bunu nasıl anladık peki çünkü anahtar yerinde " " var bu jsondır bu kadar.
ama sakın js objesi ile karıştırma çünkü birbirlerine o kadar benziyorlarki

Js Objesi 

```js
[
    { Name: "serkan","Age":20},
    { Name: "mehmet","Age":25},
    { Name: "selin","Age":30},
    { Name: "ahmet","Age":18},
]
```

" " yok burada o zaman bu js objesi bu kadar.



