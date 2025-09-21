
border box mantığı aslında şu

bir kutuya padding verildiğinde o kutu genişliği 300px ise 50 verdiysen padding normal şartlarda border-box kullanmadıgında bu box genişliği 300 + 50 den 350px olur. bu hiç de iyi bir durum olmaz çünkü sürekli site tasarlarken düşünmen gerekir kaç vereceğini ama border-box verdiginde buna ihtiyaç kalmaz 300px ise kutu 50 padding verdiginde 300px kalır box padding içeride genişler yani padding gelir ama büyümez


```css
*{
 box-sizing: content-box;
}//content baz varsayılan değerdir her sayfada bu değer işte padding verildiginde genişleme yapar yukardaki sorun yani

*{
 box-sizing: border-box;
}
```

### 📊 Karşılaştırma:

#### 1. `content-box` (varsayılan davranış)

```css
.box {
  width: 200px;
  padding: 20px;
  border: 5px solid black;
  box-sizing: content-box;
}
```

Toplam genişlik: `200 (content) + 40 (padding) + 10 (border)` = **250px**


#### 2. `border-box`

```css
.box {
  width: 200px;
  padding: 20px;
  border: 5px solid black;
  box-sizing: border-box;
}
```

Toplam genişlik = **200px**  
→ Bu 200 pikselin içine hem padding hem border dahildir.


box sizing sadece paddingler içindir onlar hesaplamak için kullanılır buna margin dahil degildir