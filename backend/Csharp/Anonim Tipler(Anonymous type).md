
```csharp
var people = new List<object>
{
   new { Id = 1, Name = "Ahmet", Age = 30 },
   new { Id = 2, Name = "Mehmet", Age = 25 },
   new { Id = 3, Name = "Ayşe", Age = 28 }
};


foreach (var person in (List<dynamic>)people)
{
    Console.WriteLine(person.Name);
}

```

object foreach de türü belli olmadığından patlar cast lazım tek dönüşecek tip dynamic başkada olamazdı sınıf sistemi varya genel bir tip lazım bize bir var var ama o da bu iş için tasarlanmadı hatayı yersin tek yöntem dynamic kullanmak ve böyle run time'ye bırakır orada çalışır bu kod.

ama bence zorunlu değilsen Listyeyi object tanımlama çünkü gereksiz şekilde cast ile uğraşırsın bundan gidip Listeyi dynamic ile tanımlarsak cast işlemi olmadan rahatına bakarsın.

Hatta Net Coredeysen ViewBag'in kendi türü Dynamic olduğundan Listede Dynamic ise cast işlemi gerekmez direk ViewBag ile tanımlarsın foreach'i. eğer object ise liste mecburen Cast yapacaksın.

ViewData veya Tempdate varsayılan Kendi Türleri object olduğundan cast zorunlu  yani

```csharp
     public IActionResult Index()
     {
         var list = new List<dynamic>
        {
            new {Name="Serkan",Surname="Altunay"},
            new {Name="Serkan",Surname="Altunay"},
            new {Name="Serkan",Surname="Altunay"},
            new {Name="Serkan",Surname="Altunay"},
        };

         ViewData["personeller"]=list;
         return View();
     }
```

Bu kullanım dynamic türde bir List içinde tipi belli olmayan anonim nesne oluşturma işlemidir.

```csharp
@foreach (var item in ViewData["personeller"] as List<dynamic>)
{    <h1>@item.Name</h1>
}
```

Listte tamam dynamic de viewData object olduğundan cast gerekli dynamic'e çevirdik bizde objectten. yada şunu yapardın as yazmaya uğraşma viewData ve viewBag'e verilen isimler birbirinde çalışır çünkü aynı yerde tutuluyor bunlar viewBag viewDatanın daha kısa yazım tarzı aynılar birebir herşeyleri ondan sen personeller dediysen viewData da viewbag.Personeller dersen çalışır direk ondan viewbag türü dynamic olduğundan as dynamic yapmana gerek yok.

```csharp
        public IActionResult Index()
        {
            var list = new List<dynamic>
           {
               new {Name="Serkan",Surname="Altunay"},
               new {Name="Serkan",Surname="Altunay"},
               new {Name="Serkan",Surname="Altunay"},
               new {Name="Serkan",Surname="Altunay"},
           };
            TempData["personeller"] = list;
            return View();
        }
```

```csharp
@foreach (var item in TempData["personeller"] as List<dynamic>)
{    <h1>@item.Name</h1>
}
```
Tempdatanın kendi türü de Object olduğu için cast lazım dynamic'e çevirmen gerekek başka tür olmaz anonim tip ya object ya dynamic olur object olamaz cast lazım cast içinde bir tür lazım o da dynamic olur.  Listenin türü object olsa da farketmez sonuçta en son cast ile dynamic olmuyor mu oluyor sorun yok yani.

Bu kodlar Consol Programı ile yapılmıştır

```csharp
using System;

List<object> people = new List<object>()
{
    new {Name="Serkan",Age=30},
    new {Name="Ahmet",Age=45},
    new {Name="Murat",Age=20}
};

foreach(var item in people)
{
    dynamic person = item;//dönüşüm gerekiyor object oldugundan
    Console.WriteLine($"Name: {person.Name}, Age: {person.Age}");
}
```

ama object ile değilde dynamic ile yapsaydık tür dönüşümüne ihtiyaç kalmazdı ama pek güvenli değil bunu dynamic yapmaakta

```csharp
var satisListesi = new List<dynamic>
{
    new { SatisID = 123, SatisTarihi = DateTime.Now, SatisTutari = 300.50 },
    new { SatisID = 124, SatisTarihi = DateTime.Now.AddDays(1), SatisTutari = 150.75 }
};

foreach (var satis in satisListesi)
{
    Console.WriteLine($"SatisID: {satis.SatisID}, SatisTutari: {satis.SatisTutari}");
}
```

onemli:: anonim tip 

```csharp
List<Person> people = new List<Person>
{
    new Person{Name="Serkan",Age=20},
    new Person{Name="Ahmet",Age=20},
    new Person{Name="Vural",Age=80},
    new Person{Name="Sinan",Age=40},
};

//Kısacası biz list türünü sınıf ile tanımlayıp böyle bir anonim tip oluşturuyoruz eğer böyle yapmasaydık object ile türü işaretlerdik bu da cast ile tür dönüşümü gerektirildi.

foreach(Person person in people)
{
    Console.WriteLine($" {person.Name} {person.Age}");
}

public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
}
```


Yani

- `object` her tür veriyi tutabilir ama **özelliklere (property) erişim compile-time'da bilinmez.**
    
- Derleyici `object` türünden `satis.SatisID` diye bir özelliğin olup olmadığını bilmediği için hata verir.
    
- Sadece `dynamic` kullanırsan bu erişim mümkün olur ama bu da **tür güvenliğini kaybettirir**.

En doğru ve en güvenili böyle anonim tip durumlarında sınfıtan türetmek daha doğrudur.


