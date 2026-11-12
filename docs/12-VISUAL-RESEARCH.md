# 12 — Görsel Araştırma ve İkinci Konsept

12 Kasım 2026. **İnceleme taslağıdır.** Kullanıcı işlevleri anlaşılır buldu, ilk görünümü fazla genel buldu ve benzer uygulamaları inceleyerek özgün bir görsel öneri hazırlamamızı istedi. Tasarım tercihlerini bu tur için ekibe bıraktı. Bu çalışma uygulama geliştirme, ürün kararı onayı veya yayın değildir. 01–03 kaynaklarındaki kabul edilmiş kurallar değiştirilmedi; aşağıdaki yerleşimler tasarım ekibinin önerisidir. Örnek veriler uydurmadır.

## Gerçekten ne incelendi?

Üç uygulamanın resmî açık kaynak depolarındaki görseller indirilip görsel olarak açıldı. Yalnız ürün tanıtım metinlerinden tasarım sonucu çıkarılmadı. Kaynak bağlantıları aşağıda sabit depo sürümüne bağlıdır.

**Sınır:** Bu görseller 12 Kasım 2026'da erişilen resmî depo varlıklarıdır; uygulamaların bugün kurulan son sürümünün ekranı oldukları doğrulanmadı. Özellikle Tasks ve Loop görselleri eski Android görünümü taşır. Bunlardan güncel moda, tam font ailesi, gerçek dokunma alanı veya ölçülmüş boşluk iddiası çıkarılmadı.

| Uygulama ve görülen kanıt | Gözlenen ilke | Gün Gün'deki özgün karşılığı | Taşınmayan unsurlar |
|---|---|---|---|
| [Tasks.org resmî deposu](https://github.com/tasks/tasks), [açık/koyu görev listesi](https://raw.githubusercontent.com/tasks/tasks/14a773c6d289ce8dc10e81320199957d049a5480/fastlane/metadata/android/en-US/images/phoneScreenshots/4_en-US.jpg) | Görevler ayrı kutular olmadan, aynı hizada onay alanı/metin ve ince ayırıcılarla taranıyor; alt işler girintiyle ayrılıyor. | Bugün'deki esnek işleri açık satırlar ve tek tamamlama alanıyla gösterme; her satırda ayrı Tamamla düğmesini kaldırma önerisi. | Eski araç çubuğu/çekmece, çok renkli çipler, pembe yüzen düğme, marka ve birebir yerleşim. |
| [Loop Habit Tracker resmî deposu](https://github.com/iSoron/uhabits), [ana ekran](https://raw.githubusercontent.com/iSoron/uhabits/82c1ff287a8ac05321bfa95b61399fa8a2d1da5b/screenshots/1.png) | Ortak tarih başlıkları ve sabit hizalar, aynı bilgiyi her satırda tekrarlamadan okumayı kolaylaştırıyor. | Haftada sol tarih sütunu + sağ içerik sütunu; yedi günü dikey tarama. Bu, Loop'un yatay geçmiş tablosunun kopyası değil. | Başarı halkaları, renkli alışkanlık matrisi, kaçırılan gün çarpıları ve eski yükseltilmiş yüzeyler. |
| [Cashew resmî deposu](https://github.com/jameskokoska/Cashew), [bütçe/kategori tanıtım görseli](https://raw.githubusercontent.com/jameskokoska/Cashew/9cfbe50c16d95429891d44faf5f2c77a3abdb93b/promotional/PlayStore/Phone%20Screenshots/Frame%202.png), [genel tanıtım](https://raw.githubusercontent.com/jameskokoska/Cashew/9cfbe50c16d95429891d44faf5f2c77a3abdb93b/promotional/GitHub/SocialPreviewGitHub.png) | Ana tutar güçlü; bağlam ve ikincil sayılar daha hafif. Kategori tutarları sağda hizalanıyor. | Para'da bilinen aylık harcama tek odak; kategoriler sağa hizalı tutarlı sade hesap listesi. | Mor/çok renkli tema, emoji kategori görselleri, yüzdeler, halka/çizgi grafikleri, modüler kart yığını ve marka. |

Kaynak sürümleri: Tasks `14a773c6d289ce8dc10e81320199957d049a5480`; Loop `82c1ff287a8ac05321bfa95b61399fa8a2d1da5b`; Cashew `9cfbe50c16d95429891d44faf5f2c77a3abdb93b`. Depo sürüm tarihi, ekran görselinin çekim tarihi değildir.

[Structured](https://structured.app/), [Tiimo](https://www.tiimoapp.com/) ve [Todoist](https://www.todoist.com/) siteleri bu ortamdan 403 erişim hatası verdi; ekranları görülmüş gibi anlatılmadı. Ivy Wallet'ın resmî README'si bakımın 2024'te sona erdiğini belirttiğinden güncel görsel örnek olarak seçilmedi. Araştırma üç kullanılabilir uygulamada durduruldu; ek araç/hesap kurulmadı.

## İlk görüntüdeki sorun ve seçilen öneri

İlk maket anlaşılırdı, fakat marka/başlık tekrarı, her kayıt için kutu, aynı kalın başlıklar ve art arda düğmeler içerik türlerini birbirine benzetiyordu. Haftada yatay gün seçimi yalnız bir günü açıyor, bütün haftanın ritmini gizliyordu. Para'daki çubuklar ürünün sade liste yönünü genel bir gösterge paneline yaklaştırıyordu. Telefonları yan yana sunmak tek başına ürün içi eşit kart kuralının ihlali sayılmadı.

Üç yön tartıldı: ilk maketin yalnız boşluklarını düzeltmek hızlı ama karakter farkı zayıf; tüm ekranı takvime çevirmek esnek işi sahte saate bağlama riski taşıyor; **açık ajanda ve hesap listesi** ise mevcut işlevlerle uyumlu. Sonuncusunu tasarım ekibi bu tur için seçti; kullanıcı onayı olarak kaydedilmedi.

Önerinin ortak dili: serin beyaz, koyu mürekkep metin, ölçülü turkuaz ve yalnız dikkat noktalarında küçük şeftali vurgusu. Marka her ekranda tekrar etmez. Kimlik, süsten çok sayıların/metnin hizası ve farklı içeriklere verilen farklı görsel ritimden gelir. Nunito/Figtree yönü korunur; raster çıktı gerçek font doğrulaması değildir.

- **Bugün:** yalnız kesin saatli randevu güçlü turkuaz zaman bandında; esnek işler açık tamamlama satırlarında; dikkat kayıtları küçük sıcak kenar işaretiyle. Üç eşit kart yok. Boş daire tamamlama alanı önerisidir; erişilebilir adı ve geri alma davranışı ileride doğrulanır.
- **Hafta:** yedi gün dikey tarih sütununda; saatli olay, esnek iş ve kesinleşmemiş tercih metinle ayrılır. Fitness örneği 16/18/20/22 tarihleriyle gün aşırıdır; sabit haftalık dört gün kuralı değildir. Alt eylem önce değişiklik incelemesine götürür; görüşmeyi kesinleştirmiş saymaz.
- **Para:** bilinen harcama öne çıkar, gelir ve planlanan/aktarılan birikim ayrılır; grafik yerine sade kategori listesi. Bilinmeyen elektrik tutarı ayrı görünür; kullanılabilir para uydurulmaz. Önceki taslaktaki 9.200 TL gider dağılımı korunur: 4.000 + 3.000 + 1.200 + 1.000.
- Beş alt sekme aynı sırada kalır. Seçilme işareti küçültülür; yeni bir gezinme veya özellik eklenmez. Gölgeler/gradient/cam/emoji/mor kullanılmaması hedeflenir.

## Görsel çıktı ve kontrol

İkinci konsept üretildi: [ikinci ekran taslakları](assets/ikinci-ekran-taslaklari.png). Bu, uygulamanın gerçek ekranı değil; kullanıcı incelemesine hazır raster tasarım önerisidir.

Görseli gözden geçirirken doğrulananlar: hafta görünümünde yedi gün var; fitness 16/18/20/22 tarihlerine gün aşırı yerleştirilmiş; gider toplamı 4.000 + 3.000 + 1.200 + 1.000 = 9.200 TL; bilinmeyen elektrik tutarı sıfır gösterilmiyor; 2.000 TL birikim planı 1.500 TL gerçekleşen transferden ayrı; örnek randevular ve tutarlar kurgusal. Ana ekran ve takvim için ayrı gezinme simgeleri görünür.

Sınırlamalar: raster görselde hafif turkuaz ton dokusu kaldı. Yazı tipi, kontrast, 48 dp dokunma alanı, 360/412 px genişlikler ve koyu tema doğrulanmadı. Hafta ekranı kurgusal, kısmi bir örnektir; kullanıcının tam programı değildir ve gösterilmeyen tercihler iptal anlamına gelmez. Bu bir öneridir, onaylanmış tasarım değildir; araştırma/üretim kaydı görselin tüm üretim klişelerinden arınmış olduğu iddiası taşımaz. Sleek kullanılmadı; hesap veya anahtar bağlanmadı. Uygulama kodu, yeni bağımlılık veya dağıtım yoktur. Depoya yalnız Gün Gün için üretilen iki konsept görseli eklenir; araştırmada incelenen üçüncü taraf ekran görüntüleri eklenmez.
