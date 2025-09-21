
Özel sınıf elemanları olan Constructor ve Diconstructor'da sınıfın ismi ile tanımlama aynı isimde oluyordu ama buradaki DeConstruct da bu durum yok.



```csharp
MyClass my = new MyClass()
{
    Name = "Serkan",
    Age = 20
};

var (x, y) = my;
//Deconstruct tupple değer döndürür  
//my referansı oluşur oluşmaz değerleri verilmiş 
//out ile işaretlenenlere atama yapılıyor çünkü out dışarıya aktarılacak ya ondan

//(string x,int y) = my; yada türlerini kendin belirle var kullanmadan

public class MyClass
{
    public string Name { get; set; }
    public int Age { get; set; }

    public void Deconstruct(out string name,out int age)//outlar ile dışarıya açtık değerlerimizi
    //return edemeyiz zaten void fonksiyon ama out ile dışarıya return edilmiş olur yani
    {
        name = Name;//name değeri out ile işaretlendi bundan dolayı dışarıya açık değeri dogrudan kullanbilrisin
        age = Age;
    }
}
```

Recoldlarda bu dahada kolay şekilde yapılabiliyor