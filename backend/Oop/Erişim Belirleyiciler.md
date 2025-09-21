
- Türetilen sınıf (`Derived`) daha **geniş erişime** sahip olamaz base class’tan (`Base`).
    
- Base class’ın erişimi türetilen sınıftan **daha kısıtlı ise** `Inconsistent accessibility` hatası alırsınız.

yani base classı private veya internal dervied class ise public ise hata alırsın çünkü dervied daha üstün oluyor ya ondan base classda public olmalı.

```csharp
 abstract class Hayvan // Type
{
    public abstract void SesCikar();
}

 class Kopek : Hayvan
{
    public override void SesCikar()
    {
        Console.WriteLine();
    }
}
```

ya ikiside private olacak ya da public olacak abstract class private class public olunca dervied class daha geniş yetkisi olacağından abstract class'a göre hata verecek.


Bir sınıfın erişim belirleyicisi verilmezse varsayılanı private değildir internaldır.

private field , propertyler de geçerli bir durum classlarda internal varsayılan.

bir sınıfın erişim belirleyicisi private olamaz , protected da olamaz dikakt et zamnı geldiğinde gencay yıldız ile inceleyecez.