
Getter Only properties demek property get ile işaretleyip set ile işaretlemediğimiz durumdur.
ama ben nesne oluştururken değer atamak istiyorsam hata verecek çünkü readOnly oldu. bundan dolayı c# 9.0'da gelen yeni özellik olan init-only properties get; yanına init; yazıyorsun ve nesne oluşturldugu an propertylere atama yapabiliyorsun sonrasında readOnly oluyor.


```csharp
Book book = new Book()
{
    //Constructor yok ama Propertylere değer atanabilir Nesne oluşturulduğu an
    Name = "Serkan",
    //Author Çıkmıyor
    Description="Bu bir test açıklamasıdır"
};
Console.WriteLine($"{book.Name} {book.Description}");

class Book
{
    public string Name { get; set; }//bunda set olduğundan nesne kısımında değer atanabilir
    public string Author { get; }//atanamaz set yok get olduğundan ReadOnly
    public string Description { get; init; }//c# 9.0 özelliği olan init-only-property ile hem
    //redOnly olacak hemen başlangıçta değer atanacak bu sorunu düzeltmek için getirmişler.
}
```

Bu duruma Object Initializer (Nesne Başlatıcı) denir. nesne oluşturulduğu an constructor olmadan da propertylere değer atayabilirsin.

<hr>

```csharp
List<Book> books = new List<Book>()
{
    new Book() { Name = "Kitap1", Description = "Açıklama1" },
    new Book() { Name = "Kitap2", Description = "Açıklama2" }
};
```

Listelerde'de aynı durum var Object Initializer (Nesne Başlatıcı) durumu.

Record Nedir ?

Record bir objenin topyekün olarak sabit/değişmez olarak kalmasını sağlamakta ve bu durumu güvence altına almaktadır.

yani biz init-only-propertyler tanımlamak istersek recordlar bunu yapar tek tek init ile işaretlemekten daha kolaydır.

Böylece bu obje, artık değeri değişmeyeceğinden dolayı esasında objeden ziyade bir değer gözüyle bakılan bir yapıya dönüşmektedir.


```csharp
public class Person
{
    public string Name { get; set; }
}

var p1 = new Person { Name = "Ali" };
var p2 = new Person { Name = "Ali" };

Console.WriteLine(p1.Equals(p2)); // False çünkü referansları farklı
```
Bu kod false döner çünkü Classdan oluşturulan referanslar birbiirni aynısı değildir farklıymış gibi değerlendirlir birbirinden bağımsızıdırlar yani

Ama

```csharp
public record Person(string Name);

var p1 = new Person("Ali");
var p2 = new Person("Ali");

Console.WriteLine(p1.Equals(p2)); // True çünkü içerik (değer) aynı
```

Recordlarda bu durum farklı çünkü değer ön plandadır nesne değil ondan kod referansın içindeki field'a baktı Ali dğeri Ali true döndü ama classlarda bu durum false döner.

Recordlar'da classlarda olduğu gibi referans türlüdür değer ön planda nesneye göre ama referans türünde

```csharp
class MyClass
{
    public int Age { get; init; }
    public string Name { get; init; }
}
```

```csharp
MyClass myClass = new MyClass()
{
    Age = 19,
    Name = "Serkan"
};

/* myClass.Age = 20; init ile tanımlandığından dolayı değiştiremezsin birdaha ama değişitermek
  istyorsan with keywodünü kullanmalısın önceden with kewyordü yoktu yazılımcılar kendisi with fonksiyonu
  yazıyormuş ama sonra gelen güncellemelerde bunu otomatikleştirmişler with fonksiyonunuda eklemişler. */
```

```csharp
MyClass my2 = new MyClass()
{
    Age = 20, //değişmesini istediğini ise yeni değerini ver
    Name = myClass.Name //değişmesiini istemediğini aynen bırak 
};//yani yukardaki deiştirilmeme sorununu önceden böylede çözebiliyorlarmış 
//bir nesne oluşturup onun içinde değerleri güncelleyip bu nesneden devam ediyorlarımış.

```

bir kullanım tarzıda Classın içine with fonksiyonu yazma


```csharp
MyClass m = MyClass.With(15);
//burada ise yukardaki koddaki myClass ile tanımlanış nesnenin içeriğini değiştirmek istediğimizde new ile nesne oluşturmadık with metodunda değerler önce bir verilecek değişien isimler değişsecek kalcaklar kalacak sonra new myClass ile fonksiyon return edecek aynı mantık oluyor.
class MyClass
{
    public int Age { get; init; }
    public string Name { get; init; }

    public MyClass With(int property2)
    {
        return new MyClass
        {
            Age = property2,//yeni değerini aldı
            Name = this.Name//deger aynı kalsın diye tekrar atama aynı değeri
        };
    }
}
```


Bu kullanım ise Recoldlar'da geçerli olacak ve artık bu yukardaki kodları manuel yapmaktan kurtaracak kodlar

```csharp
MyRecold m = new MyRecold()
{
    Age=20,
    Name="Serkan"
};

MyRecold m2 = m with { Age = 25 };//Age değişti Name ise aynen serkan olarak kaldı.
//nesne with fonksiyonu kendisi oluşturacak verilere göre

record MyRecold
{
    public int Age { get; init; }
    public string Name { get; init; }
}
```


Positional Recold Nedir ? 

Classlardaki gibi constructor ve deconstruct tanımlamıyorsın record MyClass dedikten sonra ( ) açıyorsun değişkenlerini veriyorsın recordlar kendisi bunu otomatik yapıyor ondan sonra nesnei üret ve tupple veya constrcutor olarak kullan tanımlamadan


```csharp
MyRecold myRecold = new MyRecold("Serkan","Altunay");

var (x,y) = myRecold;//ya var ile kullan yada türlerini kendin yaz

(string a,string b) = myRecold;

Console.WriteLine(a);

record MyRecold(string Name,string LastName)
{

}
```


bu propertyleri recold'a verdigimiz zaman kendisi compiler seviyesinde construct ve destructor oluşturuyor ve propertyleri init olarak işaretliyor.


```csharp
MyRecold myRecold = new MyRecold("Serkan","Altunay");

var (x,y) = myRecold;//ya var ile kullan yada türlerini kendin yaz

(string a,string b) = myRecold;

Console.WriteLine(a);

record MyRecold(string Name,string LastName)
{
    //public MyRecold()
    //{

    //}//hata verir overiede etmiyor otomatik tanımladık ya recold () ile ondan bir constractır daha tanımalamak
    //istiyorsan this ile ilk oluşan constracıtıra değerlerini vermen gerekiyor

    public MyRecold() : this("cafaf", "fafafaf")
    {

    }//çalışır 
}
```