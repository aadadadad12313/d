
1️⃣ `??` Operatörü (Null-coalescing)

```csharp
değişken ?? varsayılanDeğer
```

- Anlamı: **Eğer değişken `null` ise sağdaki değeri kullan**.
- Null değilse, mevcut değer kullanılır.
- 
Adı üstünde Null-coalescing yani null olduğunda kontröl için geliştirlmiş.


 **2️⃣ `?:` Operatörü (Ternary / Koşul Operatörü)**

```csharp
şart ? doğruysa : yanlışsa
```

Anlamı: **Şart doğruysa ilk değeri, değilse ikinci değeri kullan**.

```csharp
int sayı = 10;
string mesaj = (sayı > 5) ? "Büyük" : "Küçük";
Console.WriteLine(mesaj); // "Büyük"
```

Şimdi dikkat et burada `??` operatörü null değer kabul eden değişkenlerde kullanılır.

```csharp
   public ActionResult Index(int id)
        {
            int UrunId = id ?? 1;
        }
```

hata verir çünkü id int olduğundan boş olamaz default kuralı. ?? ise null yapmaya çalışıyor hata verir
ondan int id'yi null değer kabul edebilsin diye tanımlayacaksın hata ortadan kalkacak. sonra ?? ile null ise şu şu olsun diyeceksin.


```csharp
   public ActionResult Index(int? id)
        {
            int UrunId = id ?? 1;
        }
```
