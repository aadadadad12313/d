

```js
class MyClass{

    Name="serkan"; //property
    age=20;

    constructor(){
        console.log(this.Name,this.age);  //this ile ulaşıyoruz değişkene c# da'da böyle aslında ama compiler
        //  bunu compiler seviyesinde yapıyordu ama burada yok tabii.
    }
}

const n1=new MyClass();
console.log(n1);
```

class içinde yalnızca property tanımlanabilir local değişkenler tanımlanamaz const let gibi.

bu durum js objesinde'de öyle aslında

```js
const kullanici = {
  ad: "Serkan",
  yas: 28,
  email: "serkan@example.com",
  aktif: true
 }
```

Yani ister Class ister Obje farketmez const let kullanılamaz bunlar değişken tutmak için tasarlanmamış ondan sadece property tutulmalıdır. propertyde böyle tanımlanıyor js'de.

Kritik Yapalım şimdi this.ad diyebiliyorsak doğrudan o zaman bu bir objedir this arkada obje olarak tutuluyor demek yani

```js
const user = {};
user.ad = "Serkan";
user.yas = 30;
```

bir objeyi mesela direk tanımlandıgı anda içine data vermek zorunda değiliz sonra verebiliriz o zaman bu mantıkla this de bir obje boş bir obje. classlarda kullandıgımız zamanda this.name this.age diye diye içini doldurabiliyoruz bu propertylerin.

Yada 

```js
class MyClass{

    constructor(){
      this.Name='serkan';
      this.age=20;
      console.log(this.Name,this.age);
    }

    Yazdir(){
        console.log(`${this.Name} ${this.age} başarıyla yazdırıldı.`);
    }
}

const n1=new MyClass();
console.log(n1,n1.Yazdir());
```

JavaScrriptte this keywordü kullanmak zorundaısn c# daki gibi otomatik bir yapı yok kendisi yapmıyor.