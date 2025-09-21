

Main Metodunda neden Static keywordü var

`Main` metodu programın başlangıç noktasıdır ve CLR (Common Language Runtime) tarafından çağrılır. CLR bir nesne örneği oluşturmadan bu metodu çalıştırabilmek için `static` olması gerekir.

**1. Static Context Kuralı** Static bir metod (Main) içinden sadece static üyelere erişebilirsiniz.
yani static bir metod varsa içinde static metod çağırları olacak demektir.

Özet

Sen Net core de static metod içinde çağırım yapmadıgından static metod içinde yalnızca statik metod çağırımları yapılır ksımını bilmiyordun ve Net core de zaten kullanmaycaksın çünkü sen static metod tanımlarsın ama onun içinde bir metod tanımlaması yaparmısın orasını bilemem o zamanki operasyona bağlı.

###### Özet

- `Main` metodu static olmak zorunda (CLR gereksinimi)
- Static metodlar sadece static üyeleri çağırabilir.

```csharp
class Program
{
    static void Main(string[] args)
    {
        // Aynı sınıf içinde - direkt çağırabilirsin
        Hesapla();        // ✅ Bu çalışır
        
        // Sınıf adı ile de çağırabilirsin (gereksiz ama çalışır)
        Program.Hesapla(); // ✅ Bu da çalışır ama gereksiz ama
        // Başka sınıftan çağırırken sınıf adı ZORUNLU
        // yani sadece Hesapla() ismini metodu tanımladığın yerde class da 
        // kullanabilirsin başka bir class da Sınıf ismini de belirtmen                  gerekecek.
    }
    
    static void Hesapla() 
    {
        Console.WriteLine("Static metod çalıştı");
    }
}
```




```csharp
class Program
{
    static void Main(string[] args)
    {
        // Nesne oluşturmadan direkt çağırabilirsiniz
        Hesapla(); // Bu çalışır veya
        Program.Hesapla();
    }
    
    static void Hesapla() 
    {
        Console.WriteLine("Static metod çalıştı");
    }
}
```

 Static demek  Sınıf ismi sonra Metod ismi ile o metodu kullanablirsin demek new yapmadan.
 

```csharp
Hesapla()
```
Dikkat edersen bu örnekte static olan metod böyle tanımlanmış aynı sınıfta ise Program.Hesaplaya gerek yok ama farklı bir sınıfnta ise Program.Hesapla olarak çağırmakta zorundasın.



Eğer Main Statik tanımlamasaydık yani öyle hayar ederim 
CLR'nin arka planda yaptığı (görünmez):

```csharp
// CLR şöyle davranabilirdi:
class Program
{
    void Main(string[] args) // static değil
    {
        Console.WriteLine("Çalıştı");
    }
}

// CLR arka planda:
// Program instance = new Program();
// instance.Main(args);
```

olurdu ama bu yöntem yok sen Main'i static ile işaretleyeceksin zorundasın müdahale edemiyorsun ama kendi kodumuz olsaydı biz bunu bir instance ile de yapardık static'i silerdik ama buraya dediğim gibi müdahale olmuyor.


yani adamlar isteseydi Main'i static yapmaz gider CLR çalışırken bir instance yani nesne üretir o nesnenden Main metodunu çalıştırıldı ama adamlar static ile yapmayı tercih etmişler.



Yani Bir Main metodu dikkat edersen static keywordü ile işaretlenmiştir yani bu ne demek bu Main metodunu CLR çağırırken nesne oluşturmadan yani new yapmadan direk sınıf ismi ile metoduna erişim yapıyor. eğer main'de static olmasaydı ne olurdu aşağıdaki gibi nesne oluşturulacak 

```csharp
Program programInstance = new Program();
programInstance.Main(args); 
```
çalışmaz bu kodda main static keywordü var tabiki ama static olmasaydı bu kod ile nesne oluşturup o instance yani nesneden erişim yapacaktık ama static oldugundan direk sınfı ismi ile new yapmadan direk erişiyorsun statik çok ama çok önemli bir konu dikakt et c# full static sen kod yazarken new yapmıyorsun hep direk sınıf ismi sonra nokta diyerek new yapmadan direk alıp kullanıyorsun ama bazılarında new yapıyorsun sebebi bu işte gidip arkada bakarsan ne var diye o classın içinde yada metodun içinde new yapmadıklardında static keywordünü new yaptıklarıdnda ise static keywordünün olmadıgını bu yüzden instance oluşturup bu sınınıfın memberlarını kullanmak için instance oluşturuduğunu anlayacaksın.



Dikkat Et

Main metodu static demek arkada Program.Main() denecek asla new ile instance oluşmayacak demek bu.

```csharp
  static void Main(string[] args)
    {
        Program.Hesapla() //1
        
        Hesapla(); //2
        
        Program s=new Program(); //3
		s.Yazdir();
    }
    
    static void Hesapla() 
    {
        Console.WriteLine("Static metod çalıştı");
    }
    
    void Yazdir(){
    }
```

1. Durumda Hesapla Static oldugundan Static main de sınıf ismi + metod ismi ile çağrılabilir.  çünkü Main Metodu Static, static bir metod içinde static metod ister baska static olmayan metodu kabul etmez.

2. Durumda ise Hesapla direk yazılmış aslında 1.Durumun kısa yazımı yani aynı class da isen sen uzunca Program.Hesapla yazmana gerek yok direk isim ile çağırabilirsin ama farklı bir dosyada isen bunu sınıf ismi ile çağırman gerekecek.

3. durumda ise static bir metod içinde static bekler ama instance üretemez demedik Program Sınıfnından bir instance üretip o instance üzerinden metoduna erişip kullanaiblirsin. 

## Kural

Static metoddan:

- ✅ Static metodları direkt çağırabilirsin
- ✅ Static olmayan metodları nesne üzerinden çağırabilirsin
- ❌ Non-static metodları direkt çağıramazsın


## Kural:

- **Static Main** → **Static üyelere** direkt erişir ✅
- **Static Main** → **Non-static üyelere** nesne ile erişir ⚠️


---
Bu alttaki static notundan alındı bende bir ayrı not açmışım main metodu static diye ikisini burada birleiştir hallet detaylanıdr




```csharp
  public static class MyClass
  {
      public void Serkan()
      {
          Console.WriteLine("deneme");
      }
  }
```

Hata verecek neden mi şundan

MyClass tanımlamasında static var bu artık class'a ben static olduğumdan senin instance üretmene artık engelliyorum diyor bitti artık Myclass kesinlikle nesne üretemez.

Serkan metodu hata verecek neden şundan 

Serkan metodu direk yazmışşın direk yazılan bir metod aslında aklına ne gelir bu metoda nesne yoluyla erişebilirsin demek değil mi öyle o zaman adam der benim kafam karıştı MyClass instance üretmezsse sen ne yapacaksın da Serkan metodunu kullanacaksın işte ondan hata verir ondan eğer sınıf static ise sende metotlarını da static yapacaksın.

başka yolu yok bunun sen zaten sınıfı static yaparak new yapmaya karşı geldin metodu new yapıp nasıl çağıracaksın zatten ondan herşey static ile işaretlenecek.

yani

```csharp
    public static class MyClass
    {
  
        public static void Serkan()
        {
            Console.WriteLine("deneme");
        }
    }
```

```csharp
 static void Main(string[] args)
 {
     MyClass.Serkan();

     Console.ReadKey();
 }
```


<hr>

Şimdi Diğer İncelemeleri yaparım

```csharp
    public class MyClass
    {
        public static void Serkan()
        {
            Console.WriteLine("deneme");
        }
    }
```

Eğer sınıf Static omassa fonksyion Statik olursa Serkan metodu MyClass.Serkan() olarka çalışır artık sınıf ismi üzerinden yani bir  MyClass instance oluşturulduğundan MyClass.serkan göremezsin bunun sebebi

```csharp
MyClass denme = new MyClass();
denme.   // Burada Serkan metodunu göremezsin
```

- Nedeni: denme bir instance yani normal nesne.
    
- Serkan ise static, yani bu nesneye ait değil.
    
- Static metotlar nesne üzerinden görünmez, sadece sınıf üzerinden çağrılır.


Abstract Class Da Kullanımı Yine Aynı

Abstract Class dan nesne oluşamaz. e zaten abstract olmasydı static class olsaydı yine nesne oluşmayacaktı yine aynı şey burad takılacak bir durum bile yok ondan sonra metod statik tanımlandı bunu direk MyClass.Serkan() olarak çağıracaksın.

```csharp
    public abstract class MyClass
    {
        public static  void Serkan()
        {
            Console.WriteLine("deneme");
        }
    }
```


eğer static olmasaydı normal bir metod olsaydı

```csharp
public abstract class MyClass
{
    public void Serkan()
    {
        Console.WriteLine("deneme");
    }
}

public class Deneme : MyClass
{
}
```

```csharp
 MyClass deneme = new Deneme();
 deneme.Serkan();
```
Abstract Class dan bir referans Deneme Sınıfından ise bir nesne oluşturulduk sonra metoda erişildik.