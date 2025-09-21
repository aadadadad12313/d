```csharp
using Newtonsoft.Json;
using Microsoft.AspNetCore.Mvc.ViewFeatures;

namespace UdemyCarbook.WebUI.Models
{
    public class GenericStatistics
    {
        private readonly IHttpClientFactory _httpClientFactory;

        public GenericStatistics(IHttpClientFactory httpClientFactory)
        {
            _httpClientFactory = httpClientFactory;
        }

        // T türü generic bir tip biz metod tanımlarken türünü metod isminden sonra < > bu kısımda 
        // metedoun içindeki T leri kullanabilmemiz için bir tip belirtiyoruz yani T yi biz metodun tipini
        // belirtmiyoruz zaten böyle bişey yok sadece ama sadece metod içinde T kullabilmek için metodun isminden
        // sona <T> yazarsan Metodun içinden T kullanailirsin demek oluyor bu.
        public async Task setViewBagData<T>(
                string apiUrl,
                string viewBagKey,
                //ViewDatayı paremetre olarak verdik çünkü controllerda değiliz ya biz veri taşıma yapacağız
                //controllera ondan viewData kullandıkda classda viewData kullanamadık controller sınıfında
                //degiliz ya ondan bizde controller classından paremtre olarak aldık.
                ViewDataDictionary viewData,

                // Func bize şunu yapıyor
                // Func ile ResultStaticDto içinde bir sürü property var biz sadece carCount propertysini
                // almak istiyoruz ondan metoda parametre yollarken x=>x.carCount dedik bunu func şöyle aldı
                // Func<ResultStatisticsDto, object> selector = x => x.CarCount; burası şimdi paremetre böyle
                //kalacak tamammı biz allta bunu bu metodu tetikleceğiz. x burada sol taraf paremetre
                // sağ taraf ise o paremetrenin yapacağı işlem bir metod var aslında burada.
                //buna vereceğimiz burada x kısmı aslında
                // birazdan şu olacak values.values.CarCount olcak birazdan.
                Func<T, object> selector
                )
        {
            //func burada T türünde paremetre isteyecek object dönecek selector ise metod ismi
            var client = _httpClientFactory.CreateClient();
            var response = await client.GetAsync(apiUrl);

            if (response.IsSuccessStatusCode)
            {
                var jsonData = await response.Content.ReadAsStringAsync();
                // T verdik deserialize kısmında bunu fonksiyon isminden sonra <T> dedik ya işte onu
                //burada T kullabilmek için dedik ve kullandık.
                var values = JsonConvert.DeserializeObject<T>(jsonData);

                //Burada selector burada bir metod çünkü funclar bir delagetedir metod gibi çalışır
                //selector metoduna values nesnesini verdik. biz func tanımlarken.

                //Func<T, object> selector
                // şimdi funcda paremetre olarak T verdik T bir resultStatisticDto işte
                // object geriye dönüş değeri selector ise metod ismi
                //alttaki datavalue kısmında func çağırdık values nesnesini verdik çünkü values
                //nesnesi resultStaticDto türünde ondan sorun olmaz

                //aldık ya nesneyi func şimdi
                //Func<ResultStatisticsDto, object> selector = x => x.CarCount; bu durumdaydı
                // paremetrede şimdi biz selector ile values'u yolladık noldu yani

                // values=>values.carCount; olmadımı oldu 
                // values.CarCount demek oldu bu da ilgil bir sürü propertyden bu kadar değerden bunu buldu
                // ver değerini verdi sonrada viewDatalarına bastık değerleri yani burada func'ı istediğimiz
                // property seçtik ve aldık bundan func kullandık filtreleme gibi oldu yani.

                var dataValue = selector(values);
                viewData[viewBagKey] = dataValue;

                viewData[viewBagKey + "Random"] = new Random().Next(0, 101);
            }
        }
    }
}
```