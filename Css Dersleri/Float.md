
Float yan yan hizalamada kullanılır

```css
<style>
   .container{
    background-color: gray;
    overflow: auto;
   }
  .box{
    width: 200px;
    height: 200px;
    margin: 2px;
  }
  #first{
    background-color: red;
    float:left;
  }
  {
    background-color: cadetblue;
    float:left;
  }
  #third{
    background-color: green;
    float:left;
  }
  </style>
</head>
<body>
<div class="container">
    <div id="first" class="box"></div>
    <div id="second" class="box"></div>
    <div id="third" class="box"></div>
</div>
</body>
</html>
```

Floatın yan yana getirmesi biraz uğraştırıyor çünkü csslerin davranışını değiştiriyor

<font color="#6495ed">Mantık Şu</font>

Biz kodda kutuya yükseklik vermişşiz bundan containerin genişligi divlerin genişliğe kadar olmuyormu Oluyor evet sonra

Ancak biz `float: left;` dediğimizde:

- Bu kutular normal belge akışından çıkar.
    
- Tarayıcı artık bunları **container'ın içeriği** gibi saymaz.
    
- Bu yüzden **container'ın yüksekliği sıfır gibi davranır**, yani `gray` arka plan görünmez.

1  clear: both; bunu ise ilgili floatları yazdıktan sonra tanımlıyorsun ama bu daha iyi bir yöntem bu sorunu çözmek için yapılmıştır. 

Not: `clear` özelliği, sadece **float’tan sonra gelen öğelere** uygulanır.

Örnek 
```css
  <div style="clear: both;"></div>
  //bu kodu boxların altına yazman gerkir
  //Float’lı öğelerden **sonra** bir temizleyici `div` eklersin.
  //Bu, float’ların etkisini bitirir. Container içeriği tanır ve yüksekliğini alır.
```

2  overflow:auto; kapsayıcıya vermelisin evet overflow aslında taşmada kullanılır ama float yapılanmasında'da işe yarıyor yani:

> Bu, float’lı öğelere göre **otomatik olarak yüksekliği ayarlar**. Daha sade bir çözümdür ve genelde tercih edilir.


After Becore burada anlaman lazım Onların matığıda şu



```csharp
<!DOCTYPE html>

<html lang="en">

  <head>

    <meta charset="UTF-8" />

    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <title>Document</title>

    <style>

      .container {

        background-color: gray;

        /* overflow: auto; */

      }

  

      .box {

        width: 200px;

        height: 200px;

        margin: 2px;

      }

  

      #first {

        background-color: red;

        float: left;

      }

  

      #second {

        background-color: cadetblue;

        float: left;

      }

  

      #third {

        background-color: green;

        float: left;

      }

  

      .clearfix:after {

        content: "";

        display: block;

        clear: both;

      }

    </style>

  </head>

  <body>

    <div class="container clearfix">

      <div id="first" class="box"></div>

      <div id="second" class="box"></div>

      <div id="third" class="box"></div>

    </div>

  </body>

</html>
```

burada after kullanıldı çünkü biz clear:both en son box divin tam altına yazmak istiyoruz ondan clearfix classın  afteri yani tam yanına gibi düşün third classın altı olmazmı olur oraya kodu koyar ve biz elle htmle dokunmayız ve böylece baktıgımızda koda htlm'de bu bile olmaz bu da temiz kod yazmamız demek aslında tek mantık bu after demek yanına geliyor kapsayıcıya verdigimizden en sona geliyor bunun sebeni div en son bittigi yere geliyor tanımlandıgı yerin en sonu demke nerede tanımlanıyor ilk başta div nerede bityor thirde orada tanımlanıyor aslında görünmeyen kod bu 

Görünmeye

```css
<div class="container clearfix">
  <div id="first" class="box"></div>
  <div id="second" class="box"></div>
  <div id="third" class="box"></div>

  <!-- after burada devreye girer 👇 -->
  //after ile yağtığımızdan dolayı bu alttaki satırı yazmadık after bunu en sona koydu al sana mantık
  <div style="content: ''; display: block; clear: both;"></div>
</div>
```

biz htmli kirletmedik css ile yaptık burayı

video 20