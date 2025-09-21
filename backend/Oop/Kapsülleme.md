

Kapsülleme, bir nesne içerisindeki dataların (field'lardaki verilerin) dışarıya kontrollü bir şekilde açılması ve kontrollü bir şekilde veri almasıdır.

```csharp
  public class Program
  {
      static async Task Main(string[] args)
      {
          MyClass m = new MyClass();
          m.Para = 10;
          Console.WriteLine(m.Para);

      }
  
      public class MyClass
      {
          private int tutar;

          public int Para
          {
              get
              {
                  return tutar*5;
              }
              set
              {
                  tutar = value/5;
              }
          }
      }
  }
```

Bu örnekte property'e 10 değerini verdik ama property içinde yaptığımız kontrol ile değerin yarısını verdik field'a yani kapsülleme yaptık burada.

property'e değer gönderirken gönderen 10 verdim sanıyor ama field'ın değeri 2 oluyor.

aynı işlem get bloğunda da var. gerçek değer 2 ama sen onu dışarıya 5 katı olarak gönderiyorsun.

yani metod gibi işlemler yapıyoruz propertylerden sadece tek farkı metodlardan bir özelliği belirten durumlarda yani Name,Surname,Price gibi özelliklerde property kullanıyoruz bu kadar.