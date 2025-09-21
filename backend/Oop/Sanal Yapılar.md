
Name Hiding bir classadaki bir metodun kaltım alan metodda da aynı isimle tutulmasındaki karmaşıklığa name hiding diyoruz ama sanal yapılarda böyle değildir yani virtual veya override kalıtım alan sınıf o metodu ezebilir hatta yeniden oluşturabilir.

Name Hidinglerde biz karmaşıklığı ortadan kaldırmak için new operatörünü kullanırız
yani metıdu new ile işaretleyerek base classdaki metodu görmez geliyoruz ezme veya oluşturma yok burada.

sanal yapılanmalarda bir isim çakışması yoktur aynı metoda farklı özellikler ekleme mantığı vardır


> [!Warning] Run Time
> Sanal yapılar run time'da çalışır çünkü adı üstünde sanal oldukları için ilk aşama olan derleme zamanında nereden çağrılacağı belli değil bundan dolayı run time'da kararlaştırır.

bir classdaki metodu overriede etmek zorunda değilsin virtual olarak tanımlandı diye ama overriede etmek için virtual olarak tanımlanmak zorundadır.