

Git bize şunu sağlıyor version control sistemi yani biz tek tek UdemyCarbook1,UdemyCarbook2 gibi version isimlendirme yapıp kodlarımızı masaüstünde filan klasörlemiyoruz.

bize bunu git yapıyor her commit edişimizde ayrı bir versiyon oluşturuyor bize ayrı bir versiyon oluşturuyor ama bize daha önceki yaptıklarımızında kodlarını veriyor yani otomatik yapıyor yukardaki sorunu.

ama bize tek bir dosyada veriyor ama arkada bunu versiyonlayarak tutuyor git bu da bize esneklik sağlıyor kolay geçiş yapabiliyoruz versiyonlar hakkında.

Git bize versiyonlar arası farkı görme imkânı sağlıyor. Örneğin GitHub arayüzünde bir önceki commit’te eklenen kodlar yeşil, silinen veya değişen kodlar kırmızı gösterilir.

Esas projeden bir kopya oluşturup kodlar ile deneme yapmaya aynı bir projede farklı bir dal da buna branching denir. ama bunu yaptıktan sonra esas projeyle birleştirmeye merging denir.


<font color="#4bacc6"> Git Add</font>

git add dediğinde, o dosya staging area’ya (stage’e) eklenir.

git add komutu değişiklikleri Working Directory'den Staging Area'ya aktarılır.

###### <font color="#4bacc6">Commit</font>

- Stage’e aldığınız değişiklikleri commit yapabilirsiniz.
- Commit mesajı yazıp değişiklikleri Git deposuna kaydetmiş olursunuz.

###### <font color="#4bacc6">Git 3 ana parçadan oluşuyor</font>

<font color="#4bacc6">1 Working Directory</font>

Working Directory, Git projesi açtığımız klasördür. 

Bir index.html dosyasını düzenlediğinde veya sildiğinde bu işlem Working Directory'de gerçekleşir yani proje klasöründe gerçekleşir.

kısaca bizim html,css,js,cs dosyalarının bulunduğu dizine git Working Directory diyor.

<font color="#92d050"> Git Dosya Durumları</font>

Git, Working Directory'deki dosyaları kontrol ederek durumlarını belirler:

| Harf | Açılımı   |  Anlamı                                                              |
| ---- | --------- | -------------------------------------------------------------------- |
| U    | Untracked | Git tarafından henüz takip edilmeyen yeni dosya. Stage edilmemiştir. |
| M    | Modified  | Daha önce commit edilmiş bir dosyada değişiklik yapılmış.            |
| A    | Added     | Yeni dosya eklenmiş ve stage edilmiş. Commit'e hazır.                |
| D    | Deleted   | Dosya silinmiş.                                                      |
| R    | Renamed   | Dosya yeniden adlandırılmış.                                         |

<font color="#92d050"> Git Nasıl Algılıyor?</font>

- Yeni bir dosya oluşturduğunda → Git U (Untracked) durumunu gösterir
- Mevcut dosyanın içine yeni özellikler eklediğinde → Git M (Modified) durumunu gösterir
- Bu durumları Git, Working Directory'e bakarak belirler - yani senin çalışma dizinini sürekli kontrol eder.

###### <font color="#4bacc6">2 Staging Area</font>

Git’e commit etmeden önce değişiklikleri “hazır” duruma getirir ve hangi değişikliklerin commit'e dahil edileceğini belirler.


yani M harfi demek Stage'ye eklenmemiş demek. A harfi ise eklenmiş demek.

U ise yeni bir dosya oluşturdun ama bu dosya git'te yok ondan U yazıyor eğer gitte olsaydı.

Yani Stage demek Git Add Demek

```js
git add index.html
```

Bu dosyadaki değişiklikleri bir sonraki commit’e dahil et demek.

- git add  → değişiklik Staging Area'ya taşınır.
    
- git commit -m "mesaj" → Stage’deki her şey commit edilir.

> [!cite]- Dersen Eğer ben neden hiç VsCode'de stage yapmadın aslında visual code ilk kurduğumda bir uyarı çıkmıştı
>  yani her commit mesajına tıkladığımda diyor du ki önce stage'ye ekle sonra commit yap diyordu bende always diyerek sen önce commit'te tıkladığımda kendin arkada stage'ye ekle ondan sonra commit yap demişim yani stage'ye tabii ekleniyor ama arkada kendisi yapıyor yani git Add yapıyor.
> 
> Test Edildi Vs Code den ayarlardan git enableSmartCommit yazarsan çıkan iki kutudaki checkboxları kapatırsan artık değişiklik bile varsa commit butonuna direk basamazsın çünkü stage areayı manuel şekilde eklemen gerekecek.
> 
> ama eğer checkboxları seçicili bırakırsan commit butonuna basabilirsin o arkada kendisi stage'ye ekler tabili biz bunu kullanacaz.


<font color="#4bacc6">3 Repository</font>

Git'te repository, projenin tüm geçmişinin saklandığı yerdir. yani commit'lerin,branch'lerin hepsi burada tutulur.

iki tür repository çeşidi vardır.

<font color="#92d050">1 Local Repository (Yerel Repository)</font>

proje klasörün içinde bulunan git klasörünün içinde tutulur. yani git'in veri tabanıdır diyebiliriz.

Git add dediğimizde staging area'daki değişiklikler buraya kaydedilir.

<font color="#92d050">2 Remote Repository (Uzak Repository)</font>

Github, Gitlab gibi sunucularda bulunur.

Projeyi paylaşmak, yedeklemek ve ekip çalışması yapmak için kullanılır.


<font color="#4bacc6">Özet</font>

| Alan              | İşlevi                                                |
| ----------------- | ----------------------------------------------------- |
| Working Directory | Kodların üzerinde çalıştığın klasör                   |
| Staging Area      | git add dersen, bu değişiklik staging alanına alınır. |
| Repository        | Commit’lerin kalıcı olarak saklandığı Git veritabanı  |

<font color="#4bacc6">Genel Özet</font>

Projeyi kaydetmeye Commit

Bir şeyler yaparken eski koda dönmeye Undo

Bir versiyonla diğer bir versiyonu karşılaştırmaya Diff

projenin kopyasını oluşturma ve ekleme yapma

branchde olan işlemi ana projeye aktarmaya merging
