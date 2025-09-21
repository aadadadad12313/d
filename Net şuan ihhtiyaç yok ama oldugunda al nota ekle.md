
Katmanları birbirine bağlarken net core de iki katman birbirini iki yönlü bağlanamaz yani mesela
persistence katmanı application'a baglandı ya sorun yok gidip sen applicaton'ı da persistenceye bagalmaya kalkarsan döngü oluşur çift yönlü vs studio engeller tik koyarsın kalkar yani.

<hr>

```csharp
 public void Update(CommentUpdateDto dto)
 {
     _context.Comments.Update(dto);
     _context.SaveChanges();
 }
```

bu kod hata verir update metodu çünkü ef core de senin tüm entitylerin kayıtlı DbSet dedik ya context de orada Sence CommentUpdateDto var mı yok adam senden Dbset olan bir entity beklliyor ondan kod patlar.

<hr>

```json
{
  "commnentId": 0,
  "name": "string",
  "createDate": "2025-08-21T15:40:12.343Z",
  "description": "string",
  "blogId": 0,
  "email": "string",
  "blog": {
    "blogId": 0,
    "title": "string",
    "authorId": 0,
    "author": {
      "authorId": 0,
      "name": "string",
      "imageUrl": "string",
      "description": "string",
      "blogs": [
        "string"
      ]
    },
```

Net core de sen meditor kullanmayıp repository pattern kullanırsan bu çıktıyı alırsan neden mi şundan 

```csharp
 public void Update(Comment entity)
 {
     _context.Comments.Update(entity);
     _context.SaveChanges();
 }
```

bu kod mesela o uzunca kodu veriyor neden çünkü sen Dto kullanmıyorsn Entity sınıfını dogrudan kulanıyorsun Commentte navigation propertyler varyani Comment ile ilişkili refernaslar onlarıda aluyor bu entity güncellerken onlar orada oldugundan.

işte sebebi bu onlar devreye girince karışıyor ortalık. meditor de böyle olmuyordu çünkü meditor de sen hatırlarsan Dto kullanıyorusn navigation propertyleri zaten almıyorsun.
işte ondan sen Comment sınıfın dogrudan kullanmayıp propertylerini navigationdan arındırıp bir sınıf oluşturursan sorun ortadan kalkar.

eğer ben repository pattern kullacam ama bu uzunca çıktıyı alamayacam direk Comment için propertyleini alacam dersen Bir sınıf oluştur ve navigation propertyler'i sınıfıtan at olay biter.

```csharp
      public void Update(CommentUpdateDto dto)
      {
          var value = _context.Comments.Find(dto.CommnentId);

          value.Name = dto.Name;
          value.Description = dto.Description;
          value.Email = dto.Email;
          value.CreateDate = dto.CreateDate;
          value.BlogId = dto.BlogId;

          _context.Update(value);
          _context.SaveChanges();
      }
```

kısaca mantık meditor de ne yapıyorsa aynen yapıyorsun yoksa böyle Comment tablosunda bulunan navigationlar hepsinin çıktısının görürsün.


<hr>

```csharp
  var jsonData=await responseMessage.Content.ReadAsStringAsync();
```

bu kod content string'e çevriyor readStringAsync ise json olan verileri deserialize edilmeden okunabilir yapıyor şöyle

API'den gelen veri **mantıken JSON formatındadır** ama **teknik olarak byte dizisidir**.
Bu byte dizisi şöyle görünür:

```
[123, 34, 105, 100, 34, 58, 53, 34, 110, 97, 109, 101, 34, 58, ...]
```

Bu byte'ları **okunabilir JSON string'ine** çevirmek için:


```csharp
// Byte dizisi halinde
responseMessage.Content // → byte[]

// String halinde  
await responseMessage.Content.ReadAsStringAsync() // → "{"id":5,"name":"Ali"}"
```

yani gerekli iki adımlı doğrulama gibi düşün önce okunabilir yap sonra deseralize et.


<hr>


Çok ama Çok önemli Meditor notları oluşacagı zaman ekle notuna

```csharp
 public class GetCommentByIdQueryHandler : IRequestHandler<GetCommentByIdQuery, Comment>
 {
     private readonly IRepository<Comment> _repository;
     public GetCommentByIdQueryHandler(IRepository<Comment> repository)
     {
         _repository = repository;
     }
     public async Task<Comment> Handle(GetCommentByIdQuery request, CancellationToken cancellationToken)
     {
         var values =await _repository.GetByIdAsync(request.Id);
         return values;
         //return new GetCommentByIdQueryResult
         //{
         //    CommentId = values.CommentId,
         //    Name = values.Name,
         //    CreateDate = values.CreateDate,
         //    Description = values.Description,
         //    BlogId = values.BlogId,
         //    Email = values.Email,
         //};
     }
 }
```

```csharp
  public class GetCommentByIdQuery:IRequest<Comment>
  {
      public int Id { get; set; }
      public GetCommentByIdQuery(int id)
      {
          Id = id;
      }
  }
```

şimdi sen illa Result sınıfı kullanmak zorunda değilsin kendi entity sınıfınıda kullanılsın ama entity sınıfında bulunan ilişki propertyleride olacagından api de ilişkilerde gelir bundan bir dto kulanman daha doğru. 

burada dikkat etmen gereken handle metodunda 
```csharp
Task<Comment>
```
bu kısım ne ise o türden nesne döneceği yani sen Result sınıfnı tanımladın ise Result sınınfın new yapıp propertylerine atama yapacaksın bu dönecek api controllere de. ama sen entity kullacaksan entitinin ismini yazacaksın.

```csharp
   return new GetCommentByIdQueryResult
   {
       CommentId = values.CommentId,
       Name = values.Name,
       CreateDate = values.CreateDate,
       Description = values.Description,
       BlogId = values.BlogId,
       //Email = values.Email,
   };
```

dersen ben mesela Email propertysini boş bıraktım api bunun bize Email null olarak gösterecek yani sınıfı döneceğinden api bize Email tamam sen bana vermedin ben Email yokmuş gibi davranamaz Email null der geçer yani zaten hep böyle oop de böyle sınıfın propertylerine değer atanır ama bir sınınfın 100 propertysine de 100'ü nüde değer atanacak diye bişey yok değil mi ekranda ama yine olacak null da olsa olacak.



<hr>

Bir Controller içindeki bir metod'a ulaşmak için o controllerdan bir nesne üret ve çağır diğer controller da çalııyor denendi. 

ama çok iyi bir yöntem değil onun yerine bir class da ortak bir servis gibi bir yapı kurgulayıp oradan çağırman tabii en çok tercih edileni.

<hr>


C# da şunu farkettim herşey aslında static keywordü ile işaretlenmiş mesela hazır fonksiyonların çoğundan new demiyoruz neden static de ondan new yapılmıyor direk kullanıyorsun bundan sonra new yapmadığın kodlara bak arkasında ne var ne yok gir içlerine kodların artık kodun arkasında ne var diye gir bunu nasıl yapacasksın çok basit ctrl + bir tık yap git içine ilgili kodun gir bak ne olduğuna.