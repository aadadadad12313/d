
|Özellik|Açıklama|Etiket içerir mi?|CSS'den etkilenir mi?|En çok nerede kullanılır|
|---|---|---|---|---|
|`textContent`|Elementin içindeki tüm düz metni alır. Görünmeyen içerikleri de sayar.|❌ Hayır|❌ Hayır|Veri analizi, test, kod içinde veri kontrolü|
|`innerText`|Sadece görünür (CSS ile görünürlüğü etkilenmiş) metni alır.|❌ Hayır|✅ Evet|Görsel testler, kullanıcıya gösterilen veri|
|`innerHTML`|HTML etiketleriyle birlikte tüm içeriği getirir.|✅ Evet|❌ Hayır|HTML şablonları, dinamik içerik değiştirme|

✅ Kısaca:

- `textContent`: Hızlı ve güvenli düz veri okuma. En sık **script içi kontrollerde**.
    
- `innerText`: Gerçekten görünür olanı alır. Özellikle **UI testlerinde** tercih edilir.
    
- `innerHTML`: HTML’li yapı gerekiyorsa **şablon üretimi ve modifikasyon** için vazgeçilmez.


```html
<div id="ornek">
  <span style="display:none;">Gizli</span>
  <b>Görünür</b> Yazı
</div>
```

```js
const el = document.getElementById("ornek");
console.log("textContent:", el.textContent);   // "GizliGörünür Yazı"
console.log("innerText:", el.innerText);       // "Görünür Yazı"
```

- `textContent`: Hepsini verir → "GizliGörünür Yazı"
    
- `innerText`: Ekranda görüneni verir → "Görünür Yazı" Çünkü `<span style="display:none;">Gizli</span>` CSS ile gizlenmiş, dolayısıyla `innerText` bunu almaz.

Dikkat edersen innerText display none olan ksımı almamış çünkü css mantığına duyarı
text content direk alıyor duyarlı değil 
innerhtml ise html etiketleri ile alıyor. inner html ile bir p etiketini almak istiyorsan bir kapsayıcıya innerhtml diyeceksin eğer p doğrudan innerhtml dersen bu textContent gibi çalışır içindeki metni verir bundan bir kapsayıcısına vermek zorundasın.

kısaca innerhtml css deki psoude elementlerine benzer yani after gibi psoude nasıl kapsayıcısını içine en sonuna ekliyorsa inenrhtml de içine ekliyor kapsayıcısının. 

yani innerhtml çok önemli hele de js içinde dinamik bir etiket oluşturup html de onu baştırmada