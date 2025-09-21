
Nth Child kullanımı Nth Of Type kullanımı arasındaki farkı inceleyelim bunlar aslında aynı gibi görünsede aralarında ufak bir fark var.

#### 1️⃣ `:nth-child(n)`

- **Mantık:** Elemanın **ebeveyni içindeki sırasına** bakar.
    
- Sıra belirlenirken **etiket türüne bakmaz**, sadece _kaçıncı çocuk_ olduğuna bakar.
    
- Yani `div` de olsa, `p` de olsa, _tüm kardeşler_ sayılır.


```html
    <div class="bars">
  <p>1. paragraf</p>
  <span>1. span</span>
  <p>2. paragraf</p>
</div>
```

```css
.bars p:nth-child(2){
   color:red;
}
```

diyoruz ki burada bars classını içindeki 2. ögesini bul türü p ise uygula ama p degil span ise geçersizdir.


dikkat et nth child da sen bar diyorsun ama o arakda hambuger classına bakıyor yani hamurger classında ne kadar chiled varsa hepsini 1.chield ikinci child diye sıralıyor senin verdiğin 2 numarısına erişenin türü eğer bars ise örnekeki gibi ona göre uygular uygulamaz ona bakıyor.

aslında 

```html
.hamburger .bar:nth-child(2) {
  animation: 1s fadebar2 infinite;
}
```

nth child kullanırkne kapsayıcısını belirtmek en doğrusu herkes böyle yapıyor sende yap .bar'ın 2. sıradakini mi istiyorsun kapsayıcısını belirt.


ama nth of type daha kolay 

```html
    <div class="bars">
      <p>1. paragraf</p>
      <span>1. span</span>
      <p>2. paragraf</p>
    </div>
```

```css
.bars p:nth-of-type(2){
  color:red;
}
```

çünkü nth of type da direk aklından ne geçiyorsa onu şöylüyorsun diyorsun ki şu kapsayıının bilmem şurasıda ben p olan işte 5. p yi istiyorum yada 2.li yi gibi direk yazıyorsun oluyor ne kontrol var ne şey. ama nth child da 2 dersen ikinci öğeye bakar o öğe eğer tipi orada yazdığın ile uyusuyorsa tamam uygularım ama uyuşmuyorsa span filansa türü patlar orası ama nth of type daha kolay.


yani

- `.bar:nth-child(2)` → “2. sırada olan `.bar`”
- `.bar:nth-of-type(2)` → “`.bar` olanlar arasında 2. sırada olan” (etiket türüne göre sayar)
- 
nth childe sayma yapmadıgında 2.sıraya hemen yapıştırıyor cevabı bundan da patlıyor insan bir bakar orada bar varmı diye işte ondan nth of type kullan.


Önemli

After Ve Before Psoude Elementlerinde block türü varsayılan inline dır.

yani sen div::before dersen div içinde oluşacak bir etiket inline olacak div block diğe o da block olmuyor ondan inline olacagından dikkat et margin ve padding de sorunlar yaşayabilirsin.