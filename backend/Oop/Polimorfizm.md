
##### <font color="#92d050">Konu Anlatımı</font>

Polimorfizm yani çok biçimlilik bir nesnenin kendi türü dışında bir veya birden fazla türle referans ile işaretlenmesidir.

Abstract class'ı ve interfaceden nesne oluşturamazsın ama referans olarak kullanabilirsin.

Bu zamana kadar şunu biliyorduk üretilen nesne türünden referans tanımlıyorduk başka da bir şey yapma şansımız yoktu. ama artık polimorfizm öğrendik illa oluşan nesne ile referans aynı türde olmak zorunda değil işte buna polimorfizm denir.

<font color="#bfbfbf">Örnekte Konuyu İnceleme</font>

```csharp
   İnsan m = new Erkek();
   İnsan m2 = new Kadın();

    public class İnsan
    {
         public int Id { get; set; }
    }
    
    public class Erkek : İnsan
    {
      public string Serkan { get; set; }
    }

    public class Kadın : İnsan
    {
      public string Mızık { get; set; }
    }
```

Polimorfizm olabilmesi için kalıtımın olması gerekir.

eğer kalıtım yoksa sen nesne oluşturduğun tipi gidip farklı bir türden yakalama çalışırsan hata alırsın.

yani c# da aykırı olan bir mantık aslında polimorfizm.

çünkü Erkek türünü İnsan türüne atayabilir misin normal de hayır değil mi ama polimorfizm olursa bu mümkün oluyor.

<font color="#bfbfbf">çünkü Polimorfizm, kalıtım ilişkisi sayesinde bir alt türün, üst türden referansla işaretlenebilmesidir. Bu sayede alt türdeki nesne, üst türden referansla kullanılabilir.</font>

```csharp
   İnsan m = new Erkek();
```

m => İnsan referansı ile Erkek sınıfının nesnesini işaret ediyor.

yani bu ne demek, neden hata vermiyor dersen eğer cevap:

arada kalıtım varsa bir alt tür (dervied) örneğinin üst sınıf (base) tipine atanmasına Upcasting denir. bu referans tiplleri arasında güvenli ve otomatik bir dönüşümdür.

yani 

örnekte Erkek bir dervied class değil mi İnsan sınıfının.
İnsan sınıfıda base class.

o zaman Erkek yani dervied class İnsan sınıfına atanabilir geçerli bir durum var burada.

<font color="#92d050">Neden çok biçimlilik denir çünkü</font>

1. Kullanımı :  İnsan m = new Erkek(); => İnsan türünde referans (  polimorfizm değil   )

2.  Kullanımı :   İnsan m2 = new Kadın(); => İnsan türünde Kadın'ı işaret eden bir referans.

Normalde bir tür aynı tür ile atama yapılabilirken polimorfizm sayesinde bu 2'ye çıkıyor. referans tarafı farklı tür nesne tarafı farklı tür olabiliyor çünkü kalıtım olduğundan arada atama sırasında nesne oluşturulacak taraf referans tarafındaki türden türediği için sorun olmuyor.

<font color="#bfbfbf">yani</font>

İnsan sınıfı referans olarak Erkek sınıfını gösterebilir.

İnsan sınıfı referans olarak Kadın sınıfını gösterebilir.

<font color="#bfbfbf">yani </font>

bir nesneyi birden fazla referans ile gösterebilme yeteneğine oop'de polimorfizm denir.

---

<font color="#92d050">Bir diğer önemli kısım ise </font>

referans olarak verdiğin türün davranışını sergiliyorsun.

<font color="#bfbfbf">yani</font>

m dedikten sonra nokta koyarsan, sadece Id property'sini ekranda görürsün. Serkan property'si gelmez. Çünkü sen referans olarak İnsan dedin ve İnsan sınıfını işaret ettin. Nesne de o referansın memberlarını (üyelerini) görüyor.

Düşünelim ki bir hesap makinesi uygulaması yapıyoruz ve her işlem (toplama, çıkarma, çarpma, bölme vb.) ayrı bir sınıfta tanımlanmış. Polimorfizm sayesinde, sadece ihtiyacımız olan işlemin sınıfına referans oluştururuz; böylece sadece o sınıfa ait property ve metodlar görünür, diğerlerinin detaylarıyla uğraşmamıza gerek kalmaz.

<font color="#bfbfbf">Polimorfizm Örneği</font>

```csharp
int a = 100;
byte a2 = 100;
```

100 sayısını sayı olduğundan ister int olarak ister isen byte , long olarak kullanabilirsin.

işte bu bir polimorfizm'dir. bir sayıyı birden fazla tür ile kullanabilirsin.

eğer polimorfizm olmasaydı esneklik kaybolacaktı yani bir metoda sen dönüş tipini a verdiysen return ksımıda a olmak zorunda olacaktı ama polimorfizm ile a dan kalıtım alan bir sınıfı verebiliyorsun ne de olsa döndürdüğün şey a oluyor kalıtım aldığı için.

<font color="#bfbfbf">Obejct Örneği</font>

Bütün türler objectten türediği için polimorfizm var çünkü sen string bir ifadeyi ister object türüne ver ister git string türüne ver bir değişen bir durum varmı yok.

##### <font color="#92d050"> Polimorfizm Türleri</font>

###### <font color="#92d050">Static Polimorfizm</font>

Aynı isimde birbirinden farklı imzalara sahip metodların tanımlanmasıdır.

Haliyle burada bir metodun birden fazla metodu olabilmesi polimorfizmken bunlardan kullanılacak olanının derleme zamanında bilinmesine statik polimorfizm denir.


```csharp
Console.WriteLine(Matematik.Topla(10,20));

class Matematik
{
  public long Topla( int s1, int s2) => s1 + s2;
  
  public long Topla( int s1, int s2,int s3) => s1 + s2 + s3;
  
  public long Topla( int s1, int s2,int s3,int s4) => s1 + s2 + s3 + s4;
}
```

yani overloading metodlar derleme zamanında biliniyor çünkü 3 tane Topla metodu var hangi overrideyi seçtiğin şimdi kararlaştırılıyor.

Aslında metod overloading, oop açısından bakıldığında static polimorfizm olarak kabul edilir.

###### <font color="#92d050">Dinamik Polimorfizm</font>

Dinamik polimorfizm çalışma zamanında sergilenen polimorfizm'dir. Yani hangi fonksiyonun çalışacağına runtime'da karar verilir.

<font color="#00b050">yani Method Overriding (Metot Geçersiz Kılma) virtual overiede konusu işte </font>

method overloading ile karıştırma buradaki konumuz virtual override.

```csharp
Araç a = new Taksi();

a.Çalıştır();

class Araç
{
    public virtual void Çalıştır()
    {
        Console.WriteLine("Mızık");
    }
}

class Taksi : Araç
{
    public override void Çalıştır()
    {
        Console.WriteLine("Serkan");
    }
}
```

Şimdi burada Araç referansının Çalıştır metodu mu yoksa Taksinin Çalıştır metodunun mu çalıştırılacağını runtime’da belli olacağından dinamik polimorfizm deniyor buna.

Çünkü virtual tanımlanmış compiler time'de daha nesne yok new olmadı a referansı Araç türünde daha.

Ne zaman run time'ye geçilir o zaman a referansı Taksiyi işaret eder o zaman gerçek nesnenin override edilen metodu geçerli olur ekrana serkan basılır.

Kısaca virtual overiede her zaman run time'de bakılıyor polimorfizm olsun veya olmasın. polimorfizm varsa virtual overiede durumuna dinamik polimorfizm demişler.


##### <font color="#bfbfbf">Metodlarda polimorfizm kullanımı</font>

[[Metodlar]] => metotlar notunda uzun bir örnek ile bu konuyu detaylandırdım.

```csharp
  class Hayvan
 {
 }

 class Kopek : Hayvan
 {
     public  void SesCikar()
     {
         Console.WriteLine("hav hav!");
     }
 }

 static Hayvan HayvanUret()
 {
     return new Hayvan();
 }
```

HayvanUret tipi Hayvan döndürdüğü yerde hayvan polimorfizm yok.

```csharp
 static Hayvan HayvanUret()
 {
     return new Kopek();
 }
```

ama köpek de dönebilirdi çünkü köpek nede olsa kalıtım aldığı için hayvan türünü de  içeriyor ondan tip sorunu olmaz.

##### <font color="#bfbfbf">Polimorfizm tür dönüşümleri</font>

Polimorfizm, OOP'de bir nesnenin kalıtımsal açıdan ataları olan referanslar tarafından işaretlenebilmesidir. Haliyle ilgili dönüştürülebilmektedir.


<font color="#4bacc6">Örnek üzerinden inceleme</font>

```csharp
public class A
{
public int X { get; set; }
}

public class B:A
{
public int Y { get; set; }
}

public class C:B
{
public int Z { get; set; }
}
```

```csharp
A a = new C();
```

Base class referansı dervied class nesnesini tutabilir çünkü C sınıfı bir A'dır.

a referansı ihtiyaç olduğu takdirde kalıtımsal ilişkide olduğu referanslar'a dönüştürülebilir.

###### <font color="#4bacc6">Upcasting (Yukarı doğru dönüşüm)</font>

bir alt sınıf (dervied) nesnesinin üst sınıf (base) referansına atanmasıdır. Bu işlem otomatik yani implicit gerçekleşir. 

 ```csharp
 C c = new C;
 A a = c; // alt sınıf üst sınıfa atanabilir
 B b = c; // alt sınıf üst sınıfa atanabilir
 ```

 C dervied class A base class ataması olduğundan C zaten bir A değil mi zaten ondan direk karşılayabiliyor.

Upcasting aslında Implicit dönüşüme benziyor yani

alt tür örneğinin üst sınıf örneğine atanmasına

referans türlerinde Upcasting

Değer türlerinde ise Implicit denir.

###### <font color="#4bacc6">Downcasting (Aşağı doğru dönüşüm)</font>

bir üst sınıf (base) referansının alt sınıf (dervied) türüne dönüştürülmesidir.

```csharp
A a = new C();
B b =(B)a;
C c = (C)a;
```

b değişkeni cast sonucunda B referansını elde edecek C nesnesini işaret edecek.

ama bu demek değil ki a referansı artık kullanmaz hayır o da kullanılabilir.

peki neden cast yaptık da direk a diye yazmadık çünkü otomatik alt tür üst tür'ün referansını karşılayamaz.

 cast yaptık, çünkü bütün sınıflar object’ten kalıtım alıyor. Bir türü başka bir türe çevirirken cast kullanman gerekiyor.

bu işlemin başarılı olabilmesi için C nesnesinin A ve B sınıflarından türmesiş olması gereklidir.

Downcasting aslında Explicit dönüşümün referans türlerinde kullanılan halidir.





 






⚡ İşte polimorfizmin gücü burada:  
Bir metod **genel tür** (base class) döndürür ama içeride **özel bir tür** (derived class) döndürebilir. Çağıran taraf ise referansı hep `Hayvan` olarak görür, ama runtime’da aslında bir `Kopek` vardır. aynı typeof daki gibi Hayvan yazan kısım onda type yazıyor ondan sanki type return ediyormuş sanıyorsun ama o sana gidiyor runtimeType ediyor.

yani bu zamana kadar typeof da takılmamın sebebi bildiğimiz

MyClass m =new MyClass(); olarak değil de

MyClass m= new MyClass2(); olara kkullanılması çünkü sen koda baktığında dersinki MyClass2 tipinde m dersin de burada iki tür var 

compile time'de m MyClass iken Run time'da ise new olacağından m artık MyClass2 türünde oluyor işte aslında bundan bu kadar uzadı kod.
