
```css
.calculator {
    color: white; /* Bu sadece normal text elementleri için çalışır */
}
```

bu örnekte mesela color white dedik bu kapsayıcının içinde h1 veya p  varsa onlara inherit edilir kalıtımı verir ama buton input gibi form etiketleri varsa onlara etki etmez doğrudan onları seçmen gerekir nedeni :

- Ancak `<button>`, `<input>`, `<select>` gibi form elementleri tarayıcı tarafından **varsayılan stil** ile gelir ve bu renk tanımı kendi içinde yeniden belirlenmiş olabilir.
    
- Bu nedenle `.calculator { color: white; }` verdiğinde butonlar tarayıcı stilini kullanmaya devam eder.