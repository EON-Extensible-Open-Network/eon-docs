# EON — Proje Planı

> **EON — Extensible Open Network**
> *An open ecosystem for everyone.*
>
> **Durum:** Taslak v0.17 · **Son güncelleme:** 26 Kasım 2026
>
> **Doküman kuralları**
> - Madde numarası kalıcı kimliktir, **konumsal değildir**. Bir kararın içeriği değişirse madde güncellenir, numarası değişmez. Yeni kararlar sıradaki boş numarayı alır (v0.2: 29–41, v0.3: 42), bölüm içindeki fiziksel sırası okuma kolaylığına göre seçilir. **v0.5 yeni madde eklemiyor**; açık olan maddeleri kapatıyor (madde 1'in torrent yarısı, madde 34'ün kanonik serileştirmesi) ve Faz 0 çıktılarını işaretliyor. **v0.6 de yeni madde eklemiyor**; madde 2'nin zamanlama yarısını değiştiriyor (arayüz `v1` yerine alfa hattında, `v0.14`'te geldi) ve sekiz kalemin durumunu güncelliyor. **v0.7 de yeni madde eklemiyor**; madde 2'nin oynatıcı stratejisini değiştiriyor (görüntü `v0.16`'dan itibaren uygulama penceresinin içinde). **v0.8 bir madde ekliyor (43)**: EON Office ürün ailesi, EON Stream `v1`'den önce başlıyor. **v0.9 madde 44'ü ekliyor** (EON Download Manager). **v0.10 madde 45'i ekliyor**: dosya, ses ve akış ayrı ürünler; her hizmetin kendi deposu, core'u ve uygulaması. **v0.11 madde 46'yı ekliyor**: bütün EON için tek, anonimlik öncelikli hesap (EON Account). **v0.12 yeni madde eklemiyor**; madde 44b'yi değiştiriyor (sayfanın oynattığı video ve ses). **v0.13 madde 47'yi ekliyor**: EON DNS. **v0.14 yeni madde eklemiyor**; madde 2'nin platform yarısını değiştiriyor (EON Stream her masaüstü platformunda yayınlanıyor, Android telefon ve TV ertelenmişlikten çıkıyor). **v0.15 madde 48'i ekliyor**: EON Converter. **v0.16 madde 49'u ekliyor**: EON Zip. **v0.17 madde 50'yi ekliyor**: EON Browser.
> - Her maddenin bir durum etiketi vardır: **[K] Kararlaştı** · **[A] Araştırılacak** · **[E] Ertelendi**. Etiketi [A] olan bir madde karar değil, yönlendirilmiş sorudur.
> - Hukuk ve marka maddeleri (11–14, 29–32) **hukuki tavsiye değildir**; madde 13'teki avukat incelemesi tamamlanana kadar tamamı taslaktır ve tek başına dayanak yapılmaz.

---

## Vizyon

**Tek çatı, ayrı ürünler; her hizmetin kendi deposu, kendi core'u, kendi uygulaması (madde 45). Yalnızca EON Stream ile EON Edu bir core'u paylaşır.**

> **Kayıt (v0.10):** v0.8 ve v0.9'da bu satır "Tek çatı; EON Stream ile EON Edu için tek core, EON Office için kendi core'u" idi. Madde 45 ile ayrılık istisna değil kural oldu: dosya ve ses de EON Stream'in modülü değil, kendi ürünü.
>
> **Kayıt (v0.8):** Bu satır v0.7'ye kadar "Tek çatı, tek core, iki ürün" idi. EON Stream ve EON Edu için doğru olmaya devam ediyor; EON Office, aynı core'u paylaşmayan üçüncü bir ürün ailesi olarak eklendi.

**EON** (*Extensible Open Network*) bir ürün değil, **çatıdır**. Sloganı projenin ne olduğunu tanımlar: *An open ecosystem for everyone.* Çatı bilerek ürün adından geniş tutuldu — ileride kendi kernel'i ya da kendi Linux dağıtımı gelirse bunlar yeniden markalaşmaya gerek kalmadan EON altına oturur (bkz. madde 32 ve 42).

Çatının altındaki ürünler bugün:

- **EON Stream:** Açık kaynak, kâr amacı gütmeyen, modüler bir **akış (video)** uygulaması. Stremio eklenti protokolüyle uyumlu; community tabanlı eklenti sistemiyle genişler. EON Edu'nun da core'udur. *(v0.10'a kadar "video, dosya, müzik" idi; dosya ve ses madde 45 ile ayrı ürün oldu.)*
- **EON Edu:** Aynı core'un, aynı depodan derlenen kurumsal ürünü. Okullar ve uzaktan eğitim için sıkı kısıtlanmış; üzerine yalnızca Edu'ya özel modüller biner.
- **EON Download Manager:** Dosya hizmeti. Internet Download Manager'ın sevilen her şeyini yapan, bireylere her zaman ücretsiz indirme yöneticisi; kendi motoru, kendi sözleşmeleri, kendi tarayıcı eklentisi (madde 44).
- **Ses ürünü (adı [A]):** Müzik ve ses için ayrı bir ürün. **[E]** — kapsamı ve adı yazılmadan depo açılmaz (madde 45a, 43d).
- **EON Office:** Bağımsız uygulamalardan oluşan bir üretkenlik ve iş yazılımı ailesi (EON Notes, EON Writer, EON Sheets…). Uygulamalar tek bir pakete gömülmez; her biri ayrı depo, ayrı kurulum, ayrı sürüm. Ortak altyapı `eon-office-core`'dadır ve EON Stream'in core'una bağlı değildir (madde 43).
- **EON DNS:** Şifreli DNS ve filtreleme; kişinin seçtiği çözümleyiciye, cihazda denetlenen ve günlüğü cihazda kalan biçimde. Ardından kişinin getirdiği bir uca tünel (madde 47).
- **EON Converter:** Her dosyayı dönüşebileceği her biçime çeviren dönüştürücü. Çekirdeği bir kütüphanedir; EON Download Manager da aynı çekirdekle indirmeyi istenen biçimde teslim eder (madde 48).
- **EON Zip:** Arşivleyici. 7-Zip ve WinRAR'ın yapıp onun yapamadığı hiçbir şey kalmaması hedeftir; tek istisna RAR oluşturmaktır, çünkü biçim RARLAB'ındır (madde 49).
- **EON Browser:** Web tarayıcısı. Chromium'a ve Windows'un motoruna değil, Rust ile yazılmış açık kaynaklı Servo motoruna dayanır; reklam ve izleyici engelleme yerleşiktir, telemetri ve yapay zekâ yoktur, ilk açılışta tek bir istek yapmaz. Windows, Linux ve macOS (madde 50).
- **EON Account:** Bütün EON ürünleri için tek hesap. Kullanıcı adı ve parola yeter; e-posta ve telefon isteğe bağlıdır. Sunucu parolayı da kullanıcının izleme ilerlemesini de göremez (madde 46).

### Neden ortak çatı, ayrı ürün adı [K]

İki soruya birden cevap verilmesi gerekiyordu ve tek bir isim ikisini birden çözemiyordu.

**Neden ortak çatı:** Core aynı. İki ürün aynı paket formatını, aynı eklenti API'sini, aynı görüntüleyiciyi ve aynı materyal paketi motorunu kullanıyor. Öğrenci okulda indirdiği materyali evde açıyor (madde 28); öğretmen tek bir creator öğreniyor; tek dokümantasyon, tek topluluk, tek güvenlik süreci. Tamamen ilgisiz iki marka, tek ürün olan şeyi iki ürün gibi bakmak zorunda bırakırdı.

**Neden ayrı ürün adı:** Kamu kurumu EON Edu'yu değerlendirirken arama yapacak. Ortak çatı + ayrı ürün adı, o aramada kurumun **EON Edu**'yu kendi adıyla bulmasını sağlıyor; açık görüntüleyici ile bağ çatı düzeyinde kalıyor, ürün düzeyinde değil.

**Karar:** Çatı **EON** ortak; ürün adları **EON Stream** ve **EON Edu** ayrı. Teknik ayrım derleme profili düzeyinde (madde 22), iletişim ayrımı marka duruşu düzeyinde (madde 32) yapılır.

> **Kayıt (v0.3):** v0.1 ve v0.2'de karar "tek isim, iki derleme" idi ve bu planda itibar bağı bilinen bir bedel olarak kabul edilmişti. Çatı markası bu bedelin büyük kısmını ortadan kaldırıyor: artık kurumun gördüğü ad, tüketici ürününün adı değil. Madde 22 ve 32 yine gerekli — ama artık daha az iş yapıyorlar.

### Ana amaç Edu, öncelik EON Stream [K]

Projenin varlık sebebi Edu'dur. Geliştirme önceliği yine de EON Stream'dir, çünkü **EON Stream core'un ta kendisidir**: Edu'yu önce yazmaya kalkmak, aynı core'u daha az kullanıcı, daha az geri bildirim ve daha yavaş döngüyle yazmak olur. EON Stream çalıştığı anda Edu'nun büyük kısmı zaten yazılmıştır.

Dürüst kayıt: **Edu "sadece flag" değildir.** Flag'ler bir şeyleri *çıkarır* (açık marketplace, URL ile eklenti, açık torrent — madde 22), Edu modülleri ise bir şeyleri *ekler* (roller, kurum sunucusu, OIDC, okul araçları — madde 17–21, 25, 27). Formül:

> **Edu = ortak core + kısıtlama flag'leri + Edu modülleri**

Planlama bu üç kalemi ayrı ayrı sayar; yoksa Edu'nun iş yükü sistematik olarak eksik tahmin edilir.

### Öncelik sırası

~~Video → Dosya → Müzik (müzik şimdilik askıda, madde 41).~~

> **Kayıt (v0.10):** Bu sıra, video, dosya ve müziğin EON Stream'in içinde modül olduğu plan içindi. Madde 45 ile her biri ayrı ürün: EON Stream'in kapsamı videodur, dosya EON Download Manager'dır, ses ayrı ve ertelenmiş bir üründür. Ürünler arasındaki sıra proje sahibinin önceliğidir ve durum tablosunda (Açık konular) tutulur.

### Kapsam notu: sözleşme-önce, ama v0 kararsız [K]

Sözleşmeler (eklenti API'si, manifest şeması, modül sistemi, paket formatı, revizyon kaydı, politika sürümleme) baştan **genişletilebilir olacak kadar** tasarlanır — *eksiksiz* olacak kadar değil. Çalışan bir oynatıcı görmeden bunları eksiksiz tasarlamak tahmine dayalı tasarımdır. Bu yüzden:

- Tüm sözleşmeler `v0` ile başlar ve `v0` açıkça **kararsız** ilan edilir: kırıcı değişiklik yapılabilir, geriye uyum sözü verilmez.
- `v1` yayınlandığı anda semver garantisi başlar (madde 5).
- Sözleşme dosyaları koddan önce yazılır ve şema dosyası olarak depoda durur; tartışma PR üzerinden yürür.

### v1 tanımı (definition of done) [K]

v1, şu listenin **tamamı** bittiğinde ve fazlası yapılmadığında yayınlanır:

1. Masaüstü (Windows + Linux) uygulama; tek dosya kurulum, imzalı, otomatik güncellemeli.
2. Video modülü: Stremio protokolü uyumlu uzak eklentilerden katalog/meta/stream çözümleme, mpv ile oynatma, altyazı, kaldığı yerden devam.
3. URL ile eklenti ekleme, listeleme, kaldırma, sıralama.
4. Torrent akışı: sıralı indirme + yerel HTTP sunucu → oynatıcı.
5. Modül yöneticisi: birinci taraf modülleri kurma/kaldırma/güncelleme, imza doğrulama.
6. Bildirimsel tema desteği (kod yok — madde 4).
7. Uyumluluk test seti CI'da yeşil (madde 1).
8. Türkçe + İngilizce arayüz; temel erişilebilirlik ölçütleri karşılanmış (madde 35).

**v1'de olmayanlar:** marketplace, hesaplar, creator'lar, materyal paketleri, müzik, mobil, kod çalıştıran eklentiler, Edu.

---

## A. Temel mimari (1–5)

**1. Protokol katmanı: kendimiz yazıyoruz — (b). [K — v0.4'te karara bağlandı]**

Karar ölçütü şuydu: (b) yoluyla popüler eklentiler uçtan uca çözümlenebiliyorsa (b) seçilir. **Çözümlendi**, ve karar (b) oldu: protokol istemcisi `eon-stream-core` içinde bize ait koddur; `stremio-core` bağımlılık olarak alınmaz.

Deneyin sonucu (`eon-stream-core`, `examples/browse.rs`):

- **Cinemeta** (`com.linvo.cinemeta`, Stremio'nun resmi metadata eklentisi) — manifest, 8 katalog, `top` kataloğu, arama (`search=blade runner`), film metadata'sı, dizi metadata'sı + sezon/bölüm listesi: hepsi çalışıyor.
- **OpenSubtitles v3** (`org.stremio.opensubtitlesv3`) — altyazı kaynağı, 38 gerçek altyazı izi döndü.
- Protokolün tamamı: manifest + `catalog` / `meta` / `stream` / `subtitles` uçları, `extra` parametre kodlaması, `idPrefixes` filtrelemesi, ayrıntılı `resources` formu.

Gerekçe (b)'nin lehine, deneyden sonra daha da net: protokol gerçekten küçük — dört uç ve düz JSON. Buna karşılık `stremio-core` protokolün yanında Stremio'nun durum modelini, kütüphane senkronizasyonunu ve hesap API istemcisini de taşıyor; bunları içeri almak Stremio'nun ürün kararlarını da içeri almak olurdu. Kendi kodumuz **~1.300 satır** ve dört bağımlılık (`serde`, `serde_json`, `thiserror` + örnek için `ureq`).

`stremio-core` yine de değerli: **davranış referansı** olarak okunuyor, bağımlılık olarak değil.

**Deneyin ortaya çıkardığı iki şey (ikisi de kayda değer):**

1. **Gerçek eklentiler `null` gönderiyor.** Cinemeta dizi metadata'sında `videos`, `genres`, `cast` alanlarına dizi yerine `null` koyuyor. `serde`'nin `default` özelliği yalnızca alan **yoksa** devreye giriyor, açıkça `null` ise değil — yani katı bir okuyucu, var olan en yaygın eklentiyi reddediyor. Her koleksiyon alanı artık `null`'ı boş kabul ediyor (`src/serde_lax.rs`). Çizgi şurada: **yanlış tipte** bir değer hâlâ hata. `null`'a toleranslı olmak uyumluluk; bozuk veriyi tahmin etmek hata saklamak olurdu.
2. **Kimlik bilgisi path'te duruyor, host'ta değil.** Eklenti adresi `https://host/c/<token>/manifest.json` biçiminde olabiliyor. "URL'yi loglamayın" kuralı yetersizdi: path'i loglamak da aynı şeyi sızdırıyor. Bunu yazdığım bir test yakaladı (`errors_never_carry_the_addon_address`) ve hata mesajlarından path de çıkarıldı.

Arayüz her iki durumda da tamamen bize aittir ve proje lisansı serbestçe seçilir (madde 33).

**Uyumluluk test seti [K]:** Popüler eklentilerden oluşan bir test seti her sürümde CI'da otomatik çalışır. Kritik kural: **test seti üçüncü taraf sunucuların canlı uptime'ına bağlanmaz** — sabitlenmiş eklenti sürümleri ve kaydedilmiş HTTP fixture'ları kullanılır. Yoksa CI başkasının sunucusu düştüğünde kırmızı yanar ve güvenilirliğini yitirir. Gerçek uçlara karşı çalışan ayrı bir "canlı" iş vardır; onun kırılması sürüm engellemez.

"İstisnasız uyum" hareketli bir hedeftir. Yapısal olarak uyumsuz kalacak eklenti sınıfları baştan kabul edilir ve dokümante edilir: Stremio hesabına bağlı kataloglar, Stremio streaming server'ının özel uçlarına (ör. HLS transcode) dayanan akışlar, Stremio istemcisine özgü davranış bekleyen eklentiler.

**Torrent motoru: librqbit. [K — v0.5'te karara bağlandı]**

İlk aday seçildi ve bağımlılık oldu (`eon-stream-engine`, `src/torrent.rs`). Stremio'nun streaming server'ı (server.js) kapalı kaynak olduğu için açık kaynak bir Rust alternatifi gerekiyordu; librqbit Apache-2.0, aktif ve kütüphane olarak gömülebilir.

Kararı belirleyen şey BitTorrent'in tamamı değil, bu projenin gerçekten ihtiyaç duyduğu **tek** davranıştı: `ManagedTorrent::stream`, okuma başının **önünde** bir parça penceresi tutuyor ve bir atlamada (seek) pencereyi yeniden kuruyor. Bunu kendimiz yazmak, aynı yere varmak için önce DHT, uTP, web seed ve tracker protokollerini yazmak anlamına gelirdi. Parça seçimini yeniden yazan hiçbir kod yok; eklediğimiz şey senkron bir yüzey, oynatıcının aç kalmayacağı bir ön bellek ve insanın okuyabileceği bir ilerleme bilgisi.

Yapılandırma: `default-features = false, features = ["rust-tls"]` — native TLS yerine rustls, böylece Windows derlemesi OpenSSL istemiyor ve sabitlenmiş araç zinciri gerçekten araç zincirinin tamamı oluyor (madde 22).

v0.1'de yazılmayan gerekçe hâlâ geçerli ve artık ölçülebilir: Stremio'nun server'ı sadece torrent değil **transcode/HLS** de yapar. mpv kullandığımız için transcode katmanına hiç ihtiyacımız yok — sıralı indirme + yerel HTTP sunucu yetti. Bu, madde 2'deki mpv riskini almanın asıl karşılığıdır.

**Yerel HTTP sunucusu [K]:** Loopback'e bağlı, oturum başına 32 baytlık bir jetonla korunuyor, yalnızca `GET`/`HEAD` ve range isteklerini yanıtlıyor. Jetonu yanlış olan istek `403` değil **`404`** alıyor: `403`, yolun var olduğunu doğrulayıp portu "ne izleniyor" sorusunun oracle'ına çevirirdi. `Access-Control-Allow-Origin` yok — bir tarayıcı sayfası buraya istek gönderebilir ama yanıtı okuyamaz.

Açık sorunun diğer yarısı — sunucunun torrent olmayan HTTP kaynaklarını da sunup sunmayacağı — **sunmayarak** kapatıldı: bir HTTP kaynağı zaten atlanabilir durumda, onu loopback üzerinden geçirmek mpv'nin kendiliğinden yaptığı şeyin yerine bir sıçrama, bir tampon ve bozulabilecek ikinci bir şey koymak olurdu.

**2. Masaüstü: Tauri (Rust). [K]**
Gerekçe (düzeltildi): asıl kazanç indirme boyutu değil — libmpv ve torrent motoru gömüldükten sonra boyut avantajının büyük kısmı kaybolur. Asıl kazanç **tek dilde (Rust) core + protokol + torrent + istemci**, düşük bellek tüketimi, hızlı açılış ve Tauri 2 ile Android'e aynı çatıdan uzanma imkânıdır.

**Arayüzün zamanlaması — değişti (plan v0.6) [K]:** Önceki karar: arayüz `v1`'e kadar eklenmez, bütün alfa hattı uçbirimde yürür; gerekçe, yarım bir Tauri iskeletinin çürümesi ve hangi kararların gerçekten verildiğini gizlemesiydi. Yeni karar: **arayüz alfa hattında, `v0.14`'te geldi** — proje sahibinin kararı. Gerekçenin korumak istediği şey duruyor: pencere, uçbirimde çalıştığı kanıtlanmış bir sistemin üzerine yazıldı ve uçbirim sürümü `eon-stream-cli` olarak yanında yayımlanmaya devam ediyor.

**Maliyeti, açıkça:** `v1`'e kadar bakılacak ikinci bir yüzey var — her davranış değişikliği iki yerde karşılanıyor ve modüller, güncelleme denetimi, altyazı arama ve derin bağlantılar şimdilik yalnızca uçbirimde. Arayüzün erişilebilirlik ölçütleri (madde 35: klavye, ekran okuyucu) artık ertelenmiş bir iş değil, **ölçülmesi gereken açık bir iş**: kontrast ve metin ölçeklenmesi karşılanıyor, klavye ve ekran okuyucu denetimi `v1` çıkış ölçütü olarak duruyor.

**Oynatıcı stratejisi [K]:** Faz 0'ın en riskli işi mpv'yi derlemek değil, sistem webview'ının üstünde/altında native bir video yüzeyi (Windows'ta child HWND, macOS'ta NSView, Linux'ta X11/Wayland) yönetip arayüzü ona bindirmektir; z-order ve şeffaflık bilinen bir çiledir. Aşamalı gidilir:

- **v1:** mpv **ayrı süreç** olarak çalışır, `--input-ipc-server` üzerinden JSON IPC ile kontrol edilir. ~~Arayüz kontrolleri EON Stream penceresinde, görüntü mpv penceresinde.~~ Görüntü de kontroller de EON Stream penceresinde (plan v0.7, aşağıda). Düşük risk, hızlı sonuç.
- **v1 sonrası:** libmpv gömme ayrı bir dilim olarak kalıyor — artık tek pencere için değil, `--wid`'in çalışmadığı yerler (Android, gerekirse macOS/Wayland) ve tek süreç istenirse diye. Başarısız olursa bugünkü davranış geçerli kalır.

**Oynatıcının yeri — değişti (plan v0.7) [K]:** Önceki karar: `v1`'de görüntü mpv'nin kendi penceresinde, arayüzün videonun üzerine binmesi `v1` sonrasında libmpv gömmeyle. Yeni karar: **görüntü `v0.16`'dan itibaren EON Stream penceresinin içinde** — proje sahibinin kararı. Yol libmpv gömme değil ve `v1` kararının asıl yarısı duruyor: mpv hâlâ **ayrı süreç**, hâlâ JSON IPC ile sürülüyor; yalnızca `--wid` ile pencerenin tutamacı veriliyor ve resmini pencerenin çocuğu olarak çiziyor. Oynatıcı denetimleri, mpv yüzeyinden **sonra** eklenen şeffaf ikinci bir webview (`player.html`); katalog webview'ı oynatma süresince gizleniyor. z-order sorunu Win32 API'ye inmeden sıralamayla çözüldü, uygulama kodunda `unsafe` yok. Bir film açık değilken hiçbir oynatma denetimi görünmüyor.

**Maliyeti, açıkça:** Yalnızca Windows'ta ölçüldü (alt pencere sırası oynatma sırasında gerçekten okunarak). macOS (NSView) ve Linux'ta (X11/Wayland) `--wid` ayrı ayrı doğrulanmadan bu karar oralara taşınmış sayılmaz; tutamaç alınamayan platformda mpv eskisi gibi kendi penceresini açar. Tauri'nin **`unstable` özelliği** açıldı (bir pencerede iki webview için): kararlı olmayan bir API'ye bağımlılık, Tauri sürüm yükseltmelerinde kırılabilir ve her yükseltmede elle denenmesi gerekir. Madde 4'teki "arayüzün erişebildiği her şey bir modülün erişebileceği şeydir" yüzeyi bir sayfa büyüdü; aynı komut kümesini kullanıyor, yeni ağ izni yok. Oynatıcının pencere içi davranışının otomatik testi yok: ekran yakalamayla doğrulanıyor.
- **Android [K — v0.14, önce E idi]:** libmpv'nin Android derleme zinciri ayrı bir projedir. Mobilde `media3` / ExoPlayer değerlendirilir; oynatıcı soyutlaması (`PlayerBackend` arayüzü) baştan iki implementasyonu kaldıracak şekilde yazılır.
  > **Kayıt (21 Kasım 2026, sahibin kararı — plan v0.14):** Sahip, EON Stream'in çalışabileceği her platformda yayınlanmasını istedi. Ardından depolama kullanımının en zayıf Android TV'de çalışacak kadar küçültülmesini, sonra da Android TV ve telefon desteğini istedi. Karar:
  > 1. Masaüstü yayınları CI'dan çıkar: Windows x86_64 ve ARM64, Linux x86_64 ve ARM64, macOS Apple Silicon ve Intel. Her biri yayınlanmadan önce kendi koşucusunda `--selftest`'ten geçer.
  > 2. Görüntünün pencere içinde olması yalnız Windows'ta doğrulandı. Linux ve macOS'ta mpv kendi penceresini açar. `--wid` macOS'ta süreçler arasında çalışmaz, çünkü NSView bir işaretçidir. Linux/X11'de çalışabilir ama doğrulanmadı.
  > 3. Android (telefon ve TV) alfa hattında gelir. Oynatıcı ayrı bir arka uçtur (yukarıdaki `PlayerBackend`). Arayüz kumandayla (D-pad) ve dokunmayla kullanılır.
  > 4. iOS dışarıda kalır.
  >
  > **Bedeli:**
  > - CI süresi ve bakım yüzeyi kabaca üç katına çıkar.
  > - macOS ve Linux sürümleri imzasızdır; Gatekeeper ve dağıtım paketleri ayrı iştir.
  > - Pencere içi görüntü yalnız Windows'ta vardır.
  > - Android için ikinci bir oynatıcı ve kumanda arayüzü yazılır, ve her davranış üç yüzeyde karşılanır.
  >
  > v1 tanımı (Windows + Linux) değişmez. Diğer platformlar v1'in koşulu değil, alfa hattının ek çıktısıdır.

**3. Her şey modül. [K]**
Core yalnızca modül yöneticisi, ayarlar ve güncelleyiciden oluşur. Video, müzik, dosya, marketplace, creator'lar, materyal paketi görüntüleyicisi ve arayüz editörü ayrı modüllerdir: her birinin manifesti, sürümü, bağımlılıkları ve imzası vardır. İlk açılışta kullanıcı kurmak istediği modülleri seçer. Base video kurulumunda yalnızca görüntüleyici ve eklenti ekleme bulunur; marketplace ve creator'lar isteğe bağlı indirilir.

> **Kayıt (v0.10):** "Her şey modül" her ürünün **kendi içinde** geçerlidir. Müzik ve dosya artık EON Stream modülü değil, ayrı ürünler (madde 45); Stream'de modül olarak kalanlar video, marketplace, creator'lar, materyal paketi görüntüleyicisi ve arayüz editörüdür.

**Kural:** Birinci taraf modüllerimiz, community eklentilerinin kullandığı API'nin aynısını kullanır. Bu kural **test edilebilir** olmalıdır: birinci taraf bir modül API'de olmayan bir ayrıcalık kullanıyorsa CI hata verir. Aksi halde kural iyi niyet beyanı olarak kalır ve zamanla erir.

**4. İki eklenti türü. [K]**

- **Uzak eklentiler (Stremio protokolü):** Kullanıcının cihazında kod çalıştırmaz, HTTP üzerinden JSON döner. Risk profili düşük; v1'de bu vardır.
- **Yerel eklentiler:** İki alt sınıfa ayrılır ve aralarındaki sınır **sert** tutulur:
  - *Bildirimsel (v1'de var):* Tema ve düzen — renk, yazı tipi, ikon, yerleşim JSON'u. **Kod yok, kod çalıştırma imkânı yok.** Şemayla doğrulanır.
  - *Kod çalıştıran (v1'de yok — [E]):* Ancak gerçek bir sandbox tasarımı yazıldıktan sonra açılır. Tauri webview'ında üçüncü taraf JS'i izole etmek için tarayıcı eklenti altyapısı yoktur; iki yol değerlendirilir: (i) kısıtlı IPC allowlist'li ayrı webview, (ii) **WASM (wasmtime) + capability tabanlı host fonksiyonları**. Şu anki eğilim (ii): izin sistemi "manifestte istenen yetenek = host'un verdiği fonksiyon" olarak somutlaşır; dosya sistemine ve ağa varsayılan erişim yoktur.

Gerekçe: bu, yanlış yapılması en kolay ve bedeli en yüksek alandır. Bildirimsel katman kullanıcı ihtiyacının büyük kısmını zaten karşılar.

**5. Sürümlü eklenti API'si ve manifest şeması. [K]**
Faz 0'da yazılır, semver ile sürümlenir; her eklenti hedeflediği API sürümünü belirtir. `v0` kararsızdır (bkz. Kapsam notu). İstemci, desteklemediği major sürümü hedefleyen eklentiyi yüklemez ve kullanıcıya nedenini söyler.

Manifestte baştan bulunan, v1'de kullanılmayan ama format uyumluluğu için ayrılan alanlar: `price` / `license` (madde 10), `mirrors` (madde 15d), `signature`, `min_api`, `max_api`.

---

## B. Marketplace ve moderasyon (6–10)

**6. Moderasyon çekirdeği (şimdiden kurulu, feature flag ile kapalı). [K]**
Her yayın ve güncelleme, değişiklik farkıyla birlikte bir **revizyon** olarak kaydedilir. İnsan, kural botu ve LLM denetçisi aynı kuyruğa takılabilir; eylemler: uyar, gizle, kaldır. Her karar denetim kaydına (audit log) yazılır, itiraz yolu vardır. LLM taraması, revizyon kaydındaki son kontrol noktasından itibaren belirlenen aralıklarla çalışır.

v0.2'de eklenen kurallar:
- **Kalıcı kaldırma yalnızca insan onayıyla** yapılır.
- LLM taramasının **maliyet üst sınırı** vardır; sınır aşılırsa kuyruk insana düşer.
- **LLM sağlayıcısı erişilemezse otomatik onay verilmez**; öğeler "incelenmedi" durumunda bekler. Erişilemezlik asla "temiz" anlamına gelmez.
- Karar veren her aktörün (insan / kural botu / LLM) kimliği ve model sürümü kayda yazılır — şeffaflık raporu (madde 12f) buna dayanır.
- **Bootstrap:** sıfır moderatörle marketplace açılmaz (madde 30'daki kapı).

**7. Hesaplar isteğe bağlı. [K]**
Görüntüleyici hesapsız çalışır ve hesapsız **tam işlevlidir**. Your feed, puanlama ve yayınlama, marketplace modülüyle gelen ayrı bir girişle yapılır. Hesap sistemi, veri koruma yükümlülüğünün fiilen başladığı noktadır (madde 31).
> **Kayıt (v0.11):** Hesap artık ürün başına değil, bütün EON için tek: **EON Account** (madde 46). İlk açılışta "giriş yap / hesap oluştur / misafir olarak devam et" sorulur; misafir tam bir cevaptır ve soru kendiliğinden geri gelmez. Bu maddenin özü değişmez: hesapsız kullanım tam işlevlidir. Topluluk özellikleri hesap ister.

**8. Marketplace ucuz başlar. [K]**
İlk aşamada index bir Git deposunda tutulur, yayın PR ile olur. Bu aşamada barındırma büyük ölçüde depo sağlayıcısındadır (madde 29, 30/L0). Puanlama, arama, öne çıkanlar, sıralama ve feed'ler (your feed / global feed) geldiğinde gerçek bir backend'e geçilir — ve o geçiş madde 30'daki kapıya tabidir.

**9. İçerik kuralı. [K]**
Kullanıcı herhangi bir eklentiyi URL ile ekleyebilir (tarafsız görüntüleyici). EON Stream marketplace'ine ise yalnızca yasal içerik sağlayan eklentiler, temalar ve araçlar girer. **Uygulama hiçbir torrent eklentisiyle, hiçbir hazır kaynakla gelmez.** Marketplace'te telif ihlali yapan bir eklenti öne çıkarılmaz, listelenmez, aranamaz.

Bu madde, madde 29'daki hukuki duruşun teknik karşılığıdır ve gevşetilemez. Gevşetilmesi teklif edilirse madde 29 ile birlikte tartışılır.

**10. Ücretli eklentiler. [E]**
Komisyon için tüzel kişilik, vergi ve ödeme sağlayıcı şartları gerekir (madde 30, Faz L2). Şimdilik manifestte fiyat/lisans alanı ve lisans doğrulama kancası bırakılır; hiçbir ödeme akışı yazılmaz.

---

## C. Hukuk, politika ve marka (29, 30, 11, 12, 13, 31, 32, 14)

> **Bu bölüm v0.2'de tamamen yeniden yazıldı.** Önceki sürüm, yargı yeri seçilmeden Amerikan DMCA 512(g) karşı-bildirim mekanizmasını kopyalıyordu; bu, o rejimin **korumasını almadan yükümlülüğünü üstlenmek** olur. Yeni bölüm üç ilkeye oturur:
>
> **(i) Rolleri ayır** — tek bir hukuki şapka yok, üç ayrı rol var (29).
> **(ii) Yükümlülüğü aşamalandır** — ürün büyüdükçe yükümlülük artar, kapılarla (30).
> **(iii) Süreci fiilen tabi olduğun hukuka göre kur** — taklit değil (12).
>
> Numaralar konumsal değildir: 11–14 eski numaralarını korur, yeni hukuk kararları 29–32'yi alır.

**29. Hukuki konumlandırma: ne olduğumuz, ne olmadığımız. [K]**

Proje tek bir hukuki şapka taşımaz. **Üç ayrı rol** vardır ve bunları birbirine karıştırmamak savunmanın temelidir:

- **Rol A — Yazılım geliştirici (EON Stream istemcisi).** İstemci içerik barındırmaz, içerik indekslemez, kaynakla gelmez, kullanıcı adına içerik aramaz. Kullanıcının kendi eklediği bir uca bağlanan genel amaçlı bir oynatıcıdır. *Bu rol bugün geçerli olan tek roldür.*
- **Rol B — Yer sağlayıcı (marketplace index, paket metadata, kullanıcı içeriği).** Bu rol ancak marketplace açıldığında doğar; barındırma yükümlülükleri (madde 30/L1) buna bağlanır. Rol A'ya ait savunmalar Rol B'yi kapsamaz.
- **Rol C — Yazılım sağlayıcı / veri işleyen (Edu).** Edu'da öğrenci ve öğretmen verisinin **veri sorumlusu kurumdur** (okul / il / bakanlık); biz yazılımı veririz, kurum kendi sunucusunda çalıştırır (madde 24). Bize hiçbir öğrenci verisi akmaz. **Bulut hizmeti verdiğimiz gün bu rol değişir** ve bu madde güncellenir — sessizce kaymasına izin verilmez.

**Asla yapmadığımız şeyler (bağlayıcı liste; dokümantasyonda ve sitede yayınlanır):**

1. Telif ihlali yapan içeriği barındırmayız, indekslemeyiz, aranabilir kılmayız.
2. Uygulama hiçbir kaynak veya eklenti ile önyüklü gelmez (madde 9).
3. Korsan erişimi kolaylaştıran rehber, liste, öneri veya "nasıl yapılır" içeriği yayınlamayız; topluluk kanallarımızda da bunlara izin vermeyiz (davranış kuralları — madde 33).
4. Böyle bir eklentiden reklam, bağış, komisyon veya herhangi bir gelir elde etmeyiz.
5. Kullanıcıların ne izlediğini kaydetmeyiz (madde 36).
6. Korsan bir eklenti hiçbir koşulda resmileştirilmez (madde 14).

**Gerekçe:** Genel amaçlı aracın meşruluğu iki ayağa dayanır — aracın esaslı meşru kullanımı olması **ve sağlayıcının ihlali teşvik etmemesi**. Teknik tarafsızlık tek başına yetmez; teşvik etmeme fiilen ve **yazılı olarak kanıtlanabilir** olmalıdır. Bu liste o kanıttır ve bu yüzden bir pazarlama metni değil, plan maddesidir.

**30. Aşamalı hukuki yükümlülük ve marketplace kapısı. [K]**

Yükümlülük ürünle birlikte kademelenir. Her kademeye geçiş bir kontrol listesiyle olur; liste tamamlanmadan geçilmez. Kapının amacı, "sonra hallederiz" diyerek Faz L0'ın rahatlığıyla Faz L1'in riskini almayı **yapısal olarak imkânsız** kılmaktır.

**Faz L0 — Yalnızca yazılım (şu an).**
Barındırma yok, hesap yok, ödeme yok, gelir yok, backend yok. Kaynak kod ve sürümler bir depo sağlayıcısında; marketplace index'i Git deposunda, PR ile (madde 8). Kişisel veri işlenmez. Tüzel kişilik gerekmez.
*Bu fazın önemli ayrıntısı:* index bir depo sağlayıcısında (ör. GitHub) durduğu sürece o içerik için **sağlayıcının kendi bildirim-kaldırma süreci de** geçerlidir ve şikâyet doğrudan oraya gidebilir. Bu bizim seçimimiz değil, bir olgudur; madde 12b/3'te akış olarak yazılır ve dokümantasyonda gizlenmez.

**Faz L1 — Marketplace backend + hesaplar.** Geçiş kapısı: **beşi birlikte** karşılanmadan açılmaz.

1. **Tüzel kişilik** kurulmuş (dernek / şirket / mali ev sahibi — madde 38'de karşılaştırılır). Gerekçe: hesap, barındırma ve şikâyet süreci gerçek kişiye bağlı kaldığı sürece tüm hukuki risk tek bir insanın üzerindedir.
2. **Hukuki inceleme** tamamlanmış (madde 13).
3. **Yer sağlayıcı yükümlülükleri** karşılanmış: sitede açık iletişim ve bildirim adresi, çalışan bildirim kanalları (madde 12a), trafik kaydı ve saklama yükümlülüğünün kapsamı netleşmiş. **[A]** Ticari amaçla yer sağlayıcılık için BTK'dan faaliyet belgesi gerekir; kâr amacı gütmeyen bir yapının bu kapsama girip girmediği avukata sorulacak açık sorulardan biridir (madde 13, soru 1).
4. **En az iki insan moderatör + bir yedek** (madde 6'nın bootstrap kuralı). Tek kişilik moderasyon, tatilde veya hastalıkta süreci durdurur.
5. **Şeffaflık raporu altyapısı** çalışıyor (madde 12f) ve ilk raporun formatı hazır.

**Faz L2 — Ücretli eklentiler ve gelir paylaşımı (madde 10, 14).**
Ek olarak: vergi mükellefiyeti, ödeme sağlayıcı sözleşmesi, mesafeli satış ve cayma hakkı metinleri, fatura akışı, satıcı kimlik doğrulaması.

**İzlenecek eşikler [A].** Kullanıcı sayısı büyürse ek yükümlülük rejimleri devreye girebilir: 5651 kapsamındaki **sosyal ağ sağlayıcı** eşikleri (temsilci atama, veri yurt içinde tutma vb.) ve AB kullanıcısı hedeflenirse **DSA**'nın çevrimiçi platform yükümlülükleri (bildirim-eylem, gerekçe bildirimi, iç şikâyet mekanizması, şeffaflık raporu). Bu eşikler bir kez ölçülüp unutulmaz; her altı ayda bir gözden geçirilir ve bu maddeye tarih düşülür.

**11. Politika sürümleme. [K]**

Her politikanın (kullanım şartları, gizlilik, içerik politikası, moderasyon kuralları, Edu kurum sözleşmesi) semver'li sürüm numarası ve yürürlük tarihi vardır. Metinler **depoda** tutulur; değişiklikler diff olarak görünür — politika değişikliği sessizce yapılamaz.

- Her kullanıcının hangi politika sürümünü ne zaman kabul ettiği kaydedilir.
- Güncellemede uygulama içi bildirim + onay kutucuğu; önemli değişikliklerde **en az 30 gün önce duyuru** ve açık bir yürürlük tarihi.
- **Kişisel veri işlemeyle ilgili değişikliklerde açık rıza yeniden alınır**; sessiz kalma kabul sayılmaz.
- Kullanıcı kabul etmezse ne olacağı her politika için ayrıca yazılır. Örnek: marketplace hesabı dondurulur, **yerel görüntüleyici çalışmaya devam eder** (madde 7) — politika reddi uygulamayı tuğlaya çevirmez.
- Politikalar ticari platformlardan **birebir kopyalanmaz**. Yapı olarak açık lisanslı politika metinleri (ör. Basecamp, Automattic) referans alınır ve uyarlanır.

**12. Bildirim ve kaldırma süreci. [A — madde 13'e kadar taslak]**

Tasarım ilkesi: **süreç, fiilen tabi olduğumuz hukukun mekanizmasına oturur; başka bir ülkenin mekanizması taklit edilmez.** Türkiye merkezli bir yapı için pratikte üç ayrı yol, tek bir kuyruğa düşer.

**a) Kanallar.** Web formu ve e-posta; ikisi de her zaman açık, ikisi de aynı ticket sistemine düşer. Sitede açık iletişim bilgisi ve bildirim adresi bulunur. Her ticket numaralı ve izlenebilirdir; şikâyetçi durumunu görebilir.

**b) Üç yol, tek kuyruk.**

1. **Telif ve bağlantılı haklar.** Türk hukukunda eser sahibinin yolu **FSEK ek madde 4** üzerinden ilerler: hak sahibi önce içerik sağlayıcıya başvurup ihlalin durdurulması için süre verir; sonuç alınmazsa Cumhuriyet savcılığı yoluyla yer sağlayıcıdan kaldırma istenir. **Bizim gönüllü sürecimiz bu yoldan daha hızlıdır:** eksiksiz bir bildirim geldiğinde resmi yol beklenmeden ilgili öğe gizlenir. Gönüllü hız, resmi yolun yerine geçmez; onu gereksizleştirmeyi amaçlar.
2. **Kişilik hakları / özel yaşam.** 5651 kapsamında içeriğin kaldırılması veya erişimin engellenmesi için ayrı bir usul ve **kısa süreler** işler. Bu tür bildirimler **doğrudan ve öncelikli olarak insan kuyruğuna** düşer; LLM ön kontrolüne bile takılmaz.
3. **Depo sağlayıcısı yolu (Faz L0'da).** Index bir depo sağlayıcısında olduğu sürece şikâyet oraya da gidebilir ve sağlayıcının kendi süreci (ör. GitHub DMCA) bizden bağımsız işler. Bu yolun varlığı dokümantasyonda açıkça yazılır; yayıncılar için sürpriz olmaması sağlanır.

**c) LLM'in yetkisi sınırlıdır.** LLM yalnızca **eksiklik kontrolü, spam ve kopya tespiti** yapar. Bildirimi reddetme, içerik kaldırma veya hak sahipliği değerlendirmesi yapma yetkisi **yoktur**.
- Eksiksiz bildirimde ilgili öğe otomatik gizlenir.
- Eksik bildirimde **ticket kapanmaz**: şikâyetçiye eksikler ve tamamlama linki gider, ticket aynı zamanda insan kuyruğuna düşer. "Eksik" hiçbir zaman "reddedildi" demek değildir.

**d) Asgari müdahale.** Mümkün olan her durumda tüm eklenti değil, yalnızca **şikâyet edilen öğe** gizlenir. Eklentinin bütününün kaldırılması ancak ihlal bütününe yayılmışsa yapılır ve gerekçesi kayda yazılır.

**e) Yayıncı hakları ve itiraz.** Yayıncıya anında bildirim gider: gerekçe, şikâyetin özeti (şikâyetçinin kişisel verileri maskelenerek) ve itiraz yolu. İtiraz **insan** tarafından değerlendirilir.

> **Geri yayın kuralı (v0.1'den değişti).** Eski kural — "şikâyetçi belirli süre içinde dava açmazsa içerik otomatik geri yayına alınır" — dayanağı olmayan bir otomatizmdi ve başka bir hukuk düzeninin kopyasıydı. Yeni kural: **itiraz haklı görülürse içerik geri yayına alınır.** Hak sahipliği tartışmalıysa ve taraflardan biri yasal yola gitmişse içerik **yargı kararına kadar gizli kalır.** Bu proje hak sahipliği konusunda hüküm veren merci değildir ve o rolü üstlenmez; üstlenmesi hem yetkisiz hem risklidir.

**f) Kötüye kullanım ve şeffaflık.** Bildirimde doğruluk beyanı istenir. Haksız/kötü niyetli bildirim sayacı tutulur; tekrarlayan kötüye kullanım bildirim hakkının kısıtlanmasına yol açar. **Altı ayda bir şeffaflık raporu** yayınlanır: bildirim sayısı, tür dağılımı, kaldırılan ve geri yayınlanan öğe sayısı, ortalama yanıt süresi, otomatik/insan karar oranı, kötüye kullanım vakaları.

**g) Strike sistemi.** İlk ihlal uyarı kademesidir; strike'lar belirli sürede düşer; tekrarlayan ihlalcinin yayıncı hesabı kapanır. Creator'da yayın öncesi kısa bir hak rehberi ve "bu içeriğin hakları bende / lisansı şudur" beyanı bulunur (madde 15c).

**h) Edu'da aynı süreç okul düzeyinde çalışır** (madde 27b): kurum içi paylaşımda da bildirim kanalı vardır; ticket okul yöneticisine, gerekirse il düzeyine gider. Öğrenci bildirimi öğretmeni atlayabilir (madde 23).

**13. Hukuki inceleme. [K — Faz L1'in ön koşulu]**

Marketplace açılmadan önce bir **bilişim / telif hukuku avukatına** okutulacaklar: kullanım şartları, gizlilik politikası, içerik politikası, moderasyon ve bildirim süreci (madde 12), Edu kurum sözleşmesi ve veri işleme eki, eklenti lisans sözleşmesi, creator hak beyanı metni.

Avukata götürülecek **somut soru listesi** (bunlar plan içinde cevaplanmaz, cevaplanmış gibi de davranılmaz):

1. Marketplace index'i ve kullanıcı paketleri barındırmak bizi hangi rolde konumlandırır (yer sağlayıcı / içerik sağlayıcı)? Kâr amacı gütmeyen yapı için BTK yer sağlayıcı faaliyet belgesi gerekir mi?
2. Trafik bilgisi saklama yükümlülüğünün kapsamı ve süresi nedir; madde 36'daki "telemetri yok" duruşuyla nasıl uzlaşır?
3. Genel amaçlı bir oynatıcının, kullanıcının kendi eklediği üçüncü taraf uçları üzerinden gerçekleşen ihlallerdeki sorumluluk sınırı nedir? Madde 29'daki liste bu sınırı korumaya yeter mi, eksiği var mı?
4. Torrent motorunu gömülü olarak dağıtmak — hiçbir kaynak önyüklü olmadan — ayrı bir risk doğurur mu?
5. Edu'da "kurum veri sorumlusu / biz veri işleyen" ayrımı sözleşmeye nasıl yazılır; VERBİS kaydı kimin yükümlülüğü?
6. Öğrenci (çocuk) verisi için veli onayı akışı, saklama süreleri ve imha nasıl kurulur?
7. Tüzel kişilik tipi (dernek / şirket / yurt dışı mali ev sahibi) risk, vergi ve kamu satın alma açısından nasıl karşılaştırılır?
8. "Stremio eklentileriyle uyumlu" ifadesinin marka hukuku açısından güvenli sınırı nedir (madde 32e)?

Cevaplar geldiğinde ilgili maddeler güncellenir ve **[A]** etiketleri **[K]**'ya döner.

**31. Veri koruma: tasarımla asgari veri. [K]**

İlke: **toplamadığın veriyi korumak zorunda değilsin.** Uyum, politika metniyle değil mimariyle sağlanır.

- **Asla toplanmayanlar:** TC kimlik numarası (madde 21), izleme/oynatma geçmişi, öğrenci performans verisi (bizim tarafımızda), cihaz parmak izi, reklam kimliği.
- **Yerelde kalanlar:** eklenti listesi, kaldığı yer/ilerleme, ayarlar, materyal paketi ilerlemesi (madde 16b). Hesapsız kullanımda hiçbir şey dışarı çıkmaz (madde 7).
- **Kayıt (v0.11) — şifreli kasa.** EON Account'a giriş yapan kişinin ilerlemesi ve puanları cihazlar arasında taşınır; ama sunucuya **yalnızca cihazda şifrelenmiş hâlde** gider ve anahtar sunucuda hiç bulunmaz (madde 46b). Sunucu okuyamadığı veriyi saklar; bu, izleme geçmişi *toplamak* değildir ve bu maddedeki yasak aynen geçerlidir. Kasaya sunucu tarafında okunabilir bir dizin, özet ya da istatistik eklemek bu maddeyi çiğnemektir.
- **Marketplace hesabı (Faz L1):** yalnızca e-posta + kullanıcı adı + yayınlar. Aydınlatma metni, rıza kayıtları (madde 11), erişim/silme/taşıma hakları için **self-servis** akış, her veri türü için süre belirten **saklama ve imha politikası**, veri ihlali bildirim prosedürü (kuruma ve ilgili kişiye, mevzuattaki sürede).
- **Yurt dışına aktarım:** varsayılan **yok**; sunucular Türkiye'de (madde 24). Bir hizmet zorunlu olarak yurt dışındaysa (ör. LLM moderasyon sağlayıcısı — madde 6) bu **açıkça dokümante edilir**, aktarılan veri asgariye indirilir ve gereken hukuki mekanizma kurulur. **Öğrenci verisi hiçbir koşulda yurt dışı bir hizmete gönderilmez** — moderasyon LLM'i dahil.
- **Edu:** kurum veri sorumlusu, biz veri işleyen (madde 29 / Rol C). Kurumla veri işleme sözleşmesi imzalanır. Edu için **veri koruma etki değerlendirmesi** yapılır ve kurumla paylaşılır; çocuk verisi işlendiği için bu isteğe bağlı sayılmaz.
- **Eşler arası IP görünürlüğü:** madde 25'te ele alınır; varsayılan mimari bu sorunu **doğurmayacak** şekilde seçilmiştir.

**32. Marka mimarisi ve iletişim duruşu. [K — v0.3'te yeniden yazıldı]**

**Yapı:** çatı **EON**, ürün adı çatıdan sonra gelir — *EON \<Ürün\>*. Bugün **EON Stream** ve **EON Edu**; yarın ne gelirse aynı kalıba oturur (madde 42). Ürün adı tek başına kullanılmaz ("Stream" değil, "EON Stream"), çatı da tek bir ürünle eşitlenmez ("EON" bir oynatıcının adı değildir).

- **a) İsimlendirme kuralı.** Depo, paket, ikili dosya ve uygulama kimliği adları çatı-ürün-bileşen sırasını izler: `eon-stream-core`, `eon-edu-server`, `eon-stream-app`. Çatı düzeyinde olan şeyler ürün adı almaz: `eon-docs`, `.github`, EON Modül ABI İstisnası.
- **b) EON Stream'in kamuya anlatımı.** "Oynatıcı ve eklenti platformu" olarak tanıtılır. Hiçbir resmi kanalda torrent/korsan bir kullanım örneği verilmez; ekran görüntülerinde, tanıtım videolarında ve dokümantasyonda böyle bir kaynak görünmez (madde 9, 29). Duruş iletişimle değil, ürünün kendisiyle kanıtlanır.
- **c) EON Edu'nun kamuya anlatımı.** Kendi sitesi ve dokümantasyonuyla anlatılır (madde 22). Ön sayfada "kurumsal üründe şunlar **derleme aşamasında hiç bulunmaz**" listesi yer alır — savunma olarak değil, özellik olarak. Kurum için bu bir güvence maddesidir.
- **d) Görsel ayrım.** İki ürün ortak bir EON görsel kimliğini paylaşır, ürün düzeyinde ikon varyantı ve renkle ayrılır. Ortak çatı görünür olmalı; ürünler karışmamalı.
- **e) Marka politikası ve fork'lar.** İsim ve logo için marka politikası yazılır (madde 33): **kod özgürce fork'lanır, isim fork'la birlikte gitmez.** Bu, marka mimarisinin en önemli korumasıdır — aksi halde biri EON adıyla korsan bir sürüm dağıttığında ayrım yapılamaz ve EON Edu'nun itibarı bizim dışımızdaki bir aktör tarafından yakılabilir. Koruma **çatı adı** düzeyinde kurulur; ürün adlarını tek tek korumak yetmez.
- **f) Üçüncü taraf markaları [A].** "Stremio eklentileriyle uyumlu" gibi ifadeler tanımlayıcı kullanım sınırında tutulur; Stremio'nun adı veya logosu onay/ortaklık izlenimi verecek şekilde kullanılmaz, ikonlarda ve uygulama içinde yer almaz. Sınır madde 13'ün 8. sorusuyla netleştirilir.
- **g) Alan adı ve ad müsaitliği [A].** Çatı adı seçildi, ama `eon` üç harfli ve yaygın bir kelime: GitHub organizasyonu, npm/crates.io ad alanı, alan adı ve marka tescili müsaitliği **ayrı ayrı** kontrol edilmelidir. Şema `$id`'leri ve politika e-postaları şu an `eonstream.org` varsayıyor; alan adı alınmazsa bu değerler değişir. Bu, ilk push'tan önce cevaplanması gereken bir sorudur, sonra değil.

**14. Resmi yayıncı programı — kaldırma sürecinden tamamen ayrı kanal. [E — Faz L2]**

Hak sahipleri doğrulanmış yayıncı rozeti alır ve resmi eklenti yayınlar: ücretsiz tanıtım içeriği, kendi platformlarına yönlendiren eklentiler veya ücretli eklentiler (madde 10).

Değişmez kurallar:
- **Kaldırma asla pazarlık konusu olmaz.** Davet ancak kaldırma süreci tamamen kapandıktan sonra ve **ayrı bir yazışmayla** gider. Kaldırma yazışmasında program hiç anılmaz — anılması, kaldırmayı bir baskı aracına çevirir.
- **Korsan bir eklenti resmileştirilmez.** Lisans sözleşmesi imzalandıktan sonra resmi eklenti **sıfırdan** yayınlanır; eski eklentinin sürüm geçmişi, kullanıcıları veya puanı devredilmez.
- Rozet doğrulama süreci ve red gerekçeleri yazılıdır; **rozet ücretli değildir** (ücretli rozet, doğrulamayı satmaya dönüşür).

---

## D. Creator'lar ve materyal paketleri (15–16)

**15. Creator modülleri (her biri isteğe bağlı). [K]**

- **a)** Türe göre ayrı creator'lar: video, müzik, dosya ve materyal paketi. Paketleme, manifest üretimi, imzalama, doğrulama ve yayınlama ortak bir **creator-core**'da durur. *(Kayıt, v0.10: müzik ve dosya creator'ları, o türler EON Stream'den ayrıldığı için ilgili ürünlerin işidir — madde 45; EON Stream'de video ve materyal paketi creator'ları kalır.)*
- **b)** İzin listesi mantığı: her creator yalnızca kendi türüne ait formatları kabul eder (ör. dosya creator'ı mp4/mp3 kabul etmez). Tür kontrolü **uzantıya değil dosya içeriğine** (MIME / imza baytları) bakılarak yapılır.
- **c)** Akış: dosya seç → tür kontrolü → manifest otomatik doldurulur (kullanıcı isim, açıklama, ikon, kategori düzenler) → doğrulama (şema, API sürümü, kural botu) → imzalı paket → yerel önizleme → yayınla (hak beyanı, moderasyon kuyruğu, marketplace index'i). Paket yayınlanmadan dosya veya URL olarak da paylaşılabilir.
- **d) Dağıtım: web seed baştan spesifikasyonda. [K — v0.1'den değişti]**
  EON Stream'de içerik paketleri kullanıcının kendi dosyalarından oluşturulan torrent ile dağıtılır; bizim sunucularımızda dosya tutulmaz. Ancak "yayıncı uygulamayı kapatınca paylaşım durur" sorunu, kamuya açık dağıtımın tamamını güvenilmez yapar. Bu yüzden **paket formatında `mirrors` alanı ve web seed (BEP 19) desteği `v0`'dan itibaren vardır** — "ileride" değil. Yayıncı isterse kendi HTTP kaynağını (bulut linki, kendi sunucusu, kurum sunucusu) ek dağıtım kaynağı olarak girer; istemci eş bulamazsa oradan indirir. Madde 16g bunun Edu'daki özel hâlidir.
  Gerekçe: formatı sonradan genişletmek, mevcut tüm paketleri ikinci sınıf yapar. Alanın baştan var olması bedava, sonradan eklenmesi pahalıdır.
- **e)** Creator'ın ürettiği **uzak eklentiler** Stremio'da da çalışır (çift yönlü uyum). **Materyal paketleri** EON Stream'ye özgüdür; Stremio'da açılmaz ve bu beklentiye girilmez.

**16. Materyal paketi sistemi. [K]**

> Arayüzde ve kullanıcıya dönük her metinde **"materyal paketi"** denir. "Fullpack" yalnızca teknik dokümantasyonda geçen iç terimdir — Edu kitlesi (öğretmen, müdür, kurum) için jargon olmaması gerekiyor.

- **a)** EBA mantığında hiyerarşi: **ders → ünite → konu**; her konu video, ses ve dosya içerebilir. Metadata'da sınıf seviyesi, ders ve konu alanları bulunur (ör. "9. sınıf fizik" araması). Metadata şeması MEB müfredat yapısına eşlenebilir olacak şekilde tasarlanır.
- **b)** İki görüntüleme modu, creator seçer: *akış modu* (belirli sırayla ilerleme) ve *serbest mod* (dosyaları elle açma); bölüm bazında karışık kullanım mümkün. **İlerleme yalnızca cihazda tutulur, hesap gerekmez** (madde 31).
- **c)** Materyal paketi görüntüleyicisi ayrı modüldür; paket, kullanıcıda olmayan bir modül gerektiriyorsa kurulumu önerir (madde 3'teki bağımlılık sistemi). **Modül eksikse paket kısmi açılır** — hiç açılmamak yerine açılabilen kısmı gösterir ve eksiği söyler.
- **d)** Görünürlük: herkese açık (marketplace), liste dışı (link ile), sınıfa özel (Edu, hesap gerektirir).
- **e)** Lisans seçici; varsayılan öneri **CC BY-SA**. Hazır kaynaklar pakete eklenirken lisansları kontrol edilir; creator bir **kaynak/atıf listesi** alanı sunar.
- **f)** Eğitim kategorisindeki paketler daha sıkı moderasyon kural setinden geçer; öğrenci verisi toplanmaz.
- **g)** Sürekli erişilebilirlik için öğretmen kendi bulut linkini ek dağıtım kaynağı olarak ekleyebilir — bu, madde 15d'deki `mirrors` alanının aynısıdır (Edu'da bkz. madde 25).
- **h) Çevrimdışı [K]:** Materyal paketi bir kez indirildikten sonra **internet olmadan tamamen çalışır**. Bu bir yan etki değil, gereksinimdir (madde 40); okul interneti varsayılan olarak kötüdür ve bu varsayım üzerine tasarlanır.

---

## E. EON Edu (17–28)

> Formül (bkz. Vizyon): **Edu = ortak core + kısıtlama flag'leri + Edu modülleri.** Madde 22 flag'leri, madde 17–21 ve 25 ek modülleri, madde 27 kısıtlama modelini tanımlar.

**17. Rol sistemi. [K]**
Roller **kapsamlı yetkidir**: bir role *ve* bir kapsama bağlıdır ("X okulunda öğretmen"). Aynı kişi bir okulda öğretmen, başka bir okulda veli olabilir. Roller: hak sahibi, müdür, öğretmen, öğrenci, veli, moderatör. Her rolün izin seti ve kendi detaylı kurulum/kullanım guideline'ı vardır.

İzin setleri kodda **tek bir yerde** tanımlanır ve testlerle doğrulanır. Bir rolün yetkisi arayüzde düğme gizlenerek değil, **sunucuda reddedilerek** sınırlanır.

**18. Rol doğrulama zinciri. [K]**
Kimse kendi rolünü seçemez. Müdür manuel doğrulanır → müdür öğretmenleri davet eder → öğretmen öğrencileri sınıf koduyla ekler.

**Manuel doğrulamanın somut hâli (v0.1'de eksikti).** Kendini müdür gösteren biri tüm okulun öğrenci hesaplarına erişir; bu, sistemdeki en ciddi sahtekârlık vektörüdür. Pilot aşamada doğrulama = kurumun resmi e-posta alan adı + kurum telefonundan teyit + imzalı kurum formu + insan onayı.
**Bu süreç ölçeklenmez ve ölçeklenmesi beklenmez:** OIDC (madde 21) gelene kadar Edu **en fazla belirlenmiş sayıda pilot okulda** açılır. Sayı plana yazılır ve aşılmaz (bkz. açık konular). Sınıf kodları süreli ve tek kullanımlıdır; her öğrenci ekleme işlemi kayda geçer.

**19. Kurum hiyerarşisi ve iletişim. [K — kapsam daraltıldı]**
Hiyerarşi: **okul grubu → sınıflar ve çalışma grupları → kanallar.** Yalnızca üyelere açık, marketplace'ten tamamen izole; materyal ve notlar grup içinde doğrudan paylaşılır.

**Sohbet kapsamı (v0.1'den değişti).** Matrix + LiveKit/Jitsi tek başına bir ürün büyüklüğünde işletme yükü getirir (sunucu, medya deposu, federasyon, saklama, moderasyon) ve madde 23'teki denetim gereksinimiyle doğrudan çelişir. Aşamalandırılır:

- **Pilot (Faz 3): sohbet yok.** Yalnızca **duyuru akışı** (öğretmen → sınıf, tek yönlü) ve **materyal altında yorum** (kimlikli, kurum içinde, kayıtlı). Pilotun gerçek ihtiyacı budur.
- **Sonrası [A]:** Gerçek sohbet gerekirse Matrix değerlendirilir; hiyerarşi space/room yapısına oturur. Zorunlu koşullar **baştan** yazılır: **uçtan uca şifreleme kapalı** (madde 23'teki yönetim denetimi bunu gerektirir; açık E2EE ile "denetlenebilir" demek dürüst olmaz), saklama süresi tanımlı, kurum sunucusunda barındırma, federasyon varsayılan kapalı.
- **Görüntülü/sesli ders [E]:** LiveKit veya Jitsi, ayrı bir dilim. Pilotta yok.

**20. Okul araçları (her biri modül). [K]**
Sınıf bazlı yoklama, ödev listesi, ödev teslim portalı, öğretmen kontrol ve not portalı.
Notlar başta **dışa aktarılabilir** (CSV/XLSX). e-Okul ile doğrudan entegrasyon **yalnızca MEB'in resmi izniyle ve resmi API ile** yapılır; ekran kazıma veya resmi olmayan yollar hiçbir koşulda kullanılmaz.

**21. Takılabilir kimlik sistemi. [K]**
Giriş, standart bir protokolle (**OIDC**) takılıp çıkarılabilen kimlik sağlayıcılar üzerinden yapılır. e-Devlet / EBA / MEBBİS girişi ancak resmi protokolle; o zamana kadar okulun kendi hesapları (madde 18'in sınırlı pilot modu).
**TC kimlik numarası saklanmaz.** Doğrulamayı resmi sağlayıcı yapar; sisteme yalnızca "doğrulandı" bilgisi ve sağlayıcının verdiği **opak kullanıcı kimliği** gelir (madde 31).

**22. İki uygulama, tek core, derleme zamanı ayrım. [K]**
Tek repoda ortak core; EON Stream ve EON Edu iki ayrı derleme profili. Edu derlemesinde **açık torrent erişimi, URL ile eklenti ekleme ve açık marketplace derleme aşamasında dışarıda bırakılır** — gizlenmez, hiç bulunmaz. Edu'nun modül yöneticisi yalnızca Edu için imzalanmış modülleri yükler. Uygulama kimliği, güncelleme kanalı ve imza anahtarı ayrıdır (madde 34). Edu'nun kendi web sitesi ve dokümantasyonu olur (madde 32b).

v0.2'de eklenen mühendislik kuralları:
- **"Hiç bulunmaz" iddiası test edilir.** Cargo feature'ları toplamsaldır ve sızması kolaydır; CI'da Edu binary'sinde yasaklı sembollerin ve dizelerin bulunmadığını doğrulayan bir kontrol çalışır. Kontrol başarısızsa sürüm yayınlanmaz. **İddia sözle değil testle savunulur** — kuruma verilen güvence (madde 32b) buna dayanır.
- **Derleme doğrulanabilirliği [A — iddia yumuşatıldı].** Rust + Tauri ile tam bit-bit tekrarlanabilir derleme ciddi bir taahhüttür (gömülü yollar, zaman damgaları, platform araç zinciri sabitleme). v1 hedefi: **CI kaynaklı derleme kanıtı (provenance/attestation) + yayınlanan her dosyanın SHA256'sı + sabitlenmiş araç zinciri sürümleri.** Tam tekrarlanabilirlik önce Linux derlemesinde denenir; başarılırsa bu madde güncellenir ve iddia genişletilir. Karşılayamayacağımız bir iddiayı plana yazmıyoruz.

**23. Çocuk güvenliği politikası. [K]**
- Öğretmen–öğrenci iletişiminde **kanallar ve duyurular öne çıkar**; birebir özel mesaj **varsayılan olarak kapalıdır**. Açıldığı durumda okul yönetiminin denetimine açıktır ve bu **her iki tarafa açıkça bildirilir** — gizli denetim yapılmaz.
- Ders kayıtları yalnızca **bilgilendirilmiş onayla**; kayıt sırasında görünür gösterge; saklama süresi tanımlı.
- Her ekranda **"bildir" düğmesi**. Bildirim **öğretmeni atlayarak** doğrudan okul yönetimine, gerekirse il düzeyine gidebilir — öğretmen hakkında bildirim yapılabilmesi için bu zorunludur.
- Veli onayı akışı; onay kaydı politika sürümüyle birlikte tutulur (madde 11).
- Öğrenci hesapları varsayılan olarak dışa kapalıdır: kurum dışı içerik, kurum dışı kişi, kurum dışı kaynak yok (madde 27).

**24. Kamu uyumluluğu. [K]**
Mimari, kamu kurumlarının güvenlik şartlarına (ör. Cumhurbaşkanlığı Dijital Dönüşüm Ofisi **Bilgi ve İletişim Güvenliği Rehberi**) göre tasarlanır; bağımsız güvenlik denetimi hedeflenir.
**Her kurum (okul, il, bakanlık) sistemi kendi sunucusunda çalıştırabilir**; veri Türkiye'de kalır (madde 31). Kurum kurulumu için tek komutla ayağa kalkan bir dağıtım sağlanır: container/compose + kurulum rehberi + yedekleme ve geri yükleme prosedürü + güncelleme prosedürü. **Kurumda uzman BT personeli olduğu varsayımı zayıf tutulur;** kurulum bir bilgisayar öğretmeninin yapabileceği kadar basit olmalıdır.
Benimseme yolu: tek okulla pilot → il müdürlüğü → bakanlık. Her adımda referans dokümanı ve yazılı pilot raporu üretilir.

**25. Edu'da kurum içi materyal paylaşım motoru. [K — mimari basitleştirildi]**
Hedef: bir okulun materyali kurum ağı içinde, dış dünyaya hiç çıkmadan, dar bant genişliğinde dağıtabilmesi.

**Ana yol HTTP'dir.** Kurum sunucusu her zaman bir kaynak olarak bulunduğu için (öğretmen uygulamayı kapatsa da paylaşım durmaz), v1'de dağıtım **kurum sunucusundan HTTPS range-request indirme** ile yapılır.

Bu tek karar, Edu'nun ilk sürümünden şunların tamamını çıkarır: özel torrent formatı, eş koordinasyonu, tracker işletimi, eşler arası IP görünürlüğü sorunu ve ona bağlı KVKK dokümantasyonu. Kurum sunucusu zaten seed olduğu için P2P'nin kazancı yalnızca bant genişliğidir — ve o kazanç v1'i geciktirmeye değmez.

**P2P, opsiyonel hızlandırıcıdır. [A]** Bant genişliği kısıtlı okullar için, **kurum ayarıyla açılan** bir eş dağıtımı eklenir. Açıldığında geçerli kurallar: açık DHT yok, açık tracker yok, rastgele magnet linki girme alanı yok, yalnızca kurumun imzaladığı paketler, eşleri yalnızca kurumun kendi sunucusu koordine eder, eşleşme **varsayılan olarak okulun yerel ağıyla sınırlı**. Aynı ağdaki eşlerin birbirinin IP adresini görmesi KVKK dokümantasyonunda belirtilir (madde 31).

> **Teknik not:** BEP 27 "private" bayrağı bir **güvenlik sınırı değildir** — yalnızca uyumlu istemcilerde DHT/PEX'i kapatır. Güvenlik imzadan (madde 34) ve kurum koordinasyonundan gelir, bayraktan gelmez. v0.1'de bu bayrak bir güvenlik özelliği gibi okunuyordu.

Arayüzde ve dokümantasyonda bu bileşen **"materyal paylaşım motoru"** olarak tanıtılır.

**26. Özellik ayrımı. [K]**
- *EON Stream:* görüntüleyici, eklentiler, marketplace, creator'lar, materyal paketleri. Okul sistemleri **bulunmaz**.
- *EON Edu:* görüntüleyici (core'dan), materyal paketleri, öğretmenler için creator'lar, madde 17–21, 23–25 ve 27. Açık marketplace ve URL ile eklenti **bulunmaz** (madde 22).

**27. Edu kısıtlama modeli. [K]**
Varsayılan kural: **izin verilmeyen her şey yasaktır.**

- **a) Politika hiyerarşisi.** İzin listesini bakanlık belirler; il ve okul listeyi yalnızca **daraltabilir**, genişletemez.
  **Çakışma ve kazanılmış hak kuralı (v0.1'de eksikti):** Üst kademe listeyi daralttığında, daha önce paylaşılmış ve artık izin dışı kalan materyal **silinmez**. "Arşiv" durumuna geçer: yeni paylaşıma kapanır, mevcut erişim belirlenmiş bir geçiş süresi boyunca sürer, öğretmene bildirim gider. Politika sürümü ve geçiş tarihi kayda yazılır (madde 11). Aksi halde bir politika güncellemesi, dönem ortasında öğretmenlerin ders materyalini bir gecede yok eder.
- **b) Öğretmen paylaşımı.** Öğretmenler kendi bilgisayarlarındaki materyali paketleyip sınıf veya okulla paylaşabilir. Paylaşım anında hak beyanı ve lisans alanı gelir; madde 12'deki bildirim sistemi okul düzeyinde de çalışır (madde 12h). Yalnızca izinli formatlar kabul edilir (madde 15b) ve dosyalar **kurum sunucusunda zararlı yazılım taramasından** geçer (ör. ClamAV). Tarama sonucu beklemedeyken paket yayınlanmaz.
- **c) Marketplace yoktur.** Resmi ve onaylı kaynaklar **kurum kataloğunda** listelenir; bunun dışında kaynak eklenemez. Katalog girişleri kurum tarafından imzalanır (madde 34).

**28. Edu → EON Stream paket uyumu. [K]**
EON Edu'dan indirilen materyal paketleri EON Stream'nin paket görüntüleyicisiyle de açılabilir; iki uygulama **aynı paket formatını** kullanır. Bu, "aynı isim / tek core" kararının kullanıcıya dönük en somut faydasıdır: öğrenci okulda indirdiğini evde açar, öğretmen tek bir creator öğrenir.

- **a) Görüntüleme serbest, yeniden yayınlama lisansa bağlı.** Edu paketleri "kurum paketi" işareti ve madde 16e'deki lisans bilgisini taşır. EON Stream'nin creator'ı ve marketplace'i, lisans izin vermedikçe (ör. CC BY-SA) bu paketleri herkese açık olarak yeniden yayınlamaz. **Sınıfa özel materyal kamuya sızmaz** ve bu kural istemcide teknik olarak uygulanır — yalnızca politika metninde kalmaz.
- **b)** Edu'daki ilerleme, kimlik ve kurum verileri **pakete gömülmez** (madde 31).
- **c) Ters yön:** EON Stream'de hazırlanan paketler Edu'ya yalnızca kurum kataloğu onayıyla girer (madde 25, 27c).

---

## F. Proje yürütmesi (33–41)

**33. Lisans, yönetişim ve marka politikası. [A — en acil karar]**

v0.1'de "proje lisansı serbestçe seçilir" deniyor ama hiçbir yerde seçilmiyordu. Bu, **en çok aşağı akış etkisi olan ve hiç verilmemiş karardır**; Faz 0'ın ilk günü verilir, çünkü ilk dış katkıdan sonra değiştirmek her katkıcının onayını gerektirir.

Seçenekler ve sonuçları:

| Seçenek | Sonucu |
|---|---|
| **AGPL-3.0 / GPL-3.0** | Fork'lar açık kalır; bir eğitim yazılımı satıcısı core'u kapalı bir ürüne gömemez. Bedeli: bazı kamu/kurumsal satın alma süreçleri GPL'e çekingen yaklaşır; AGPL, kurum sunucusu tarafında ek soru doğurur. |
| **MPL-2.0** | Dosya düzeyinde copyleft, ortası. Core açık kalır, üzerine kapalı modül yazılabilir. |
| **Apache-2.0** | En geniş benimseme, açık patent hükmü. Kapalı fork'a izin verir. |

Not: madde 1'de (a) seçilir ve stremio-core (MIT) kullanılırsa uyum sorunu yoktur — MIT bu seçeneklerin tümüyle uyumludur. (b) seçilirse kısıt zaten kalkar.

Aynı maddeye bağlı, birlikte yazılan belgeler:
- **Katkı modeli:** DCO tercih edilir, CLA değil (CLA katkıyı caydırır, tek taraflı lisans değişikliği imkânı ise bu proje için gerekli değil).
- **Davranış kuralları** — madde 29'un 3. maddesini de kapsar.
- **Karar alma modeli:** kim birleştirir, kim sürüm yayınlar, anlaşmazlık nasıl çözülür, bakımcı nasıl eklenir/çıkarılır.
- **Marka politikası** (madde 32d): kod fork'lanır, isim fork'lanmaz.
- **Güvenlik açığı bildirim politikası** (`SECURITY.md`): iletişim adresi, yanıt süresi, makul açıklama süresi.

**34. İmza anahtarı yönetimi ve iptal. [K]**

v0.1'de imza her yerde geçiyordu (madde 3, 15c, 22, 25, 27c) ama anahtarların kim tarafından, nasıl tutulduğu hiç yazılmamıştı. Sözleşmeler yazılırken bu da yazılır.

- **Anahtar hiyerarşisi:** çevrimdışı kök anahtar → çevrimiçi imzalama anahtarları (sürüm, modül, Edu, kurum). **Edu'nun anahtarı EON Stream'den ayrıdır** (madde 22).
- **Kök anahtar** donanım anahtarında ve çevrimdışı tutulur; yalnızca ara anahtar imzalamak için kullanılır. Anahtar sahipliği tüzel kişilik kurulduğunda kuruma geçer (madde 30/L1) — kişiye bağlı kalması projenin tek kişiye bağımlılığını kalıcılaştırır.
- **Döndürme:** ara anahtarların geçerlilik süresi vardır; istemci **birden fazla geçerli anahtarı** tanıyacak şekilde yazılır, böylece döndürme sırasında kimse kilitlenmez.
- **İptal (revocation) — v0.1'de tamamen eksikti.** Kötü niyetli veya güvenliği bozulmuş bir eklenti/modül **sürümü**, istemcilerin reddedeceği şekilde işaretlenebilir: imzalı bir iptal listesi, istemcinin düzenli aralıklarla çektiği bir uçtan yayınlanır; çekemediğinde son bilinen listeyi kullanır (fail-closed değil, fail-last-known). Kapsam **sürüm bazındadır** — tüm eklentiyi değil, kötü sürümü iptal et.
- **Acil durum prosedürü:** kim, hangi gerekçeyle, kaç kişinin onayıyla iptal yayınlayabilir. İptal kararı da denetim kaydına girer (madde 6).

**Uygulandı (v0.5).** Hiyerarşinin istemci tarafı yazıldı ve çalışıyor: `eon-stream-core/src/signature.rs` (Ed25519 doğrulama, geçerlilik pencereli çok anahtarlı güven kümesi, derleme profiline göre anahtar amacı), `src/revocation.rs` (sürüm bazlı iptal, son bilinen liste, geri alma reddi, tazelik), `src/modules.rs` (kurulu modüllere uygulama).

Üç şey karara bağlandı:

1. **Kanonik serileştirme: JCS (RFC 8785).** Sözleşmede açık bırakılmıştı ve yanında şu uyarı duruyordu: iki uygulama bayt sırasında anlaşmazsa, bir istemcide doğrulanan imza diğerinde başarısız olur. Bu uyarı doğruydu, dolayısıyla **hiçbir anahtar yokken** sabitlendi — en ucuz an buydu. İmzalanan nesne üç dizge alanından oluştuğu için kanonik biçim tek bir satır: `{"id":...,"sha256":...,"version":...}`. Herhangi bir uygulama bunu dizge birleştirmeyle üretebilir.
2. **Doğrulayıcı konuyu (subject) zarftan okumaz.** Kendi okuduğu manifestten ve kendi hesapladığı özetten yeniden kurar; böylece uydurulmuş bir subject alanının tutunacağı bir yer kalmıyor.
3. **`eon-stream-core` doğrular, asla imzalamaz.** Bağımlılığın imzalama yarısı kapatıldı: aynı zamanda imzalayabilen bir istemci, ele geçirildiğinde geçerli eser üreten bir istemcidir.

**Durum: hiçbir anahtar yok.** Hiyerarşide tek bir anahtar üretilmedi. Sahiplik tüzel kişiliğe ait (madde 30) ve o kurulmadan kök anahtarı bir kişinin dizüstünde tutmak, hiç anahtar olmamasından **daha kötü** olurdu — gerçeğine benzeyen imzalar üretirdi. Dolayısıyla her yapı **boş bir güven kümesiyle** dağıtılıyor ve boş güven kümesi hiçbir şey kurmuyor. İstemci bunu belirsiz bir hatayla değil, açıkça söylüyor.

Mekanizma kuramsal değil, çalışır durumda: istemci güven kümesini çalıştırılabilir dosyanın yanındaki `eon-trust.json`'dan okuyor, yani isteyen bir anahtar üretip bir modül imzalayıp zinciri uçtan uca işletebiliyor (imza doğrulandı, içerik özeti yeniden hesaplandı, izinler gösterildi, oynanmış içerik reddedildi, sürüm düşürme reddedildi). Bu dosya tek bir makinenin neye güvendiğinin beyanıdır — kurumun kendi anahtarını kaydettireceği mekanizmanın aynısı. Doğrulamayı atlayan bir bayrak **yok**.

**35. Erişilebilirlik. [K]**

Kamu eğitim ürünü için pazarlık konusu değildir ve v0.1'de tek satır yoktu. **v1'in çıkış ölçütlerine dahildir** (bkz. v1 tanımı, madde 8):

- Tüm arayüz **klavye ile** kullanılabilir; odak göstergesi görünür; klavye tuzağı yok.
- Ekran okuyucu ile temel akışlar (eklenti ekle, içerik bul, oynat, altyazı seç) tamamlanabilir; anlamlı etiketler ve rol işaretlemesi.
- Metin/arka plan kontrastı **WCAG AA**; kullanıcı yazı tipi boyutunu büyütebilir ve arayüz bozulmaz.
- Oynatıcıda altyazı desteği, altyazı boyutu/rengi ayarı; sesli betimleme parçası varsa seçilebilir.
- Materyal paketlerinde erişilebilirlik alanları: altyazı dosyası, metin alternatifi, transkript. Creator bunları **teşvik eder** ve eksikse uyarır (zorunlu tutmaz — zorunluluk paylaşımı durdurur).
- Hareket/animasyon azaltma tercihine uyum.

**36. Telemetri ve ölçüm. [K]**

**Varsayılan: telemetri yok.** Uygulama, kullanıcı açıkça istemedikçe hiçbir kullanım verisi göndermez. Çökme raporu da **opt-in**'dir ve gönderilecek içerik gönderilmeden önce kullanıcıya gösterilir. Hiçbir koşulda ne izlendiği, hangi eklentinin kullanıldığı veya içerik başlıkları gönderilmez. **Edu'da telemetri derleme aşamasında hiç bulunmaz** (madde 22'deki CI kontrolüne dahil edilir).

Bunun karşılığı: ürün kararları için sayısal veri olmayacak. Bu **bilinçli bir takastır**; geri bildirim gönüllü kanallardan (forum, anket, pilot raporu, kurum görüşmesi) toplanır. Madde 13'ün 2. sorusu (trafik kaydı yükümlülüğü) bu duruşla uzlaştırılacak tek noktadır.

**37. Tehdit modeli. [A — Faz 0 çıktısı]**

v0.1'de güvenlik özellikleri vardı ama düşman tanımı yoktu. Faz 0'da kısa bir tehdit modeli dokümanı yazılır; her düşman için hangi maddenin savunma olduğu eşlenir. Bir düşmanın karşısı boşsa, orada bir plan eksiği var demektir.

| Düşman | Amacı | Savunma |
|---|---|---|
| Kötü niyetli eklenti yazarı | Cihazda kod çalıştırmak, veri sızdırmak | 4 (bildirimsel/sandbox), 5, 34 (iptal) |
| Kötü niyetli paket | Zararlı dosya dağıtmak | 15b (içerik tipi), 27b (tarama), 34 |
| Sahte müdür / öğretmen | Öğrenci verisine erişmek | 18 (doğrulama + pilot sınırı), 17, 21 |
| Kötü niyetli okul yöneticisi | Yetkisiz gözetim | 23 (bildirimli denetim, gizli değil), 31 |
| Öğrenci | Kısıtlamayı aşmak, materyali sızdırmak | 22 (derlemede yok), 27, 28a |
| Telif trolü | Meşru/rakip içeriği kaldırtmak | 12e, 12f (sayaç, şeffaflık) |
| Tedarik zinciri saldırısı | Yayınlanan sürüme kod sokmak | 22 (derleme kanıtı), 34, bağımlılık sabitleme |
| İdari/yasal baskı | Projeyi durdurmak | 29 (duruş), 30 (tüzel kişilik), 24 (yerinde kurulum) |

**38. Sürdürülebilirlik ve finansman. [A]**

Kâr amacı gütmeyen bir yapının bile maliyeti vardır: marketplace backend, imzalama altyapısı, moderasyon emeği, LLM tarama maliyeti (madde 6), alan adı, **kod imzalama sertifikaları**, hukuki inceleme (madde 13), bağımsız güvenlik denetimi (madde 24). Madde 10 ticareti erteliyorsa bu maliyet nereden karşılanacak? Cevapsız bırakılırsa proje, para bittiğinde değil **moderasyon emeği bittiğinde** durur.

Değerlendirilecekler: bireysel ve kurumsal bağış; kamu/vakıf hibeleri (**NLnet / NGI**, TÜBİTAK programları, Prototype Fund benzeri); kurum destek sözleşmeleri (Edu kuran okul/il için ücretli kurulum ve destek — yazılım açık kalır, hizmet ücretlidir); uzun vadede marketplace komisyonu (madde 10, L2).

Tüzel kişilik tipi bu maddeyle **birlikte** seçilir:

| Tip | Artı | Eksi |
|---|---|---|
| Dernek | Bağış toplamaya uygun, kâr amacı gütmeme algısı güçlü | Kamu satın alma ve fatura tarafı zayıf |
| Şirket | Fatura, komisyon, kamu satın alma kolay | "Kâr amacı gütmeyen" algısı zedelenir |
| Yurt dışı mali ev sahibi | Hızlı başlangıç, düşük idari yük | Türkiye'de kamu satın alma ve yerel veri iddiası için yetersiz |

Karar, madde 13'ün 7. sorusuyla birlikte verilir.

**39. Güncelleme güvenliği. [K]**

Otomatik güncelleyici, saldırı yüzeyinin en değerli parçasıdır — bir kez ele geçirilirse tüm kullanıcı tabanına kod dağıtır.

- Güncelleme manifesti ve dosyaları imzalıdır; istemci imzayı doğrulamadan hiçbir şey yüklemez (madde 34).
- **Geri alma (rollback) saldırısına** karşı sürüm numarası geriye gitmez; istemci eski sürüme düşürülemez.
- **Aşamalı yayın** (kademeli yüzde) ve **acil durdurma**: hatalı sürüm yayınlandığında dağıtım tek yerden durdurulabilir.
- Kullanıcı otomatik güncellemeyi kapatabilir; kapalıyken güvenlik güncellemeleri ayrıca ve görünür şekilde bildirilir.
- **Edu'da güncelleme kanalı kurumdan yönetilir:** kurum bir sürümü onaylamadan okul cihazlarına inmez (madde 22). Okul, dönem ortasında sürprizle karşılaşmaz.

**40. Yerelleştirme ve çevrimdışı çalışma. [K]**

- **i18n baştan:** hiçbir kullanıcıya görünen metin koda gömülmez; çeviri dosyaları ayrıdır. v1'de Türkçe + İngilizce. Sağdan sola diller için yerleşim varsayımı yapılmaz (ileride RTL eklenebilsin).
- Tarih, sayı ve saat biçimleri yerelleştirilir. **Türkçe karakter sıralaması ve i/İ büyük-küçük harf dönüşümü** doğru yapılır — arama ve sıralamada en sık yapılan hata budur ve eğitim içeriğinde ("İstanbul", "ışık") hemen görünür.
- **Çevrimdışı birinci sınıf gereksinimdir:** materyal paketleri indirildikten sonra internetsiz tam çalışır (madde 16h); eklenti listesi, ayarlar ve ilerleme yereldir; ağ yoksa arayüz hata ekranı değil **"çevrimdışı" durumu** gösterir. Edu'da bu varsayılan senaryodur, kenar durum değil.

**41. Kapsam dışı (non-goals). [K]**

Bu boyda bir planda kapsamı tutan şey yapılacaklar listesi değil, **yapılmayacaklar** listesidir. Aşağıdakiler bilinçli olarak yapılmaz; birini yapmak istersek plan güncellenir, sessizce eklenmez:

- İçerik barındırmak veya bir içerik kataloğu / arama motoru işletmek.
- Kendi video kodek veya oynatıcı motorunu yazmak (mpv kullanılır).
- Stremio'nun hesap sistemi, kütüphane senkronizasyonu veya bulut servisleriyle entegre olmak.
- Kendi eklenti protokolünü icat edip Stremio uyumluluğunu bırakmak.
- iOS (App Store kısıtları ayrı bir projedir), Android dışındaki akıllı TV platformları (Tizen, webOS), tarayıcı sürümü. Android TV v0.14'te bu listeden çıktı (madde 2).
- EON Stream'de okul araçları; Edu'da açık marketplace (madde 26).
- Sosyal ağ özellikleri (takip, profil akışı, birebir mesajlaşma) EON Stream tarafında.
- DRM'li ticari akış servisleri desteği.
- Müzik modülü **şimdilik** — askıda, kaldırılmış değil. *(Kayıt, v0.10: artık EON Stream'in modülü olarak değil, ayrı bir ses ürünü olarak; madde 45a.)*

**42. Çatının geleceği: EON'un diğer ürünleri. [E]**

Çatı markası (madde 32), ileride EON altına başka ürünlerin gelebilmesi için ürün adından geniş tutuldu. Bilinen niyetler: **kendi kernel'i** ve **kendi Linux dağıtımı**.

Bunlar bu planın kapsamında **değil** ve bu maddenin amacı onları planlamak değil, iki şeyi bugünden korumaktır:

- **a) Adlandırma alanı ayrılmıştır.** İsimlendirme kalıbı `eon-<ürün>-<bileşen>` (madde 32a). Gelecek bir ürün, mevcut depoların hiçbirini yeniden adlandırmaya gerek duymadan bu kalıba oturur. `eon-docs` ve `.github` bilinçli olarak ürün adı taşımaz; ilk günden çatı düzeyindedir.
- **b) Çatı düzeyindeki kararlar ürüne özgü yazılmaz.** Marka politikası (madde 32e), yönetişim, güvenlik bildirim süreci, davranış kuralları, imza kök anahtarı hiyerarşisi (madde 34) ve EON Modül ABI İstisnası çatı adına yazılır. Böylece ikinci ürün geldiğinde yeniden yazılmaları gerekmez — bunlar sonradan uyumlulaştırılması en pahalı belgelerdir.
- **c) Kaynak taahhüdü yoktur.** Bu madde **[E]** etiketlidir ve öyle kalacaktır. EON Stream v1 çıkmadan (madde 41'deki v1 tanımı) hiçbir kernel veya dağıtım işine kaynak ayrılmaz. Çatının varlığı bir vaat değil, bir kapı. Tek kişilik bir ekibin ikinci ürüne başlaması, birincinin bitmemesinin en yaygın sebebidir.

**43. EON Office: üçüncü ürün ailesi, EON Stream v1'den önce. [K — v0.8'de eklendi]**

**Karar — proje sahibinin kararı (19 Kasım 2026):** EON Office bugün başlıyor, EON Stream `v1` beklenmeden. İlk iş uygulamalar değil, **ortak temel**: `eon-office-spec` (sözleşmeler), `eon-office-core` (yerel çekirdek hizmeti — *19 Kasım kaydıyla: bir kütüphane, bkz. Teknoloji*) ve `eon-office-sdk` (uygulamaların bağlandığı istemci). Uygulamalar ancak core tamamlanma ölçütlerini karşıladıktan ve yol haritası (`eon-docs/office/roadmap.md`) yazıldıktan sonra, oradaki sırayla başlar.

**Madde 42c ile ilişkisi, açıkça.** 42c kernel ve dağıtım için yazıldı; Office onun kapsamında değil. Ama 42c'nin gerekçesi — *tek kişilik bir ekibin ikinci ürüne başlaması, birincinin bitmemesinin en yaygın sebebidir* — Office'e kelimesi kelimesine uyuyor. Bu madde o gerekçeyi çürütmüyor; **bilerek karşısına yazılıyor** ve bedelini aşağıda sayıyor.

**Maliyeti, açıkça:**

- **EON Stream `v1` gecikir.** `v1`'e kalan kurulum paketi, imzalama ve arayüzün klavye/ekran okuyucu yarısı (madde 35) Office ile aynı saatlerden pay alır. Office çalışması bu işlerden önce gelmez: bir `v0.X` sürümü ile Office çalışması çakışırsa sürüm kazanır.
- **Bakım yüzeyi büyür.** Bugün üç yeni depo; yol haritasında yirmiden fazla uygulama. "Boş depo açılmaz" kuralı (aşağıda) bu yükü sınırlamak için var.
- **Office'in bulut tarafı madde 30'a tabidir.** Senkron sunucusu, kurum hesapları, e-posta, toplantı — bunların hepsi kişisel veri işlemek demektir. Proje, Faz L1 kapısı açılmadan **hiçbir Office hizmetini kendisi işletmez**; yerel ve kendi sunucusunda kurulum (self-hosted) önce gelir. "EON bulutu" seçeneği bu kapıya kadar yoktur.
- **Microsoft eşdeğerliği bir yöndür, taahhüt değil.** Yol haritası her ürünü ölçülebilir kabul ölçütüne bağlar; belgelenmiş ama yazılmamış bir özellik hiçbir tabloda "hazır" sayılmaz.

**Sınırlar (bağlayıcı):**

- **a) Ayrı core.** `eon-office-core`, `eon-stream-core`'a bağımlı değildir ve tersi de yoktur. Paylaşılan şey kod değil, **çatı düzeyindeki kararlardır** (madde 42b): imza ve iptal hiyerarşisi (34 — Office için ayrı anahtar amacı), telemetri yok (36), asgari veri (31), erişilebilirlik (35), yerelleştirme ve çevrimdışı (40), güncelleme güvenliği (39).
- **b) Her uygulama ayrı süreç, ayrı depo.** *(19 Kasım kaydı: protokol ve hizmet kalktı; uygulamalar core kütüphanesini bağlar ve ortak klasör, `eon-office://` bağlantıları ve yayımlanmış özetler üzerinden buluşur — bkz. Teknoloji ve `office/design/2026-11-19-library-core.md`. Aşağıdaki cümle ilk tasarımı anlatır.)* Uygulamalar core'a belgelenmiş, sürümlü bir protokolle (yerel JSON-RPC) bağlanır; birbirinin iç yapısına değil, birbirinin **yayımladığı yöntemlere** core üzerinden ulaşır. Bir uygulamanın çökmesi diğerini düşürmez — bu bir söz değil, süreç sınırının sonucudur ve testle gösterilir.
- **c) Uygulamaya özgü işlev core'a girmez.** Core'da "not", "hücre", "e-posta" diye bir kavram yoktur; depolama, olay, arama, bildirim, zamanlama ve erişim denetimi vardır.
- **d) Boş depo açılmaz.** Bir uygulamanın deposu, o uygulamanın ilk çalışan sürümüyle birlikte açılır. Yol haritasında adı olmak depo açmak için yeterli değildir.
- **e) Yapay zekâ isteğe bağlıdır.** Hiçbir uygulamanın temel işlevi bir yapay zekâ sağlayıcısına bağlı olamaz. Varsayılan sağlayıcı **yoktur**; kullanıcı seçer (yerel model dahil). Sağlayıcıya giden her veri, gitmeden önce kullanıcıya gösterilebilir olmalıdır (madde 36'nın çökme raporu kuralıyla aynı ilke). Bir ajan, uygulamanın izin sistemini atlayamaz.
- **f) Güvenlik iddiası testle yapılır.** EON Security / Defender sınıfı ürünler kendi tespit motorunu iddia etmez; yerleşik ve denetlenmiş araçlarla (ör. işletim sisteminin kendi koruması, ClamAV, osquery) bütünleşir. Uzaktan erişim ürünleri, cihaz başındaki kişinin açık ve oturum başına onayı olmadan çalışmaz.

**Marka (madde 32 ile uyum):** "EON Office" ürün ailesinin adıdır; uygulamalar kullanıcıya **EON Notes**, **EON Writer** gibi görünür, depoları `eon-office-<bileşen>` kalıbını izler (`eon-office-notes`). Çatı-ürün-bileşen sırası korunur: ürün *Office*, bileşen *notes*. **[A]** "EON Notes" gibi adların ayrı ayrı marka ve alan adı müsaitliği madde 32g ile birlikte kontrol edilir.

**Lisans (madde 33'ün kalıbı, onayı madde 33'e bağlı):** spec `Apache-2.0`, core `GPL-3.0-or-later` + EON Modül ABI İstisnası, SDK `MIT`, sunucu `AGPL-3.0-or-later`, uygulamalar `GPL-3.0-or-later`.

**Teknoloji:** Rust; masaüstü uygulamalarda Tauri (madde 2'nin gerekçesi aynen geçerli). **`eon-office-core` bir kütüphanedir** — `eon-stream-core` gibi, uygulamaların bağladığı kodun çekirdeği; çalışan bir program değildir ve GUI bağımlılığı taşımaz. Her uygulama tek başına çalışır; ortak ihtiyaçlar (arama dizini, tercihler, uygulama kaydı, son kullanılanlar, yayımlanmış veriler) bilinen bir klasördeki paylaşılan dosyalardan, eylemler `eon-office://` bağlantılarından, bildirimler işletim sisteminin bildirim merkezinden geçer.

> **Kayıt (19 Kasım 2026):** İlk tasarımda core, her uygulamanın bağlandığı yerel bir hizmet süreciydi; Notes'u çalıştırmak için bile core ve launcher gerekiyordu. Proje sahibi bunu reddetti ("core bir app değil, kodların çekirdeği olmalıydı"; "gerçekten modüler mi?"). Dört yol karşılaştırıldı (hizmet süreci, Stream modeli kütüphane, tek kabuk + eklentiler, sunucu merkezli) ve Stream modeli seçildi; gerekçe ve bedel `eon-docs/office/design/2026-11-19-library-core.md`'de. Bedeli: bir uygulama kapalıyken onun hatırlatmaları çalmaz (isteğe bağlı bir yardımcı ileride ayrıca kararlaştırılır); merkezi izin denetimi yerine izinler kurulumda gösterilen ve kütüphanenin uyguladığı beyanlardır — ilk tasarımın kendi güvenlik belgesinin de kabul ettiği gibi, aynı kullanıcının programına karşı hiçbiri gerçek sınır değildi.

**Ölçüt:** EON Office uygulamalarından herhangi birini kullanan biri "Microsoft'unkinden iyi" diyebilmeli. Her uygulamanın kabul ölçütleri, Microsoft karşılığının herkesçe sevilen özelliklerini ve en çok şikâyet edilen yanlarının düzeltilmesini içerir.

**44. EON Download Manager: bireylere ücretsiz bir indirme yöneticisi. [K — v0.9'da eklendi]**

Proje sahibinin kararı (20 Kasım 2026): EON'un bir sonraki ürünü, Internet Download Manager'ın (IDM) sevilen her şeyini yapan, **bireysel kullanıcıya tamamen ücretsiz**, açık kaynak ve arayüzü güzel bir indirme yöneticisi. Araştırma ve analiz: `download-manager/research.md`; plan: `download-manager/plan.md`.

- **a) Kapsam.** Dinamik segmentasyon ve bağlantı yeniden kullanımı, her şeyden sonra kaldığı yerden devam, adres yenileme, tarayıcıdan devralma (eklenti + native messaging), sayfanın kendi yüklediği videoyu (HLS, doğrudan medya) indirme, kategoriler, kuyruk, zamanlayıcı, pano izleme, hız sınırı, SHA-256 doğrulama. Windows, Linux, macOS.
- **b) İçerik kaynağı değil, araç.** Uygulama içerik barındırmaz, içerik kaynağıyla gelmez ve kullanıcı adına içerik aramaz; kullanıcının verdiği bağlantıyı indirir. Bir bağlantı dosya yerine bir sayfaysa, sayfanın oynattığı video ya da ses genel amaçlı açık kaynak yardımcı programlarla (bir çıkarıcı ve FFmpeg) indirilebilir. Bunlar uygulamaya gömülmez: ilk gerektiğinde kendi projelerinin yayın sayfasından indirilir, aynı yayının yayımladığı SHA-256 ile eşleşmezse silinir ve çalıştırılmaz. **DRM'li ya da korumalı akış reddedilir; DRM aşılmaz.** Tanıtımda, ekran görüntülerinde ve belgelerde hiçbir platform adı geçmez (madde 32b'nin duruşu). Madde 9'un ruhu burada da geçerli: EON bir araç dağıtır, içerik kaynağı dağıtmaz.
  > **Kayıt (v0.12, 21 Kasım 2026):** v0.9'da "siteye özgü çıkarıcı yoktur" yazıyordu. Proje sahibinin kararıyla, sayfanın oynattığı video ve ses de indirilebilir; hukuki tarafı sahip üstleniyor ve madde 13'teki avukat sorularına eklenir. **Maliyet:** (1) çalışma anında indirilen iki yardımcı program bir tedarik zinciri bağımlılığıdır; aynı yayının sağlama toplamı bozulmaya karşı korur, ele geçirilmiş bir yayına karşı korumaz (madde 37'deki tehdit modeline yazılır); (2) sayfalar değiştikçe çıkarıcı bozulur, bu yüzden haftada bir yenilenir ve bir sürüm bozulursa eskisi tutulur; (3) bu yetenek bir platformun kullanım koşullarıyla çatışabilir ve bu risk tanıtım yapılmamasıyla azalır, ortadan kalkmaz.
- **c) Para modeli.** Bireyden para istenmez, hiçbir zaman. Kurumlara ileride ömür boyu lisans satılabilir; bu iş `eon-download-manager-marketplace` deposunda, sonra yapılır. **Uygulama deposunda abonelik veya lisans kodu bulunmaz** — devre dışı bırakılmış hali de. Bu kural madde 36'daki telemetri kuralı gibi okunur: yokluk, kapalılık değil.
- **d) Depolar.** `eon-download-manager-spec` (Apache-2.0: tarayıcı devir mesajları, devam durumu dosyası), `eon-download-manager-app` (GPL-3.0-or-later + EON Modül ABI İstisnası: motor kütüphanesi, pencere, tarayıcı eklentisi). Marketplace deposu ilk çalışan sürümüyle açılır, boş depo açılmaz.
- **e) İlk alfa `v0.0.0`'dır.** Sonraki numaralandırma sahibin tercihiyle belirlenir.

**Maliyet.** Bu, EON Stream v1'den önce açılan **üçüncü** ürün ailesidir (madde 43'ten sonra) ve madde 42c'nin uyardığı riski bilerek büyütür: tek kişilik bir ekipte her yeni ürün, öncekilerin bitmesini geciktirir. EON Office uygulamaları sahibin değerlendirmesiyle şimdilik bekletiliyor; EON Stream'in kalan `v1` işleri (kurulum paketi, imza anahtarları) bu süre boyunca ilerlemez. Kazanç: geniş bir kitleye hemen dokunan, EON'un ilkelerini (ücretsiz, açık, telemetrisiz) somut gösteren bir ürün; tarayıcı eklentisi ve native messaging altyapısı EON Stream'in ileride ihtiyaç duyacağı bir parçadır. Tarayıcı mağazalarında yayın sahibin geliştirici hesaplarını, imzalı dağıtım ise madde 30 ve 34'ü bekler; o zamana kadar eklenti elle yüklenir ve Windows'ta Akıllı Uygulama Denetimi imzasız indirmeleri engelleyebilir.

**45. Her hizmet ayrı ürün: akış, dosya ve ses ayrı; her birinin kendi deposu, core'u ve uygulaması. [K — v0.10'da eklendi]**

Proje sahibinin kararı (20 Kasım 2026): planın ilk hâlinde EON Stream, video, dosya ve müziği tek uygulamada modül olarak topluyordu (madde 3; Öncelik sırası "Video → Dosya → Müzik"). Bu artık geçerli değil. **Dosya, ses ve akış ayrı hizmetlerdir**: her biri ayrı ürün, ayrı depo seti (`-spec`, core ya da motor, `-app`), ayrı core, ayrı uygulama ve ayrı sürüm hattıdır. Sahibin ifadesiyle: "genel olarak verimlilik ve modülerlik taraftarıyım."

- **a) Hizmetler.**
  - **Akış (video): EON Stream** — `eon-stream-*`. Stremio eklentileri, torrent akışı, mpv. EON Edu bu core'un üzerinde kalır; madde 22 ve 28 değişmez.
  - **Dosya: EON Download Manager** — `eon-download-manager-*` (madde 44). EON Stream'in planlanan "dosya modülü" buraya taşınır; Faz 2'deki dosya modülü kalemi düşer.
  - **Ses:** ayrı bir ürün. Adı ve kapsamı **[A]**, kendisi **[E]**. Madde 41'deki askıdaki müzik modülünün yerine geçer; artık Stream'in modülü değil, kendi ürünüdür. Adı ve kapsamı yazılmadan deposu açılmaz (43d).
  - **Üretkenlik: EON Office** (madde 43), zaten kendi core'uyla.
- **b) Core paylaşımı yalnızca verimliyse.** Bir ürün başka bir ürünün core'unu ancak kodu çoğaltmaktan daha az iş ve daha az risk getiriyorsa kullanır. Paylaşım bir bağımlılık kararıdır; gerekçesi, bağımlılığın beyan edildiği yerde yazılır (madde 37). Varsayılan ayrılıktır: **hiçbir core başka bir ürünün ihtiyacı için genişletilmez** — ihtiyaç varsa ihtiyaç duyan ürün kendi koduna yazar ya da paylaşılabilir küçük bir kütüphane ayrı depo olarak çıkar.
- **c) Ortak olan kod değil, sözleşme ve çatı kararlarıdır:** lisans kalıbı (33), telemetri yokluğu (36), imza hiyerarşisi (34), erişilebilirlik (35), yerelleştirme (40), adlandırma (32a, 42a), "boş depo açılmaz" (43d), asgari veri (31).
- **d) Madde 3 her ürünün kendi içinde geçerlidir.** EON Stream'in core'u modül yöneticisi olarak kalır; ama video dışındaki türler artık Stream modülü değildir.
- **e) Bir ürünün kullanıcısı diğerini kurmak zorunda değildir.** Ürünler birbirine bağlanacaksa (ör. Stream'deki bir bağlantıyı EON Download Manager'a vermek), bu belgelenmiş bir sözleşmeyle olur — ortak bir süreçle ya da birbirinin iç yapısına bağlanarak değil.

**Maliyet, açıkça:**

- Altyapının bir kısmı — güncelleyici, imza doğrulama, ayarlar, yerelleştirme, pencere iskeleti — ürün başına yeniden yazılır ya da kopyalanır. (b) bunu sınırlar, sıfırlamaz. Bir güvenlik düzeltmesi birden fazla depoda yapılmak zorunda kalabilir; bu yük, ortak sözleşmeler ve aynı CI kalıpları ile hafifletilir ama ortadan kalkmaz.
- Depo sayısı artar (bugün 21). Her birinin CI'ı, lisans kapısı ve sürüm hattı bakım ister; tek kişilik ekipte bu, madde 42c'nin uyardığı yükün parçasıdır.
- Madde 28'deki "tek motor, tek öğrenme" faydası yalnızca EON Stream ile EON Edu arasında kalır; dosya ve ses kullanıcıları bu ortaklıktan yararlanmaz.
- Kullanıcı için tek uygulama yerine birkaç uygulama: kurulum, tanıtım ve destek ürün başına yapılır.

**Kazanç:** her ürün tek başına anlaşılır, test edilir, sürümlenir ve gerekirse bırakılabilir; bir ürünün gecikmesi diğerini bekletmez; kullanıcı yalnızca ihtiyacı olanı kurar; bir ürünün mimari hatası diğerine sızmaz. EON Office'te "core bir uygulama değil, kodun çekirdeği" ilkesiyle (madde 43, 19 Kasım kaydı) zaten sınanmış yol budur.

**46. EON Account: bütün EON için tek, anonimlik öncelikli hesap. [K — v0.11'de eklendi]**

Proje sahibinin kararı (20 Kasım 2026): EON ürünleri için tek bir hesap. Araştırma ve analiz: `account/research.md`; plan: `account/plan.md`. Sahibin istekleri ve karşılıkları:

- **a) Anonimlik esas.** Hesap için kullanıcı adı ve parola yeter. E-posta ve telefon **isteğe bağlıdır**; eklenirse kurtarma ve giriş için kullanılır, sunucuda şifreli tutulur. Parola sunucuya hiç gitmez, özeti bile gitmez: giriş **OPAQUE** (RFC 9807) ile yapılır. Sunucu IP adresi, tarayıcı/istemci bilgisi ya da istek günlüğü yazmaz; hız sınırları yalnızca bellekte tutulur.
- **b) İlerleme ve puanlar sunucuda, ama okunamaz.** Misafirin her şeyi cihazında kalır. Giriş yapan kişinin izleme ilerlemesi ve puanları **cihazda şifrelenmiş bir kasada** sunucuya gider; anahtar parolasından ve yalnızca kendisinde olan bir kurtarma anahtarından türetilir. Sunucu kasanın yalnızca boyutunu ve son değişme zamanını bilir. Madde 31'deki "izleme geçmişi toplanmaz" kuralı bu yüzden bozulmaz (madde 31'deki kayıt).
- **c) Kurtarma, dürüstçe.** Kurtarma anahtarı varsa her şey korunur. Yalnızca doğrulanmış e-posta/telefon varsa parola yenilenir, ama kasa boş başlar. İkisi de yoksa hesap kurtarılamaz. Bu, kayıt ekranında hesap açılmadan önce söylenir.
- **d) Tek hesap.** Bir e-posta zaten bir hesaba aitse ikinci hesap açılmaz; o adrese gelen mesaj hangi hesapla giriş yapılacağını söyler. Yanıt herkese aynıdır, bu yüzden bir adresin kayıtlı olup olmadığı dışarıdan öğrenilemez.
- **e) Diğer hizmetler.** EON uygulamaları sunucuyla doğrudan konuşur. Başka bir hizmet (ör. başkasının işlettiği bir topluluk sunucusu) imzalı bir doğrulama alır (madde 34'ün biçimi: JCS üzerinde Ed25519). Bu doğrulamada her hizmete farklı bir takma kimlik yer alır; iki hizmet kullanıcılarını birbiriyle eşleştiremez. Kullanıcı adı yalnızca kişi o hizmetle paylaşmayı seçerse gider.
- **f) Misafir modu ve topluluk.** Uygulama ilk açılışta giriş ister; "misafir olarak devam et" her zaman seçeneklerden biridir (madde 7). Topluluk özellikleri hesap ister.
- **g) Şeffaflık.** Sunucu neyi, neden, ne kadar süre tuttuğunu ve kimin okuyabildiğini makinece okunur bir belge olarak yayınlar (`/v0/transparency`). Belge şemayla aynı sabitlerden üretilir; listede olmayan bir tablo testte hata verir. Her hesap tutulan her şeyi dışa aktarabilir ve kendini tamamen silebilir. **Uygulamalarımıza ait olmayan hiçbir şeyin verisi tutulmaz.**
- **h) Depolar.** `eon-account-spec` (`Apache-2.0`): sözleşme ve kriptografi. `eon-account-client` (`MIT`): her uygulamanın bağladığı kütüphane. `eon-account-server` (`AGPL-3.0-or-later`): referans sunucu.
- **i) Sunucu yazılır, işletilmez.** Madde 30'daki Faz L1 kapısı karşılanana kadar projenin işlettiği bir EON Account sunucusu **yoktur**. Uygulamalarda sunucu adresi boş başlar; kapı açıldığı gün varsayılan adres yazılır ve hesap hazırdır.
- **j) [A] 5651 ve trafik kaydı.** Sunucu bugün trafik bilgisi tutmayacak şekilde yazılır. Yer sağlayıcı sayılıp sayılmadığı ve hangi trafik bilgisinin saklanması gerektiği madde 13'teki avukat sorularına eklenir. Saklama gerekiyorsa bu bir plan kararıyla eklenir ve şeffaflık belgesinde yayınlanır; sessizce eklenmez.

**Maliyet, açıkça:**

- **Kurtarılamayan veri.** Parolasını unutan ve kurtarma anahtarı olmayan kişinin kasası geri gelmez; destek ekibi de getiremez. Anonimliğin bedeli budur ve kişiye önceden söylenir.
- **Denetlenmemiş sürüm.** `opaque-ke`'nin denetimi 2021'de, `v4`'ten önce yapıldı. Kriptografiyi kendimiz yazmıyoruz, ama kullandığımız sürüm denetlenmedi; bu, madde 37'deki tehdit modeline yazılır.
- **İstemci yükü.** Parola esnetme (Argon2id, 64 MiB) cihazda çalışır. Zayıf cihazda giriş bir saniyeye yaklaşabilir.
- **Yeni bir yüzey.** Hesap sistemi, veri koruma yükümlülüğünün fiilen başladığı yerdir (madde 7, 31). Sunucu yazılmış olsa da işletilmesi tüzel kişiliği, hukuki incelemeyi ve madde 31'deki aydınlatma ve imha metinlerini bekler.
- **Daha fazla iş.** Üç depo daha, ve EON Stream'in topluluk istemcisinin doğrulamalara taşınması. EON Stream'in `v1` işleri bu sürede yine ilerlemez (madde 44'teki maliyetle aynı).

**Kazanç:**

- Kişi kendini tanıtmadan, tek bir adla bütün EON'u kullanır.
- İlerlemesi cihazdan cihaza gider; bu sırada sunucu da onu işleten de ne izlediğini bilemez.
- Bir ihlalde çalınacak parola ya da izleme geçmişi yoktur.
- Topluluk sunucuları kendi parola veritabanlarını tutmak zorunda kalmaz.

**47. EON DNS: şifreli DNS, cihazda filtreleme; ardından kişinin seçtiği uca tünel. [K — v0.13'te eklendi]**

Proje sahibinin isteği (20 Kasım 2026): Cloudflare'in 1.1.1.1 / WARP uygulamasının yaptığı her şeyi yapan, daha iyi görünen bir ürün. Araştırma ve analiz: `dns/research.md`; plan: `dns/plan.md`.

- **a) Kopyalanamayan ve kopyalanmayan.** WARP'ın asıl varlığı Cloudflare'in küresel ağıdır. EON'un ağı yoktur ve madde 30'daki kapı açılmadan altyapı işletmez. Cloudflare'in WARP uçlarına üçüncü taraf bir istemciyle kaydolmak başkasının kullanım koşullarına dayanır; **EON DNS bunu yapmaz.**
  > **Kayıt (21 Kasım 2026, sahibin kararı):** Sahip Roblox'u test etti ve EON DNS ile giremedi. Türkiye'de Roblox, alan adına bakılarak (SNI/DPI) bağlantı düzeyinde engelleniyor; doğru IP'ye giden TLS el sıkışması 50–100 ms içinde kesiliyor (ölçüldü). Şifreli DNS bunu aşamaz; ClientHello'yu parçalamak bu ağda %50 tuttu. Sahip üçünü birden seçti: **Cloudflare WARP'ın ücretsiz ağı, kişinin kendi WireGuard uç noktası ve ileride EON sunucusu için altyapı.** Bu, (a)'daki "WARP uçlarına kaydolmayız" kararını değiştirir. **Bedeli:** EON, Cloudflare'in resmî olmayan bir istemcisi olur; Cloudflare bu kaydı, API sürümünü ya da koşullarını her an değiştirebilir ve EON DNS'in WARP kipi o gün çalışmayı bırakır. Kullanım koşulları riski sahibe aittir (madde 13'e eklenir). EON'un kendi sunucusu yine madde 30'u bekler.
- **b) Cihazda yapılan her şey, daha iyisiyle.** Herhangi bir çözümleyiciye (Cloudflare, Quad9, Mullvad, AdGuard, DNS0 ya da kişinin kendi sunucusu) DoH, DoT ya da DoQ ile şifreli DNS. Birden çok çözümleyici yarıştırılır. DNSSEC cihazda doğrulanır. Açık listelerle cihazda filtreleme yapılır: kötü amaçlı yazılım, reklam ve izleyici, yetişkin içerik; buna kişinin kendi kuralları eklenir ve her engelin sebebi gösterilir. Çözümleyici karşılaştırması bu bağlantıdan gerçek ölçümle yapılır. İki kip vardır: kendi yerel çözümleyicimiz ya da Windows 11'in kendi şifreli DNS'i.
- **c) Günlük cihazda ve kapalı başlar.** Telemetri yoktur (madde 36). Sorgu günlüğü yalnızca bellekte tutulur, varsayılan olarak kapalıdır, tek tuşla silinir. Cihazdan sorguların kendisinden başka hiçbir şey çıkmaz.
- **d) Hiçbir zaman DNS'siz bırakmaz.** Kapatma, çıkış ve çökme sonrası ilk açılış, ağ bağdaştırıcılarının önceki ayarını geri yükler.
- **e) Tünel ikinci dilimdir.** Tünel, kişinin getirdiği bir uca (kendi sunucusu ya da bir sağlayıcının WireGuard yapılandırması) gider ve 1.1.1.1 uygulamasında olmayan bir kill switch ile gelir. EON'un işlettiği bir uç yoktur.
  > **Kayıt (21 Kasım 2026):** Tünel ertelenmedi, öne alındı. Üç sağlayıcı aynı yapılandırmayı üretir: WARP (1.1.1.1 uygulamasının yaptığı gibi cihaz kaydı), kişinin `wg-quick` dosyası, ve bir EON tünel sunucusu (sözleşme ve istemci hazır, sunucu yok). Paketleri WireGuard'ın kendi imzalı çekirdek sürücüsü (`wireguard-nt`) taşır; ilk ihtiyaçta indirilir, sabit SHA-256 ile doğrulanır. Sistemi değiştirme yetkisini bir **EON DNS hizmeti** taşır (WARP'ın `warp-svc`'si gibi): bir kez tek izinle kurulur, sonra anahtar izin sormadan çalışır. Kill switch henüz yok; hizmet durursa trafik doğrudan gider (açıkça yazılı).
- **f) Depo.** `eon-dns-app` (`GPL-3.0-or-later` + EON Modül ABI İstisnası): motor kütüphanesi, pencere, tepsi. Kendine ait bir sözleşme doğduğunda `-spec`, EON'un işleteceği bir çözümleyici gündeme geldiğinde (madde 30'dan sonra) sunucu deposu açılır; boş depo açılmaz (43d).
- **g) İlk alfa `v0.0.0`'dır**: şifreli DNS yarısı.

**Maliyet:**

- Dördüncü ürün ailesi, EON Stream `v1`'den önce (madde 42c'nin uyarısı büyür).
- Sistem DNS'ini değiştirmek yönetici izni ister. Hatalı bir geri yükleme bilgisayarı ağsız bırakabilir; bu yüzden geri yükleme kendi başına test edilir ve çökmeden sonra onarılır.
- Hickory DNS'in kendi ikili dosyası "üretim için önerilmez" diyor. Kütüphanelerini kullanıyoruz ve test ediyoruz, ama olgunlaşmamış bir bağımlılık taşıyoruz.
- Filtre listeleri üçüncü tarafların emeğidir; yanlış engeller olur. Bu yüzden her engelin sebebi gösterilir ve tek tuşla izin verilir.
- Tünel bir sürücü (`wireguard-nt`) ve ayrı bir güvenlik incelemesi gerektirir; ayrı bir dilim olmasının sebebi budur.

**Kazanç:** WARP'ın tek şirkete bağlı kalan DNS yarısının açık, denetlenebilir ve kişinin kontrolünde bir karşılığı; engellenen her şeyin görünür olması; EON'un ilkelerini (telemetri yok, hesap gerekmez, kaynak açık) somut gösteren bir ürün daha.

---

**48. EON Converter: her dosyayı dönüşebileceği her biçime çeviren dönüştürücü; çekirdeği EON Download Manager'a da hizmet eder. [K — v0.15'te eklendi]**

Proje sahibinin isteği (22 Kasım 2026): "Aklına gelebilecek her türlü dosyayı dönüşebileceği her türlü dosyaya dönüştürebilen" bir uygulama. Örnekler: TXT'den PDF'e, PDF'ten Markdown'a hatasız, videodan GIF'e, GIF'ten videoya, "hepsi". Çekirdek modüler olacak ve sonra EON Download Manager'a eklenecek: bir şey indirilirken o dosyanın olabileceği bütün biçimler seçenek olarak sunulacak. İstenen biçim kaynakta yoksa kişiye bir kez sorulacak, sonra içerik en verimli dönüşen biçimde indirilip istenen biçime çevrilecek. Araştırma ve analiz: `converter/research.md`; plan: `converter/plan.md`.

- **a) Yönlendirici, motor değil.** Hiçbir tek program her şeyi çevirmez; ciddi her dönüştürücü bir yönlendiricidir. EON Converter dosyanın ne olduğunu içeriğinden anlar ve hedefe giden en ucuz dönüşüm zincirini bulur. Her adımı kanıtlanmış bir motor yapar: FFmpeg, Pandoc, Typst, PDFium, ImageMagick ve çekirdeğin kendi Rust dönüşümleri. LibreOffice ve Calibre kuruluysa onlar da kullanılır. "Her biçim", bu motorların okuyup yazabildiklerinin birleşimidir: birkaç yüz biçim.
- **b) Kayıp dürüstçe söylenir.** Bazı dönüşümler doğası gereği bir şey kaybeder. PDF bir sayfa tarifidir; Markdown'a çevrildiğinde metin, başlık ve listeler gelir, tablo ve sütunlar yaklaşık gelir. Taranmış PDF'te OCR olmadan metin yoktur. GIF 256 renktir. Uygulama "hatasız" sözü vermez; kayıp olan adımı çalıştırmadan önce gösterir.
- **c) Motorlar ilk ihtiyaçta indirilir, paketlenmez.** Her biri kendi projesinin yayın sayfasından gelir; sürümü ve SHA-256'sı koda sabitlenir (madde 37). Yalnızca resim ve veri çeviren biri hiçbir şey indirmez. Yönetici kurulumu isteyen LibreOffice ile Calibre indirilmez, yalnızca kuruluysa kullanılır.
- **d) Korumalı içerik çevrilmez.** DRM'li medya ve e-kitap reddedilir (madde 44b'nin ilkesi).
- **e) Depolar.** `eon-converter-core` kütüphanedir: biçim tablosu, içerikten tanıma, yönlendirme grafiği, motorlar ve iş takibi. Pencere bağımlılığı yoktur. `eon-converter-app` pencere ve komut satırıdır. İkisi de `GPL-3.0-or-later` + EON Modül ABI İstisnası taşır. Boş depo açılmaz (43d); ikisi de çalışan ilk sürümle açılır.
- **f) EON Download Manager ikinci dilimdir.** DM çekirdeğe git `rev` ile bağlanır. Ekleme penceresi, dosyanın olabileceği her biçimi listeler. İstenen biçim kaynakta yoksa şu sorulur: "Bu içeriğin özgün biçimi X. Y istiyorsunuz. Z olarak indirilip EON Converter tarafından otomatik dönüştürülecek. Devam edilsin mi?" Seçenekler Devam, İptal ve "Bir daha sorma"dır. Kaynak, çekirdeğin en ucuz yolu seçtiği biçimdir; örneğin MOV için MP4 indirilir ve yeniden kodlanmadan kabı değiştirilir. Bu dilim madde 44'te ayrıca kayda geçer.
- **g) İlk alfa `v0.0.0`'dır**: araştırmadaki her ailede dönüştüren uygulama, yayınlanmış ve doğrulanmış.

**Maliyet:**

- Beşinci ürün ailesi, EON Stream `v1`'den önce (madde 42c'nin uyarısı yine büyür).
- Yedi dış motor, yedi ayrı tedarik zinciri taahhüdüdür. Her sürüm yükseltmesi yeni bir SHA-256 sabitlemesi ve yeniden test demektir.
- İlk ihtiyaçta indirme bir defalık bekleme getirir: FFmpeg yaklaşık 190 MB açılır, Pandoc 44 MB'tır. Bunun karşılığında uygulama küçük kalır.
- LibreOffice kurulu değilse Word düzeni Typst üzerinden PDF'e birebir geçmeyebilir. Bu, kayıp olarak gösterilir.
- Kalite iddiası bir test matrisine bağlıdır. Her biçim ailesinden gerçek dosyalarla CI'da çalışan dönüşümler bakım ister.

**Kazanç:** Çevrimiçi dönüştürücülere dosya yüklemeden, reklamsız, hesapsız, cihazda çalışan tek bir dönüştürücü; EON Download Manager'ın her indirmeyi istenen biçimde teslim edebilmesi; motorları ve rotası açık, kaybı önceden söyleyen bir araç.

**49. EON Zip: 7-Zip ve WinRAR'ın yapabildiği her şeyi yapan, ikisinin de yapmadığı işleri ekleyen arşivleyici. [K — v0.16'da eklendi]**

Proje sahibinin isteği (22 Kasım 2026): WinRAR ve 7-Zip'in topluluk tarafından neden sevildiği, iyi yanları ve bunları nasıl yaptıkları incelenecek. İkisinin yapıp bizim yapamadığımız tek bir şey kalmayacak. Üstüne, ikisinin de yapmadığı ama yararlı olacak özellikler eklenecek ve yeni nesil, temiz bir görünümle sunulacak. Araştırma ve analiz: `zip/research.md`; tam yetenek tablosu: `zip/parity.md`; plan: `zip/plan.md`.

- **a) Ölçüt yetenek tablosudur.** `zip/parity.md`, 7-Zip ve WinRAR'ın her yeteneğini bir satır olarak tutar; EON Zip'in o satırdaki durumu (hangi sürümde var, hangi sürüme planlı ya da neden yapılamaz) iş yapılan PR'da güncellenir. "Hiçbir şey kalmasın" sözü bu tabloyla izlenir, ilk sürümle değil.
- **b) RAR oluşturulamaz; bu açıkça söylenir.** RAR biçimi RARLAB'ındır. unRAR lisansı, kodunun RAR uyumlu bir sıkıştırıcı yapmakta kullanılmasını yasaklar ve hiçbir özgür program RAR oluşturmaz (7-Zip de oluşturmaz). EON Zip RAR ve RAR5 açar, çıkarır ve tek adımda 7z ya da ZIP'e çevirir. RAR kullanma sebeplerinin her biri için açık bir karşılık sunulur: katı sıkıştırma (7z), kurtarma verisi (PAR2), değişikliği gösteren karma bildirimi.
- **c) Yerel biçimler saf Rust'tır.** 7z, ZIP (AES-256 dahil), TAR ve gzip/bzip2/xz/zstd çekirdekte, bağımsız kütüphanelerle oluşturulur ve açılır. 7-Zip'in tek başına okuduğu biçimler (RAR, ISO, CAB, MSI, WIM, DMG, VHD ve diğerleri) 7-Zip'in kendisiyle (`7zz`) açılır. 7-Zip ilk ihtiyaçta kendi yayınından indirilir ve SHA-256 ile sabitlenir (madde 37). Ayrı bir program olduğundan lisansı (LGPL + unRAR kısıtı) bizimkiyle birleşmez.
- **d) Güvenlik çekirdektedir ve kapatılamaz.** İkisinde de vahşi doğada kullanılmış açıklar oldu (WinRAR CVE-2023-38831 ve CVE-2025-8088, 7-Zip CVE-2025-0411 ve CVE-2025-11001). Hedef klasörün dışına yazan yollar ve bağlantılar her zaman reddedilir. Windows'un "internetten geldi" işareti iç içe arşivler dahil korunur. Program, betik ve çift uzantılı dosyalar çıkarmadan önce gösterilir. Arşiv bombasında durulup sorulur.
- **e) İkisinin yapmadıkları:** dağınıklık yapmayan akıllı çıkarma; çıkarmadan önizleme ve arama; her biçim için açık standart PAR2 kurtarma verisi; eski ZIP'lerde Türkçe ve diğer dillerdeki dosya adlarının kod sayfasının kendiliğinden tanınması; sıkıştırmadan önce her yöntemin beklenen boyutu; zaten sıkışık dosyaların boşuna sıkıştırılmaması; paylaşılabilir karma bildirimi; sözle anlatılan sağlık raporu. Hepsi Windows, Linux ve macOS'ta aynı pencerede, Türkçe ve İngilizce, ücretsiz, hesapsız, telemetrisiz.
- **f) Depolar.** `eon-zip-core` kütüphanedir, pencere bağımlılığı yoktur. `eon-zip-app` pencere ve komut satırıdır. İkisi de `GPL-3.0-or-later` + EON Modül ABI İstisnası taşır ve çalışan ilk sürümle açılır (43d).
- **g) Sağ tık menüsü.** Windows'ta kullanıcının kendi kayıt anahtarına, yönetici izni olmadan, klasik menü olarak eklenir. Windows 11'in üst düzey menüsü imzalı bir seyrek paket ister; bu, imza anahtarları olunca gelir (madde 30).
- **h) İlk alfa `v0.0.0`'dır.** `zip/parity.md`'de `v0.0.0` diye işaretlenmiş her satırı yapan, yayınlanmış ve doğrulanmış sürüm.

**Maliyet:**

- Altıncı ürün ailesi, EON Stream `v1`'den önce (madde 42c'nin uyarısı yine büyür).
- Arşivleyici, internetten gelen dosyaları açan bir programdır; saldırganların ilk hedeflerinden biridir. Güvenlik kuralları ve testleri sürekli bakım ister; bir açık, kullanıcının bilgisayarına yazılan bir zararlı demektir.
- 7-Zip'e bağlı biçimler, bir dış programın sürüm takibini ve yeniden sabitlemesini getirir.
- Yetenek tablosu söz verir: tablodaki her ⏳ satırı ileride yapılması gereken iştir; yapılmazsa tablo dürüstçe güncellenmelidir.
- RAR oluşturamamak, RAR isteyen birinin isteğini karşılamaz; bu bir kayıptır ve açıkça söylenir.

**Kazanç:** Ücretsiz, açık kaynaklı, reklamsız ve rahatsız etmeyen; 7-Zip'in sıkıştırmasını ve biçim genişliğini, WinRAR'ın kurtarma ve onarım güvencesini aynı yerde sunan; ikisinin de güvenlik açıklarının sınıfını baştan kapatan, her platformda aynı görünen bir arşivleyici.

**50. EON Browser: Chromium'a ve Windows'un motoruna dayanmayan, mahremiyeti varsayılan yapan, hafif bir tarayıcı. [K — v0.17'de eklendi]**

Proje sahibinin isteği (26 Kasım 2026): insanların hangi tarayıcıyı neden sevdiği, hangisinin neyi iyi ve neyi kötü yaptığı ve insanların mahremiyetine önem veren mükemmel tarayıcının nasıl olması gerektiği araştırılacak; ardından tarayıcımızın çekirdeği için yol haritası yazılıp geliştirilecek. Hafif ve en iyisi olacak. Çalışma sırasında iki şart eklendi: **"mümkünse kendi motorumuz ya da tamamen açık kaynaklı ve herkesin sevdiği bir şey olsun; insanlar Windows'tan ve Chromium'dan nefret ediyor"** ve **yalnızca Windows değil, bütün platformlar**. Araştırma ve analiz: `browser/research.md`; plan ve yol haritası: `browser/plan.md`; durum: `browser/status.md`.

- **a) Motor Servo'dur.** Rust ile yazılmış, MPL-2.0 lisanslı, Google'dan ve Apple'dan bağımsız, Linux Foundation Europe çatısındaki motor; Nisan 2026'dan beri crates.io'da gömülebilir bir kütüphane, her ay sürüm ve altı ayda bir LTS hattı. Sistem webview'ı (Windows'ta WebView2; Microsoft'un kapatılamayan zorunlu tanılama verisi madde 36 ile çelişir), CEF ve her Chromium çatalı sahibin şartıyla elendi. Gecko'nun masaüstünde gömme yolu yoktur; Firefox'a yama uygulanmış bir çatal olur ve dürüst yedek olarak kalır. Sıfırdan motor yazmak tek kişilik bir ekip için mümkün değildir. "Kendi motorumuz" burada şu anlama gelir: kendimiz derleyip yayınladığımız, kodunu okuyup yamaladığımız ve geliştirilmesine katkı verdiğimiz bir motor. 26 Kasım'da sahibin makinesinde `servo 0.8.0` clang olmadan derlendi; Wikipedia, Google araması, GitHub ve Reddit ekran dışında doğru çizildi, istek kancası izleyicileri engelledi.
- **b) Bütün masaüstü platformları ilk sürümden.** Windows, Linux ve macOS; x86_64 ve ARM64. Her sürüm CI'da üçü için derlenir ve yayınlanır.
- **c) Çekirdek motordan bağımsızdır ve ağa kendisi çıkmaz.** `eon-browser-core` engelleme, bağlantı temizleme, HTTPS'e yükseltme, adres çubuğu, arama motorları, site ayarları, geçmiş, yer imleri, oturum ve parmak izi politikasını taşıyan bir kütüphanedir; Servo'ya, pencereye ve bir HTTP istemcisine bağımlı değildir. Liste güncellemesini uygulamanın verdiği bir indirme işleviyle yapar. Motor değişse de politika kalır.
- **d) Sözleşme motordan önce gelir.** Telemetri yoktur. Kişi bir sayfa açana kadar tarayıcı **hiçbir istek yapmaz** (ilk açılış, boşta bekleme, adres çubuğuna yazma); bu, temiz bir profille CI'da test edilir. Yapay zekâ özelliği, sponsorlu kutucuk, satış ortaklığı bağlantısı, satılmış varsayılan arama motoru ve sorulmadan kurulan bileşen yoktur. Her koruma varsayılan olarak açıktır ve her biri kapatılabilir.
- **e) Varsayılanlar Firefox'un Standard'ı değil, Strict'idir.** Yerleşik reklam ve izleyici engelleme (Brave'in `adblock` motoru; EasyList, EasyPrivacy, uBlock Origin listeleri, çerez bildirimleri), normal pencerede de izleme parametrelerinin silinmesi ve yönlendirme sarmalayıcılarının atlanması, yalnızca HTTPS, üçüncü taraf durumunun bölümlenmesi ya da engellenmesi, Global Privacy Control. Do Not Track gönderilmez (parmak izine bir bit ekler). Arama önerileri kapalıdır; arama motoru ilk açılışta sorulur. Listeler sürümle birlikte gelir (GPL-3.0 uyumlu); kaynaklarından güncellenmesi bir kez sorulur.
- **f) Parmak izi tekdüzelikle değil, normalleştirmeyle korunur.** Yeni bir tarayıcının Tor gibi bir kalabalığı yoktur. Kullanıcı aracısı aynı sistemdeki Firefox ESR'nin dizesiyle aynıdır, Servo simgesi çıkarılır. Çekirdek sayısı 4 ya da 8 gösterilir, gerisi siteye göre rastgeleleştirilir; dil ve saat dilimine uyumluluk için dokunulmaz. Hangisinin hangi siteyi bozduğu ölçülerek ilerlenir.
- **g) Ayrı ürünler ayrı kalır (madde 45).** İndirmeler kuruluysa EON Download Manager'a devredilir, tünel EON DNS'in işidir, senkron EON Account'un mühürlü kasasından geçer (madde 46b).
- **h) WebExtensions yoktur.** Servo'da yoktur ve madde 4, sandbox tasarımı olmadan kod çalıştıran eklentiyi yasaklar. İnsanların eklentiye en çok ihtiyaç duyduğu iş olan engelleme yerleşiktir.
- **i) Depolar.** `eon-browser-core` (kütüphane) ve `eon-browser-app` (Servo ve pencere). İkisi de `GPL-3.0-or-later` + EON Modül ABI İstisnası taşır ve çalışan ilk sürümle açılır (43d). Tauri kullanılmaz: Tauri, Windows'ta WebView2 demektir.
- **j) Sürümler.** Çekirdek ve uygulama `v0.0.0` ile başlar; yol haritası `browser/plan.md`'dedir. Uyumluluk listesi (`browser/compat.md`) en çok kullanılan sitelerin her Servo sürümünde nasıl çizildiğini kaydeder.

**Maliyet:**

- Yedinci ürün ailesi, EON Stream `v1`'den önce (madde 42c'nin uyarısı yine büyür). Tarayıcı, ailenin en büyük ve bakımı en uzun sürecek ürünüdür.
- **Siteler bozulacak.** Servo üç büyük motordan gençtir. Giriş, form, video (MSE) ve uzun ömürlü web uygulamaları henüz ölçülmedi. DRM yoktur; Netflix ve benzerleri oynamaz. Bu açıkça söylenir; gizlice Chromium'a düşen bir yedek yoktur.
- **Windows'ta sandbox ve site izolasyonu yok.** Servo kendisini "olduğu gibi, güvenlik garantisi olmadan" sunar. Kötü niyetli bir sayfanın ele geçirdiği içerik süreci Chrome ya da Firefox'taki gibi kapatılamaz. Her sürüm notu bunu yazar; JIT siteye göre kapatılabilir.
- Servo'nun API'si her ay kırılır. Uygulama Servo'yu tam sürümüyle sabitler; kararlı kanal bir LTS hattını izler.
- Servo, yapay zekâyla yazılmış katkı kabul etmez; EON'un yukarı akışa göndereceği düzeltmeler elle yazılır.
- Diskte yaklaşık 100 MB'tır (bir sistem webview'ı 15 MB olurdu). "Hafif" bu yüzden bellek, açılış süresi, boşta işlemci ve sıfır arka plan trafiği olarak tanımlanır, ölçülür ve her sürümle yayınlanır.
- Ailenin geri kalanı Tauri'dir; tarayıcı değildir. Arayüzün nasıl çizileceği (Servo'nun kendisinde HTML mi, egui mi) prototiple karara bağlanacak açık bir sorudur [A].
- Servo'nun insan ve para tabanı incedir: ayda yaklaşık 8 bin dolar düzenli bağış alır, ve Servo üzerine kurulu Verso Ekim 2025'te insan gücü ve finansman yetmediği için arşivlendi. Çekirdeğin motordan bağımsız olması bu riskin karşılığıdır.

**Kazanç:** Chromium'a, Google'a, Apple'a ve Microsoft'a dayanmayan; telemetrisi, yapay zekâsı ve reklamı olmayan; korumaları kutudan çıktığı gibi açık ve her birini kişinin kapatabildiği; ilk açılışta tek bir istek yapmayan ve bunu testle kanıtlayan; her masaüstü platformunda aynı olan, Rust ile yazılmış bir tarayıcı. Ana akım tarayıcıların 2025–2026'da güven kaybettiği her noktada tersini yapan ve bunların hiçbiri için motorun olgunlaşmasını beklemeyen bir ürün.

---

## Faz planı

**Faz 0 — Temel ve risk azaltma** *(amaç: v1'in önündeki bilinmezleri kaldırmak)*

1. **Lisans + yönetişim kararı** (madde 33) — ilk gün.
2. Repo yapısı, araç zinciri sabitleme, CI iskeleti.
3. **Riskli iş önce:** mpv ayrı süreç + IPC ile bir dosyayı oynatan Tauri kabuğu (madde 2).
4. Protokol kararı: (a) mı (b) mi — bir haftalık deney (madde 1).
5. Torrent motoru seçimi: librqbit ile sıralı indirme + yerel HTTP sunucu → mpv (madde 1).
6. Sözleşme taslakları `v0`: eklenti API'si, manifest şeması, modül manifesti + izinler, paket formatı (`mirrors` dahil), imza ve iptal formatı (madde 5, 15d, 34).
7. Uyumluluk test seti iskeleti + fixture kaydı (madde 1).
8. Tehdit modeli dokümanı (madde 37).

**Faz 1 — v1 (EON Stream, masaüstü)**
v1 tanımındaki 8 kalem, fazlası değil. Marketplace yok, hesap yok, creator yok, Edu yok.

**Faz 2 — Genişleme** *(hâlâ backend yok, hâlâ Faz L0)*
~~Dosya modülü;~~ *(v0.10: dosya ayrı ürün — madde 45)* materyal paketi görüntüleyicisi (madde 16); creator-core + video/paket creator'ları (madde 15); Git tabanlı marketplace index (madde 8); moderasyon çekirdeği kapalı flag ile kurulu (madde 6).

**Faz 3 — Edu pilot** *(ön koşul: Faz L1 kapısı — madde 30)*
Roller ve izin setleri (17); doğrulama zinciri + pilot okul sınırı (18); kurum sunucusu + HTTPS materyal dağıtımı (25); duyuru ve yorum (19'un pilot kapsamı); kurum kataloğu (27c); Edu derleme profili + CI sembol kontrolü (22); okul araçlarından yoklama ve ödev (20); veri koruma etki değerlendirmesi ve kurum sözleşmesi (31).
**Tek okulla başla, yazılı rapor üret** (madde 24).

**Faz 4 — Marketplace backend ve kurumsal yaygınlaşma**
Hesaplar (7); puanlama, arama, feed'ler; moderasyon açık; şeffaflık raporu (12f); OIDC sağlayıcıları (21); il müdürlüğü ölçeği (24); P2P hızlandırıcı değerlendirmesi (25).

**Faz 5 — Ücretli eklentiler ve resmi yayıncı programı** *(ön koşul: Faz L2)*
Madde 10, 14.

---

## Reddedilen alternatifler (neden hayır)

| Alternatif | Neden değil |
|---|---|
| Electron | Bellek ve boyut maliyeti; Rust core ile iki dilli mimari; torrent + mpv gömme Tauri'de daha doğal. |
| Flutter / React Native | Masaüstü olgunluğu ve native oynatıcı entegrasyonu zayıf; Rust core'a köprü maliyeti. |
| Stremio'yu fork'lamak | Arayüz ve ürün kararları bize ait olmalı; kapalı streaming server'a bağımlılık; Edu'nun kısıtlama modeli fork'a sığmaz. |
| Kendi eklenti protokolünü icat etmek | Ekosistem etkisi sıfırdan başlar; en büyük tek avantaj (mevcut eklentiler) kaybolur. |
| Web uygulaması (tarayıcıda) | Torrent, yerel dosya erişimi, mpv ve çevrimdışı çalışma tarayıcıda mümkün değil ya da ciddi kısıtlı. |
| **Edu için ayrı isim ve ayrı kod tabanı** | Ortak core kararının tüm faydasını iptal eder (tek paket formatı, tek arayüz dili, tek topluluk, madde 28); iki ürünü bakmak küçük ekibi böler. Bedeli madde 22 ve 32 ile ödenir. |
| Edu'da açık DHT/tracker ile torrent | Kurum sunucusu zaten kaynak; HTTP yeterli. P2P, eş koordinasyonu ve IP görünürlüğü sorununu bedelsiz getirmez (madde 25). |
| Pilotta Matrix + LiveKit | Tek başına ürün büyüklüğünde işletme yükü; madde 23'teki denetim gereksinimiyle E2EE çelişkisi (madde 19). |
| v1'de kod çalıştıran yerel eklentiler | Gerçek sandbox olmadan güvenlik iddiası dürüst olmaz; bildirimsel katman ihtiyacın çoğunu karşılar (madde 4). |
| DMCA tarzı otomatik karşı-bildirim | Yargı yeri seçilmeden başka bir rejimin mekanizmasını kopyalamak, korumasını almadan yükümlülüğünü üstlenmek olur (madde 12e). |
| Video, dosya ve müzik tek uygulamada (ilk plan) | Bir türün gecikmesi hepsini bekletir; kullanıcı istemediği türü de kurar; core, en karmaşık türün ihtiyaçlarına göre büyür. Ayrı ürünlerin bedeli madde 45'te yazılı. |
| Telemetri ile ürün ölçümü | Edu'da öğrenci verisi riski, EON Stream'de güven maliyeti; takas bilinçli (madde 36). |

---

## Açık konular / sıradaki adımlar

**Hemen — Faz 0 başlamadan:**
- [ ] **Proje lisansı ve yönetişim kararı** (madde 33) — her şeyin önünde
- [ ] Tüzel kişilik tipi ön araştırması (madde 38) + avukat görüşmesi randevusu
- [ ] Madde 13'teki 8 soruluk listeyi avukata gönderilecek biçimde hazırla

**Faz 0 çıktıları:**
- [x] mpv IPC prototipi (madde 2) — ayrı süreç + JSON IPC çalışıyor. Tauri kabuğu `v0.14`'te geldi: arayüz, uçbirimde çalıştığı kanıtlanmış bir sistemin üzerine yazıldı (bkz. madde 2, arayüzün zamanlaması). `v0.16`'dan beri görüntü de pencerenin içinde (bkz. madde 2, oynatıcının yeri).
- [x] Protokol kararı: kendimiz (madde 1) — Cinemeta ve OpenSubtitles v3'e karşı doğrulandı.
- [x] Torrent motoru seçimi ve sıralı akış prototipi (madde 1) — librqbit; magnet → üstveri → ön bellek → loopback HTTP → mpv zinciri gerçek bir sürüye karşı uçtan uca test edildi.
- [x] Modül manifesti ve izin sistemi `v0` taslağı (madde 3, 4, 5, 22, 27 buna bağlı) — şema yazıldı **ve** uygulandı; bildirimsel modülün izin isteyemeyeceği kuralı artık şemada zorlanıyor, yalnızca temada değil.
- [x] Paket formatı + `mirrors` / web seed + creator-core sözleşmesi `v0` (madde 15, 16, 28) — şema yazıldı; uygulaması Faz 2.
- [x] İmza ve iptal formatı taslağı (madde 34) — taslak çıktı, kanonik serileştirme (JCS) sabitlendi ve istemci tarafı uygulandı. Anahtar **yok**: madde 34'teki duruma bakın.
- [ ] Tehdit modeli dokümanı (madde 37)
- [x] Erişilebilirlik kontrol listesi (madde 35) — v1 çıkış ölçütü olarak. Kontrastın **ölçülebilir** kısmı artık otomatik: tema belgeleri WCAG 2.1 oranlarına karşı denetleniyor ve iki yerleşik palet kendi denetiminden geçiyor (CI'da adlandırılmış bir kontrol). Klavye ve ekran okuyucu ölçütleri arayüzle birlikte, `v1`'de.

**v1 tanımındaki sekiz kalemin durumu (v0.14 itibarıyla):**

| # | Kalem | Durum |
|---|---|---|
| 1 | Masaüstü, tek dosya kurulum, imzalı, otomatik güncellemeli | kısmen — güncelleyici ve imza doğrulama var; kurulum paketi ve anahtarlar yok |
| 2 | Video modülü: katalog/meta/stream, mpv, altyazı, devam | bitti |
| 3 | URL ile eklenti ekleme, listeleme, kaldırma, sıralama | bitti |
| 4 | Torrent akışı: sıralı indirme + yerel HTTP sunucu → oynatıcı | bitti |
| 5 | Modül yöneticisi, imza doğrulama | bitti — anahtar olmadığı için hiçbir şey kurmuyor, tasarım gereği |
| 6 | Bildirimsel tema desteği | bitti |
| 7 | Uyumluluk test seti CI'da yeşil | bitti — CI'da adlandırılmış bir kontrol |
| 8 | Türkçe + İngilizce; temel erişilebilirlik | iki dil bitti (arayüz dahil, yeniden başlatmadan); erişilebilirliğin klavye ve ekran okuyucu yarısı açık |

Kalan: **kurulum paketi, imzalama anahtarları** ve arayüzün hâlâ uçbirime yolladığı işler — modüller, güncelleme denetimi, altyazı arama, derin bağlantılar.

**Karara bağlanacak, aceleci olmayan:**
- [ ] Yerel kod eklentileri için sandbox tasarımı: WASM mı, izole webview mı (madde 4)
- [ ] **Edu pilot okul sayısı üst sınırı — somut bir sayı yaz** (madde 18)
- [ ] Finansman yolu ve ilk hibe başvurusu (madde 38)
- [ ] Sohbet gerçekten gerekli mi; gerekirse Matrix mi (madde 19)
- [ ] Android oynatıcı arka ucu (madde 2) — v0.14'ten beri alfa hattında, telefon ve TV
- [ ] Edu'da P2P hızlandırıcı gerekli mi (madde 25)
- [ ] Ses ürünü: adı ve kapsamı (madde 45a) — [A]; ürünün kendisi [E]
- [ ] EON Account sunucusu için 5651 trafik kaydı sorusu madde 13'teki avukat listesine (madde 46j)
- [ ] EON Browser: arayüz Servo'nun içinde HTML mi, egui mi — prototiple karar (madde 50, [A])
- [ ] EON Browser: YouTube, giriş, form ve video uyumluluğunu tünelsiz bir bağlantıda ölç (madde 50j)
