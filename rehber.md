# Google Go Rehberi

[Genel Bakış](./README.md) | Rehber | [Kararlar](change) | [Best Practices](change)

**Not:** Bu, Google'da [Go Stili](./README.md)ni özetleyen bir dizi belgenin parçasıdır. Bu belge normatif ve kanoniktir. Daha fazla bilgi için [genel bakış](./README.md) bölümüne bakın.

## Stil İlkeleri

Okunabilir Go kodu yazma konusunda nasıl düşünülmesi gerektiğini özetleyen birkaç kapsayıcı ilke vardır. Aşağıdakiler, önem sırasına göre okunabilir kodun nitelikleridir:

1. [Netlik](change): Kodun amacı ve gerekçesi okuyucu için açıktır.
2. [Basitlik](change): Kod, amacını mümkün olan en basit şekilde gerçekleştirir.
3. [Özlülük](change): Kod yüksek bir sinyal/gürültü _(SNR - anlaşılır veya alakalı -mecazi olarak)_ oranına sahiptir.
4. [Sürdürülebilirlik](change): Kod, bakımı kolayca yapılabilecek şekilde yazılmıştır.
5. [Tutarlılık](change): Kod, daha geniş Google kod tabanı ile tutarlıdır.

### Netlik
Okunabilirliğin temel amacı, okuyucu için anlaşılır bir kod üretmektir.

Netlik öncelikle etkili adlandırma, faydalı yorum ve verimli kod organizasyonu ile sağlanır.

Netlik, kodun yazarının değil okuyucunun merceğinden görülmelidir. Kodun okunmasının kolay olması, yazılmasının kolay olmasından daha önemlidir. Kodda netliğin iki farklı yönü vardır:

#### Kod aslında ne yapıyor?
Go, kodun ne yaptığını görmenin nispeten kolay olacağı şekilde tasarlanmıştır. Belirsizlik durumlarında veya bir okuyucunun kodu anlamak için ön bilgiye ihtiyaç duyabileceği durumlarda, kodun amacını gelecekteki okuyucular için daha net hale getirmek için zaman ayırmaya değer. Örneğin, aşağıdakiler yardımcı olabilir:

- Daha açıklayıcı değişken adları kullanın
- Ek yorum ekleyin
- Kodu boşluk ve yorumlarla ayırın
- Daha modüler hale getirmek için kodu ayrı fonksiyonlara/yöntemlere dönüştürün

Burada herkese uyan tek bir yaklaşım yoktur, ancak Go kodu geliştirirken netliğe öncelik vermek önemlidir.

#### Kod yaptığı şeyi neden yapıyor?
Kodun mantığı genellikle değişkenlerin, fonksiyonların, metodların veya paketlerin adlarıyla yeterince iletilir. Bunun olmadığı durumlarda yorum eklemek önemlidir. _"Neden?"_ sorusu özellikle kod okuyucunun aşina olmayabileceği ayrıntılar içerdiğinde önemlidir:

- Dildeki bir ayrıntı, örneğin bir kapanış bir döngü değişkenini yakalayacaktır, ancak kapanış birçok satır uzaktadır
- İş mantığının _(business logic)_ bir ayrıntısı, örneğin gerçek kullanıcı ile kullanıcıyı taklit eden birini ayırt etmesi gereken bir erişim kontrolü
- 
Bir API'nin doğru şekilde kullanılması özen gerektirebilir. Örneğin, bir kod parçası karmaşık olabilir ve performans nedenleriyle takip edilmesi zor olabilir veya karmaşık bir matematiksel işlemler dizisi tip dönüşümlerini beklenmedik bir şekilde kullanabilir. Bu ve daha pek çok durumda, gelecekteki bakımcıların _(maintainer)_ hata yapmaması ve okuyucuların kodu tersine mühendisliğe gerek kalmadan anlayabilmesi için eşlik eden yorum ve belgelerin bu hususları açıklaması önemlidir.

Açıklık sağlamaya yönelik bazı girişimlerin _(fazladan yorum eklemek gibi)_ dağınıklık yaratarak, kodun zaten söylediği şeyleri yeniden ifade ederek, kodla çelişerek veya yorumları güncel tutmak için bakım yükü ekleyerek kodun amacını gizleyebileceğinin farkında olmak da önemlidir. Gereksiz yorumlar eklemek yerine kodun kendi adına konuşmasına izin verin _(örneğin, sembol adlarının kendilerini tanımlamasını sağlayarak)_. Yorumların kodun ne yaptığını değil, bir şeyin neden yapıldığını açıklaması genellikle daha iyidir.

Google kod tabanı büyük ölçüde tek tip ve tutarlıdır. Genellikle göze çarpan kodlar _(örneğin, alışılmadık bir model kullanarak)_ bunu iyi bir nedenle, genellikle performans için yapar. Bu özelliği korumak, okuyucuların yeni bir kod parçasını okurken dikkatlerini nereye vermeleri gerektiğini netleştirmek açısından önemlidir.

Standart kütüphanede bu prensibin işlediği birçok örnek bulunmaktadır. Bunların arasında:

- [sort paketi](https://cs.opensource.google/go/go/+/refs/tags/go1.19.2:src/sort/sort.go) bakımcı yorumları.
- Aynı pakette hem kullanıcılara ([godoc](https://pkg.go.dev/sort#pkg-examples)'ta görünürler) hem de bakımcılara ([testlerin bir parçası](https://google.github.io/styleguide/go/decisions#examples) olarak çalışırlar) fayda sağlayan [iyi çalıştırılabilir örnekler](https://cs.opensource.google/go/go/+/refs/tags/go1.19.2:src/sort/example_search_test.go).
- [strings.Cut](https://pkg.go.dev/strings#Cut) sadece dört satırlık bir koddur, ancak [çağrı sitelerinin netliğini ve doğruluğunu](https://github.com/golang/go/issues/46336) geliştirir.

### Basitlik
Go kodunuz, onu kullanan, okuyan ve bakımını yapan kişiler için basit olmalıdır.

Go kodu, hem davranış hem de performans açısından hedeflerini gerçekleştiren en basit şekilde yazılmalıdır. Google Go kod tabanında, basit kod:

- Yukarıdan aşağıya okunması kolaydır
- Ne yaptığını zaten bildiğinizi varsaymaz
- Önceki kodların tümünü ezberleyebileceğinizi varsaymaz
- Gereksiz soyutlama seviyelerine sahip değildir
- Sıradan bir şeye dikkat çeken isimlere sahip değil
- Değerlerin ve kararların yayılımını okuyucuya açık hale getirir
- Gelecekteki sapmaları önlemek için kodun ne yaptığını değil neden yaptığını açıklayan yorumlara sahiptir
- Kendi başına ayakta duran belgelere sahip
- Faydalı hatalara ve faydalı test hatalarına sahiptir
- Genellikle "akıllı" kod ile karşılıklı olarak münhasır olabilir

Kod basitliği ile API kullanım basitliği arasında ödünleşimler ortaya çıkabilir. Örneğin, API'nin son kullanıcısının API'yi doğru bir şekilde daha kolay çağırabilmesi için kodun daha karmaşık olması faydalı olabilir. Buna karşılık, kodun basit ve anlaşılması kolay kalması için API'nin son kullanıcısına biraz fazladan iş bırakmak da faydalı olabilir.

Kod karmaşıklığa ihtiyaç duyduğunda, karmaşıklık kasıtlı olarak eklenmelidir. Bu genellikle ek performans gerektiğinde veya belirli bir kütüphane ya da hizmetin birden fazla farklı müşterisi olduğunda gereklidir. Karmaşıklık haklı olabilir, ancak müşterilerin ve gelecekteki bakımcıların karmaşıklığı anlayabilmeleri ve yönlendirebilmeleri için eşlik eden dökümanlarla birlikte gelmelidir. Özellikle kodu kullanmanın hem "basit" hem de "karmaşık" bir yolu varsa, bu doğru kullanımı gösteren testler ve örneklerle desteklenmelidir.

Bu ilke, karmaşık kodun Go'da yazılamayacağı veya yazılmaması gerektiği ya da Go kodunun karmaşık olmasına izin verilmediği anlamına gelmez. Gereksiz karmaşıklıktan kaçınan bir kod tabanı için çabalıyoruz, böylece karmaşıklık ortaya çıktığında, söz konusu kodun anlaşılması ve sürdürülmesi için özen gerektirdiğini gösterir. İdeal olarak, gerekçeyi açıklayan ve gösterilmesi gereken özeni tanımlayan eşlik eden yorumlar olmalıdır. Bu durum genellikle kodu performans için optimize ederken ortaya çıkar; bunu yapmak genellikle bir tamponu _(buffer)_ önceden ayırmak ve bir goroutine ömrü boyunca yeniden kullanmak gibi daha karmaşık bir yaklaşım gerektirir. Bir bakımcı bunu gördüğünde, söz konusu kodun performans açısından kritik olduğuna dair bir ipucu olmalı ve bu durum gelecekteki değişiklikler yapılırken gösterilen özeni etkilemelidir. Öte yandan, gereksiz yere kullanılırsa, bu karmaşıklık gelecekte kodu okuması veya değiştirmesi gerekenler için bir yüktür.

Kodun amacının basit olması gerekirken çok karmaşık olduğu ortaya çıkarsa, bu genellikle aynı şeyi başarmanın daha basit bir yolu olup olmadığını görmek için uygulamayı yeniden gözden geçirmek için bir işarettir.

#### En az mekanizma
Aynı fikri ifade etmenin birkaç yolu varsa, en standart araçları kullananı tercih edin. Sofistike mekanizmalar genellikle mevcuttur, ancak sebepsiz yere kullanılmamalıdır. Gerektiğinde koda karmaşıklık eklemek kolaydır, oysa gereksiz olduğu anlaşıldıktan sonra mevcut karmaşıklığı ortadan kaldırmak çok daha zordur.

1. Kullanım durumunuz için yeterli olduğunda temel bir dil yapısı _(örneğin bir channel, slice, harita, loop veya struct)_ kullanmayı hedefleyin.
2. Böyle bir yapı yoksa, standart kütüphane içinde bir araç arayın _(HTTP istemcisi veya şablon motoru-template engine gibi)_.
3. Son olarak, yeni bir bağımlılık eklemeden veya kendi kütüphanenizi oluşturmadan önce Google kod tabanında yeterli bir çekirdek kütüphane olup olmadığını değerlendirin.

Örnek olarak, testlerde geçersiz kılınması gereken varsayılan bir değere sahip bir değişkene bağlı bir bayrak içeren üretim kodunu düşünün. Programın komut satırı arayüzünün kendisini (örneğin `os/exec` ile) test etme niyetinde değilseniz, `flag.Set` kullanmak yerine bağlı değeri doğrudan geçersiz kılmak daha basittir ve bu nedenle tercih edilir.

Benzer şekilde, bir kod parçası küme üyeliği kontrolü gerektiriyorsa, `boolean` değerli bir map (örneğin, `map[string]bool`) genellikle yeterli olur. Küme benzeri türler ve fonksiyonlar sağlayan kütüphaneler, yalnızca bir map ile imkansız veya aşırı karmaşık olan daha karmaşık işlemler gerektiğinde kullanılmalıdır.

### Özlülük
Kısa ve öz Go kodu yüksek bir sinyal/gürültü oranına sahiptir. İlgili ayrıntıları ayırt etmek kolaydır. Adlandırma ve yapı, okuyucuyu bu ayrıntılar boyunca yönlendirir.

Herhangi bir zamanda en göze çarpan ayrıntıları ortaya çıkarmanın önüne geçebilecek pek çok şey vardır:

- Tekrarlayan kod
- Gereksiz sözdizimi
- [Opak isimler](change)
- Gereksiz soyutlama
- Boşluk Alan _(White Space)_

Tekrar eden kod özellikle neredeyse aynı olan her bölüm arasındaki farkları belirsizleştirir ve okuyucunun değişiklikleri bulmak için benzer kod satırlarını görsel olarak karşılaştırmasını gerektirir. [Tablo odaklı test](change), her bir tekrarın önemli ayrıntılarından ortak kodu kısaca ayırabilen bir mekanizmanın iyi bir örneğidir, ancak tabloya hangi parçaların dahil edileceğinin seçimi, tablonun ne kadar kolay anlaşılacağı üzerinde bir etkiye sahip olacaktır.

Kodu yapılandırmak için birden fazla yol düşünüldüğünde, hangi yolun önemli ayrıntıları en belirgin hale getirdiği dikkate alınmalıdır.

Ortak kod yapılarını ve deyimleri anlamak ve kullanmak da yüksek bir sinyal/gürültü oranını korumak için önemlidir. Örneğin, aşağıdaki kod bloğu [hata yakalamada](https://go.dev/blog/errors-are-values) çok yaygındır ve okuyucu bu bloğun amacını hızlı bir şekilde anlayabilir.

```go
// İyi:
if err := doSomething(); err != nil {
    // ...
}
```

Kod buna çok benziyor ancak çok az farklıysa, okuyucu değişikliği fark etmeyebilir. Bu gibi durumlarda, dikkat çekmek için bir yorum ekleyerek hata kontrolünün sinyalini kasıtlı olarak "[artırmaya](change)" değer.

```go
// İyi:
if err := doSomething(); err == nil { // eğer hata YOKSA
    // ...
}
```
### Sürdürülebilirlik
Kod, yazıldığından çok daha fazla kez düzenlenir. Okunabilir kod sadece nasıl çalıştığını anlamaya çalışan bir okuyucu için değil, aynı zamanda onu değiştirmesi gereken programcı için de anlamlıdır. Netlik burada anahtar kelimedir.

Sürdürülebilir kod:

- Gelecekteki bir programcının doğru şekilde değiştirmesi kolaydır
- Zarif bir şekilde büyüyebilecek şekilde yapılandırılmış API'lere sahiptir
- Yaptığı varsayımlar konusunda nettir ve kodun yapısıyla değil, sorunun yapısıyla eşleşen soyutlamaları seçer
- Gereksiz bağlantılardan kaçınır ve kullanılmayan özellikleri içermez
- Söz verilen davranışların sürdürüldüğünden ve önemli mantığın doğru olduğundan emin olmak için kapsamlı bir test paketine sahiptir ve testler arıza durumunda net, eyleme geçirilebilir teşhisler sağlar
- 
Arayüzler _(interface)_ ve tipler _(type)_ gibi, tanımları gereği kullanıldıkları bağlamdan bilgi kaldıran soyutlamalar kullanılırken, bunların yeterli fayda sağladığından emin olmak önemlidir. Editörler ve IDE'ler, somut bir tip kullanıldığında doğrudan bir yöntem tanımına bağlanabilir ve ilgili belgeleri gösterebilir, ancak aksi takdirde yalnızca bir arayüz tanımına başvurabilir. Arayüzler güçlü bir araçtır, ancak bir maliyeti vardır, çünkü bakımcının arayüzü doğru bir şekilde kullanması için temel uygulamanın özelliklerini anlaması gerekebilir, bu da arayüz belgelerinde veya çağrı yerinde açıklanmalıdır.

Sürdürülebilir kod, gözden kaçırılması kolay yerlerde önemli ayrıntıların gizlenmesini de önler. Örneğin, aşağıdaki kod satırlarının her birinde, tek bir karakterin varlığı veya yokluğu satırı anlamak için kritik öneme sahiptir:

```go
// Kötü:
// :=  yerine = kullanılması bu satırı tamamen değiştirebilir
if user, err = db.UserByID(userID); err != nil {
    // ...
}
```

```go
// Kötü:
// Satırın ortasındaki ! kolaylıkla gözden kaçabilir
leap := (year%4 == 0) && (!(year%100 == 0) || (year%400 == 0))
```
Bunların hiçbiri yanlış değildir, ancak her ikisi de daha açık bir şekilde yazılabilir veya önemli davranışa dikkat çeken bir yorum eklenebilir:

```go
// İyi:
u, err := db.UserByID(userID)
if err != nil {
    return fmt.Errorf("invalid origin user: %s", err)
}
user = u
```
```go
// İyi:
// Gregorian (Miladi) artık yılları sadece year%4 == 0 değildir.
// Bkz. https://en.wikipedia.org/wiki/Leap_year#Algorithm.
var (
    leap4   = year%4 == 0
    leap100 = year%100 == 0
    leap400 = year%400 == 0
)
leap := leap4 && (!leap100 || leap400)
```

Aynı şekilde, kritik bir mantığı veya önemli bir uç durumu gizleyen bir yardımcı fonksiyon, gelecekteki bir değişikliğin bunu düzgün bir şekilde hesaba katmamasını kolaylaştırabilir.

Tahmin edilebilir isimler sürdürülebilir kodun bir başka özelliğidir. Bir paketin kullanıcısı veya bir kod parçasının bakımını yapan kişi, belirli bir bağlamda bir değişkenin, metodun veya fonsiyonun adını tahmin edebilmelidir. Aynı kavramlar için fonksiyon parametreleri ve alıcı adları _(`func (a alıcıİsmi) fonksiyonİsmi()`)_, hem dokümantasyonu anlaşılır tutmak hem de kodu minimum ek yük ile yeniden düzenlemeyi kolaylaştırmak için genellikle aynı adı paylaşmalıdır.

Sürdürülebilir kod, bağımlılıklarını en aza indirir _(hem örtük hem de açık)_. Daha az pakete bağlı olmak, davranışı etkileyebilecek daha az kod satırı anlamına gelir. Dahili veya belgelenmemiş davranışlara bağımlılıktan kaçınmak, gelecekte bu davranışlar değiştiğinde kodun bakım yükü getirme olasılığını azaltır.

Kodu nasıl yapılandıracağınızı veya yazacağınızı düşünürken, kodun zaman içinde nasıl gelişebileceğini düşünmek için zaman ayırmaya değer. Belirli bir yaklaşım gelecekte daha kolay ve daha güvenli değişikliklere daha elverişli ise, biraz daha karmaşık bir tasarım anlamına gelse bile, bu genellikle iyi bir değiş tokuştur.

### Tutarlılık
Tutarlı kod, daha geniş kod tabanında, bir ekip veya paket bağlamında ve hatta tek bir dosya içinde benzer kod gibi görünen, hissedilen ve davranan koddur.

Tutarlılık kaygıları yukarıdaki ilkelerin hiçbirini geçersiz kılmaz, ancak bir eşitliğin bozulması gerekiyorsa, tutarlılık lehine bozmak genellikle faydalıdır.

Bir paket içindeki tutarlılık, genellikle tutarlılığın en önemli seviyesidir. Aynı soruna bir paket içinde farklı şekillerde yaklaşılması ya da aynı kavramın bir dosya içinde farklı isimlere sahip olması çok sarsıcı olabilir. Ancak bu durum bile belgelenmiş stil ilkelerini veya global tutarlılığı geçersiz kılmamalıdır.

## Temel kılavuz ilkeler
Bu yönergeler, tüm Go kodlarının uyması beklenen Go stilinin en önemli yönlerini bir araya getirir. Okunabilirlik sağlandığında bu ilkelerin öğrenilmesini ve takip edilmesini bekliyoruz. Bunların sık sık değişmesi beklenmemektedir ve yeni eklemelerin yüksek bir çıtayı aşması gerekecektir.

Aşağıdaki yönergeler, tüm topluluk genelinde Go kodu için ortak bir temel sağlayan [Effective Go](https://go.dev/doc/effective_go)'daki önerileri genişletmektedir.

### Biçimlendirme
Tüm Go kaynak dosyaları, `gofmt` aracı tarafından çıkarılan biçime uygun olmalıdır. Bu biçim, Google kod tabanında bir ön gönderim kontrolü ile zorunlu kılınır. [Oluşturulan kod](https://docs.bazel.build/versions/main/be/general.html#genrule), Kod Arama'da da taranabildiği için genellikle biçimlendirilmelidir (örneğin, [format.Source](https://pkg.go.dev/go/format#Source) kullanılarak).

### MixedCaps
Go kaynak kodu, çok sözcüklü adları yazarken alt çizgi yerine `MixedCaps` veya `mixedCaps` _(Camel Case)_ kullanır.

Bu, diğer dillerdeki gelenekleri bozsa bile geçerlidir. Örneğin, bir sabit dışa aktarılmışsa `MaxLength` _(`MAX_LENGTH` değil)_ ve dışa aktarılmamışsa `maxLength` _(`max_length` değil)_ olur.

Yerel değişkenler, başlangıç kapitalizasyonunun seçilmesi amacıyla [dışa aktarılmamış](https://go.dev/ref/spec#Exported_identifiers) olarak kabul edilir.

### Satır Uzunluğu
Go kaynak kodu için sabit bir satır uzunluğu yoktur. Eğer bir satır çok uzun geliyorsa, kırmak yerine yeniden düzenlenmelidir. Zaten olması gerektiği kadar kısaysa, satırın uzun kalmasına izin verilmelidir.

- Bir [girinti değişikliğinden](https://google.github.io/styleguide/go/decisions#indentation-confusion) önce (örn. fonksiyon bildirimi, koşullu),
- Uzun bir `string`'i (örneğin bir URL) birden fazla kısa satıra sığdırmak için,

satırı bölmemeye dikkat edin.

### İsimlendirme
İsimlendirme bilimden çok sanattır. Go'da isimler diğer birçok dile göre biraz daha kısa olma eğilimindedir, ancak [aynı genel kurallar](https://testing.googleblog.com/2017/10/code-health-identifiernamingpostforworl.html) geçerlidir. İsimler şöyle olmalıdır:

- Kullanıldıklarında [tekrarlayıcı](https://google.github.io/styleguide/go/decisions#repetition) hissettirmemeleri,
- Bağlamı göz önünde bulundurmaları,
- Zaten açık olan kavramları tekrarlamamaları,

gerekir.

[Kararlar](change)da isimlendirme konusunda daha spesifik rehberlik bulabilirsiniz.

### Yerel tutarlılık
Stil kılavuzunun belirli bir stil noktası hakkında söyleyecek bir şeyi olmadığı durumlarda, yakın çevredeki kod (genellikle aynı dosya veya paket içinde, ancak bazen bir ekip veya proje dizini içinde) konuyla ilgili tutarlı bir duruş sergilemediği sürece, yazarlar tercih ettikleri stili seçmekte özgürdür.

Geçerli yerel stil hususlarına örnekler:

- Hataların biçimlendirilmiş yazdırılması için `%s` veya `%v` kullanımı,
- Muteksler yerine tamponlanmış kanalların _(buffered channel)_ kullanımı.

Geçersiz yerel stil hususlarına örnekler:

- Kod için satır uzunluğu kısıtlamaları,
- İddia _(assertion)_ tabanlı test kütüphanelerinin kullanımı.

Yerel stil, stil kılavuzuyla uyuşmuyorsa ancak okunabilirlik etkisi bir dosyayla sınırlıysa, genellikle tutarlı bir düzeltmenin söz konusu CL'nin _(Control Language - Kontrol Dili)_ kapsamı dışında olacağı bir kod incelemesinde ortaya çıkacaktır. Bu noktada, düzeltmeyi takip etmek için bir hata dosyası oluşturmak uygun olacaktır.

Bir değişiklik mevcut bir stil sapmasını daha da kötüleştirecekse, daha fazla API yüzeyinde ortaya çıkaracaksa, sapmanın mevcut olduğu dosya sayısını artıracaksa veya gerçek bir hata ortaya çıkaracaksa, yerel tutarlılık artık yeni kod için stil kılavuzunu ihlal etmek için geçerli bir gerekçe değildir. Bu durumlarda, yazarın aynı CL'deki mevcut kod tabanını temizlemesi, mevcut CL'den önce bir refactor gerçekleştirmesi veya en azından yerel sorunu daha da kötüleştirmeyen bir alternatif bulması uygundur.
