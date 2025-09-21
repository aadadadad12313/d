
**`_ViewStart.cshtml`** dosyası, bir projede **tüm Razor görünümlerinin (View)** başlangıcında otomatik olarak çalıştırılan bir dosyadır.

📌 **Temel amacı**: Ortak ayarları veya kodları her sayfaya tek tek yazmak yerine, hepsinde otomatik olarak uygulamaktır.  
En yaygın kullanım alanı **tüm sayfalara ortak bir Layout tanımlamaktır**.

## 📍 Mantığı

- MVC uygulaması bir View render ederken önce **`_ViewStart.cshtml`** dosyasını okur.
    
- layout belirlenir ama önce istek gelen view render edilir içindeki kodlar çalışır sonra layout dosyasına kodları aktarılır ve çalıştırılır.

-  yani tamam viewstart en erken çalışır ama layouttu okur çalıştırmaz önce action olan view çalışır kodları viewdataları filan render edilir okunur bunlar bittikten sonra en son layoutta basma yapılıp sayfa açılır.

   yani kafa karışıklığı olacak bişey yok biz normal de nasıl tanımlarıdık layout yolunu gider action içindeki viewde tanımlardık önce orası okunur du ama çalışmıyormuş demek alttaki kodları da okuyor sonra layouttu okuyor ve ekrana basıyor bunda sorun yoktu.

    benim kafamı karıştıran viewstart viewlerden önce çalışan bir dosya olması tamam en önce çalışıyor sonra gidip action view'inin kodlarını çalıştırıp layoutu render ediyior yine aynı bak durum.

    burada viewstart ilk çalışıyor tamam viewstart kullanmasydın da zaten view'in en üstüne zaten yazıyoesun layout'u ondan yine aynı mantık staticten dinamiğe çektik ama asla layout dosyası başta çalışmaz çalışırsa viewdatalar filan daha render edilmediginden ilgil action metodunu view'i layoutta baya bir yer null olur.
    
	ya zaten sen partial konsuunda dedin ki view içindeki viewbaglar layoutta kulalnılabilir dedin mantıkken view önce çalışmassa layout nasıl viewbagleri kullansın tam tersi olsa önce layout çalışşa sonra view çalışşa olmaz ki ondan önce view sonra layoutt çalışıyor.


📌 Örnek

```cs
@{
    Layout = "~/Views/Shared/_Layout.cshtml";
}
```

Ama biz bazen her view'de bir layout kullanmak istemeyiz yada kullanırız ama viewStart dosyasındakini kullanmayı istemeyiz bu durumda layout kullanmayacaksan layout=null kullacaksan ama viewStartda ki layout değilse ilgli controllerın view'inde layout= diyerek adresini yazacaksın default layout ezilecek.

## <font color="#6495ed">ViewImports</font>

ViewImports ise tek tek kütüphaneleri namespace olarak model de tanımlamak yerine tek bir view de  tanımlayıp bütün viewlerde geçerli olmasıdır kısaca viewStart nasıl layoutları generic yapıyorsa viewImports da namespacelerin yollarını  tek tek tanımlamaktan kurtarıyor.


Örnek :

```csharp
@using UdemyCarbook.WebUI
@using UdemyCarbook.WebUI.Models
@using UdemyCarbook.Dto.AboutDtos

@using X.PagedList.Mvc.Core
@using X.PagedList.Web.Common
@addTagHelper *, X.PagedList.Web.Common

@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
```

Dikkat edersen, .NET Core’a özel tag helper’lar dosyada otomatik olarak tanımlı geliyor. Çünkü mimari, üst sürümlerden itibaren bu namespace’i _ViewImports dosyasına otomatik olarak ekliyor. Böylece her bir view dosyasında tek tek bu namespace’i tanımlamak zorunda kalmıyoruz.

