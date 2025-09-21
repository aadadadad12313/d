

`margin`, bir elementin dış boşluklarını yani **çevresindeki diğer elementlerle olan mesafeyi** tanımlar.

margin uygulanan bir element eğer inline ise width height margin padding yemez ondan o elementi display block veya inline-block yapmalısın.


`padding`, bir elementin **kenarları ile içeriği arasındaki boşluğu** ifade eder.

padding verilen bir öğe width 500 ise 50px padding verdiginde width'e eklenir 550px olur. yani genişlik büyür marginde bu yoktu.

padding uygulanan öğe inline olsa bile olur margindeki gibi block veya inline-block yapmana gerek yok.


Margin box sizing border box özelliginde geçerli değil unutma margin width'i arttırır 15px mi verdin 15px itekrer ögeyi ama padding bunu yapmaz içerden verir yani box sizing yapılandırılmasına uyar ve width'i arttırmayacagından iki öğer var mesela padding-left dersin margin-left görevi görür yani. bu işin kurarı bu bazı yerler padding'e uymasada uyarmış gibi yapacağız.

Bu durumda, **görev aslında `margin-left`’in**, ama **taşma riskine karşı `padding-left` daha güvenli**

Yani demek istedigim şu benim margin iki öge arasında boşluk bırakmada kullanılamaz taşmaya sebep olur ama padding içerden iter  ve aldığı destek box sizinden dolayıda ögeleri arasındaki boşkuk açada en iyisi witdh hesaplanamz mesela box sizing paddingde oldugundan mantık tam bu marginin görevi burada padding'e kalıyor tamam bu bir istisna.