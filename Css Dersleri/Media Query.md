
Max-width 780px mesela demek 780px ve altında geçerli

Min-Width 1200px mesela 1200px ve üstünde geçerli.

| Yazım                     | Ne Anlama Gelir?                                  |
| ------------------------- | ------------------------------------------------- |
| `@media (max-width:...)`  | Tüm cihazlarda geçerli, sadece genişliğe bakar    |
| `@media screen and (...)` | Sadece ekranlarda (monitör, telefon) geçerli olur |

Ondan biz screen and ile kullanma taraftarıyız .

```css
   body {
        background-color: cornflowerblue;
      }

      @media (max-width: 768px) {
        body {
          background-color: brown;
        }
        h2 {
          font-size: 20px;
        }
      }

      @media screen and (max-width: 500px) {
        body {
          background-color: blueviolet;
        }
        h2 {
          font-size: 10px;
        }
      }
```

iki tane yazım tarzı var screen and daha iyi çünkü sadece leptop,bilgisayar ve tablette geçerli sadece @media ise tümünde yani yazıcı filan tv de bunlar olmasın diye biz screen and koşulu ile belirtiyoruz.


```css

     *{
        box-sizing: border-box;
     }

     .box{
        width: 25%;
        height: 250px;
        background-color: cornflowerblue;
        border:1px solid black;
        float:left;
     }

     @media screen and (max-width:768px){
        .box{
            width: 50%;
        }
     }

     @media screen and (max-width:600px){
        .box{
            width: 100%;
        }
     }
```

sayfanın genişliği mesela o anda 1000px diyerim genişlik neyse onun 25'i 250px diye sallayarım divler block level oldugundan sıgmadı zaman altta geçer sığan yerlerde responsivesi ayarlanmasa sayfayı küçülttükçe sığan yerler birbirine iyice sıkışır işte tam bu durumda @media query bize belli şarta gelince şunu yap demmeizi sağlıyor.