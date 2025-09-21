
###### 1️⃣ namespace nedir?

- Sınıfları (`class`), arayüzleri (`interface`), enum’ları (`enum`) ve diğer yapıları gruplamak için kullanılır.
    
- Farklı kütüphanelerde aynı isimde sınıflar olsa bile çakışmayı önler.

```csharp
namespace MyCompany.Project.Models
{
    public class Personel
    {
        public int Id { get; set; }
        public string Adi { get; set; }
    }
}
```

- Burada `Personel` sınıfı, `MyCompany.Project.Models` namespace’inin içinde.
    
- Eğer başka bir namespace içinde de `Personel` sınıfı olursa, isim çakışmaz.

###### 2️⃣ `using` nedir?

`using`, **başka namespace’lerdeki sınıfları kısaca kullanabilmemizi sağlayan anahtar kelimedir**.

Uzun namespace yazmak yerine, başa `using` yazarız.

```csharp
using MyCompany.Project.Models;

Personel p = new Personel();  // Artık namespace yazmaya gerek yok
```

şimdi using olmassaydı namespacelerini belirtmek gerekirdi her satırda gidip personelle

```csharp
MyCompany.Project.Models.Personel p =new MyCompany.Project.Models.Personel();
```
derdik ama using ile sayfa genelinde geçerli olacak bir yol belirttik buradan direk kullanabiliyoruz.


###### Önemli Burası

```csharp
using System.ComponentModel.DataAnnotations;

namespace DI.Models
{
    public class Personel
    {
        public int SicilNo { get; set; }
        public string Adi { get; set; }
        public string Soyadi { get; set; }
        public string Resim { get; set; }

    }
}
```


mesela burada sen gideceksin Category sınıfnı yazaksın diyerim o zaman using'e ihtiyaç yok çünkü sen zaten o sınıf ile aynı namespace altındasın yani aynı klasördesin ondan namespace ve using gerekmez farklı klasörler de ise işte o zaman yolunu belirtmen gerekir.

ama sadece aynı klasörde ise eğer o klasörün içinde bile klasör varsa olmaz yolu belirtmen gerekir.


```csharp
namespace ConsoleApp1
{
    class Program
    {
        static void Main(string[] args)
        {
            A a=new A();
            B b=new B();
            C c=new C();

            Console.ReadKey();
        }
    }
}
```

```csharp

namespace ConsoleApp1.NewFolder
{
    public class A
    {

    }
    public class B
    {

    }
}

public class C
{

}
```

şimdi dikkat Edersen 

1. A ve B neden hata verdi?

`A` ve `B` sınıfları `ConsoleApp1.NewFolder` isim alanı (namespace) içinde tanımlandı.  
Ama `Program` sınıfın `ConsoleApp1` isim alanında.

Dolayısıyla `Program` içinden `A` ve `B` kullanmak için şunlardan birini yapman gerekiyor:

- `using ConsoleApp1.NewFolder;` eklemek
    
- veya `ConsoleApp1.NewFolder.A a = new ConsoleApp1.NewFolder.A();` şeklinde tam yol (fully qualified name) yazmak.

 2. C neden hata vermedi?

`C` sınıfı hiçbir namespace içinde değil → yani _global namespace_ içinde.

C#’ta isim arama sırası şöyle işler:

1. Aynı namespace içindekiler aranır.
    
2. Eğer bulunmazsa global namespace içindekiler aranır.
    

`Program` sınıfı `ConsoleApp1` namespace’indeydi, ama `C` globaldeydi.  
Global namespace her yerden erişilebilir olduğu için `C` doğrudan bulundu → hata vermedi ✅