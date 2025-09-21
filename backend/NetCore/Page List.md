
```csharp
 public ActionResult KolayTablolar(int sayfa = 1)
        {
            var sorgu = from x in c.Carilers
                        group x by x.CariSehir into g
                        select new SınıfGrup
                        {
                            Sehir=g.Key,
                            Sayi = g.Count()
                        };

            return View(sorgu.ToList().ToPagedList(sayfa, 10));
        }
```

Dikkat et Return View kısmı bize sadece 10 veri veriyor sonra diğer istek diğer 10 veriyi veriyor.

```csharp
@Html.PagedListPager(Model, sayfa => Url.Action("KolayTablolar", "İstatistik", new { sayfa }), new PagedListRenderOptions
{
    LiElementClasses = new List<string>
    { "page-link"}
})
```

### <font color="#00b0f0">Çalışma Mantığı</font>

```csharp
 public ActionResult KolayTablolar(int sayfa = 1)
```
 
 ilk sayfa açıldıgında buraya değer gelmeyecek bundan sayfada değer yoksa 1 olarak al dedik değer yoksa varrsa zaten sayfa=1 değerini atamaz gelen değeri alır.

```csharp
return View(sorgu.ToList().ToPagedList(sayfa, 10));
```

 sayfa → kaçıncı sayfa gösterilecek.
 10 → her sayfada kaç kayıt gösterilecek.

```css
@Html.PagedListPager(Model, sayfa => Url.Action("KolayTablolar", "İstatistik", new { sayfa }), new PagedListRenderOptions
```

ilk paremetre Model şunu yapacak :Modelde zaten bizim View ile gelen veriler yakalanacak.


ikinci paremetrede Url.Action ile dinamik linkler oluşturuyoruz new sayfa ile bir döngü var arkada şöyle  yani

“Her sayfa numarası için bir URL oluştur; bu URL’de `sayfa` parametresi o numara olsun.” yani sayfa lambdası verdik ya arkada bir döngü çalışacak pageList döngüsü var yani modelin count 

değerinine göre o hesaplıyor sayfa verdin ya lambda olarak lambadaya göre sayfa 1 sayfa 2 sayfa 3 diye o döngü çalışıyor linkte buna göre oluşuyor sen bunu action da sayfa olarak action'a 

yazmassan kod patlar çünkü link lambdaya göre oluşacagından sayfa tıklamada linke sayfa propertysi sayfa isimli action tararfından aynı isimle binding edilmeli değil mi?

```csharp
for (int i = 1; i <= Model.PageCount; i++)
{
    var url = Url.Action("KolayTablolar", new { sayfa = i });
    // sonra bu URL bir <a> tag'ına gömülür
}
```

Yani burada 1. sayfa için: `/İstatistik/KolayTablolar?sayfa=1` linki
- 2. sayfa için: `/İstatistik/KolayTablolar?sayfa=2` linki
- 3. sayfa için: `/İstatistik/KolayTablolar?sayfa=3` linki
- 4. sayfa için: `/İstatistik/KolayTablolar?sayfa=4` linki
- 5. sayfa için: `/İstatistik/KolayTablolar?sayfa=5` linki

Oluşacak.

Sonrasında oluştu ya ben bir sayfaya geçmek istiyorum tıkladığımda 2'ye mesela

```html
<a href="/İstatistik/KolayTablolar?sayfa=2">2</a>
```

sonundaki sayfa=2 action tetiklenecek ve action tarafından yakalanacak tekrar bölümler oluşacak.

Toplamda 5 link oluşuyor her defasında çünkü 10 veri varya onu hesaplıyor pageList arkada ona göre döngü ile o kadar pageList olusturuyor.

Hesaplama Mantığı :

```css
int toplamKayit = 47; // Toplam kayıt sayısı int sayfaBuyuklugu = 10; // Sayfa başına kayıt int toplamSayfa = Math.Ceiling(47.0 / 10.0); // = 4.7 → 5 sayfa
```


Özet

Önce Action Çalışısıyor sayfa diğer partialları ile bizim pageListleri de sayfaya ekliyor ilk sayfa açıldığında bir değer vermeliyiz daha ben action'ı tetikleyemem çünkü bundan 1 den başlasın diyerek action metodunun paremetresine sayfa=1 dedim.

Sonra return view kısmında dedim ki ToPagedList(sayfa, 10)); yani sayfa burada kaçıncı sayfa gösterillecek 1 oldugundan 1. sayfa. sonra ikinci paremtre ise kaç veri gösterilecek pageListte 10 veri ve bundan Model 10 veri alıyor o zaman Foreach'e 10 veri geliyor.


```css
    @Html.PagedListPager(Model, sayfa => Url.Action("KolayTablolar", "İstatistik", new { sayfa }), new PagedListRenderOptions
   {
       LiElementClasses = new List<string>
   {   "page-link"}
  
  })
```

Burada ise Model zaten bizim View ile verdiğimiz veriler.

ikinci paremetre önemli burası arkada bir döngü var burada verdiğin new {sayfa} action'a url'in sonuna bir değer veriyorsun bu değer arkada bir döngü ile sürekli veriliyor pageList A taglarına yani. mesela senin 50 verin mi var bunu 10 olmak üzere istedik ya bundan bunu hesaplıyor pageList ben kaç link butonu oluştıyım sayfada diye ona göre uygun formatta link butonu oluşturuyor ama link istiyor bizde ona uygun linkleri Url.Action ile veriyoruz Mesela Url.ActionLink diye de metod var ama bunu burada kullanmayız çünkü zaten döngü a etiketi oluşturuyor doğrudan bizden link bekliyor actionLink doğrudan link ile a etiketi de oluşturur buna gerek yok.

Tamam sayfa açıldı artık sen diyerim 2. sayfayı görmek istiyorsun o zaman şunlar olacak 2. sayfanın a etiketine tıkladığın an şu link oluştu

```html
<a href="/İstatistik/KolayTablolar?sayfa=2">2</a>
```

buna tıkladığında action tetiklenir action da paremetre olarak sayfa vermitir   linkte de sayfa propertysi oluşmuş demekki lambda kısmında verdigin değere göre oluşuyor bunlar binding edilecek ve ilgili işlemler tekrarlanacak.

Yani lamda olarak verdiğin değişken örnek alınarak bir url oluşturuluyor onu yakalıp aynı isimle actionda gerekn işlemleri yapıyoruz.