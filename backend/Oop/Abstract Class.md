
Bir sınıf bir sınıftan kaltım alıyorsa inheritane yani kaltım bir sınıf abstract veya interface'den kalıtım alıyorsa buna implementasyon denir.


Abstract class kullanımı bir tercih meselesidir yani kullanmasanda olur farklı yollardan da bir class ile halledebilirsin ama interfacesin bir amacı vardır.


Ne zaman kullanılmalı peki
* Hem implementasyona zorlamalı hemde bir class mantığı neyse classda metod,property gibi nasıl tanımlanıyorsa birebir buradada oldgundan sana + impement sağlar interfaceden farkı bu
* interfacede bir class gibi davranamassın içinde sadece imza tutarsın scope tanımlanmaz.


Abstract classlardan ve İnterface'den Nesne Üretilemez ama 

İnterface'de 

 IAnimal myDog = new Dog(); şeklinde mydog dog sınıfın işaret edebilir polimorfizm

Abstract Class'da ise

  Dog dog = new Dog("Buddy"); başka bir sınıf abstract sınıfı kaltım alır o sınıftan nesne oluşturulur


Önemli

kaltımda kalıtım alan sınıf nesne oluştruldugu an bir üst sınıftan da nesne oluşturuluyordu işte bu durumda abstract class eğer kalıtım alınıyorsa compiler bize nesne oluşturma izni vermiyor ama kendisi bu durumda nesne oluşturuyor compiler seviyesinde ama interface'de bu durum yok. 


```csharp
new MyClass2();

abstract public class MyClass 
//burada public olmalı kesinlikle yoksa hata veriyor asbtract ise
{
    public virtual void Yazdir()
    {
        Console.WriteLine("C#");
    }

    public abstract void Z();
    //bu zorlaki uygulanmak zorunda sınıfa abstract olduğundan
    //public'de olmalı çünkü interface'deki gibi kendisi otomatik public yapmıyoruz private olursa hata verir
    //derki sen abstract diyorsun uygulansın zorla ama private ne alaka der.
}//burası Abstract yani soyut sınıftır


public class MyClass2 : MyClass
 //kaltım demiyoruz buna implementasyon diyoruz
{//burası ise Concrete classdır yani somut  ondan n katmanlı mimaride böyle isimlendiriyormuş.
    public override void Z()
    {
        Console.WriteLine("dada");
    }
    //interface gibi otomatik implement ediliyor.
     //dikkat edersen virtual yok ama overiede ediliyor ama illa virtual olarak tanımlanması gerekmiyor
     //yani abstract ile tanımlansanda overiede edebilirsin.

}
```


Abstract bir sınıf abstract bir sınıfa kalıtım verir implementasyon yapmaz ama abstract bir sınıf classa verseydi implemantasyon yapardı gibi düşünebilirsin.