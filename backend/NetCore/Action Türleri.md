

<font color="#4bacc6">View Result</font>

```csharp
 public IActionResult Index()
 {
     return View();
 }
```

View bir viewResult türündedir Yani View kullanıcıya bir view render edip döndürmek amaçlı kullanılır.

Şöylede yazılabilir :

```csharp
public IActionResult Index()
 {
     ViewResult result=View();
     return result;
//Aslında mantık şu View bir sınıf nesne oluşturacak bir sınıf nesne tam      burada oluşturuluyor ViewResult ise referans veren ksıım içinde hazır birkaç tane property var bunları veriyor result . basıldıgında polimorfizm var burada. 
 }
```


<font color="#4bacc6">Partial View Result</font>

Sayfa yapısını partiallar ile parçalara bölmeye yarar ajax gibi anlık yenilemelerde kullanışlıdır.
view ile döndürmek demek bütün sayfayı render etmek demek ama partialViewResult sadece o partiallın olduğu yeri render et demek.

```csharp
  public PartialViewResult Index()
   {
      PartialViewResult result = PartialView();
      return result;
   }
```


<font color="#4bacc6">Json Result</font>

**JsonResult**, bir controller'dan tarayıcıya **JSON formatında veri göndermek** için kullanılan bir dönüş tipidir. Yani bir action içinde `Json()` metodunu çağırarak C# nesnesini JSON'a çevirip dış dünyaya aktarabilirsin.

```csharp
    public JsonResult Index2()
    {
        JsonResult result = Json(new Product
        {
            Name="Serkan",
            LastName="Altunay"
        });
        //Json şeklinde gider bundan foreach ile doğrudan alıp basamazsın dönüşüm gerekli c# koduna
        //Ajax kullanıp basabilrisn yada buradan  deserialize edilmesi gerekir.
        return result;
    }
```

Bu doğrudan tarayıcıda tutulur oraya istek attıp javaScript ile fetch attıp alabilirsin verileri.


<font color="#4bacc6">Empty Result</font>

.NET Core'da `EmptyResult`, bir controller action'ının hiçbir içerik döndürmemesi durumunda kullanılır. Yani istemciye yalnızca HTTP durum kodu gider, ama gövde tamamen boştur—ne JSON, ne string, ne HTML.

```csharp
public IActionResult SadeceIslemYap()
{
    // Örneğin log kaydı, veri tabanı işlemi vs.
    Console.WriteLine("İşlem yapıldı");

    // Ama istemciye hiçbir şey gönderilmez
    return new EmptyResult();
}
```

<font color="#4bacc6">Content Result</font>

İstemciye düz metin, HTML veya farklı bir MIME türüyle içerik döndürür.

```csharp
public IActionResult Mesaj()
{
    return Content("Merhaba dünya!");
}
```


<font color="#4bacc6">Action Result</font>

Tüm özel dönüş türleri (`ViewResult`, `JsonResult`, `ContentResult`, `EmptyResult`, `RedirectResult` vs.) aslında `ActionResult` sınıfından türemiştir.

```csharp
public ActionResult Index()
{
    return View(); // ViewResult döner ama ActionResult ile sarılır
}

public ActionResult Index()
{
    return json();
}

public ActionResult GetProduct(int id)
{
    var product = _db.Products.Find(id);
    if (product == null)
    {
        return Json(); // 404 döner
    }

    return Ok(product); // 200 OK + JSON döner
}
```

Kısacası tüm yukarda yazan türlerin hepsini actionResult ile'de yapabilirsin en ataları action result diğer türler ise bundan türeyen childrenlar oradan istediğini kullan.

<font color="#4bacc6">IAction Result</font>

Action Result interfacesidir.