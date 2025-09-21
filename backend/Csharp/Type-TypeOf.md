
###### <font color="#92d050">Type</font>

Type ile herhangi bir tipin property bilgisini , namespace , attribut , method, hangi Assembly de bulunuyor gibi bütün bilgilere erişebiliriz hatta dinamik nesne bile üretebiliriz.

<font color="#92d050">Type</font> sınıfı herhangi bir tip hakkında bilgiyi tutar.

<font color="#92d050">Typeof</font> verilen tipin bilgisini almanı sağlayan bir operator.


Typeof dediğinde sana dönen şey System.Type ama bir sorun var değil mi

Typeof System.Type döndürüyor tamam da Type sınıfı da Type o zaman abstract class dan instance mi oluşuyor diye düşünebilirsin hayır öyle olmuyor şimdi.


<font color="#bfbfbf">Mantık</font>

Type aslında internal sealed bir soyut abstract sınıf içinde metotların propertylerin imzalarını tutuyor.

RuntimeType => sınıfı ise Type sınıfından türeyen gerçek bir sınıf imzaların içi bu classda dolduruluyor Type sınıfıda bu snıfta implemet ediliyor.

typeof yazdığımızda => Type t=new RuntimeType(); istance oluşuyor ama biz typeof'a bakınca System.Type olarak görüyoruz ama arkada Runtimetype olarak değer dönüyor.

çünkü burada bir polimorfizm mantığı var.

```csharp

	 Hayvan h = HayvanUret();

      abstract class Hayvan // Type sınıfı gibi düşün
      {
          public abstract void SesCikar();
      }

      class Kopek : Hayvan // RuntimeType sınıfı
      {
          public override void SesCikar()
          {
              Console.WriteLine("hav hav!");
          }
      }

      static Hayvan HayvanUret() //typeof keywordü
      {
          return new Kopek();
      }
```

yani görüyorsun HayvvanUret metodu geri dönüş değeri Hayvan ama return tarafı Kopek polimorfizm var aynısı Type türünde RuntimeType dönmesi gibi zaten return gibide hayvan dönemez çünkü abstract o.

Type yani hayvan base class olduğu için Kopek yani RuntimeType nesnesini alabilir çünkü RunTimeType bir Type iki tane türü var.

```csharp
Type t = typeof(Kopek);
Console.WriteLine(t.GetType()); // System.RuntimeType
```

Type t = typeof(Kopek); => bu kısmın t kısmına kadar olan kısım compile time'de türü Type olarak görülecek çünkü daha new ksımı compile timede çalışmaz run time'de çalışacak o zaman tipi Type değilde new artık çalışacağından RuntimeTpe olacak.

| Kod                       | Compile-time tipi | Runtime tipi |
| ------------------------- | ----------------- | ------------ |
| `Kopek k = new Kopek();`  | Kopek             | Kopek        |
| `Hayvan h = new Kopek();` | Hayvan            | Kopek        |

mantık çok basit new yani nesne oluşturmada runTime de yapılmıyor mu o zamana kadar yani compile time de h hayvan bitti. ama run time'de kopek artık.



Type bir abstract classtır doğrudan nesne oluşturulamaz.

işte bu yüzden Typeof kullanılarak run time'de RuntimeType isimli classdan nesne oluşturulur.

RuntimeType classı Type'den kalıtım aldığından base classlık var yani polimorfizm olduğundan gelen instance yani RuntimeType aslında Type'dır ondan sorun olmadan türler eşleşebilir.


 int, string, Person → bunların hepsi birer type.
    
 Type sınıfı → bu türlerin bilgisini içinde saklayan nesnedir.

 Typeof ise type türünü bu bilgileri bulup veren bir operatördür.


```csharp
Type t = typeof(string);
Console.WriteLine(t.FullName); // System.String
Console.WriteLine(t.IsClass);  // True
```

Burada t artık string hakkında istediğin soruyu sorabileceğin bir type nesnesidir.

neyden kalıtım almış , namespacesi ne , value type mı ne herşeyi öğrenebilirsin.

###### <font color="#92d050">Typeof ve GetType farkı</font>

<font color="#92d050">TypeOf</font>

- typeof bir C# operatörüdür.
    
- Görevi: Verilen tipin System.Type nesnesini döndürmektir.
    
- Derleme zamanında (  Compile-Time'da ) tip bilgisini elde eder.

- elimizde instance olması gerekmez paremetre olarak tipin adını ister.

```csharp
Type t1 = typeof(string); // Aslında RuntimeType instance'ı döndürür
```

 Buradaki string compile time'de biliniyor o yüzden typeof ile type bilgisine ulaşabiliyoruz.
 
 Type bir abstract class ondan new ile nesne oluşturamazsın referans kısmında type kullanırsın ama new kısmında aşağıdakini kullanırsın.

<font color="#92d050">RuntimeType</font>

 Bu sınıf internal sealed olduğu için biz doğrudan göremeyiz, ama typeof() veya GetType() ile runtime’da oluşturulur.
    
 Yani typeof(string) → aslında RuntimeType instance döner, referans tipi Type’dır.

- Bu RuntimeType nesnesi Type abstract class'ından türetilmiş
- Polimorfizm sayesinde bunu Type referansı olarak kullanabiliyorsun.

Yani aslında şuna benzer (ama direkt böyle yapamazsın) :

```csharp
RuntimeType actualObject = new RuntimeType(); // Runtime dahili olarak yapar
Type t1 = actualObject; // Sen bunu Type referansı ile tutarsın
```
    
Bunu doğrulayabilirsin:

```csharp
Type t1 = typeof(string);
Console.WriteLine(t1.GetType().Name); // "RuntimeType" yazdırır
```

Biz typeof kullanarak RuntimeType nesnesi arkada oluşuyor doğrudan new ile yapamazsın çünkü geliştiriciler internal olarak tanımlamış RuntimeType sınıfını ondan o assemblyde olmadığımızdan internal classına erişemeyiz.


 <font color="#92d050">GetType</font>

- Çalışma zamanı (runtime) aşamasında kullanılır.
    
- Bir nesnenin gerçek tipini verir.
    
- Yani elimizde bir nesne (instance) olması gerekir.

```csharp
string s = "merhaba";
Type t1 = s.GetType();
```


###### <font color="#92d050">Temel Fark</font>

- typeof → tip üzerinden çalışır (statik, compile time).
    
- GetType() → nesne üzerinden çalışır (runtime).

```csharp
// typeof ile derleme zamanında tür bilgisi
Type t1 = typeof(string);

// GetType ile runtime’da nesne üzerinden tür bilgisi
object o = "merhaba";
Type t2 = o.GetType();

Console.WriteLine(t1 == t2); // True
```

Görüyorsun, compile-time ve runtime sonunda aynı `Type` nesnesini verebilir. Ama compile-time ile nesne yaratmaya gerek yok, runtime ile instance’ı yaratmak zorundasın.

###### <font color="#92d050">Typeof Kullanım Yerleri </font>

DI Container aslında tüm güçünü reflectiondan alıyor diyebiliriz.

reflaction kullanabilmemiz için typeof kullanmamız gerekli.

Reflection notuna bakabilrsin konunun detayları orada.

[[Reflection]]

[[Dependency Injection]]

onemli:: Type Typeof




Mantık şu şimdi unutmadan yaz sonra düzeltirsin

Typeof compile time'de yani derleme zamanında çalışıyor çünkü typeof senden bir tip istiyor bu tip zaten derleme zamanında var olduğundan runTime ile işin yok senin typeof'u direk kullanabilirsin compile time'de.

ama gelelim GetType'ye bu zaten object sınıfının hazır gelen bir metodu bu metodun çalışması için bir instance gerekli zaten new yapılan nesne Run Time de nesnesi okuşacagından o zaman GetType'da doğrudan run time de çalışacak gidip compile time de çalışamaz çünkü daha nesne yok ortada.


peki dersen eğer typeof runtimeType döndüryor nesnesi yani ama bakınca Ssystem type geri dönüş değeri neden işte bu polimorfizmin okonunsu

cevap
Hayvan h = new Kopek(); => h compile time de nesne oluşmadıgından hayavan ama run timede kopek olacak.

işte bu mantık gibi typeof derleme zamanında system type döndürecek ama run time de nesnesi okulacagındna o an ne olacak runtitmeType tipinde olacak al sana mantıgı yukardakinden farkı varsa gel yüzüme tükür yarın.

`System.Type` sınıfı **her sınıfın metadata’sını temsil eden bir runtime nesnesidir**. çünkü nesne oluşutur nesne dedigimzi şey run time de oluşur

typeof(Kopek); yani burada 



### 2️⃣ Bizim mock örneği ile aynı mantık

`Type t = new RuntimeTypeMock(typeof(Kopek));`

- Burada **elle concrete bir instance** ürettik (`RuntimeTypeMock`)
    
- `t` compile-time’da `Type` referansı
    
- Runtime’da **gerçek nesne** → `RuntimeTypeMock`
    
- Bu, `typeof`’un aslında arkada yaptığı şeyi **elle taklit etmek** gibi düşünebilirsin.



🔑 1. Referans nedir?

Hayvan h = new Kopek();
h → Hayvan tipinde bir referans (yani compile-time’da Hayvan türü görünüyor).

Ama new Kopek() → runtime’da gerçek nesne Kopek.

h üzerinden çağırdığın her şey, aslında Kopek’in davranışına göre çalışır (polimorfizm).

🔑 2. typeof ile aynı şey

Type t = typeof(Kopek);
t → Type tipinde bir referans (System.Type, compile-time’da öyle görünür).

Ama typeof(Kopek) → runtime’da gerçek nesne RuntimeType.

Yani t değişkeni Type gibi görünüyor, ama arkasında RuntimeType nesnesi var.


Polimorfizm (Polymorphism)

```csharp
public static Araç deneme()
{
    return new Taksi(); // Polimorfik davranış
}
```

**ÖNEMLİ AÇIKLAMA:**

- Metodun **imzası** (signature) `Araç` döndüreceğini söylüyor
- `return new Taksi();` kısmında gerçekten **Taksi nesnesi** oluşturuluyor
- Ama bu Taksi nesnesi, **Araç tipinde bir referansa** atanıyor

**Compile-time vs Runtime farkı:**

**Compile-time'da:**

- Compiler metodun `Araç` döndüreceğini görür
- `deneme()` metodunu çağıran kod, sadece `Araç` tipindeki metotların kullanabilir
- Yani `Araç araç = deneme();` dediğimizde, `araç` değişkeni Araç tipinde kabul edilir

**Runtime'da:**

- Gerçekte oluşturulan nesne **Taksi** nesnesidir
- Memory'de Taksi'nin tüm özellikleri bulunur
- `virtual` metotlar çağrıldığında, **gerçek nesnenin** (Taksi) metodu çalışır


yani metodda olsa return kısmı daha compile timede bakılmıyor typeofda böyle türü ne ise geri dönüş ona bakılıyor compie timede ondan system type diyor adam ama run time de return runtimetype olacagını görüyor run time de bu oluyor. new kısmına o zmana baklıyor compile time dedigimz şey zaten kod bakmaz sadce tiplere bakıyor gidip de new yapmıyor returnda new varya ona şimdi bakmaz bakmayacagı şeyide comğiel timede yazmaz.


yani

**Compile-time'da:**

- Compiler **sadece metod imzasına** bakar: `Araç deneme()`
- Return kısmındaki `new Taksi()` **çalıştırılmaz**, sadece tip uyumluluğu kontrol edilir
- "Taksi, Araç'tan türetilmiş mi?" → Evet → OK
- Çağıran kod için bu metod **Araç döndürür** olarak işaretlenir
- `typeof()` da metodun **declared return type**'ına bakar → **System.Type: Araç**

**Runtime'da:**

- Artık kod çalışıyor, `new Taksi()` **gerçekten execute** edilir
- Memory'de **gerçek Taksi nesnesi** oluşturulur
- `GetType()` veya **RuntimeType** gerçek nesnenin tipine bakar → **Taksi**
- Virtual method dispatch gerçek nesnenin metodunu çağırır


chat gpt son kez dediğine ak


Aynen 👌 çok güzel yakalamışsın.  
Adım adım toparlayayım:

---

### 1️⃣ Compile-time’da ne oluyor?

- Derleyici (`csc`) sadece **tip kontrolü** yapar.
    
- `typeof(Kopek)` gördüğünde:
    
    - `System.Type` dönecek bir şey olarak işaretler.
        
    - `RuntimeType` falan hiç bakmaz, çünkü compile-time’da sadece _referans tipi_ önemlidir.
        

Yani compile-time işi: **“bu metodun dönüş tipi doğru mu, derlenebilir mi?”**

---

### 2️⃣ Runtime’da ne oluyor?

- Program çalıştığında CLR devreye girer.
    
- `typeof(Kopek)` çağrıldığında CLR, metadata’dan bir `RuntimeType` nesnesi üretir.
    
- O nesne `System.Type` referansına atanır.
    

Aynı mantık senin `return new Taksi();` örneğinde de var:

- Compile-time: dönüş tipi `Araç`.
    
- Runtime: `new Taksi()` gerçekten bellekte bir nesne oluşturur.
    

---

### 3️⃣ Neden `System.Type` yazıyor?

- Çünkü derleyici sadece **dönüş tipine** bakar → `System.Type`.
    
- Runtime’da `RuntimeType` olduğunu göstermek compile-time’ın işi değil; onu CLR çalışırken belirler.
    

---

### 🔑 Özet senin dediğinle birebir:

- **Compile-time** → sadece türlere bakar, `new` çalıştırmaz.
    
- **Runtime** → nesneyi gerçekten üretir (`RuntimeType`).
    

---

İstersen ben sana bunun nasıl “compile-time sadece tip kontrolü yapıyor, nesne üretmiyor” olduğunu çok basit bir örnekle göstereyim mi? (örneğin hiç çalışmayan ama derlenen kod ile).









Polimorfizm notuna ekle

## Soyut Referans Nesnesi

**Compile-time tipi:** Referansın tanımlandığı soyut sınıf veya interface tipi **Run-time tipi:** Referansın gerçekte işaret ettiği somut (concrete) nesnenin tipi

Örnek:

java

```csharp
abstract class Animal 
{
    public abstract void MakeSound();
}

// Dog somut sınıfı
class Dog : Animal 
{
    public override void MakeSound() 
    { 
        Console.WriteLine("Woof"); 
    }
}

class Program 
{
    static void Main() 
    {
        Animal myAnimal = new Dog(); 
}
```

Bu durumda:

- **Compile-time tipi:** `Animal` (soyut sınıf)
- **Run-time tipi:** `Dog` (somut sınıf)

böyle işte polimrofizm varsa compile time de referans tipi görülüyr run time nesne gelince tip belli oluyor  ama aşağıdaki class nesnesinde a =new a () oldugunda hem compile time a hem run time de a geliyor polimorfizm konsuu cidden cok güzel esnek çok iyi bir konu ama anlamasınıda yaptık artık bizim önümüzde kim durabiliriririrrrrrrrrrrrrrr.

## Class (Somut Sınıf) Nesnesi

**Compile-time tipi:** Nesnenin tanımlandığı somut sınıf tipi **Run-time tipi:** Aynı somut sınıf tipi (genellikle aynı)

Örnek:

```csharp
Dog myDog = new Dog();
```

Bu durumda:

- **Compile-time tipi:** `Dog`
- **Run-time tipi:** `Dog`




---
Bu kısımlar tammen benim süzgeçimden geçen yerler ilgili satıra daha sonra konumlandır




<font color="#4bacc6">Typeof: </font> Typeof kullanarak nesne üretmeden verdiğin tipin bilgilerine compile time'de ulaşabilirsin.
dersen eğer neden run time'de çalışmıyor bu typeof çünkü nesne üretmiyor adam neden orada çalışşın ki eğer nesne üretip referansı typeof'a verebilseydik o zaman evet run time olacaktı.

<font color="#4bacc6">GetType: </font> GetType object sınıfının bir propertsi olduğundan nesne üretmen lazım ki property'i kullanabilesin.

nesne üretmekten bahsediyorsak o zaman RunTime'de çalışacak demek bu.

##### <font color="#92d050">Reflection Örneği Controller'ın reflection ile oluşması</font>

Dependency Injection container'ı reflection ile constructor'ları analiz eder ve gerekli bağımlılıkları inject eder.

<font color="#4bacc6">Adım Adım Controller Oluştuma</font>

Adım 1 : Önce uygulama başlatırır.

```csharp
// Program.cs
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddControllers(); // Controller'ları kaydet
```

Adım 2 : Controller Keşfi

Framework, assembly'leri tarar controller base class'ından türeyen sınıfları bulur.
Reflection kullanarak bu sınıfları metadata'sını okur.

```csharp
Type[] controllerTypes = assembly.GetTypes()
    .Where(t => t.IsSubclassOf(typeof(Controller)))
    .ToArray();
```


Adım 3: Routing Tablosu Oluşturma

- Her controller'ın action method'larını reflection ile bulur
- Route attribute'larını okur
- URL pattern'lerini oluşturur


Adım 4 : Route Eşleştirme

```csharp
// Gelen URL: /api/users/123
// Bu URL hangi controller/action ile eşleşiyor?
var matchedRoute = routingTable.FindMatch("/api/users/123");
// Sonuç: UserController, GetById action
```

Adım 5 : Controller'dan nesne oluşturma

```csharp
// Reflection ile controller type'ını al
Type controllerType = typeof(UserController);

// Constructor'ı bul
ConstructorInfo constructor = controllerType.GetConstructors()[0];

// Constructor parametrelerini al
ParameterInfo[] parameters = constructor.GetParameters();

// DI Container'dan bağımlılıkları çöz
object[] dependencies = new object[parameters.Length];
for (int i = 0; i < parameters.Length; i++)
{
    dependencies[i] = serviceProvider.GetService(parameters[i].ParameterType);
}

// Controller instance'ını oluştur
object controllerInstance = Activator.CreateInstance(controllerType, dependencies);
```

Adım 6 : Action Methodunu Çağırma

```csharp
// Action method'u reflection ile bul
MethodInfo actionMethod = controllerType.GetMethod("GetById");

// Method parametrelerini hazırla (route values, query params vs.)
object[] actionParameters = PrepareActionParameters(actionMethod, request);

// Method'u çağır
object result = actionMethod.Invoke(controllerInstance, actionParameters);
```