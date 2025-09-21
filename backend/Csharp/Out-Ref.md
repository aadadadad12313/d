
<font color="#92d050">Değer Tipi Parametreler (`int`, `double`, `struct`, vb.)</font>

 Normal de değer tipli değikenler metoda geçerken kopyalanır yani tür den bir tür oluşur metod bitince o tür silinir yani orjinal tür'ün değişkeni etkilenemez.

bu davranışı out ve ref kullanarak değiştirebilirsin. out ref kullanılıyorsa tür kopyalanmaaz referans olarak geçer. 

yani kısaca biz metodlarda paremetre ksımında class türünde bir referans tanımlayabiliyorduk ve bu referans neyi işaret ediyordu paremetre olarak gönderilen referansın nesnesini işaret ediyordu birinde yapılan değişiklik diğerinde de oluyordu işte

bu durum'u değer türlü değişkenlerde de yapmak istersen out ve ref kullacaksın.
    

<font color="#92d050">Ref</font>

```csharp
public void DoSomethingRef(ref int number) {
    number += 10;
}

int x = 5;
DoSomethingRef(ref x); // x artık 15 olur
```

yani void bir metoddan bile gittin değer aldın.

<font color="#92d050">Out</font>
```csharp
public void DoSomethingOut(out int number) {
    number = 10; // out parametreye mutlaka değer atanmalı
}

int y;
DoSomethingOut(out y); // y artık 10 olur
```

out da aynı 

Aralaındaki fark ref başlangıçda bir değer atanması lazımken out'ta ise başlangıçta bir değer atanmaması lazım. artık hangisi kolayına geliyorsa ikisi de aynı konuyu kapsıyor istediğini kullan.

