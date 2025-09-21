
onemli::Route oluşturma ve Attribüte route kullanımı

## <font color="#6495ed">Route</font>

Route : Gelecek olan isteğin hangi rotaya gidecegini belirleyen şablonlardir.

sen request attığında attğın endpoint'i ayrıştırıyor controlleri filan actiondan ayırıp parçalıyor sonra ilgili tanımlanan endpointler ile eşleştirme yapar uyan endpoint'i bulur.

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(
        name: "default",
        pattern: "{controller=Home}/{action=Index}/{id?}");
});
```

`UseRouting()` önce çalışır - URL'yi analiz eder, `UseEndpoints()` sonra çalışır - hangi endpoint'e gideceğini belirler.

`app.UseEndpoints()`, URL routing'i yapılandırır ve hangi URL'lerin hangi controller/action'lara bağlanacağını tanımlar.


```csharp
 app.UseEndpoints(endpoints =>
 {
     endpoints.MapDefaultControllerRoute();
 });
```

yukardaki kod yerine bu kodda aynı işlemi yapar default tanımlama controller'ı home action'ı index.

bak burada dikkat etmen gereken yer var mesela sen istek attakken controller'ı verdin ama action vermeden istek attın diyerim o zaman controller'a mesela Product diyerim action olmadığından gider bakar controller'ı var ben buna Product verecem ama action'ı yok default olan rota da neydi bakar Index o zaman rota şöyle olur Product/Index buna gider.

```csharp
pattern: "{controller=Home}/{action=Index}/{id?}");
```

yani burası bu endpoint'e uygun istek gelirse onu işler ama gelmezse Controller olarak hazır değeri yollar Home'yi yani action'ında da aynı durum var action yoksa nullsa hemen Index ismini yollar.


Kendin Custom rotanı yapmak istiyorsan aslında biliyorsun yapmayı

```csharp
 app.UseEndpoints(endpoints =>
 {
     endpoints.MapControllerRoute(
         name: "default",
         pattern: "{controller=Home}/{action=Index}/{id?}");
 });
```

bu yapı name olarak unique benzersiz bir isim verecen pattern'de controler home değil de Default yazarsın action genellikle Index olur oldu mu sana custom rota.

varsayılan rotası Controller'ın Home'ydi şimdi default oldu custom oldu yani.

Burada dikkat etmen gereken nokta {  } her bir süslü parentez bir paremetredir. ve bunlları doldurmalısın.


```csharp
 endpoints.MapControllerRoute(
     name: "default3",
     "anasayfa",  // Tarayıcıda yazılacak URL
     new {controller="Product", action="Index"} // Hedef Controller/Action
     );
```

Bu kullanım ise şuna yarar sen url de anasayfa yazdın enter'a tıkladın attın requesti diyerim direk bu istek okunur ve Product Index sayfasına atar seni burada bunu yaptık. böyle çok kullanılan sayfaları direk url de yazarız direk o sayfaya atar yaa hani onu yaptık güzel bence kullanışlı mimari destekliyor.

yani
- Kullanıcı tarayıcıya **`/anasayfa`** yazarsa →
- `ProductController` içindeki **`Index`** action çalışır.


> [!note] Önemli
> Routeları özelden genele doğru koy burada sıralama önemli çünkü eşleştirme yapıyor ya hani ilk bulduğunu alır ondan en özeli ilk başa koyarasan dahi iyi olur en sona da genellikle mapDefaultRoute koyulur hiçbiri uymassa onla eşleşin diye

```csharp
app.MapControllerRoute(
    name: "areas",
    pattern: "{area:exists}/{controller=Home}/{action=Index}/{id?}"
);

app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Default}/{action=Index}/{id?}"
);
```

ASP.NET Core routing sistemi **ilk eşleşen route'u** kullanır

Mesela benim UdemyCarbook'un areas'ı daha önce tanımlandı neden çünkü request ile uyarsa direk bu çalışşın uymassa alttaki çalışşın şimdi tam tersi olursa bazı sorunlar çıkabilir burada belki o sorunlar çıkmaz ama en çok kullanmadığını başa koy ki rahat et.

```csharp
@Html.ActionLink("Anasayfa","Index","Product")
<a asp-action="Index" asp-controller="Product">Anasayfa</a>
```

Burada iki kullanımda farketmez ikiside aynı işlemi yapıyor sayfa çalıştırırken senin endpointlerine bakıyor eşleştirme yapıyor ve sen dikkat edersen a'ların üzerine geldiginde locahost/Product/Index yazıyor yani bu ne demek arkada bunu otomatik anlamış mimari buna göre linkleri oluşturmuş.

yani sen burada bir link oluşturuyorsun bu link'in request atar gibi gerekn paremetrelerini veriyorsun bu link ilgili eşleştirme ayağa kalkarken iglil endpoint'e uygulanır. ona göre link oluşur.

mesela 

```csharp
@Html.ActionLink("Anasayfa","Index","Product")
```

```csharp
 endpoints.MapControllerRoute(
     name: "default3",
     "anasayfa",
     new {controller="Product", action="Index"}
     );
```

bu link oluşurken default3 ile eşleseceğinden bizim linki adresi üzerine gelirse a etiketinin şu olur: 
https://IocaIhost:44348/anasayfa olur. yani Product/Index yazmıyor dikkat edersen bu da demek oluyor ki eşleşme otomatik mimari tarafından yapılmış.



## <font color="#6495ed">Attribute Routing</font>


```csharp
app.MapControllers();
```

bu fonksiyon şu işe yarıyor Controller'ları keşfeder ve sadece attribute routing'i aktif eder

Bu Kodu kullanmalısın Attribute Routing çalışması için ama dikkat önemli bir kriter var eğer mapControlerRoute fonksiyonu varsa gerekmez bu fonksiyona.

Neden diye sorarsan zaten mapControllerRoute için de tanımlı bu sen bir daha yazmana gerek yok.

Ama ne zaman gerek var dersen şurda gerek var işte mapControllerRoute'nin olmadığı yerde mapControllers diye yazmalısın ki route Attributler çalışın. 

örnek olarak Api'ler de biz conventional route kullanmayız yani mapControllerRoute olmaz.
o zaman mecbursun orada mapControllers fonksiyonu'nu tanımlamaya.

yani sen normal bir kullanımda UI Katmanın da yani MapControllerRoute ile git ister route kullanmadan kullan MapControllerRoute geçerli olsun ister git route kulllan attribute route geçerli olsun.

Çalışma Sırası Önce

1  Attribute routing her zaman önce çalışır.
2  Conventional routing ise, sadece attribute route bulamıyorsa devreye girer.

 **Attribute Routing Nedir?**

- **Route**: Kullanıcı tarayıcıda bir URL’ye girdiğinde hangi controller ve action çalışacak onu belirler.
    
- **Attribute Routing**: Rotaları controller ve action metotlarının üzerine `[Route]`, `[HttpGet]`, `[HttpPost]` gibi attribute’larla doğrudan tanımlama yöntemidir. yani adı üstüinde attribüt bazlı çalışıyor conventional routingde olduğu gibi çalışmıyor farkları var şu mesela

 mesela normal bir route nasıl çalışır httget olmassa bile httget kabul edilir ama attribute routing de bu yok tek tek httget olduğunu belirtmelisin çünkü sen ezdin artık o conventinal routing mantığını ondan burada böyle işliyor.
	
- **Conventional Routing**’den farkı: `Program.cs` veya `Startup.cs`’de genel bir şablon tanımlamak yerine **her metoda özel rota yazarsın**.

yanisi şu ya gider controller seviyesinde hem controller hem action paremetrerlini verirsin startupdaki gibi olur.

```csharp
[Route("[controller]/[action]")]
public class HomeController : Controller
{
    [HttpGet]
    public IActionResult Index() => Content("Index");

    [HttpGet]
    public IActionResult Hakkinda() => Content("Hakkında");
}
```

ya da controller ismini verirsin action ismini siler oradan httget  (  "    "    ) ile isimlendirerek her bir action'a farklı isimler verirsin eğer koddaki gibi yaparsan Index kullanırısn ama Index'e mesela serkan ismini verisen httgette yani artık Index ismi ezilir Serkan ismi olur ve bu da güzel bir kullanım tarzı apilerde biz mesela hep metod ismini değil httgette verdğimiz isimleri kullanıyoruz dikkat edersen.

ya da

```csharp
[Route("[controller]")]
public class HomeController : Controller
{
    [HttpGet("About")]
    public IActionResult Index() => Content("Index");

    [HttpGet("Car")]
    public IActionResult Hakkinda() => Content("Hakkında");
}
```

böyle yaparak da action ismini url de kullanmayıp direk Home/About dersek Index tetiklenir işte bu yöntem güzel. aynısı startup da da yaparsın nasıl yaparsın gider endpointte action paremetresini kaldırırsın buraya.

bak iki yöntem var ya startup üzerinde route tanımlarsın ya gidip her controllerda route tanımlarsın orası sana kalmış.

Conventional routing temelde **action adını** URL’nin bir parçası olarak bekler. 

bir başka yöntem ise attribute routing işte bu çok çok esnek illa metod ismini kullan demek zorun da değilsin gider farklı bir isimlendirme yaparsın isteği o yeni tanımladığın isimle atarsın bence baya iyi bir özellik.

aklına şu gelmesin ben startup da action tanımlaamyım controller tanımlayım action ksımını controllerda şu şekilde

```csharp
[HttpGet("Car")]
```

vereyim bu da olur olmaz çünkü bu bir attribüt ondan bunun çallışması için bir controlleri veya statik bir ismi de olabilir bunları hep controlelrda birlikte yapacaksın startupla asla birlilte kullanılmaz.

yani

```csharp
using Microsoft.AspNetCore.Mvc;

namespace Net_Core_Dersleri.Controllers
{
    [Route("ana/[action]")]
    public class ProductController : Controller
    {
        public IActionResult Index()
        {
            return View();
        }

        public IActionResult Deneme()
        {
            return View();
        }
    }
}
```

route attribute sana iki şey sağlıyor

1. controller ismini farklı isim ile istek atabiimeyi 
2. action ismine farklı bir isim ile istek atabilmeyi

bunu startup üzerinde yapamazsın yapabilmen için ne yapman lazım controller ismi product ise serkan olarak istek atamya çalıyorsun o zaman product ismi serkan olacak başka türlü nasıl yakalasın startupda ki endpoint ama bu route attribute işte bun yapıyor isteği farklı isim ile karşılıyor hem controller ismi değişmiyor sana bir sürü yol sağlıyor.


## <font color="#6495ed">işin mantığı önemli</font>


```csharp
using Microsoft.AspNetCore.Mvc;

namespace Net_Core_Dersleri.Controllers
{
    [Route("[controller]")]
    //[Route("serkan")]
    public class ProductController : Controller
    {
        [Route("ahmet")]
        public IActionResult Index()
        {
            return View();
        }
        [Route("[action]")]
        public IActionResult Deneme()
        {
            return View();
        }
    }
}
```


burda Product/ahmet diye istek atabilirsin
eğer route[serkan ] geçerli olsayı serkan/ahmet diye atardın

eğer  [Route("[controller]")] geçerli olaydı ve metod isimleri route ile belirtrmeseydi Index diye istek atamazsın atamazsın sebebi metod isimleri bakılmaz ondan route tanımlayıp her biriine bir isim vermen gerekiyor yada direk controller seviyeisnde controller yanına action paremetresiinde verebilirsin.

deneme adlı actionda ise route değeri action paremetresi almış yani bu şu demek 
Route sırası **controller route + action route**  değil mi öyleyse ikinci / paremetre action demek olur oradan gelen ismi yakala buraya ver demek.

<hr>

diğer önemli bir husus ise route olan bir yer asla ama asla startupda ki yapı ile hrbirlikte çalışmaz 
ondan route tanımladıgın anda route attribute sınırları dahil olur ondan starupla ilişkin biter operasyonlarını yaparsın burada.

<hr>


```csharp
[Route("api/[controller]")]
```

Dikkat et biz startup da tanımlarken route orada {  } ile paremetre tanımlarken burada [   ] köşeli parentezler ile paremetre tanımlamasının yapıyorsun.

Apilerde her controller üzerinde bu route tanımlaması olmalı çünkü api controllerlar bizim bildiğimiz controller'a göre farklı çalışıyor.

mesela normal bir controllerda biz route vermeyiz ne yapar alır controller ismini ve action ismini birleştirip bir url oluştururken api controllerda bunu kendimiz yapmalıyız.

ondan bu route burada durmalı bu aslında api/ dedikten sonra bir paremetre istiyor o paremetrede zaten ilgili istek atılan controllerın ismi gelecek ve böylece bir sürü controller apisi çakışamayacak.

api/Comments 
api/Blogs 

gibi ama birde önemli olan yer var bu birinci yapman gereken di. birde action isimlerini adam otomatik alıp url yapmıyor bunuda yaptırmalıyız onuda httget içine ( ) açıp isim belirtiyorsun böylece controller içindeki get istekleri filan çakışmıyor.

bunu yapmasan iki tane get istegi olursa 

api/blogs get istegi attı diyerim hangi action attı getBlogs
api/blog get isttegi attı diyerim hangi action attı getBlogId

mesela ama action İsmine bakmıyor get istegine bakıyor ve url'e bakıyor url böyle olur

Get api/blogs/ 

ve bu durumda bizim 10 tane get isteği bile olsa sadece çalışan ne olur ilk yazılan get isteği olur getList yani diğerlerini kullanamazsın ismi yok çünkü. 


Ondan

```csharp
 [HttpGet]
 public async Task<IActionResult> ContactList()
 {
     var values = await _getContactQueryHandler.Handle();
     return Ok(values);
 }

 [HttpGet("{id}")]
 public async Task<IActionResult> GetContact(int id)
 {
     var value = await _getContactByIdQueryHandler.Handle(new GetContactByIdQuery(id));
     return Ok(value);
 }
```

şimdi burada dikkat et  {  id  } var bu ne demek paremetre demek routelerde böyleydi biliyorsun.

sen bunun apisinde veya consumensinde bir değer veriyorsun 5 mesela burada ne olacak o zaman

şimdi consume sırasında artık nerdeyse controller ismini belirtiyorsun 

```csharp
[Route("api/[controller]")]
```

burası oluyor mu

api/Comment 

sonra hangi action tetiklenecek biliyorsun apiler de action ismine göre isimlendirme yok tek tek isimlendirme yapmalısın.

5 değerini gönderdik o zaman paremetre alacak action bulunacak bulundu action o tetiklenecek bunda 5 değeri var url de değil mi bu gidecek actionda ki id değerine bind edilecek sonra iligl meditor işlemleri başlayacak sıralama bu.


```csharp
[HttpDelete("{id}")]
public async Task<IActionResult> RemoveCar(int id)

[HttpDelete("{id2}")]
public async Task<IActionResult> RemoveCarByBrand(int id2)
```

Mesela bu kod çalışmaz neden mi şundan sen 5 paremetresini yolladın diyerim hangisinde yakalanacak ayırt edilecek ikisi de tek paremetre bekliyor. ondan burası patlar.


```csharp
  [HttpGet("{id}")]
  public async Task<IActionResult> GetCar(int id)
  {
      var value = await _getCarByIdQueryHandler.Handle(new GetCarByIdQuery(id));
      return Ok(value);
  }
  
  [HttpDelete("{id}")]
  public async Task<IActionResult> RemoveCar(int id)
  {
      await _removeCarCommandHandler.Handle(new RemoveCarCommand(id));
      return Ok("Araba Silindi");
  }
```

mesela bunlarda hata olmaz çünkü birisi get isteğinde çalışacak biris delete de aynı şeyler değil yukardaki ile ama iki tane delete veya get isteği aynı paremetre sayısında olursa iste orada ayrım yapacak bir yöntem isim gerekecek.


 ```csharp
  [HttpGet("serkan")]
```

böyle yaparak paremetresiz isim verme işlemini yapıyoruz.


peki dersen eğer neden biz normal bir startup üzerinden apileri de kolaylık yapamıyoruz böyle attributler ile uğraşıyoruz sebebi şu

apinin alacağı paremetreler değişkenlik gösterir bir api bir paremetre alırken diğeri 3 alır yada hiç almaz veya paremetre almayan statik bir isim yazar yani bir bütünlük sağlayıp da apilerde bir endpoint tanımlayamazsın startup da ne yaparsan yap tek tek apilerine httget ile isim vermelisin yada paremetresini artık ne gerekiyorsa.

yani

- **Conventional routing** bu kadar esnek değildir. Genelde `{controller}/{action}/{id?}` gibi sabit kalıplar kullanır.
    
- **Attribute routing** ile her action’a özel, semantik ve RESTful URL tanımı yapılabilir.


Kullanım Tarzı

```csharp
[Route("GetReviewByCarId")]
[HttpGet]
public async Task<IActionResult> GetReviewByCarId(int id)
{
}

 [HttpPost("CreateReview")]
 public async Task<IActionResult> CreateReview(CreateReviewCommand command)
 {
 }
```

bu ikisi de aynıdır ikisi de olur farketmez route içinde rota belirtirsin httget get isteği olacağını belirtir. bunlar da sorun olmaz ama ben tek tek hem get attribütü yazıyım hem route attribütü ile her seferinde uğraşmak istemem dersen eğer get veya post arttık neyse (  ) açıp route tanımlamasını buraya yaparsan daha kolay ve okunabilir olur ama zorunda değilsin sana bağlı.

yani biz httpost içine parantez açıp bişey yazarsak artık bu url olur action ismi varsayılan olarak kullanılmaz bu url kullanılır yazmasan hiçbir şey default olarak action ismi neyse o kullanılır.

yani httget içine bişey yazmak demek url demke url demek route demek ondan bu attribütü kullanmak gereksiz.

## <font color="#6495ed">Örnek Udemy Carbookdan Alınan Bir Örnek</font>


```csharp
﻿using Microsoft.AspNetCore.Mvc;
using Newtonsoft.Json;
using System.Text;
using UdemyCarbook.Dto.LocationDtos;

namespace UdemyCarbook.WebUI.Areas.Admin.Controllers
{
    [Area("Admin")]
    [Route("Admin/AdminLocation")]
    public class AdminLocationController : Controller
    {
        private readonly IHttpClientFactory _httpClientFactory;

        public AdminLocationController(IHttpClientFactory httpClientFactory)
        {
            _httpClientFactory = httpClientFactory;
        }

        [Route("Index")]
        public async Task<IActionResult> Index()
        {
        }
        
	    [Route("Car")]
        public async Task<IActionResult> Car()
        {
        }
```

gördüğün gibi route controller seviyesinde rota olarak tanımlanmış bizim varsayılan bir endpoint startup da var ama o bizden controller/action bekliyor bunu karşılayamaz çünkü bu admin/controller/action diye url olduğundan.

bizim startup daki endpoint bunu karşılayamadığından ya böyle her controllera bir route belirtirsin yada startup'a yeni bir endpoint ile bunu daha generic yaparsın.

şimdi dersen eğer neden metotların üstünde route tanımladık normalde controller url'i zaten metod ismini alıyor dersen bunun sebebi biz burada Route ile ezdik bu normal mimarinin davranışını ondan bir action route belirtmen gerekiyor işter controller seviyeisnde route belirt action için ister her metoda ayrı isim ile belirt ama action olmak zorunda.

yani 
- **Conventional routing** → `[Route]` yazmazsan, `Category/Index` otomatik çalışır. yani`Controller` + `Action` isimleri otomatik URL olarak kullanılır. bu buraya ait bir davranış.

- **Attribute routing** → `[Route]` yazarsan, tüm yolları elle belirtmek zorundasın (aksi halde match olmaz).

## <font color="#6495ed">bir örnek daha</font>

```csharp
using Microsoft.AspNetCore.Mvc;

namespace Net_Core_Dersleri.Controllers
{
    [Route("ana/[action]")]
    public class ProductController : Controller
    {
        public IActionResult Index()
        {
            return View();
        }

        public IActionResult Deneme()
        {
            return View();
        }
    }
}
```

şimdi burada ana/Index dersen seni index sayfasana atacak deneme dersen ona atacak.

dersen nasıl action'ları bu yakalıyor özel bir paremetre action mimari bunu anlıyor nasıl startup da yapıyorsa orda anlıyorsa burada da anlıyor senin yazdığın url de ana dan sonra gelen /Index kısmını yakalıyor ve bu isim ile eşleşen action'ı tetikliyor.
