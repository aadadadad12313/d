
Abstraction (Soyutlama) bir nesnenin karmaşık yapısını basitleştirerek sadece gerekli olan memberları kullanma. gereksiz memberların soyutlama işlemidir.

yani bir nesne oluştudugumuzda  referans olarak verdigimiz değişkenin memberlarından bize ihtiyaç olmayan memberlarını görünyorsak bu pek doğru bir yaklaşım değil soyutlama yani abstraction yapılması lazım.

Abstraction bir yaklaşımdır ondan abstractionın en çok kullanıldıgı yerler interface ve Abstract'tır.


dikkat ederseniz eğer abstraction davranışı: member'lan ayıkladığı/gizlediği için 'encapsulation', kalıtımsal işlem gerektirdiği için 'inheritance' ve farklı referanslar kullandırdığı için 'polimorfizm' kavramlarıyla doğruda bağlantılı bir davranıştır.



```csharp
//UserService userService = new UserService;

//userService. bütün sınıfın memberları gözüktü ben sadece ValidateUser gelsin istiyorum ondan ya interface ile ya da abstract ile soyutlamalısın

UserService userService = new UserService;
IUserValidateService validateService = userService;

//validateService. ve ekrana validateUser fonksiyonu gelmiş oldu
//userService nesnesini IUserValidateService türüne dönüştürüp 
//IUserValidateService içindeki kodları referans alıyor

interface IUserValidateService
{
    public bool ValidateUser(UserInfo info);
}

public class UserService: IUserValidateService
{
    public void CreateUser(UserInfo info)
    {
    }

    public void RemoveUser(int id)
    {
    }

    public bool ValidateUser(UserInfo info)
    {
        return true;
    }
}

public class UserInfo
{
}
```

