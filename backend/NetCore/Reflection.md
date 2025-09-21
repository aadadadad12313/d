
Reflection, C#'ta çalışma zamanında (runtime) tiplerin, metotların, özelliklerin ve diğer üyelerin bilgilerini inceleme yeteneğidir. Bu, programın kendi yapısını analiz edebilmesini sağlar.

Yani normalde derleme (compile) sırasında bildiğin sınıf, method, property gibi yapıları, program çalışırken dinamik olarak öğrenebilir, çağırabilir veya değiştirebilirsin.

Kısaca reflection depedency injection'ın bel kemeği yani

sen addScoped diyosun bu ne demek sadece gidip DI Container'a kaydet demek de bundan arkada instance oluşmayacak mı kim yapacak işte reflaction yapacak.

###### <font color="#92d050"> Reflection ne işe yarar?</font>

- Bir sınıfın özelliklerini, metotlarını, propertylerini, constructor’larını öğrenmek.
    
- Dinamik olarak nesne oluşturmak (`Activator.CreateInstance` ile).
    
- Metot çağırmak veya değer okumak/yazmak (property veya field) çalışma zamanında.


C# da   değer türlü değişkenler veya struct , class gibi tipleri tipini almak istediğinde typeof kullanmak zorunda değilsin.

çünkü bu türler objectten türediği için objectin GetType diye metodu var bu metod typeof görevi görüyor.


```csharp
 string Name = "Serkan";
 
 Type t = Name.GetType();

 Console.WriteLine(t);
```

Yukardaki kodun aynısı olur bu kod bize GetType da typeof gibi bir Type nesnesi döndüğünü kanıtlar.


Reflection konusunda En temel sınıf Type'dır.

Type olmadan hiçbir şey yapamazsın type'yi elde etmen lazım.

```csharp
string isim = "Serkan";

Type t = isim.GetType();

Console.WriteLine(t.Name);
```

tipi elde ettikten sonra o tipin ismini namespacesini propertisini artık ne ararsan işlem yapabilirsin burada öenmli olan tipi el etme bu da çok basit.


```csharp
  static async Task Main(string[] args)
  {
    Type t=MyClass.
    
  }

  public class MyClass
  {
      public string Name { get; set; }
  }
```

MyClass . dersen hiçbir şey çıkmaz sen objectten kalıtım almış bir classı direk içindeki memberları kullanmazsın değil mi.

iki seçenek var ya instance üretip GetType metodunu kullanacan ama bu program cs de filan zor olur.

diğer seçenek ise Typeof kullanarak nesne üretmeden direk sınfın tipini vererek compile time de çalışacak.

şimdi aradaki fark çok basit

GetType run time de çalışıyor çünkü classların nesneleri  run time de oluşur ondan GetType compile time de çalışsaydı eğer hata verirdi daha nesne oluşmadı ki run time de oluşacak ya nesne işte o zaman nesne oluşacağı zaman tipini al diyoruz getType de.

ama biz typeof ile zaten bir nesne oluşturmadan alabiliyoruz ondan compile timede çalışıyor mu run time de çalışması zaten saçma olur çünkü run timeyi neden beklesinki mecbur değil.

```csharp
  static async Task Main(string[] args)
  {
      Type t = typeof(MyClass);

      PropertyInfo[] s = t.GetProperties();
      foreach(PropertyInfo prop in s)
      {
          Console.WriteLine(prop.Name);
      }
  }

  public class MyClass
  {
      public string Name { get; set; }
      public int Age { get; set; }
  }
```

Myclass sınıfının proeprtylerini ekrana bastırdık reflection ile.

DI da çoğu şey reflection hep adamlar öyle yazmış istersen sen de reflection yazabilrisin işte bu GetProperties gibi metotları kullanarak sende çok çok ileride reflection da ilerleyeceksin.

#### <font color="#a5a5a5">Tip bilgisini alma</font>

```csharp
Type type = typeof(DateTime);

Console.WriteLine(type.Name);
Console.WriteLine(type.FullName);
Console.WriteLine(type.Namespace);
Console.WriteLine(type.Assembly);
Console.WriteLine(type.BaseType);
Console.WriteLine(type.IsClass);
Console.WriteLine(type.IsValueType);
Console.WriteLine(type.IsPrimitive);
Console.WriteLine(type.IsAbstract);
Console.WriteLine(type.IsSealed);
```

Datetime bir strcut bunun type tipini aldık ve reflection ile tüm bilgilerini konsol ekranında listeledik.

#### <font color="#a5a5a5">Dinamik Nesne Oluşturma</font>

```csharp
    public class Program
    {
        static async Task Main(string[] args)
        {
            Type type = typeof(MyClass);

            // sonra arkada dinamik nesne oluşturdu propertylerini buldu adam ekrana bastırdı.

            object instance = Activator.CreateInstance(type);

            // Tüm property’leri al
            PropertyInfo[] properties = type.GetProperties();

            foreach (var prop in properties)
            {
                object value = prop.GetValue(instance);
                Console.WriteLine($"{prop.Name} = {value}");
            }

        }

        public class MyClass
        {
            public string Name { get; set; } = "Serkan";
            public int Age { get; set; } = 20;
        }
    }
```


Typeof kullandın MyClassın neyi varsa tüm bilgilerini type de tutuluyor.

Classın içindeki memberlara ulaşmak için bir nesne oluşması lazım ondan dinamik nesne oluşturduk run time'de sonra propertyleri değerleri ile ekrana bastırdık.

yani DI Container'da aslında Reflection kullanarak arkada verdiğin tip ile nesne oluşturuyor sonra sana onu döndürüyor bu sayede Dinamik bir yapı sağlıyoruz.

#### <font color="#a5a5a5">s</font>

[[Type-TypeOf]]

[[Dependency Injection]]

onemli:: Reflaction

