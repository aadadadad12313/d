
bir sınıf, yapı, veya arayüzün birden fazla dosya içinde tanımlanmasını sağlar.

yani aynı isimde bir sürü class tanımlayabilrisin partial keywordü ile bu arkada bir bütün olarak oluşacak zaten. 

yani fiziksel olarak ayrı ama compiler bunu bir bütün olarak görecek tek bir instance üretecek yani.

Partial Classların içinde aynı isimde metodlar veya propertyler olamaz. nede olsa bunlar birleşecek sonunda.


<font color="#92d050">Partial Neler Olabilir şunlar olabilir </font>

Class, İnterface, Struct, Record ,Abstract Class bunlar partial olabilir.

Partial keywordü class,Interface,Struct,Class dan hemen önce gelmelidir kuralı bu.

Ama Abstract class da olaylar farklı çünkü Abstract keywordü var partial da bir keyword ve partial keywordü'nün kuralı var beni hemen class dan önce yaz ondan abstract class da partial şöyle kullanılır.

```csharp
public abstract partial class
{
}
```
Böyle olacak çünkü Abstract class da bir class yani partial'ın kuralı var beni şunlardan önce yaz diyor class dan, interface, struct, record.

###### <font color="#92d050">Partial Metodlar </font>

```csharp
 partial void x();
```
  partial da bir imza atıp gövdeyi doldurmamak istiyorsan partial olarak işaretleyeceksin.

```csharp
public void Deneme()
{
    Console.WriteLine("mız");
}
```

Partial bir class da böyle direk metotda tanımlayabilirsin ama içini doldurmak zorundasın sen eğer dersen ben içini doldurmayacam şimdilik partial olarak işaretleyeceksin.

```csharp
    public partial class Örnek
    {
        partial void X();
        partial void Y();
        partial void Z();

        partial void X()
        {
            Console.WriteLine("Serkan Altunay");
        }
}
```
Partial olarak tanımlanmış bir metod default olarak private kabul edilir. ondan sen gidip de Örnek sınıfında bir nesne oluşturmaya kalkarsan X metoduna erişmezsin.

Nasıl Erişebilrisin iki seçenek var önceden şunu yaparladı 

<font color="#92d050">1. Yöntem</font>

```csharp
 public partial class Örnek
 {
     partial void X();
     partial void Y();
     partial void Z();

     partial void X()
     {
         Console.WriteLine("Serkan Altunay");
     }

     public void Yazdir()
     {
         X(); // Veya metod yerine constractır içinde çağırırsın sana kalmış
     }
```

Tamam partial olan bir metod nesne oluşturulduğundan erişilemez kabul ama normal bir metod nesne oluşturulduğunda erişilebilir ve o normal metodun içinde partial metodun çağrısının yaparsan olur. yani söyle.

```csharp
   static void Main(string[] args)
   {
       Örnek ö = new Örnek();
       ö.Yazdir();

       Console.ReadLine();
   }
```


<font color="#92d050">2. yöntem</font>

Artık Partial metodlar yeni sürümlerde private zornluluğu kalktı gidip sen public olarak işaretlersen çalışır kodun.

```csharp
    public partial class Örnek
    {
        public partial void X();
        partial void Y();
        partial void Z();

        public partial void X()
        {
            Console.WriteLine("Serkan Altunay");
        }
}
```

public erişim belirleyicisini koyarsan direk nesne oluşturduktan sonra kullanabilirsin.

```csharp
 static void Main(string[] args)
 {
     Örnek ö = new Örnek();
     ö.X();

     Console.ReadLine();
 }
```
gördüğün gibi erişim sağlandı.

###### <font color="#92d050">Partial Metodların Dönüş Değeri</font>

Peki Türleri void mi olacak hep önceden void zorunluydu şimdi ise ister void ister int yap sana kalmış hepsi geçerli hale getirildi.

 ```csharp
     public partial class Örnek
    {
        public partial void X();
        public partial int Y();
        public partial float Z();

        public partial void X()
        {
            Console.WriteLine("Serkan Altunay");
        }

        public partial int Y()
        {
            return 20;
        }

        public partial float Z()
        {
            return 20.10f;
        }
    }
 ```

```csharp
   static void Main(string[] args)
   {
   
       Örnek ö = new Örnek();

       ö.X();
       Console.WriteLine(ö.Y());
       Console.WriteLine(ö.Z());
      
       Console.ReadLine();
   }
```

<hr>

```csharp
   public partial class Örnek
   {
      partial int Y();

        partial int Y()
       {
           return 20;
       }
   }
```

 bu kod hata verecek çünkü 

- Eğer bu metod non-void ise: hangi erişim belirteciyle çağrılacağını net olarak bilmesi gerekir.
    
- Derleyici: “Bir parçayı public yaptım mı? private mı? Eğer başka bir dosyada implementasyon varsa, bu metod başka yerden erişilebilir mi?” sorusunu sorar.

Yani non-void partial metodlarda erişim belirteci zorunludur, çünkü dönüş değeri olan metodlar başka yerden çağrılabilir ve C# bunu güvenli şekilde çözmek ister.

- Void partial metodlarda bu sorun yok çünkü:
    
    - Eğer hiç implementasyon yoksa derleyici kodu tamamen siler.
        
    - ,Erişim belirteci olmasa bile koddan silindiği için herhangi bir etkisi yok.


```csharp
   public partial class Örnek
   {
      public partial int Y();

        public partial int Y()
       {
           return 20;
       }
   }
```

dönüş değeri void'den farklı ise public olarak işaretlenmeli öyle de yapılmış kod geçerli.


###### <font color="#92d050">Özet</font>

Partialda isimleri,türleri ve namespaceleri aynı olmalı.

Sağ Tık yapıp class oluşturmada hata alırsan bil ki sebebi dosya çakışmasıdır çünkü aynı dizinde aynı isimde dosya olamaz.

hadi dosyayı farklı isimlendirdin diyerim aynı dizin de aynı isimde class da olamaz sadece partial kewordü ile işaretlenmiş yapılar aynı dizinde farklı dosyalarda aynı class isminde olabilirler.

Aynı Dll de olacak bunlar yani gidip farklı katmanda vermeyi bırak birini b klasöründe tanımlarsan namespaceleri farklı olacağından partial olarak kullanılamaz.

yani

ConsoleApp1 ayrı bir namespace

ConsoleApp1.Models ayri bir namespace 

olunca partiallar birleşmez.

Partial olan bir class, struct,interface,recor bunlarda partial keywordü ile bir metod tanımlaması yapılabilir.

Partial Metodlar ne işe yarar dersen aynı interface de nasıl imza tanımlıyorsa sonra içini dolduruyorsak partial yapılar da bize bunu sağlıyor ama farkı var tabili partial yapılar da sen imza tanımladın ama sonra içini doldurmadın hata filan vermez.

program derlenirken birleştirme esnasında bakar imzası var gövdesi sonrada doldurulmamış o zaman bunu yok sayar çünkü gövdesi doldurulmamış olduğundan.

C# 9.0 öncesinde partial metodlar void olmak zorundaydı yani dönüş değeri alamazlardı. 

C# 9.0 ile dönüş değeri alabilir hale geldiler.

Büyük ve karmaşık yapıdaki sınıfları daha okunabilir ve düzenlenebilir parçalara bölmek için partial kullanılabilir.

###### <font color="#92d050">Örnek 1</font>

 ModelMetadataType bu konuyu Data Annotations da işlemiştik.
 
  ModelMetadataType Atribütünü Entity sınıfında tanımlıyorduk ama bizim Entity sınfıını bozabilir veya Single Responsibility Principle prensibine ters düşme ihtimali var ondan bu Entity sınıfı yalnız kendi görevini yani propertyleri tutmalı validasyonu ise başkası yapmalı değil mi.

biz gidip Entity sınıfını partial yapsak sonra aynı namespace de bir sınıf daha tanımlasak aynı isim de bir partial class  sonra onda yapsak bu ModelMetadataType Atribütünü konusunun kodlarını ne de olsa en sonda birleştirlirip çalışmayacak mı hem sorumluklar olabildiğince ayrı tutulur. 

ama yine de en sonda birleşecek olsun tek yerde birlikte görmemizden iyidir.

Kodları

```csharp
namespace Net_Core_Dersleri.Entity
{
    public partial class Product
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }
}
```

```csharp
namespace Net_Core_Dersleri.Entity
{
    [ModelMetadataType(typeof(ProductMetadata))]
    public partial class Product
    {
       
    }
}
```

Visaul Studio şimdi class oluştururken soruyor sana aynı ismi koydurmuyor sen bir faklı isim koy sonra classın ismi Prodcut yap bişey olmuyor yani.

```csharp
namespace Net_Core_Dersleri.Entity
{
    public class ProductMetadata
    {
        [Required(ErrorMessage = "Ürün adı zorunludur.")]
        [StringLength(10, ErrorMessage = "En fazla 50 karakter.")]
        public string Name { get; set; }
    }
}
```

Partial kullanarak Producttan iki class birleşti hem entity sınıfımız riske girmedi hem de işimiz görürdü. ne de olsa compiler onu birleştirdi tek bütün yaptı.

###### <font color="#92d050">Örnek 2</font>


```csharp
namespace WindowsFormsApp1
{
    partial class Form1
    {
       private void InitializeComponent()
        {
            this.button1 = new System.Windows.Forms.Button();
            this.SuspendLayout();
            
            this.button1.Location = new System.Drawing.Point(275, 184);
            this.button1.Name = "button1";
            this.button1.Size = new System.Drawing.Size(75, 23);
            this.button1.TabIndex = 0;
            this.button1.Text = "button1";
            this.button1.UseVisualStyleBackColor = true;
        
        }
        private System.Windows.Forms.Button button1;
    }
}
```

InitializeComponent metodu tanımlamış Form1 classında diğer bir form1 clasında ise bunu constractırda yazmış yani amacı şu burada Form1 formu instance oluşturuldu an UI kodları önce bir çalışşın sonra eventlar çalışşın demiş.

```csharp
namespace WindowsFormsApp1
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {

        }
    }
}
```

Burası Windows formun event olayların yazıldığı bir Form1 sınıfı farkedersen partial olarak tanımlanmış bir class yani bu ne demek oluyor bunun parçaları var yani bir sürü form1 classı var compiler derlerken bunu bir bütün olarak birleştiriyor demek.

işte o diğer parçada şu Form1.Designer.cs adında bir dosya var içinde ise partial class Form1 yine var bu iki class farklı yerlerde tutulup compiler aşamasında bir bütün olsun demiş.

InitializeComponent metodu mesela ayrı bir dosyada tanımı yapılmış diğer bir dosyada ise Constractır içinde yazılmış işte parital tamamen bunu sağlıyor kod kalabılığının parçalara bölüp kullanmakta zor kullanışlı.

eğer dersen dosya ismi Form1.Designer ama class ismi partial class form1 hata vermez mi hayır vermez çünkü

C#’ta dosya adı ile içindeki sınıf adı aynı olmak zorunda değildir. 

eğer öyle olsaydı partial kullanamazdın çünkü aynı namespace de yalnızca bir dosya ismine izin veriyor program ama farklı dosya ismi oluşturup aynı class ismine program karışamaz çünkü bu c# dilinin sana sunduğu bir avantaj.

yani sorun burada Visual Studionun bizi engellemesi c#'ın değil ondan karıştırma.


<font color="#92d050">Peki bu partial bize Windows Formda nasıl bir kolaylık sağlıyor</font>

partial olmasa Designer kodunu manuel olarak Form1.cs içinde yazmak zorunda kalırdın ve tasarım kodu ile event kodları iç içe geçerdi çok karışırdı.
