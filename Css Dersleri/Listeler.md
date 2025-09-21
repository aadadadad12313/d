
```html
    <ul>
      <li>ssd</li>
      <li>ssd</li>
      <li>ssd</li>
    </ul>
```

burada oluşacak madde imlerini kaybetmek için list-style-type veya list-style propertylerine none dersen kayberdersin ama burada esas nokta hangisine bunları ul'a mı yoksa li'ye mi vereceğin.

cevap her ikisini de verebilirsin farketmez ama ul'a vermek daha iyi en kapsayıcısı olduğundan.

bazen ul li olsa bile madde imleri gözükmez bunun sebebi ise 

```css
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
```

universal selector yani *  ile bütün etiketlere margin ve padding sıfırlıyoruz ya ondan kayboluyor şimdi neden ve nasıl kaybolduklarını inceleyerim.


```css
ul{
padding:0;
margin:0;
}
```

şimdi padding 0 verdiğinde şunlar olacak

padding 0 verdiğin an ul içinde var olan default 40px sıfırlanır.

şimdi gelelim bu padding content'e etki ediyor ama madde sembolüne etki etmiyor yazıya etki ediyor çünkü madde sembolünü li altında bulunan bir psoude element buna bakabilirsin developer toolsdarn marker adlı bir psoude element var yani ul padding'i yazının madde sembolünün değil

dersen eğer ul 0 oldugunda tamam yazı sola geli ama madde sembolü neden o da sola geliyor oldugu yerde kalsın sembol dersen onda da marken içinde bir property var

```css
list-style-position: outside;
```

<font color="#f79646">bu property madde sembolünün neyin içinde olacağını belirliyor bu outside özelliği madde sembolü content içinde olmasın ayrı olsunlar demek ama inside olsaydı o zaman madde sembolü content içine dahil edilitdi.</font>

<font color="#92d050">eğer inside olup content içine dahil edilitsen padding 0 da madde imi kaybolmaz en sola gelir çünkü inside sembölüde content'e aldığından onla beraber tutuyor ama varsayılan durumda outside ayrı tuttugundan ul 0 oldugunda content en sola madde sembolü ayrı oldugundan en solun soluna gelir.</font>

<font color="#f79646">şimdi dersen eğer tamam anladım ben psoude element oldugundan ayrı tutuluyorlar ama kapsayıcının ul yani 0 oldugunda psoude nasıl etkilenmiyor dersen bunun sebebi padding ve margin inherit edilemezler yani kalıtım olarak verilemezler ki ul 0 oldugunda sadece ul padding'i sıfırlanır yani padding kalıtım olarak verilemez ondan içindeki psoude veya div de olsa kalıtım filan yok ondan sadece ul etkilenecek ul o da content olarak yazıyı alıp başa koyacak.</font>


## <font color="#f79646">Özet</font>

yani zaten doğru baktığından ul üzerine geldiğinde developer tools dan şunu göreceksin

![[Pasted image 20250819182056.png]]

padding gördüğün gibi yazıların orada madde sembolünde durmamış devam etmiş yani ul 0 oldugunda madde imi ne vermiyorsun içeriğer veriyorsun içerik en sola geliyor madde sembolüde bundan en solun görünmeyen tarafta kalıyor olay bu.

ama ul da biz list-style-position: inside; dersek madde sembolü yazıya dahil olur kaybolmaz yani varsayılan outside olduğundan yazıya dahil değeri ondan solda görünmeyen alanda kalıyor.

### 🔹<font color="#f79646"> 2. ::marker farklıdır</font>

`::marker`, özel bir pseudo-elementtir.

- Normal `::before` gibi “content box” içinde değildir.
- Tarayıcı, onu **list item box’ın dışında özel bir alanda** çizer.
- Yani `li` kutusunun “kenarına” sabitlenmiş gibidir.
    
O yüzden:
- `ul` veya `li` padding → **content box’ı etkiler**
- Marker’ın bulunduğu özel alanı etkilemez

<font color="#f79646">yani psoude element tamam var ama bizim css de yazdığımız gibi bir classın içinde değil farklı bir yerde tarayıcı onu orada tanımlıyor ondan ul 0 olması kapsayıcının içinde olmadığından ul 0 dan etkilenmez eğer bizim her zamanki gibi psoude element olsaydı etkilenirdi kasapyının içinde bir öğer varsa kapsayıcısının padding'ininden etkilenecek tabilide ama burada bu yok işte.</font>