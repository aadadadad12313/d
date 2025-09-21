
### <font color="#6495ed">This Keywordü</font>

```csharp

namespace ConsoleApp1
{
    class Program
    {
        static void Main(string[] args)
        {

            new MyClass("Serkan","Altunay");

            Console.ReadKey();
        }
    }

    public class MyClass
    {
        public MyClass()
        {
            Console.WriteLine("İlk bu çalısacak");
        }

        public MyClass(string a):this()
        {
            Console.WriteLine("İkinci bu çalışacak");
        }

        public MyClass(string a, string b):this(a)
        {
            Console.WriteLine("Üçüncü bu çalışacak");
        }
    }
}
```

ilk üç parametreli constructor tetiklenecek ama çalışmadan this(a) oldugundan bir paremetreli constrıctırı tetikleyecek o da çalışmadan this oldugundan paremetresiz constractırı tetikleyecek sonra çalışa çalışa en son tetiklenen üçüncü constrıctır olacak.


this keywordünün bir kullanımıda sınıfın içindeki property ve değişken neyse artık onu sınfın  metotların içinde ulaşmak için kullanılır ama bunu compiler seviyesinde yapıldıgıdan biz elle tek tek yazmıyoruz ama pythondaki selff komutu bunu yapmamdıgından  sürekli self self yazıyoruz. 

```csharp
  public class MyClass
    {
        public string isim = "serkan";

        public MyClass()
        {
            Console.WriteLine("İlk bu çalısacak"+ isim);
        }

        public MyClass(string a):this()
        {
            Console.WriteLine("İkinci bu çalışacak"+this.isim);
        }

        public MyClass(string a, string b):this(a)
        {
            Console.WriteLine("Üçüncü bu çalışacak"+this.isim);
        }
    }
```


### <font color="#6495ed">Base Keywordü</font>

kalıtım alan bir sınıf kalıtım aldığı sınıfın metotlarına,constructırlarına erişmesi için kullanılır

```csharp
namespace ConsoleApp1
{
    class Program
    {
        static void Main(string[] args)
        {

            new MyClass2();

            Console.ReadKey();
        }
    }

    public class MyClass
    {
        public string isim = "serkan";

        public MyClass()
        {
            Console.WriteLine("İlk bu çalısacak"+ isim);
        }

        public MyClass(string a):this()
        {
            Console.WriteLine("İkinci bu çalışacak"+this.isim);
        }

        public MyClass(string a, string b):this(a)
        {
            Console.WriteLine("Üçüncü bu çalışacak"+this.isim);
        }
    }

    public class MyClass2:MyClass
    {
        public MyClass2():base("Serkan","Altunay")
        {
            Console.WriteLine("Son Constructor");
        }
    }
}

```

MyClass2 sınıfının consttırı çalıştığı zaman kalıtım aldıgı MyClass sınıfının eşleşen paremetreli constırıcıtna gider ve onu çalıştırılır sonra kendisi çalışır ama burada MyClass constrıcıırında this oldugundan o da MyClass içindeki tek paremetreli constırıcı çalışıtıryor o da paremetresiz constırıcırını çalıştırılıyor derken hepsi çalışıyor en son bizim MyClass2 constırıcı çalışıp en altta bizim constractırın çıktısını veriyor. 

metotların içinde fonksiyon çağırmak istersen bir sınıftan kalıtım alan sınıfa base yazmak zorunda değilsin thisde olduğu gibi compiler kendisi yapıyor. ama isimleri iki sınıftada aynı ise thisde olduğu gibi base diye belirtmen gerekiyor.


### <font color="#6495ed">ReadOnly</font>

ReadOnly ile tanımlana bir referansı ya tanımladıgın an değerini vereceksin yada bir constrıctırda değerini vereceksin başka türlü fonksiyonda filan veremezsin ondan biz sürekli constractırda veriyormuşuz.

ReadOnly ile tanımlanan referanslar sadece okunabilir olur.

> [!note] Karşılaştırma
> ReadOnly ile Const karıştırabilir aralarında fark Const sadece yazıldıgı an değer atanır başka türlü constrıda değer atayamazsın ama ReadOnly'de işter tanımladıgın an ata ister Constructor'da atama imkanın var


### <font color="#6495ed">Sealed</font>

bir sınıfın ya da bir metodun türetilmesini engellemek için kullanılır. Bu, özellikle miras ilişkisinde önemli bir rol oynar. record'larda da kullabılabilir.

```csharp
sealed class Animal
{
}

class Örnek:Animal
{
 //hata verir sealed olarak işaretkenmiş animal sınıfı
}
```

Yada Overiede edilmiş fonksiyonlarda kullanılabilir

```csharp
class Animal
{
    public virtual void Speak()
    {
        Console.WriteLine("Animal is speaking.");
    }
}

class Dog : Animal
{
    public sealed override void Speak()  // 'sealed' burada kullanılıyor
    {
        Console.WriteLine("Dog is barking.");
    }
}

class Bulldog : Dog
{
    // Aşağıdaki kod hata verir çünkü 'Speak' metodu 'Dog' sınıfında sealed olarak işaretlendi.
    // public override void Speak() { Console.WriteLine("Bulldog is barking loudly."); } 
}
```