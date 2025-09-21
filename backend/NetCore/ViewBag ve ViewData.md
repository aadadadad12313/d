
##### <font color="#6495ed">Model Bazlı Veri Gönderimi</font>

 ```csharp
  public IActionResult Index()
 {
     
     var product=new List<Product>
     {
         new Product{id=1, Name="Serkan"},
         new Product{id=2, Name="Selin"},
         new Product{id=3, Name="Mehmet"},
     };

     return View(product);
 }
```

Sonra bunu view sayfasında model karşılanacak türü belirtmemiz gerekiyor

```csharp
@model List<WebApplication1.Model.Product>
```
Model gelen dataya göre burdaki view'i ayarlıyor.


> [!note] Model-nodel arasındaki fark
> model gönderilen tipi yakalarken Model yakalanan tipin verileri yakalar yani **eri nesnesine erişmek için kullandığın nesnedir**.


##### <font color="#6495ed">Veri Taşıma Kontrolleri</font>

###### <font color="#92d050">1 Viewbag</font>

View'e gönderilecek datayı dynamic şekilde tanımlanan bir değişkenle taşımamızı sağlayan bir veri taşıma kontrolüdür.


```csharp
 public IActionResult Index()
 {
     
     var products=new List<Product>
     {
         new Product{id=1, Name="Serkan"},
         new Product{id=2, Name="Selin"},
         new Product{id=3, Name="Mehmet"},
     };

     ViewBag.product = products;

     return View();
 }
```

```csharp
<ul>
    @foreach (var product in ViewBag.product as List<WebApplication1.Model.Product>)
    {
        <li>@product.id</li>
        <li>@product.Name</li>
    }
</ul>
```

Viewbag dynamic oldugundan çalışma anında (runtime)'da belirlenir. yazıdında otomatik tamamlama gelmez bundan biz türünü Derleme Zamanı (Compile Time)'da  belirledik.

Viewbag viewDatanın aslında daha indexer değil de daha sade bir şekilde yazmamız için tasarlandı.

`ViewData` → dictionary tabanlı (key, value)

- `ViewBag` → `ViewData` üzerine kurulmuş, **daha okunabilir ve kolay bir yazım** sağlar
    
- **İkisi aynı yeri paylaşır** (farklı koleksiyonlar değil)

bundan dolayı sen mesela bir viewbag de bir key verdin o key viewDatada da tanımlarsan aynı yeri kullanıyorlar ya ondan viewdatanın çıktısıda viewbag ile aynı olur çünkü sen viewbag tanımladııgn an run time da bu viewDataya dönüşüyor arkada. 

ya da tam tersi viewDataya sen key ver viewbag de bişey tanımlama ama cshtml de viewbag key'inin yaz çalışacak. 

> [!note] Örnek
> Hatırlarsan UdemyCarbook da StatisticsController da biz generic yapı yapmıştık consumelere işte o zaman controllerda ViewData kullandık ama cshtml view'inde biz daha önceden viewBag ile yapmıştık hiç viewBagleri değiştirmemiştim çalışmıştı işte sebebi bu viewBag demek ViewData aslında sadece daha okunaklı bir biçimi aynı yerde tutuluyorlar.


###### <font color="#92d050">2 ViewData</font>

ViewBag'de olduğu gibi actiondaki datayı view'e taşımamızı sağlayan bir kontroldür. 

object türlüdür eğer view de sen kullanmak istersen hesaplama filan yapacaksan unboxing edecen ama dersen ben işte veri taşıcam veriyi işlemlere tabii tutmayacam o zaman unboxing'e ihtiyaç yok çünkü 

 Razor, object türündeki veriyi doğrudan string'e çevirebilir. Bu yüzden:
 unboxing gerekmez.

```csharp
   public IActionResult Index()
   {
      ViewBag.soyisim = 100.50;
      ViewData["isim"] = 19;
      TempData["yas"] = 19;
      return View();
    }
```

```csharp
@ViewBag.soyisim
@ViewData["isim"]
@TempData["yas"]
```

yani

Unboxing sadece **değeri kullanmak** istediğinizde gerekli

yani biz yukarda gerekmedi unboxing çünkü

Siz sadece:
1. Değerleri atadınız (Boxing oldu)
2. View'a gönderdiniz
3. View'da yazdırdınız (Razor otomatik ToString() yaptı)

Hiçbir yerde object'ten geri orijinal türe dönüştürme yapmadınız, bu yüzden unboxing gerekmedi!


<hr>


```csharp
ViewData["product"] = products;
```

```csharp
<ul>
    @foreach (var product in ViewData["product"]  as List<WebApplication1.Model.Product>)
    {
        <li>@product.id</li>
        <li>@product.Name</li>
    }
</ul>
```

Viewdata boxing'e tabidir ondan view'da unboxing edilmelidir viewbag dynamic'ti ama viewdata böyle değil ondan as veya  cast ile unboing yapılmalıdır.


###### <font color="#92d050">3 Tempdata</font>

ViewData'da olduğu gibi actiondaki datayı view'e taşımamızı sağlayan bir kontroldür. yine viewData gibi object türlüdür. 

şunu unutöa Tempdata tamam bir action'a sen data yolluyorsun ama bu demek değil ki kendin Tempdatayı tanımladığın actionda da kullanamıyorsun demek değil.

yani sen hem tanımladıgın tempdatanın cshtml'inde hem de RedirectoAction metodu ile yönlendirdiğin action da bu değeri kullanabilirsin.


```csharp
TempData["product"] = products;
```

tempdata cookie kullanıyor verileri bunun üzerinden taşır. 
tempdata actionlar arası veri taşıma kontrollüdür.


###### <font color="#92d050">Kısaca Üç yöntem de aynen kullanılabiliyor kodda sadece isiimlerini değiştir tempdate yazacaksan viewDatayı sil bunu yaz bu kadar yani kod üzerinde değişiklik bile yapmana gerek yok. yani ilk viewbag kodu güzei o hepsine geçerli isimlerini değiştir koy bitti.</font>

dersen nasıl gerek yok mesela viewBag dynamic diğer viewdata ve tempadate gibi object değil ondan viewbag cast olmaması lazım dersen işte burada iki kullanımın  var

ya gideceksin viewbag'i cast edersin inteliesence gelir
ya etmessin normal de etmessin genellikle inteliesence'siz kullanırsın bu kadar sana kalmışs

onemli::viewbag,viewdata,tempdata

###### 1️⃣ `as` operatörü ne yapar?

- `as` operatörü **sadece referans tipler (class, string, interface,List)** için çalışır.
    
- Value type (struct, int, double, bool, DateTime vb.) için **çalışmaz**. bunlar için cast operatörü gereklidir.

```csharp
   public IActionResult Index()
   {
        var personeller = new List<Product>
        {
            new Product {Name="serkan",soyisim="altunay"},
            new Product {Name="mızık",soyisim="altunay"},
        };

        ViewData["Personeller"] = personeller;
        return View();
   }
```

```csharp

@foreach (var item in ViewData["Personeller"] as List<Product>)
{
    <h1>@item.Name</h1>
    <h1>@item.soyisim</h1>
}
```

as operatörü sadece listeler gibi referans tiplerinde geçerli yan int float double filan olsaydı List yerine patlardı kod onun için normal cast yapacaksın.

yani
```csharp
@foreach (var item in (List<Product>) ViewData["Personeller"])
{
    <h1>@item.Name</h1>
    <h1>@item.soyisim</h1>
}
```

cast işlemi (   ) demek zaten kolayca yapabiliyorsun.



###### <font color="#00b050">Şimdi Çok Önemli Kritikleri Deneyelim </font>

1. kullanım

```csharp
public IActionResult Index()
{
    var products = new List<dynamic>
    {
       new {id=1, Name="Serkan", soyisim="Altunay"},
       new {id=2, Name="Selin", soyisim="Yılmaz"},
       new {id=3, Name="Mehmet", soyisim="Demir"},
    };
    ViewBag.v = products;
    return View();
}
```

```csharp
@foreach (var item in ViewBag.v)
{
    <h1>@item.Name</h1>
    <h1>@item.soyisim</h1>
}
```

dynamic olduğundan tür viewbag de dynamic hiçbir sorun olmaz anonim tip  başarılı bir şekilde oldu.

2. kullanım

 ```csharp
      public IActionResult Index()
     {
         var products = new List<object>
         {
            new {id=1, Name="Serkan", soyisim="Altunay"},
            new {id=2, Name="Selin", soyisim="Yılmaz"},
            new {id=3, Name="Mehmet", soyisim="Demir"},
         };
         ViewBag.v = products;
         return View();
     }
 ```

Object varsa cast gerekli ondan bir türe cast lazım Türü anonim olduğundan tek tür kaldı objectti zaten tanımlarken kullandık ondan dynamic türe cast yapacan ama viewbag varsa gerek yok çünkü viewbag dynamic ya cast otomatik oluyor.

aslında mantık şu object compile time da çözülüyor ya item.Name diyor patlıyor ama sen cast ile dynamic yaparsan bu run time'da çözülür o zamanda tipler zaten belli olmuş oluyor olay bundan ibaret.

```csharp
@foreach (var item in ViewBag.v)
{
    <h1>@item.Name</h1>
    <h1>@item.soyisim</h1>
}
```

yada şunu eklersin

```csharp
as List<dynamic> 
```

diyeceksin viewbag'e gerek yok bu kod viewData da var ama.


3.kullanım
1️⃣ Anonim tipler compile-time’da yaratılır ama tipi **derleme zamanı bilinir**, property’ler ise runtime’da erişilir

Object önce compole time da şunu yapar bu tür

```csharp
object o = new { Name = "Serkan" };
Console.WriteLine(o.Name); // ❌ HATA
```

- `o` tipi **object** olarak bilinir.
    
- Derleyici sadece **object’in bildiği şeyleri** görür: `ToString()`, `GetHashCode()`, `Equals()`.
    
- Derleme zamanı, `o.Name` diye bir property olmadığı için **hata verir**.
    

> Yani compile-time’da object: **sadece object’in üyelerini tanır**, anonim tipin property’lerini **bilmez**.


## 2️⃣ Runtime (Çalışma Zamanı)

```csharp
object o = new { Name = "Serkan" };
dynamic d = o;
Console.WriteLine(d.Name); // ✅ ÇALIŞIR
```

- `o` runtime’da hâlâ **anonim tip nesnesi**dir ve property’leri vardır.
    
- `dynamic` ile cast yaptığımızda derleyici kontrol etmez, runtime’da bakar:
    
    1. `d`’nin gerçek tipi nedir? → anonim tip
        
    2. Bu tipin `Name` property’si var mı? → var → değer döner
        

> Yani runtime’da object aslında gerçek tipine sahiptir, sadece compile-time bunu göremez.

yani run time demek atama demek biz atamayı sabahtan beri compile time da yapıyoruz ondan patlıyoruz bunun bir yolu yok ya object kullacan dynamic 'e çevirecen ya dynamic bir tip kullacan yani viewbag 


View data anoniim type

```csharp
    public IActionResult Index()
    {
        var products = new List<object>
        {
           new {id=1, Name="Serkan", soyisim="Altunay"},
           new {id=2, Name="Selin", soyisim="Yılmaz"},
           new {id=3, Name="Mehmet", soyisim="Demir"},
        };
        ViewData["v"] = products;
        return View();
    }
```

```csharp
@foreach (var item in ViewData["v"] as List<dynamic>)
{
    <h1>@item.Name</h1>
    <h1>@item.soyisim</h1>
}
```

çalışır gayet. 

```csharp
    public IActionResult Index()
    {
        var products = new List<dynamic>
        {
           new {id=1, Name="Serkan", soyisim="Altunay"},
           new {id=2, Name="Selin", soyisim="Yılmaz"},
           new {id=3, Name="Mehmet", soyisim="Demir"},
        };
        ViewData["v"] = products;
        return View();
    }
```

```csharp
@foreach (var item in ViewData["v"] as List<dynamic>)
{
    <h1>@item.Name</h1>
    <h1>@item.soyisim</h1>
}
```
bu da olur dynamic liste viewData obje ya dynamic'e de cast ettik olay bitti.



```csharp

        public IActionResult Index()
        {
            var products = new List<dynamic>
            {
               new {id=1, Name="Serkan", soyisim="Altunay"},
               new {id=2, Name="Selin", soyisim="Yılmaz"},
               new {id=3, Name="Mehmet", soyisim="Demir"},
            };
            TempData["v"] = products;
            return View();
        }
```

```csharp
@foreach (var item in TempData["v"] as List<dynamic>)
{
    <h1>@item.Name</h1>
    <h1>@item.soyisim</h1>
}
```

bu da olur bak burada Liste ister obje olsun ister dynamic farketmez önemli olan Tempdata türü obje ya bunu foreach hemen erişmesin degerlere compole time da run time bırakmak için dynamic'e çevir Tempdatayı olay bu senn işin controller da değil tam foreach viewDatasını dynamic'e çevirip run time'da belirlensin ki atamalarımınz o an olacak biz sabajtam beri atama sırasını ekren yaptırurouz object ile adam da ben de yokki sana vereyim diyor patlıyor kod.


bak object skınıtıdıır unboxing cast lazım ama sen anonim sınıf varsa hapı yuttun tek yöntem dynamic

ama dynamic varsa zaten sorun yok kullan run time da belli olacak ıvırı zıvırı



Şunuda not al tempData yukarda kullandın Anonim olarak düşünmede normal bir class olarak düşün eğer tempData içinde liste class fikan vrarsa ve bu redirectoaction metodu ile diğertarafa gidecekse senin json serialize işlemini yapman lazım eğer ben yola-lmayacam action'a kendi tanımladıgım action da kullanacam dersen tama gerk yok kullnırısn direk.

hata veriyor çünkü tempdate içinde bir liste var kompeks bir değer var dönüştürme yapman gerkiyor


yani kodları bunlarda tempdate serialize

```csharp

        public IActionResult Index()
        {
            var products = new List<dynamic>
            {
               new {id=1, Name="Serkan", soyisim="Altunay"},
               new {id=2, Name="Selin", soyisim="Yılmaz"},
               new {id=3, Name="Mehmet", soyisim="Demir"},
            };
            TempData["v"] = products;
           return RedirectToAction("Privacy");
        }

```


```csharp
      public IActionResult Privacy()
      {
          object value=TempData["v"];
          return View();
      }

```
return view yoksa rediretoaction var ondan senin serailize yapman lazım başka türlü olmaz hata verir çünkü tempdate cookie tutuyor ya string iştiyor sonra verdiign actionda deseralize yapacan filan.

