

```css
 <style>
      .box {
        width: 1400px; /*burası bizim kapsayıcımız görevi görecek ve en fazla 1400px kaplayacak 2500 verisen alt ögelerden
        birine taşma olur*/

        background-color: cornflowerblue;
        height: 100px;
      }
      .box2 {
        width: 100%; /*serkan dendigi anda 1400px kapsayıcının genişligini 1400px olarak alır
    gidipte 2500px deseydin olmazdı çünkü üstündeki kapsayıcı 1400pxel en fazla bu kadar kaplıyor*/

        height: 100%;
        background-color: brown;
      }
    </style>
```

```css
 <div class="box">

      <div class="box2">İçerigi en fazla 14000px el olabi</div>

    </div>
```

