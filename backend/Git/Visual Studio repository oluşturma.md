
###### <font color="#92d050">Visual Studio 2022 ile</font>

yeni bir projede git takibi olmadığından, Git Changes  bölümünden  Git Repository seçeneğini seçip açılan formda Account kısmından Github hesabına giriş yapman lazım.

Repository name ve visibility (public/private) kısmını projenin konusuna göre belirlersin. sonrasında Create and Push diyerek Visual Studio 2022 üzerinden bir repository oluşturmuş olursun.

Commit geçmişlerini görmek için git changes bölümünden commit mesajının olduğu yerin hemen üstündeki view all commits seçeneğine tıkla.

###### <font color="#92d050">Visual Studio 2017 ile</font>

iki seçeneğin var ya repository'i sen oluşturacan public to repo yapacan yani.
yada aynı 2022'nin yaptığı gibi repository'i uyguluma oluşturacak yani public to github.

önce yeni bir proje'de en altta şu seçeneğe tıklayıp git seçeneğine tıkla Sync bölümüne atıyor.

tıkladıktan sonra eğer o bölümü kaybedersen Team Explorer'dan Sync bölümüne tıkla.

![[Pasted image 20250912195728.png|200]]

<font color="#92d050">Bu bölümde iki seçenek var </font>

public to github => bu bölüm önceden yoktu GitHub Extension for Visual Studio eklentisi indirince geldi bu bölümün yaptığı hiç repository açmana gerek yok github'da kendisi açıp projedeki dosyaları ekler o repository'e.

publlic to repo => ben eskiden bu bölümü kullanıyordum bu bölümün yaptığı şey ise repository'i sen açıyorsun bu sadece projedeki dosyaları repository'e yolluyor.

public to repoda dikkat et isim olarak uygulamada bu yazar ama Publish to Remote Repository kelimeside bu olabiliyor chat gpt sana bunu derse anlaki public to repo o.

Commit geçmişlerini görebilmek için team explorer bölümünden Branches veya Sync seçeneğini seç push butonunun hemen yanında bulunan action seçeneğini seç view history'i seç.


###### <font color="#92d050">Visual Code ile</font>

Source Control bölümünden Initialize Repository seçeneğine tıklayarak git klasörünü oluşturuyoruz sonra commit mesajı yazıp publish brach seçeneğine tıklıyoruz oradan da private mi olacak yoksa public mi olacak oluşan repo onu seçiyoruz.