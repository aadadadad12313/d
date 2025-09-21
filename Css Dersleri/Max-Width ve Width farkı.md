
## <font color="#00b0f0">Tam Mantık </font>

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
    
       img{
        max-width:800px;
        height: auto;
      }
    </style>
  </head>
  <body>
      <img src="https://images.unsplash.com/photo-1501785888041-af3ef285b470?w=640" width="640" alt="Dağ manzarası">
    </div>
  </body>
</html>
```

max-width nedir sence adı üstünde maximum genişlik 800px olsun yani resim boyutu orjinal yani 640 burada max-width 800 tamammı
yani ne olması gerekiyor mantıkken 800px sınır ihlali yok değilmi max width'de o zaman 640px resmin boyutu olacak.
haa ne zaman sınır ihlali olur max-withde o zaman max-width karşıstında ceza değeri kaç 800px o zaman 800px ceza olacak resme olay bu kadar.


yani max-width'e sabit değer verilmez sürekli zaten olması gerkende bu % lik deper verilir ondan o yüzde değeri tarayıcının yada kapsayıcını width'i degil mi haa iste aynı mantık oluyır yine % den px'el çıkacak yine karşılaştırma olacak ve o değişen artık tarayııcnı yada kapsayııcnın o anki widyh'i neyse o kadar resmin boyutu olacak o zamanda.

### Karşılaştırma:

```
Resmin istediği: 640px
Max-width limiti: 500px
640px > 500px → LİMİT AŞILDI! cezayı yedin max-width değerini almak zorundasın ne zaman cezadan çıkarsın o zaman verdiğin resmin boyutunu kullanbilirsin ama o zamana kadar cezalısın diyoruz kısaca.
```

<hr>


```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
    
       img{
        width:100%;
      }
    </style>
  </head>
  <body>
      <img src="https://images.unsplash.com/photo-1501785888041-af3ef285b470?w=640" width="640" alt="Dağ manzarası">
    </div>
  </body>
</html>
```

width 100% demek dinamik yüksekik demek yani şuan burada herhangi bir div olmadıgında img'nin kapsayıcısı body oldugundan onun şu anki boyutu ne ise body'nin img'nin boyutu da o  oluyor.

Max Width ise 

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
    
       img{
        max-width:100%;
      }
    </style>
  </head>
  <body>
      <img src="https://images.unsplash.com/photo-1501785888041-af3ef285b470?w=640" width="640" alt="Dağ manzarası">
    </div>
  </body>
</html>
```

- `max-width: 100%`: Görsel **en fazla kapsayıcı kadar olabilir**, ama daha küçükse büyümez.
yani her img'nin kendine ait bir width'i vardır ona göre max-width kıyaslama yapıp büyükse küçülttür değilse aynen bırakır.

##### <font color="#6495ed"> ondan biz tam ekrandayız resim 640px biz büyügüz ekran olarak  o zaman resim 640px ama ekran küçüldü biz kaybettik o zaman devreye şart koyduk ya max-width sartı dedik orada kaybedersek savaşı 100% olsun dedik tarayıcı aldık hangi boyutausa resimde o boyutta olacak yani mantık tamamen bu.</font>

yani tam ekranda devre dışı kaldı devreye girmesi için bekliyor neyi bekliyor resmin biizm ekrandan büyük olmasını bekliyor o zaman devreye girecek ve 100% koşulu o an uygulacak 500 400 300 diye diye otomatik arttırma ama tam ekranda olmayacak width:100% daha kontrollü hali şunda geçerli bunda değil mantıgı var.

 Senaryo 1: Ekran/kapsayıcı 1000px

- Görsel 640px
    
- 640px < 1000px olduğu için ➜ **Görsel boyutu değişmez.**
    
 Senaryo 2: Ekran/kapsayıcı 500px

- Görsel 640px
    
- 640px > 500px ➜ **Görsel otomatik olarak 500px’e küçülür.**
    

Yani görselin **kendi boyutunu "kapsayıcıya göre" sınırlıyor.**

