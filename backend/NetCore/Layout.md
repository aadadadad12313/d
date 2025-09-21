
Layout sayfasında iki tane metod çok önemli birincisi renderBody zaten biliyorsun ilgili layout'u kullancak sayfanın içindeki sonuç layout dosyasına basmaya yarar.

ikincisi renderSection ise layout sayfasında tanımlanır ve bu renderSection'ı kullacak sayfaların özel isteklerini yerine getirir daha çok sayfa yüklendiğinde şu javaScript olayı olsun gibi kontrol etmek için kullanılır.

```csharp
 @RenderSection("Scripts", required: false)
```

ilk paremetre özel bir isim bunu layout'u kullacak sayfada kullancaksın ikinci paremetre ise false olmalı çünkü varsayılan true bu da şunu yapıyor şimdi bu layout'u 50 tane html sayfasını kullanıyor diyerim 50'sinde de Script adında renderSection o sayfada tanımlamalısın yoksa hata verir true oldu için iste burada ben bazı sayfalarda renderSection kullacam bazılarnda kullanmayam dersen false vericeksin kullanmadın sayfalar hata fırlatmayacak yoksa hata fırlatır.

```csharp
@section Scripts {
    <script src="~/lib/microsoft/signalr/dist/browser/signalr.min.js"></script>
    <script type="text/javascript">
        $(document).ready(()=>{
            var connection = new signalR.HubConnectionBuilder()
                .withUrl("https://localhost:7126/CarHub")
                .build();
            $('#constatus').text(connection.state);

            connection.start()
                .then(() => {
                    $('#constatus').text(connection.state);
                   setInterval(()=>{
                        connection.invoke("SendCarCount");
                   },1000)
                }).catch((err)=>{console.log(err)});

                connection.on("ReceiveCarCount",(value)=>{
                    $("#carCount").text(value);
                });
        });
    </script>
}
```

Script ismi farkdersen layoutta renderSection içinde tanımladığın isim içine js kodları yazılmış ve layoutta altta tanımlandıgından sayfa bittikten sonra çalışacak ve normal direk script yazmaktan daha kontröllü olacak.

## 📍 Mantığı

- MVC uygulaması bir View render ederken önce **`_ViewStart.cshtml`** dosyasını okur.
    
- layout belirlenir ama önce istek gelen view render edilir içindeki kodlar çalışır sonra layout dosyasına kodları aktarılır ve çalıştırılır.

-  yani tamam viewstart en erken çalışır ama layouttu okur çalıştırmaz önce action olan view çalışır kodları viewdataları filan render edilir okunur bunlar bittikten sonra en son layoutta basma yapılıp sayfa açılır.

   yani kafa karışıklığı olacak bişey yok biz normal de nasıl tanımlarıdık layout yolunu gider action içindeki viewde tanımlardık önce orası okunur du ama çalışmıyormuş demek alttaki kodları da okuyor sonra layouttu okuyor ve ekrana basıyor bunda sorun yoktu.

    benim kafamı karıştıran viewstart viewlerden önce çalışan bir dosya olması tamam en önce çalışıyor sonra gidip action view'inin kodlarını çalıştırıp layoutu render ediyior yine aynı bak durum.

    burada viewstart ilk çalışıyor tamam viewstart kullanmasydın da zaten view'in en üstüne zaten yazıyoesun layout'u ondan yine aynı mantık staticten dinamiğe çektik ama asla layout dosyası başta çalışmaz çalışırsa viewdatalar filan daha render edilmediginden ilgil action metodunu view'i layoutta baya bir yer null olur.

    ya zaten sen partial konsuunda dedin ki view içindeki viewbaglar layoutta kulalnılabilir dedin mantıkken view önce çalışmassa layout nasıl viewbagleri kullansın tam tersi olsa önce layout çalışşa sonra view çalışşa olmaz ki ondan önce view sonra layoutt çalışıyor.
