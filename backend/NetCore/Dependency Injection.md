
##### <font color="#92d050">Konu Anlatımı</font>

Dependency Injection, bir sınıfın ihtiyaç duyduğu bağımlılıkları kendisi yaratmak yerine dışarıdan almasıdır.

Yani bir sınıfın başka bir sınıfa ihtiyaç duyduğunda, o bağımlılığı kendi içinde new ile oluşturmak yerine dışarıdan verilmesini sağlar böylelikle tek yerden nesnelerini yönetir kontrol sağlarız.


<font color="#92d050">DI Kullanmadan:</font>

```csharp
public class Service
{
    private readonly Repository _repository;

    public Service()
    {
        _repository = new Repository();
    }
}
```

Böyle olunca sen bağımlı oluyorsun buraya yarın bir gün repository sınıfını silsen 20 tane yerden tek tek silmen gerekecek.

<font color="#92d050">Dependency Injection ile</font>

Önce DI Container'a kaydetmen gerekiyor servislerini istendiğinde instance oluşturulacak.

```csharp
services.AddScoped<IRepository, Repository>();
```

Controller'ları veya Handler'ları kaydetmek zorunda değilsin çünkü bunlar otomatik DI takibinde.

```csharp
public class AdminController
{
    private readonly IService _service;

    public AdminController(IService service)
    {
        _service = service;
    }
}
```

AdminController constructor’ında bir IService talep ediyor.

Normal de sen bir interfaceden instance oluşturamazsın ondan bunun concretini yani implement edildiği sınıfı constractıra yazıp new yapman lazımdı ama DI Container diyorki 

sen herhangi bir classına IRepository yazdığında ben bunu reflection ile yakalayacam ve o sınıfa kaydettiğin Repository sınıfının nesnesini dinamik olarak üretecem.


DI container arkada aynen şöyle yapyor : 

```csharp
var constructor = typeof(RemoveAboutCommandHandler).GetConstructors()[0];
var parameters = constructor.GetParameters();
```

Reflection ile AdminController'ın cosntractır paremetresine baktı eğer DI da kaydı var ise instance üretecek. 

```csharp
var repoInstance = Activator.CreateInstance(typeof(Repository));
```

Bu

```csharp
IRepository repository=new Repository();
```

new Repository() ile aynı şeydir ama runtime’da, tip bilgisi üzerinden yapılır.

yani oluşan repoInstance nesnesi ilgili controllerın constractırına reflection ile constractır kodunu yazıp paremetre olarak repoInstance isntancesi gönderiliyor yani arkada doğrudan return gibi bişey yok reflection kodları var.

<font color="#92d050">Özet:</font>

- Reflection kullanıyor → hem constructor parametrelerini bulmak, hem de instance yaratmak için.

- DI container bunu senin manuel new yapmana gerek kalmadan otomatik yapıyor.
    
- Yani AddScoped vs. ile kayıtlı tipleri bulmak için reflection’a bakıyor; instance yaratmak için de reflection kullanıyor.

##### <font color="#92d050">AddScoped ,  AddTransient , AddSingleton</font>

ASP.NET Core’da Dependency Injection (DI) Container kullanırken servisleri eklerken `AddScoped`, `AddTransient`, `AddSingleton` gibi metotlarla servislerin yaşam süresini (lifetime) belirtiyoruz.

Asp Net Core Dedency Injection Container kullanırken servisleri eklerken AddScoped, AddTransient, AddSingleton metotlarla servislerin yaşam süresini belirtiyoruz ve kaydını DI'ya ekliyoruz.

Yani aynı Fluent Validasyon yaparken AddValidatorsFromAssemblyContaining metoduna ister tek tek validasyon sınıflarını yazmak zorundaydık ya eskiden ama şimdi bu metodda reflection ile bir

Sınıf veriyoruz bir tip yani o ilgili o sınıfn assemblysini reflection kodu ile ulaşıyor ve Abstract Valditor dan kalıtım almış bütün sınıfları DI container'a ekliyor. 

yani

```csharp
builder.Services.AddValidatorsFromAssemblyContaining<CreateReviewValidator>();
```

```csharp
typeof(SomeType).Assembly
```

 reflection ile o assembly içindeki tüm `IValidator<T>` implementasyonlarını buluyor.

containg metoduna bir tane sınıfı verisen tipi yani reflection ile arkada o sınınıfn Assemblysine ulaşılacak ve ne kadar AbstractValidator dan kalıtım alnımış bütün sınıfları kaydetmeyi yapacak DI Container'a böylelikle tek tek bütün sınıfları eklemeyecen.

###### <font color="#92d050">AddScoped</font>

```csharp
services.AddScoped<TService, TImplementation>();
```

- Her HTTP request (istek) boyunca tek bir instance üretir.
    
- Aynı request içinde birden fazla yerde çağrılsa da aynı obje kullanılır.
    
- Yeni bir request geldiğinde yeni bir instance oluşur.

<font color="#a5a5a5">Kullanım yerleri : </font>

çoğunlukla AddScoped kullanılır zaten çünkü normalde DI olmadan da biz doğrudan controllerlara new yazdığımızdan sayfayı yenilediğinde tekrardan bir nesne üretilmez mi üretilir.

aynı işlemi AddScoped'da yapıyor her requestte yeni bir istance üretip bir öncekini Dispose ediyor.

###### <font color="#92d050">AddSingleton</font>

- Uygulama çalıştığı sürece tek bir instance oluşturur.
    
- İlk ihtiyaç duyulduğunda yaratılır ve hep aynı obje kullanılır.

  İster sayfayı yenile diğer sayfaya geç istediğini yap program kapanmadı sürece aynı nesne üzerinden işlemler yapılacak.
 
<font color="#a5a5a5">Kullanım yerleri:</font>

Değişmeyen, ortak kullanılan servisler

###### <font color="#92d050">AddTransient</font>

Resolve demek =>  DI Container'dan talep edilen servisin instancesi oluşturulup enjete edilmesi işlemidir.

- Constructor servisi çağırdığında (yani resolve edildiğinde) yeni bir nesne gelir. 
    
- Yani servisi 10 kere çağırırsan, bellekte 10 farklı nesne üretilir.

##### <font color="#92d050">Örnek Tüm Yaşam Sürelerini Canlı Görelim </font>

```csharp
builder.Services.AddScoped<IRandomService, RandomService>();
```

```csharp
namespace WebApplication1
{
    public interface IRandomService
    {
        int GetNumber();
    }
}

public class RandomService : IRandomService
{
    private readonly int _number;
    public RandomService()
    {
        _number = new Random().Next(1, 1000);
    }
    public int GetNumber() => _number;
}
```


```csharp
public class HomeController : Controller
{
    private readonly IRandomService _random1;
    private readonly IRandomService _random2;

    public HomeController(IRandomService random1, IRandomService random2)
    {
        _random1 = random1;
        _random2 = random2;
    }

    public IActionResult Index()
    {
        ViewBag.Random1 = _random1.GetNumber();
        ViewBag.Random2 = _random2.GetNumber();

        return View();
    }
}
```


```csharp
<p>1: @ViewBag.Random1</p>
<p>2: @ViewBag.Random2</p>
```

###### <font color="#92d050">AddScoped ile olacaklar</font>

Çıktı olarak iki tarafta da aynı değeri görecen sebebi şu :

AddScopedda ne dedik IRandomService istendiğinde RandomService sınıfnındna nesne oluştur demedik mi dedik ama AddScoped olan bir sınıf sadece bir kez nesne oluşturulur bir requestte.

ondan dolayı Home controllerın constractırında iki tane talep olabilir ama iki tane nesne oluşmaz bir kez oluşur RandomService constracıtır bir kez çalışır

bir sayı üretir mesela 20 bunu alır talep edilen bütün yerlere bu nesneyi verir bundan dolayı

iki referans da aynı nesneyi işaret ediyor cevaplar aynı çıktı bundan dolayı.

yani sayfa yenilenene kadar oluşan tek bir nesneyi kullanabiliyorsun.

###### <font color="#92d050">AddSingleton ile olacaklar</font>

Uygulama boyunca ekranda iki değerde de aynı değeri yazar sayfayı yenile yak yık diğer sayfaya geç asla yeni bir nesne oluşmaz ve bundan dolayı sayılar ekranda hep aynı kalır.

yani bir kez nesne oluşuyor uygulama boyunca o nesne kullanılıyor.

###### <font color="#92d050">AddTransient ile olacaklar</font>

Ekrandaki sayılar farklı çıkacak çünkü AddTransient ile tanımlanan servisler Her `resolve` edildiğinde **yeni bir instance** oluşturulur.

resolve demek DI Container'dan talep edilen servisin instancesi oluşturulup enjete edilmesi işlemidir.

yani DI daki servis talep edildiğinde sürekli nesne oluşacak her tablep edene al sana nesne diyoruz.

böylece her iki değer de farklı çıkacak çünkü constructor'a gelen hep farklı nesneler olduğu için.

###### <font color="#92d050">Yaşam Sürelerinin Özeti</font>

- Scoped → sayfa yenileyince değişiyor ama aynı request içinde hep aynı.
    
- Transient → aynı sayfa içinde bile iki farklı alan farklı sayı alıyor.
    
- Singleton → uygulama restart etmeden hiç değişmiyor, hep aynı sayı.

##### <font color="#92d050">Örnek UdemyCarbook GenericStatistics</font>

```csharp
builder.Services.AddScoped<GenericStatistics>();
```

```csharp
 public class AdminStatisticsController : Controller
 {
     private readonly GenericStatistics _statistics;

     public AdminStatisticsController(GenericStatistics statistics)
     {
         _statistics = statistics;
     }

     public async Task<IActionResult> Index()
     {
         await _statistics.setViewBagData<ResultStatisticsDto>
           ("Statistics/GetCarCount",
            "CarCount", ViewData, x => x.CarCount);

         await _statistics.setViewBagData<ResultStatisticsDto>
         ("Statistics/GetLocationQuery",
          "LokasyonCount", ViewData, x => x.LocationCount);

```

Burada ister AddScoped kullan, ister diğer yaşam süreleri metodlarını (Transient, Singleton) kullan, hepsi aynı sonucu verir. Çünkü GenericStatistics sadece bir kez DI Container’dan resolve ediliyor:

```csharp
AddScoped => request boyunca tek nesne
Transient => controller inject sırasında tek nesne
Singleton => tüm uygulama boyunca tek nesne
```

Anlaşıldığı üzere, GenericStatistics sınıfı DI Container'dan yalnızca bir kez servis istediği için, hangi yaşam süresi kullanılırsa kullanılsın bir nesne oluşturulacağından dolayı sınıfın içindeki metoda ulaşacak mıyız evet bize bu yeterli burada ondan sonra ulaşılan metodu 20 kez çağrıp kullandık yani bir nesne üzerinden 20 kez metod çağırdık.

##### <font color="#92d050">DI Container'da İki overload var</font>

AddScoped , AddTransient , AddSingleton metotlarının iki farklı overload'u vardır.
###### <font color="#6495ed">1. Type overload (Generic Olmayan Metod)</font>

```csharp
Services.AddScoped(typeof(IAppRoleRepository),typeof(AppRoleRepositories));
builder.Services.AddScoped(typeof(IRepository<>), typeof(Repository<>));
```

Bu overriede bizden Type türü bekliyor. iki seçeneğin var:

<font color="#4bacc6">Typeof</font> => Compile time'de çalışır çünkü nesne üretmeden direk tip verirsin.

<font color="#4bacc6">GetType </font>=> Object sınıfının bir proprty'si olduğundan nesne üretmek gerekli run time'da nesne üretileceğinden getType run time'da çalışır.

Ama biz typeof kullanacağız bu metotda çünkü GetType kullanabilmen için nesne üretmen gerekiyor nesneyi DI üretmeyecek mi zaten ben üreteceksem o zaman DI kullanmama ne gerek var öyle değil mi.

Zaten GetType kullanamazsın DI Container'da çünkü nesne oluşturmamız gerekecek AppRoleRepositoryden ama onun constractrında bir Paremetre var DI anlasın diye interfaceyi yazdık ya onu dolduramazsın program.cs de ondan Typeof kullanacağız.


<font color="#d8d8d8">Murat Yücedağın yaptığı gibi yapma</font>

```csharp
builder.Services.AddScoped(typeof(IAppRoleRepository), typeof(AppRoleRepositories));
```

çünkü aynı sonuç tamam da generic tip overridesi daha sağlıklı çünkü sen buraya int verebilirsin hata vermez ama generic tip olsa new constraint olacağından hemen hata verir daha güvenli yani tip kontrollü yapar.

<font color="#4bacc6">Kullanım Senaryosu:</font>

Bazen hangi implementasyonun kullanılacağını runtime'da karar vermek isteriz.

Generic versiyon bunu yapamaz çünkü generic paremetre tip'i derleme zamanında çalışır.

<font color="#4bacc6">Generic Tiplerde generic olmayan metodu kullanamak zorundasın</font>

Çünkü generic tip'in derleme zamanında alacağı tip'in belli olması lazım ama generic bir tip daha alacağı tip belli değil DI'da ondan hata verir.

Mesela

```csharp
builder.Services.AddScoped<IRepository<>, Repository<> >();
```

Hata verir çünkü generic bir metod içinde generic bir tip tanımlaması yapmışın ama tip'ini vermemişin böyle bir kullanım yok o tip hemen verilecek compile time'de çözülecek ama sen daha tip'i bilmiyorsun oraya customer mı gelecek product mı gelecek ondan bu kullanım hata verecek.

O zaman bizim Type overridesini kullanmamız gerekecek.

```csharp
builder.Services.AddScoped(typeof(IRepository<>), typeof(Repository<>));
```

```csharp
public class CustomerController
{
    private readonly IRepository<Customer> _repo;

    public CustomerController(IRepository<Customer> repo)
    {
        _repo = repo;
    }
}
```

DI Container controller'ı oluştururken constructırına bakar reflection ile`IRepository<Customer> repo` var bakıyor kayıtlarına benim kayıtlarımda buna uyan bir şablon varmı diye `IRepository<>` ile kıyaslıyor evet diyor o zaman Customer tipini ver içi boş olan tip'e diyor.

###### <font color="#6495ed">2. Generic overload (Generic Metod)</font>

```csharp
builder.Services.AddScoped<IUserService, UserService>();
```

Generic overload kullanabilmen için elinde somut tipler olmalı IUserService ve UserService gibi. compile time'de tipleri belli olduğundan generic tip overload'ını kullanabilirsin.

Generic overload daha güvenlidir çünkü generic tipler compile time'da belli olacağından hata olduğundan uyarı verebilir mesela int türünden tip verdi generic tip olduğundan new constraint varsa anında hatayı çakar güvenli olur böylece.

ama normal overide'yi kullansaydın tip kontrollü yapamazdın.

Kısaca generic overload bizim normal metod overiedesini aynısı sadece onuun generic hali böylelikle hatayı daha erkenden compile time'da almamızı sağlar bize güvence verir.

<font color="#d8d8d8">Arkadaki kodu</font>

```csharp
  public static IServiceCollection AddScoped<TService,TImplementation>(
      this IServiceCollection services,
      Func<IServiceProvider, TImplementation> implementationFactory)
      where TService : class
      where TImplementation : class, TService
  {
      ThrowHelper.ThrowIfNull(services);
      ThrowHelper.ThrowIfNull(implementationFactory);

      return services.AddScoped(typeof(TService), implementationFactory);
  }
```

Generic overload aslında Type overload (Normal metod)'u içinde barındırır. 

```csharp
 return services.AddScoped(typeof(TService), implementationFactory);
```

Kaynak koduna bakarsan Generic metodumuz içinde aslında normal metodu barındırıyor sadece tek farkı bizden generic tip istiyor.

alıyor bu tipleri TService tip'i IUserService, UserService tip'i ise implementationFactory oluyor metod içinde sonra normal metodu çağırıyor bu metod biliyorsun type istiyor ondan typeof operatorünü kullanıyorsun.

AddScoped'a gidip int verisen hata vericektir çünkü where T : Class kısıtlaması var gelen tip int, class olmadıgından hata alırısın compile time'de.

<font color="#4bacc6">Kullanım Senaryosu:</font>

Tipleri compile time'de biliyorsan generic overridesi kullanılmalı buda içinde normal metod overiedesini o çalıştıracak. 

###### <font color="#6495ed">Aralarındaki fark</font>

| Özellik    | Generic (<TService, TImplementation>) | Type overload (typeof)    |
| ---------- | ------------------------------------- | ------------------------- |
| Tip kararı | Compile-time                          | Runtime                   |
| Esneklik   | Sabit, hızlı                          | Dinamik, runtime kontrolü |

 Generic metod, daha kısa yazım, ama sadece kapalı tiplerde (Product,Caregory gibi generic olmayan tipler)'de işe yarar. 
    
Generic olmayan metod ise, daha genel, yani hem kapalı hem açık tipleri (generic tipler) de destekler.

Aslında generic overload sadece bir syntactic sunar. İçerde yine generic olmayan metodu çağırıyor.

##### <font color="#92d050">DI Container Nasıl Nesne Üretir</font>

Önce DI Container'a kayıt eklemen lazım.

```csharp
services.AddScoped<IDeneme, DenemeRepository>();
```

Aslında Arkada bu şöyle tutuluyor

```csharp
// ServiceCollection içinde bir liste var
var serviceDescriptor = new ServiceDescriptor
{
    ServiceType = typeof(IDeneme),      // Interface
    ImplementationType = typeof(DenemeRepository), // Concrete class
    Lifetime = ServiceLifetime.Scoped       // Yaşam döngüsü
};

// Bu descriptor bir listeye ekleniyor
_serviceDescriptors.Add(serviceDescriptor);
```

yani biz IDeneme tipini veriyoruz gidiyor bu metoda 

```csharp
public Type ServiceType { get; }
public Type ImplementationType { get; }
```

diye bir propertyler Type olarak tanımlanmış yani Type bir abstract class tı RuntimeType diye bir classdan instance oluşturuluyordu ama biz doğrudan bunu yapamaıyorduk çünkü Type bir abstract class olduğundan nesne oluşturulamaz. nesne oluşturulacak class'ı RuntimeType classı ama bu class'a da internal olduğundan bizim erişimiz yok bulunduğu namespace de erişilebilir. bunun için bu erişmeyi typeof operatörü arkada bizim yerimize yapıyordu.

yine öyle olmuş arkada Type olarak tanımlandıkları için propertylere değer atanırken typeof diyerek instance oluşturulup atanma işlemi yapılmış.

sonra constractırlarına bu IDeneme , IMyClass, IMyclass2'yi referans olarak belirtmen lazım.

```csharp
  public class ProductController : Controller
  {

      private readonly IDeneme _IDeneme;

      public ProductController(IDeneme IDeneme)
      {
         _IDeneme =IDeneme;
      }
  }
```

bu şekilde yazmamız gerekiyor ama aklıma takılan sorular var tamam yazdıkta adam bundan nasıl nesne üretecek arkada sonra nasıl benim constractırımı nasıl bulacak ne istediğini nereden bilecek gibi güzel sorular.

```csharp
builder.Services.AddControllers();
```

Bu metod net coredeki Mvc mimarisinin bir sürü özelliğini kullanmamızı sağlıyordu model binding , routelerin çalışması gibi DI tarafınada özellik sunuyor tüm controller'ları otomatik DI'ya kayıt ediyor.

yani controller'ları DI'ya kaydetmene gerek yok sadece kendin oluşturduğun repositoryleri kaydetmen gerekir.

yani ASP.NET Core'un kaynak kodunda böyle bir şey var:

```csharp
 
public static IServiceCollection AddControllers(this IServiceCollection services)
{
    // 1. Assembly'den tüm Controller'ları bul
    
    var assembly = Assembly.GetExecutingAssembly();
    var controllerTypes = assembly.GetTypes()
        .Where(t => t.Name.EndsWith("Controller") && t.IsSubclassOf(typeof(Controller)));
    
    // 2. Her Controller'ı Transient olarak kayıt et
    
    foreach (var controllerType in controllerTypes)
    {
        services.AddTransient(controllerType);
         // HomeController, ProductController vs.
    }
    
    return services;
}
```

Birebir böyle değil sadece biz DI tarafındaki kodu inceliyoruz yoksa AddControllers içinde bir sürü kod var model binding , routingler gibi.

```csharp
// /Home/Index isteği geldi
// → HomeController lazım
// → DI Container: HomeController'ın constructor'ına bak
// → IUserService parametresi var
// → UserService'i analiz et ve oluştur
// → HomeController'ı oluştur
```

 DI Container'a kaydetme yaptık AddScoped , AddTransient , AddSingletion gibi metodlar sayesinde artık bizim nesne oluşturulacak sınıf belli sadece isteği bekliyor.

bunun için reflection yolu ile constractırlara bakıyor constractırların paremetresine bakıyor.

tip karşılaştırması yapıyor yani bakıyor bende olan kayıtlar ile şuan incelediğim sınıfın constractırında olan sınıf ismi bende varmı yokmu diye bakıyor bulursa şunu yapııyor.

```csharp
TService singletonInstance = new TImplementation();
```

nesnesi oluşacak ve reflection ile senin istediğin constractıra verilecek bu nesne paremetre yoluyla.

<font color="#d8d8d8">yani</font>

```csharp
new HomeController(singletonInstance)
```

tabiki bu reflection ile yapılacak böyle direk new olmayacak run time de nesne oluşturulacak ama temel mantık bu.

##### <font color="#92d050">Gerçek Örnekler</font>

```csharp
public class ProductService
{
    private readonly ProductRepository _repository; // concrete (somut) bağımlılık

    public ProductService(ProductRepository repository)
    {
        _repository = repository;
    }
}
```

şimdi biz eğer DI kullancağız diyorsak bu kesinlikle constractır da paremetre olarak class yazma neden mi çünkü classlar sık değişir yarın sen gidersin classın ismini değiştrirsin o zaman bu DI bile seni kurtalmaz tek tek isimlerini tespit etmek zorunda kalırsın ondan interfacelerin değişme olasılığı az olduğundan işi interfacelere bırakmak en doğrusu.

aslında  olay şu bu kodda Class olması değil sorun interface olsada sorun olur çünkü sen startup içinde bunu tek bir değer ile kaydedersen o tek bir değer değişince bunları da değiştirmen gerek bu zor olur ama sen gider startupda iki tane paremetre ile kaydedersen birisini tespit amaçlı kullanırsın diğerini ise nesne oluşturmak için kullanırsın böyle nesne oluşturulacak yer değişşe bile sorumluk ilk parmetrede olacagından sorun olmaz.

şimdi şunu netleştirek

```csharp
Person p = new Person { Name = "Ahmet", Age = 30 };
PrintPersonInfo(p);
```

önce burada

```csharp
Person p = new Person { Name = "Ahmet", Age = 30 };
```
burada p person türünde bir referans

```csharp
new Person { Name = "Ahmet", Age = 30 };
```
ise nesne oluşturma işlemi p ye atanıyor ilgil propertylerine değer atanarak ama.

```csharp
PrintPersonInfo(p);
```

burda ise referans'ı paremetre olarak verdik metoda.

```csharp
public void PrintPersonInfo(Person person)
{
    Console.WriteLine($"Name: {person.Name}, Age: {person.Age}");
}
```
referansımızı aynı türden yakalamk gerekir class'ın türü Person ise Person person diye yakaladık

ve tamam artık operasyonel işlemlerini yapaiblirsin.


ilk önce Startup da

```csharp
 services.AddSingleton<FakeData>();
```

bu kod ile dedik ki uygulama boyunca FakeData dan yalnızca bir tane new FakeData diyerek instance üret ve onu kullan ömrün boyunca dedik.


```csharp
    public class ProductController : Controller
    {

        private readonly FakeData _context;

        public ProductController(FakeData context)
        {
            _context = context;
        }

        public IActionResult Index()
        {
            return View();
        }
    }
```

constractır da paremetre olarak FakeDatayı verdik yani new oluşacak class'ı startup da belirttiğimiz sınıfı yani. 

ve arkada bir FakeData fake=new FakeData() diyerek bir nesne oluştu.

tamam şimdi geldik bunu kullanmaya sen bu nesneyi anca tanımladığın scoplar arasında kullabilirsin yani constractırdan başka yerde bu nesneye erişemezsin sen bunu classın geneline yayman için senin aynı türden bir referans oluşturup nesneyi referansa atayıp projeye yayman gerekir.

```csharp
 private readonly FakeData _context;
```

onu da bu satır yaptı işte bu Index içinde buna erişemezsin.


##### <font color="#92d050">Peki DI da GetType kullanılabilr mi</font>

```csharp
MyService service = new MyService();
services.AddScoped(service.GetType());
```

- `GetType()` runtime’da çalışır → yani önce `service` instance’ı yaratman gerekiyor.
    
- DI container amaçlarından biri nesne yaratmayı yönetmek, instance’ı senin yerine yaratmak.

  Eğer instance yaratıp `GetType()` verirsen, DI container artık nesneyi yönetemez, çünkü sen instance’ı zaten oluşturmuşsun.

  cevap : hayır kullanamazsın adamlar type tipi demiş tamam getType'de type döndürüyor da küçük bir ayrım var getType run time'de çalışıyor ve senden instance bekliyor.

  eğer instanceyi ben üretirsem o zaman AddScoped ne işe yararki değil mi ondan hata alırsın.

| Adım                 | Açıklama                                                             |
| -------------------- | -------------------------------------------------------------------- |
| `typeof` kullanımı   | Sadece tip bilgisini verir. Henüz nesne yok.                         |
| AddScoped aldığı tip | DI container’a “hangi tipten nesne üretileceğini” söyler.            |
| Runtime’da           | Container `MyService` tipinden nesne üretir, request boyunca saklar. |
| GetType kullanılsa   | Nesneyi hemen üretmek zorunda kalır, scoped mantığını bozar.         |


##### <font color="#92d050">s</font>

[[Type-TypeOf]]

[[Reflection]]
