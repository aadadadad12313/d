
```js
<button onClick={ () => handleClick(value)}>Tıkla</button>
```

```js
const calculatorTitle=document.querySelector('.header h1');
const buttons=document.querySelectorAll("button");
const resetButton=document.getElementById("reset");

// şimdi ilk adım buttons içinde operatörleri ayıklamamız lazım
// çünkü biz sadece sayı butonlarına işlem yapacağız bunun
// içinde classı olmayanlar diyeceğiz classı olmayanlar
// sayı butonları ya operatörleri şurtlayacağız.

buttons.forEach(button=>{
    // butonların classı varmı diye baktık eğer bir butonun
    // classı bile varsa lenght değeri 1 olacağından şutlayacağız.
    if(button.classList.length===0){
 
        // button.addEventListener('click',sendNumberValue(button.value))
        // tamamda bu kod yanlış paremtre verdik. bu zamana kadar parentesiz yazdık
        // ama ben paremetre verecem metodlar böyle olur dersen bunu bildirmenin yolu şu
        // bura da hata vermesinin sebebi şu direk çalışıyor () parentez olduğunda bunu bildirmen gerek.

        button.addEventListener('click',()=>sendNumberValue(button.value))
        // ()=> eklemek bu bildirim hemen çağırma metodu beklet demek yani
        // bişey olunca çalışacak bir fonksiyon demek
        // yani reacttaki mantık buradan geliyor reactta genellikle paremetresiz fonksyion gönderilmez.
    }
})
```

Bu reacttaki kullanım tarzı js den geliyor paremetre vermek istiyorsan ( ) kullanırsın ama programlama dünyasında metod tanımı olduğundan hemen çalışacak bu da hata verdirir daha dom yüklenmeden filan ondan ()=> şeklinde tanım ile bu sorun aşılır asıl mantığı js den geliyormuş yani.