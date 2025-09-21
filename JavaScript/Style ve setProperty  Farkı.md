

Style

```js
h2.style.color = "red";
```

style'de sadece propertye value verirsin ama onun içinde !important filan diyemezsin js de böyle sadece value istiyor red !important olmaz ama bazı durumlarda hele de bootstrap de bu patlar çünkü !important olmadan olmaz kod çalışmaz ondan setProperty metodunun üçüncü paremetresi important vermek için tasarlanmış oraya ver onu rahatına bak.

```js
h2.style.setProperty("color", "red", "important");
```

