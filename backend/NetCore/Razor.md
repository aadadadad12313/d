
Razor cshtml dosyasında c# kodunu yazmamızı sağlayan bir teknolojidir. 

html ile kod yazmamızı sağlayan bir teknolijdir.

Razor chtml dosyalarında html ile c# kodlarını server tarafında çalıştırılmasını sağlayan bir teknolojidir.


###### <font color="#92d050">Razor da yorum yapma</font>

Eğer razor'ın yorum satırını kulanmazsan gidip html'in yorum satırını kullanırsan sunucuda sayfa kaynaığını göster yaparsan veya developer toolstanda direk bakabilirsin her türlü görebilirsin. yorum gözükür ama giidp razor'ın yorum satırını kullanırsan gözükmez sayfa kaynağında bundan dolayı c# kodlarında razor yorumunu kullanmalısın.


Html Yorumu

```html
<!-- Bu bir HTML yorumudur -->
```

Sunucuya yorum satırı gider yani developer toolsdan bakarsa görürsün.

Razor Yorumu

```csharp
@* Bu Razor yorumudur. HTML çıktısına yansımaz *@
```

Sunucuda gözükmez yani developer toolsdan baksan bile göremezsin. çünkü sunucuya razor yorumu gitmez sadece geliştirici görebilir.

```csharp
<html>
    <head>
        <title>sas</title>
        <style>
            div{
                /* Css İçin */
            }
        </style>
    </head>

    <body>

        @* Razor için *@
        <!-- Html için -->

        <script>
            // JavaScript
        </script>
    </body>
</html>
```

yani aslında html de css de ve js de ne varsa aynen geçerli nasıl html de sen // veya /* */ yapamıyorsan cshtml'de de olmuyor. html'in yorum satırı olan 

```csharp
<!-- Html için -->
```
kullanılıyor gibi. yani js ve css de ne ise cshtml de öyle.

c# kodlarıda var artık cshtml de onun içinde razor'ın yorum satırını kullan.


###### <font color="#92d050">Razor İşlemleri</font>


```csharp
@{
    string Name = "Serkan";
}

@{
    Console.WriteLine(Name);
}
```

Normal de c# da bir scope içinde tanımlan o scop dışında erişilemez. 

burda o yok farkedersen farklı scopelerde birbirine erişim var.

aslunda yine c# scop içinde tanımlanan dışarıdan erişilemez oluyor sen burada ayrıı ayrı scopeler görsende compiler bunu çalıştırıken aynı partial class gibi bir bütün olarak tek scop da birleştirecek.

```csharp
@{
    if (true)
    {
        <h1>Doğru</h1>
    }

    else
    {
        <h1>Yanlış</h1>
    }
}
```

Razor da html yazabilmemiz kadar doğru bişey yok zaten.


```csharp
<h1>@( 3+5 )</h1>

<h1>@( 3==3 ? "Evet" : "Hayır" )</h1>
```

Süslü parentez değil artimetiksel işlemlerde parentez kullanılıyor.

Dersen Neden süslü parentez değil de parenetez kullanılıyor sebebi

sen süslü parentez ile kod bloğu açıyorsun yani konsol programını düşün oradasın bildğin değişken tanımlıyorsun if else foreach filan ama sen gidip bir çıktııyı bastıracaksan etikete süslü parenetez dersen adam c# da name diye bişey mi var der.

bu yüzden süslü parenetez olamaz kullanman gereken parenetez.

aynı durum @item.Name içinde geçerli sen gidip @ {  item.Name   } yaparsan c# da böyle bir kullanımmı var hayır farklı şekillerde kullanacaksın ki karışamayacak.

yani

```csharp
@{
    string metin = "serkan";

    <h1>@metin</h1>
}
```

metin diye bir değişken tanımladım tamam doğru c# da geçerli.

peki h1 de ne diyecek ekrana bastırmak için metin dersen anlamaz düz string algılar @ {   } süslü parentez yapsan c# bloğu açar zaten olsaydı şimdi olrudu zaten c# bloğunun içindesi olmıyor işte.

c# da @{  metin } diye bir syntax varmı hayır. yok ondan veirleri kullanırken @ ile veya @  (    ) parenetrez ile kullanabilrsin.

```csharp
@{
    string metin = "serkan";

    <h1>@metin</h1>
    <h1>@(metin)</h1>
}

<h1>@metin</h1>
<h1>@(metin)</h1>
```

hepsi geçerli. ama dikkat etmen gerek bir nokta var

@item.Name mesela tek bir işlem olduğundan  (     ) gerek yok ama bir null kontrolü gibi işlemlerde veya tarih formatlıyoruz ya birden fazla işlem de (    ) parentrez gerekli. 


Buna da dikkat et

```csharp
@
{
  foreach(){
  }
}
```

bunu böyle yapmak zorunda değilsin


```csharp
@foreach()
{
}
```

@ dedikten sonra foreach'ci sen razor'a bağlarsan razor senin foreach scopunu kullanır tekraradan bir scope açmak zorunda kalmazsın.

bağlama derken anlamışındır yani @ dedikten sonra scope açmıyoruz @foreach bitişik yazılıyor sonra açılan scope ortak kullanılıyor.

Razor'ı iç içe de kullanabilirsin

```csharp
@{
    @foreach (ResultReservationDto item in Model)
    {
        <h1>@item.Name</h1>
    } //iç içe razor oluşturma bu.
    
    @{
     foreach (ResultReservationDto item in Model)
    {
        <h1>@item.Name</h1>
     }
    }// bu da iç içe razor oluşturma sadece daha uzun yazımı üstekinin

    foreach (ResultReservationDto item in Model)
    {
        <h1>@item.Name</h1>
    }//normal razor içinde bir kod
}
```

hepsi oluyor ister git razor içinde razor tanımla ister git tanımlama zaten razor scopesinde isen.

yani 

```csharp
@{
    foreach (ResultReservationDto item in Model)
    {
        <h1>@item.Name</h1>
    }
}
```

bunun kısaltılmış hali 

```csharp
    @foreach (ResultReservationDto item in Model)
    {
        <h1>@item.Name</h1>
    }
```

budur. böyle olunca iki scope yerine tek scop oluyor.

onemli::Razor
