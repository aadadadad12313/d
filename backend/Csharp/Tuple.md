
**<font color="#92cddc">Tuple, birden fazla değeri tek bir yapıda gruplamak için kullanılan bir veri yapısıdır.</font>**

```csharp
  //1. kullanım tarzı var olmadan
  (ValueTuple<string, int>)kisi = ("Serkan", 30);

  //2. kullanım tarzı daha okunaklı
  (string ad, int yas) kisi = ("Serkan", 30);
  Console.WriteLine(kisi.ad);

  //3. kullanım tarzı ise var ile kullanımı
  var kisi = ("Serkan", 30);
  Console.WriteLine(kisi.Item1);
```
