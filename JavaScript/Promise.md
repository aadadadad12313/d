
```js
const benimPromisim = new Promise((resolve, reject) => {
  // burası callback function
});
```

burda nesne oluştururken farkedersen fonksiyon tanımladık çünkü Promise bir sınıfıtır ve bu sınıf constructor metodu bizden bir paremetre istemiyor bir function istiyor yani callback function.

```js
const islemFonksiyonu = (resolve, reject) => {
  const basariliMi = true;
  
  if (basariliMi) {
    resolve("İşlem başarılı!");
  } else {
    reject("İşlem başarısız!");
  }
};

// Promise içine dışarıdan fonksiyon veriyoruz
const benimPromisim = new Promise(islemFonksiyonu);
```

yani böylede kullabilrsin ister dışarıda tanımla function'ı ister içinde farketmez.


🧵 Kısaca Özet

| Terim       | Anlamı                                      |
| ----------- | ------------------------------------------- |
| `Promidse`  | Asenkron işlemi temsil eden JS sınıfı       |
| `resolve()` | Başarılı sonuç bildirir → `.then()` çalışır |
| `reject()`  | Hatalı durum bildirir → `.catch()` çalışır  |
| `then()`    | `resolve()` çağrılırsa burası çalışır       |
| `catch()`   | `reject()` çağrılırsa burası çalışır        |


Mantık Şu

Callback functionda

```js
function işlemYap(callback) {
  let success = true;

  if (success) {
    callback(null, "İşlem başarılı!");  // Hata yoksa, başarı durumu
  } else {
    callback("Bir hata oluştu.", null);  // Hata varsa, error parametresi ile
  }
}

işlemYap((error, result) => {
  if (error) {
    console.error(error);  // Hata
  } else {
    console.log(result);    // Başarı
  }
});
```
böyle yapabiliriz


yada promise ile söylede 

```js
const prom = new Promise((resolve, reject) => {
  let success = true;

  if (success) {
    resolve("İşlem başarılı!");  // Başarı durumu
  } else {
    reject("Bir hata oluştu.");  // Hata durumu
  }
});

prom
  .then(result => console.log(result))  // Başarı durumunda
  .catch(error => console.error(error));  // Hata durumunda
```

Promise ile async işlemler yapmak daha kolay catch bloğu var bu da bir avantaj.

<font color="#4bacc6">Fetch Kullanımı</font>

 biz sürekli böyle mi yapacağız hayır tabiki fetch kullancağız fetch aslında bir promisse döndürür ondan doğrudan kullanacağız hazır fonksyionu xmlHttpRequest ile hiç uğraşmayacağız.

```js
fetch("Example/item.json")

  .then((response) => response.json())

  .then((data) => {

    console.log(data);

  })
  .catch((err) => {

    console.log(err);
  });
```

doğrudan then kullanbiliyoruz çünkü pronise kendisi dmndürüyor fetch ilk then json olan objeyi js objesine çeviriyor sonra bir promise daha dönüyor sonra ikinci then datayı basıyor ekrana yani bu then asaması aslınca `resolve()` catch ise'de  `reject()`' aslında yukardaki kodda kendimiz yaptıgmızda bolca kullandık.

