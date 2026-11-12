# 01 - Ürün Gereksinimleri: Gün Gün

Sürüm 1.1 — 12 Kasım 2026. Kullanıcının planlama görüşmesinde kabul ettiği değişiklikler işlendi. Proje hâlâ planlama aşamasında; bu belge kodlama veya yayın onayı değildir. Kaynak önceliği: AGENTS > PRD > SCREENS > DESIGN > ARCHITECTURE > DATA-MODEL > ROADMAP. Açık kararlar bölüm 10'dadır.

## 1. Amaç ve başarı

Gün Gün, günlük karar yükünü azaltan ve kişisel para yönetimini görünür kılan, tek kullanıcılı Türkçe planlayıcıdır. Kullanıcı ne yapacağı belli olduğunda başlayıp sürdürebiliyor; temel ihtiyaç, önemli işlere yer ayırmak ve ödemeleri unutmamaktır. Uygulama teşhis koymaz veya başarıyı kişiliğe bağlamaz.

İlkeler: az giriş, az tekrar, sakin dil; belirli saatli işler ile esnek işler ayrı; plan bozulunca toparlanma; tüm boşlukları doldurmama; yerel veri ve şifreli yedek. Tamamlama tek dokunuştur; süre, not ve puan zorunlu değildir. Bir bilginin değişmesi bağlı ekranlara yansır, aynı kayıt tekrar istenmez.

Başarı: ilk anlamlı kullanım yaklaşık 3 dakika; gider girişi ortalama <=10 saniye; temel akışlarda 2–3 adım hedefi; sayı tuşlarına basış hariç hızlı gider en fazla 4 gezinme/onay dokunuşu. Kullanıcı günlük karar yükünün azaldığını bildirir. Gerçek veri pilotundan önce yedek geri yükleme kanıtlanır. Bildirim güvenilirliği gerçek Android cihazda ölçülür; sınırsız teslim garantisi verilmez.

## 2. Kullanım bağlamı

- Mesai hafta içi 08:00–17:00. Ek iş bu aralıkta uygun boşlukta yapılır; 10:00 veya 15:00 gibi sabit başlangıcı yoktur. Süre hedefi ayarlanabilir; eski bir saat varsayımı zorunluluk değildir.
- Akşam kişisel planlama için yaklaşık 18:30 sonrası uygundur. Uyku saati, görüşme ve antrenman süreleri henüz netleşmedi; geceyi bütünüyle dolu kabul etme.
- Öğle arasında oğulla görüşme yoktur. Cumartesi sabit görüşme günü olarak düşünülüyor; hafta içi 2–3 akşam isteği var, Salı/Perşembe öncelikli tercihlerdir. Gün/saatler annesiyle kesinleşir. Tercih, kesin randevu değildir; 13:00–18:00 gibi eski saatler otomatik atanmaz.
- Evde fitness bir gün çalışma/bir gün dinlenme döngüsündedir. Program içeriği OpenGym'de yönetilir; Gün Gün zaman ayırma ve tamamlama sağlar. API/uygulama entegrasyonu vaat edilmez.
- Boks henüz kayıt aşamasındadır; gün, saat ve başlangıç tarihi bilinmiyor. Kayıt/arama bir hazırlık görevi olabilir. Önceki belgelerdeki Aralık başlangıcı ve salsa rutini kesin plan kabul edilmez; yeni etkinlik olarak eklenebilir.
- Dil öğrenme, okuma, yeni hedefler; sinema, tiyatro, arkadaş buluşmaları ve geziler aynı yapıdan eklenebilir.
- Yeni ev faturalarının ilk tutarları bilinmeyebilir. Otomatik ödeme mümkün; banka bağlantısı olmadan talimatın gerçekleştiği varsayılmaz. Harcama dağılımı ve birikim takibi temel ihtiyaçtır.
- Kişisel tutarlar ve özel profil bilgileri depoya girmez; ayarlar kullanıcı tarafından çalışma anında girilir.

## 3. Sürüm kapsamı

| Aşama | İçerik |
|---|---|
| MVP / Faz 0–7 | Kademeli karşılama; Bugün/Plan; saatli, esnek ve döngüsel aktiviteler; haftalık planlama; hızlı kayıt; temel hedef ve küçük adım bağlantısı; planı toparlama; Android bildirim yaklaşımının erken doğrulanması ve temel hatırlatmalar; temel hesap/gelir/gider/fatura; aylık kategori dağılımı, birikim ve ödeme görünümü; kart/taksit; güvenli yedek; tema/çevrimdışı; fiş fotoğrafı ve .ics |
| v1 / Faz 8–11 | Ayrıntılı hedef/öğrenme günlüğü ve gelişim istatistikleri; bütçe limitleri, ileri grafikler; maddi hedefler; isteğe bağlı PIN/biyometri. Seri gibi göstergeler ancak ayrıca onaylanan tasarımla, ikincil ve isteğe bağlı |
| v2 / Faz 12–14 | Kullanım kanıtıyla hatırlatma iyileştirmeleri; fiş QR/OCR; isteğe bağlı gelişmiş kural önerileri/AI |
| Ultimate | Şifreli senkronizasyon, widget, Play Store, canlı kur, otomatik Drive yedeği, ek varlık türleri; ayrıca değerlendirilir |

Kapsam dışı: çok kullanıcı, sosyal ağ, banka bağlantısı, otomatik ödeme başlatma, iOS'a özel geliştirme, başka arayüz dili, reklam/analitik. Uzman fitness/dil eğitim içeriğini yeniden üretmek ve zorunlu üretken AI kapsamda değildir.

## 4. MVP davranışları

### F1. Kademeli karşılama
Kısa giriş: veri telefonda kalır, temel müsait zamanlar, ilk anlamlı iş. Ayrıntılı finans, spor, hedef, tema ve bildirim tercihleri ilgili bölüm ilk kullanıldığında tamamlanır. Adımlar atlanabilir, taslak saklanır, eksik alanlar uydurma değerlerle doldurulmaz. Bildirim izni ihtiyacı açıklanıp uygun bağlamda istenir. Finans ilk kullanımı hesap/varlık ve bakiye gereksinimini açıklar.

### F2. Bugün
Üç bölüm: **Zamanı belli**, **Uygun olduğunda**, **Dikkat gerekiyor**. Kesin randevular saatlidir; esnek işler zaman aralığında görünür ve yapay gecikme üretmez. Ödeme/izin sorunu gibi önemli durumlar görünürdür. En fazla üç özet uyarı, geri kalanına açık erişim; kritik öğeler sessizce gizlenmez. Tamamlananlar sakin bir listede tutulur. Hedef/başarı yüzdesi ve seri ana ekranın merkezi değildir.

### F3. Haftalık planlama ve Plan
Gün/hafta/ay görünümü. Kısa haftalık gözden geçirme: kesin görüşme/dersler → aktif hedefler → esnek işlerin uygun günleri → boşluk ve dinlenme. Tercih ve onaylanmış randevu ayrı görünür. Eksik süre/saat kapasite hesabında belirsizlik olarak gösterilir. Çakışma veya fazla yük için öneri sunulur; onaysız taşıma yapılmaz. Ek işin iş penceresi içinde bulunması çakışma değildir. Fitness döngüsü hafta sınırında sıfırlanmaz.

### F4. Hedef, aktivite ve etkinlik
Hedef sonuçtur; aktivite onu ilerleten eylemdir; etkinlik belirli veya henüz kararlaştırılmamış bir buluşmadır. Aktiviteler bir hedefe bağlanabilir ama zorunlu değildir. Tekrarlı çalışma saatli veya esnek; tekrar sabit hafta günleri, gün aralığı veya haftalık sıklık olabilir. Dil çalışması örneği: haftada üç oturum; kitap okuma bağımsız rutin olabilir. Tamamlama hedefte de görünür; ikinci bir tamamlanma kaydı istenmez. Hedefte otomatik sahte yüzde yok; oturum/eylem sayısı gösterilir, sayısal hedef varsa ayrı tanımlanır.

### F5. Hızlı kayıt ve hazırlık
Tek satırla fikir/görev yakalanır; başlık dışında tarih, kategori veya hedef zorunlu değildir. Tarihsiz kayıtlar “Yapmak istediklerim” alanında bekler, gecikmiş sayılmaz. Daha sonra görev, hedef veya etkinliğe dönüştürülür; dönüşüm kopya üretmez. Bilet alma, rezervasyon veya salonu arama ilgili etkinliğe bağlı hazırlık görevi olabilir. Ön koşul tamamlanmadıysa etkinlik kesinleşmiş gösterilmez.

### F6. Planı toparlama
“Bugün yetişmeyecek”: başka güne taşı, bu haftadan çıkar veya aktiviteyi/hedefi duraklat. İşler otomatik yarına yığılmaz. Tek oluşumu taşıma ile bütün tekrar kuralını değiştirme ayrıdır; sonraki tarihlere etkisi önizlenir. Fitness ertelenirse döngünün kayıp kaymayacağı açık karardır (bölüm 10); sessizce seçim yapılmaz. Kaçırılmış işlerden bir başarısızlık skoru üretilmez.

### F7. Aile zamanı ve notlar
Tercih edilen günlerden kesin randevuya geçiş kullanıcı onayıyla olur. Bir haftada birden fazla görüşme desteklenir; yalnız Cmt/Paz seçicisi kullanılmaz. İsteğe bağlı anı notları korunur. Gerçekleşmiş süre ile planlanan süre ayrıdır; eski zorunlu öğle/haftalık saat hedefi kaldırılmıştır.

### F8. Hedefler ve kategoriler
Hedef durumları: aktif, daha sonra, duraklatılmış, tamamlanmış. Aktif hedefe küçük adımlar ve tekrarlar bağlanabilir. Notlar isteğe bağlıdır; her akşam bütün hedeflere not yazma zorunluluğu ve varsayılan 21:30 baskısı yoktur. Kategori türleri aktivite/hedef/gelir/gider/sabit; ad, ikon, 8 renk, sıralama ve arşiv. Sistem kategorileri silinmez, yeniden adlandırılabilir.

### F9. Hesaplar, işlemler ve maaş
TRY banka/nakit, USDT ve gram altın; elle kur ve güncellik etiketi. Pozitif tam sayı tutarlar, varlık ölçekleri, çift tutarlı dönüşüm; gelir/gider düzenleme, silme/geri al, arama ve ay filtresi. Maaşın tarihe bağlı otomatik kaydı ve kaçırılan ay telafisi korunur; tutar değişikliği gelecekteki aydan geçerlidir. Bu kayıt banka doğrulaması değildir; gerçekleşmeyen gelirin düzeltilme akışı gerekir.

### F10. Faturalar ve düzenli ödemeler
Düzenli gider tanımı ile her dönemin faturası ayrılır. Tutar bilinmiyor/tahmini/kesin olabilir; vade bilinmiyorsa uydurma tarih atanmaz. Ödeme bekliyor/kısmi/ödendi durumları, kalan tutar ve kayıtlı ödemeler görünür. Otomatik ödeme talimatı ayrı bilgi, ödeme gerçekleşmesi ayrı olaydır. Vade gelmesi otomatik olarak “ödendi” yapmaz. “Ödedim” tek işlemle faturayı ve harcamayı günceller; tekrar dokunma çift harcama üretmez. Hatırlatmayı ertelemek vadeyi değiştirmez. Fatura/ödeme düzeltmeleri bakiye ve uyarıyla tutarlı kalmalıdır.

### F11. Para özeti ve temel aylık dağılım
MVP'de gelir/gider, kategori başına harcama, birikime ayrılan tutar, yaklaşan ödemeler, maaşa kadar yükümlülükler ve tutarı bilinmeyen faturalar görünür. Tahmini kullanılabilir para, seçili likit hesaplar ve kapsanan dönemle açıklanır; altın/USDT otomatik harcanabilir nakit sayılmaz. Bilinmeyen tutar/vade varsa sıfır kabul edilmez, sonuç eksik/tahmini etiketlenir. Kart alışverişi gider, kart ödemesi nakit akışıdır; aynı hesapta iki kez düşülmez. Kesin formül ve kart ödeme ayrıntıları bölüm 10'daki tasarım kapısına bağlıdır.

### F12. Kart, taksit ve birikim
Kart limit/kesim/son ödeme; taksit önizlemesi, son taksitte kuruş farkı, posted satırları koruma, 12 aylık yük. Taksitlerin ekstreye yansıması ile gerçek ödeme ayrıdır. Kısmi ekstre ödemesi, iade ve düzeltmeler tasarlanıp test edilmeden finans akışı tamamlanmış sayılmaz. Birikim hedefi, “ayırmayı planladım” ve gerçek transfer ayrı gösterilir; henüz aktarılmamış tutar gerçekleşmiş birikime yazılmaz.

### F13. Hatırlatmalar
Temel hatırlatma ilk kullanılabilir sürüm ihtiyacıdır; v2'ye ertelenmez. Saatli etkinlikte hazırlık/ulaşım payı; esnek işte kullanıcının seçtiği pencere; tarihsiz fikirde haftalık gözden geçirme. Fatura ve alışkanlık farklı önem/tekrar tercihleri taşır. Sessiz saatler, kategori tercihi, tamamla/ertele/bugün olmayacak eylemleri; finansal onay için gerekli alanlar eksikse ilgili form açılır. Tamamlanan/taşınan/silinen kaydın eski bildirimi iptal edilir. Erteleme geçmiş bildirimi sonsuz çoğaltmaz. İzinler ve Android kısıtları açık gösterilir. Platform seçimi ve cihaz kanıtı Faz 0'dadır; PWA veya Capacitor seçildiği varsayılmaz.

### F14. Yedek, veri güvenliği ve takvim
Gerçek veriyle kullanım başlamadan şifreli `.gungun` yedek ve geri yükleme hazır olmalıdır. Parola zorunlu; fişler isteğe bağlı. Geri yüklemede özet ve üzerine yazma onayı; önce şifreli kurtarma yedeği önerisi; yanlış parola/bozuk veri mevcut veriyi değiştirmez. Birleştirme, çakışma/silinme politikası kararlaştırılıp test edilene kadar sunulmaz. Kalıcı depolama durumu ve yedek hatırlatması görünürdür. .ics yardımcı dışa aktarımdır, teslim garantisi veya temel bildirim çözümünün yerine geçen vaat değildir. Tekrar içe aktarmada takvim sağlayıcısına bağlı çoğalma riski açıklanır.

### F15. Fiş, tema ve erişilebilirlik
Fotoğraf çek/seç, küçült ve işleme bağla; OCR sonraki sürüm. Açık/koyu/sistem tema, 48 dp hedefler, %130 yazı, hareket azaltma, TalkBack ve WCAG AA. Kaynak: 03-DESIGN.md. Etkinlikten gider eklerken tarih/açıklama ve varsa tahmin hazır gelir; kaydetme kullanıcı onayı ister ve ikinci işlem üretmez.

## 5. v1 ayrıntıları
Derin hedef günlüğü, gelişim ve hafta sonu istatistikleri, bütçe limitleri/uyarıları, özel SVG grafikler, maddi hedefler, PIN/WebAuthn kilidi. Basit kategori dağılımı ve birikim takibi zaten MVP'dedir. Kilit veritabanını şifrelemez; dürüstçe açıklanır. İsteğe bağlı seri/halka ancak ayrı tasarım onayıyla; eski zorunlu %60/joker hesabı geçerli değildir.

## 6. v2 ve sonrası
Bildirimler için gerçek kullanımda görülen eksikler; fiş QR → cihaz içi OCR, 20 örnekte toplam tutar doğruluğu >=%90 olmadan otomatik kayıt yok; sunucu/AI her aktarım için açık onay ister. Gelişmiş öneriler önce açıklanabilir kurallarla; AI isteğe bağlı ve varsayılan kapalıdır.

## 7. Fonksiyonel olmayan gereksinimler
MVP/v1 verisi yerelde, dış servis/analitik yok (uygulama dosyalarının kurulması/güncellenmesi hariç). Android odaklı; web/PWA uyumluluk hedefi Chrome son iki majör sürüm. Yerel sarmalayıcı seçilirse işletim sistemi destek aralığı Faz 0'da doğrulanır. Türkçe, tr-TR, Europe/Istanbul, Pazartesi hafta başlangıcı. Her çoklu yazma atomik, şema değişimi migrasyonlu; performans 04 bölüm 10. İlk sürüm platformu henüz seçilmedi.

## 8. Ton
Kısa, sade, yargısız; hata neden ve çözüm söyler. Tekrar tekrar not/süre/puan isteme. Yüzde ve seri kaybıyla baskı kurma; plan değişikliğini olağan karşıla. Sadeleştirme finansal eksikleri gizlemek anlamına gelmez.

## 9. İzleme ve durum
Gerçek pilotta haftalık kısa görüşme: planlama yükü azaldı mı, hatırlatmalar yararlı mı, ödeme ve birikim görünümü anlaşılır mı? Analitik eklenmez; kullanıcı geri bildirimi ve cihaz kontrolleriyle değerlendirilir. Şu anda çalışan uygulama, test sonucu veya tamamlanmış faz yoktur.

## 10. Varsayımlar ve açık kararlar

| ID | Konu | Durum / ne zaman çözülür |
|---|---|---|
| O1 | PWA mı Android sarmalayıcı mı; bildirim teslimi, kurulum/güncelleme ve yerel veri | Capacitor erken değerlendirme adayı; seçilmedi. Faz 0 öncesi seçenekler, Faz 0 cihaz deneyi ve kullanıcı kararı |
| O2 | Fitness tek oturumu erteleyince döngü kayar mı? | Karar açık; iki davranış önizlemeyle karşılaştırılır, Faz 2'de netleşmeden otomatik kaydırma yok |
| O3 | Görüşme kesin gün/saat/süreleri, uyku, fitness süresi, boks takvimi | Kullanıcının planlama verisi; bu belge kesin değer uydurmaz. Tarihsiz/tercih kayıtlarıyla yararlı kullanım devam eder |
| O4 | Tahmini kullanılabilir para formülü, kart ödemesi dağıtımı, iade ve düzeltme | Finans detay tasarımı: Faz 3 temeli ve Faz 5 kart kapısı. Beklenen/gerçekleşmiş, borç/nakit ayrımı zorunlu |
| O5 | Yedek birleştirme çatışma ve silinme politikası | Henüz seçilmedi. Üzerine yazma + şifreli kurtarma yedeği temel akış; birleştirme onay/test gelene kadar kapalı |
| O6 | Bildirim tekrar sayısı/aralığı, sessiz saatler, kilit ekranı ayrıntısı | Kategori bazlı, kullanıcı kontrolünde; Faz 0/2 tasarımı ve cihaz denemesinde netleşir. Sabit 15:00 sorgusu yok |
| O7 | Nunito/Figtree, depo görünürlüğü ve güncel araç sürümleri | Font cihazda doğrulanacak; açık depo varsayımı korunur, görünürlük değiştirilmedi; sürümler resmî kaynaktan kurulumda doğrulanır |

Yeni öneri onaylı karar gibi yazılmaz. Bu sürümde kullanıcıca kabul edilen yön: üçlü plan modeli, haftalık gözden geçirme, hızlı kayıt, tek veri girişi, erken bildirim/yedek, temel finans ve baskısız toparlanma. Yeni ürün kararı gerekiyorsa kısa toplu sorular kullanılır.
