
Myclass dan bir nesne oluşturuldugu anda hazır metodlar geliyor bunun sebebi her bir classın nasıl bir gizli consturctırı varsa bir classın kalıtım aldıgı bir sınıf vardı o sınıf Object sınıfıdır ondan geliyor bu hazır metodlar

şimdi diğeceksin ki bir sınıf kaltım aldıgında bu sınıfıda alırsa o zaman oop yapısına ters ondan dolayı kalıtım alan sınıf object kalıtımı almaz ama kaltım aldıgı sınıf Object alacağı için kalıtımı kalıtım alan sınıfada gelecek o metodlar yine aynı mantık olacak.

Hazır gelen metodlar:
Equals,GetType,GetHashCode,ToSttring


c# da kalıtım alan bir class nesne oluştururken kalıtım aldıgı bütün classlardan da nesne
oluşturulur ilk kalıtımı veren ilk class çalışır.

birden fazla constructor da aynıı sayıda paremetre veremiyorduk ama istisna var
bir construcorda int varsa türünde bir paremetereli deger mesela ikinci constructor
da ise string türünde bir tür olursa kabul oluyor ama int olsaydı olmazdı.

this keywordü zaten bu zamana kadar kullandık classların içindeki fieldlara erişmek için kullanıyorduk ama compiler bunu kendisi yaptıgından bize gerek kalmıyordu ama classın içindeki istedigimiz constractırı çağırmak istediğimizde this kullanıyoruz.

<font style="color:cornflowerblue">base ise kalıtım alan class kalıtım aldıgı base classı çağırmak için kullanılıyor </span>


```javaScript
 public class MyClass
    {
        public string Ad;

        public MyClass(string ad)
        {
            Ad = ad;
            Console.WriteLine("MyClass constructor çalıştı "+this.Ad);
        }

    }

    public class Myclass2: MyClass
    {
        public int Yas;
        public Myclass2(string ad,int yas)
        {

        }
    }
```

şimdi yukardaki kod hata verecek myClass2'nin constructırı çünkü

Myclass2 bir nesne oluşturulduğu anda myClass da ruluyor bununesne oluştun sebebi tamamen compiler yapıyor yani kod ile değil ondan dolayı compiler Mclass nesne oluşturmaya çalıştıgında constructorda paremetre var ben oraya ne verecem diyor o da hataya neden oluyor ondan Myclass2'ye base ile Myclass constructırın paremetresine veri gönderiliyor


```javaScript
namespace ConsoleApp1
{
    class Program
    {
        static void Main(string[] args)
        {

            new Myclass2("Serkan",20);

            Console.ReadKey();
        }
    }

    public class MyClass
    {
        public string Ad;

        public MyClass(string ad)
        {
            Ad = ad;
            Console.WriteLine("MyClass constructor çalıştı "+this.Ad);
        }

    }

    public class Myclass2: MyClass
    {
        public int Yas;
        public Myclass2(string ad,int yas):base(ad)
        {
            Yas = yas;
            Console.WriteLine("Ad ve yaş alan constructor çağrıldı. Yaş: "+Yas);
        }
    }
}
```

burda ilk Myclass2 constuctırı myClass consturctırını çalıştıracak ad değişkenini gidip Myclass consturctırıa verecek sonra orası çalıştıkdan sonra dönüp MyClass2 constractırı çalışacak.

```javaScript

 public class Myclass2: MyClass
    {
        public int Yas;
        public Myclass2(string ad,int yas):base(ad)
        {
            Yas = yas;
            Console.WriteLine("Ad ve yaş alan constructor çağrıldı. Yaş: "+Yas);
            base.Serkan();
        }
    }
```

yani bir classdan kalıtım alan class dogrudan base kullanılamıyor illa bir fonksiyon veya constructırın içinde olması gerekiyor yani.

```javaScript
public class Derived : Base
{
    base.Method(); // ❌ Hatalı: bu satır bir metodun ya da constructor'ın dışında
}
```

