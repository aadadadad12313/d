
## <font color="#0070c0">Paketlerin Anlamları</font>

📦 1. `Microsoft.EntityFrameworkCore.SqlServer`

- SQL Server ile Entity Framework Core’un konuşmasını sağlar.
    
- `DbContext` içinde `UseSqlServer()` metodunu aktif hale getirir.

bu metodu bize bu paket sağlıyor.

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder.UseSqlServer("Server=.;Database=MyDb;Trusted_Connection=True;");
}
```

📦 Microsoft.EntityFrameworkCore.Tools

bu paket package manage consol'a kendi kodlarını tanıtır. Komut tanımlayıcıdır yani. 


🛠️ 2. `Microsoft.EntityFrameworkCore.Design`

design paketi ise tools paketi consol'a kodlarını tanımladı artık update-database dediğinde ne kod olacağı consol ekranı biliyor design da ise enter'a bastıgında migration dosyasını bu paket oluşturacak yani tools kodları tanımladı design migration dosyasını hazırladı.

- **Ne sağlar?** Migration üretimi, `Add-Migration`, `Update-Database` gibi komutların arka planını.
    
- **Kodda doğrudan görünmez**, ama CLI ( yani komut arayüzü demek ) komutları çalışırken devreye girer. yani bu demek add-migration yazdın enter'a bastın onu diyor o zaman design paketi devreyi girer diyor.

- Migration dosyalarını oluşturmak için gerekli olan **design-time** araçları içerir.
    
- `Add-Migration`, `Update-Database` gibi komutlar bu paket olmadan çalışmaz.


```csharp
dotnet ef migrations add InitialCreate
dotnet ef database update
```


📦 Microsoft.EntityFrameworkCore  Paketi

**Ne sağlar?** EF Core’un temel sınıflarını: `DbContext`, `DbSet`, `ModelBuilder`, `SaveChanges`, `ChangeTracker` vs.

dbset veya dbcontext gibi bir sürü fonksiyonu bu paket aracığıyla katmanımıza ekliyoruz.
```csharp
using Microsoft.EntityFrameworkCore;

public class AppDbContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
}
```

yani 

```csharp
_context.Posts.Add(post); // EF Core'da
```

bizim crud metodları hep bu paketten geliyor.


şimdi gelelim bu paketlerin hangi zamanda çalıştıklarına

## 🧠<font color="#0070c0"> Runtime vs Design-Time Nedir?</font>

| Kavram          | Tanım                                                                                                                                  | Ne Zaman Olur?                        | Örnek Senaryo                                                     |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------- | ----------------------------------------------------------------- |
| **Runtime**     | Uygulamanın çalıştığı, kodun işletildiği zaman                                                                                         | Kullanıcı uygulamayı açtığında        | `DbContext.SaveChanges()`, `UseSqlServer()` gibi işlemler çalışır |
| **Design-Time** | Geliştirme aşamasında, kod yazarken veya CLI komutu çalıştırırken yani ClI demek komut arayüzü yani package consol ekranın kastediyor. | Kod yazarken veya migration üretirken | `dotnet ef migrations add`, `Update-Database` gibi komutlar       |


## 📦 <font color="#0070c0">Entity Framework Core Paketleri ve Çalışma Zamanları</font>

| Paket Adı                                 | Görevi                                                                      | Çalışma Zamanı     |
| ----------------------------------------- | --------------------------------------------------------------------------- | ------------------ |
| `Microsoft.EntityFrameworkCore`           | DbContext, DbSet, LINQ, Change Tracking gibi temel ORM bileşenlerini sağlar | ✅ Runtime + Design |
| `Microsoft.EntityFrameworkCore.SqlServer` | SQL Server veritabanı provider’ı. `UseSqlServer()` metodunu sağlar          | ✅ Runtime          |
| `Microsoft.EntityFrameworkCore.Design`    | Migration dosyaları oluşturmak için gerekli design-time araçları            | ⚠️ Sadece Design   |
| `Microsoft.EntityFrameworkCore.Tools`     | Visual Studio içi migration komutları ve EF komutlarının CLI entegrasyonu   | ⚠️ Sadece Design   |

Design ve tools zaten designer zamanı olmalı zaten çünkü sadece consol üzerinden çalışıyorlar ondan.

entityFramework zaten biliyorsun sürekli debugger attıgında muhakkak debugger seni Context sınınfına atıyor işte bundan runTime de çalışıyor bağlantıyı okuyor çalıştırıyor.


## <font color="#0070c0">Kritik</font>

Senin katmanının sürümü ne ise 8.0 ise sen gideceksin 8.0 sürümüne ait paketleri kuracaksın gidip de 5.0 kurmayacaksın aksi taktirde hatalar verir.
