

```css
width: 100%;
```
Elementin genişliğini, içinde bulunduğu parent (üst) elementin genişliğinin %100'ü kadar yapar.

```css
height: auto;
```

Yüksekliği kendin hesapla, içeriğe veya oranına göre otomatik ayarla demek.

yani olay şu verilen image clasının o anki width değerini alıyor sonra resmin orjinal width height'ını alıyor yani oranını buna göre hesaplama yapılıyor.

- Orijinal boyut (intrinsic size): **728 × 410 px**
    
- Yeni genişlik (CSS ile verilen width): **529 px**
    
- `height: auto` → oran korunacak. alıyor css sayfası bu bilgileri sonra


oran olarak 410÷728    çıkıyor cevap 0.5631868

sonra çıkan cevap ile şuanki width'i çarpıyoruz ve şu anki yükseliğimizi hesaplıyoruz.

529×0.5631868     çıkıyor cevap   297.8


böyle karar veriyor auto olunca dersen biz neden auto kullanırım şimdi img classında width'i dinamik yapıyoruz. height'ı sabit bırakırsak resim çok genişler height hep 300px de olur resim çok küçük olur genişliği height yine 300 olur yani bu durumu istemezssen auto demen yeterli.
