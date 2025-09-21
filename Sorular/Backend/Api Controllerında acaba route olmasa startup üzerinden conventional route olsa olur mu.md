
onemli::Api Controllerında acaba route olmasa startup üzerinden conventional route olsa olur mu

hayır olmuyor hata vermiyor yapmayı denesen ama api controller hep attribute routing yaklaşımına göre kurgulanmış mesela 

api controllerında yer alan

```csharp
[ApiController]
```

bu attribut varsa conventional'a izin vermiyor bu attribut routing attribut ile çalışıyor peki dersen bunu silek biz convetional yapmayı deneyek sileibilrisin ama bu attribut çok iş yapıyor

mesela model binding etmeyi filan bu attribüt yapıyor burada bunu kaldırmak söz konusu bile olamaz zaten kaldırsanda pek bişey değişmiyor swagger görmüyor attribüt routing bekliyor yani

olmaz kullanmassın attribüt routin den başka bir route yaklaşımı.