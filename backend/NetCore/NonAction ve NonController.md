
<font color="#4bacc6">NonAction</font>

.NET Core'da `[NonAction]` attribute'u, bir metotun aksiyon (action) metodu olarak kullanılmaması** gerektiğini belirtmek için kullanılır. 
yani sıradana bir metod düşün string int döndüren bunu sen url'de ilgili controllerdan istek attımmı sana o metodun sonucu geliyor bu istenmeyen bir durum ondan non action ile işaretlersen bu durum olmaz.

```csharp
 [NonAction]
 public string YardimciMetot()
 {
     return "Sadece içeride kullanılmalı";
 }
 //public olan herşey action olarak kabul edilir
```

### <font color="#4bacc6">Dikkat</font>

non action attribütünü controller da bir metod tanımladıysan yaz tamam kabül yaz.

ama ViewComponentte bir metod varsa ona yazma neden zaten sen ViewComponentte istek
atamazsın url de yazsanda nonAction yazmssanda ondan yazma.

ama biz controllerlara istek atıyoruz ondan public bir metod varsa ve Iaction değilse sen belirt attribütünü ama ViewComponent ise gerek yok zaten ViewComponentlere url den istek atamazsın controller değil çünkü o.

<font color="#4bacc6">NonController</font>

bir yapının Controller sınıfı olmadan veya doğrudan bir Controller olmadan çalışması anlamına gelir. Yani MVC'nin (Model-View-Controller) `Controller` kısmı olmadan yapılan işlemleri ifade eder.