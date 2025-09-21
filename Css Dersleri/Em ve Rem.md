
Em Kullanımı

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
   
   body{
    font-size: 20px;
   }
   p{
    font-size: 2em; /* Veya 200% diyerekde body'nin 2 katı şeklinde yazabilirsin. */ 
    /* 40 px çıktısını verecek 
    bide bunu 2em değilde 200% şeklinde de yazılabilrdi emle'de farketmez 
    */
   }
    </style>
  </head>
  <body>
     <p>ddad</p>
  </body>
</html>
```

şimdi burada body varsayılan deperi 16px'dir ama biz varsayılan değereni ezip 20px yaptık ondan em 1rem'i 16px değil 20px olarak algılayıp ona göre hesaplamalar yapalacak.

tamam ama bu em kullanımı değişiyor kapsayıcı font size mesela 20px ise içerdeki de ona göre ebeyven gibi onu alıyor body'e bakmıyor kodlar büyüdükçe ama ben bir değeri sabit kılmak istiyorum buda rem ile oluyor rem 16px'dir değişmez ebeveyn gibi ilişkisi yoktur kendi başına takılır yani.

Rem Kullanımı

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  
	<style>
	
   body{
    font-size: 20px;
   }
   div{
    font-size: 2em;
   }

   p{
      font-size: 3rem;
   }

    </style>
  </head>
  <body>

     <div>ddad
      <p>sdada</p>
  
	 </div>

  </body>
</html>
```

Burda em var div 40px yani ama p de rem kullanılmış eğer em kullanılsaydı orada ne olurdu

tabiki em kapsayıcıya göre kendini ayarlardı o zaman 120px'el olur = yani 40px div oldu içinde p var 40 * 3em =120px oldu ama ben rem kullandım ondan kapsayıcı 40px'el olsun sabit 16px değer p nin değeri 48px'el gelir. rem kapsayıcya göre içerdeki öğeyi hizalamaz yani ondan artık ben rem kullancam. burda rem body'e de bakmıyor bu arada ondan bir const değeri var body'e bile eziyor.