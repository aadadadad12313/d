
##### <font color="#f79646">Program.cs</font>

Program.cs dosyası aslında bir konsol uygulaması oluşturduğumuzdaki isim ve içinde main var

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;

namespace WebApplication1
{
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateHostBuilder(args).Build().Run();
        }

        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                });
    }
}
```

`CreateHostBuilder()` metodu, ASP.NET Core uygulamasını **çalıştıracak olan web sunucusunu (Kestrel)** ve **uygulama altyapısını (Startup.cs)** **ayağa kaldırır (başlatır)**.

Program.cs İcerisinde ayaga kaldırılacak hostun kullanacağı konfigurasyonlar neleden alacağını bildirmektedir.

##### <font color="#f79646">Startup</font>

**`Startup.cs`** dosyası, ASP.NET Core uygulamalarında uygulamanın **başlangıç yapılandırmalarının yapıldığı** önemli bir dosyadır. Bu dosya, uygulamanın nasıl başlatılacağını ve yapılandırılacağını belirler.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

namespace WebApplication1
{
    public class Startup
    {
        public void ConfigureServices(IServiceCollection services)
        {
        }

        public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }

            app.UseRouting();

            app.UseEndpoints(endpoints =>
            {
                endpoints.MapGet("/", async context =>
                {
                    await context.Response.WriteAsync("Hello World!");
                });
            });
        }
    }
}
```

ConfigureServices metodu bu uygulamada kullanilacak servislerin eklendiği/konfigure edildiği metottur.

Configure metodu da uygulamada kullanılacak middleware/ara kkatmanlari/ara yazilimları çağırmaktayız.

AppSetting.json dosyası statick metinleri örneğin veri tabanı bağlantısı gibi static metinleri appSetting'e yazacak ve heryerden ulaşabileceksin böylece heryere tanımlama ihtiyacın olmayacak.


##### <font color="#f79646">Özetle</font>

Program.cs sayfası sunucuyu kuruyor
Startup.cs ise servisleri yapılandırıyor.

