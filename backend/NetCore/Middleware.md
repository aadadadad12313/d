Middleware, gelen HTTP isteğini işlerken uygulama içinde sırayla çalışan **ara yazılım bileşenleridir**.

ASP.NET Core'da middleware'ler `Startup.cs` dosyasındaki `Configure` metodunda tanımlanır:

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseRouting();         // Middleware: route eşleştirme
    app.UseAuthentication();  // Middleware: kimlik doğrulama
    app.UseAuthorization();   // Middleware: yetkilendirme
    app.UseEndpoints(...);    // Middleware: endpoint çalıştırma
}
```

Kısaca Middleware, HTTP istekleri geldiğinde sırayla çalışan ve isteği işleyip yanıtı şekillendiren yazılım katmanlarıdır.

bu midlleware şunu yapar gelen URL'leri analiz edip hangi endpoint'e gideceğini belirleyen middleware'dir.