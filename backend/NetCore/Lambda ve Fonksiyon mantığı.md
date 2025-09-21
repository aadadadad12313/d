
Lambda ile

```csharp
services.Configure<RouteOptions>(options =>
{
    options.LowercaseUrls = true;
    options.AppendTrailingSlash = false;
});
```

 Lambda Olmadan (Ayrı Metot)

```csharp
// 1) Önce bir metot tanımla
private void MyRouteConfig(RouteOptions options)
{
    options.LowercaseUrls = true;
    options.AppendTrailingSlash = false;
}

// 2) Sonra ConfigureServices içinde kullan
services.Configure<RouteOptions>(MyRouteConfig);
```

![[Pasted image 20250814012301.png]]

burası bizden bir RouteOptions döndüren bir metod istiyor ister bunu lambda ile yaparsın ister gidip ayri bir fonksiyon tanımlayıp yaparsın zaten orada yazıyor hangi paremetreyi kullacan.


Örnek 2

```csharp
        services.AddCors(opt =>
        {
            opt.AddPolicy("CorsPolicy", ConfigureCorsPolicy); // Lambda yok, metod referansı
        });
    
    // Ayrı metod - CorsPolicy konfigürasyonu
    private static void ConfigureCorsPolicy(CorsPolicyBuilder builder)
    {
        builder.AllowAnyHeader()
               .AllowAnyMethod()
               .SetIsOriginAllowed(IsOriginAllowed)
               .AllowCredentials();
    }
```

Zaten bir metod sana ne döndürdüğü kod yazarken ekrana geliyor diyor adam CorsPolicyBuilder istiyorum ister lamda ile hiç uğraşmadan yaparsın yada ayrı bir fonksiyonda istenen classdan paremetre alıp gerekn işlemleri yürütürsün orası sana kalmış.


bak ne buldum buraya dikkat et

`Action<T>` .NET'teki bir **delegate türü**. 
yani yukaradki kodlardarın paremetre açıklamasında action<T> yazıyordu meğer bu bir void döndüren delageteymiş oradan hemen void olduğunu tanımlayacağın fonksiyonda anlıyorsun.

`Action` = **Hiçbir şey döndürmeyen** (void) ama **parametre alabilen** bir delegate türü.