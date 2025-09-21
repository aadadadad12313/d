
onemli::ok metodu api

Ok metodu bir 200 status code veren bir metod'dur. eğer işlem basarılı olduğunda 200 dönecek. yani sen nasıl bir apiye istek atıp 200 geliyorsa sana iste bu ok sayesinde işlem oldugunda 200 gerecek tarayıcı sana hata fırlatmayacak.


1️⃣ `Ok(value)` davranışı

```csharp
var value = 2;
return Ok(value);
```

Çıktısı Api de 

```json
2
```

Direk 2 yazar repository pattern da mesela böyle oldu neden çünkü herhangi bir dto yoktu gitti aldı değeri int türünde verdi direk.

- `value` burada **primitive bir tip** (`int`, `string`, `bool`, vb.).
    
- ASP.NET Core bunu **raw response** olarak yazıyor, yani body’de direkt `2` var.
    
- JSON olarak `{ "value": 2 }` gibi dönmez, çünkü framework primitive tipleri JSON objesi olarak sarmalamıyor.

2️⃣ MediatR / DTO ile çalışırken

```csharp
public class CommentCountDto
{
    public int Count { get; set; }
}

return Ok(new CommentCountDto { Count = 2 });
```

```json
{
  "count": 2
}
```

burada ise artık dto'lar var ondan ok metodu akıllıca davranıp json'a çeviriyor.
