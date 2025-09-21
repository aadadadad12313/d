
###### <font color="#92d050">Konu Anlatımı Türler</font>

<font color="#92d050">Type (Tip): </font>Bütün değer türleri ve referans türleri bir type'tır.

<font color="#92d050">Tür:  </font>ikiye ayrılır Değer türleri ve Referans türleri olmak üzere :

Tür dediğimizde genelde hangi kategoriye girdiğini kastederiz:

- Değer türleri (Value types) → int, double, bool, struct
    
- Referans türleri (Reference types) → class, interface, delegate, string, object


<font color="#92d050">Value type - Primitive Type yani değer türlü değişkenler</font>

C# programlama dilinde Ram de veri tutabilmek/depolayabilmek için o verinin türü bildilirmek zorundadır.

Bir degiskenle RAM'de alan tahsisinde bulunulduğuunda buna değer türlü degisken diyoruz. Yani tuttugu deger bir normal deger olan degiskenlere değer türlü denmektedir.

Valude type'lar stack de saklanır doğrudan değeri tutar.

```csharp
int  a = 500;       
short b = 200;
```

<font color="#92d050">Referans Türleri (Reference Types)</font>

Heap'de saklnır nesnenin adresini tutar.

---

Dikkat object ve dynamic hem değer türlü hem referans türlü olabilir. yani hem int olabilir hem de Person diyerek referans türlü de tutablirsin.

Şimdi çok basit gencay yıldız da böyle anlatıyor sen bir türün değer mi referans tür mü olduğunu nasıl bilecen tak diye cevap şu

bir değer varsa isim değerin var bu bir değer bitti.

değer türlü ama bir ben varsan kompilen benim ismimi yaşımı tutuyorsa bu artık değerden daha fazlası ondan referans türlüdür.

nesneleri tutan değişkenlere referans türlü değişkenler diyoruz. yani new varsa onun referansı yok mu a sa a referansı diyoruz ya işte adı üstünde refarans varsa referasn türlü değişken.

bir değişken tanımlarken verinin türüne uygun bir tür belirleyeceksin sen sonra ram bu verdiğin tür'e göre ram de alan tahsis edecek yani sen int verirsen şu kadar byte kaplayacak gibi.

Bir tür verdin, RAM’de yer ayrıldı. Daha sonra türünü değiştirip farklı bir boyut tahsis edemezsin; RAM tahsisi değişken tanımlanırken yapılır.

###### <font color="#92d050"> Değer Türleri Ram de ne kadar alan kaplar</font>

| Veri Türü | Açıklama                            | RAM'de Kapladığı Alan | Değer Aralığı                  |
| --------- | ----------------------------------- | --------------------- | ------------------------------ |
| bool      | Mantıksal değer (true/false)        | 1 byte                | `true` / `false`               |
| byte      | 8-bit işaretsiz tam sayı            | 1 byte                | 0 – 255                        |
| char      | Unicode karakter                    | 2 byte                | '\u0000' – '\uffff'            |
| int       | 32-bit işaretli tam sayı            | 4 byte                | -2,147,483,648 – 2,147,483,647 |
| float     | 32-bit tek duyarlıklı ondalık sayı  | 4 byte                | ±1.5 × 10⁻⁴⁵ – ±3.4 × 10³⁸     |
| double    | 64-bit çift duyarlıklı ondalık sayı | 8 byte                | ±5.0 × 10⁻³²⁴ – ±1.7 × 10³⁰⁸   |
| decimal   | 128-bit yüksek hassasiyetli sayı    | 16 byte               | ±1.0 × 10⁻²⁸ – ±7.9 × 10²⁸     |
| DateTime  | Tarih ve saat bilgisi               | 8 byte                | 01/01/0001 – 31/12/9999        |

##### <font color="#92d050">Dönüşüm Türleri</font>

###### <font color="#92d050">Implicit (Örtük) Dönüşüm</font>

Implicit dönüşümler otomatik olarak gerçekleşen ve veri kaybı riski olmayan dönüşümlerdir.

```csharp
int sayi = 100;
long buyukSayi= sayi; // güvenli
```

int türünün aralığı long'dan küçük olduğundan veri kaybı olmadan atanabilir çünkü int hangi değeri tutarsa tutsun long'da tutar bunu ondan Implicit Dönüşümdür.

```csharp
int sayi =100;
float ondalikli=sayi; // güvenli
double doubles=sayi;  // güvenli
```

bu örnekte Implicit örneğidir çünkü int ve float aynı boyutta da olsalar float daha fazla değer aralığna sahip olduğundan veri kaybetmeksizin int'in değerini tutabilir.

ama tam tersi durumda floattan int'e dönüşüm explicit dönüşüme girer çünkü flaot bir ondalıklı sayi tutarken int'e tam sayıya dönmede ondalıklı sayılar kalkacak veri kaybı olacağından explicittir.


Reference türlerde implicit dönüşüm

```csharp
string metin = "Merhaba";
object nesne = metin;  // string → object (türetilen sınıftan taban sınıfa)
```

###### <font color="#92d050">Explicit (Açık) Dönüşüm</font>

Explicit dönüşümler cast operatörü ile yapılır. veri kaybı olan dönüşümlerdir.

```csharp
double sayi=123.456;
int kucukSayi=(int)sayi; //123 ekrana yazacak 456 ondalıklı değeri kaybolacak

long uzunSayi=1000;
short kisaSayi=(short)uzunSayi // veri kaybı riski var

object nesne = "Merhaba Dünya";
string metin=(string)nesne
```


onemli:: Değer Türleri ve Referans Türleri

