

Projemezin ana kodlarını değiştirmeden bir branch açıp projemizin kopyası yani ona değişiklikler yapıp testini yapıp beğenirsek ana branchimiz olan mastet'a merge edecez.

meiklikrge etmek demek aslında bir branchden bir branch'e yapılan değişleri o branchde de olsun demek aslında bir nebi commit atıyorsun.

comit etmek dememişlerde merge demişler işte bu sebepten.


<font color="#92d050">Merge</font>

Merge, Git’te iki branch’in kodlarını birleştirmek demektir.

Özetle:

- Sen projede farklı bir branch’te (ör. `feature/login`) çalışıyorsun.
    
- Ana kod (master) ayrı ilerliyor.
    
- İşin bitince, feature/login branch’indeki değişiklikleri main branch’e aktarmak için merge işlemi yaparsın.


Merge edilmiş bir branchi silersen master'a zaten geçtiğinden commit olarak silebilirsin gönül rahatlığıyla.

Merge Etmek için önce Main branchinde olmalısın sonra Branch panelinde merge etmek istediğin branch’i (örneğin `feature/login`) bul. sağ tıkla Merge into Current Branch yap ve bitti.


<font color="#92d050">Merge Ederken Çakışma Olması</font>

İki branch aynı dosyanın **aynı satırını** farklı şekilde değiştirmişse Git hangisini seçeceğini bilemez.

yani sen eğer git brranch kullan AdminTestimonail cotnrollerina constractırına bir ekleme yap sonra git master'da yap adam diyecek aynı dosya aynı satır olduğundan güvenlik olsun diye çakışma hatası verdirmişler çünkü şimdi çakışma olmassa bu yeni branch ezer master'ı o zaman kod kaybı yaşanabilir iyi bir bence uyarı.

ondan branh kullanıyorsa aynı dosyada değişiklik yap ona bişey demiyorum ama aynı satır'ı elleme.







featuredayken mastera geçiste uyarı vermiyorsa featuredeki o dosya masterda yoktur ondan izin veriyordur eger varsa zate uyarı verecek seçim yapman gerkecek
