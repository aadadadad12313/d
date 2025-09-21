
##### <font color="#4bacc6">Git Reset</font>

git reset, Head işaretçisini geri alıp başka bir commit'e taşıyan komuttur. Yani

Head’i (hangi commit üzerinde olduğumuzu gösteren işaretçi) değiştirir.

isteye bağlı olarak working directoryi veya staging area'yı da geri alabilirsin.

<font color="#92d050">3 tane özelliği var:</font>
###### <font color="#4bacc6">--soft</font>

Head'ı (yani branch'i) belirtilen commit'e alır.

committeki değişiklikleri staging area'ya ekler.

En güvenli reset türüdür. çünkü ne working directory'i ne de staging area'yı değiştirir commit geri alındıysa alındı o geri alınan commit'in özellikleri durur silinmez sonra tekrar commit yapman için zaten bunların durması gerekiyor.

<font color="#92d050">Kullanım senaryosu</font>

Commit'i geri alayım, ama aynı değişikliklerle tekrar commit atayım.

Yanlış commit mesajı yazdıysanız,
###### <font color="#4bacc6">--mixed</font>

Head'ı belirtilen commit'e alır.
Staging area'yı temizler.
working directory'deki değişiklikler durur.

<font color="#92d050">Kullanım senaryosu</font>

Commit'i geri alayım, değişiklikler dosyada kalsın, ama staging area'ya eklenmesin.

yani commit ederken bir tane dosyayı yanlışıkla ekledin veya commit mesajını yanlış yazdın diyerim commit'e git reset mixed kullanırsın stage area temizleneceğinden tekrar git add yapar dosyayı bu sefer seçmessin bu işe yarar.

soft ile benzer ama soft stage areaye atar direk ondan istemediğin dosyayı çıkaramazssın.
###### <font color="#4bacc6">--hard</font>

Head'i belirtilen commit'e alır.
Staging Area temizlenir.
Working Directory döndüğümüz commitin haline döner.

<font color="#92d050">Kullanım senaryosu</font>

Son commit’i ve tüm değişiklikleri tamamen sil, sanki hiç olmamış gibi olsun.
###### <font color="#4bacc6">Özet</font>

En risklisi hard komutudur. çünkü commit geçmişini, working drectoy'i, staging arae'yı geri alır. diğer komutlar birini alırsa diğerini koruyordu.
###### <font color="#4bacc6">Head</font>

Head, şu anda üzerinde çalıştığımız commit'i işaret eder.

Head yerine branch isminide açıkca yazabilirsin.

Ama Head kulllanmak daha iyi çünkü hangi branch'deyim diye düşünmen gerekmiyor.

Genellikle head aktif branchin son commitini gösterir normal kullanımda.

Ama bazı durumlarda Head branch'i değil de doğrudan commit'i de işaret edebilir buna Detached head denir.

```csharp
HEAD → main → commit abc123  => Normal durumda head branch'i işaret eder
HEAD → commit abc123 (branch yok) => Detached HEAD'de ise branch olmadan doğrundan commit'i işaret eder.
```

Peki detached head nasıl kullanılır dersen şöyle:

```csharp
git checkout abc1234 => master yazmıyorsunda commitin ismini yazıyorsun
```

##### <font color="#4bacc6">Git Reset Kullanımı</font>

```csharp
git reset --soft HEAD~1
```

<font color="#6495ed">Head~1</font> → şuan bulunduğun commit'in bir önceki commit'ini işaret eder.

<font color="#6495ed">--soft</font> → Head'i ve branch işaretçisini geri alır ama:

Working directory ve staging area değişmez.

yani commit silinir ama dosyalar sanki commit için hazırlanmış (staging) halde kalır.

Hemen tekrar git commit dersen, aynı commit mesajını farklı bir içerikle tekrar oluşturabilirsin.

```csharp
git commit -m "Product + Category + Controller tek commit"
```

Commitleri geri aldıktan sonra githuba commit etmemiz gerekli ister visual studio üzerinden commit işlemini yap işter de gith basden yap.

```csharp
git push --force origin main
```

<font color="#6495ed">Şimdi burada iki durum var</font>

<font color="#92d050">1 Git push</font>

Eğer bu alınan commitler github da yoksa git push kullanabilirsin. çünkü alınan commitler daha githuba pushlanmadığından bu commitleri tek bir commit yapıp psuhlama da bir sorun olmaz.

<font color="#92d050">2 Git push -- force</font>

<font color="#4bacc6">Force demek</font>

- Remote’daki commitleri ezerek senin local commit geçmişini zorla gönderir.
    
- Yani remote’yi “benim local geçmişimle aynı yap” demek.

Eğer commitler github da da varsa git push --force kullanman lazım git push kullanrısan olmaz çünkü local de silip birleştirmiş olabilirsin ama githubda da silmen lazım yoksa local da 10 commit githubda 15 commit olur iki depo da senkronize olmadığından git push veri kaybı olacağından hata verir.

git push --force'yi visual studio üzerinden yapamazsın çünkü push butonu git push yapar bize --force yapan lazım bunun için gith bash kullanman lazım.
##### <font color="#4bacc6">Peki yanlış commiti geri aldık diyerim iptal etmemiz lazım duruma göre iki yol var</font>

###### <font color="#92d050">1 Git Reflog</font>

Git Reflog, Head'in bir önceki konumnu işaret eder yani head'in geçmişte nereye baktığını temsil eder.

yapılan değişiklikleri (masterdan, feature branchine geçme işlemini, commitleri, reset işlemlerini) git hep reflog'a yazar.

yani reflog bizim geçmiş kayıtlarımızı tutar.

```csharp
git reflog
```

yazarak şu çıktıyı elde ederiz.

```csharp
4073332 (HEAD -> master) HEAD@{0}: reset: moving to HEAD~1
a37c4c4 (origin/master, origin/HEAD) HEAD@{1}: commit: e
4073332 (HEAD -> master) HEAD@{2}: commit: d
1a2d4e8 HEAD@{3}: commit: c
25c4854 HEAD@{4}: commit: b
3225ad4 HEAD@{5}: commit: a
4d9ebab HEAD@{6}: commit: Add project files.
ad36aa4 HEAD@{7}: commit (initial): Add .gitattributes and .gitignore.
```

HEAD{0} şu an bulunduğumuz commit bizim hangi commit'e dönek istiyorsan onun index numarasını yazacaksın.

Head{0} yazma dikkat et o şuan ki commit olduğundan bir değişiklik olmaz.

```csharp
 git reset --hard HEAD@{1}
```

Bu kod ile şu olur:

Head, a37c4c4 hash'li commit'e yani e commit'ine döner.

--hard olduğundan working directory ve staging araea e commit'inin haline sıfırlıanır. 

yani Head{0} commit'i yok olur.

###### <font color="#92d050">2 Pull</font>

Eğer geri aldığımız commitler github da varsa pull yapıp githubda bulunan commitleri tekrardan local depoya alabilirsin.

##### <font color="#4bacc6">Reset ile Reflog arasındaki fark</font>

```csharp
git reset --soft HEAD~1
```

Bu komut mevcut dalda bir commit geriye gider.

```csharp
git reset --hard HEAD@{1}
```

- Bu komut reflog'a bakarak bir önceki HEAD pozisyonuna geri döner.
- +HEAD{1} reflog'daki bir önceki pozisyonu ifade eder (son yaptığınız işlemden önceki durum)

yani ikiside git reset ile kullanılıyor ama farkları şu:

Head~1 demek son committen bir önceki commit'e dönme demek kullandığın paremetreler göre bunun özellikleri değişir soft,head,mixed gibi.

Head{1} demek ise reflogdaki kayda bakar Head{1} dediğimiz için Head{1} anına dönersin.

bu head{1} anı illa commit işlemi olmak zorunda değil bir branchden bir branch'e geçme işlemi bile reflogda kaydı tuutlur sürekli bundan reflogdaki kayıtları incelemen hangi işleme dönmen gerekiyorsa onun index numarasını yazıp dönme işlemini yapman gerekiyor.


##### <font color="#4bacc6">Git Revert</font>

`revert`, geçmiş commit’i geri almak için yeni bir commit oluşturur.

Commit geçmişi değiştirilmez, branch tarihi korunur.

Özellikle paylaşılan branch’lerde güvenlidir.

Seçilen commit’in tersini uygulayan yeni bir commit oluşturur. yani normal bir commit dosya eklemek iken revertin yaptığı commit dosyayı silemek olduğundan böyle deniyor.

Reset kullanmak takım çalışmalarında güvenli değil çünkü son commit'i siler özelliklerini stagaging area'ya ekler yeni commit yaparsın sonra ama tarih şimdiki zamanın tarihi olacak. 

ama revert kullanırsan commit silinmez yeni bir commit oluşturur böylelikle tarih kaybolmaz commit'in.

```csharp
C1 -- C2 -- C3 -- C4 (HEAD)
```

c3 commitini silsen revert ile c4 hata vermez çünkü c3 silinniyor c4 sonrasında yeni bir c5 commiti oluşturuluyor zincir kopmuyor yani.

```csharp
C1 -- C2 -- C3 -- C4 -- C5(revert C3)
```



Reset ile Revert farkı

Reset commit edilmiş ama githuba psh edilmemişerde kullanılrı

Rever ise commit edilmiş ama githuba gitmiş olanlardak ullanılır