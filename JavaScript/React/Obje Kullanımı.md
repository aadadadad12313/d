
onemli::Arrow Function,obje kullanımı

Objenin içinde Fonksiyon Tanımlama

```js
const calc={

    "/":(firstNumber,secondNumber)=>{
        return firstNumber/secondNumber;
    },
    
    "/":(firstNumber,sendNumberValue)=>firstNumber/sendNumberValue,
    
     "/": function (firstNumber, secondNumber) {
        return firstNumber / secondNumber;
    }
}
```

1. Kullanım Arrow Function
2. Kullanım Arrow Function Kısa yazımı
3. Kullanım Normal Function kullanarak yapıldı.


yukardaki kodu çağırma işlemi de şöyle  yapılır.

```js
console.log(calc["/"])
```
önce [  ] ile anahtar değerini verisen sana gider valuesini verir.
ama bunu valuesi bir değer değil fonksyion ondan çıktısı şöyle olur 

```js
(firstNumber,sendNumberValue)=>firstNumber/sendNumberValue
```

yani value fonksyion ama çalıştırılmadı daha ondan fonskiyonu bize metin olarak verdi ama çalışması lazım nasıl şöyle

```js
console.log(calc['/'](20,10))
```

(    ) paremetreleri dolduruyorsun fonksiyon çalışıyor.


her üçü de aynı işlemi yapar.

```js
const user = {
  isAdmin: true,
  isOnline: false
};
```

boolean değer de vereblirsin objeye.


Bide şöyle kullanım Var Objeler de methodun key value değirde propert şeklinde

```js
const user = {
  name: "Serkan",
  greet: function() {
    console.log("Merhaba " + this.name);
  }
};

user.greet(); // Merhaba Serkan
```

bu key value şeklinde değil property şeklinde method ondan direk user.greet(); şeklinde çağırırsın ama en üstte olan kod key value oldugundan onu böyle kullanmazssın.