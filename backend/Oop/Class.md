
###### <font color="#92d050">Konu Anlatımı</font>

Benim bir yaşım var 20 bu bir değer iken benim ismin yaşım doğduğum yer gibi alanlar bir araya geldiğinde nesne oluşur. 

Stack de Değer türlü değişkenler ve Referanslar tutulurken Heap de ise Nesne,obje,string tutulur.


Ben bir developer olarak bellekteki stack de tutulan tüm değerlere erişebiliyorum ama heap'e erişim hakkına sahip diyiriz yani nesneler hapis tutuluyor bunu çıkarmalıyız o hapisten.

Neden Referans Türlü değişkenler dendiği anlaşılıyor artık Class'lara Structlar gibi yapılara referans olmadan heap'e erişemiyoruz.

Şimdi biz bir developer olarak Heap'e erişemiyoruz ama Stack bunu yapabiliyor. dolasıyla biz Stack de heap'i işaret eden referanslar tanımlıyoruz bu referanslarda gidip bizim yapamadığımız şeyi yani heap'e erişmeyi bizim yerimize yapıyor.

Yani biz heap'e dolaylı yoldan erişiyoruz doğrudan değil. Önce Stack'e erişiyoruz Stack gidiyor Heap'e erişiyor.


Biz Nesneye ulaşabilmek için Nesnenin türü yani sınfın türünden bir tür ile referansımızı oluşturuyoruz.

```csharp
MyClass m; // Referans

m=new MyClass(); // m referansı ile MyClass türünde Nesne oluşturuldu.
```

Yani bize c# tarafından ön tanımlı gelen int , float gibi türler gibi class ile kendimiz türümüzü oluşturuyoruz.

buna referans türlü değişkenler denir.

Mesela hazır classları Kod yazarken o classların türünden bir tür ile karşılıyorsun işte sebebi bu Sınıf ismi demek aslında c# da tür oluyor.

Sen bir metoda eriştiysen class içinde o metodun türü ne sınıfın ismi yani sınıfnı ismi bir tür ise Myclass aslında bir int , float nasıl bir tür ise Myclass da bir tür yani.

yani hazır gelen int float double türü mü sence daha fazla yoksa proje de binlerce bulunan class,struct,interface,abstract class referans türlü değişkenler mi daha fazla.


Nesne Oluşturabilmek için önce bir modelleme yapman lazım onuda class ile yapıyoruz.

<font color="#92d050">Class da Neler Tutulur</font>

Propertyler , Fieldlar , Metodlar , Constractırlar

Biz önce bir class da kodları yazarız kodlarımız bitince artık bu kodlarımız çalışması lazım bundan nesne oluşturup kodlarımızı oluşturduğumuz nesneden çalışıtrıyoruz.


Classlar da güzel bir örnek var o da şu mesela Türkiye de herkesin bir kimliği var 80 milyon milletin yani herkes tek bir class dan türemiş bir nesne. 

herkes de aynı propertyler, fieldlar var ama bu nesnelerin içindeki veriler herkes de farklı birinde hilmi yazarken diğerinde başka bişey yazıyor ama fıtrat olarak bunlar aynı model den oluştu işte class tamda budur.


###### <font color="#92d050">Classlar 3 Yerde Oluşturulabilir</font>

<font color="#a5a5a5">1. Namespace içinde %99 bu kullanılır</font>

```csharp
namespace ConsoleApp1
{
    public class Program
    {
        static void Main(string[] args)
        {
        }
    }
    
    public class MyClass
    {
    }
}
```

<font color="#a5a5a5">2. Namespace dışarısında yani global olarak</font>

```csharp
namespace ConsoleApp1
{
    public class Program
    {
        static void Main(string[] args)
        {
        }
    }
}

  public class MyClass
    {
    }
```

Global class oldu bu Global class sadece aynı assembly içinde geçerlidir yani sadece tanımladındığı katmanda geçerlidir başka katmanda kullanılacaksa referans verirsen kataman o zaman yine erişileblilir tabikide.

<font color="#a5a5a5">3. Nested class yani iç içe class tanımlama</font>

```csharp
  public class Program
  {
      static void Main(string[] args)
      {
      }
      public class MyClass
      {
      }
  }
```


###### <font color="#92d050">Referans Oluşturma</font>

Bir sınıfın adı tür olarak kabul edilir ve referans oluştururken o tür ismi kullanılır.

```csharp
int a;
string b;
bool b;
```

Bunlar nasıl bir tür ise biz de bir tür oluşturacağız.

```csharp
MyClass m; // Şuan stack de MyClass türün m referansı oluştururdu. yani referans demek aslında class isminden bir değişken oluşturmak demek.
```

bitti MyClass türü oluşturduk bu aslında bir int float gibi bir tür yani önce stack de yerimizi aldık bunu yaparak.

sonra devam edip new kullanınca stack gidecek heap'den bizim classı kullanabilecek.

Eğer bir referans nesneyi referans etmiyorsa null değere sahip olacak.

###### <font color="#92d050">Önemli</font>

```csharp
  public class Osman
  {
      Context c = new Context();
      c. // erişemezsin
      
      public Osman()
      {
         c. // erişilir
      }
      
      public void Deneme()
      {
        c. // erişilir
      }
  }
```

Bir sınıftan nesne oluşturdun diyerim o nesnenin memberlarına anca constractır veya metod içinde olacaksın ki erişebilirsin gidip metod veya constructor dışında c . dersen memberları göremezsin.

Eğer oluşan referansı metotların dışıında kullanmaya kalksaydık gider consume etme sırasında dışarıya yazardık sonra direk kullanırdık bu java scriptte bu olurdu ama c# da böyle yok.

bundan sürekli generic yapmak için servisler yazıyoruz.

<font color="#92d050">Örnek</font>

```csharp
 public class AdminBannerController : Controller
 {
     public AdminBannerController(IHttpClientFactory httpClientFactory)
     {
        
     }
     public async Task<IActionResult> Index()
     {
         var client = httpClientFactory.CreateClient();
         return View();
     }
}
```

Hata alırsın çünkü httpClientFactory constractır içindeki referansı sen constractır içindeki yerden başka yerde kullanmazsın.

Index metodunda kullanabilmek için aynı türden bir referans oluşturacaksın heryerde kullancaksın.

yani

```csharp
 public class AdminBannerController : Controller
 {
     private readonly IHttpClientFactory _httpClientFactory;

     public AdminBannerController(IHttpClientFactory httpClientFactory)
     {
         _httpClientFactory = httpClientFactory;
     }

     public async Task<IActionResult> Index()
     {
         var client = _httpClientFactory.CreateClient();
         return View();
     }
}
```

`httpClientFactory` sayesinde Index metodunda sınıfnın memberlarına ulaşabilirsin.

###### <font color="#92d050">Metoda Referans verme</font>

| Tip           | Açıklama                                                                         |
| ------------- | -------------------------------------------------------------------------------- |
| Değer Tipi    | int, bool, double, struct gibi tipler. Metoda gönderildiğinde **kopyası** gider. |
| Referans Tipi | class, array, string gibi tipler. Metoda gönderildiğinde referansı gider.        |

- Metoda değerin bir kopyası gönderilir.
    
- Metodun içinde yapılan değişiklik, dışarıdaki orijinal değişkeni etkilemez.

Yani değer türlü değişkenlerde int float double strcut gibilerini kastediyorum bunların kopyası alınacağından ve bu kopya metodda kulalnılıp değişiklikler bu kopyaya uyaralancak ya bizim ana orjinal değişkenlerimiz değişmeyecek değişen şeyler kopya olacak.

ama referans türlü yani nesnelerde paremetre giderken kopya alınmaz referasn alınır bu da gönderdiğin referansı alacağından doğrudan metodda yaptığın bir değişiklik ana orjinal referansa yanısyacak.

```csharp
static void Main()
{
    int x = 10;
    Degistir(x);
    Console.WriteLine(x); // 10
}

static void Degistir(int sayi)
{
    sayi = 99; // sadece kopya değişti
}
```

x hala 10’dur, çünkü `sayi` parametresi x’in kopyasıdır.

gördüğün gibi sadece sayi 99 oldu gidip de x 99 olmadı yada sen sayi diye isim yerine gider paremetreye de x yazardın yani main de x ile metoddaki x aynı değil kopyalama mantığı varya ondan işte orjinale etki  edemez yani değer türlü değişkenler.

 Eğer parametre referans tipi (class, object, string, `List<T>` gibi):

- Metoda, nesnenin bellekteki referansı gönderilir.
    
- Yani metodun parametresi, dışarıdaki değişkenle aynı nesneyi işaret eder.
    
- Bu yüzden metodun içinde nesnenin özelliklerini değiştirirsen, dışarıdan da o değişiklik görülür.

```csharp
class Ogrenci
{
    public string Ad { get; set; }
}

static void Main()
{
    Ogrenci o = new Ogrenci { Ad = "Ali" };
    Degistir(o);
    Console.WriteLine(o.Ad); // "Veli"
}

static void Degistir(Ogrenci ogr)
{
    ogr.Ad = "Veli"; // aynı nesneyi işaret ediyor
}
```

Görüldüğü üzere nesneler referans türlü olduğundan kopya alınmadığından referansı alındığından metod içindeki bir değişiklik tüm orjinal kısmada etki ediyor.


```csharp
MyClass m = new MyClass();  // Yeni bir MyClass nesnesi oluşturuluyor
m.Id = 1;                   // Bu nesnenin Id özelliği 1 olarak ayarlanıyor
Deneme(m);                  // Bu nesne Deneme metoduna gönderiliyor yani referansı
```

Fonksiyona paremete gönderirken referansı yani bu referansın işaret ettiği nesneyi verdik.

dolasıyla metod paremetresinde bu referansın türü ile aynı olacak bir tür olmalı.

```csharp
  public static void Deneme(MyClass A)
  {
  }
```

ki öyle de oldu.

buradaki asıl mantık şu 

sen oluşturduğun bir nesneyi referans olarak verdin metoda.

metod paremetresinde ise yine bir referans var A referansı bu neyi işaret ediyor biliyormusun senin referans olarak verdiğin nesneyi yani şuan stack de

2 referans var m ve A  ve bunlar =>  iki referansda aynı nesneyi işaret ediyor.

yani m referansı kopyalandı birebir oldu A referansı sen gittin A referansı da operasyonel işlemlerde yaptın memberlara değer verdiğin anda bundan m referansıda etkilenecek neden peki


çünkü aslında A referansı diye bişey yok o aslında m referansı sen sadece paremetre olarka verdin onu mantık tamamen bu. sen gidip m.Id=1 sonrada m.Id=2 dersen nasıl en sondaki cevap 2 çıkarsa

aynısı m.Id=1 ve A.Id=2 dersen cevap 2 çıkar çünkü A.Id demek Aslında M.Id demek de ondan.


```csharp
   static void Main(string[] args)
   {

       MyClass m = new MyClass();
       m.Id = 1;

       Console.WriteLine(m.Id);

       Deneme(m);
       Console.WriteLine(m.Id);

   }

   public static void Deneme(MyClass m)
   {
       m.Id = 2;
   }
```

bak gördün mü kopyası olduğunun kanıtı önce nesne 1 oldu Id propertsi sonra metoda parmetre olarak aynı referansı verdik ve o metod çalıştı o metod nesnenin propertysini 2 yaptı sonra tekrar

m referansını kullandın 2 yazdı al sana kanıtı adam aynı adresi paremetre olaark verdi sonra gitti metod operasyonel işlem yaptı. 

###### <font color="#92d050">Özet</font>

Sağ tık yapıp class oluştururken hata alırsan bil ki sebebi dosya çakışmasıdır çünkü aynı dizinde aynı isimde dosya olamaz.

hadi dosyayı farklı isimlendirdin diyelim aynı dizin de yani namespace de aynı isimde class tanımlayamazsın sadece partial keyword’ü ile işaretlenmiş yapılar tanımlayabilirsin.
