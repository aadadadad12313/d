

Biz bir uzunca kodda kullanılan navbar,header,main,footer gibi bölümleri tek bir dosyada bir bütün olarak tutarsan kod bakımı ve kod hatası olduğunda bütün bölümler etkilenir.

işte bu durumdan kurtulmak için bütünü parçalara bölüp parçalardan bütün oluşturma işlemi yaparak bir Modüler Tasarım Yapılanması sağlıyoruz.


Şimdi Net Core de modüler tasarım yapılandırılmasını kullanmak için iki yol var biri partial view diğer viewComponent yapılandırmasıdır.


## <font color="#6495ed">PartialView Nedir?</font>

Modüler tasarimda herbir modulun ayri bir .cshtml/parca olarak tasarlanmasini ve İhtiyac dogrultu-sunda İlgili parcanin cagrilip kullanilmasini saglayan bir yontemdir...



onemli:: partial ve viewComponent Konusu


Partial View kullanımı şimdi iki şekilde var bir controllerda partialViewResult tanıımlarsın bu ile ilgil o paritalda controller bazlı işlemler requestler yapaiblirsin ama dersen ben request filan yapmayacam sadece html çıktısı lazım o zaman controllerda işin yok direk bir klasör oluştur partial klasörü sonra içine ilgili bir view dosyası ona html kodlarını ver sonra ilgili view'de çağır

yani request ihtiyacınımı var hem de partial olacak controller gerekli ama partial ihtiyacın var controllera ihtiyacın yok controller gerekli değil.

Sidebar.cshtml

```html
<div class="sidebar">
    <a href="#">Dashboard</a>
    <a href="#">Profil</a>
    <a href="#">Mesajlar</a>
    <a href="#">Ayarlar</a>
</div>
```

Navbar.cshtml

```html
<header>
    <h1>Logo veya Başlık</h1>
</header>

<nav>
    <a href="#">Ana Sayfa</a>
    <a href="#">Hakkında</a>
    <a href="#">Hizmetler</a>
    <a href="#">İletişim</a>
</nav>
```

S.cshtml

```csharp
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Header ve Navbar</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: Arial, sans-serif;
        }

        header {
            background-color: #222;
            color: white;
            padding: 15px 20px;
            text-align: center;
        }

        nav {
            background-color: #444;
            display: flex;
            justify-content: center;
        }

            nav a {
                color: white;
                padding: 14px 20px;
                text-decoration: none;
                transition: background 0.3s;
            }

                nav a:hover {
                    background-color: #666;
                }

        .sidebar {
            width: 200px;
            height: 100vh;
            background-color: #333;
            color: white;
            padding-top: 20px;
        }

            .sidebar a {
                display: block;
                color: white;
                padding: 12px 20px;
                text-decoration: none;
                transition: background 0.3s;
            }

                .sidebar a:hover {
                    background-color: #555;
                }

        .content {
            flex: 1;
            padding: 20px;
        }
    </style>
</head>
<body>
    @Html.Partial("~/Views/Partial/_NavbarPartial.cshtml")

    @Html.Partial("~/Views/Partial/_SidebarPartial.cshtml")
    <div class="content">
       @RenderBody()
    </div>

</body>
</html>
```

Burada layout dosyamızın html kodlarını tek sorumluk prensibine göre her view kendi görevini üstlenecek şekilde tasarladık.

```csharp
@Html.Partial("~/Views/Partial/_SidebarPartial.cshtml")

@{
    Html.RenderPartial("~/Views/Partial/_SidebarPartial.cshtml");
} 
<partial name="~/Views/Partial/_SidebarPartial.cshtml"></partial>
```

bu üç kullanım tarzıda geçerlidir aynı şeyi yapar hangisi kullanmak istersen artık sonda ki partial tag helper kütüphanesinin etiketi.



Partial'ın Kritiği yani veri gönderme işlemi

controllerda bir view'e verilen data view'in içindeki paritallara gidebilir o data oradan erişilebilir sonra layout kullanıyorsa o view ve o layout'un içinde bir partiallarda varsa o partiallarda controllerin view'i render edilirken layot'un paritalllarınada o datalar gider bu mimari böyle işler.


```csharp
using Microsoft.AspNetCore.Mvc;
using WebApplication1.Model;

namespace WebApplication1.Controllers
{
    public class HomeController : Controller
    {
        public IActionResult Index()
        {
            Product p = new Product()
            {
                Name = "se",
                Description = "s"
            };

            return View(p);
        }
    }
}
```

Sidebar.cshtml

```csharp
@model Product

<div class="sidebar">
    <a href="#">@Model.Name</a>
    <a href="#">@Model.Description</a>
    <a href="#">Dashboard</a>
    <a href="#">Profil</a>
    <a href="#">Mesajlar</a>
    <a href="#">Ayarlar</a>
</div>
```

Dikkat edersen Index'in view'ine yolladık datayı ama orada hiçbir model tanımlaması yapmadık datayı o viewde tanımlanana partialllara taşıdık o view de görünmeyen ama arka ilk çalışacak layout view'inin partiallana yolladık bu datayı yani datayı partiallara taşıyabiliyoruz.



Bir Diğer önelli kritik ise Biz yukardaki örnekte Index view'inde herhangi bir operasyonel bir çalışma yapmadıgımız için bir model kullanmadık şimdi model kullansak mesela product sınıfın model olarka aldık diyerim bu product modeli bütün partialların modeli olur sen dersen ben partiallarda 

bu view'in modelini değil farklı bir model kullanacan bunu html.partial'ın ikinci paremetresinde ne türden model geleceğini orada belirtmen gerekiyor yani ana view'in modeli neyse belirtmediğinde de kullanabilirsin ama farklı bir model lazımsa sana işte burada belirmek zorunasın.

Örnek olarak

```csharp
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using WebApplication1.Model;

namespace WebApplication1.Controllers
{
    public class HomeController : Controller
    {
        public IActionResult Index()
        {
            Product p = new Product()
            {
                Name = "se",
                Description = "s"
            };

            ViewBag.v = "Serkan Altunay ve mızık altunay";
            

            List<string> p2 = new List<string>()
            {
                "serkan",
                "altunay"
            };

            ViewBag.v2 = p2;
            return View(p);
        }
    }
}
```

Index.cshtml
```csharp
@model Product
<h1>kdnkandandandkandknadknadkna</h1>
```

Layout.cshtml
```csharp
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Header ve Navbar</title>
</head>
<body>
    <partial name="~/Views/Partial/_SidebarPartial.cshtml" model="ViewBag.v2"></partial>
    <div class="content">
       @RenderBody()
    </div>
</body>
</html>
```

Sidebar.cshtml

```csharp
@model List<string>

@{
    var p = Model;
    if (true)
    {
        
    }
}
<div class="sidebar">
    
    <a href="#">Dashboard</a>
    <a href="#">Profil</a>
    <a href="#">Mesajlar</a>
    <a href="#">Ayarlar</a>
</div>

@ViewBag.v
```

Home Controllerindaki Index actionında biz viewbag.v diye bir viewbag verdik ama gittik bunu partiallarında kullanabildik dikkat edersen veya gittik viewbag ile bir list string tutup bunu partiallına verdik ama burada list'in string oldugunu model'e verirken türünü belirttik bunun içinde partiallın oldugu yerin ikinci kısmınıda model belirlemesii yaptık oraya viewbag dedik orası layout yani layout içine de ana Index view'inin verileri oraya geliyor oradan aldık ve gitti sidebar view'ine gerekli operasyonlarını yaptı.

<font color="#6495ed">Çok Önemli Partial View olarak tanımlanan bir view asla ama asla içinde renderSection tanımlanamaz tanımlarsan hata almazsın ama kodun çalışmaz.</font>


## <font color="#6495ed">ViewComponent Nedir?</font>

Şimdi viewComponent mantığını anlamak için partial view yapılandırılmasını anlaman lazım partial view de biz bir partialın veri alması için bir controller şart yani ilgili bir controller bütün partiallarına veri göndermeli bir partial da bu sıratmaz ama birden fazla partial tanımlandığında controller’ın amacından çıkarır sürekli bir controllerin action'ında içindeki partialllara data yollamak hemde maliyetlidir bu yüzden bu yükü başka bir class yüklense oradan datayı yollasak işte bu yönteme ViewComponent denir.


Öncellikle iki kısımdan oluşur ViewComponent

1. cs dosyası ilgili backend işlemlerinin yürütüleceği yer
2. cshtml ilgil view üretilen backendlerin çıktısının vereceği yer.


ilk yapmamız gereken bir ViewComponents klasörü oluşturmak
sonra oraya bir class oluşturmak classın sonu ViewComponent ile bitirmek adettendir.

classın ViewComponenttden kalıtım alması lazım ve burası önemli içinde classın bir Invoke adlı bir metod olması gerekiyor bunun sebebi viewComponent bu metoda sayfa render edilirken tetikliyor ve iligli backend kodlarını çalıştırıyor bu olmassa çalışamaz bu çalışamazssa view'in modeli patlar.

sonra shared klasörünün altına Components oluştur ve ilgili bir cs classının isminin viewComponent ismini alma sadece ismini al o adla klasör oluştur ve içine  Default adlı bir view oluştur.


Biz UdemyCarbook projesinde ViewComponent klasöründe yani classların olduğu klasörde componentPartial ekini kullanmışız hep aslında viewComponent olara isimlendirmemiz gerekir ama olsun artık bu saaten sorna onu değiştiremeyiz Components klasörünün içinde klasör adı oluşturur iken ise direk o eki yazdık çünkü o ek isim viewComponet olsaydı tamam gerek kalmazdı ama componentPartial eki orada tanımlı olmadııgnıdan mimaride mecbur yazmışız mantıgı bu yani o projedi o kısım gayet düzgün ve mantıklı.


Örnek

```csharp
using Microsoft.AspNetCore.Mvc;

namespace WebApplication1.ViewComponents
{
    public class SidebarViewComponent:ViewComponent
    {
        public IViewComponentResult Invoke()
        {
            return View();
        }
    }
}
```

Shared/Components/Sidebar/Default
```csharp
<div class="sidebar">

    <a href="#">Dashboard</a>
    <a href="#">Profil</a>
    <a href="#">Mesajlar</a>
    <a href="#">Ayarlar</a>
</div>
```

burada ise SidebarViewComponent classını Sidebar ismini aldık geri kalan eki attık ve mimari bunu bu şekilde arayıp bulacak.

layout.cshtml

```csharp
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Header ve Navbar</title>
</head>
<body>
    @await Component.InvokeAsync("Sidebar")
    <div class="content">
       @RenderBody()
    </div>
</body>
</html>
```

burada await kullanmamızın sebebi InvokeAsync fonksiyonu async olduğundan task döndürüyor ondan await ile işaretlenmesi gerekiyor hatırlarsan bu gerekliydi async ile işaretlenen bir yapıda await olması await var c# da task muklakka olmalı zaten gibi.


### <font color="#6495ed">ViewComponent Kritiği buna bakmadna bu notu geçme belki buranın en önemli notu</font>

View Componentler kesinlikle bir get post delete remove işlemi yapamaz içine tamam biz bu zamana kadar tek ne yaptık backend olarak api den veri çektik tamam da bu get işteği değil şöyle değil viewComponent içinde hangi metod var Invoke metodu var bu metod ilgili viewComponent çağrıldığı anda yani bu çağırım ne zaman oluyor controller içindeki bir action render edilirken içinde bulunan InvokeAsync viewComponenti Invoke metodunu çalıştıryor arkada ve içinde ne 

kadar operasyonel işlem varsa yapıyor bir seferde sonra view'ini ayağa kaldırıyor bu get işteği mi şimdi değil Invoke sayesinde bir nevi get işteği olabiliyor diyebilirz ama post delete update gibi işlemler tıklama gibi olaylar ya bunu controller yapar iligli istediği karşılardı ya controllerlar bu controlelr mı viewComponnetler bunun sen gidip viewComponetleri bir tarayıcı urlinde 

Components/Sidebar yazınca seni ilgil ViewComponentemi atıyor hayır yani request karşılayaamz bunu yapsa yapsa Controller yapar viewComponent controller da bulunmuyorsa request atamaz request atamassa get delete post updatte yapamaz o zaman


Diğer bir kritik ise 

ViewComponent partial da nasıl controllerin verilerini viewBaglerini datalarını filan alabiliyorsa bunda da olay birebir aynı layout dahil controller içindeki bir action metodunun bütün kodları layoutta kullabılabilir dataları sonra içinde zaten kullanılabilir sonra içindeki partial viewComponent varsa onlarda da kullanılabilri var oğlu var yani her türlü kullanılabilir bunlar.


Diğer bir kritik ise 

Şimdi Partiallarda çok operasyonel işlemler yapamıyoruz çünkü partial'ın kendine ait bir controlleri yok bunu controller üzerinden yapıyoruz tamamda biraz detaylı düşünürsen mesela bir sayfa yaptınn butona tıkladığında ilgili partial içine o id'yi geçirmek istiyorsun bunu partial ile yapamazsın ki 

çünkü o id'yi alıp o id'ye göre işlemler yapamn lazım bir cs uzantılı class lazım partial dediğin cshtml türünde bir view eee o zaman yapacakların çok kısıtlı

Amaaa

ViewComponnette bunu yaparsın InvokeAsync metodunun ikinci paremetresine controllerdan gelen id'yi alırsın gider bunu viewComponet classında yakalarsın sonra ilgil backend operasyonlarını yapıp ViewComponentin View'ine yollarsın olay bitti 

mantık tamamen bu bize adım adım controller gerekli partial da ikinci bir controller yok ama viewComponentte ikinci controller mantığı oluşturuabiliyor çaktırmadan.

Örnek :

Layout.cshtml
```csharp
@await Component.InvokeAsync("Sidebar",new Product{ Name = "Serkan", Description = "Elektronik" })
```

Burada new ile bir Product classı oluşturduk ve içindeki propertyleri doldurup ilgili viewComponentin classında yakaladık. 

Diğer bir kritik ise NonViewComponent

NonViewComponent Normalde ASP.NET Core, `ViewComponent` sınıfından türeyen tüm sınıfları otomatik olarak ViewComponent olarak tanır. Ancak bazı durumlarda bu sınıflardan biri ViewComponent gibi davranmamalıysa, yani Razor tarafından render edilmemesi gerekiyorsa, bu sınıfa `[NonViewComponent]` attribute'u eklenir.

```csharp
using Microsoft.AspNetCore.Mvc;
using WebApplication1.Model;

namespace WebApplication1.ViewComponents
{
    [NonViewComponent]
    public class SidebarViewComponent:ViewComponent
    {
        public IViewComponentResult Invoke(Product p)
        {
            return View();
        }
    }
}
```