
ASP.NET Core'da validation (doğrulama), kullanıcıdan gelen verilerin belirli kurallara uygun olup olmadığını kontrol etme sürecidir. Bu, hem güvenlik hem de kullanıcı deneyimi açısından kritik bir adımdır.


<font color="#4bacc6">ASP.NET Core'da Validation Türleri</font>

| Tür              | Açıklama                                                                                            |
| ---------------- | --------------------------------------------------------------------------------------------------- |
| Data Annotation  | Model sınıfı üzerinde `[Required]`, `[StringLength]`, `[EmailAddress]` gibi öznitelikler kullanılır |
| FluentValidation | Daha esnek ve okunabilir kurallar tanımlamak için kullanılan harici bir kütüphane                   |
|                  |                                                                                                     |

##### <font color="#4bacc6">Data Annotation</font>

```csharp
using System.ComponentModel.DataAnnotations;

namespace WebApplication1.Model
{
    public class Product
    {
        [Required(ErrorMessage ="Lütfen Product Name'i giriniz.")]
        [StringLength(20,ErrorMessage ="Lütfen Product Name'i en fazla 20              karakter giriniz.")]
        public string Name { get; set; }

        [Required(ErrorMessage = "Lütfen Product Description'i giriniz.")]
        [StringLength(100, ErrorMessage = "Lütfen Product Description'i en             fazla 100 karakter giriniz.")]
        public string Description { get; set; }
    }
}
```

Required Attribütü tanımlanan yer zorunludur girilmesi.
StringLength Attribütü metnin uzunluğunu belirler.


```csharp
using Microsoft.AspNetCore.Mvc;
using System.Linq;
using WebApplication1.Model;

namespace WebApplication1.Controllers
{
    public class ProductController : Controller
    {
        public IActionResult Index()
        {
            return View();
        }

        [HttpPost]
        public IActionResult Index(Product model)
        {
            if (!ModelState.IsValid)
            {
                return View(model);
            }

            return View();
        }
    }
}
```

ModelState validation için özel bir propertydir. validation işlemi bittikten sonraki durumu öğreniriz hangi propertyler false döndü hangiler true döndü gibi.

IsValid bir hata yoksa true döndülür varsa hata false döndülür.

şimdi yukardaki kodda binding edildi model içinde propertyler doldu sonra if koşuluna girdik diyelim mesela bir tane şartı sağlamadık girdik ve uyan propertyler'i sayfaya tekrar gönderdik böylece her hatada doğru olan propertyleri tekrardan girmemize gerek kalmaycak hatalı olanlar hemen doldurup sorunu ortadan kaldıracağız.

```html
@addTagHelper *,Microsoft.AspNetCore.Mvc.TagHelpers
@model WebApplication1.Model.Product

<form asp-action="Index" asp-controller="Product" method="post">

    <input type="text" asp-for="Name" placeholder="ismini giriniz: " />
    <span asp-validation-for="Name" style="color:red; display:block;"></span>

    <input type="text" asp-for="Description" placeholder="Fiyatını Giriniz: " />
    <span asp-validation-for="Description" style="color:red; display:block;"></span>

    <button>Gönder</button>

</form>
```


Burası Önemli

```html
<span asp-validation-for="Name" style="color:red; display:block;"></span>
```

Validation hata mesajı span etiketi olarak tanımlıyoruz. böyle yapmışlar validation'u yapanlar span etkiketine atribute filan vermişler buna uygulamışar p ye asp-validation-for dersen olmuyor span'a olacak yani. bundan span'a css yazacaksın güzelleştirmek için.

onemli::post olduktan sonra binding edilen form içinde hayla veriler kalıyor return view dahii olsa neden?

onemli::Data Annotations

Önemli Burası 

post olduktan sonra binding edilen form içinde hayla veriler kalıyor return view dahii olsa neden?

Meslea bir form var elimizde 

```html
<form asp-action="Index" asp-controller="Product" method="post">
    @Html.TextBoxFor(x => x.Name)
    @Html.TextBoxFor(x => x.Description)
</form>
```

```csharp
[HttpPost]
public IActionResult Index(Product model)
{
    // Model geliyor burada
    return View(); // dikkat et: model gönderilmiyor
}
```

Yani :
##### <font color="#4bacc6">🧠 2. ASP.NET Core’un “ModelState” Akıllılığı:</font>

ASP.NET Core’un `ModelState` adında bir yapısı var. Bu yapı, gelen form verilerini **otomatik olarak tutar** ve `View()` içinde `@Html.TextBoxFor(...)` gibi helper’lar bu `ModelState` verilerine öncelik verir.

**Yani:**

- Sen `return View();` desen bile (yani boş bir view dönsen bile)
    
- ASP.NET Core otomatik olarak formdan gelen `Name` ve `Description` alanlarını **ModelState** içinde saklar.
    
- View tarafında `@Html.TextBoxFor(...)` çalışırken önce Model'e bakmaz, **ModelState**’e bakar ve oradan gelen değeri yazmaya devam eder.

Yani formda biz asp-for veya html helper gibi binding helperları varsa post olsa bile inputların value değerleri kalır işter controller tarafında return view de ister view içine binding sonrası nesneni ver aynı oluyor.


Şimdi Data Annotations'un daha temiz daha okunaklı yazımını yapacağız.

##### <font color="#4bacc6">ModelMetadataType</font>

ModelMetadataType` attribute'u, ASP.NET Core uygulamalarında validation gibi metadata bilgilerini doğrudan model sınıfına yazmak yerine, bu sorumluluğu ayrı bir sınıfa devretmek için kullanılır.

```csharp
using Microsoft.AspNetCore.Mvc;
using System.ComponentModel.DataAnnotations;
using WebApplication1.ModelMetaDataType;

namespace WebApplication1.Model
{
    [ModelMetadataType(typeof(ProductMetaData))]
    public class Product
    {
        public string Name { get; set; }
        public string Description { get; set; }
    }
}
```

```csharp
using System.ComponentModel.DataAnnotations;

namespace WebApplication1.ModelMetaDataType
{
    public class ProductMetaData
    {
        [Required(ErrorMessage = "Lütfen Product Name'i giriniz.")]
        [StringLength(20, ErrorMessage = "Lütfen Product Name'i en fazla 20 karakter giriniz.")]
        public string Name { get; set; }

        [Required(ErrorMessage = "Lütfen Product Description'i giriniz.")]
        [StringLength(100, ErrorMessage = "Lütfen Product Description'i en fazla 100 karakter giriniz.")]
        public string Description { get; set; }
    }
}
```

Olay bu kadar ilgli validations propertylerini alacaksın bir sınıfta vereceksin ViewModel gibi sonra ilgili entity'de bunu ModelMetadataType ile çağıracaksın.


##### <font color="#4bacc6"> DataAnnotations ile Client Side İşlem Yapma</font>

```csharp
   public class RegisterViewModel
   {
       [Required(ErrorMessage = "Email boş olamaz")]
       [EmailAddress(ErrorMessage = "Geçerli email giriniz")]
       public string Email { get; set; }

       [Required(ErrorMessage = "Şifre boş olamaz")]
       [MinLength(6, ErrorMessage = "Şifre en az 6 karakter olmalı")]
       public string Password { get; set; }
   }
```


```csharp
 [HttpPost]
 public IActionResult Index(RegisterViewModel m)
 {
     if (!ModelState.IsValid)
     {
         return View(m);

     }
     return View();
 }
```

```csharp
@model WebApplication1.Models.RegisterViewModel
    
<form asp-action="Index">
    <input asp-for="Email" />
    <span asp-validation-for="Email"></span>

    <input asp-for="Password" />
    <span asp-validation-for="Password"></span>

    <button type="submit">Kaydol</button>
</form>

<script src="~/lib/jquery/dist/jquery.min.js"></script>
<script src="~/lib/jquery-validation/dist/jquery.validate.min.js"></script>
<script src="~/lib/jquery-validation-unobtrusive/jquery.validate.unobtrusive.min.js"></script>
```

Fluenttekinin aynısı.
