


Source Control Bölümünden Intialize Repository butonuna tıklıyorsun. bu ne yapıyor şunu yapıyor gidiyor projenin olduğu klasöre git klasörü oluşturuyor.


Vscode de dosyaların yanında bazı harfler var

| Harf  | Açılımı   | Anlamı                                                               |
| ----- | --------- | -------------------------------------------------------------------- |
| **U** | Untracked | Git tarafından henüz takip edilmeyen yeni dosya. Stage edilmemiştir. |
| **M** | Modified  | Daha önce commit edilmiş bir dosyada değişiklik yapılmış.            |
| **A** | Added     | Yeni dosya eklenmiş ve stage edilmiş. Commit’e hazır.                |

![[Pasted image 20250811162626.png]]

yukaradaki icon commit bölümünde git history eklentisini açıyor ve oradan yaptığın tüm comitlerin geçmisini sayfalarını ne değşti ne değişmediğini filan bakıyorsun kim kaydetti commiti ne zaman kaydetti oradan görebilirsin.

Ve bu bölümden yaptığın comitleri karşılaştırılmasını inceleyebilirsin bunun için

comitlerin olduğu git history sayfasında bir sürü comitten 
isteiğin bir comitte tıkladığında açılan dosyalarda Workspace var bu bu comittin bir üstündeki ile kıyaslama yapar comit varsa previeus da var o da bu seçtiğin comittin altındaki comit ile kıyaslamasını yapar eğer dersen ben ne bir üstte nede bir alttaki ile kıyaslama yapacam o zamanda 

bir tane comit seç orada butonlar var o comitin olduğu yer de more adında ona tıkla Select this commit seç bu birinci comiti seçtin şimdi diğer bir tane comit daha seç onda da more yap ama bu sefer Compare with bunu seç 

![[Pasted image 20250811165011.png|150]]

seçtikten sonra  COMPARE COMMITS bu bölümden tıkla bir kez karşılaştırma çıkacka seçtiklerin için. 


Eğer bir işlem yaptın ve işlemi iptal edecen bunun için üç noktadan changes altındaki Discard All Changes tıkla ve yaptığın herşey silinecek eskiye dönecek. yada tek bir dosyaya sağ tıklayıp onuda değişikliğini geri alabilirsin Discard Changes diyerek.


Git Revert yaparsan eğer seçtiğin comit silinmez ama revert diye otomaitk bir comit oluşur ve seçtiğin comit kodları gider eskiye döner ama bunu yaparken en son comitten başlayıp dönmek istediğin comitte doğru gitmelisin aksi takdirde hata verir çünkü comit2 silersen comit3 hata verir ben comit2 yi kullanıyorum ben ne hata yerim diye ondan comit 3 comit2 gibi en tepeden sileceksin 

bunu git history sayfasından more kısmından revert this ile yapıyorsun evet diyorsun.


ve bir diğer alternatfi comit silmenin reset işlemi bu revertteki gibi ayrı bir commit oluşturmaz ve comitler kalmaz seçtiğin comite gider direk o ana ne kodlar kalır ne comitler herşeyi siler ben bunu sevdim nasıl yapıyoruz peki git history sayfasında more kısmı varya oraya tıklamadan orada durna butonlarda hard olana tıkla evet de orası işte direk olacak.