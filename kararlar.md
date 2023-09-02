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

#### Döküman yorumları

Tüm üst düzey dışa aktarılan adlar, açık olmayan davranış veya anlama sahip dışa aktarılmamış tür veya işlev bildirimlerinde olduğu gibi belge yorumlarına sahip olmalıdır. Bu yorumlar, açıklanan nesnenin adıyla başlayan [tam cümleler](change) olmalıdır. Daha doğal okunmasını sağlamak için adın önüne bir artikel _("a", "an", "the")_ getirilebilir.

```go
// İyi:
// A Request represents a request to run a command.
type Request struct { ...

// Encode writes the JSON encoding of req to w.
func Encode(w io.Writer, req *Request) { ...
```

Döküman yorumları [Godoc](https://pkg.go.dev/)'ta görünür ve IDE'ler tarafından ortaya çıkar ve bu nedenle paketi kullanan herkes için yazılmalıdır.

Bir dokümantasyon yorumu, aşağıdaki sembol veya bir yapıda görünüyorsa alan grubu için geçerlidir.

```go
// İyi:
// Options configure the group management service.
type Options struct {
    // General setup:
    Name  string
    Group *FooGroup

    // Dependencies:
    DB *sql.DB

    // Customization:
    LargeGroupThreshold int // optional; default: 10
    MinimumMembers      int // optional; default: 2
}
```

**En İyi Uygulama (Best Practice)**: Dışa aktarılmamış kod için doküman yorumlarınız varsa, dışa aktarılmış gibi aynı geleneği izleyin _(yani, yorumu dışa aktarılmamış adla başlatın)_. Bu, hem yorumlarda hem de kodda dışa aktarılmamış adı yeni dışa aktarılan adla değiştirerek daha sonra dışa aktarmayı kolaylaştırır.

### Yorum cümleleri

Tam bir cümle olan yorumlar büyük harfle yazılmalı ve standart İngilizce cümleler gibi noktalanmalıdır. _(Bir istisna olarak, aksi açıksa, bir cümleye büyük harfle yazılmamış bir tanımlayıcı adıyla başlamakta sakınca yoktur. Bu tür durumlar muhtemelen yalnızca bir paragrafın başında yapılmalıdır)_.

Cümle parçalarından oluşan yorumlarda noktalama veya büyük harf kullanımı için böyle bir gereklilik yoktur.

Dokümantasyon yorumları her zaman tam cümleler olmalıdır ve bu nedenle her zaman büyük harfle yazılmalı ve noktalanmalıdır. Basit satır sonu yorumları _(özellikle struct alanları için)_, alan adının konu olduğunu varsayan basit ifadeler olabilir.

````go
// İyi:
// A Server handles serving quotes from the collected works of Shakespeare.
type Server struct {
    // BaseDir points to the base directory under which Shakespeare's works are stored.
    //
    // The directory structure is expected to be the following:
    //   {BaseDir}/manifest.json
    //   {BaseDir}/{name}/{name}-part{number}.txt
    BaseDir string

    WelcomeMessage  string // displayed when user logs in
    ProtocolVersion string // checked against incoming requests
    PageLength      int    // lines per page when printing (optional; default: 20)
}
````

### Örnekler

Paketler kullanım amaçlarını açıkça belgelemelidir. [Çalıştırılabilir bir örnek](http://blog.golang.org/examples) sağlamaya çalışın; örnekler Godoc'ta görünür. Çalıştırılabilir örnekler test dosyasına aittir, üretim kaynak dosyasına değil. Bu örneğe bakın ([Godoc](https://pkg.go.dev/time#example-Duration), [kaynak](https://cs.opensource.google/go/go/+/HEAD:src/time/example_test.go)).

Çalıştırılabilir bir örnek sağlamak mümkün değilse, örnek kod kod yorumları içinde sağlanabilir. Yorumlardaki diğer kod ve komut satırı parçacıklarında olduğu gibi, standart biçimlendirme kurallarına uymalıdır.


#### Adlandırılmış Return parametreleri

Parametreleri adlandırırken, fonksiyon imzalarının Godoc'ta nasıl göründüğünü göz önünde bulundurun. Fonksiyonun adı ve return parametrelerinin türü genellikle yeterince açıktır.

```go
// İyi:
func (n *Node) Parent1() *Node
func (n *Node) Parent2() (*Node, error)
```

Bir fonksiyon aynı türden iki veya daha fazla parametre döndürüyorsa, isim eklemek yararlı olabilir.

```go
// İyi:
func (n *Node) Children() (left, right *Node, err error)
```

Çağrının yapıldığı alanda belirli return parametreleri üzerinde işlem yapması gerekiyorsa, bunları adlandırmak eylemin ne olduğunu önermeye yardımcı olabilir:

```go
// İyi:
// WithTimeout returns a context that will be canceled no later than d duration
// from now.
//
// The caller must arrange for the returned cancel function to be called when
// the context is no longer needed to prevent a resource leak.
func WithTimeout(parent Context, d time.Duration) (ctx Context, cancel func())
```

Yukarıdaki kodda `cancel`, çağıranın yapması gereken belirli bir eylemdir. Ancak, return parametreleri yalnızca `(Context, func())` olarak yazılsaydı, "cancel function" ile ne kastedildiği belirsiz olurdu.

Adlar gereksiz tekrarlar oluşturuyorsa adlandırılmış sonuç parametreleri kullanmayın.

```go
// Kötü:
func (n *Node) Parent1() (node *Node)
func (n *Node) Parent2() (node *Node, err error)
```

Fonksiyon içinde değişken bildirmekten kaçınmak için return _(sonuç)_ parametrelerine isim vermeyin. Bu uygulama, küçük uygulama kısalığı pahasına gereksiz API ayrıntılarına neden olur.

[Çıplak _(naked)_ geri dönüşler](https://tour.golang.org/basics/7) yalnızca küçük bir fonksiyonda kabul edilebilir. Orta büyüklükte bir fonksiyon olduğunda, döndürdüğünüz değerlerle ilgili açık olun. Benzer şekilde, sırf çıplak geri dönüşleri kullanmanıza olanak sağladığı için return parametrelerini adlandırmayın. [Netlik](change) her zaman fonksiyonunuzda birkaç satır tasarruf etmekten daha önemlidir.

Ertelenmiş bir kapanışta değerinin değiştirilmesi gerekiyorsa, bir sonuç parametresini adlandırmak her zaman kabul edilebilir.

> **İpucu**: Tipler, fonksiyon imzalarında genellikle isimlerden daha açık olabilir. [GoTip #38: Adlandırılmış Tipler Olarak Fonksiyonlar](change) bunu göstermektedir.
>
> Yukarıdaki [WithTimeout](https://pkg.go.dev/context#WithTimeout)'ta, gerçek kod sonuç parametre listesinde ham bir `func()` yerine bir [CancelFunc](https://pkg.go.dev/context#CancelFunc) kullanır ve belgelemek için çok az çaba gerektirir.

#### Paket yorumları

Paket yorumları, `package` cümlesinin hemen üzerinde yer almalı ve açıklama ile paket adı arasında boş satır bulunmamalıdır. Örnek:

```go
// İyi:
// Package math provides basic constants and mathematical functions.
//
// This package does not guarantee bit-identical results across architectures.
package math
```

Her paket için tek bir paket yorumu olmalıdır. Bir paket birden fazla dosyadan oluşuyorsa, dosyalardan tam olarak birinin paket yorumu olmalıdır.

`main` paketler için yorumlar biraz farklı bir biçime sahiptir; burada BUILD dosyasındaki `go_binary` kuralının adı paket adının yerini alır.

```go
// İyi:
// The seed_generator command is a utility that generates a Finch seed file
// from a set of JSON study configs.
package main
```

İkilinin adı tam olarak BUILD dosyasında yazıldığı şekilde olduğu sürece diğer yorum stillerinde sorun yoktur. İkili adı ilk kelime olduğunda, komut satırı çağrısının yazımıyla tam olarak eşleşmese bile büyük harfle yazılması gerekir.

```go
// İyi:
// Binary seed_generator ...
// Command seed_generator ...
// Program seed_generator ...
// The seed_generator command ...
// The seed_generator program ...
// Seed_generator ...
```

İpuçları:

- Örnek komut satırı çağrıları ve API kullanımı yararlı belgeler olabilir. Godoc biçimlendirmesi için kod içeren yorum satırlarını girintileyin.
- Belirgin bir birincil dosya yoksa veya paket yorumu olağanüstü uzunsa, doc yorumunu yalnızca yorum ve paket cümlesiyle `doc.go` adlı bir dosyaya koymak kabul edilebilir.
- Birden fazla tek satırlı yorum yerine çok satırlı yorumlar kullanılabilir. Bu öncelikle, örnek komut satırları `(ikili dosyalar için)` ve şablon örneklerinde olduğu gibi, belgelerin kaynak dosyadan kopyalayıp yapıştırmanın yararlı olabileceği bölümler içermesi durumunda yararlıdır.

```go
// İyi:
/*
The seed_generator command is a utility that generates a Finch seed file
from a set of JSON study configs.

    seed_generator *.json | base64 > finch-seed.base64
*/
package template
```

- Bakımcılara yönelik olan ve tüm dosya için geçerli olan yorumlar genellikle içe aktarma bildirimlerinden sonra yerleştirilir. Bunlar Godoc'ta gösterilmez ve paket yorumlarıyla ilgili yukarıdaki kurallara tabi değildir.

### İçe Aktarmalar (Imports)
#### İçe aktarma isimlendirmeleri (Import renamings)
İçe aktarmalar yalnızca diğer içe aktarmalarla isim çakışmasını önlemek için yeniden adlandırılmalıdır. _(Bunun bir sonucu olarak, [iyi paket adları](change) yeniden adlandırma gerektirmemelidir.)_ Bir ad çakışması durumunda, en yerel veya projeye özgü içe aktarımı yeniden adlandırmayı tercih edin. Paketler için yerel adlar _(takma adlar)_, alt çizgi ve büyük harf kullanımının yasaklanması da dahil olmak üzere [paket adlandırma kılavuzu](change)na uygun olmalıdır.

Oluşturulan protokol tampon paketleri, adlarından alt çizgileri kaldıracak şekilde yeniden adlandırılmalı ve takma adları bir pb son ekine sahip olmalıdır. Daha fazla bilgi için [proto ve stub en iyi uygulamaları](change) bölümüne bakın.

```go
// İyi:
import (
    fspb "path/to/package/foo_service_go_proto"
)
```

Hiçbir yararlı tanımlayıcı bilgi içermeyen paket adlarına sahip içe aktarmalar _(örn. v1 paketi)_ önceki yol bileşenini içerecek şekilde yeniden adlandırılmalıdır. Yeniden adlandırma, aynı paketi içe aktaran diğer yerel dosyalarla tutarlı olmalıdır ve sürüm numarasını içerebilir.

**Not**: Paketin [iyi paket adları](change)na uyacak şekilde yeniden adlandırılması tercih edilir, ancak bu genellikle satılan dizinlerdeki paketler için mümkün değildir.

```go
// İyi:
import (
    core "github.com/kubernetes/api/core/v1"
    meta "github.com/kubernetes/apimachinery/pkg/apis/meta/v1beta1"
)
```

Adı, kullanmak istediğiniz ortak bir yerel değişken adıyla çakışan bir paketi içe aktarmanız gerekiyorsa _(örn. `url`, `ssh`)_ ve paketi yeniden adlandırmak istiyorsanız, bunu yapmanın tercih edilen yolu `pkg` son ekidir _(örn. `urlpkg`)_. Bir paketi yerel bir değişkenle gölgelemenin mümkün olduğunu unutmayın; bu yeniden adlandırma yalnızca paketin böyle bir değişken kapsamdayken hala kullanılması gerekiyorsa gereklidir.

#### İçe aktarmaları Gruplama (Import Grouping)
Import'lar iki grup halinde düzenlenmelidir:

- Standart kütüphane paketleri

- Diğer (proje ve 3. parti) paketler

```go
// İyi:
package main

import (
    "fmt"
    "hash/adler32"
    "os"

    "github.com/dsnet/compress/flate"
    "golang.org/x/text/encoding"
    "google.golang.org/protobuf/proto"
    foopb "myproj/foo/proto/proto"
    _ "myproj/rpc/protocols/dial"
    _ "myproj/security/auth/authhooks"
)
```

Proje paketlerini birden fazla gruba ayırmak kabul edilebilir, örneğin yeniden adlandırılmış, yalnızca yan etkiler için içe aktarılmış veya başka bir özel içe aktarma grubu için ayrı bir grup istiyorsanız.

```go
// İyi:
package main

import (
    "fmt"
    "hash/adler32"
    "os"


    "github.com/dsnet/compress/flate"
    "golang.org/x/text/encoding"
    "google.golang.org/protobuf/proto"

    foopb "myproj/foo/proto/proto"

    _ "myproj/rpc/protocols/dial"
    _ "myproj/security/auth/authhooks"
)
```

**Not**: İsteğe bağlı grupların korunması - standart kütüphane ve Google içe aktarmaları arasındaki zorunlu ayrım için gerekli olanın ötesinde bölünme - [goimports](https://google.github.io/styleguide/go/golang.org/x/tools/cmd/goimports) aracı tarafından desteklenmemektedir. Ek içe aktarma alt gruplarının uygun bir durumda tutulması için hem yazarların hem de gözden geçirenlerin dikkati gerekir.

Aynı zamanda AppEngine uygulaması olan Google programlarının AppEngine içe aktarmaları için ayrı bir grubu olmalıdır.

Gofmt her grubu içe aktarma yoluna göre sıralamaya özen gösterir. Ancak, içe aktarmaları otomatik olarak gruplara ayırmaz. Popüler [goimports](https://google.github.io/styleguide/go/golang.org/x/tools/cmd/goimports) aracı Gofmt ve içe aktarma yönetimini birleştirerek içe aktarmaları yukarıdaki karara göre gruplara ayırır. [goimports](https://google.github.io/styleguide/go/golang.org/x/tools/cmd/goimports)'un içe aktarma düzenlemesini tamamen yönetmesine izin verilebilir, ancak bir dosya revize edilirken içe aktarma listesi dahili olarak tutarlı kalmalıdır.

#### "blank" Import (`import _`)
Yalnızca yan etkileri için içe aktarılan paketler _(`import _ "package"` sözdizimi kullanılarak)_ yalnızca bir ana pakette veya bunları gerektiren testlerde içe aktarılabilir.

Bu tür paketlerin bazı örnekleri şunlardır:

- [time/tzdata](https://pkg.go.dev/time/tzdata)
- görüntü işleme kodunda [image/jpeg](https://pkg.go.dev/image/jpeg)

Kütüphane dolaylı olarak bunlara bağlı olsa bile, kütüphane paketlerinde boş _(blank)_ içe aktarmalardan kaçının. Yan etki içe aktarımlarını ana paketle sınırlamak, bağımlılıkları kontrol etmeye yardımcı olur ve çakışma veya boşa derleme maliyeti olmadan farklı bir içe aktarıma dayanan testler yazmayı mümkün kılar.

Aşağıda belirtilenler bu kuralın tek istisnasıdır:
- [Nogo statik denetleyicisi](https://github.com/bazelbuild/rules_go/blob/master/go/nogo.rst)ndeki izin verilmeyen içe aktarma kontrolünü atlamak için boş bir içe aktarma kullanabilirsiniz.
- `//go:embed` derleyici yönergesini kullanan bir kaynak dosyada [embed](https://pkg.go.dev/embed) paketinin boş bir içe aktarımını kullanabilirsiniz.

**İpucu**: Üretimde dolaylı olarak bir yan etki içe aktarımına bağlı olan bir kütüphane paketi oluşturursanız, kullanım amacını belgeleyin.

#### Nokta Import (`import .`)
`import .` formu, başka bir paketten dışa aktarılan tanımlayıcıların herhangi bir niteleme olmaksızın mevcut pakete getirilmesini sağlayan bir dil özelliğidir. Daha fazlası için [dil spesifikasyonu](https://go.dev/ref/spec#Import_declarations)na bakın.

Bu özelliği Google kod tabanında **kullanmayın**; işlevselliğin nereden geldiğini söylemeyi zorlaştırır.

```go
// Kötü:
package foo_test

import (
    "bar/testutil" // ayrıca "foo" paketini de import eder
    . "foo"
)

var myThing = Bar() // "Bar", "foo" paketinde tanımlanmıştır; niteleme gerekmez
```

```go
// İyi:
package foo_test

import (
    "bar/testutil" // ayrıca "foo" paketini de import eder
    "foo"
)

var myThing = foo.Bar()
```


### Hatalar (Errors)
#### Döndürülen Hatalar (REturning Errors)
Bir fonksiyonun başarısız olabileceğini belirtmek için `error` kullanın. Geleneksel olarak, `error` son return _(sonuç)_ parametresidir.

```go
// İyi:
func Good() error { /* ... */ }
```

`nil` hata döndürmek, aksi takdirde başarısız olabilecek bir işlemin başarılı olduğunu belirtmek için kullanılan deyimsel bir yoldur. Bir fonksiyon bir hata döndürürse, aksi açıkça belirtilmedikçe, çağıranlar tüm hata dışı dönüş değerlerini belirtilmemiş olarak kabul etmelidir. Genellikle, hata olmayan geri dönüş değerleri sıfır değerleridir, ancak bu varsayılamaz.

```go
// İyi:
func GoodLookup() (*Result, error) {
    // ...
    if err != nil {
        return nil, err
    }
    return res, nil
}
```

Hata döndüren dışa aktarılmış fonksiyonlar, hataları `error` türünü kullanarak döndürmelidir. Somut hata türleri ince hatalara karşı hassastır: somut bir `nil` işaretçisi bir arayüze _(interface)_ sarılabilir ve böylece `nil` olmayan bir değer haline gelebilir _(konuyla ilgili [Go SSS yazısı](https://golang.org/doc/faq#nil_error)na bakın)_.

```go
// Kötü:
func Bad() *os.PathError { /*...*/ }
```

**İpucu**: Bir `context.Context` argümanı alan bir fonksiyon genellikle bir hata döndürmelidir, böylece çağrı yapan kod fonksiyon çalışırken bağlamın iptal edilip edilmediğini belirleyebilir.

#### Hata Dizeleri (Error Strings)
Hata dizeleri _(dışa aktarılan bir ad, özel bir isim veya kısaltma ile başlamadığı sürece)_ büyük harfle yazılmamalı ve noktalama işaretleriyle bitirilmemelidir. Bunun nedeni, hata dizelerinin kullanıcıya yazdırılmadan önce genellikle başka bir bağlam içinde görünmesidir.

```go
// Kötü:
err := fmt.Errorf("Something bad happened.")
```

```go
// İyi:
err := fmt.Errorf("something bad happened")
```

Öte yandan, tam görüntülenen mesajın _(günlük kaydı, test hatası, API yanıtı veya diğer kullanıcı arayüzü)_ stili değişir, ancak genellikle büyük harfle yazılmalıdır.

```go
// İyi:
log.Infof("Operation aborted: %v", err)
log.Errorf("Operation aborted: %v", err)
t.Errorf("Op(%q) failed unexpectedly; err=%v", args, err)
```

#### Hataları İşleme (Handle Errors)
Bir hatayla karşılaşan kod, hatanın nasıl ele alınacağı konusunda bilinçli bir seçim yapmalıdır. Hataların `_` değişkenleri kullanılarak atılması genellikle uygun değildir. Bir fonksiyon hata döndürürse, aşağıdakilerden birini yapın:

- Hatayı hemen ele alın ve giderin.
- Hatayı çağırana geri döndürün.
- İstisnai durumlarda `log.Fatal` veya _(kesinlikle gerekliyse)_ `panic` çağrısı yapın.
**Not**: `log.Fatalf` standart kütüphane günlüğü değildir. Bakınız [#logging].

Bir hatayı görmezden gelmenin veya atmanın uygun olduğu nadir durumlarda _(örneğin, asla başarısız olmayacağı belgelenen [(*bytes.Buffer).Write](https://pkg.go.dev/bytes#Buffer.Write) çağrısı)_, eşlik eden bir açıklama bunun neden güvenli olduğunu açıklamalıdır.

```go
// İyi:
var b *bytes.Buffer

n, _ := b.Write(p) // asla nil olmayan bir hata döndürmez
```

Hata işleme ile ilgili daha fazla tartışma ve örnek için bkz, [Efektif Go](http://golang.org/doc/effective_go.html#errors) ve [en iyi uygulamalar](change).

#### Bant içi hatalar

C ve benzeri dillerde, fonksiyonların hataları veya eksik sonuçları bildirmek için -1, null veya boş dize gibi değerler döndürmesi yaygındır. Bu, bant içi hata işleme olarak bilinir.

```go
// Kötü:
// Lookup, anahtar için değeri veya anahtar için eşleme yoksa -1 değerini döndürür.
func Lookup(key string) int
```

Bant içi hata değerinin kontrol edilmemesi hatalara yol açabilir ve hataları yanlış fonksiyona atfedebilir.

```go
// Kötü:
// Aşağıdaki satır, giriş değeri için Parse işleminin başarısız olduğuna dair bir hata döndürür
// oysa hata, missingKey için bir eşleme olmamasıydı.
return Parse(Lookup(missingKey))
```

Go'nun çoklu geri dönüş değerlerini desteklemesi daha iyi bir çözüm sunar ([Go'nun çoklu geri dönüşlerle ilgili etkin bölümü](http://golang.org/doc/effective_go.html#multiple-returns)ne bakın). İstemcilerin bant içi bir hata değerini kontrol etmesini gerektirmek yerine, bir fonksiyon diğer dönüş değerlerinin geçerli olup olmadığını belirtmek için ek bir değer döndürmelidir. Bu geri dönüş değeri bir hata veya açıklama gerekmediğinde bir boolean olabilir ve son geri dönüş değeri olmalıdır.

```go
// İyi:
// Lookup, anahtar için değeri veya anahtar için eşleme yoksa ok=false değerini döndürür.
func Lookup(key string) (value string, ok bool)
```

Bu API, `Lookup(key)`'in 2 çıktısı olduğundan, çağıranın derleme zamanı hatasına neden olan `Parse(Lookup(key))`'i yanlış yazmasını önler.

Hataların bu şekilde döndürülmesi, daha sağlam ve açık hata işlemeyi teşvik eder:

```go
// İyi:
value, ok := Lookup(key)
if !ok {
    return fmt.Errorf("no value for %q", key)
}
return Parse(value)
```

`strings` paketinde olduğu gibi bazı standart kütüphane fonksiyonları bant içi hata değerleri döndürür. Bu, programcının daha fazla özen göstermesini gerektirme pahasına dize manipülasyon kodunu büyük ölçüde basitleştirir. Genel olarak, Google kod tabanındaki Go kodu hatalar için ek değerler döndürmelidir.

#### Hata Akışını Girintile (Indent Error Flow)
Kodunuzun geri kalanıyla devam etmeden önce hataları ele alın. Bu, okuyucunun normal yolu hızlı bir şekilde bulmasını sağlayarak kodun okunabilirliğini artırır. Aynı mantık, bir koşulu test eden ve ardından bir terminal koşuluyla _(örn. `return`, `panic`, `log.Fatal`)_ biten herhangi bir blok için de geçerlidir.

Son koşul karşılanmazsa çalışan kod, `if` bloğundan sonra görünmeli ve bir `else` cümlesinde girintili olmamalıdır.

```go
// İyi:
if err != nil {
    // hata işleme
    return // ya da continue, vb.
}
// normal kod
```

```go
// Kötü:
if err != nil {
    // hata işleme
} else {
    // girinti nedeniyle anormal görünen normal kod
}
```

> **İpucu**: Bir değişkeni birkaç satırdan fazla kod için kullanıyorsanız, genellikle `if`-with-initializer stilini kullanmaya değmez. Bu durumlarda, bildirimi dışarı taşımak ve standart bir `if` deyimi kullanmak genellikle daha iyidir:
> ```go
> // İyi:
> x, err := f()
> if err != nil {
>   // hata işleme
> return
> }
> // x'i kullanan birçok kod daha
> ```
> 
> ```go
> // Kötü:
> if x, err := f(); err != nil {
>   // hata işleme
>   return
> } else {
>   // x'i kullanan birçok kod daha
> }
> ```

## Dil
### Değişmez Biçimlendirme (Literal Formatting)
Go, tek bir ifadede derinlemesine iç içe geçmiş, karmaşık değerleri ifade etmenin mümkün olduğu son derece güçlü bir [bileşik değişmez sözdizimi](https://golang.org/ref/spec#Composite_literals)ne sahiptir. Mümkün olduğunda, değerleri alan alan oluşturmak yerine bu değişmez sözdizimi kullanılmalıdır. Değişmezler için `gofmt` biçimlendirmesi genellikle oldukça iyidir, ancak bu değişmezleri okunabilir ve sürdürülebilir tutmak için bazı ek kurallar vardır.

#### Alan adları
Struct değişmezleri genellikle geçerli paketin dışında tanımlanan türler için **alan adları**nı belirtmelidir.

- Diğer paketlerdeki türler için alan adlarını ekleyin.
  ```go
  // İyi:
  good := otherpkg.Type{A: 42}
  ```
  Bir struct içindeki alanların konumu ve alanların tam kümesi _(alan adları atlandığında her ikisinin de doğru olması gerekir)_ genellikle bir struct'ın genel API'sinin bir parçası olarak kabul edilmez; alan adının belirtilmesi gereksiz bağlantıdan kaçınmak için gereklidir.
  ```go
  // Kötü:
  // https://pkg.go.dev/encoding/csv#Reader
  r := csv.Reader{',', '#', 4, false, false, false, false}
  ```
  Bileşimi ve sıralamasının sabit olduğu belgelenen küçük, basit yapılarda alan adları atlanabilir.
  ```go
  // İyi:
  okay := image.Point{42, 54}
  also := image.Point{X: 42, Y: 54}
  ```
- Paket-yerel türler için alan adları isteğe bağlıdır.
  ```go
  // İyi:
  okay := Type{42}
  also := internalType{4, 2}
  ```
  Kodu daha anlaşılır hale getiriyorsa alan adları yine de kullanılmalıdır ve bunu yapmak çok yaygındır. Örneğin, çok sayıda alana sahip bir struct neredeyse her zaman alan adlarıyla başlatılmalıdır.
  ```go
  // İyi:
  okay := StructWithLotsOfFields{
    field1: 1,
    field2: "two",
    field3: 3.14,
    field4: true,
  }
  ```
#### Eşleşen Parantezler
Bir parantez çiftinin kapanış yarısı her zaman açılış paranteziyle aynı miktarda girintiye sahip bir satırda görünmelidir. Tek satırlı değişmezler mutlaka bu özelliğe sahiptir. Değişmez birden fazla satıra yayıldığında, bu özelliğin korunması, değişmezler için ayraç eşleştirmesini, fonksiyonlar ve `if` deyimleri gibi yaygın Go sözdizimsel yapıları için ayraç eşleştirmesiyle aynı tutar.

Bu alandaki en yaygın hata, çok satırlı bir struct literal içindeki bir değerle kapanış parantezini aynı satıra koymaktır. Bu durumlarda, satır virgülle bitmeli ve kapama ayracı bir sonraki satırda görünmelidir.

```go
// İyi:
good := []*Type{{Key: "value"}}
```

```go
// İyi:
good := []*Type{
    {Key: "multi"},
    {Key: "line"},
}
```

```go
// Kötü:
bad := []*Type{
    {Key: "multi"},
    {Key: "line"}}
```

```go
// Kötü:
bad := []*Type{
    {
        Key: "value"},
}
```

#### Gruplanmış (Kucaklanmış) Parantezler
Dilim ve dizi değişmezleri için parantezler arasında boşluk bırakmaya (diğer adıyla "kucaklamaya") yalnızca aşağıdakilerin her ikisi de doğru olduğunda izin verilir.

- [Girinti eşleşmeleri](change)
- İç değerler de değişmezler veya proto oluşturuculardır (yani değişken veya başka bir ifade değildir)

```go
// İyi:
good := []*Type{
    { // Gruplanmamış
        Field: "value",
    },
    {
        Field: "value",
    },
}
```

```go
// İyi:
good := []*Type{{ // Düzgün gruplanmış
    Field: "value",
}, {
    Field: "value",
}}
```

```go
// İyi:
good := []*Type{
    first, // Gruplanamamaz
    {Field: "second"},
}
```

```go
// İyi:
okay := []*pb.Type{pb.Type_builder{
    Field: "first", // Proto Builders dikey alandan tasarruf için gruplanabilir
}.Build(), pb.Type_builder{
    Field: "second",
}.Build()}
```

```go
// Kötü:
bad := []*Type{
    first,
    {
        Field: "second",
    }}
```

#### Tekrarlanan tür adları
Tekrarlanan tür adları slice _(dilim)_ ve map _(eşleme)_ değişmezlerinden çıkarılabilir. Bu, karmaşayı azaltmada yardımcı olabilir. Tür adlarını açıkça tekrarlamak için makul bir durum, projenizde yaygın olmayan karmaşık bir türle uğraşırken, tekrarlanan tür adlarının birbirinden uzak satırlarda olması ve okuyucuya bağlamı hatırlatmasıdır.

```go
// İyi:
good := []*Type{
    {A: 42},
    {A: 43},
}
```
```go
// Kötü:
repetitive := []*Type{
    &Type{A: 42},
    &Type{A: 43},
}
```
```go
// İyi:
good := map[Type1]*Type2{
    {A: 1}: {B: 2},
    {A: 3}: {B: 4},
}
```
```go
// Kötü:
repetitive := map[Type1]*Type2{
    Type1{A: 1}: &Type2{B: 2},
    Type1{A: 3}: &Type2{B: 4},
}
```

**İpucu**: Yapı değişmezlerinde tekrar eden tür adlarını kaldırmak istiyorsanız, `gofmt -s` komutunu çalıştırabilirsiniz.

#### Sıfır değerli alanlar
Sonuç olarak netlik kaybolmadığında, [sıfır değerli](https://golang.org/ref/spec#The_zero_value) alanlar struct değişmezlerinden çıkarılabilir.

İyi tasarlanmış API'ler genellikle daha iyi okunabilirlik için sıfır değerli yapı kullanır. Örneğin, aşağıdaki yapıdan üç sıfır değerli alanın çıkarılması, dikkati belirtilen tek seçeneğe çeker.

```go
// Kötü:
import (
  "github.com/golang/leveldb"
  "github.com/golang/leveldb/db"
)

ldb := leveldb.Open("/my/table", &db.Options{
    BlockSize: 1<<16,
    ErrorIfDBExists: true,

    // These fields all have their zero values.
    BlockRestartInterval: 0,
    Comparer: nil,
    Compression: nil,
    FileSystem: nil,
    FilterPolicy: nil,
    MaxOpenFiles: 0,
    WriteBufferSize: 0,
    VerifyChecksums: false,
})
```
```go
// İyi:
import (
  "github.com/golang/leveldb"
  "github.com/golang/leveldb/db"
)

ldb := leveldb.Open("/my/table", &db.Options{
    BlockSize: 1<<16,
    ErrorIfDBExists: true,
})
```

Tablo güdümlü testlerdeki yapılar, özellikle test yapısı önemsiz olmadığında, genellikle [açık alan adları](change)ndan yararlanır. Bu, söz konusu alanlar test senaryosuyla ilgili olmadığında yazarın sıfır değerli alanları tamamen atlamasına olanak tanır. Örneğin, başarılı test senaryoları hatayla ilgili veya başarısızlıkla ilgili alanları atlamalıdır. Sıfır değerinin test senaryosunu anlamak için gerekli olduğu durumlarda, örneğin sıfır veya `nil` girdilerin test edilmesi gibi, alan adları belirtilmelidir.

#### Özlülük

```go
tests := []struct {
    input      string
    wantPieces []string
    wantErr    error
}{
    {
        input:      "1.2.3.4",
        wantPieces: []string{"1", "2", "3", "4"},
    },
    {
        input:   "hostname",
        wantErr: ErrBadHostname,
    },
}
```

#### Netlik

```go
tests := []struct {
    input    string
    wantIPv4 bool
    wantIPv6 bool
    wantErr  bool
}{
    {
        input:    "1.2.3.4",
        wantIPv4: true,
        wantIPv6: false,
    },
    {
        input:    "1:2::3:4",
        wantIPv4: false,
        wantIPv6: true,
    },
    {
        input:    "hostname",
        wantIPv4: false,
        wantIPv6: false,
        wantErr:  true,
    },
}
```

### Nil Dilimler (Slices)

Çoğu amaç için, `nil` ile boş dilim arasında işlevsel bir fark yoktur. `len` ve `cap` gibi yerleşik fonksiyonlar `nil` dilimlerinde beklendiği gibi davranır.

```go
// İyi:
import "fmt"

var s []int         // nil

fmt.Println(s)      // []
fmt.Println(len(s)) // 0
fmt.Println(cap(s)) // 0
for range s {...}   // no-op

s = append(s, 42)
fmt.Println(s)      // [42]
```

Boş bir dilimi yerel değişken olarak bildirirseniz _(özellikle bir geri dönüş değerinin kaynağı olabilecekse)_, çağıranların hata riskini azaltmak için `nil` tanımlamayı tercih edin.

```go
// İyi:
var t []string
```

```go
// Kötü:
t := []string{}
```

İstemcilerini `nil` ve boş dilim arasında ayrım yapmaya zorlayan API'ler oluşturmayın.

```go
// İyi:
// Ping hedefleri pingler.
// Returns başarı ile sonuçlanan sunucuları döndürür.
func Ping(hosts []string) ([]string, error) { ... }
```

```go

// Kötü:
// Ping hedefleri pinler ve başarılı olan hostları döndürür
// Giriş boş ise boş gelebilir.
// nil bir sistem hatası oluştuğunu belirtir.
func Ping(hosts []string) []string { ... }
```

Arayüzleri tasarlarken, `nil` dilim ile `nil` olmayan, sıfır uzunluklu dilim arasında bir ayrım yapmaktan kaçının, çünkü bu ince programlama hatalarına yol açabilir. Bu genellikle `== nil` yerine boşluğu kontrol etmek için `len` kullanılarak gerçekleştirilir.

Bu uygulama hem `nil` hem de sıfır uzunluklu dilimleri "boş" olarak kabul eder:

```go
// İyi:
// describeInts, s boş olmadığı sürece, verilen önek ile s'yi tanımlar.
func describeInts(prefix string, s []int) {
    if len(s) == 0 {
        return
    }
    fmt.Println(prefix, s)
}
```

API'nin bir parçası olarak ayrıma güvenmek yerine:

```go
// Kötü:
func maybeInts() []int { /* ... */ }

// describeInts s'yi verilen önek ile tanımlar; tamamen atlamak için nil geçer.
func describeInts(prefix string, s []int) {
	// Bu fonksiyonun davranışı, maybeInts() fonksiyonunun 'boş' durumlarda (nil veya []int{})
    // ne döndürdüğüne bağlı olarak istemeden değişir
  if s == nil {
    return
  }
  fmt.Println(prefix, s)
}

describeInts("Here are some ints:", maybeInts())
```

Daha fazla tartışma için [bant içi hatalar](change)a bakın.

### Girinti karışıklığı
Satırın geri kalanını girintili bir kod bloğu ile hizalayacaksa satır sonu eklemekten kaçının. Bu kaçınılmazsa, bloktaki kodu sarılmış satırdan ayırmak için bir boşluk bırakın.

```go
// Kötü:
if longCondition1 && longCondition2 &&
    // 3. ve 4. koşullar, if içindeki kodla aynı girintiye sahiptir.
    longCondition3 && longCondition4 {
    log.Info("all conditions met")
}
```

Belirli yönergeler ve örnekler için aşağıdaki bölümlere bakın:
- [Fonksiyon biçimlendirme](change)
- [Koşullar ve döngüler](change)
- [Değişmez Biçimlendirme (Literal Formatting)](change)