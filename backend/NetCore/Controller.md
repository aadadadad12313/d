
Controller oluşturabilmemiz için önce startup dosyasına servisimizi eklememiz gerekir 

ConfigureServices

```csharp
 services.AddControllersWithViews();
```


Controllers Adında bir klasör oluşturmamız lazım mecubri değil aynı adla olması ama genellikle tercih edilir.

```csharp
using Microsoft.AspNetCore.Mvc;

namespace WebApplication1.Controllers
{
    public class HomeController : Controller
    {
        public IActionResult Index()
        {
            return View();
        }
    }
}
```

Bir sınıfı request alabilir ve response döndürebilir yani controller yapabilmek için o sınıfı Controller class'ından türetmemiz gerekmektedir.


Controller sınıfları içierisinde istekleri karşılayan metodlara action metod denir.

Action metod controllera gelen isteiğ karşılayan ve gerekli operasyonları gerçekleştiren metodlardır.

ASP.NET Core MVC'deki controller'lar, **middleware** ve **routing** mekanizmaları aracılığıyla otomatik olarak yönetilir. Bu nedenle controller nesnelerini manuel olarak oluşturmanıza gerek yoktur. Controller, gelen HTTP isteklerine karşılık olarak çalışır ve sistem, gerekli bağımlılıkları (services, diğer class'lar) otomatik olarak sağlar.

yani biz oluştudugumuz controllardan bir nesne üretmiyoruz mvc mimarasi bunu kendisi yapıyor startup'a eklediğimiz servis ile.

onemli:: httget ve httpost attribütleri,attribüt

Controller'lar controllerdan kalıtım alanlar yani api'yi kastedmiyorum controllerlar httget ve post isteği alabilir sadece yani put isteği alamaz bunu alsa alsa api controller alabilir neden alamazlar dersen eğer html form yüzünden html formlar da iki seçenek var zaten ya get ya post ondan controllerda da yok.

csharp

```csharp
public class HomeController : Controller
{
    [HttpGet]   ✅ Form ile gelebilir
    [HttpPost]  ✅ Form ile gelebilir  
    [HttpPut]   ❌ Form ile GELEMEz (JS gerekir)
}

// API Controller - HTTP client için  
[ApiController]
public class ApiController : ControllerBase
{
    [HttpGet]    ✅
    [HttpPost]   ✅
    [HttpPut]    ✅ Tüm HTTP verb'ler desteklenir
    [HttpDelete] ✅
}
```
