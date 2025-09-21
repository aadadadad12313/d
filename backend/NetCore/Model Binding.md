

```csharp
using Microsoft.AspNetCore.Mvc;

namespace WebApplication1.Controllers
{
    public class ProductController : Controller
    {
        public IActionResult Index()
        {
            return View();
        }

        [HttpPost]
        public IActionResult Index(string txtText,string txtQuantity)
        {
            return View();
        }
    }
}
```

Burada Index aksi belirtilmediği sürece HttGet özelliği taşır.

Formdan gelen verilerin yakalanması için [HttpPost] kullanılması gerekir ve<font color="#6495ed"> Index actionı </font>
<font color="#6495ed">içinde paremetrede yakalanır.</font> veriler mvc mimarisi paremetlerin isimleri ile formdaki name attributi
ile aynı olanları binding eder.

```html
@addTagHelper *,Microsoft.AspNetCore.Mvc.TagHelpers

<form method="post" asp-action="Index" asp-controller="Product">

	<input type="text" name="txtText"/>
    <input type="text" name="txtQuantity"/>
    <button>Ekle</button>
</form>
```

<font color="#9bbb59">formun tetiklenmesi için formun içinde bir butonun olması ve asp-action ve asp-controller attributlerinin tanımlanması gerekir asp-controller zorunlu değil yani actionı verdin ya mvc mimarisi actionın oldugu controlleri bulabiliyormuş ama baktın mı okunması için tanımlanması her zaman iyidir.</font>


Şimdi burada dikkat etmen gerek şey formda type text olarak tutuyoruz tamamda model binding aşamasında action paremetresi eğer o form nesnesinin decimal gibi bir türe sahipse model binding esnasında dönüşüm oluyor bu sorun olmuyor yani o türe çevriliyor ondan biz bu zaman kadar text tanımladık hep decimal int gibi şeylere ama binding bunu yapmasaydı eğer patlardı kod patlamadığına göre hipotez doğru.

```csharp
 public IActionResult Index()
 {
     return View();
 }

 [HttpPost]
 public IActionResult Index(Product product)
 {
     return View();
 }
```

```csharp
namespace WebApplication1.Model
{
    public class Product
    {
        public string txtText { get; set; }
        public int txtQuantity { get; set; }
    }
}
```
 
 Burada ise ilgili propertyleri paremetle olarak vermek yerine Product classında tanımlayıp binding edebiliriz. işte bu kullanım aslında mvc mimarisinin ne kadar akıllı olduğunu gösteriyor.

<font color="#6495ed">Kısaca binding dediğimiz olay bir formun name'i ile action metodunun paremetresinin isimlerinin aynı olanı ile eşleşmesidir.</font>


> [!note] Nesne Oluşma
> Model Binding mekanizması formdan gelen verileri alır ve Product sınıfının özelliklerine eşleşiyorsa bir Product nesnesi oluşturur.

```csharp
 public IActionResult Index(Product product)
```

yani buradan aslında bir nesne oluşuyor arka da. nesne oluşmadan sen bir classadki propertyleri zaten kullanmazsın birinin bunu oluşturması gerekiyor. bunu ben yapamam çünkü binding'i mvc yapıyor o binding etme esnasında kendisi yapacak.


```csharp
@addTagHelper *,Microsoft.AspNetCore.Mvc.TagHelpers
@model WebApplication1.Model.Product

<form method="post" asp-action="Index" asp-controller="Product">

    <input type="text" asp-for="txtText"/>
    <input type="text" asp-for="txtQuantity" />
    <button>Ekle</button>

</form>
```

```csharp
  public IActionResult Index()
  {
      return View();
  }

  [HttpPost]
  public IActionResult Index(Product product)
  {
      return View();
  }
```

burada dikkat etmen gerek @model çünkü view null veri göndermedik ama model tanımlandı bu geçerli çünkü @model view null olsa bile tanımlanabilir ama view içine veri yollarsan o zaman @model kullanmasan kod patlar.

Asp-for ise name'den daha iyi çünkü ne yazıp yazmadığımız IntelliSense yapıyor yani otomatik tamamla çııkıyor bize rahatlık sağlıyor.

<font color="#ff0000">Html Helper ile kullanımı</font>

```csharp
@addTagHelper *,Microsoft.AspNetCore.Mvc.TagHelpers
@model WebApplication1.Model.Product

<form method="post" asp-action="Index" asp-controller="Product">
    @Html.TextBoxFor(x => x.txtText, "", new { placeholder = "Metin giriniz" })
    @Html.TextBoxFor(x => x.txtQuantity, "", new { placeholder = "Sayi giriniz" })
    <button>Ekle</button>
</form>
```


Önemli Burası

```csharp
using Microsoft.AspNetCore.Mvc;
using WebApplication1.Model;

namespace WebApplication1.Controllers
{
    public class ProductController : Controller
    {
        public IActionResult Index()
        {
            var product = new Product();
            return View(product);
        }

        [HttpPost]
        public IActionResult Index(Product product)
        {
            return View();
        }
    }
}
```

Şimdi get durumunda sayfa açılırken yani product sınıfından nesne oluşturup View'e yollarsan eğer binding varsa sayfada daha post olmadan o binding olan kısımlar bu modelin propertyleri olduğundan içinde hazır veri varsa mesela veri tabanından id 3 olanı çektin attın view'e hemen propertyler formdaki binding yerlerine uygulanır value gibi gözükür sayfada sonra ise post olduğunda hatılarsan yukarda post oldugundan nesne oluşturuluyor demiştim burada farklı oluyor yani biz get olarak producttı yolladığımız için getteki producttan post ediliyor postta nesne oluşturulmuyor gette oluşturuldu bir defa post kullandı devam etti.


onemli::@model veri yollamasakda kod çalışabilir

### <font color="#6495ed">Önemli</font>

Farkedersen yukardaki tüm kodlarda @Model var ama biz Index get aşamasında model de veri yok null verip hata fırlatmadı neden peki çünkü @model null olabilir return view() srıasında veri yollamadıysan @model tanımladın mesela ama view'de veri yok sorun yok çalışır render edilir yani ama gidipde o sayfada @model.title gibi @modeli doğurdan kullanmaya çalışırsan  o zaman null hatası alırısn tek birşey de hata almazın o da asp-for gibi binding aşamasında çünkü bu veriye erişmek iştemez o anda..

yani @model tanımlayabilirsin cshtml de ama doğrudan view null ise kullanmazsın tek binding de kullabilirsin @model zaten binding olması için gerekli hata vermez  yani bindingde asp-for erişmek istemez dataya o anda.


#### <font color="#00b050">Örnek</font>

```csharp
    public async Task<IActionResult> UpdateCategory(UpdateCategoryDto updateCategoryDto)
    {
      return view();
    }
```

mesela bu örnek model binding olacağı zaman şu olacak

Tarayıcı veya client bir JSON gönderir:
```json
{
  "id": 5,
  "name": "Elektronik",
  "description": "Kategori açıklaması"
}
```

ASP.NET Core framework arka planda şunları yapar:

Senin verdiğin action metodundaki class türünü alır o türü Deserialize eder yani new yapıp sana bir instance verecek sende o instanceyi action metodunda referans ile karşılayıp kullanacan.

```csharp

// HTTP body JSON alınıyor
string body = await new StreamReader(HttpContext.Request.Body).ReadToEndAsync();

// JSON parse edip DTO instance yaratılıyor
UpdateCategoryDto updateCategoryDto = JsonSerializer.Deserialize<UpdateCategoryDto>(body);

// Metod çağrısı
await UpdateCategory(updateCategoryDto);

```

yani bu yukarıdaki kod Deserialize sonucunda şunu oluşturuyor 
```csharp
UpdateCategoryDto a=new UpdateCategoryDto()
{
	 Id = 5,
    Name = "Elektronik",
    Description = "Kategori açıklaması"
};
```

bir class oldu json dan işte bunu artık await UpdateCategory(updateCategoryDto); bu kod ile ilgil action metodu yani bu updateCategoryDto referansını veriyor ve biz de 

```csharp
    public async Task<IActionResult> UpdateCategory(UpdateCategoryDto updateCategoryDto)
     {
      return view();
    }
```

bizde referans tanımlıyoruz ve ne oluyor gidiyor net core'nin verdiği nesne referansı artık biz action metodunda farklı bir referans olara kullanıyoruz ve bu referansta yapacağımız bir değişiklik net core'nin verdiği referansdaki proeprtyleride değiştirecek gibi oluyor.