
Use State aslında bir render işlemi yanii react bir render sistemi olduğundan tepeden tırnağa sen normal bir field tanımlarsan eğer o bir kez çalışır sonra sen bir daha butona tıklarsın o component render olmaz 


React Tepeden Tırnağa bir Render ile çalışır.

Sen değişken kullanarak ekranda bir değer bastıramazsın yani bir butona tıklandığında 0 yerine 1 olsun Java Scriptte değilsin Reacttasın React diyorki sana beni yenileyeceksin render edeceksin süreklü diyor senin tanımladıgın bir field render yapıyormu hayır o zaman senin field kodda çalışıyor ama web sayfasında 0 kalıyor ama consoldan bakarsan o field çalışıyor.

o zaman biz field yerine bişey kullacaz ama ne yani sayfayı render edecek verimi kaybetmeyecek field olacak ama reactta uygun bir bişey işte Tam Bu <font color="#92d050">Use Sate dir.</font>


UseState şöyle çalışır sen bir değer verirsin initial value yani sonra sen ona değer gönderdiğinde o değer ile inital value kıyaslanır yani değişme olduğu an ekranı Render Edecek Use State Hook'u.

```js
import { useState } from "react";

function App() {
  let [count, setCount] = useState(0);

  return (
    <>
      <p>sayaç: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Arttır
      </button>
    </>
  );
}
export default App;

```

önce 0 dan başladı sonra 1 oldu render değişti değer render'a tabii tuttu.
sonra use state artık 1 sonra sen 2 yaptın gönderdin adam baktı yine değişti render'a tabii tuttu gene.

Dikkat Eğer bunu bir Field İle Yapsaydık Şu Olurdu

```js
let count = 0;

function App() {
  function arttir() {
    count++;
    console.log("Yeni count:", count); // sadece console’da görürsün
  }

  return (
    <>
      <p>sayaç: {count}</p>
      <button onClick={arttir}>Arttır</button>
    </>
  );
}

export default App;
```

Çalıştı ama consolda değiişiyor sayfa 0 da kaldı render lazımmm olmazsa render hiçbir anlamı yok yani Reactta kod yazmanın sıfırsın sıfır burada.

```csharp
import { useState } from "react";
function App() {
  let count = 0;
  const [count2, setCount] = useState(count);

  return (
    <>
      <p>sayaç: {count2}</p>
      <button
        onClick={() => {
          count++;
          setCount(count);
          console.log(count);
        }}
      >
        Arttır
      </button>
    </>
  );
}
export default App;
```

bu örnekte ise hem useState hem field tanımladım.

Şimdi sadece field olsaydı field değerleri kaybolmazdı neden render etmezdi sayfayı ondan benim field değerleri normal js gibi kaybolmaz.

Ama useState ile Field varsa useState render ettiği anda senin componentinin tüm kodları fieldları yani sıfırlanacak.

şimdi kodu incelersek eğer

count 0 sonra  count++; ile count 1 oldu useState 1 oldugundan ekranı render etti 1 oldu ama sonra count 1 silindi 0 oldu gene neden çünkü render da tüm filedlar silinecek.

count 0 oldu count++ ile count yine 1 oldu useState'ye 1 gidecek ama dikkat useState bir önceki kayıtlı değeri 1 olarak aldı ya şimdi değeride 1 olarak alırsa baktı değerler aynı render yok ekranda 1 kaldı yine render olmadı ve field'lar sıfırlanmadı yani count=1 oldu sıfırlanmadı dikkat.

sonra bir kez daha butona tıklarsa count=1 sonra count++ ile 2 olduk useState 2 gidince use state içinde 1 yok mu 1=2 eşitmi diye bakar hayır o zaman güncelle ve render et der sayfanın ekrnaında 2 yazar state 2 olur render olacagından field artık tekrar 0 olur gibi gibi gidecek.