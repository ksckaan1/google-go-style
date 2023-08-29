# Google Go Stili

Genel Bakış | [Rehber](./rehber.md) | [Kararlar](change) | [Best Practices](change)

## Hakkında
Go Stil Kılavuzu ve beraberindeki belgeler, okunabilir ve deyimsel Go yazmak için mevcut en iyi yaklaşımları kodlar. Stil Kılavuzuna bağlılığın mutlak olması amaçlanmamıştır ve bu belgeler hiçbir zaman kapsamlı olmayacaktır. Amacımız, dile yeni gelenlerin yaygın hatalardan kaçınabilmesi için okunabilir Go yazma tahminlerini en aza indirmektir. Stil Kılavuzu, Google'da Go kodunu inceleyen herkes tarafından verilen stil kılavuzunu birleştirme işlevi de görür.

--------------------------------------------------------------
| Döküman            | Adres                                                 | Birincil Kitle             | Normatif | Kuralcı |
|--------------------|-------------------------------------------------------|----------------------------|----------|---------|
| Stil Klavuzu       | https://google.github.io/styleguide/go/guide          | Herkes                     | Evet     | Evet    |
 | Stil Kararları     | https://google.github.io/styleguide/go/decisions      | Okunabilirlik Danışmanları | Evet     | Hayır   |
| En İyi Uygulamalar | https://google.github.io/styleguide/go/best-practices | İlgilenen Herkes           | Hayır    | Hayır   |

### Dökümanlar

1. [Stil Kılavuzu](change), Google'da Go stilinin temellerini özetlemektedir. Bu belge kesindir ve Stil Kararları ve En İyi Uygulamalardaki tavsiyeler için temel olarak kullanılır.

2. [Stil Kararları](change), belirli stil noktalarına ilişkin kararları özetleyen ve uygun olduğunda kararların arkasındaki mantığı tartışan daha ayrıntılı bir belgedir.

Bu kararlar zaman zaman yeni verilere, yeni dil özelliklerine, yeni kitaplıklara veya ortaya çıkan kalıplara göre değişebilir, ancak Google'daki bireysel Go programcılarının bu belgeyi takip etmesi beklenemez.

3. [En İyi Uygulamalar](change), zaman içinde gelişen, yaygın sorunları çözen, iyi okunan ve kod bakım gereksinimleri için sağlam olan kalıplardan bazılarını belgeler.

Bu en iyi uygulamalar standart değildir, ancak Google'daki Go programcılarının, kod tabanını tekdüze ve tutarlı tutmak için mümkün olan yerlerde bunları kullanmaları önerilir.

Bu belgelerin **amaçladığı**:
- Alternatif stilleri tartmak için bir dizi ilke üzerinde anlaşmaya varma,
- Go stilinin yerleşik olaylarını kodlama,
- Go deyimleri için standart örnekler sağlama,
- Çeşitli stil kararlarının artılarını ve eksilerini belgeleme,
- Go okunabilirlik incelemelerinde sürprizleri en aza indirmeye yardımcı olma,
- Okunabilirlik danışmanlarının tutarlı terminoloji ve rehberlik kullanmasına yardımcı olmadır.

Bu belgelerin **amaçlamadığı**:
- Okunabilirlik incelemesinde verilebilecek kapsamlı bir yorum listesi olmaması,
- Herkesin her zaman hatırlaması ve uyması beklenen tüm kuralları listelememesi,
- Dil özelliklerinin ve stilinin kullanımında sağduyunun yerine geçmemesi,
- Stil farklılıklarından kurtulmak için büyük ölçekli değişiklikleri gerekçelendirmemesidir.

Bir Go programcısından diğerine ve bir takımın kod tabanından diğerine her zaman farklılıklar olacaktır. Ancak, kod tabanımızın olabildiğince tutarlı olması Google ve Alphabet'in yararınadır. (Tutarlılık hakkında daha fazla bilgi için [kılavuza](change) bakın.) Bu amaçla, uygun gördüğünüz şekilde stil iyileştirmeleri yapmaktan çekinmeyin, ancak Stil Kılavuzunda bulduğunuz her ihlali gözden kaçırmanıza gerek yoktur. Özellikle, bu belgeler zaman içinde değişebilir ve bu, mevcut kod tabanlarında fazladan karışıklığa neden olmak için bir neden değildir; en son en iyi uygulamaları kullanarak yeni kod yazmak ve yakınlardaki sorunları zaman içinde ele almak yeterlidir.

Stil meselelerinin doğası gereği kişisel olduğunu ve her zaman içsel ödünleşimler olduğunu kabul etmek önemlidir. Bu belgelerdeki rehberliğin çoğu özneldir, ancak tıpkı `gofmt`'ta olduğu gibi, sağladıkları tekdüzelikte önemli bir değer vardır. Bu nedenle, stil önerileri gerekli konuşma yapılmadan değiştirilmeyecektir. Google'daki Go programcıları, aynı fikirde olmasalar bile stil kılavuzunu takip etmeye teşvik edilir.

## Tanımlamalar

Stil belgeleri boyunca kullanılan aşağıdaki kelimeler aşağıda tanımlanmıştır:
 - ### **Kanonik** _(Kanuni, Kuralcı)_ : Kuralcı ve kalıcı kurallar oluşturur.

Bu belgelerde "kanonik", tüm kodların _(eski ve yeni)_ uyması gereken ve zaman içinde önemli ölçüde değişmesi beklenmeyen bir standart olarak kabul edilen bir şeyi tanımlamak için kullanılır. Kanonik belgelerdeki ilkeler, hem yazarlar hem de hakemler tarafından anlaşılmalıdır. Bu nedenle standart bir belgede yer alan her şey yüksek bir çıtayı karşılamalıdır. Bu nedenle, standart belgeler genellikle daha kısadır ve standart olmayan belgelere göre daha az stil öğesi belirtir.

https://google.github.io/styleguide/go#canonical

- ### Normatif: Tutarlılık oluşturma amaçlı.

Bu belgelerde "normatif", önerilerin, terminolojinin ve gerekçelerin tutarlı olması için Go kod gözden geçirenleri tarafından kullanım için üzerinde anlaşmaya varılan bir stil öğesi olan bir şeyi tanımlamak için kullanılır. Bu unsurlar zaman içinde değişebilir ve gözden geçirenlerin tutarlı ve güncel kalabilmesi için bu belgeler bu tür değişiklikleri yansıtacaktır. Go kodunun yazarlarının normatif belgelere aşina olmaları beklenmez, ancak belgeler, okunabilirlik incelemelerinde gözden geçirenler tarafından sıklıkla referans olarak kullanılacaktır.

https://google.github.io/styleguide/go#normative

- ### Deyimsel: Yaygın ve tanıdık.

Bu belgelerde "deyimsel", Go kodunda yaygın olan ve tanınması kolay, tanıdık bir model haline gelen bir şeye atıfta bulunmak için kullanılır. Genel olarak, her ikisi de bağlamda aynı amaca hizmet ediyorsa, deyimsel bir kalıp tek deyimsel bir şeye tercih edilmelidir, çünkü bu okuyucular için en tanıdık olacaktır.

https://google.github.io/styleguide/go#idiomatic

## Ek Referanslar
Bu kılavuz, tüm Go topluluğu genelinde Go kodu için ortak bir temel sağladığından, okuyucunun [Etkili Go](change)'ya aşina olduğunu varsayar.

Aşağıda, Go stili hakkında kendi kendine eğitim almak isteyenler ve incelemelerinde daha fazla bağlantı kurulabilir bağlam sağlamak isteyen incelemeciler için bazı ek kaynaklar bulunmaktadır. Go okunabilirlik sürecindeki katılımcıların bu kaynaklara aşina olmaları beklenmez, ancak okunabilirlik incelemelerinde bağlam olarak ortaya çıkabilirler.

### Dış Referanslar
- [Go Dil Belirtimi](change) [EN]
- [Go SSS](change) [EN]
- [Go Bellek Modeli](change) [EN]
- [Go Veri Yapıları](change) [EN]
- [Go Arabirimler (Interfaces)](change) [EN]
- [Go Atasözleri](change) [EN]
- Go Tip Bölümleri - beklemede kalın
- Birim Testi Pratikleri - beklemede kalın

### İlgili Testing of the Toilet Makaleleri
- [TotT : Tanımlayıcı Adlandırma](change) [EN]
- [TotT : Test Durumu vs. Test Etkileşimleri](change) [EN]
- [TotT : Etkili Test](change) [EN]
- [TotT : Risk Odaklı Test](change) [EN]
- [TotT : Zararlı Olarak Değerlendirilen Değişiklik Dedektörü Testleri](change) [EN]

### Ek Dış Yazılar
- [Go ve Dogma](change) [EN]
- [Daha azı, katlanarak daha fazladır](change) [EN]
- [Esmerelda'nın Hayal Gücü](change) [EN]
- [Ayrıştırma için Düzenli İfadeler (Regexp)](change) [EN]
- [Gofmt'un tarzı kimsenin favorisi değil ama Gofmt herkesin favorisi](change) (YouTube) [EN]