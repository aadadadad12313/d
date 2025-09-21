
<font color="#92d050">Model</font>
İşlenecek olan veriyi temsi/ eden katmandır. Genellikle veritabanı işlemlerinin yapıldığı katmandır.


<font color="#92d050">Örnek </font>
Entity Framework Core, Entity Models, Ado.NET, Repository, Veritabanı İşlemleri.


<font color="#92d050">Mvc</font>

İstek neticesinde elde edilen verileri görselleştirecek, görsel çıktısını verecek katmandır.

<font color="#92d050">Örnek :</font>
HTML CSS JavaScript, Besim, Müzik, Video


<font color="#92d050">Controller</font>

Gelen requestleri karşılayacak olan ve requestin içeriğine göre gerekli model işlemlerini üstlenecek olan katmandır.

<font color="#92d050">Örnek:</font>

Algoritmalar,servisler,veritabanı vs çağırarak istenilen veriyi üretmekten ve ihtiyaç dahilinde üretilen bu veriyi View ile görselleştirmekten sorumludur.


<font color="#92d050">Çalışma Adımı :</font>

önce controller'a istek gelecek controller bu isteği değerlendirip ilgili Model katmanında ilgili görevini yapacak sonra manipüle ettiği veriyi tekrar View'e gönderecek

Yani 

client yani web tarayıcısı istek attıyor controller bunu karşılıyor sonra model katmanında ilgili işlemini yaptıkdan sonra dönen veriyi view'e veriyor.

1.Controller
2.Model
3.View 
çalışıyor.

