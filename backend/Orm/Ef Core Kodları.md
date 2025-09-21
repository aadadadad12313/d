
From Nedir ?

```csharp
from cp in _context.CarPricings
```

SQL de karşılığı

```sql
SELECT * FROM CarPricings AS cp
```


<hr>

🧱`select` Nedir?

LINQ'de `select`, sorgunun sonucunda hangi verilerin döneceğini belirtir.

```csharp
  select new CarPrincingViewModel
  {
      ModelName = g.Key.AracModeli,
      BrandName = g.Key.Brand,
  };
```

SQL de karşılığı

```sql
SELECT
    b.Name + ' ' + c.Model AS ModelName,
    ...
FROM ...
```

Ama SQL'de doğrudan `CarPrincingViewModel` gibi bir nesne oluşturulmaz. SQL sadece veri döner; nesne oluşturma işi C# tarafında yapılır.


```csharp
return _context.Comments.Select(x => new Comment
{
    CommentId = x.CommentId,
    CreateDate = x.CreateDate,
    Description = x.Description,
    Name = x.Name
}).ToList();
```

bu kullanım da neden Select kullanıyoruz dersen asıl sebebini anladım sonunda cevap şu sen select kullanmayıp ToList ile de direk listeleme yapardın ama asıl neden şu select kullanmanın

Select bize seçim sağlıyor yani bir Comment tablosunda bulunan Name propertysini alma diyebilirsin yada al diyeblirsin gibi eğer Select olmassa ToList olsaydı tablo da ne var ne yok bütün propertyleri alırdı ama select ile seçim yapıyorsun mesela navigation propertylerini katmıyoruz gibi.


<hr>

```csharp
join c in _context.Cars on cp.CarId equals c.CarId
```

SQL de Karşılığı

```sql
INNER JOIN Cars c ON cp.CarId = c.CarId
```

1️⃣ `join` ne demek?

 iki veri kaynağını birbirine bağlamak için kullanılır.

2️⃣ `on cp.CarId equals c.CarId` ne yapıyor?

- `cp.CarId` → `CarPricings` tablosundaki araba ID’si
    
- `c.CarId` → `Cars` tablosundaki araba ID’si
    
- `equals` → C#’da karşılaştırma yapıyor, **SQL’de eşittir = ile aynı işlemi yapıyor.
