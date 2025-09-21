bir HTML öğesinin **içeriği kutusundan (container) taştığında** nasıl davranacağını kontrol eder.

| Değer                    | Anlamı                                            |
| ------------------------ | ------------------------------------------------- |
| `visible` _(varsayılan)_ | Taşan içerik görünür (taşar dışarı)               |
| `hidden`                 | Taşan içerik **gizlenir**, görünmez olur          |
| `scroll`                 | Taşma olsa da olmasa da **kaydırma çubuğu çıkar** |
| `auto`                   | Taşma varsa kaydırma çubuğu çıkar, yoksa çıkarmaz |

`overflow: visible;` (Varsayılan)


```css
.box {
  width: 200px;
  height: 100px;
  overflow: hidden;
}
```

Tasan içerik gizlenir görünmez

overflow: scroll

```css
.box {
  width: 200px;
  height: 100px;
  overflow: scroll;
}
```

İçerik taşsa da taşmasa da **hem yatay hem dikey kaydırma çubuğu** görünür.


