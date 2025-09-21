
En altta iki başlık var hayati derece önemli sakın notları düxenleede silmeye kalkma



Api'yi Consume Etmek Nedir

Bir servisi yani bir apiye istek atmak ve gelen cevabı ilglil view'inde kullanmak demektir.

İki Tür Yaklasım vardır Doğrudan HttpClient sınıfını kullanmak veya IHttpClient interfacesiin kullanmak DI olacak tabi burada bir sınıf oluşacak DefaultHttpClientFactory adında bunun içindeki bir metod var CreateClient bu da sana HttpClient instancesi dönecektir yani ilkinde direk yaparken ikincisinde biraz uzuyor ama bizi iyi bir küffetten kurtarıyor.


1. **HttpClient** HttpClient'tan direk instance yani nesne oluşturmak

```csharp
public class ServiceWithHttpClient
{
    private readonly HttpClient _httpClient;
    
    public ServiceWithHttpClient()
    {
        _httpClient = new HttpClient(); // Manuel yaratım
    }
    
    public async Task<string> GetDataAsync()
    {
        return await _httpClient.GetStringAsync("https://api.example.com/data");
    }
}
```

Veya  DI'ya kaydetmeyi kendin yapardın sonra constractır'a paremetre verirdin.
neyse sonuç olarak HttpClient sınfınından instance üretiyorsun doğrudan. IhttpClientFactory gibi bir interface kaydı otomatik olarak yok.

##### Avantajları

- Kullanımı çok kolay, ekstra konfigürasyona gerek yok.
- Küçük uygulamalar için yeterli.

### Dezavantajları

- **Socket Exhaustion (soketlerin tükenmesi)** riski var:  
    Eğer her request’te `new HttpClient()` yaparsan, .NET altında sürekli yeni socket açılır ve kapatılmaz. Bu da sunucunun portlarının tükenmesine yol açar.
    
- **Konfigürasyon yönetimi zor**:  
    Header, BaseAddress, Timeout gibi ayarları her seferinde elle yapmak gerekir.
    
- **Test edilebilirliği düşük**:  
    Mocklamak veya dependency injection ile yönetmek zor.





###### <font color="#92d050">IHttpClientFactory'de ise </font>

sen üretmiyorsun Fabrika üretior adı üstünde factory yazıyor İnterfacede


```csharp
public class ServiceWithFactory
{
    private readonly IHttpClientFactory _httpClientFactory;
    
    public ServiceWithFactory(IHttpClientFactory httpClientFactory)
    {
        _httpClientFactory = httpClientFactory;
    }
    
    public async Task<string> GetDataAsync()
    {
        var client = _httpClientFactory.CreateClient(); // Factory'den al
        return await client.GetStringAsync("https://api.example.com/data");
    }
}
```

Dependency Injection da şu servis var

```csharp
builder.Services.AddHttpClient();
```

ve bu serviste ise şu Kayıt var

```csharp
services.AddSingleton<IHttpClientFactory, DefaultHttpClientFactory>();
```

yani IHttpClientFactory interface constractırda varsa sen git DefaultHttpClientFactory sınıfından bir nesne oluştur ve ver.

arkada nasıl nesne oluşacak şöyle olacka bu da

```csharp
IHttpClientFactory s=new DefaultHttpClientFactory();
```

gidecek bu Constractır içindeki interfaceye s referansı vereccek.
sonra ne olacka sen s referansından ilgili classın içindeki memberları kullanabilcen yani CreateClient metodu bu ne işte HttpClient bu sınıf CreateClient metodu HttpClient sınfından bir instance üretecek arkada ama doğrudan yazmıyoruz.

### Avantajları

- ✅ **Socket reuse (soketleri doğru yönetir)** → Socket exhaustion olmaz.
    
- ✅ **Centralized config** → BaseAddress, Header, Timeout gibi ayarları startup’ta yapabilirsin.
    
- ✅ **Typed Clients** → Servis bazlı strongly-typed client tanımlanabilir.
    
- ✅ **Polly entegrasyonu** → Retry, Circuit Breaker gibi politikalar eklenebilir.
    
- ✅ **Dependency Injection ile uyumlu** → Test edilebilirliği yüksek.
    

### Dezavantajları

- Daha fazla boilerplate (fazladan tanımlama gerekir).
    
- Çok küçük projeler için fazla detaylı gelebilir. ama büyük projeler çok kullanışlıdır.


<hr>

## Özet Karşılaştırma

| Özellik             | HttpClient         | IHttpClientFactory  |
| ------------------- | ------------------ | ------------------- |
| **Socket Yönetimi** | ❌ Manuel, riskli   | ✅ Otomatik, güvenli |
| **DNS Refresh**     | ❌ Sorunlu          | ✅ Otomatik (2dk)    |
| **Configuration**   | ❌ Her yerde tekrar | ✅ Merkezi           |
| **Multiple API**    | ❌ Karmaşık         | ✅ Kolay             |
| **Middleware**      | ⚠️ Limited         | ✅ Rich pipeline     |
| **DI Integration**  | ❌ Manuel           | ✅ Native            |
| **Testing**         | ❌ Zor mock         | ✅ Kolay mock        |
| **Memory Leaks**    | ⚠️ Risk var        | ✅ Güvenli           |

en önemlisi de ilk satırdaki soket konusu şimdi her iki yaklaşımda da HttpClient sınıfı sunucuya HTTP isteği atıyordu veri göndermek için. Sen IHttpClientFactory olanı kullanırsan bu soket yönetimi otomaitk uğraşmana gerek yok.

ama doğrudan HttpClienttan instance üretirsen eğer soketleri manuel yapacaksın nasıl yapacaksın peki şöyle using bloğu ile

```csharp
using var client = new HttpClient();
```

using yazılmasın sebebi Bu nesneyi kullan bitince otomatik temizle" demek. HttpClient için kritik çünkü socket'leri serbest bırakmamız lazım. 

```csharp
using(var client=new HttpClient())
{
  var response = await client.GetAsync("https://api.com");
}
```

bu kod ise üstteki kodun daha uzun yazımı.

Peki dersen using neden yazdık yazmasan da olur ama HttpClient sınıfınını kaynaklarını soketlerini işlem bitince serbest bırakman lazım çünkü her metodda ayrı ayrı tanımlıyoruz ya genellikle HttClient sınfını işte bundan bir sürü soket olursa bellek de donmalar olur. ondan using bloğu işlem bitince IDispose metodunu çalıştıracak ve hem bağlantıyı kapatacak ve ne var ne yok silecek yani nesne duracak ama artık o nesneye erişemeyeceksin.


<hr>



<font color="#92d050">HttpClient Nedir</font>

- `HttpClient`, başka bir sunucuya **HTTP isteği göndermek** için kullanılan .NET sınıfıdır.
    
- GET, POST, PUT, DELETE gibi metotlarla veri alabilir veya gönderebilir.
- **Soket**, bilgisayarın ağ katmanında veri göndermek/almak için açtığı bir “port ve bağlantı noktası”dır.
    
- `HttpClient` her HTTP isteği yaptığında **arkada bir TCP soketi kullanır**.

işte bu soketlerin açık kalmaması gerekir işlem bittikten sonra kapatılmalı.


- IHttpClientFactory **arka planda bir HttpClient instance’ı yaratır**, ama:
    
    - **Soket havuzunu yönetir**.
        
    - Aynı sunucuya yapılan istekte mevcut soketi tekrar kullanabilir.
        
    - Bu sayede sürekli yeni soket açılmaz.
    
    - Eski yöntem: Her `new HttpClient()` → yeni soket,  using bloğunu kendin manuel açıp dispose metodunu çalıştıracaksın ölme eşeğim ölme yani.
        
    - Factory: Soket havuzlu, yönetilen HttpClient → Aynı sunucuya yapılan istekte mevcut soketi tekrar kullanabilir. Bu sayede sürekli yeni soket açılmaz.
yani
- Eski yöntemde her client → yeni soket.
    
- Factory → soketleri yönetiyor, tekrar kullanıyor, risk yok.







<font color="#92d050">Şuan Tercih Edilmeyen yöntem Doğrudan New yapmak</font>



```csharp
 [HttpPost]
        public async Task<IActionResult> CreateCategory(CreateCategoryDto createCategoryDto)
        {
            using var client = new HttpClient();
            var jsonData = JsonConvert.SerializeObject(createCategoryDto);
            var content = new StringContent(jsonData, Encoding.UTF8, "application/json");

            var response = await client.PostAsync(_apiBaseUrl, content);

            if (response.IsSuccessStatusCode)
                return RedirectToAction("Index");

            return View();
        }
```

Dikkat edersen kodlarımız aynı sadece Eskiden adamlar HttClient sınıfnı manuel instance üretirken biz IHttpClientFactory İnterfaceden referans alarak ve DefaultHttpClientFactory Sınıfından nesne oluşturarak oluşan sınıfın içindeki CreateClient ile Aynı işlemi yapıyoruz.

CreateClient bize HttpClient metodunun arkada instancesini yapıyor yani new'liyor.
sadece yeni kullanımda artık bunlar bizden almışlar kendileri arkada yapıyor.








artık Tercih edillen IHttpClientFactory dir. yani DI da kayıtlı olan IHttpClientFactory su sınıftan nesne oluşturuyor DefaultHttpClientFactory DI container sonra sende içinde bulunan bu sınıfın metoduna yani CreateClient erişim hakkı sağlıyorsun. zaten bu interface içinde bir tane metod var o da CreateClient bu.





Yeni Yöntem ile Consume

IHttpClientFactory



Ne Yapar ?
```csharp
string content = await response.Content.ReadAsStringAsync();
```

Önce şunları anlayalım sonra ne yaptığına bakacaz

```csharp
    public async Task<IActionResult> Index()
    {
        HttpClient client = _httpClientFactory.CreateClient();
        HttpResponseMessage responseMessage= await client.GetAsync("https://localhost:7126/api/Reservations");
        if (responseMessage.IsSuccessStatusCode)
        {
            string jsonData = await responseMessage.Content.ReadAsStringAsync();
        }

        return View();
    }
```

Şimdi Kodları Açıklayarım

```csharp
 HttpClient client = _httpClientFactory.CreateClient();
```

HttpClient ile **HTTP isteklerini** yazıyoruz:

`HttpClient` bir HTTP isteği göndermek için kullanılır.

- HttpClient kullanmak = server’a “HTTP isteği” göndermek
- Amaç: veri almak veya göndermek

yani ilk adım bir http isteği atmak bunuda CreateClient metodu ile yapıyoruz.





    

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 1234

{"id":1,"title":"Merhaba dünya","body":"Lorem ipsum..."}
```

bir istek yapıldığında böyle bişey gelir. 

```csharp
  string jsonData = await responseMessage.Content.ReadAsStringAsync();
```

bu ReadAsStringAsync metodu ile yukardaki 

```csharp
{"id":1,"title":"Merhaba dünya","body":"Lorem ipsum..."}
```
kısmın  body'sinin içeriğini yani datayı alırız string formatta.
yani

await responseMessage.Content.ReadAsStringAsync(); → HTTP cevabının body kısmını string olarak döndürür.

peki neden biz string'e çeviyoruz çünkü

Çünkü HTTP’nin “dışarıdan gelen cevabı” daima bayt dizisi (`byte[]`).  
Bizde ReadAsStringAsync metodu ile Bunu encoding bilgisine göre (`UTF-8` gibi) **string**’e çeviriyoruz.


<hr>

```csharp
StringContent stringContent = new StringContent(jsonData, Encoding.UTF8, "application/json");
```


```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 1234

{"id":1,"title":"Merhaba dünya","body":"Lorem ipsum..."}
```

ikinci satırdaki kod ise header alanı get isteğinde böyle gelir ama post ve put isteğinde sen bunu kendin oluşturacaksın çünkü sen apiye istek attıyorsun getteki gibi api sana değil.

```csharp
Content-Type: application/json; charset=utf-8
```
Bu ise header alanı güncelleme ve post işleminde yani apiye veri gönderirken header alanını dolduruyoruz ya işte böyle bişey oluyor yani post ile dolduruyoruz get ile görüyoruz gibi düşünebilirsin.

```csharp
StringContent stringContent = new StringContent(jsonData, Encoding.UTF8, "application/json");
```
işte bu kod header alanın dolduruyor.

<hr>


> ASP.NET Core’da `IHttpClientFactory` kullanmak daha doğru bir yaklaşımdır. Böylece `HttpClient`’in yaşam döngüsünü yönetebilir ve performans sorunlarından (soket tükenmesi gibi) kaçınabilirsiniz.



onemli::Consume İşlemi



şunuda notunu al aam birazda araştır önce ben yinede kodlarını vereyim

System.Text.Json.JsonSerializer

```csharp
   var jsonData = JsonSerializer.Deserialize<List<GetAuthorQueryResult>>(content, options);
```

bunda - `System.Text.Json` **varsayılan olarak case-sensitive** çalışır.
    
- JSON property ismi ile sınıf property ismi **büyük/küçük harf tam olarak eşleşmeli**.
    
- Eğer farklı ise:
    
    - `PropertyNameCaseInsensitive = true` eklemelisin, yoksa deserialize sonucu `null` olur.

yani sorun şu sen properti olarak büyük harf ile başlayıtorsun AuthorId mesela json bunu authorId olarak tutuyor eğer bu kütüphaneyi kullanrısan PropertyNameCaseInsensitive metodunu false den true yapıp bu ayrımı dikkate al diyorsun yoksa propertyler eşitlenmez nede olsa AuthorId farklı authorId farklı şeyler


JsonConvert.DeserializeObject

```csharp
var values = JsonConvert.DeserializeObject<List<ResultBrandDto>>(jsonData);
```

- `Newtonsoft.Json` (Json.NET) **varsayılan olarak case-insensitive** çalışır.
    
- Yani JSON property ismi `authorId` veya `AuthorId` fark etmez, sınıf property’si ile eşleşir.
    
- Bu yüzden sen `PropertyNameCaseInsensitive` gibi bir ayar yapmak zorunda değilsin.

ama bu kütüphaende bu json ve property isimleri farkını kendi ayarı true oldugundan bizim elle yapammaız gerekmiyor. bundan JsonConvert kütüphanesiini proje genekinde kullanmısız demek.


**JsonConvert (Newtonsoft.Json)**:

- Varsayılan olarak property isimlerini olduğu gibi kullanır
- Automatic camelCase dönüşümü yapmaz
- Daha esnek ve öngörülebilir davranış

**JsonSerializer (System.Text.Json)**:

- Varsayılan olarak **exact match** arar (büyük/küçük harf duyarlı)
- DTO'daki `Name` property'si ile JSON'daki `name` eşleşmez


<hr>

En son Notlar bitterken consume ksıındınaki post ksımalarıdaki arkadaki yapıyı notuna ekle bırakıyorum notları class notundan aldım burada olmalı en son ekle



```csharp
    public async Task<IActionResult> UpdateCategory(UpdateCategoryDto updateCategoryDto)
     {
      return view();
    }
```

mesela bu örnek model binding olacağı zaman şu olacak

Tarayıcı veya client bir JSON gönderir:
```json
{
  "id": 5,
  "name": "Elektronik",
  "description": "Kategori açıklaması"
}
```

ASP.NET Core framework arka planda şunları yapar:

Senin verdiğin action metodundaki class türünü alır o türü Deserialize eder yani new yapıp sana bir instance verecek sende o instanceyi action metodunda referans ile karşılayıp kullanacan.

```csharp

// HTTP body JSON alınıyor
string body = await new StreamReader(HttpContext.Request.Body).ReadToEndAsync();

// JSON parse edip DTO instance yaratılıyor
UpdateCategoryDto updateCategoryDto = JsonSerializer.Deserialize<UpdateCategoryDto>(body);

// Metod çağrısı
await UpdateCategory(updateCategoryDto);

```

yani bu yukarıdaki kod Deserialize sonucunda şunu oluşturuyor 
```csharp
UpdateCategoryDto a=new UpdateCategoryDto()
{
	 Id = 5,
    Name = "Elektronik",
    Description = "Kategori açıklaması"
};
```

bir class oldu json dan işte bunu artık await UpdateCategory(updateCategoryDto); bu kod ile ilgil action metodu yani bu updateCategoryDto referansını veriyor ve biz de 

```csharp
    public async Task<IActionResult> UpdateCategory(UpdateCategoryDto updateCategoryDto)
     {
      return view();
    }
```

bizde referans tanımlıyoruz ve ne oluyor gidiyor net core'nin verdiği nesne referansı artık biz action metodunda farklı bir referans olara kullanıyoruz ve bu referansta yapacağımız bir değişiklik net core'nin verdiği referansdaki proeprtyleride değiştirecek gibi oluyor.


yani yukardaki örnekler ile bu gerçek bir Net Core Model binding örnegi tamamen aynı çünkü en sonda bizim action metoduna gidiyor oluşan nesneyi veriyor bizde action metodunda o tür'e ait bir class referansı tutarsak model binding mekanizması sayeinse artık iligli propertyleri kullanabiliriz.



---
---
---
---
---

###### <font color="#92d050">Deserialize işlemi nasıl yapılır</font>

<font color="#92d050">Adım 1</font>

```csharp
var user = JsonSerializer.Deserialize<UserDto>(json);
```

 JsonSerializer generic metod olduğundan aldığı UserDto tipini reflection ile UserDto'nun bütün bilgilerini alır.

```csharp
Type hedefTip = typeof(UserDto);
```

Artık JsonSerializer hangi sınıfı deserialize edeceğini biliyor.

<font color="#92d050">Adım 2</font>

 Json, Encoding.UTF8.GetBytes(...)  byte dizisine dönüştürülür.

```csharp
  string json = "{\"Name\":\"Ali\",\"Age\":30}";
  byte[] bytes = Encoding.UTF8.GetBytes(json);
```

JSON string UTF-8 byte'lara çevrilir yani diyordu ya yapay zeka byte şeklinde tutuluyor onun ispatı.

<font color="#92d050">Adım 3</font>

Serializer boş bir `Dictionary<string, object>` yaratıyor

 apiden gelen json kodları c# da Dictionary yani key value'ya çevrilir.
 
```csharp
Dictionary<string, object> dict = new Dictionary<string, object>
{
    { "Id", 1 },
    { "Name", "Serkan" },
    { "Age", 25 }
};
```

string burada key object ise value oluyor. Id key 1 ise value oluyor. zaten objectten başka bir türde olamaz mantıkken çünkü int desen her value int olacak diye bir kural yok ondan object denmiş.

<font color="#92d050">Adım 4</font>

```csharp
object obj = Activator.CreateInstance(hedefTip);
```

 verilen userDto tipinden Run time'de dinamik bir nesne oluşturulur.

Bu nesneye henüz property değerleri atanmadı, sadece hafızada yer ayrıldı.


şimdi geldik reflection ile verilen UserDto sınıfın içindeki propertyleri bulmya

```csharp
PropertyInfo[] properties = hedefTip.GetProperties();
```

sınıfın içindeki propertyleri aldık.

```csharp
foreach (var prop in properties)
{
    if (dict.ContainsKey(prop.Name))
    {
        object value = dict[prop.Name];

        // Tip dönüşümü yapılır, örneğin int, string, DateTime vs.
        prop.SetValue(obj, Convert.ChangeType(value, prop.PropertyType));
    }
}
```

SetValue diyerek Dictionary nesnesindeki değerleri UserDto sınıfının propertylerine atamadan önce

`prop.SetValue(obj, Convert.ChangeType(value, prop.PropertyType));`

<font color="#bfbfbf">Metodun imzası</font>

```csharp
public static object ChangeType(object value, Type conversionType);
```

ChangeType metodunu dikkatli incelersek static bir metot olup direk sınıf isminden çağrabilirsin nesne oluşturup çağıramazsın.

iki paremetre alıyor bu overriedesi ilk paremetre object türden bir tür istiyor ikinci paremetre ise Type sınıfı istiyor bunu typeof ile dışarıdan bir tip vereceksin.

methodun geri dönüş türü object türden olduğundan cast etmen gerekcek.

ama bu kodda gerek yok çünkü SetValue içine yazıldığından bizim ChangeType SetValue'nun ikinci paremetresi object türden istiyor bu da object döndüreceğinden herhangi bir cast'e burada gerek yok.

value => json'dan gelen değer 123 mesela.

prop.PropertyType => property’nin tipi (`int`, `byte`, `DateTime` vb.)

ama şunlarda var

```csharp
int x = (int)Convert.ChangeType("123", typeof(int));
```
ChangeType object döndüğünden içindeki değeri kullanabilmen için cast etmen gerekir.

<font color="#00b050">peki bu metodu biz şimdi neden kullandık </font>çünkü UserDto'nun propertylerin türlerini bulup o tür'e göre tür vermen lazm.

byte tanımladıysa bytle türünü yollamalıyız gidip de int yollarsan türler uyuşmadığından hata alırsın. 
işte bundan `Convert.ChangeType` metodu bu işlemi otomatik yaptı.


---

Deserailizenin son adımı ise

```csharp
UserDto user = (UserDto)obj;
```

dinamik üretilen obj nesnesi object türünde olduğu için cast işlemi yapıldı otomatik.

böylelikle propertylerin değerleri hazır git istediğin view'de kullan.

---

farkedersen fieldlar almadık çünkü serializer işlemleri property esaslı çalışır. reflection propertyleri çekti GetProperties diyerek yanlış anlasılmasın fieldlar da gayet olabilir di ama mimari propertyler üzerinden kurgulanmış eğer fieldları alsaydı GetFields metodu kullanılacaktı reflection da ama kullanılmamış.

###### <font color="#92d050">Serialize işlemi nasıl yapılır</font>

```csharp
UserDto user = new UserDto { Id = 1, Name = "Serkan", Age = 25 };
string json = JsonSerializer.Serialize(user);
```

 user değişkeni içi doldurulmuş şekilde sayfadan geldi model binding ile bunu serailize işlemine tabii tutarım.

Deserialize'de olduğu gibi seralize'de senden bir tip istiyor artık anladın mantığı tip isteniyorsa bil ki reflection arkada olacak.

<font color="#00b050">Adım 1</font>

```csharp
Tyoe tip = user.GetType();// UserDto
```

Şimdi biz deserialize yaparken Typeof kullandık çünkü o generic tipli bir metotdu ona tip vermen lazımdı verdin typeof olarak arkada kullanıldı ama serialize metodun da sen bir nesne veriyorsun bu nesnenin bilgilerini sen ulaşabilmek için run time zamanını beklemen lazım çünkü nesneler yani new'ler run time'de oluşuyor ya ondan run time metodu olan GetType kullanıldı Typeof compiler time'de çalışır ama daha new olmadıki hata alırısın ondan GetType metodu kullanıldı.

<font color="#00b050">Adım 2</font>

```csharp
PropertyInfo[] properties = tip.GetProperties();
```

UserDto sınıfının propertyleri elde edilir. Field’lar default olarak alınmaz → JSON’a sadece property’ler yazılır.

```csharp
foreach (var prop in properties)
{
    object value = prop.GetValue(user); // runtime’da property değeri alınır
}
```

bulunan bütün propertyleri tek tek dönüp GetValue ile değerlerini okuduk.

<font color="#00b050">Adım 3 </font>

Json'a dönüşecek olan propertylerin tiplerinde ufak bir sorun oluyor arkada karar veriyor bakıyor türlerine primitive tipler yani int float double bunları json kabul ediyor ama referans tipli türleri kabul etmiyor mesela Datetime bir struct olduğundan DateTime gibi JSON’da direkt karşılığı olmayan tipler **string veya özel format** olarak dönüştürülür yani.

```csharp
DateTime now = new DateTime(2025, 9, 9, 22, 30, 0);
```

api de görüyorsun ya "2025-09-09T22:30:00" datetime türleri çıktısını işte sebebi bu kendisi otomatik bir nense üretiyor Datetime structından.

<font color="#00b050">Adım 4</font>

Json'da Key-value çiftleri şekilde yazılırlar.

```csharp
{"Id":1,"Name":"Serkan","Age":25}
```

- Burada property adı → JSON key
    
- Property değeri → JSON value

en sonda bellekte tutulması için her karakter byte dizisine çevirir Serlailize metodu string olarak bu byte dizisini döndürür.

```csharp
{"Name":"Ali"}
```

Bu json byte'ye döndüğünde bu olacak => 7B 22 4E 61 6D 65 22 3A 22 41 6C 69 22 7D


<font color="#00b050"> Adım 5</font>

- Tüm property’ler işlendi → tek bir JSON string oluşturulur.
    
- `JsonSerializer.Serialize(user)` → string olarak döner.


<font color="#bfbfbf"> 🔑 Özet Mantık</font>

1. Nesnenin tipi alınır (`user.GetType()`)
    
2. Reflection ile public property’ler bulunur (`GetProperties`)
    
3. Her property’nin değeri alınır (`GetValue`)
    
4. Değerler JSON uyumlu string’e çevrilir (converter + tip kontrol)
    
5. Key-value eşleştirilip JSON string üretilir.


##### <font color="#92d050">Serialize ve Deserialize işlemlerinin daha ksıa yapılma kodları</font>

##### <font color="#92d050">JsonSerializer İle</font>

<font color="#4bacc6">Get isteğinde => GetFromJsonAsync</font>

```csharp
var response = await client.GetAsync("api/users/1"); 
var jsonString = await response.Content.ReadAsStringAsync(); 
var user = JsonSerializer.Deserialize<UserDto>(jsonString);
```

bu uzun tarzı

```csharp
var user = await client.GetFromJsonAsync<UserDto>("api/users/1");
```

bu ise yukardaki 4 satırın tek satırda yapılmış hali her şey içerde birebir bu metodda oluyor ama biz yazmaya uğraşmıyoruz.


<font color="#4bacc6">Post isteğinde => PostAsJsonAsync</font>

```csharp
// Eskisi (StringContent ile):
var jsonData = JsonSerializer.Serialize(user);
var content = new StringContent(jsonData, Encoding.UTF8, "application/json");
var response = await client.PostAsync("api/users", content);

// YENİSİ (Tek satır):
var response = await client.PostAsJsonAsync("api/users", user);
```

<font color="#4bacc6">Put İsteğinde => PutAsJsonAsync</font>

```csharp
var response = await client.PutAsJsonAsync("api/users/1", user);
```



aradan sonra buradan başlacank incelmeeler kalındıgı yerden devam edilecek ama ben şimdi son ellimegeçen notları bırakyım bunları örnke olarka dogruladıktan sonra kullanırsın

JsonConvert (Newtonsoft) Kısa Yöntemleri:

```csharp
// POST
var response = await client.PostAsJsonAsync("api/users", user);

// GET Response Read
var user = await response.Content.ReadAsAsync<UserDto>();

// GET Direct (bu yok, sadece response'dan read var)
var response = await client.GetAsync("api/users/1");
var user = await response.Content.ReadAsAsync<UserDto>();
```

JsonSerializer (.NET) Kısa Yöntemleri:


```csharp
// POST
var response = await client.PostAsJsonAsync("api/users", user);

// GET Response Read  
var user = await response.Content.ReadFromJsonAsync<UserDto>();

// GET Direct
var user = await client.GetFromJsonAsync<UserDto>("api/users/1");
```

🎯 GERÇEK Fark:

|Method|JsonConvert|JsonSerializer|
|---|---|---|
|**POST**|✅ `PostAsJsonAsync`|✅ `PostAsJsonAsync`|
|**PUT**|✅ `PutAsJsonAsync`|✅ `PutAsJsonAsync`|
|**GET Direct**|❌ YOK|✅ `GetFromJsonAsync`|
|**Response Read**|✅ `ReadAsAsync<T>`|✅ `ReadFromJsonAsync<T>`|

using System.Text.Json; // JsonSerializer burada using Newtonsoft.Json; // JsonConvert burada

dilkkat et şunu dene yani Serializer ve convert aynı dosyada olunca hata veriyor ondan ya yollarını tam belritince metodların buna dikkat et dene hatayı al sor biç.


dikkat eğer serializer kullanıyorsan ekrnada bişey görmüyorsan sebebi property isiimler büyük harf api dekiler küçük hatf ya reflection bunu dönüştürmüyor ilgili proeprtylere farklı zannetiyor onun için şu kodu yazmalınsı sonra detaylaıca incele

incele datayına kdara gir anladım ama iiçnden geç

```csharp
  public async Task<IActionResult> Index()
  {
      var responsMessage = await client.GetAsync("Testimonial");
      if (responsMessage.IsSuccessStatusCode)
      {
          var options = new JsonSerializerOptions
          {
              PropertyNameCaseInsensitive = true, // Büyük/küçük harf umursamaz
              PropertyNamingPolicy = JsonNamingPolicy.CamelCase // camelCase'e çevir
          };

          var jsonData = await responsMessage.Content.ReadAsStringAsync();
          var values = JsonSerializer.Deserialize<List<ResultTestimonialDto>>(jsonData, options);
          return View(values);
      }
      return View();
  }
```


şuna da bak

   JsonSerializerOptions options = new JsonSerializerOptions
   {
       PropertyNameCaseInsensitive = true,
       PropertyNamingPolicy = JsonNamingPolicy.CamelCase
   };

 PropertyNameCaseInsensitive = true, bu deserialization büüyk küçük harf ayrımını halleldiyo
       PropertyNamingPolicy = JsonNamingPolicy.CamelCase bu da serialze sırasında property isimlerini camelCase'e dönüştürür.
#### <font color="#92d050">Serialize İşlemi nasıl yapılır artık arkada ki işlemi anladık</font>

##### <font color="#92d050">GetAll ve GetById</font>

```csharp
    public async Task<IActionResult> Index()
    {
      1  var responsMessage = await client.GetAsync("Testimonial");
      2  if (responsMessage.IsSuccessStatusCode)
        {
      3     var jsonData = await responsMessage.Content.ReadAsStringAsync();
      4      var values = JsonConvert.DeserializeObject<List<ResultTestimonialDto>>(jsonData);
            return View(values);
        }
        return View();
    }
```

###### <font color="#00b050">Satır 1</font>

Satırın Anlamı : HttpClient ile Api'ye Http Get isteği atıyorsun

arkada olanlar

1.  TCP/IP üzerinden Http isteği gönderilir.

2. Sunucudan Json verisi gelir yani response.

3.  HttpResponseMessage nesnesi oluşturulur içinde SatusCode, Content , Headers gibi propertyler vardır.

4.  Bu sırada json byte formatında henüz  deserialize edilmedi.

Özet : Elimeze geldi veriler ama byte formatında.


###### <font color="#00b050">Satır 2</font>

Http durum kodunu kontrol ediyoruz.
200 OK gibi durumlarda true , 404,500 de ise false döndürür yani boolean türünde bir property.

Özet : genellikle gelen Http durum kodunu almamızı ve başarısız ise Deserialize etmeyi engelliyoruz bloğa girdirmiyoruz yani.


###### <font color="#00b050">Satır 3</font>

Bize gelen jsonlar byte dizisi olduğundan string'e dönüştürülmesi gerek.

yani UTF-8 byte’lar string’e çevrilir bu satırda.

```csharp
 responsMessage.Content.ReadAsStringAsync();
```

bu kodları oop açısından inceleyerim çünkü content bir proeprty ama nokta demiş metodu çağırmış sence bu uzaktan bakınca mümkün mü hayır değil mi ? .

```csharp
responsMessage.Content
```

responsMessage HttpResponseMessage sınıfıının referansı üzerinden Content propertysine ulaşılıyor.

```csharp
 public HttpContent Content
{
   get
   {
     return content;
   }
   
   set
   {
    if (NetEventSource.Log.IsEnabled())
    {
      if (value == null)
     {
         NetEventSource.ContentNull(this);
     }
     else
     {
         NetEventSource.Associate(this, value);
     }
   }
    _content = value;
    }
 } 
```

Content property’sinin tipi HttpContent olduğundan için de get set blokları işlem yapıyor ve en son sana HttpContent nesnesi get ile döndürülüyor arkada. sonra gelen nesne ile HttpContent sınıfının kullanabiliyorsun yani ReadAsStringAsync meotdunu kullanıyorsun bu meotdda bize string türünde bir değer ile byte dizisini string formata çeviriyor.

[[Polimorfizm]] => Propertynin Türü Sınıfsa bu ne demek olur
bu nota bakabilirsin propertylerin türü sınıf ise canlandırılmasını yaptık.

Daha iyi anlamak için o satırı parçalayarım.

```csharp
HttpContent content = responsMessage.Content;
```

- responsMessage → HttpResponseMessage nesnesiydi zaten.

- Content property → HttpContent tipinde nesne döndürüyor.

```csharp
Task<string> task = content.ReadAsStringAsync();

string jsonData = await readTask;
```

await gerekli çünkü ReadAsStringAsync Task string döndürüyor ondan biz tek satırda yazdığımızda bu metod yüzünden await yazıyormuşuz.


###### <font color="#00b050">Satır 4</font>

```csharp
  var values = JsonConvert.DeserializeObject<List<ResultTestimonialDto>>(jsonData);
```

Bunu zaten alttaki başlıklarda uzunca inceledik öncce reflection ile verdiğimiz ResultTestimonialDto tipi propertyleri bulunulacak sonra string jsonların değerleri bu ResultTestimonialDto bu sınıfını propertylerine setValue yapa yapa atama yapacak ama nesne oluşturacak önce tabiki.

##### <font color="#92d050">Create ve Post</font>

```csharp
        [HttpPost]
        public async Task<IActionResult> CreateTestimonial(CreateTestimonialDto createTestimonialDto)
        {
          1  var jsonData = JsonConvert.SerializeObject(createTestimonialDto);
          2   StringContent stringContent = new StringContent(jsonData, Encoding.UTF8, "application/json");
            var responsMessage = await client.PostAsync("Testimonial", stringContent);
            if (responsMessage.IsSuccessStatusCode)
            {
                return RedirectToAction("Index", "AdminTestimonial", new { area = "Admin" });
            }
            return View();
        }
```

###### <font color="#00b050">Satır 1</font>

Serialize metodu bizden Type paremetresi istiyor arkada typeof ile bü verilen tip'in bilgileri reflection kullanılarak propertlerine erişilip propertylerin isimleri ve değeri Json Formatına dönüştürülecek.

yani kodumuz şu oldu 

```csharp
{"Name":"Ali","Message":"Great service"}
```

###### <font color="#00b050">Satır 2</font>

```csharp
StringContent stringContent = new StringContent(jsonData, Encoding.UTF8, "application/json");
```

Bu satırda StringContent ile Http Body hazırlandı yani header alanını , body'i  bunları doldurdu..

jsonData => JSON string (C# nesnesi serialize edilmiş hali)

 Encoding.UTF8 => string’in byte’a çevrilme formatı yani json string ya byte çeviriyoruz apiye yollarken.

application/json => HTTP header’a eklenen content-type


StringContent ile artık elimizde json var Http post body'sine eklememiz lazım

yani bu kod artık şu olacak istek de

```csharp
POST /users HTTP/1.1
Host: api.example.com
Content-Type: application/json; charset=utf-8 // 1 Header
Content-Length: 52

{"name":"John", "age":30, "email":"john@example.com"} // 2 Body
```

JsonData body kısmına gider Encoding-Utf8 ve Application - Json ise header alanına gider.

###### <font color="#00b050">Satır 3</font>

```csharp
var responsMessage = await client.PostAsync("Testimonial", stringContent);
```

artık herşey hazır olduğundan`stringContent'i` Http post isteği olarak gönderir ve sunucudan gelen yanıtı alır.

<font color="#00b050">Satır 4</font>

```csharp
   if (responsMessage.IsSuccessStatusCode)
   {
       return RedirectToAction("Index", "AdminTestimonial");
   }
```

await ile yanıt beklenir thread blok edilmez eğer gelen cevap 200 ise başak bir sayfaya yönlendirme işlemi olur.


##### <font color="#92d050">Dikkat</font>

> [!cite]+ Unutma
> 2 tane serialize işlemi oluyor Create ve Update Post isteğinde ve GetById de. 
> 2 tane de deserialize işlemi oluyor GetAll da ve Update Get isteğinde.
