
```csharp
var personelListesi = new List<Personel>
{
    new Personel { Id = 1, Ad = "Ayşe Yılmaz", Pozisyon = "Yazılım Geliştirici", Yas = 28 },
    new Personel { Id = 2, Ad = "Mehmet Demir", Pozisyon = "Veri Analisti", Yas = 32 },
    new Personel { Id = 3, Ad = "Zeynep Kaya", Pozisyon = "Proje Yöneticisi", Yas = 35 },
```

bu c# kodunda nesne oluşturma işlemi yapılıyor personelListesine düşüm ki 100 tane veri ekleniyor nesne oluşturarak.

yani demem o ki bunu bir personel tablosu olarak düşün içine veri ekleniyor ya new new diye diye bunu da her satır olarak düşün illa bir basit bir projede sql de tablo oluşturmak zorunda değilsin ondan böyle sahte veriler ile de aynı çıktıyı elde edebilirsin.