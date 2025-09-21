
###### <font color="#92d050">Method Mantığı</font>


```csharp
public int GetNumber()
{
    return 42;  // 42 bir int, metodun return tipi int
}
```

Örneği inceleyerim bize neler diyor bakalım

```csharp
public int GetNumber()
```

bu kısım bir metodun imza kısmıdır.

sözleşmeye diyoruz ki int türünden bir metod tanımlaıdım ben eğer bu metodu kullanacak biri olursa bu int türünden karşılasın diyoruz gidip de şunu yapamaz.

```csharp
string sayi = GetNumber(); // Hata imza kısmı int ise int olamlı sayi türü
```

```csharp
public int GetNumber()
{
    return 42;
}
```

metodun türü int olduğundan int ile karşılayacaksın metodu yani retrun tipi 20 oldu 20 int diye değil dönüş değer int olduğu için.

zaten c# int türünde ise return kısmıda int olmak zorunda ama esas mantık olarak bakalsan olaya türü ne ise metodun onla karışlayacaksın çağırma esnasında.

çünkü polimorfizm konusunda metodu A tanımlarsın B döndürebilirsin bu geçerli bir konu şimdi burada düşün bakalım ben bu metodu A ile mi karşılayacam B ile mi c# hata vermedi işte kafa karışıklığı artık olmaması için bu notu alıyorum.

cevap Metod türü A ise A ile karşılayan her zaman.

peki neden hata vermez return kısmı farklı bir tür dersen çünkü polimorfizm varsa arada kaltım muhakkak vardı bu yüzden return edilen nesne zaten dönüş türünü içeriyor ondan türleri farklı olabilir ama aynı yerde yetişmiş bunlar.

<font color="#bfbfbf">Örnek : </font>

```csharp
 static async Task Main(string[] args)
 {
     IAnimal myDog = AnimalExtensions.MakeDog();
     // IAnimal döndü, IAnimal ile yakaladık ✅
 }

 public interface IAnimal
 {
     void Speak();
 }

 public class Dog : IAnimal
 {
     public void Speak()
     {
         Console.WriteLine("Hav hav");
     }
 }

 public static class AnimalExtensions
 {
     public static IAnimal MakeDog() // IAnimal döndürüyor
     {
         return new Dog();
     }
 }
```

Bir metodun imza kısmında olan tür o metoda ne döndürüleceğini şöyler.

yani return deki değer sadece değeri döndürür onu int olması bir anlam ifade etmiyor dönüş tipini imza ksımı belirliyor metod içinde sadece değer dışarıya yollanıyor.

<font color="#bfbfbf"> Peki Neden Dog Değil de IAnimal?</font>

Çünkü metodun imzası `IAnimal` döndüreceğini söylüyor. İçeride `Dog` yaratsa da, dışarıya `IAnimal` olarak çıkıyor.

```csharp
// Bu YANLIŞ olur:
Dog myDog = AnimalExtensions.MakeDog(); // ❌ HATA! 
// Çünkü metod IAnimal döndürüyor, Dog değil
```

Kısaca : Metodun dönüş türü = Yakalayacak değişkenin türüdür.

Peki Neden Hata vermedi metod biz mesela int türünde tanımladıysak metodu gidipde string return edemiyoruz değil mi işte burada aslında yine aynı mantık var türler farklı olabilir ama bunlar kalıtım aldığı için birbirinin tüleri olabiliyor.

yani Dog döndürdüğünde aslında IAnimal döndürüyorsun arada kalıtım olduğundan herhangi bir sorun olmuyor çünkü birbirinden kalıtım alıyorlar.

<font color="#92d050">yani Alt tür → Üst türe dönüştürülebilir (Upcasting)</font>

```csharp
// Bu ikisi aynı şey:
IAnimal myDog = new Dog();  // Direkt yaratım
IAnimal myDog = AnimalExtensions.MakeDog();// Factory method ile
```

bütün polimorfizm olan metodlar zaten arkada new yapılıyor factory metod oluyor yani biz görmüyoruz bende bunun örneğini yaptım.

Konu Hakkında ilgili Gerçek Örnekler <font color="#4bacc6">=></font> [[Context Sınıfı]] bu başlığa bak <font color="#4bacc6">=></font> Where metodu mantığı arkada ne döndüğü bakmadan geçme

[[Polimorfizm]]<font color="#4bacc6"> => </font>bu başlığa bak  <font color="#4bacc6">=></font>  Metodlarda polimorfizm kullanımı

###### <font color="#92d050">Method Overloading (Metot Aşırı Yükleme)</font>

Aynı isimde birbirinden farklı imzalara sahip metodların tanımlanmasıdır.

Yani hazır metotları çağırırken çıkıyor ya, 5 tane override’ı var: 2 parametreli, 3 parametreli... İşte onlar Method Overloading, yani aynı isimde tanımlanmış ama parametreleri farklı olan metotlar.

Metod Overloading aslında bir polimorfizm'dir. 

polimorfizm çeşitlerinden ise static polimorfizm'e örnektir.

çünkü aynı isimdeki metodların hangi imzanın kullanılacağı derleme zamanında bilineceğinden static polimorfizm'e örnektir.

```csharp
Console.WriteLine(Matematik.Topla(10,20));

class Matematik
{
  public long Topla( int s1, int s2) => s1 + s2;
  
  public long Topla( int s1, int s2,int s3) => s1 + s2 + s3;
  
  public long Topla( int s1, int s2,int s3,int s4) => s1 + s2 + s3 + s4;
}
```

yani overloading metodlar derleme zamanında biliniyor çünkü 3 tane Topla metodu var hangi overriedeyi seçtiğin şimdi kararlaştırılıyor.

aslında metod overlading oop açısından bakıldığında static polimorfizm olarak kabul edilir.


onemli::Metod Overlading