
onemli:: IEnumerable 


```csharp
model IEnumerable<DI.Models.Personel>
```

IEnumerable Listelerin atası çok gelişmemiş sadece okuma yapar yani çok mantıklı çünkü List kullanmak demek add delete update remove gibi bir sürü metodu properti'si var.
ama sen sadece Index de listeleme yapmak istersen ne gerek var List tanımlayıp kodu şişirmeye Listenin en temeli bile bize yeter bu durumda.

yani demek istenen

```csharp
public class List<T> : IEnumerable<T>, ICollection<T>, IList<T>, ...
```

zaten List classının bütün özelilikleri kalıtım aldığı interfacelerden geliyor yani gidip

```csharp
List<Personel> s =new List<Perosnel>();
```

yapmaktansa referans kısmını IEnumerable yaparsak sadece bu interfacenin metotları,propertyleri gerir o zaman tam istenen durum olur sadece okuma yaparız yani içinde okuma kodlarında baika create gibi metodları olamaz IEnumerablenin.

```csharp
IEnumerable<Personel> s =new List<Perosnel>();
```

#### <font color="#0070c0">Mvc Örneği</font>

```csharp
public class FakeData
    {

        public List<Personel> personeller = new List<Personel>()
        {
            new Personel { SicilNo = 1001, Adi = "Ahmet", Soyadi = "Yılmaz",              Resim = "https://images.unsplash.com/photo-1507003211169" },
            new Personel { SicilNo = 1002, Adi = "Ayşe", Soyadi = "Kaya",                 Resim = "https://images.unsplash.com/photo-1494790108755" },
        };
```


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
            return View(_context.personeller);
        }
    }
```

Cshtml view'inde

```csharp
@model IEnumerable<DI.Models.Personel>
```

şimdi bu örnektte dersen eğer 

Ben Listeleri List classından tanımladım ama gittim modele IEnumerable interfacesini verdim hata vermezmi hayır vermez.

çünkü List zaten IEnumerable interfacesini implement eder.

Biz Listeden nesne oluşturduk List ile tamam
```csharp
List<Personel> liste = new List<Personel>();
```

sonra model de bunu 

```csharp
IEnumerable<Personel> siraliListe = liste;
```

böyle yaptık yani model de böyle düşün oluşan list nesnesini IEnumerable referansına verdik hata vermez yani burada Cast işlemi yok tip uyumu var . 

yani List implement aldığı interfacelere direk atanabilir tür farklılığı olmaz zaten List demek o tür demek olduğundan sorun olmaz.

Tek bu durum IEnumerable için de geçerli değil ICollection da List'i implement ediyor @model diyerek onu da yazabilirdin yani hepsi aynı işlemi yapar.
