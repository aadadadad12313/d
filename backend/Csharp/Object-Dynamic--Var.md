

###### <font color="#92d050">Var</font>

Şimdi önce şunu Bilmelisin var'ı tam olarak anlamak için

sen sağ sol prensibini hatırla sol taraf tür belirtiyoruz sağl tarafta ise o türe ait değer tutuyoruz.

senin en sık yaptığın hata önce sol tarafı düşünüyorsun hayır hep sağ tarafı düşüneceksin sağ tarafta mesela 10 var o zaman bu sol tarafta int olarak tutacam diyeceksin. 

sürekli bu şekilde düşünmek zorunda kalmamak amacıyla sorumluluğu compile'ra bırakmak en doğrusu ondan var keywordü kullan rahat et.

var'ı genellikle diiler arası Entegrasyon da ortak bir nokta buluşmak için tasarlamışlar mesela java decimal yok BigDecimal var mesela bir kodu her iki dile yazacaksan ayrı ayrı tanımlamk yerine ortak bir nokta olarak var kullanmak daha mantıklı her dilde kendi türünde çalışır.

ama şuan sektörde herkes bu amacın dışına çıkmış üşendikleri için tipleri yada bilmiyorlar tipleri kendileri direk var yazıyor geçiyor işte bu durum gereksiz bir maliyet yapar nede olsa türler compile time da az da olsa bir zaman harcayacak ondan bu gereksiz maliyeti oluşturmamak amacıyla türünü kendin ver biliyorsan.

var keyword'ü derleyici tarafından compile zamanında işlenir. Derleyici, atanan değere bakarak tipi otomatik olarak çıkarır ve `var`'ı gerçek tip ile değiştirir.

```csharp
var number = 42; // Derleyici bunu int number = 42; olarak görür
```

Şimdi Burası Önemli

Dersen eğer var compile time'da çalışıyor ama intelisence gibi şeyler f5'e bile basmadan anında kullanabiliyorsun türü bile anında belli oluyor bunun sebebi ne ?

Cevap: IDE aslında arkada mini bir derleyici çalıştırıyor⚙️

Visual Studio, Rider, VS Code gibi IDE’ler sen yazarken:

- Kodunu sürekli arka planda parse (çözümleme) ediyor.
    
- Bir nevi “küçük bir derleme” yapıyor.
    
- Bu sayede sen yazarken tip çıkarımını yapabiliyor.
    

Yani:

- Sen `var x = 5;` yazınca → IDE hemen arka planda `int x = 5;` diye çözümleyip tip bilgisini çıkarıyor.
    
- O yüzden `x.` yazınca → `int` tipine ait tüm metotlar (ToString, CompareTo, vs.) IntelliSense’de görünüyor.
- 
<font color="#f79646">Compile time mı bu?</font>

Evet, compile time mantığıyla yapılıyor, ama “gerçek derleme” değil.

IDE sana hızlı yardım edebilmek için derleyicinin type inference (tip çıkarımı) kurallarını taklit ediyor.

Sen F5’e bastığında asıl “gerçek derleme” yapılıyor.

yani var anında geliyor diye derleme olmuyor sadece Visual Studionun erken Küçük çaplı derlemesi bu.


Dikkat

Var Paremtre olarak verilemez 

Ne paremetre olarak ne de scope içinde değeri verilmesse hata verir çünkü adam anında değerini atayacak sen x diyosun değişken ismi koydun güzel peki değerini vermedin de var zaten değere bakıp tür atamayacak mı o zaman patlayacak.

ondan paremetre olarak var değil object,dynamic, Generic Tip yani T  kullanılabilir.


```csharp
void MyMethod(var x)
{
    Console.WriteLine(x);
}
```
Hata

Var'ın türü belli olduktan sonra tür değişmesi olamaz var diye gidip hem string sonra int filan diyemezsin. çünkü var kullandın tür belli oldu double oldu mesela tamam artık bu türden ilerleyecen.
###### <font color="#92d050">Dynamic</font>

Dynamic Var keywordüne çok benzer sadece var'ın Run Time daki versiyonudur.

Dynamic Compile Time zamanında hayla Dynamicttir Run Time da işte türü belirlenecek.


  dynamic keywordü run time çalışma zamanında belirleneceğinden şuan o dynamic olan değişken türü belli değil şöyle düşün belli olmayan yere şuan sen int mi string mi hangii iligli türün ilgili fonksiyonlarını veya memberlarını getireceğini bilemezin ondan nokta dendiğinde boş gözükür dynamic.


```csharp
dynamic a=10;
a .
```

a'nın türü hayla dynamic türü belli olmadı ama Run Time de int olarak yer alacak ama biz bunu göremeyeğiz.

gelmez türü belli değil Run Time Çalışma Zamınında berlirlenecek a'nın türü float mı string mi int mi oldugunu karar vermedi için işlem yapaiblirsin ama İntelisense olmayacağından Run Time da hata alman çok yüksek ondan dikkatlice yazıcaksın.


```csharp
 dynamic a = 5;   // int
 dynamic b = 3;   // int

// Aritmetik toplama
dynamic sum = a + b;  
Console.WriteLine($"Aritmetik Toplama: {sum}"); // 8
```


<hr>


```csharp
 dynamic s = "ss";

 Console.WriteLine(s.GetType());
```
Çıktısı String olacak yani bu kod bize ne anlatıyor compile time da türü belli olmadı ama hata vermemesi için compile time da hayla dynamic türde kaldı Run Time de türüne döndü yani string'e ve fonksiyonlarını çalıştırdı.


```csharp
         dynamic s = 10;

         s = "20";

         Console.WriteLine(s.GetType());
```

Dynamic bir önemli kuralı var o da şu türü değişebiliyor sen int ver sonra string ver hata vermez.

###### <font color="#92d050">Object </font>

object türü tüm türleri karşılayan ister değer türlü ister refarans türlü değişkenleri karşılayabilir.

aslında kalıtım alamyan bir sınıf yoktur hiç kaltım almayan bir sınıf bile Objectten kalıtım alıyor. 

kanıtı ise nesne üretirdikten sonra getType gibi metodlar var bunlar Object classından gelen metodlar.

object bütün türlerin atasıdır yani hiçbir tür yokken object vardı diyebiliriz.

yani objectte bütün türler karşılanabilir dynamicte de olur ama dynamic karşılamayı run time da yapıyor ya işte adem değil o çünkü run time de yine türünü belli oluyor ama objectte bir ademlik doğrudan var atası çünkü dynamic sadece run time zamanına kadar türü belli etmediği için adem diyemeyiz bence.

object Referans Türlüdür ama Bu demek değilki değer türleri karşılayamaz hayır hem Nesneleri hem değer türleri yani ne var ne yoksa karşılıyor.

bir değeri gittin Objette verdin diyelim Objectti bir kutu olarak düşün o kutunun içine adam gitti verdiğin değeri ve türünü kaybetmeden koydu kutuya ve kutuyu kapattı.


```csharp
object a="serkan"; 
```

sen gidip foreach de filan a dersen hata alırsın çünkü a'nın türü object sen foreach de object dönmeye çalışıyon ama senin yapman gerken object a nını içindeki kutuyu açıp değerini ve türünü alman lazım onu kullanman lazım foreach de yani cast yapman lazım çünkü sen a'yı kullanmaya devam edersen a object tür object kullanırsın.

yani a yazdın mı ekrana bastırabilirsin a dersin serkan yazar object türler ekrana filan bastıramaz diyr bişey yok ama object türde bastırılsın öemli burası string olarak basıtramazsın bak ondan sen bir operasyonel işlemde patlarsın objet türü sen string olarak işlemlerde kullanmak için object türü string'e çevirmen yani unboxing yaparak cast yapman lazım.

şimdi objeyi kutulamaya boxing denir.
ama kutunun içinden çıkarmaya ise unBoxing denir.

yani bu iki kavram sadece object keywordün de var gidip de dynamic de filan boxing yok sadece kutulamada var yani objette var.

yani biri sana boxing veya unboxing dedi kesinlikle objectten bahsediyordu başka bir ihtimal yok. 

<font color="#92d050"> Cast İşlemi </font>

Cast işlemi objecte özel değil kafan karışmasın cast çokca kullanılır float'ı inte çevirmede filan ama objectte cast işlemine biz unboxing kavramı ile adlandıracğız olay bu başkada bir bişeyi yok sadece kavram aslında cast = unboxing.

Cast yapabilmek için değişkenin yanına gelip () ile açıkca dönüştürmek istediğin tür'ü yazıyorsun.

```csharp
object a=10;
Console.WriteLine(a); //object türde

int a2=(int) a; //object türden int'e dönüştü
Console.WriteLine(a2);
```

`(int) a` ile önce a int'e dönüştürüldü sonra a2 değişkenine atandı. 

unboxing yani cast yapıldı ama dikkat et boxing tür ne ise o türden unboxing yapılır sadece int ise int unboxing olur gidipde string boxing edileni  int olarak unboxing yapamazsın.

Şimdi bir oop incelemesi yaparım diyoruz ya object bütün değerleri alabilir tamamda 

sağ sol prensibi olarak düşün   object = object olmalı ama object kullanıdığında

objejt = int gibi bişey oluyor yani tür hatası sence neden olmuyor çünkü oop açısından bakarsan 
int iki türü var birincisi int ikinci objectten kalıtım aldıgından object türüne sahip.

object = int atamasında int aslında objectten kalıtım aldığından bu atamada sorun çıkmaz. çünkü C#’ta alt sınıf (derived class) nesnesi her zaman üst sınıf (base class) türüne atanabilir, çünkü alt sınıf üst sınıfın tüm özelliklerini içerir.

```csharp

            MyClass2 s2 = new MyClass2();

            MyClass s= s2;

            Console.WriteLine(s.Name);

            Console.ReadKey();

    public class MyClass
    {
        public string Name { get; set; } = "serkan";
    }

    public class MyClass2:MyClass
    {
        public int Age { get; set; }
    }
}
```

MyClass2 burada tür olarak hem Myclass2 türünde hemde kalıtım aldığı türde iki türü var yani tek tür'ü yok ondan istediğini istediğin anda kullanabilirsin.

Bir diğer örnek ise

```csharp
class A { }
class B : A { }
class C : B { }
class D : C { }
class E : D { }

class Program
{
    static void Main(string[] args)
    {
        E e = new E();

        A a = e; // geçerli
        B b = e; // geçerli
        C c = e; // geçerli
        D d = e; // geçerli
        E e2 = e; // geçerli (kendisi zaten)

        object o = e; // geçerli (her şey object’e atanabilir)
    }
}
```

Bu örnekte ise E sınıfı nesne oluşturma aşamasında

E e =new E(); olur referans E türünde ise E'Ye ve E' sınıfının miras aldığı bütün sınıf propertyleri gelir.

B b=new E(); olursa E' sınıfından nesne oluşur ama B sınıfı referans olduğundan B sınıfın ve A sınıfının Referansları Ekrana gelir C D gelemez çünkü referans ne ise o kısma kadar referanslarını içerecek.


object türüne verilmiş bir değer kendi değerinde tutulur yani özelliğini kaybetmez buna boxing denir.

object türünde verilen bir veriyi kendi değerine dönüştürmeye unboxing denir.

> [!note] Önemli
> Yani kafama takılıyor diyorsun ya Net Coreda neden herşeyi objectten türetmişler işte bundan her türlü veriyi alabilmesinden çünkü her metod object türünden istiyor biz veriyi veriyoruz kendisi arkada unboxing yapıyor biz değeri vererek boxing yapıyoruz olay bitiyor. sen ister o ilgili metoda int türünden ver ister string ver hata vermez mantıkken


Objecte verilen bir veri direk kullanılamaaz içinde o türe ait tür ve değer durur buxing yapılma işlemi diyorduk buna ama o türü o kutudan alıp kullanmak için boxing yapmalıyız.

object unboxing yapılmadan kullanılamaz çünkü türü object oldugundan boxinge tabii tutulur bundan içindeki türü unboxing yaparak tutup çıkarmalıyız.


```csharp
  int sayi = 42;
  object kutu = sayi; //boxing      

  int tekrarSayi = (int)kutu; //unboxing
```


<font color="#92d050">As Operatörü</font>

as operatörü yalnızca sadece sınıflar ve nullable tipler için geçerlidir.

başarısız olursa null döner castteki gibi hata fırlatmaz bundan as daha güvenli en azından null dönüyor hata fırlatmasından iyi bişey.

int, float gibi değer tiplerinde çalışmaz.

çünkü as operatorü bir hata olduğunda null döndürecek ama değer türlü değişkenler null kabul etmeyeceğinden kullanılmaz temel sebebi bu.


<font color="#4bacc6">Söz dizimi:</font>

```csharp
nesne as hedefTuru
```

- nesne: dönüştürmek istediğin nesne.
- hedefTuru: nesneyi dönüştürmek istediğin sınıf veya interface.


```csharp
A a = new B();
B b = a as B;
```

veya cast ile => B b = (B)a;

###### <font color="#92d050">Özet Ve İncelemeler</font>

var bir keyword iken object ise bir türdür. var atanan değerin türüne bürünürken, object atanan değeri Boxing yaparak object'te dönüştürür.

Var Derleme aşamasında değerin türüne bürünürken
Dynamic ise Run Time de verilen türün değerine bülünecektir.

Object ise Derleme zamanında sadece `object`’in üyelerini görürsün, gerçek tipini bilemezsin.
runtime de **:** İçinde hangi tip varsa (`int`, `string`, `Person` vb.), o tip çalışır ama erişmek için **cast** gerekebilir.

yani

```csharp
object obj = 42;          // Compile time: object olarak görülür
                          // Runtime: int değeri tutar

int value = (int)obj;     // Runtime'da tür kontrolü yapılır
//eğer cast işlemi türünde hata varsa run time de hata verir.
```

aslında boxing biz istemesek de oluyor sen object tanımlıyorsun bu boxing deniyor sen istesen de istemesende oluyor ama unboxing sana bağlı.


Aslında bir Tür int float gibi normal olanı compile time da kontrol edilmesidir yani derleme zamnında ama bazı keywordleri adamlar run time'da özel olarak yapmayı başarmış bu da bir istisnai bir durum.


Var hemen gelir türü f5'e basmadan çünkü arkada Visual Stuionun compile time yapıyor küçük kendisi hızlıca ondan gelir ama gerçekten yapmıyor Ama visual studio run time olayını asla yapamaz ne sahtesini ne gerçeğini çalışmadan kodlar asla olmaz. ondan da run time da belli olanlar intelisencesi olamaz.

onemli::Dynamic,Object,Var,Cast,As