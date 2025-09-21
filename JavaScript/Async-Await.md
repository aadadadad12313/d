
Promisenin bir kolay yazım tarzıda Async Awaittir çünkü sürekli fetch ile then then çok iç içe giriyor async bunu daha güzel bir görünüme sokuyor 

async await kullanabilmek için önce fonksyionun en başına Async yazıyorsun sonra promise dönen yerlere await yazıyorsıun zaten iki tane promise dönen yer var birincisi fetch ile istek attıgımız yer ikincisi response.json bunlar await ile işaretliyorsun.


Await tanımlana yer tamamlanana kadar o satırda bekle alt satıra geçmez bitince devam eder.

```js
const getTodos=async()=>{

 let response=await fetch('Example/item.json');
 const data=await response.json();
 console.log(data);
};

getTodos();
```

en başa fonksiyon tanımlanmadan önce async keywördü yazılır sonra promise dönen yerlere await ile bekletmelisin json ve fetch promise dönüyor ve orada istek bitene kadar beklemesi için await kullanırdı.


<font color="#6495ed">Şimdi burada fetch iki türlü kullanılabilir ister then ile kullan ister await ile bu keywordlerin görevi isteğin tamamlanana kadar beklemesi.</font>

<font color="#6495ed">await daha çok tercih edilir çünkü okunabilirliği daha iyi.</font>

```js
async function getData() {
  try {
    const response = await fetch(url);
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error('Hata:', error);
  }
}
```

```js
fetch(url)
  .then(response => response.json())
  .then(data => {
    console.log(data);
  })
  .catch(error => {
    console.error('Hata:', error);
  });
```