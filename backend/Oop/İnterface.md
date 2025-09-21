
İnterface içinde erişim belirleyici belirtmek zorunda değilsin interface oldugundan erişim belirleyici varsayılan public'dir.

İnterface nesnelere direkt olarak bir Arayüz/şablon oluşturulmasını ve bu arayüzün üzerinden geliştirici ile nesne arasındaki etkileşimin daha da kolaylaştırılmasını sağlayan bir araçtır.

> [!info] İnterface
> İnterface türkçesi arayüz'dür.


İnterface Abstract Classlarda olduğu gibi bir sınıfın memberlarını filtreleme yapar gerksiz memberları atar gerekli memberları rahatlıkla ulaşmka için interface yapılandırlmasını kullanılırız.

[[Abstract Class]]


Can-Do Bir sınıfın bir interface’i uygulayarak belirli işlevleri yerine getirebildiğini gösterir. interface bir classa uygulattığından memberlarını can do ilişkisidri abstract class da öyle

is-a genellikle kalıtım (inheritance) ile kurulur. kalıtım alan bir sınıf is-a ilişikisidir


C# ve OOP’de “can do” yaklaşımı bir nesnenin hangi eylemleri yapabileceğini belirtir. Bu da genellikle **interface’lerle** ya da bazen abstract class’larla temsil edilir.

is a ise
- `is a` ilişkisi **inheritance** (miras) ile kurulur.
    
- Kodun yeniden kullanılmasını ve mantıksal soyutlamayı sağlar.
    
- Polimorfizm (çok biçimlilik) kullanılarak farklı türler aynı taban sınıf gibi davranabilir.


<hr>

İnterface bir sözleşmediir

biz interfaceleri farklı bir davranışa sahip özellikleri interfacede tanımlıyoruz sonra özellikleri bir classda tanımlıyoruz böylece her davranş kendi görevini yapıyor.


İnterface referans türlüdür yani heapde saklanır değer türlü değişkenler ise int float double gibi stackde saklanır 

yani interfacden nesne oluşturulamaz ondan benim kafam daha önceden şu yapılanmadırmayı anlamıyordu

    IDeneme deneme=new MyClass();
    İşte burada biz neden MYclassı yazmadık ilk yere yada Neden IDenemeyi new den       sonra yazmadık gibi

Cevap şu olacak: new demek eşitliğin sağ tarafı nesne oluşturma yeri İnterfaceden nesne oluşturulamaz yani IDenemyi new den sorna yazamansın Peki nedne Myclassı ilk kısma yazmadık bu polimorfizm İnterface ile referans verdik ve böyle Myclass içindeki IDenemeye ait memberları referans olarka verdigimizden dolayı sadece IDeneme memberları gelecek  <i style="font-size:30px">.</i> koydugumuzda

interfacelerde Field tanımlanamaz çünkü fieldlar önceden ne  amaca yönelik tanımlanacağı bilinmez her metodun ayrı bir fieldi olacağından saçma olur bundan dolyaı interfacelerden filed tanımla kaldırılmıştır.

ama abstract classlar bir sınıf görevi gördüğünden tanımlanabilir.

<hr>

kısacası abstract class da interface'de bir kardeş gibidir interfacelerde sadece imzlar tutulur abstract classalrda ise hem imza hem normal bir clasısn yapabiliecegi tüm işlemier yapabilir her ikiside implementasyon yapar kalıtım değil.

Bir önemli fark isede bir abstract class implement edildiğinde overiede edilmesi gerekiyor ama interfacelerde öyle değil sanki implement edildiğinde kendin yazmışın gibi herhangi bir keyword koymuyorsun.

İnterfaceleri implement etme yöntemleri var üç tane

Ameleus yöntemi herşeyi sen kendin manuel olarak yapıyorsun.

İmplement yöntemi bizim hızlıca visual studionda ampüle  tıklayıp implemenet etmesidir.

Explicity İmplement yöntemi ise yine visual studioda yapılıyor ampüle tıklanınca implement all members implement seçmen gerekiyor bu şu ise yarar biz normal kullanımda public olarak işaretleriz implementasyonda memberları ama excipilty private olarak işaretler memberları 

yani Explicity İmplement  yöntemi tam olarak şudur bir interface X metdou var bir interfacde'de X metodu var aynı isimdeler aynı yapıdalar bir classsa implement ettiğinde hata vermez ama bir tane X metodu oluşur orada hata vermez sadece hangi interface bunu yaptı bilinmez bu durum Name Hiding durumudur bir interface ezildi diğer interfacedeki X çalıştı gibi ondan bu Explicity İmplement ile implementasyonu yapıyoruz


Bir class birden fazla interfaceden implement edilebilir
bir interface birden fazla interfacelerden kalıtım alabilir


```csharp
IA a=new MyClass();
a. //sadece A gelir çünkü polimorfizm olur bu da IA referansı oldugundan ilgili sınıfnın ilgili memberları
    //gelir
    
public class MyClass : IA, IB, IC
{
    public void A()
    {
        throw new NotImplementedException();
    }

    public void B()
    {
        throw new NotImplementedException();
    }

    public void C()
    {
        throw new NotImplementedException();
    }
}

interface IA
{
    void A();
}

interface IB
{
    void B();
}

interface IC
{
    void C();
```


<font color="#6495ed">Name Hiding Nedir</font>

Birden fazla interface'de aynı bulunan imzaların tek bir sınıfa uygulatılmasına Name Hldlng denmektedir.

```csharp
IX x=new MycClass(); //Explicity implement böyle referası interface olmalı

MycClass my = new MycClass();
/* my. olmaz çünkü Explicity implement olduğundan private işaretleniyordu ya gözükmezki mantıkken patlar yani kod
 burada ondan referans class ile değil interface ile vermelisin mantık tamda buradan geliyor.*/ 

interface IX
{
    void X();
}

interface IY
{
    void X();
}

public class MycClass : IX, IY
{
    //public void X()
    //{
    //    throw new NotImplementedException();//hata vermedi ama bir tane oluştu olmaz bu
    //}
    void IX.X()
    {
        throw new NotImplementedException(); //Explicity implement yapıldı
    }

    void IY.X()//Dikkat et privade olmak duurmunda Explicity implement
    {
        throw new NotImplementedException(); //Explicity implement yapıldı
    }
}
```


C# 8.0 ile gelen bir özellikle artık interfacelerin imzalarının içinide doldurabiliyorsun Bu özelliğe Default İmplemenatation denir.