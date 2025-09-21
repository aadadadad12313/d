
Async, yani **asenkron**, JavaScript gibi dillerde işlemlerin **eşzamanlı olmadan** yürütülmesini sağlar. Normalde kodlar sırayla çalışır (senkron), ama async sayesinde bazı işlemler arka planda devam ederken diğer kodlar çalışmaya devam edebilir.


- `async` **anahtar kelimesi**: Bir fonksiyonu asenkron hale getirir. Bu fonksiyon otomatik olarak bir `Promise` döner.
    
- `await` **anahtar kelimesi**: Sadece `async` fonksiyonlar içinde kullanılır. Bir `Promise`'in tamamlanmasını bekler.

### 🔄 Senkron vs Asenkron

| Özellik      | Senkron                | Asenkron                             |
| ------------ | ---------------------- | ------------------------------------ |
| İşlem sırası | Satır satır            | Arka planda çalışabilir              |
| Bekleme      | Her işlem tamamlanmalı | Beklemeden diğer işlemler devam eder |
| Kullanım     | Basit işlemler         | Ağ, dosya, zamanlayıcı işlemleri     |


```js
console.log(1)

console.log(2)

console.log(3)

console.log(4)
//senkron çalışır yani üstteki bitmeden alttaki kod çalışmaz
```

```js
console.log(1)

console.log(2)

setTimeout(() => {

    console.log(3)

}, 2000);

console.log(4)

//asenkron çalışır setTimeOut bu yüzden 2 saniye kod satırda durmaz ilerle  4'ü erkana basar sonra süre dolunca 3'ü de ekrana basar en altta yani
```


XmlHttpRequest ile istek atma fetch'in alternatifi

### 🔄 `readyState` Aşamaları

| Değer | Durum Adı          | Açıklama                                                              |
| ----- | ------------------ | --------------------------------------------------------------------- |
| `0`   | `UNSENT`           | Nesne oluşturuldu ama henüz `open()` çağrılmadı                       |
| `1`   | `OPENED`           | `open()` çağrıldı, istek yapılandırıldı ancak henüz `send()` edilmedi |
| `2`   | `HEADERS_RECEIVED` | Sunucuya istek gönderildi, yanıt başlıkları alındı                    |
| `3`   | `LOADING`          | Yanıtın gövdesi geliyor, `responseText` erişilebilir olabilir         |
| `4`   | `DONE`             | İstek tamamlandı (başarılı ya da hatalı)                              |
|       |                    |                                                                       |

<font color="#6495ed">Örnek :</font>

```js
const request=new XMLHttpRequest();

request.open('GET','https://jsonplaceholder.typicode.com/todos');

request.onload=()=>{

    if(request.readyState===4){

        console.log(request.responseText);
    }
}

/* Buradaki onload fonksiyonu istek bittiğinde başarlı çalışırsa kendi kendiligine tetiklenecek

içindeki readState ise şaun requestin hangi aşamada olduğunu beliretiyor 4 demek en son aşama oldugunda ekrana bas 2 filan yazamadık

mantıkken çünkü 2 de istek atıyor filan derken 4 verilerin çekildgi aşama
responseText ile de ekrana bastırdık

request.status==200 ise statuc code bildiğimiz net core'dan eğer istek attığımzıda 200 döenrse her şey yolunda demesi */

request.send();
```