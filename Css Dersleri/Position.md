
Relative 

```css
.box1{
    background-color: gray;
    width: 120px;
    height: 120px;
}

.box2{
    background-color: green;
    width: 120px;
    height: 120px;
    position: relative;
    top:20px;
    left:20px;
}
.box3{
    background-color: brown;
    width: 120px;
    height: 120px;
}
```

burada box2 relative olduğundan akış içinde devam  eder kapsayıcının öğeleri arasında hizallama yaparız yani.

ama absolute olsaydı normal akışın dışına çıkardı ve body'e göre hizalanırdı bunu önlemke için kapsayıcıya relative veririzki absolute olan öğe body'e giderken relativeyi görüp anlasın hizalamayı burada yapacağını.


Not

Top felsefesi aslında şöyle üstten boşluk bırak
Left felsefesi soldan şu kadar boşluk bırak demek

marginde'de böyle hepsinde mantık tamamen bu diyeceksin ki matıkken neden iki öğe yan yana geldinde margin-right veriyoruz onda da 

seçtiğimiz bir box1 mesela margin-right verisen sonuna doğru bir boşluk oluşur yanına geldiğinde box2 o boşluğun yanına gelir


```css

.deneme{
 position: relative;
 top:0px;
 top:0rem;
 top:0em;/*0 varasa sonuna ne yazarsan yaz farketmez hepsi 0 nede olsa*/

 left:20px; /*ama burada farkeder işte hangi birimi vereceğin ondan burada sonuna bir birimi belirtmelisin doğrudan 20 yazamazsın*/
}
```

fixed kullanımı

fixed unutma sadece sadece sayfaya gere konumlandırılır yani gidip bodye göre konumlanmaz eğer öyle olsaydı zaten scroll ile altta indikçe ekranda kalmazdı absolute veya relative görse bile onlara görede konumlanamz sadece ama sadece fixed sayfaya göre konumlanır.