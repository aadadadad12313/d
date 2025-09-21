

## <font color="#8db3e2">Kullanıcıdan Veri Alma Yöntemleri</font>

1 Form ile model binding yapabiliyoruz bunu yukarada örneklendirdik

2 QueryStringler ile de yapabiliyoruz bu ne yapıyor url üzerinden ? ile verileri açıkca yazıyoruz istek neticesinde ilgili propertyler ile eşleşiyor bunu şurada kullanabiliriz Formun veri taşıması için post olması gerekir ama Querystringler de get ile post atabiliyoruz. 

Örnek :

```csharp
https://localhost:44384/product?a=5&b=20&c=40
```

```csharp
  public IActionResult Index(string a, string b, string c)
  {
      return View();
  }
```

tarayıcıda ilgi action'a istek atarken ? ile isimler ile tanımladığın propertyler aynı olanlar binding ediliyor form olmadan olay bu. & ise birden fazla isim varsa ve anlamında.

```csharp
 public IActionResult Index()
 {
     string a = Request.Query["a"];
     string b = Request.Query["b"];
     string c = Request.Query["c"];
     return View();
 }
```

illa action'ın paremetresinde yakalanacak diye bişey yok Request.Query ile de fonksyion içinde de yakanabilir veri.

<font color="#c0504d">Bu QueryStringler url üzerinden olduğundan post işlemlerinde olmaz çalışmazlar post isteklerinde veriler body içinde gönderilir, url’de değil.</font>


3 Router Paremeter

 ```csharp
  pattern: "{controller=Default}/{action=Index}/{id?}");
```

Router üzeirnden id mesela zornlu değil ya ama biz /5 dediğimizde eğer ilgili actionda ilgili property varsa id propertysi ona hemen bind edilmesidir.

##### <font color="#548dd4">Kendin custom Routeni oluşturmak istiyorsan mapControlerRoute ile yapılıyor.</font>

```csharp
 endpoints.MapControllerRoute("Deneme", "{controller=Home}/{action=Index}/{a}/{b}/{c}");
```

`controller=Home` sadece **varsayılan değerdir**. Yani controller verilmezse Home alınır. Ama `Product` yazarsan `ProductController` devreye girer. Url de verirsen Product/Index bu böylece alınır ama vermezsen controlleri filan varsayılan olarak tanımlana home alınır.

```csharp
 public IActionResult Index(string a,string b,string c)
 {
     return View();
 }
```

Sonrasında bu url'e bir istek attınmı ilgili kurala uyan controller ve action tetiklenecek ve a b c değerleri binding edilecek. 

Burada dikkatli olman gerken yer url tanımlarken endpoins'e verdiğin a b c değerlerini bire bir ilgil
actionda da tanımlaman gerekir ki mvc mimarisi onları aynı olan propertyleri binding mekanizması ile anlayabilsin.

> [!note] QueryStringler ile Router Farkı
> QueryStringler url üzerinden açıkca hem property'i yazarsın hem değerini yazarsın ama Router da sen property kısmını endpoint içinde yeni bir url oluştururken içine yazarsın url de ise bunu sadece değerini verirsin yani bir bakıma gizlemiş olursun Router da property'i bu da QueryStringlerden daha güvenli olduğunu gösteriyor Rourter Tanımlanmasının.


##### <font color="#548dd4">4 Headers ile veri alama yöntemi</font>

Headers'lar tarayıcıdan url üzerinden verilemez çünkü Çünkü tarayıcı adres çubuğu sadece **URL parametrelerini** destekler, **özel header bilgilerini** manuel olarak göndermeye izin vermez. bundan postman aracını kullanarak header bölümünden ilgili url'e istek attık ve ilgili Actiona istek geldi gönderdiğimiz değer geldi headers değişkenine.

```csharp
       public IActionResult Index()
       {
           var headers = Request.Headers.ToList();
           return View();
       }
```

mesela header ile ne yapabliriz gerçek hayatta bir json web token isteği atabiliriz yani

Authorization: Bearer abcdefg123456 gibi 

```csharp
GET /api/products HTTP/1.1
Host: example.com
Authorization: Bearer abc123
Content-Type: application/json
X-UserId: 42
```

Postmanda bir istek Header isteği atıldığında bu şekil tarayıcıda tutulur metadata olarak yani buradan kolaylıkla bu bilgilere ilgili fonksiyonlar ile ulaşabiliriz. ama bu işlemi headers'ı yani tarayıcıda yapamayız özel eklentiler gerekir ama postman en iyisidir.

### <font color="#6495ed">5 Axaj ile veri alma yöntemi</font>


```js
<script src="https://code.jquery.com/jquery-3.7.1.min.js"></script>

<button id="button">Gönder</button>

<script>

    $("#button").click(()=>{
    $.ajax({
        url:"https://localhost:44384/product/Index",
        type:"POST",
        data:{
            a:"a data",
            b:"b data"
        },
        success:function(response){
            console.log("Çıktısı: ",response);
        }
     });
    });
</script>
```

```csharp
[HttpPost]
public IActionResult Index(string a,string b)
{
    return View();
}
```

