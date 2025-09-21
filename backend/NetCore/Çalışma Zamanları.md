
onemli::Çalışma Zamanı,compile time,run time


| Zaman            | Ne kontrol edilir?                    | Örnek                        | Hata ne zaman görülür? |
| ---------------- | ------------------------------------- | ---------------------------- | ---------------------- |
| **Compile-Time** | Syntax, tip uyumu, property/method    | `int x = "hello";`           | Derleme sırasında      |
| **Runtime**      | Dynamic, cast, değerler, null kontrol | `dynamic y = "a"; y.Length;` | Program çalışırken     |

1️⃣ Compile-Time (Derleme Zamanı)

İlk önce Compile Time çalışıyor 

hani bu bazen yanlış türe bişey atandığında hemen uyarı veriyor seni koda atıp işte o an sen Compile Timedesin.

```csharp
int x = 5;
x = "hello"; // ❌ Compile-time hatası
```

Özellik: Hata derleme sırasında yakalanır, program hiç çalışmaz.


2️⃣ Runtime (Çalışma Zamanı)

Compile Time başarılı olursa ortaya exe veya dll çıkar. Bu dosya çalıştırıldığında runtime başlar.
    
- Dynamic binding, cast, değerler, null referans gibi kontroller program çalışırken çözülür
- Hatalar bu aşamada ortaya çıkar (örn. NullReferenceException).

```csharp
dynamic y = "hello";
Console.WriteLine(y.Length);
```

- `y.Length` → Runtime çalışınca çözülür.
