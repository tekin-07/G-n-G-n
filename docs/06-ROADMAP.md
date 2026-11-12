# 06 - Yol Haritası ve Faz Planı

12 Kasım 2026 güncellemesi. Ürün hâlâ planlama aşamasındadır; hiçbir faz tamamlanmış değildir. Eski faz içeriği bu belgeyle değiştirilmiştir. Kodlama başlangıcı ayrıca kullanıcı onayı ister. Her seferinde tek faz; zaman sözü yok. `[ ]` yapılmadı, `[x]` yalnız kanıt ve onayla.

Öncelik: bildirim/platform kanıtı → güvenli veri ve yedek → esnek plan → temel para/faturalar → hedef/sosyal bağlantılar → kart/taksit → tamamlayıcı işlevler ve gerçek kullanım. Temel hedef bağlantısı erken, ayrıntılı günlük/grafikler sonra gelir.

## Faz kapısı
Kod içeren faz: pnpm check (Biome + tsc + Vitest + dependency-cruiser + build), ilgili anlamlı testler, değişen belgeler, çevrimdışı ve veri güvenliği kontrolleri. UI varsa 360/412 px açık/koyu ekran görüntüsü, 03-DESIGN bölüm 3 ve 11 değerlendirmesi, axe ciddi/kritik 0, %130 yazı. Android adaptörü varsa ilgili gerçek cihaz senaryoları ayrıca geçer; Playwright tek başına OS bildirimini kanıtlamaz. Kullanıcı kontrol/onayı sonrası faz etiketi, yetkilendirilmiş yayın ve doğrulama.

Sadece belge güncellemesinde kaynak tutarlılığı/bağlantı/diff kontrol edilir; henüz olmayan pnpm komutları çalıştı denmez, faz tamamlandı işaretlenmez. Deneme dalında başarısız platform deneyi açık sonuçtur; ürün kabul kapısını geçmiş sayılmaz.

## MVP

### Faz 0 - Platform/bildirim kararı, kabuk, tema ve doğrulama
- [ ] 0.1 PWA sınırlaması ve Capacitor adayını kullanıcıya somut seçenek/bedellerle sun; PRD O1 için deneme kapsamını onaylat. OS/eklenti/Node/TypeScript/Vite sürüm ve uyumluluğunu resmî kaynakla doğrula; sürüm/cihaz kararını kaydet.
- [ ] 0.2 Gerçek Android'de bildirim deneyi: kapalı uygulama, kilit, çevrimdışı, yeniden başlatma/pil/izin, eylem, iptal ve değişiklik. Yedi günlük gözlem ve bilinen sınırlar; sonuçtan sonra platform seçimi. Sessiz saat/tekrar/mahremiyet varsayımlarını O6'da netleştir.
- [ ] 0.3 Git gizlilik korumaları; pnpm workspace, core/data/web, strict/noUncheckedIndexedAccess, Biome, dependency-cruiser; dev/build/check/lint/typecheck/test/test:e2e/depcheck komutları. Gereksiz server/eklenti iskeleti yok.
- [ ] 0.4 Hash router ve beş sekme; i18n, ikon kayıtları, tema token'ları; Nunito/Figtree yerel font, Türkçe karakter ve tabular rakam önizlemesi; kullanıcı font onayı.
- [ ] 0.5 Kontrast, CSP altında dialog/blob görseli, manifest/ikon/çevrimdışı kabuk, yarım formu koruyan güncelleme. Seçilirse Android kalıcılık ve paket yaşam döngüsü.
- [ ] 0.6 CI ve web dağıtım yapılandırması; Android seçildiyse imzalama/dağıtım/güncelleme ve sırların saklanmasını ayrıca belgeleyip yetkiyle uygula. Pages'in APK dağıtımı yerine geçmediğini açıkla.
- [ ] 0.7 Temel testler, beş sekme gezinme, çevrimdışı açılma, axe; bildirim platform testlerini tarayıcı testinden ayır.
- [ ] 0.8 README.md, docs/VERSIONS.md ve platform kararı/deney sonuçlarını yaz. Hiç ölçülmeyen desteğe garanti verme.
Kabul: kabuk kullanılabilir; temel bildirim ihtiyacını karşılayan yol kanıtı veya açık engel; platform seçimi ve kullanıcı onayı olmadan ihtiyaç karşılandı denmez.

### Faz 1 - Veri temeli, şifreli yedek ve kademeli başlangıç
- [ ] 1.1 core/time ve money: minor unit/BigInt/yuvarlama, finans vektörleri; plan şemaları ve repository portları.
- [ ] 1.2 Dexie sürüm 1, Zod doğrulama, atomik yazma, soft-delete/8 sn geri al/30 gün temizlik, migrasyon fixture'ı. Yeni tablolar 05'e göre; açık tasarım kararlarını sessizce kesinleştirme.
- [ ] 1.3 Şifreli yedek/üzerine yazma geri yükleme, format çerçeveleme ve boyut/KDF sınırları, eski sürüm ve bozuk veri, şifreli kurtarma önerisi. Birleştirme O5 çözülmeden sunulmaz.
- [ ] 1.4 Kalıcı depolama/kapasite ve yedek hatırlatması; yeni cihazda bildirim izni/alarmları yeniden oluşturma.
- [ ] 1.5 Kısa onboarding ve kaldığı yerden devam; temel müsait zamanlar, ilk görev; finans ve ayrıntılar ihtiyaç anında. Sistem rutinlerini kullanıcı seçimine göre üret.
Kabul: taslak ve veri kapat/açta korunur; yedek gidiş-dönüş ve hatalı geri yükleme güvenlidir. Bu kapı geçmeden gerçek kullanıcı verisi pilotuna başlanmaz.

### Faz 2 - Günlük/haftalık plan, hızlı kayıt, toparlama ve hatırlatmalar
- [ ] 2.1 Saatli, esnek pencere, intervalDays ve haftalık sıklık; sabit oluşum kimliği; öğle görüşmesi ve sabit dört gün spor tohumunun olmaması.
- [ ] 2.2 Bugün üç bölümü; Plan gün/hafta/ay; hızlı kayıt ve tarihsiz liste. Temel hedef/aktivite bağlantısı için veri desteği.
- [ ] 2.3 Haftalık plan taslağı/onayı; birden fazla görüşme tercihi/kesin randevu; müsait kapasite ve dinlenme/ulaşım payı, belirsiz süre gösterimi.
- [ ] 2.4 Taşı/bu haftadan çıkar/duraklat; O2 fitness erteleme kararını önizlemeyle netleştir. Tek oluşum ve tekrar kuralını ayır; otomatik yarına yığma yok.
- [ ] 2.5 OS bildirim adaptörü: kategori/sessiz saat, izin göstergesi, eylem, iptal/uzlaştırma, eski veya iki kez gelen eylem. 15:00 zorunlu sorgusu yok.
- [ ] 2.6 İsteğe bağlı aile notları ve tamamlananlar; zorunlu %60/seri/joker altyapısı kurma.
Kabul: A1,A2,A6,A8,A9,A10 ve B1–B4/B8'in ilgili bölümleri; iki farklı mesai saatinde ek iş geçerli, haftayı aşan fitness doğru, kapalı uygulama bildirim kanıtı güncel.

### Faz 3 - Temel para, faturalar, aylık dağılım ve birikim
- [ ] 3.1 Hesaplar/elle kur, gelir/gider, hızlı giriş, arama/filtre, sil/geri al; kategori başına aylık dağılım ilk sürümde.
- [ ] 3.2 Gelir kaynakları, maaş/zam ve telafi; otomatik kaydın banka doğrulaması olmadığını belirt, gerçekleşmeyeni düzelt.
- [ ] 3.3 Düzenli gider ve dönem faturası; bilinmeyen/tahmini/kesin tutar, vade, otomatik ödeme talimatı ve gerçek ödeme ayrımı; tek seferlik/kısmi ödeme, kalan borç ve uyarı.
- [ ] 3.4 Para özeti: gerçekleşen gelir/gider, beklenenler, maaşa kadar yük; bilinmeyenleri sıfır saymama. O4 formül tasarımını belgele ve vektörlerle kanıtla; kart desteği gelene kadar kartlı senaryoya kesin rakam sunma.
- [ ] 3.5 Transfer/dönüşüm; birikim planı ile gerçekleşen transfer ayrımı, tek işlemde bağlantılar. Yedek kapsamını yeni tablolara genişlet.
Kabul: A3,A5,A7,A9,B5,B7,B9; ilk faturası bilinmeyen kullanıcı akışı; çift ödeme/çift düşüm yok. Temel aylık para görünümü v1'e ertelenmez.

### Faz 4 - Hedefler ve sosyal etkinlik bağlantıları
- [ ] 4.1 Aktif/daha sonra/duraklatılmış/tamamlandı hedefleri; küçük adımlar, dil/okuma gibi esnek tekrarlar; tamamlama hedefte tek kaynaktan görünür.
- [ ] 4.2 Tarihsiz fikir → hedef/görev/etkinlik dönüşümü; hazırlık görevleri (bilet, rezervasyon, kayıt) ve onay/kesinleşme.
- [ ] 4.3 Sinema/tiyatro/gezi/arkadaş buluşması; yer ve hatırlatma payı; etkinlikten gider formuna hazır bilgiler, kullanıcı onayı, çift kayıt koruması.
- [ ] 4.4 İsteğe bağlı kısa not/Anılar; program içeriği için dış uzman uygulamayı yeniden üretme. Duraklatma geçmişi silmez; gelecekteki işler/alarmlara etkisi önizlenir.
Kabul: B3,B4,B6,B8; “İngilizce haftada üç oturum” ve “Bilet al → tiyatro” akışları; ikinci giriş/not zorunluluğu yok. Ayrıntılı günlük ve grafik v1'e kalır.

### Faz 5 - Kart, taksit ve finans uç durumları
- [ ] 5.1 Kart/kesim/vade, taksit dağıtımı, ekstre ve 12 aylık yük; yalnız bekleyen taksitleri düzenleme.
- [ ] 5.2 O4: kısmi kart ödemesi dağıtımı, iade, yanlış işlem düzeltmesi ve faiz kapsamını netleştir. Banka eşleşmesi varmış gibi davranma; kapsanmayan durumu açık göster.
- [ ] 5.3 “Ekstreye yansıdı”/“ödendi” ayrımı; eski taksit bazlı ödeme bayrağı isteği ancak kullanıcı ihtiyaç duyarsa, muhasebeyi çiftlemeyen tasarımla.
- [ ] 5.4 Kullanılabilir para formülünü kart yükleriyle doğrula; posted alışveriş ve kart ödemesinin çift düşülmediğini kanıtla; uyarı ilgili ekstrenin kalanına göre.
Kabul: A4 ve O4 sınır vektörleri. Kısmi ödeme/iade belirsizken kart akışı tamamlandı sayılmaz.

### Faz 6 - Tamamlayıcı aktarım ve dosya akışları
- [ ] 6.1 .ics: kesin tarihli etkinlikler ve desteklenen tekrarlar, önizleme, UID/tekrar içe aktarma sınırları; takvim ve uygulama alarmını çiftletmeme rehberi. Dış sağlayıcı bildirimi garantilenmez.
- [ ] 6.2 Fiş fotoğrafı kamera/galeri, <=250 KB, işleme bağlama, silme; isteğe bağlı yedek ve geri yükleme.
- [ ] 6.3 Yeni tüm verilerin yedeklenmesi, silinmiş referansların yönetimi, bozuk/yanlış parola; O5 ayrıca çözülmüşse birleştirme, aksi halde özellik sunulmaz.
Kabul: A7,A9 ve cihaz dosya/paylaşım kontrolleri. Yedek ilk kez burada yapılmaz; Faz 1'deki güvenli akış genişler.

### Faz 7 - Gerçek hafta, erişilebilirlik, performans ve MVP yayını
- [ ] 7.1 07'deki tüm MVP senaryoları; görsel/axe/TalkBack/yazı büyütme, çevrimdışı ve OS hatırlatma kontrolleri.
- [ ] 7.2 Normal iş günü, yoğun gün, görüşme değişikliği, kaçırılmış fitness ve yaklaşan faturalarla bütünleşik hafta akışını kullanıcıyla değerlendir.
- [ ] 7.3 04 bölüm 10 performans ölçümü; 5.000 işlem ve iki yıllık aktivite; dış ağ isteği kontrolü.
- [ ] 7.4 Hakkında/gizlilik/sürüm/CHANGELOG; açık yayın onayıyla seçilen platforma dağıtım, geri dönüş planı, v0.1.0.
- [ ] 7.5 Yedi günlük kullanım geri bildirimi; karar yükü, hatırlatma yorgunluğu, ödeme ve veri kaybı değerlendirmesi. Bulguları önceliklendir.
Kabul: gerçek cihaz ve yedek kanıtı; kullanıcıya yararlı, baskısız günlük akış. Başarısızlık/eksik kapılar açık raporlanır.

## v1
Faz 8: ayrıntılı hedef/öğrenme günlüğü, geçmiş gelişim/hafta sonu/ay istatistikleri; temel aylık kategori dağılımı zaten Faz 3'tedir. Seri/halka gibi göstergeler ayrı onay ve isteğe bağlı kullanım ister.
Faz 9: aylık/kategori bütçe limitleri, ileri SVG grafikler, sakin uyarı.
Faz 10: maddi hedef tutar/tarih/bağlı hesap ve birikim hesabı.
Faz 11: PIN/WebAuthn kilidi, yanlış deneme gecikmesi ve dürüst sınırlar; veri içeride şifreliymiş gibi anlatılmaz.

## v2 ve Ultimate
Faz 12: temel bildirim kurmak değil, ölçülen kullanım sorunlarını iyileştirmek; yeni platform/sunucu değişikliği ayrıca onay ister.
Faz 13: QR/OCR; 20 gerçek fiş yalnız cihazda, >=%90 toplam doğruluğu yoksa öneri ve kullanıcı onayı. Sunucu/AI aktarımı açık onaylı.
Faz 14: gelişmiş kural önerileri, isteğe bağlı/varsayılan kapalı AI. Ultimate: senkronizasyon/widget/Play Store/canlı kur/Drive/ek varlıklar ayrıca sıralanır.

## Devam noktası
Şimdilik kodlama başlamaz. Bir sonraki planlama işi ikinci görsel yönü rafine etmek ve gerçek haftanın normal/aksayan gün ekran/eylem akışlarını somutlaştırmaktır; kullanıcı ikinci tasarımı daha yakın buldu, son hâlini onaylamadı. PRD O1–O7 kararları ilgili aşamada ele alınır, bu adım Faz 0'ı otomatik başlatmaz. Sorular kısa toplu gruplar hâlinde; zaten cevaplanan ayrıntılar tekrar sorulmaz. Yeni oturumda AGENTS + PROJECT-CONTEXT + CONTINUATION, ardından ilgili kaynak bölümleri okunur.
