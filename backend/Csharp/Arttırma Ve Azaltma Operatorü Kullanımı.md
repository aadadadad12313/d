
- Prefix (ön ek)
    
    - `++sayi` → önce artırır, sonra kullanır.
    - `--sayi` → önce azaltır, sonra kullanır.
        
- Postfix (son ek)
    
    - `sayi++` → önce mevcut değeri kullanır, sonra artırır.
    - `sayi--` → önce mevcut değeri kullanır, sonra azaltır.

1. Kod (tek satırda yazdırma)
```csharp
	int sayi = 0;
	Console.WriteLine(sayi++);
    Console.ReadKey();
```

şimdi bu kod kritik 

- `sayi = 0`
    
- `sayi++` → önce mevcut değer (0) yazılır, sonra `sayi` artırılır.
    
- Çıktı: 0
    
- Kod bittikten sonra `sayi = 1` olur ama bu yazdırmaya yansımaz.


2. Kod (önce artırma işlemi var, sonra yazdırma)
```csharp
int sayi = 0;

sayi++;

Console.WriteLine(sayi); 
```

bu kodda ise sayi++ olduğu an atanacak değeri yok değil mi ondan bu değişkenin 0 değerini kimse tutamayacak sonra ikinci adıma geçer hemen sayiyi arttırmaya yani 1 oldu tamam hafızada güncellendi 1 oldu arttık ekrana 1 yazdırdı.

```csharp
    int sayi = 0;
    
    int a = sayi++;

    Console.WriteLine(sayi++);
```

Burada ise a 0 olacak neden çünkü sayi değeri önce a ya atanacak kural böyle sayi  0 ise 0 atancak atama işlemi bittikten sonra ikinci adım olarak ++ olacak sayi 0 dan 1 olacak ama a 0 atandıgından a 0 kalacak.

sonra ekrana yazdırmaya geldiğinde ise önce sayi değeri 1 olduğundan ekrana 1 yaxdı sonra 2 oldu.

Özetle: 

Postfix ++ “değeri üretir” ve ardından artırır.

- Değer kullanılmazsa, hemen artırılmış gibi görünüyor. örnek sayi++
    
- Değer kullanılırsa (Console.WriteLine gibi), önce eski değer alınır, sonra artırılır.

