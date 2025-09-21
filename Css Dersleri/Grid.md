
Bir grid oluşturmada iki şey söz konusudur

1 grid container --container'a grid propertleri yazılacak
2 grid item -- ise bu propertylerin görevlerini yapacak



Developer toolsdan grid sistemine özel bir yer var orası layout bölümü var oradan grid ile hizalamanı daha kolay daha kafanda canlandırması daha kolay olabiliyor.


## <font color="#6495ed">Grid Template Areas</font>

Sanırdığı kadar kolay değil çok ince bir ayrıntısı var o da şu mesela isimleri verdik her bir seçici ile grid-area diye sonra gittik bunu grid-template-areas da sutunları tanımlıyoruz ya orada çok ince bir ayrıntı var.

```css
@media screen and (max-width: 768px) {
  .gridLayout {
      grid-template-areas:
    "tower tower"
    "flower old"
    "modern serious"
    "russian russian"
    "business avm"
    "greenHome greenHome"
  }
}
```

bu kod tamamen geçerli çünkü 

tower yazdık ya bunun yanına tower yazdık olur yan yana aynı isimler olur ama birdaha aynı isimde alt tarafta yine tower'a tower olmassa tekli tower kesinlikle kod olmaz patlar çünkü dikdörtgen biçiminde bakıyormuş bu property.

mesela

```css
@media screen and (max-width: 768px) {
  .gridLayout {
      grid-template-areas:
    "tower tower"
    "flower old"
    "modern serious"
    "russian russian"
    "russian avm"
    "greenHome greenHome"
  }
}
```

russianda patlar hem iki tane yan yana russian demişşin hemde altuna ilk sutuna russian demişşin 
L şekli oluşuyormuş dikdörgen şekli lazmımmış.

yani iki ismi yan yan'a verirsen daha da bunu yan yana isim tanımlama yapmasan kod olmaz kesinlikle


```css
@media screen and (max-width: 768px) {
  .gridLayout {
      grid-template-areas:
    "tower tower"
    "flower old"
    "modern serious"
    "russian russian"
    "russian russian"
    "greenHome greenHome"
  }
}
```
bu olur ama tek bir yere russian yazılırsa solunda veya sagında farketmez farklı bir isim olursa çalışmaz.

```css
  grid-template-areas:
    "tower tower serious"
    "tower tower old"
    "modern flower flower"
    "russian business business"
    "russian business business"
    "avm avm greenHome"
    "avm avm greenHome";
}
```

Çalışır bu kodda tamamen doğru.

```css
grid-template-areas:
    "a a a"
    "b c c"
    "b c c";
```

Mesela bu da çalışır a a a demiş daha da a koymamış neden çünkü kullanamazsın.
c c demiş ve aynı satırın altına tam aynı yere bak ne sag ne sola c c tekrar demiş gitmişte başa c c b dememiş neden çünkü yapamazsın.

bunun sebebi belli bunu öyle kafana göre yazamassın bu dikdörtgen den kastı biz grid-column grid-row kullanmayalım daha kolay yazalım diye adamlar bunu yapmış biz öyle oraya a buraya b dersek grid-column ile yapamadıgını bunla da yapamazsın değil mi sondan başlasın ama alttın ilk satırına gelsin gibi bir söylem olamaz.

```css
grid-template-areas:
  "a c c"
  "b c c"
  "b c c"
  "a c c";
```

bu da olmaz a lardan dolayı yani 3 seçenek var

`a`’yı tek blok yapmalısın (ya üstte ya altta ya da dikey şekilde kesintisiz).

başkada yok a varsa ya a a a olacak yada a b b altında hemen ilk sırada a olacak öyle 2 satır aşagıda a yazarsan diğer taa üstte patlar abii nasıl birleştirecek adam grid-column grid-row'u dşün b'leri ezip de mi yapsın olmaaaz.



## <font color="#6495ed">Grid Column  veya Row farketmez span mantığı</font>

`grid-column: 1 / span 3;` ne demek?

### Örnekle anlatayım:

- 1 ile 2 arasındaki alan → 1. sütun
    
- 2 ile 3 arasındaki alan → 2. sütun
    
- 3 ile 4 arasındaki alan → 3. sütun
    
- 4 ile 5 arasındaki alan → 4. sütun
    
- 5 ile 6 arasındaki alan → 5. sütun

###### `grid-column: 1 / span 3;` ne demek?

- 1. çizgiden başla
- 3 sütun kadar genişlik kapla yani:
    
    - 1-2 arası (1. sütun)
        
    - 2-3 arası (2. sütun)
        
    - 3-4 arası (3. sütun)

Örnek:
```css
grid-row: 2 / span 4;
```

**Başlama çizgisi:** 2

- **span 4** → “Başladığın yerden itibaren toplam 4 satır kapsa”
- Sayalım:

```css
Başlangıç çizgisi 2  
↓  
1️⃣ 2. satır  
2️⃣ 3. satır  
3️⃣ 4. satır  
4️⃣ 5. satır
```

yani span demek start column'undan başlayarak span kaç ise o kadar git demek.

mesela diyerim start 2 span da 4

2.satır          1.sayma
3.satır         2.sayma
4.satır         3.sayma
5.satır        4.sayma tamam span 4 dü burada bitecek ve burdaki ıızgrala numaralı arkada belirlenip kaplanacak 

unutma span sütun bazlı sütun ne demek 5 kutu varsa 5 sutun demek ızgara gibi her kutunun sol ve sağ köşesine numara vermez.

```css
Grid çizgileri:  1    2    3    4    5
                 |    |    |    |    |
Sütunlar:          A    B    C    D
```

yani 1 span demek bir sütun demek bir sütun demek iki grid çizgisi arası demek.

Peki span neden çok kullanılır şimdi uzaktan baktığında columnları sayıyorsun ya 3 kolon var diyorsun direk span 3 dersin bu o demek olur ama ızgra çizgilerini sayamazsın span ondan daha iyi.

![[Pasted image 20250812202147.png]]

Mesela burada ben 9 dan başlarayak C ye kadar kaplamak istiyorum 

o zaman span ile sayabilirsin hadi sayalım 9 yazan buton 1 sütun 6 2 sütun 3 sütun c 4 sütün
demek o zaman 2 satırdan başlıyoruz dersek 2/span 4 bitti ama bunu ızgara ile böyle kafadan sayamazsın.