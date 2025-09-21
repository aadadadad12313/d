
Generic yapı bir tipe değil birden fazla tipe özelleştirilmiş bir sınıftır.

Neden generic tip diyoruz çünkü Tip demek int float Class interface hepsi tip'tir ama tür ise değer ve referans tür diye ikiye ayrılıyor ondan biz Tip diyoruz çünkü Ayrım yok burada class da olur int de olur.

Generic class tanımlama yaparken sınıf isminin hemen yanına < > sembolleri arasına bir isim veriyorsun ahmet mehmet olabilir ama genellikle Type kelimesinin kısaltılmış hali olan T kullanılır.

`public class MyClass<T>` demek, “MyClass adlı bir generic sınıf tanımlıyorum, içine farklı tipler verilebilir, bu tip T olarak temsil edilecek” demektir.

yani sınıfa tip filan vermiyorsun classın içindeki kodlara tip veriyorsun.

```csharp
 MyClass<Örnek> myClass = new MyClass<Örnek>();
 
 public class MyClass<T>
 {
     T d;
 }
 
 public class Örnek
 {

 }
```

Burada MyClass'dan nesne oluştururken verilen tip sınıfa gidecek. T artık o zaman Örnek olacak.

dikkat et bu kodda referans türü MyClass değil MyClass<Örnek> tir.

bundan dolayı sen MyClass diye generic olmayan bir sınıf tanımlayabilirsin çünkü türü ayrı bunların.

🔎 Örnekle daha net:

`List<int>` derken mesela bunu türü List mi hayır List int türü birlikte yani çünkü Listeler bile generic yapı ile tasarlanmış neden çünkü Listelerin bir sürü türü var int var float var double var.

eğer generic olmasalardı Listeler ayrı ayrı classlarda her türü farklı isim verilirdi ama genric yapı sayesinde bir isim ile sadece tipini biz belirterek bunu çok kolaylıkla yöntebiliyoruz. 

###### <font color="#92d050">Örnek :</font>

int için

```csharp
public class ListInt
{
     public void Add(int item)

}
```

string için

```csharp
public class ListString
{
     public void Add(string item)

}
```

böyle mi olacak her tür için farklı koleksiyon ismi mi kullanacaz hayır kodlar aynı sadece int olan yer diğerinde string bunları dışarıda kullanıcı verince o olaya atanacak olay bitecek.

Yani :

```csharp
public class List<T>
{
     public void Add(T item)
     {
     }
}
```

Gerçek Listenin arkasında olan kod bu.

sen gidiyorsun string veriyorsun new yaparken T yazan yer nesne oluşacağı zaman string oluyor int yazıyorsun int oluyor ve böylece tekrar tekrar sınıf tanımlamak zorunda kalmıyorsun.

Sen mesela Listler'de ekleme yapıyorsun mesela List'teyi int olarak tanımladın int olarak ekleme yaparsın yada string tanımladın string olarak ekleme yaparsın ama ne de olsa yaptığın kodlar aynı sadece türleri farklı olduğundan add add diyerek kodu tekrar yazıyorsun.

işte generic tipler sayesinde kesin bir tip belirtmiyorsun sınıfta kullanılan tipleri nesne oluşturuken belirtiyorsun sonra belirttin türü mesela int class da full T olan yerlere int olarak işleniyor.

string belirtirsen string olarak işlenecek yani tekrar tekrar kod yazmanı engelleyecek.

###### <font color="#92d050">Generic Class</font>

Basit bir örnek: Generic Olmadan

```csharp
public class IntList
{
    private List<int> items = new List<int>();

    public void Add(int item) => items.Add(item);
    public int Get(int index) => items[index];
}
```

Bu sınıfın içinde operasyon da full int olarak işlem yapılmış.

ama string , float , double içinde ekleme olsa listeye o zaman  3 class daha tanımlayacaksın onlara da ayrı ayrı ekleme işlemine tabi tutacaksın.

ama Generic tip ile bunu tek bir sınıfta yapıyorsun çünkü kodlar tamamen aynı sadece int olan yer diğerinde string olacak bunun için tekrar bir kod niye yazacaksın gerek yok.


Generic ile

```csharp
public class MyList<T>
{
    private List<T> items = new List<T>();

    public void Add(T item) => items.Add(item);
    public T Get(int index) => items[index];
}
```

```csharp
MyList<int> m= new MyList<int>(); // T artık int oldu

MyList<string> m= new MyList<string>(); // T artık string oldu
```

Sınıf ismnin yanında bir isim belirtiyorsun bunun amacı sınıfnın içindeki kodlara bir tip vermek.

sen nesne oluştururlen bir tip verecen myClass,int,float artık ne verirsen bu verdiğin türler T yerine gelecek ve işlemi gerçekleştirecek böylelikle tekrar eden classlar oluşturmak zorunda kalmayacaksın.

###### <font color="#92d050">Generic Kısıtlamalar (Constraints)</font>

Bazı durumlarda `T`’nin hangi tiplerle kullanılacağını sınırlamak isteyebilirsin:

```csharp
public class Repository<T> where T : class
```

böylelikle sen Repository'e tpi olarak sadecce sınıf gönderebilirsin gidipde int yollayamazsın.

###### <font color="#92d050">Peki Aynı İsimde bir sınıf olsa ve Aynı isimdeki sınıftan kalıtım alabilir mi </font>

Evet alabilir çünkü bunlar farklı türlerdir birisi MyClass türü iken diğeri `MyClass<About>` türüdür aynı mı bunlar sence hayır.

```csharp
namespace ConsoleApp1
{
    public class Program
    {
        static async Task Main(string[] args)
        {

          MyClass myClass = new MyClass();

        public class MyClass<Mızık>
        {
            public decimal Price { get; set; }
        }

        public class MyClass:MyClass<Customer>
        {
            public string Name { get; set; }
            public string Description { get; set; }
        }

        public class Customer
        {
        }
    }
}
```

Generic bir class ile generic olmayan bir class aynı değildir compiler farklı tipde görür onları.

![[Pasted image 20250902031650.png|350]]

Görüldüğü üzere biz bir sınıfın ismini yazarken çıkıyor ya işte generic olanı var olmayanı var sen hangisini kullanacan diye işte tam bizim bu konu işte o adamlar hem sınıfı generic yapmışlar hem generic olmayanı aynı isimde yapınca bir sürü seçenek çıkıyor sınıfın adını yazınca.

Bir Generic Class generic tipden nesne oluşturulduğunda hata verir derki sana generic tip olduğundan nesne oluşturulamayan bir tipi vermiş de olabilirsin der ve kesin bir şekilde belirtmen lazım der sana nesne oluşabilecek bir tip olduğunu işte bundan constraint ile belirtiyoruz.

```csharp
       MyClass<Örnek> myClass = new MyClass<Örnek>();
        
        public class MyClass<T>
        {
            T d = new T();
        }
        public class Örnek
        {

        }
```

Hata vericek çünkü herhangi bir kısıtlama yapmadın.

###### <font color="#92d050"> Base Class Constraint</font>

Generic tiplerde gelen tipin belirtilen tip ile bir kalıtımı varmı yokmu bunun kısıtlamasını yapar.

```csharp
        static async Task Main(string[] args)
        {
            MyClass<A> myClass = new MyClass<A>();
            MyClass<B> myClass2 = new MyClass<B>();
            MyClass<C> myClass3 = new MyClass<C>();
          
        }
        public class MyClass<T> where T:A
        {
        }
        public class A
        {
        }

        public class B:A
        {
        }
        public class C : B
        {
        }
```

Base Class neydi kaltım alan bir sınıfın kalıtım aldığı sınıf değil miydi işte burada base classlık yoksa hata ver diye bir sınırlama yaptık.

Eğer gelen tip A ise A'nın A ile bir base classlığı varmı var.
B ise B'nin A ile base classlığı varmı A dan kalıtım aldığı için var.
C ise C'nin A ile base classlığı varmı B'den kalıtım aldığı için B'de A dan aldığı için yine var.

---

```csharp
        static async Task Main(string[] args)
        {
            MyClass<A> myClass = new MyClass<A>();
            MyClass<B> myClass2 = new MyClass<B>();
            MyClass<C> myClass3 = new MyClass<C>();
          
        }
        public class MyClass<T> where T:Class
        {
        }
        public class A
        {
        }

        public class B:A
        {
        }
        public class C : B
        {
        }
```

Bu kodda ise Class diyerek verilen tip kesinlikle class olmalıdır demiş olduk.


###### <font color="#92d050"> New Constraint</font>

Generic class'a gelen tipin nesnesi oluşturulacağı zaman hata alırsın çünkü gelen class'da belki 

paremetreleri constractır var generic tipler paremetreli constractırı kabul etmiyor illa paremetresiz 

bekliyor bunu belirtmen gerekir bundan new contraint ile paremetresiz constractır olacağını açıkça söylüyoruz.

 gelen tip'in interface veya static class olma ihtimali var bunlardan nesne oluşamayacağı için program açıkca gelen tipin class olduğunu tanımla diyor.


```csharp

       MyClass<A> myClass = new MyClass<A>();
          
        public class MyClass<T> where T: new()
        {
        }
        public class A
        {
        }

```


böylelikle classda istediği kadar paremetreli constractır olsun biz paremetresiz constractırı garanti ediyoruz.

Veya

```csharp
       MyClass<A> myClass = new MyClass<A>();
          
        public class MyClass<T> where T: class, new()
        {
        }
        public class A
        {
        }
```

Hem Base class hem de new contraint birlikte uygulanabilir.

Dikkat et base class contarint önce yazılmalı new constraintten yoksa hata alırsın.
  

###### <font color="#92d050">Generic Metod Tanımlama</font>

Generic method, tip parametresi alabilen ve farklı veri tipleriyle çalışabilen bir metottur. Sınıfı generic yapmak yerine sadece ilgili metodu generic tanımlayabilirsin.

```csharp
 public class Program
 {
     static async Task Main(string[] args)
     {
         Deneme<int>(45);
         Deneme<string>("Merhaba");
     }

     public static void Deneme<T>(T sayi)
     {
         Console.WriteLine(sayi);
     }
 }
```


Burada

```csharp
Deneme<int>(45);
```

int dediğimiz anda T olan yerler int olacak metotda aynı şekilde string içinde string olacak her yer 

böylelikle iki metod yerine tek metod tanımlanarak operasyonlar yapılabilir.


###### <font color="#92d050">Özet</font>

Generic Tiplere birden fazla tip verebilirsin.

Eğer bir interface generic tanımlanmışsa Bu interface’i implement eden class da o generic tip parametresini almalı.


###### <font color="#92d050">Generic Tipleri Repository Pattern kullanımı</font>

<font color="#bfbfbf">Generic Tip kullanmasak eğer</font>

```csharp
public interface IProductRepository
{
    List<Product> GetAll();
    Product GetById(int id);
    void Add(Product product);
    void Update(Product product);
    void Delete(Product product);
}

public interface ICategoryepository
{
    List<Category> GetAll();
    Category GetById(int id);
    void Add(Category product);
    void Update(Category product);
    void Delete(Category product);
}
```

tek tek kaç tane entity sınıfın varsa o kadar interface yazmak zorunda kalırsın.

<font color="#bfbfbf">ama generic tipler ile tek interface ile bu işi halledebiliriz.</font>

```csharp
    public interface IGenericDal<T> where T : class
    {
        void insert(T p);
        void delete(T p);
        void update(T p);
        List<T> GetAll();
        T GetById { get; set; }
    }
```

<font color="#bfbfbf">Aynı yöntemi classlar içinde yapabiliriz.</font>

```csharp
  public class ProductRepository : IProductDal
    {
        Context c = new Context();

        public void Insert(Product p)
        {
            c.Add(p);
            c.SaveChanges();
        }

        public void Update(Product p)
        {
        }

        public void Delete(Product p)
        {
        }

        public Product GetById(int id)
        {
        }
    }
```

tek tek bütün entityler için entity repositorysi oluşturursun.

<font color="#bfbfbf">Generic Tip kullanarak</font>

```csharp
public class GenericRepository<T> : IGenericDal<T> where T : class
{
    Context c = new Context();
    public void delete(T p)
    {
    }
    public T getByİd(int id)
    {
    }
    public List<T> GetListAll()
    {
    }
    public void insert(T p)
    {
    }
	public void update(T p)
    {
    }
}
```

tek bir entitye değil de bütün entitylere yaptık generic tip sayesinde.

<font color="#d8d8d8">Kodun Özeti</font>

interface'de where T : class yazılması zorunlu değildir hata almazsın yazmadın diye çünkü sen gidip de T'den nesne oluşturmaya kalkmadın sürece bu tanımı yapmak zorunda değilsin.

ama güvenlik açısından yazılması tavsite edilir çünkü gelecek tipin class olduğunu garantiye alırsın tip olarak abstract class , interface , structta gelebilri.

<font color="#a5a5a5">bu kodda</font>

```csharp
public class GenericRepository<T> : IGenericDal<T> where T : class
```

diyerek şu tanımlamada kafan karışabilir şimdi hemen canladıralım.

```csharp
GenericRepository<About> _repository = new GenericRepository<About>();
```

Önce `GenericRepository<About>` generic tipi About olarak verdik geldi sınıfa T artık About.

her T yazan yer artık About olacak. interfacenin generic tipide About oldu için o da About oldu. About için implementasyon yaptı.

<hr>

İkiside T olmak zorunda değil mesela interface tarafı Ahmet mehmette olabilir. ama

GenericRepository tarafı T ise IGericDal da T olmalı çünkü sen

```csharp
GenericRepository<About> _repository = new GenericRepository<About>();
```

derken About tipini GenericRepository'e veriyorsun T artık About oldu ama interface de generic 

olduğundan onada tip vermelisin işte elimizde bulunan T'yi vereceksin yani About'u vereceksin farklı bişey isim olabilir mi sence.

yani

```csharp
public interface IGenericDal<A>
{
    void Insert(A entity);
}
```

```csharp
public class GenericRepository<T> : IGenericDal<T> where T : class
```

interface tarafı farklı bir isimlendirme yapabilrsin hata vernez.