
| Değer     | Açıklama                                                           |     |
| --------- | ------------------------------------------------------------------ | --- |
| `fill`    | Görsel kutuyu tamamen doldurur. Gerekirse **oran bozulur**.        |     |
| `contain` | Görsel oranı korunur, kutuya **tam sığacak şekilde küçültülür**.   |     |
| `cover`   | Oran korunur, kutu tamamen doldurulur ama **görsel kırpılabilir**. |     |

- `contain`: Görselin tümü görünür kalır, ama oranı bozulmadan kutuya sığdırılır. Kutunun tamamını kaplamayabilir.
    
- `cover`: Görselin oranı korunur **ve** kutu tamamen doldurulur. Ancak gerekirse görselin dışa taşan kısımları **kırpılır**.

Fill varsayılan değeridir img içinde bulunduğu div'i tam kaplar ama img'nin gerçek boyutunu dikkat almaz verdiğin img içinde width height değerini alır ondan bulanık olur.

### 💬 Basitleştirilmiş tanım

- `contain`: Görsel **orijinal oranına sadık kalarak** kutunun **içinde en fazla ne kadar büyüyebiliyorsa** o kadar yer kaplar.
    
- `cover`: Görsel **kutunun ölçüsüne göre** oranını koruyarak büyür ve kutunun tamamını **kaplar**. Gerektiğinde kırpılır.

| Özellik   | `contain`                                          | `cover`                                               |
| --------- | -------------------------------------------------- | ----------------------------------------------------- |
| Yaklaşım  | Görselin oranını koruyarak **tamamını** gösterir   | Görselin oranını koruyarak **kutuyu doldurur**        |
| Boşluklar | Kutu ile görselin oranı uyuşmazsa **boşluk kalır** | Boşluk **kalmaz**, ama görselin bir kısmı **kesilir** |
| Boyutlama | Görsel, oranını koruyarak kutunun içine sığdırılır | Görsel, oranını koruyarak kutunun tamamını kaplar     |
| Kullanım  | Tam görseli göstermek istiyorsan                   | Tasarımda görselin kutuyu doldurması gerekiyorsa      |
|           |                                                    |                                                       |

Mesela ben object-fit: contain; bu konuyu daha önce foot ordering projesinde uygulamısım ne yapmışız olda şunu yapmısız

![[Pasted image 20250728174126.png]]

header alanı mesela burası cover kullanmısız neden peki çünkü resmi fill durumu bulanık hale getiriyor cover kullandık contain kullansaydık yukardan aşağıdan boşluklar oluyor patlıyor kod.
![[Pasted image 20250728174218.png]]
pizza resmi burda 80% kaplıyor diğer yazıların olduğu kısımda 20% kaplıyor sayfayı img 80% içinde oldugundan pizza resmi çok büyük duruyor resim hem kötü duruyor cover verirsek yine büyük olur içindeki div'i kaplar ama contain verilirse içindeki div'i üstten alttan boşluklar bırakır tam istediğimiz görüntüyü oluşturdu.
