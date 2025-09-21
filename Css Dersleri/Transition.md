
```css
  .box{
      width: 500px;
      height: 500px;
      background-color: brown;
      color:white;
      transition: color 0.3s;
    }

    .box:hover{
      color:red;
    }
```

All kullanırsan box classı içindeki tüm propertylere sen hover da verdiğin süre ile hepsini değiştirebilirsin.

ama sadece color değişşin hover olduğunda background mesela beyaz olsun ama transition'a dahil olmasın dersen tek tek belirtmelisin.

```css
    .box{
      width: 500px;
      height: 500px;
      background-color: brown;
      color:white;
      transition: color 0.3s,background-color 0.3s;
    }

    .box:hover{
      color:red;
      background-color: white;
    }
```
Mesela burada şunu yaptık hover olduğunda transition'a yazdığın propertylerin rengi bu süre ile değişecek all kullansan daha az zahmetli olurdu ama bu da color ve background-color'ı etiklemekle kalmazdı width height gibi onları da eklerdi eğer hover da width heightta olsaydı mecuberen all oldugundan hover oldugundan onlarda bir süre animasyona tabii kalırdı.

```css
 .box{
      width: 500px;
      height: 500px;
      background-color: brown;
      color:white;
      transition: color 0.3s;
    }

    .box:hover{
      color:red;
      background-color: white;
    }
```

Mesela bu kodda transition all değirde tek tek seçtik propertyleri bu da bize hover olduğunda color süresini ayarladık değişme süresini all yapsaydık hoverin içindeki hepsi değişirdi white da 0.3s saniye beklerdi ama ben bazen isterim ki sadece renk değişşin iste bu durumlarada tek tek seçmekte iyi olabilir.


`.box`:

```css
transition: transform 0.3s ease;
```

Bu, **`transform` özelliği değişirse**, bu değişimin **0.3 saniyede** ve **ease (yumuşak)** bir şekilde geçmesini sağlar.  
Henüz `transform` uygulanmamış olsa bile **gelecekteki bir `transform` değişimine** animasyon uygulanmasını garanti eder.

`.box:hover`:

```css
transform: translateX(50px);
```

Fareyle kutunun üzerine gelindiğinde, **50px sağa kayma (translateX)** uygulanır.  
Bu durumda `transform` devreye girer ve `.box`'a tanımlanmış `transition` özelliği sayesinde bu kayma **animasyonlu şekilde olur**.

yani transform tanımlamasan bile box da varasyılan olarak none olarak kabul edilir.

yani transition verdiğin class da illa gidip property vermek zorunda değilsin transition da verdin diye sadece ben bunu istiyorun bu kuralı hover olduğunda dersin hover ile değişiklik oldugunda transition hemen onu yakalayacak.


❓ Peki `.box` class'ında 5 tane property varsa, `transition: all` sadece onları mı kapsar?

❌ Hayır, sadece `.box` class'ındakiları kapsamaz.

### ✅ `all`, **herhangi bir anda değişen geçiş yapılabilir tüm özellikleri** kapsar.

Yani `.box`'ta olmasa bile, hover sırasında bir özellik değiştiriliyorsa (örneğin `transform`), bu da `all`'a dahil edilir.

| Fonksiyon     | Davranış Açıklaması                                 | Örnek Geçiş Hissi           |
| ------------- | --------------------------------------------------- | --------------------------- |
| `linear`      | Sabit hızda başlar ve biter                         | Düz, robotik geçiş 🧱       |
| `ease`        | Başta yavaş, ortada hızlanır, sonda tekrar yavaşlar | Doğal ve dengeli ⚖️         |
| `ease-in`     | Yavaş başlayıp hızlanır                             | Bekleyerek başlayan 🚶‍♂️💨 |
| `ease-out`    | Hızlı başlayıp yavaşlar                             | Ani çıkış, nazik bitiş 🎢   |
| `ease-in-out` | Yavaş başlar, ortada hızlanır, yavaş biter          | Denge ve akış hissi 💃      |

Buradaki ease default özelliği transition.


## <font color="#245bdb">Transition Delay</font>

Geçiş başlamadan önceki bekleme süresidir varsayılan değeri 0 dır yani hemen başlar hover olduğunda burada öenmli olan kısım şurası sen 0.2s yazıyorsun ya animasyon kısmına orası o verdigin transition animasyonunun kaç saniyede biteceği yavaş yavaş ilerler mesela sen color::red dersen o hemen red olmaz yavaş yavaş rengi değişir o bunu yapar ama bu delay olan ise animasyonu bekletir verdiğin süre ile o animasyonu çalıştırıır 2 saniye dersen 2 saniye sonra.

```css
transition: color 0.3s linear 2s;
```

delay son kısma yazılır.


Transition'ı biz hover ile tetikliyoruz ya hover'a transition yazmayız bunun nedir hover olmaktan çıktığın an transition özelliğide hemen gider kötü görüntü olur yani bundan biz hover'a değil sadece class'a tanımlayıp hover olduğunda değişecek propertyleri tanımlarız hoverda.