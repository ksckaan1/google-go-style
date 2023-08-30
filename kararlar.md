# Google Go Kararları

[Genel Bakış](./README.md) | [Rehber](./rehber.md) | Kararlar | [Best Practices](change)

**Not:** Bu, Google'da [Go Stili](./README.md)ni özetleyen bir dizi belgenin parçasıdır. Bu belge [normatif](change)tir ancak kanonik değildir ve temel stil kılavuzuna tabidir. Daha fazla bilgi için genel bakış bölümüne bakın

## Hakkında

Bu belge, Go okunabilirlik danışmanları tarafından verilen tavsiyeler için standart rehberlik, açıklama ve örnekleri birleştirmeyi ve sağlamayı amaçlayan stil kararlarını içerir.

Bu belge **kapsamlı değildir** ve zaman içinde gelişecektir. [Temel stil kılavuzu](change)nun burada verilen tavsiyelerle çeliştiği durumlarda, **stil kılavuzu önceliklidir** ve bu belge buna göre güncellenmelidir.

Go Style belgelerinin tamamı için [Genel Bakış](change) bölümüne bakın.

Aşağıdaki bölümler stil kararlarından kılavuzun başka bir bölümüne taşınmıştır:

- **MixedCaps**: bkz. [rehber#mixed-caps](change)
- **Biçimlendirme**: bkz. [rehber#biçimlendirme](change)
- **Satır Uzunluğu**: bkz. [rehber#satır-uzunluğu](change)

## İsimlendirme
Adlandırma konusunda kapsamlı rehberlik için [temel stil kılavuzu](change)ndaki adlandırma bölümüne bakın. Aşağıdaki bölümler, adlandırma kapsamındaki belirli alanlara ilişkin daha fazla açıklama sağlamaktadır.

### Alt çizgiler
Go'daki isimler genel olarak alt çizgi içermemelidir. Bu ilkenin üç istisnası vardır:

1. Yalnızca oluşturulan kod tarafından içe aktarılan paket adları alt çizgi içerebilir. Çok sözcüklü paket adlarının nasıl seçileceği hakkında daha fazla ayrıntı için [paket adları](change) bölümüne bakın.
2. Test, Benchmark ve `*_test.go` dosyaları içindeki Örnek fonksiyon adları alt çizgi içerebilir.
3. İşletim sistemi veya `cgo` ile birlikte çalışan düşük seviyeli kütüphaneler, [syscall](https://pkg.go.dev/syscall#pkg-constants)'da olduğu gibi tanımlayıcıları yeniden kullanabilir. Bu durumun çoğu kod tabanında çok nadir olması beklenir.

### Paket isimleri

Go paket adları kısa olmalı ve yalnızca küçük harfler içermelidir. Birden fazla sözcükten oluşan bir paket adı, tümüyle küçük harflerle kesintisiz olarak bırakılmalıdır. Örneğin, [tabwriter](https://pkg.go.dev/text/tabwriter) paketinin adı `tabWriter`, `TabWriter` veya `tab_writer` değildir.

Yaygın olarak kullanılan yerel değişken adları tarafından [gölgelenmesi](change) muhtemel paket adlarını seçmekten kaçının. Örneğin, `count` yaygın olarak kullanılan bir değişken adı olduğu için `usercount`, `count`'tan daha iyi bir paket adıdır.

Go paket adlarında alt çizgi olmamalıdır. Adında alt çizgi olan bir paketi içe aktarmanız gerekiyorsa _(genellikle oluşturulan veya üçüncü taraf kodundan)_, içe aktarma sırasında Go kodunda kullanıma uygun bir adla yeniden adlandırılmalıdır.

Bunun bir istisnası, yalnızca oluşturulan kod tarafından içe aktarılan paket adlarının alt çizgi içerebilmesidir. Belirli örnekler şunlardır:

- Harici bir test paketi için `_test` son ekinin kullanılması, örneğin bir entegrasyon testi

- [Paket düzeyinde belge örnekleri](https://go.dev/blog/examples) için `_test` son ekini kullanma

`util`, `utility`, `common`, `helper` ve benzeri bilgilendirici olmayan paket adlarından kaçının. ["Yardımcı paketler" hakkında](change) daha fazla bilgi edinin.

İçe aktarılan bir paket yeniden adlandırıldığında _(örneğin `import foopb "path/to/foo_go_proto"`)_, paketin yerel adı yukarıdaki kurallara uygun olmalıdır, çünkü yerel ad paketteki sembollerin dosyada nasıl referans alınacağını belirler. Belirli bir içe aktarma birden fazla dosyada, özellikle aynı veya yakın paketlerde yeniden adlandırılırsa, tutarlılık için mümkün olan her yerde aynı yerel ad kullanılmalıdır.

Ayrıca bakınız: [Paket adları hakkında blog gönderisi](https://go.dev/blog/package-names)ne gidin.

### Alıcı isimleri

[Alıcı](https://golang.org/ref/spec#Method_declarations) değişken adları şöyle olmalıdır:

- Kısa (genellikle bir veya iki harf uzunluğunda)
- Tipin kendisi için kısaltmalar
- Bu tür için her alıcıya tutarlı bir şekilde uygulanır

| Uzun İsim                 | Kısa İsim               |
|---------------------------|-------------------------|
| func (tray Tray)          | func (t Tray)           |
| func (info *ResearchInfo) | func (ri *ResearchInfo) |
| func (this *ReportWriter) | 	func (w *ReportWriter) |
| func (self *Scanner)      | 	func (s *Scanner)      |

### Sabit isimleri
Sabit isimleri Go'daki diğer tüm isimler gibi [MixedCaps](change) kullanmalıdır. _([Dışa aktarılan](https://tour.golang.org/basics/3) sabitler büyük harfle başlarken, dışa aktarılmayan sabitler küçük harfle başlar)_. Bu, diğer dillerdeki gelenekleri bozsa bile geçerlidir. Sabit isimleri değerlerinin bir türevi olmamalı ve bunun yerine değerin neyi ifade ettiğini açıklamalıdır.

```go
// İyi:
const MaxPacketSize = 512

const (
    ExecuteBit = 1 << iota
    WriteBit
    ReadBit
)
```

MixedCaps olmayan sabit adları veya `K` önekine sahip sabitleri kullanmayın.

```go
// Kötü:
const MAX_PACKET_SIZE = 512
const kMaxBufferSize = 1024
const KMaxUsersPergroup = 500
```

Sabitleri değerlerine göre değil, rollerine göre adlandırın. Bir sabitin değeri dışında bir rolü yoksa, onu sabit olarak tanımlamak gereksizdir.

```go
// Kötü:
const Twelve = 12

const (
    UserNameColumn = "username"
    GroupColumn    = "group"
)
```

### Büyük harfler

Baş harf veya kısaltma olan isimlerdeki kelimeler _(örneğin, `URL` ve `NATO`)_ aynı büyüklükte olmalıdır. `URL`, `URL` veya `url` _(`urlPony` veya `URLPony`'de olduğu gibi)_ olarak görünmeli, asla `Url` olarak görünmemelidir. Bu aynı zamanda `identifier`'ın kısaltması olan `ID` için de geçerlidir; `appId` yerine `appID` yazın.

Birden fazla baş harf içeren isimlerde _(örneğin `XMLAPI`, `XML` ve `API` içerdiği için)_, belirli bir baş harf içindeki her harf aynı büyüklükte olmalıdır, ancak isimdeki her baş harfin aynı büyüklükte olması gerekmez.

Baş harfi küçük harf içeren adlarda _(örneğin **DDoS**, `iOS`, `gRPC`)_, [dışa aktarılabilirlik](https://golang.org/ref/spec#Exported_identifiers) için ilk harfi değiştirmeniz gerekmedikçe, baş harf standart düzyazıda olduğu gibi görünmelidir. Bu durumlarda, baş harfin tamamı aynı büyüklükte olmalıdır _(örn. `ddos`, `IOS`, `GRPC`)_.

| Büyük Harfler | Kapsam             | Doğru  | Yanlış                         |
|---------------|--------------------|--------|--------------------------------|
| XML API       | 	Dışa Aktarılmış   | XMLAPI | XmlApi, XMLApi, XmlAPI, XMLapi |
| XML API       | 	Dışa Aktarılmamış | xmlAPI | xmlapi, xmlApi                 |
| iOS           | 	Dışa Aktarılmış   | IOS    | Ios, IoS                       |
| iOS           | 	Dışa Aktarılmamış | iOS    | ios                            |
| gRPC          | 	Dışa Aktarılmış   | GRPC   | Grpc                           |
| gRPC          | 	Dışa Aktarılmamış | gRPC   | grpc                           |
| DDoS          | 	Dışa Aktarılmış   | DDoS   | DDOS, Ddos                     |
| DDoS          | 	Dışa Aktarılmamış | ddos   | dDoS, dDOS                     |

### Getters

Temel kavram `get` kelimesini kullanmadığı sürece (örneğin bir HTTP GET), fonksiyon ve metod adlarında `Get` veya `get` öneki kullanılmamalıdır. İsme doğrudan isimle başlamayı tercih edin, örneğin `GetCounts` yerine `Counts` kullanın.

Fonksiyon karmaşık bir hesaplama yapmayı veya uzaktan bir çağrı yürütmeyi içeriyorsa, okuyucuya fonksiyon çağrısının zaman alabileceğini ve engellenebileceğini veya başarısız olabileceğini açıkça belirtmek için `Get` yerine `Compute` veya `Fetch` gibi farklı bir kelime kullanılabilir.

### Değişken adları

Genel kural, bir ismin uzunluğunun kapsamının boyutuyla orantılı ve bu kapsam içinde kaç kez kullanıldığıyla ters orantılı olması gerektiğidir. Dosya kapsamında oluşturulan bir değişken birden fazla kelime gerektirebilirken, tek bir iç blok kapsamındaki bir değişken, kodu açık tutmak ve gereksiz bilgilerden kaçınmak için tek bir kelime veya hatta sadece bir veya iki karakter olabilir.

İşte kabaca bir temel. Bu sayısal kılavuzlar katı kurallar değildir. Bağlama, [netliğe](change) ve [özlülüğe](change) göre karar verin.

- Küçük kapsam, 1-7 satır gibi bir veya iki küçük işlemin gerçekleştirildiği kapsamdır.
- Orta kapsam, birkaç küçük veya bir büyük işlemdir, örneğin 8-15 satır.
- Büyük kapsam, bir veya birkaç büyük işlemdir, örneğin 15-25 satır.
- Çok geniş bir kapsam, bir sayfadan daha fazlasını kapsayan herhangi bir şeydir (örneğin, 25 satırdan fazla).
- 
Küçük bir kapsamda son derece açık olan bir isim _(örneğin, bir sayaç için `c`)_ daha büyük bir kapsamda yetersiz kalabilir ve kodun ilerleyen kısımlarında okuyucuya amacını hatırlatmak için açıklama gerektirebilir. Çok sayıda değişkenin veya benzer değerleri ya da kavramları temsil eden değişkenlerin bulunduğu bir kapsam, kapsamın önerdiğinden daha uzun değişken adları gerektirebilir.

Kavramın özgüllüğü, bir değişkenin adının kısa tutulmasına da yardımcı olabilir. Örneğin, kullanımda yalnızca tek bir veritabanı olduğu varsayıldığında, normalde çok küçük kapsamlar için ayrılabilecek `db` gibi kısa bir değişken adı, kapsam çok büyük olsa bile mükemmel bir şekilde anlaşılır kalabilir. Bu durumda, kapsamın büyüklüğüne bağlı olarak tek bir veritabanı kelimesi muhtemelen kabul edilebilir, ancak `db` kelimesi çok az alternatif yorumu olan çok yaygın bir kısaltma olduğu için gerekli değildir.

Bir yerel değişkenin adı, değerin nereden geldiğinden ziyade ne içerdiğini ve mevcut bağlamda nasıl kullanıldığını yansıtmalıdır. Örneğin, en iyi yerel değişken adının `struct` veya `protokol tampon` _(buffer)_ alan adıyla aynı olmaması sıklıkla karşılaşılan bir durumdur.

Genel olarak:

- `count` veya `options` gibi tek kelimelik isimler iyi bir başlangıç noktasıdır.
- Benzer adları belirsizleştirmek için ek sözcükler eklenebilir, örneğin `userCount` ve `projectCount`.
- Yazmaktan tasarruf etmek için sadece harfleri atmayın. Örneğin `Sandbox`, özellikle dışa aktarılan adlar için `Sbx` yerine tercih edilir.
- Çoğu değişken adından [türleri ve tür benzeri kelimeler](change)i çıkarın.
  - Bir sayı için `userCount`, `numUsers` veya `usersInt`'ten daha iyi bir isimdir.
  - Bir dilim için, `users`, `userSlice`'dan daha iyi bir isimdir.
  - Kapsam dahilinde bir değerin iki versiyonu varsa tür benzeri bir niteleyici eklemek kabul edilebilir, örneğin `ageString` olarak saklanan bir girdiniz olabilir ve ayrıştırılmış değer için `age` kullanabilirsiniz.
- [Çevreleyen bağlam](change)dan açıkça anlaşılan kelimeleri atlayın. Örneğin, bir `UserCount` metodunun uygulanmasında, `userCount` adlı bir yerel değişken muhtemelen gereksizdir; `count`, `users` veya hatta `c` de aynı şekilde okunabilir.

#### Tek harfli değişken adları
Tek harfli değişken adları tekrarı en aza indirmek için yararlı bir araç olabilir, ancak kodu gereksiz yere opak hale de getirebilir. Kullanımlarını, tam kelimenin açık olduğu ve tek harfli değişken yerine görünmesinin tekrarlayıcı olacağı durumlarla sınırlayın.

Genel olarak:

- Bir metod alıcısı değişkeni için tek harfli veya iki harfli bir ad tercih edilir.
- Yaygın türler için tanıdık değişken adları kullanmak genellikle yararlıdır:
  - `io.Reader` veya `*http.Request` için `r`
  - `io.Writer` veya `http.ResponseWrite`r için `w`
- Tek harfli tanımlayıcılar, özellikle indisler _(index)_ _(örn. `i`)_ ve koordinatlar _(örn. `x` ve `y`)_ için tamsayı döngü değişkenleri olarak kabul edilebilir.
- Kapsam kısa olduğunda kısaltmalar kabul edilebilir döngü tanımlayıcıları olabilir, örneğin `for _, n := range nodes { ... }`.

### Tekrarlama
Bir Go kaynak kodu parçası gereksiz tekrarlardan kaçınmalıdır. Bunun yaygın bir kaynağı, genellikle gereksiz kelimeler içeren veya bağlamlarını veya türlerini tekrarlayan tekrarlayan isimlerdir. Kodun kendisi de aynı veya benzer bir kod parçasının birbirine yakın yerlerde birden çok kez görünmesi durumunda gereksiz yere tekrarlanabilir.

Tekrarlayan isimlendirme, aşağıdakiler de dahil olmak üzere birçok şekilde olabilir:


#### Paket vs. dışa aktarılan sembol adı
Dışa aktarılan sembolleri adlandırırken, paketin adı her zaman paketinizin dışında görünür, bu nedenle ikisi arasındaki gereksiz bilgiler azaltılmalı veya ortadan kaldırılmalıdır. Bir paket yalnızca bir türü dışa aktarıyorsa ve bu türün adı paketin kendisinden sonra geliyorsa, kurucu `(initializer)` için kanonik ad, eğer gerekliyse `New`'dir.

> **Örnekler**: Tekrarlayan İsim -> En Uygun İsim
>
> `widget.NewWidget` -> `widget.New`
> 
> `widget.NewWidgetWithName` -> `widget.NewWithName`
> 
> `db.LoadFromDatabase` -> `db.Load`
> 
> `goatteleportutil.CountGoatsTeleported` -> `gtutil.CountGoatsTeleported` ya da `goatteleport.Count`
> 
> `myteampb.MyTeamMethodRequest` -> `mtpb.MyTeamMethodRequest` ya da `myteampb.MethodRequest`

#### Değişken İsimleri vs. Tipleri
Derleyici her zaman bir değişkenin türünü bilir ve çoğu durumda okuyucu için de bir değişkenin nasıl kullanıldığına göre hangi türde olduğu açıktır. Bir değişkenin türünü yalnızca değeri aynı kapsamda iki kez göründüğünde açıklamak gerekir.

| Tekrarlayan İsim            | Daha Uygun İsim      |
|-----------------------------|----------------------|
| var numUsers int            | var users int        |
| var nameString string       | var name string      |
| var primaryProject *Project | var primary *Project |

Değer birden fazla biçimde görünüyorsa, bu durum `raw` ve `parsed` gibi ekstra bir sözcükle veya temel temsil ile açıklığa kavuşturulabilir:

```go
// İyi:
limitStr := r.FormValue("limit")
limit, err := strconv.Atoi(limitStr)
```

```go
// İyi:
limitRaw := r.FormValue("limit")
limit, err := strconv.Atoi(limitRaw)
```
#### Harici bağlam vs. yerel isimler
Çevresindeki bağlamdan bilgi içeren isimler genellikle fayda sağlamadan ekstra gürültü yaratır. Paket adı, metod adı, tür adı, fonksiyon adı, içe aktarma yolu ve hatta dosya adı, içindeki tüm adları otomatik olarak nitelendiren bir bağlam sağlayabilir.

```go
// Kötü:
// "ads/targeting/revenue/reporting" paketinde
type AdsTargetingRevenueReport struct{}

func (p *Project) ProjectName() string
```

```go
// İyi:
// "ads/targeting/revenue/reporting" paketinde
type Report struct{}

func (p *Project) Name() string
```

```go
// Kötü:
// "sqldb" paketinde
type DBConnection struct{}
```

```go
// İyi:
// "sqldb" paketinde
type Connection struct{}
```

```go
// Kötü:
// "ads/targeting" paketinde
func Process(in *pb.FooProto) *Report {
    adsTargetingID := in.GetAdsTargetingID()
}
```

```go
// İyi:
// "ads/targeting" paketinde
func Process(in *pb.FooProto) *Report {
id := in.GetAdsTargetingID()
}
```

Tekrarlama genellikle tek başına değil, sembolün kullanıcısı bağlamında değerlendirilmelidir. Örneğin, aşağıdaki kodda bazı durumlarda iyi olabilecek, ancak bağlam içinde gereksiz olan çok sayıda isim vardır:

```go
// Kötü:
func (db *DB) UserCount() (userCount int, err error) {
    var userCountInt64 int64
    if dbLoadError := db.LoadFromDatabase("count(distinct users)", &userCountInt64); dbLoadError != nil {
        return 0, fmt.Errorf("failed to load user count: %s", dbLoadError)
    }
    userCount = int(userCountInt64)
    return userCount, nil
}
```

Bunun yerine, bağlamdan veya kullanımdan açıkça anlaşılan isimlerle ilgili bilgiler genellikle atlanabilir:

```go
// İyi:
func (db *DB) UserCount() (int, error) {
    var count int64
    if err := db.Load("count(distinct users)", &count); err != nil {
        return 0, fmt.Errorf("failed to load user count: %s", err)
    }
    return int(count), nil
}
```

### Yorum
Yorumlarla ilgili kurallar _(nelerin yorumlanacağı, hangi stilin kullanılacağı, çalıştırılabilir örneklerin nasıl sağlanacağı vb. dahil)_, genel bir API'nin belgelerini okuma deneyimini desteklemeyi amaçlamaktadır. Daha fazla bilgi için [Effective Go](change) bölümüne bakınız.

En iyi uygulamalar _(Best Practices)_ belgesinin [dokümantasyon kuralları](change)na ilişkin bölümünde bu konu daha ayrıntılı olarak ele alınmaktadır.

**En İyi Uygulama** _(Best Practice)_: Belgelerin ve çalıştırılabilir örneklerin kullanışlı olup olmadığını ve beklediğiniz şekilde sunulup sunulmadığını görmek için geliştirme ve kod incelemesi sırasında [belge önizlemesi](change)ni kullanın.

**İpucu**: Godoc çok az özel biçimlendirme kullanır; listeler ve kod parçacıkları satır sarmayı önlemek için genellikle girintili olmalıdır. Girintileme dışında, genellikle süslemeden kaçınılmalıdır.

#### Yorum satırı uzunluğu
Yorumların dar ekranlarda bile kaynaktan okunabilir olmasını sağlayın.

Bir yorum çok uzun olduğunda, birden fazla tek satırlık yoruma sarılması önerilir. Mümkün olduğunda, 80 sütun genişliğinde bir terminalde iyi okunacak yorumlar hedefleyin, ancak bu kesin bir sınır değildir; Go'da yorumlar için sabit bir satır uzunluğu sınırı yoktur. Örneğin, standart kütüphane genellikle noktalama işaretlerine dayalı olarak bir yorumu kırmayı seçer, bu da bazen tek tek satırları 60-70 karakter işaretine daha yakın bırakır.

Yorumların uzunluğunun 80 karakteri aştığı pek çok mevcut kod vardır. Bu kılavuz, okunabilirlik incelemesinin bir parçası olarak bu tür bir kodu değiştirmek için bir gerekçe olarak kullanılmamalıdır _(bkz. [tutarlılık](change))_, ancak ekipler, diğer yeniden düzenlemelerin bir parçası olarak bu kılavuzu takip etmek için yorumları fırsatçı bir şekilde güncellemeye teşvik edilir. Bu kılavuzun birincil amacı, tüm Go okunabilirlik danışmanlarının, öneriler yapıldığında ve yapıldığında aynı öneriyi yapmasını sağlamaktır.

Yorum hakkında daha fazla bilgi için [The Go Blog'un dokümantasyon hakkındaki bu yazısı](https://blog.golang.org/godoc-documenting-go-code)na bakın.

```go
# Good:
// Bu bir yorum paragrafıdır.
// Godoc'ta satırın uzunluğunun bir önemi yoktur
// ama küçük ekranlarda kısa yorum satırlarının okunması daha kolaydır.
//
// Uzun URL'ler hakkında çok fazla endişelenmeyin.
// https://supercalifragilisticexpialidocious.example.com:8080/Animalia/Chordata/Mammalia/Rodentia/Geomyoidea/Geomyidae/
//
// Benzer olarak, çok fazla satır sonu nedeniyle garip hale
// gelen başka bilgileriniz varsa,
// muhakemenizi kullanın ve engellemek yerine
// yardımcı oluyorsa uzun bir satır ekleyin.
```

Küçük ekranlarda tekrar tekrar sarılacak yorumlardan kaçının, bu kötü bir okuyucu deneyimidir.

```go
# Kötü:
// Bu bir yorum paragrafıdır. Godoc'ta tek tek satırların uzunluğu önemli değildir;
// ancak sarma seçimi dar ekranlarda veya kod incelemesinde tırtıklı çizgilere neden olur,
// bu da can sıkıcı olabilir, özellikle de tekrar tekrar sarılacak bir yorum bloğu içindeyken.
//
// Uzun URL için çok fazla endişelenmeyin:
// https://supercalifragilisticexpialidocious.example.com:8080/Animalia/Chordata/Mammalia/Rodentia/Geomyoidea/Geomyidae/
```

