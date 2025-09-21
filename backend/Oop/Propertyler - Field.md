
##### <font color="#92d050">Field</font>

Fieldlar nesne içerisinde değer tutmamızı sağlayan alanlardır. 

Fieldlar classlar içindeki değişkenlerdir yani c# da değişkenler class içinde field olarak adlandırılıyor.

class içinde tanımlanan değişkenler field'tır.

classın içinde method varsa onun içinde tanımlana bir field değil local değişkendir.

aynı konsol programındaki gibi işte biz konuyu ilk konsolda öğrendiğimiz için Program classının içinde metod tanımlarken içindeki değişkenlere adamlar local değişkenler diyordu hiç onların field dediğini duydun mu hayır.

```csharp
    MyClass m = new MyClass();
    m.a = 10;

    MyClass m2 = new MyClass();
    m2.a = 20;

public class MyClass
{
   public int a;
   public int b;

}
```

görüldüğü üzere değerler tuttuk.

public olmasa field dışarıdan erişilemez ondan bir field oluşturyorsan public yazman alışkanlık haline getirmelisin.


Fieldlara default olarak değerler atanır her tür için kendi türüne uygun değerler mesela

int türüne 0 , string türüne null , bool türüne false verilir.

```csharp
      MyClass m = new MyClass();
      Console.WriteLine(m.a); // 0

  public class MyClass
  {
     public int a;
     public int b;
  }
```

yani field class içinde geçerlidir herhangi bir değişken değişkendir ama class içinde field'tır.

class içindeki fieldların varsayılan değerleri vardır. ama classın dışında tanımlanan değişkenlerin yoktur onlara bir değer vermek zorundasın.

mesela bir metodun içindeki değişkene değer vermek zorunda iken classın içindeki field'a değer vermek zorunda değilsin varsayılan değerleri olduğundan onlar atanıyor zaten boş bıraksanda.

##### <font color="#92d050">Propertyler</font>

Fiedları dışarıya kontrollü bir şekilde açmamızı sağlayan ve dışarıdan gelecek değerleride kontorllü bir şekilde almamızı sağlayan yapılaara property denir.

Property esasında metottur. metotlardan farkı paremetre almazlar ve içerisinde get ve set bloklarını barındırır.

Property kullanıyorsak Encapsulation(Kapsülleme)'yi uyguruyoruz demektir. yani property tanımlamak aslında bir imza içinde yaptığımız değeri almak değiştirmek bunlar kapsülleme konusunun konularıdır property kullanarak yapılır.


```csharp
      public static int X()
      {
          return 0;
      }

      public class A
      {
          public int X
          {
              get
              {
                  return 0;
                // get bloğu property'nin değeri çağrıldığında get bloğu                          tetiklenir ve değeri return edilir.
              }

              set
              {
                // set bloğu property'e bir değer atandığında tetiklenir
              }
          }
      }
```

Örnekte görüldüğü gibi property aslında bir metod gibi çalışır.

Yani

Propertyler ile bir metotda ne yapabiliyorsan propertylerde de yapabilirsin.

Nesne üzerinde değer okuma ve yazma işlemlerinde metodlar yerine propertyler kullanılır.

yani

Bir özelliği belirtiyorsan kimlik,yas,ad gibi property kullan eğer bir eylemi belirtiyorsan yani bir iş yaptırma gibi metodları kullan.

Biz yazılımmcılar nesnelerimiz içerisindeki fieldlara direkt erişilmesi istemeyiz.
dolayısıyla filedları kontrollü bir şekilde dışarıya açmak için propertyleri kullanırız böylece  fieldları private tanımlarız nesne üzerinden propert'e erişip kontrollü bir şekilde fied'ın değerini veririz yani propert içinde bir konreol yaparsın tarih şu şu tarihten önce ise şu değeri ver gibi kontrol ile veriyoruz işte.

Yani property yapıları özünde nesne içerisindeki bir field'ın dışarıya kontrollü çılmasını ve kontrollü bir şekilde dışarıdan değer almasını sağlayan yapılardır işte propertylerin bu işlevine Encapsulation( Kapsülleme) denir.





Peki biz get set bloklarını kullanmayacaksak neden property kullanıyoruz field değil de çünkü

Net Core tarafındaki bütün kütüphaneler reflection kullanarak hep property üzerine kurgulanmış da ondan mesela Context sınıfındaki DbSet'ler property öyle değil mi bunlar reflection ile bulunuyor.

yani Context sınıfın tipini alıyor adam GetProperties() diyor sınıfıın propertylerini çekerek propertyleri elde ediyor sonra ilgili işlemlerini yapıyor gidip de field çekmiyor adam.

veya Ef Core entity sınıflarını işlerken reflection ile sınıfların proeprtylerine bakıyor fieldlar olsa bile onları işleme tabii tutmaz ki çünkü arkada GetProperties metodu var propertyleri çekiyor eğer arkada GetField metodu olsaydı olurdu ama adamlar proeprtyler üzerinde tüm işlemleri yürütmüşler.

veya Serialize ve Deserialize işlemleri propertyler üzerinden çalışır şöyle yürütürür arkada sırayla

<font color="#92d050">serialize ve deserialize arkalarında ne olduğunu görmek istiyorsan </font>

[[Consume]] => Deserialize işlemi nasıl yapılır










##### <font color="#92d050">Propertynin Türü Sınıfsa bu ne demek olur</font>

Propertinin tipi sınıf ise propertynin ismi bildiğimiz referans oluyor.

Başta null olur hiçbir şeyi işaret etmez doldurmamız gerekecek yani new yapmak.

```csharp
Deneme deneme = new Deneme();

deneme.Name => diyerek property referansına eriştik new yapacan burada yani.

deneme.Name = new MyClass(); => Propert MyClass türünde ya hani.

deneme.Name.Description = "Serkan";

public class MyClass
{
  public string Description { get; set;}
}

public class Deneme
{
  public MyClass Name { get; set;}
}
```

işte bu kadar bundan sonra propertyden ilgili sınıftan nesne oluştu artık oluşan nesneden ilgili sınıfın memberlarını kullanabilirsin.

Aslında kafan karışması çok saçmaydı çünkü proeprty tarafından bakma olaya bu aslında bir field değil mi get set olması bir anlam ifade etmez ki zaten o bloklar kullanılmıyor. demek istedğim.

```csharp
        public class MyClass
        {
            public string name;
        }

        public class Deneme
        {
             public MyClass Name;
        }
```

Görüldüğü üzere propertyleri kaldırdım field yaptım yine çalıştı yani olay şu

Deneme classından nesne oluştuğu zaman referansı ile Name field'ındana ulaşıp türü referans tipli olduğundan kullanabilmemiz için nesne oluşturulması gerekecek.



##### <font color="#92d050"> Property İmzaları</font>

Birkaç şekilde property oluşturma yolu vardır.

###### <font color="#92d050">Full Property</font>

En sadece property yapılanmasıdır.

Full propertylerde set bloğu tanımlanmazsa sadece okunabilir (Read only) veya get bloğu tanımlanmazsa (Write only) olacaktır.

Property hangi türden bir field'ı temsil ediyorsa property'nin türü de o türden olmalıdır.

Genellikle proeprty isimleri field isminin baş harfini büyük yazarak isimlendirilir.

```csharp
   private string name;
   public string Name
   {
       get
       {
          // Property üzerinden değer talep edildiğinde bu blok tetiklenir.
          // yani değer buradan gönderilir.
          
          // return name;
          return name+" Serkan"
       }
       set
       {
          // Property'e değer gönderileceksek set blou tetiklenir ve bu                     gönderilen değeri value keywordü ile yakalarız.
          
          // value keywordü property'nin türü ne ise o türden olacakatır.
           name = value;
       }
   }
```

private string name => private yaptık çünkü amacımız zaten field'ı gizleyip property üzerinden field'ın değerini göndermek ama bu değil ki public olmasın field istersen public de olabilir bu kodda hata vermez ama amacımız'a ters olur.

return name => field'daki değeri direkt göndermişiz.

return name+"Serkan" => field'daki değeri değiştirip göndermişiz böylelikle kullanıcı asla bilemeyecek field'daki gerçek değeri.

Kullanımı main içerisinde.

```csharp
 MyClass m = new MyClass();
 m.Name = "Test"; // set bloğu tetiklendi
 Console.WriteLine(m.Name); // get bloğu tetiklendi
```

bana Test Serkan değeri geldi ama aslında field'a test değerini ben yolladım ama get kısmı bana test ile serkan değerini birleştirip yolladı gerçek değeri verdi ama kontrol yapıp verdi işte property budur.

<font color="#4bacc6">Read Only</font>

```csharp
   private string name;
   public string Name
   {
       get
       {
          return name;
       }
```

Set bloğu olmadığı için sadece okunur bir property oldu yani Read Only olmuş oldu.

<font color="#4bacc6">Write Only</font>
```csharp
   private string name;
   public string Name
   {
       set
       {
          name=value;
       }
```

Get bloğu olmadığı için sadece değer verilebilir property olmuş oldu yani Writer Only propery oldu.

###### <font color="#92d050">Prop</font>

Bir property field'daki değere hiç kapsülleme yapmadan değeri veriyor ve alıyorsa uzun uzun full property yazmamıza gerek yoktur.

field'daki değere müdahale olsun olmasın direk erişim yapılmasını istemeyiz ondan property kullanılır.

Prop ile propert oluşturduğumuz da field'ı kendmiz oluşturmaya gerek yoktur kendisi arakada otomatik bir field oluşturacaktır.

Prop olarak tanımlanmış imzalarda property read only olabilir ama write only property olamaz.

```csharp
 //public string name;
 //public string Name
 //{
 //    get
 //    {
 //        return name+" serkan";
 //    }
 //    set
 //    {
 //        name = value;
 //    }
 //}
 
public string Name{get; set;}
```

Prop property, full property'nin daha kısa yazılmış tarzıdır.

Full propery'de olduğu gibi manuel field tanımlanmadı compiler ona field oluşturacak arkada.

ne zaman prop property kullanılır dersen değeri verip değiştirmeden alıyorsan kullanılabilir ama içerde kapsülleme yapmak istersen kullanamazsın full property kullanman lazım.


53 de kalındı