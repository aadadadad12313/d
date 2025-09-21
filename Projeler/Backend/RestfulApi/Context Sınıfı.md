
Context sınıfı DbContext den kalıtım ( inherit ) alan bir sınıftır.

##### <font color="#92d050">DbContext Ne İşe Yarar?</font>

Görevi, C# ile yazdığın sınıfları (Entity’leri) veritabanındaki tablolarla eşleştirmek ve uygulama ile veritabanı arasındaki iletişimi yönetmek.

Context sınıfında Uygulamanın hangi veri tabanına bağlanacağını belirlenir.

Yazılan Linq sorgularını Context sınıfı çalıştırır yani

<font color="#bfbfbf">Örnek üzerinden detaylı analiz yapalım</font>

```csharp
IQueryable<Product> query = context.Products.Where(p => p.Price > 1000);

List<Product> result = query.ToList()
```

context.Products dediğimiz Context sınıfında bir propertydir. 

propertinin türü`DbSet<Product>` dir.

Dbset generic bir tip alan bir sınıftır.

###### <font color="#92d050">Where metodu mantığı arkada ne döndüğü bakmadan geçme</font>

Where metodu DbSetin bir metodu ve bu metod arkada imzası IQueryable interfacesi olduğundan türü IQueryable olarak karşılanmak zorundayız çünkü sana IQueryable türünden dönüş yapıyor içerde nesne dönebilir return ile ama bu öenmlii değil mantık burada metodun dönüş tipinde.

içerde return new yaptığı bir nesne yani burada  polimorfizm var.

Where metodu `EntityQueryable<Product>` bu sınıfı new yapıp sana return ile döndürüyor bunu nasıl yaptığına benzer bir örnek verelim.

[[Metodlar]] => metotlar notunda up uzun bir örnek ile bu konuyu detaylandırdım.

<font color="#bfbfbf">Arkada olan Where metodunun kodları</font>

```csharp
        public static IQueryable<TSource> Where<TSource>(this IQueryable<TSource> source, Expression<Func<TSource, bool>> predicate)
        {
            ArgumentNullException.ThrowIfNull(source);
            ArgumentNullException.ThrowIfNull(predicate);

            return source.Provider.CreateQuery<TSource>(
                Expression.Call(
                    null,
                    new Func<IQueryable<TSource>, Expression<Func<TSource, bool>>, IQueryable<TSource>>(Where).Method,
                    source.Expression, Expression.Quote(predicate)));
        }
```

görüldüğü üzere Where metodunun geri dönüş değeri `IQueryable<TSource>` olduğundan biz metodu kullanırken bu türden kullanmamız gerekiyor.

CreateQuery yeni bir sorgu yaratıyormuş dedik ya where sorgu oluşturur diye kanıtı.


```csharp
return source.Provider.CreateQuery<TSource>(expressionTree);
```

bu return ederken CreateQuery metodunu çağırıyor o metodun içinde nesne oluşturuyor yani bu arkada Factory method tanımlanmış arkada nesne oluşturuluyor.

```csharp
    public IQueryable<TElement> CreateQuery<TElement>(Expression expression)
    {
        // IQueryable<TElement> nesnesini burada oluşturuyor:
        return new EntityQueryable<TElement>(this, expression);
}
```

yani metodlar ile nesne üretmek olunca iş biraz uzuyor kendimize ait olsaydı sınıf biz direk

```csharp
 IQueryable<TElement> deneme = new EntityQueryable();
```

```csharp
// Bu ikisi aynı şey:
IAnimal myDog = new Dog();  // Direkt yaratım
IAnimal myDog = AnimalExtensions.MakeDog();// Factory method ile
```

Factory method ile new kısmı arkada oluyor sonra nesnenin referansını adam sana yolluyor sende  where metodunu imzasını IQueryable olduğundan bununla karşılıyorsun olay tamamen bundan ibaret.

<hr>

###### <font color="#92d050">Örneğin mantığı</font>

Where metodu çalıştığında sorgu veri tabanına gitmez çünkü Ef Core ertelenmiş çalıştırma mantığı ile çalışır.  

Ef sadece bir Linq sorgu ifadesi oluşturur.

ToList metodu çalıştığı an sorgu ifadesi alınır sql diline çevrilir ver veri tabanına yollar kodları.

<font color="#92d050">Arka planda üretilen sql</font>

```sql
SELECT [p].[Id], [p].[Name], [p].[Price]
FROM [Products] AS [p]
WHERE [p].[Price] > 1000;
```

veri tabanından dönen cevaplar c# nesnelerine dönüştürlür yani sql'deki tablonun her bir satırı bir Product nesnesi olacak.

Yani

- `Where(...)` sadece sorgu ifadesi oluşturur.
    
- `ToList()`, `FirstOrDefault()`, `Count()`, `Any()` gibi **yürütücü metodlar (execution methods)** çağrıldığında sorgu gerçekten SQL’e gide ve veriler çekilir.


```csharp
List<Product> result = context.Products
                    .Where(p => p.Price > 1000)
                    .ToList();
```

peki dersen eğer iki satırda değil de tek satıda yazsak ne olur.

kodlar ardı ardına yazıldığından önce where bir sorgu ifadesi oluşturur ve bekletilir. ardından ToList metodu olduğundan  sql'e çevirir kodları yani bekleme süresi olarak ayrı ayrı yazmadan daha kısadır.


##### <font color="#92d050">DbSet nedir </font>

Dbset veri tabanındaki bir tabloyu c# kodunda temsil eden generic bir sınıftır. her DbSet belirli bir entity türü için bir koleksiyon görevi görür ve o tabloya yönelik Crud işlemlerini gerçekleştirmemizi sağlar.

```csharp
public DbSet<Product> Products { get; set; }
```

bu satır Products tablosunu temsil eder veri tabanında.

DbSet sayesinde Linq sorguları yazılabilir çünkü metodlaru bu sınıftan geliyor gelen metodlarda add remove metoldarı olduğundan Crud işlemini bu sınıf sayesinde yapıyoruz.

<font color="#92d050">Peki dersen eğer bir sınıf ise DbSet nesne nasıl oluşuyor cevap Reflection</font>


Sen `new AppDbContext()` dediğinde veya DI'ya eklersin otomatik kendisi new yapar neyse. context sınıfı DbContext den kalıtım aldığından DbContext sınıfının önce constractırı varsa o çalışacak oop bu.

DbContext çalıştı constractırı ve içinde reflection kodları var amacı DbSetlerden reflection ile instance üretmek.

reflection AppDbContext sınıfını propertylerine erişir kodlar ile Properties metodu vardı hatırlarsan reflection'ın neyse sonra property türlerine göre nesne üretir.

yani

## 📌 Arkada Olan Süreç

1. **DbContext Oluştuğunda**
    
    - Sen `new AppDbContext()` dediğinde, EF Core `DbContext` sınıfının kurucusu (`constructor`) devreye girer.
        
    - EF, reflection kullanarak senin context içindeki `DbSet<T>` property’lerini bulur.
        
2. **DbSet Nesneleri Dinamik Olarak Yaratılır**
    
    - EF, `Products` gibi property’lerin tipine bakar (`DbSet<Product>`).
        
    - Daha sonra `DbSet<Product>` için **proxy nesnesi** oluşturur.
        
    - Bu proxy, `IQueryable<Product>` gibi davranır ve sorguları SQL’e çevirir.
        
3. **Sorgu Hazırlayıcıya (Query Provider) Bağlanır**
    
    - Oluşturulan `DbSet` nesnesi, veritabanına bağlanacak olan `EntityQueryProvider` ile ilişkilendirilir.
        
    - Böylece sen `context.Products.Where(...).ToList()` yazınca, sorgu SQL’e çevrilip çalıştırılır.






DbSet soyut sınıf ama gerçek sınıfı `InternalDbSet<Product>` bundan instance dinamik nesne ile ütetilir sonra üretien nesne get set yani property bu set propertysine nesne atanır



### 3. Kısacası

- Sen `context.Products` dediğinde, aslında `InternalDbSet<Product>` tipinde bir nesne alıyorsun.
    
- Yani **DbSet kendisi new’lenmiyor**, EF Core arka planda `InternalDbSet<T>` new’leyip property’ye koyuyor.
- 


Yani dbset'e yakından bakarsak

DbSet bir sınıf ama generic tip alan bir sınıf.

sonunda get set var property.

yani uzaktan bakınca anlaman gereken ` DbSet<Product>` türünde bir property tanımlaması var.

dersen get set ne alaka DbSet bir sınıf değil mi property olurmu neden olmasın lan nedenn n

Entityleri get set ile tutmuyormuyuz class türünden eee aynı şey.







<hr>

sonra düzelticlecekler



Önce sen controllerda context sınıfını constractır ile istedin bu sırada DI da kayıtlı olduğundan bizim sınıftan bir instance üretilecek ve sınıfnımızın yardımcı metodu çalışacak ve olaylar başlayacak.

```csharp
public DatabaseContext(DbContextOptions<DatabaseContext> options) 
    : base(options)
{
}
```

constractırımız çalıştı ve 

```csharp
DbContextOptions<DatabaseContext> options
```
bunu zaten DI doldurdu dolu yani burada ne var peki dersen  EF Core’un `DbContext`’ine bağlanırken gerekli olan **ayarların** (provider, connection string, davranışlar vs.) saklandığı sealed bir sınıf.

bunu biz ilk önce doldurduk DI tarafında şöyle

```csharp
services.AddDbContext<DatabaseContext>(options =>
    options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));
```

Burada `options => ...` dediğin şey aslında bir `DbContextOptionsBuilder<DatabaseContext>`.

- `UseSqlServer` çağrıldığında builder içine **provider = SqlServer** + **connection string** bilgisini ekliyor.
    
- Sonra EF Core bu builder’dan bir `DbContextOptions<DatabaseContext>` nesnesi oluşturuyor.
    
- Bu nesne dependency injection ile senin constructor’a geliyor.

burada sen aryarlarını yaptın sql server kullanacağını connection string'ini tutugun ismi filan her şeyi verdin bunu okudu önce sonra geldi constractırda options paremetresi artık senin hangi veri tabanın kullacan gibi bütün bilgilere hakim.

```csharp
: base(options)
```

base ile options paremetresi artık dolu ve bütün bilgiler var ve bizim context sınıfnın kaltım aldığı bir DbContext var bu sınıfda bizim  bilgileri tutmalı neyin ne oldugunu bilemli ondan onun constractırına geçmen gerkeiyor paremeteyi ondan sen burada base keywördü ile options paremetresini ilgil sınıfnın ilgili constractırına verdin ve orası çalıştı


şimdi yukardakiler sadece mantık tı şimdi sıra ile çalışmayı inceleyerim

ilk önce DbContext sınıfı base classdan değer aldı burası çalışacak.

neden diye sorarsan kural buydu base verdiğinde önce kaltım aldığın sınıfın ilgili constractırı çalışacak sonra bizim context sınıfımızın constractırı çalışacak.

```csharp
protected DbContext(DbContextOptions options)
{
    _options = options;
    _changeTracker = new ChangeTracker(this);
    _database = new DatabaseFacade(this);
    _contextServices = new DbContextServices(this, options);
}
```



şimdi gelelim bir diğer context tanımlama yöntemine yani constractır olmadan yapmaya

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder.UseSqlServer("connection string...");
}
```

şimdi sınıfı sen kendin mecburen new leyecen önce context sınınfı çalışacak.

sonra context sınınıf çalıştığı an DbContext'in paremetresiz constaractırı çalışacak onda this keywördü var yani şu

```csharp
 protected DbContext()
     : this(new DbContextOptions<DbContext>())
 {
 }
```

bu gidecek ikinci constractırını çalıştıracak.

```csharp
        public DbContext([NotNull] DbContextOptions options)
        {

            _options = options;
```

gibi birkaç kodu daha var neyse options boş oldu tabi çünkü DI olsaydı dolardı çoktan işte bu tip durumlar için bir metodu var onConfugiring diye onu çalıştıracak bize.

constractırdan çıktından sonra altta bir kod var

```csharp
var optionsBuilder = new DbContextOptionsBuilder(_options);
OnConfiguring(optionsBuilder);

var options = optionsBuilder.Options;
```

DbContextOptionsBuilder bu sınıftan nesne oluşturdu adamlar biz DI kullandıgımızda startup da context belirtiyorsun ya o kendisi DbContextOptionsBuilder bu sınıf oluşturuyor ama burada DI olmadıgında kod sana yine oluştıruyor sana nesneyi veriyor sende DbContextOptionsBuilder bu sınıfnın içindeki useSqlServer metodunu kullanabiliyorsun yoksa kullanazmsın.

bak OnConfiguring metoduna nesneyi paremetre olarak verdi. boş tabiki hepsi ama şimdi dolacak o context sınıfında biz dolduracağız.


var options = optionsBuilder.Options; ile de artık doluyor metod çalışınca.


önemli DI ya kaydettin ya ama sen addscoped demedin nasıl oldu filan dersen addDbContext onu arkada varsayılan olarak diyor.