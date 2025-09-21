
Partial Kullanımı 

controllerdan gelen istek neticesinde render edilmeyecek ama bir view esnasısnda belirli bir noktda html.partialla hedef linki çağırıp render edilmesidir.


```csharp
 public IActionResult Index()
 {
     ViewBag.v = "serkan altunay ve mızık latunay";
     return View();
 }
```

Index sayfası

```csharp
@Html.Partial("~/Views/Home/deneme.cshtml")
```

deneme.cshtml partial sayfası yani

```cshtml
@ViewBag.v
```

Önemli bir viewdeki partiala ana viewden gönderdiğin viewbag gibi taşıma kontrölleri içindeki partialdada kullanılabiliyor.


<hr>


Home Controller
```csharp
   public class HomeController : Controller
    {
        public ActionResult Index()
        {
            Category category = new Category
            {
                CategoryId = 1,
                CategoryName = "Serkan"
            };

            return View(category);
        }
    }
```

index.cshtml
```csharp
@model WebApplication2.Models.Category

@Model.CategoryId
<br />
@Model.CategoryName

@Html.Partial("~/Views/Home/deneme.cshtml",new WebApplication2.Models.Product())
```

ikinci paremetresi model ister bizden ister controllırdan viewbag ile bir model taşı ister ise new ile buradan bir model oluştur.

deneme.cshtml

```csharp
@model WebApplication2.Models.Product

<h1>Burası partialdır</h1>
<br />

@Model.ProductId
<br />
@Model.ProductName
```

Eğer partiala herhangi bir model vermessen o varsayılan olarak partailın tanımlandıgı index sayfasınını modelini alır ondan category vermsende kabul eder viewbag index de tanımlansada dolaylı yoldan partialında geçer.


<hr>

Action Kullanımı :

```csharp
@Html.Action("deneme") //paremetre olarak model vermene gerek yok modeli kendisi arkada üretir ama partial olsaydı vermek zorundasın model üretmez o
```

action bizden bir model istemiyor çünkü zaten actionın görevi bir controller metoduna gidip onu çağırmak bunu yaptığı sırada onun modelini alıyor ama partial böyle yapmadığından yani aslında ben burada partiali controller tarafına yazmamalıydım çünkü partial sadece view bunu actiona yapmadan da bir shadred'ta tutulabilir bundan ona model veriyoruz ama action bir controller kullandıgından kendisi üretiyor modelini.

index de böyle cağırıldın bunu bunu yazman yeterli olurdu çünkü partial da bir model istiyor bizden render etmede modeli kendisi oluşturmuyor ama bu action kendisi yapıyor daha kullanışlı bu ama kötü olanı mvc5 de var bu net core de yok bundan biz net core'de ViewComponent yapısını kullanıyoruz aynı işlemi buda yapıyor.


Yani buraya bak topluyoruz artık

5. **Partial view çağırmak için `Html.Partial` yeterli, controller action’a gerek yok.**
    

- Çünkü partial view dosyasını direkt render eder, ekstra işlem yok.
    

6. **Ama dinamik veri veya işlem için partial controller action kullanılır.**

onemli:: partial ve viewComponent Konusu