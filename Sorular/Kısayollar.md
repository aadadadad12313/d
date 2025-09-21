
###### <font color="#6495ed">Ctrl + T</font>

Proje genelindeki dosyaların ismini hızlıca buluyor. 

En son kapatılan dosyaları gösteriyor böylelikle hemen kapattığını hızlıca açabiliyorsun.

ama anlık kapatılan dosyayı hemen görmek istiyorsan 

![[Pasted image 20250906172747.png|500]]

kırmızı alanların olduğu yere bir defa tıklaman gerkeiyor öyle listeyi güncelliyor.


ama cshtml yani razor dosyalarını bulmuyor.

amacı hızlıca dosyaya erişmek hiç klasörden o klasöre ondan da o klasöre girmeye uğraşmadan hızlıca bize aradığımız dosyayı bulup veriyor.


###### <font color="#548dd4">Visual Studio Ctrl Shift F  </font>

<font color="#92d050">Find in Files</font>

Proje genelinde bütün dosyaların içinde aramak istediğin bir kelime varsa Find in Files tam bunun için geliştirilmiştir.

amaç : proje genelinde dosyaların içinde aramak istediğin kelimeli dosyaya gitmek.

<font color="#92d050">Replace in Files</font>

Aslında Ctrl Shift F yapınca <font color="#00b050">Replace in Files</font> menü var ama asıl kısayolu Ctrl + Shift + H dir. sen Ctrl+ Shift + F yapınca dikkat edersen Find in Files seçili gelir Ctrl + Shift + H yapınca <font color="#00b050">Replace In Files </font>seçili gelecek.

yani internette dersen eğer Ctrl Shift f silme yaparmı diye hayır cevabını alırsın Çünkü ctrl Shift f denince Find İn Files anlıyorlar ama sen Ctrl Shift H dersen ğer Replace İn Files olarak anlayacaklar o zaman derler evet silebilirsin tabikide diye.

Proje genelinde bütün dosyaların içinde aramak istediğin bir kelime varsa ve bu kelimeyi bu araç bulduktan sonra değiştirmek istiyorsan işte bu panelin de geliştirilme amacı budur.

ilk kısma aranacak kelimeyi yaz alttaki kısma ise yeni ne koyacağını yaz sonra Replace All yap ve olay bitsin tek tek uğraşma.

mesela gidip api url'lini mi değiştirecen consumelerde 230 tane consume var diyelim tek tek uğraşma bir tık ile hepsini yapıyor.

amaç : proje genelinde dosyaların içinde aramak istediğin kelimeyi bulmak ve dosyalara gitmeden bizim yerimize proje de ne kadar o kelimeden varsa alttaki inputa yazdığımız kelime ile değiştirmek.  

sadece değiştirmek derken ekleme yapmak değil silme de yapar nasıl mı sen ilk inputa aranacak kelime ile ararsın ikinci inputa değiştirilecek kelimeyi vermessin o da ilk inputtaki kelimeyi bulur gider ikinci inputta hiç bir kelime olmadığından varsayılan olarak null algılar ilk kelimeinin olduğu dosyalarda null ile değiştirir o zamanda silmiş gibi olur.

silme olduğunda metin silinecek ama satırı silinmeyecek bundan regex kullanmalısın nasıl mı şöyle

Use regular expressions seçili olacak menüde sonra bu kodu yaz find kısmına yani ilk kutuya.


```csharp
^\s*return View\(\);\s*\r?\n
```

Eğer metinde parentez var ise regex de / birlikte kullanılmalıymış yoksa olmaz.

<font color="#92d050">Uyarı</font>

> [!todo]+
> Regex ile işlem yaptıktan sonra regexin seçili olmasını kapat çünkü daha sonra regex açık kalırsa metinler olsa bile 0 arama sonucunu verir regexi kapat muhakkak.



Not : 

Replace etmeden önce bir find ile o seçtiğin kelimeyi ara çünkü 

```csharp
var client= _httpClientFactory.CreateClient();
```
ile

```csharp
 var client = _httpClientFactory.CreateClient();
```

fark var ilkinde = sembolü client ile bitişik ikincide boşuk var aralırnda ondan eğer ilkini seçip replace yaparsan sadece client= olanlar değişir ikinci koddaki değişmez çünkü aramaya uymuyor.

ondan önce replace yapmadan bir find yap ara önce kelimeyi bak bakalım bir sürü satır ile uyuyor mu çünkü bazı dosyalarda harf kaçabilir buna dikkat et küçük harfte fark eder büyük harfte fark eder. hatta boşluk bire farkder değişmemesi için ondan dikkatli ol.

###### <font color="#548dd4">Visual Studio Ctrl F ve Ctrl H</font>

<font color="#92d050">Ctrl F</font>

bir kelimeyi dosyada aramak için kullanılır.

<font color="#92d050">Ctrl H</font>

Ctrl H ise aranacak kelimeyi dosyada bulup değiştirmek için kullanılır.

<font color="#92d050">Ctrl H veya Ctrl F üzerinden çentik iconundan ulaş sana kalmış </font>

visual studio da projenin tamamında değil ama aynı dosyada tekrarlayan kısımları mesela meditor'ün handler kısmı kopyala yapıştırırken isimleri tek tek yazmak yerine Ctrl H yapıp ilk kısma aranacak kelime ikinci kısma ise yeni kelimeyi yaz replace yap.

###### <font color="#6495ed"> Ctrl + F ,  Ctrl + Shift + F  , Ctrl + T Farkı</font>

Ctrl F açık dosya içi hızlı kelime araması yapar.

Ctrl Shift F tüm projede kelime araması yapar.

Ctrl T tüm projede dosya araması yapar ve en son kapatılan dosyayı listeler böylelikle hemen açarsın kapatılan dosyayı.


###### <font color="#6495ed">Solution Explorer İle Arama</font>

![[Pasted image 20250902165338.png|400]]


Ctrl T den daha hız kazandılır bize aslında çünkü search panelini alttan çekecenden arama yapacan derken solution explorer da direk arama kutusuna yazarsın direk eşleşen dosyayı bulurusun bence önceli solution explorer'a verilmeli.

Hem visual studio 2017 de hem 2022 de aynı yerde arama kutusu aslında bu Ctrl T ile aynı şeyi yapıyor mesela

Ctrl T view Compoenentlerin Default.cshtml dosyasını sadece Default.cshtmleri gösterirken bu Solution Explorer ise Hem Klasörünü hem Default.cshtmli gösteriyor yani viewComponent arıyorsan bence solution exploer ile arama  yap.

visual studionun 2017 Ctrl T'sini beğenmedim çünkü sabitlenmiyor alt tarafa ondan 2017 de solution explorer kullanabilirsin çünkü sürekli Ctrl T mi yapacan arama yapmak için solution zaten panel de kabak gibi duruyor oradan direk aramanı yaparsın.


###### <font color="#6495ed">Visual Studio'da Formatlama</font>

Bütün sayfayı formatlamak istiyorsan Ctrl + K sonra Ctrl + D

Ama bazen bazı yerlerin formatlamasını beğenmessen mouse ile formatlamak istediğin alanı seç sonra Ctrl + K sonra Ctrl F yaparsan sadece seçili alanı formatlar böylelikle seçmedğin alanlar formatlanmaz.

Visual Studio 2017 ve 2022 de formatlama için kısayollar aynı.

###### <font color="#6495ed">Visual Studio'da Ctrl Shift V yaparsan eğer kopyaladığın metinler panoda tutulur oradan kopyaladığın tüm metinleri bulup seçebilirsin.</font>

yani ctrl c yapınca bir önceki kopyalanan gidiyor ama ctrl shift v kısayolu ile açılan panoda kaç önce kopyaladığın metinlerede ulaşıp kullanabilirsin.


###### <font color="#548dd4">Visual Code Birden fazla satırdaki kodu değiştirme</font>

Ctrl+Shitt+L Visual Code de çoklu seçim yapmak için kullanılır eger seçtigin kelimeleri klavyeden bastıgında silmesin isityorsan Ctrl+Shitt+L  dedikten sonra sağ ok tuşuna basmalısın

###### <font color="#548dd4">Visual Code Görünmeyen Kelimelerini Silmek İçin Ctrl Shift V İle Satır Boşlukları Olmadan Kopyalanır.</font>

###### <font color="#548dd4">Visual Code Computed Bölümü ile Margin,Padding,height Gibi Bir Sürü Property Anlık Takip Edebiliyorsun Değerlerini</font>

###### <font color="#548dd4">Avatar Resimleri İçin</font>

```csharp
[iyo8 - Artbreeder](https://www.artbreeder.com/iyo8)
```


###### <font color="#548dd4">Developer Toolsdan Etikete Çift Tıklayınca Onun Etiketini Değiştirebiliyorsun </font>

###### <font color="#548dd4">Responsive Tasarımında Rahat Etmek Amaçlı Sayfayı Küçültmek Yerine İncele Kısmında Soldan İkinci İkona Tıkla Yani Toogle Device İconına Tıkla Oradan Çek Büyült</font>

###### <font color="#6495ed">Visual Studio Debug'ı kapatmadan tekrar başaltma</font>

Visual Studio da debug attıktan sonra yanlışıkla bir satırı atladın diyerim debugu durdurup yeniden çalışıtırma debugun orada restart debugin menüsü var veya ctrl shift f5 yap.

amaç zaman kazanmak tekrar kapat aç uzun sürer böylesi daha hızlı.

###### <font color="#6495ed">Visual Studio 2022 de veya 2017 de bir cshtml dosyasında sayfayı açmak için Ctrl + Shift + W yapman gerekiyor.</font>

2017 de sağ tık View Browser da olur ama 2022 de yok bu.

###### <font color="#6495ed">~ sembolünün yapımı</font>

~ işaretini daha önce Alt + 0126 kombinasyonuyla ile yapıyordum. Ancak artık daha pratik bir kısayolunu öğrendim AltGr+ü iki kez basman yeterli.

altgr + ü ye iki kez basınca iki kez ~ yapıyor ya bunun nedeni iki kez basmandan oluyor. tuşlara eğer gidip altgr + ü ye bir kez basıp sonra boşluğa basarsan bir kez erkana basar.
