# 02 - Ekranlar ve Akışlar

12 Kasım 2026 planlama güncellemesi. Görsel kaynak 03-DESIGN.md; kapsam 01-PRD.md. Arayüz örnekleri ürün tasarımıdır, çalışan ekran değildir. Mobil 360–412 dp; metinler i18n/tr.ts.

## 1. Gezinme ve ortak davranış
Alt çubuk: Bugün, Plan, Hedefler, Para, Daha fazla; ikon + yazı. Hash rotaları #/bugun, #/plan, #/hedefler, #/para, #/daha-fazla korunur. “Ekle” ortak eylemi hızlı kayıt/görev, etkinlik, gider, gelir ve hedef eklemeye erişir; tek satırlık hızlı kayıt en az alanla açılır. Para'da gider kısayolu kolay erişilir. Kesin yerleşim maketle değerlendirilir.
Formlar alt sayfa/tam sayfa, geri tuşu bir adım geri; dolu formu terk ederken taslağı koruma veya vazgeçme onayı. Tek dokunuş tamamlar; not/süre isteğe bağlı. Bir kaydın farklı ekranları aynı kaydı kullanır. Bildirim eylemi de aynı iş kuralını çağırır.

## 2. Ekranlar

### 2.1 Karşılama
Kısa veri/gizlilik açıklaması → temel müsait zamanlar → ilk iş veya Bugün. Ayrıntılar atlanabilir ve taslak kalır. İlk Para kullanımında hesap/varlık, ilk hatırlatmada neden izin gerektiği açıklanır. Altı uzun adımı bitirmeden kullanım engellenmez. Finans/saat eksikleri tamamlanmış gibi gösterilmez.

### 2.2 Bugün
Başlık ve tarih, gerekirse tek kısa motivasyon cümlesi. Altında üç sakin liste:
1. **Zamanı belli:** kesinleşmiş randevu ve ders; saatli zaman çizgisi burada, yalnız kesin saatler için. Hazırlık görevi ve ulaşım payı görünür.
2. **Uygun olduğunda:** mesai içinde ek iş, seçilen gündeki esnek dil/okuma/fitness. Yapay başlangıç saati/kalan dakika sayacı yok. Pencere devam ederken “gecikti” yazılmaz.
3. **Dikkat gerekiyor:** yaklaşan/geçmiş fatura, kesinleştirilecek görüşme, bildirim izni veya yedek ihtiyacı. En fazla üç özet satırı + kalan sayısı/Tümünü gör; ciddi eksikler kaybolmaz.
Tamamlananlar katlanabilir listede; tek satırlık para özetinden Para'ya geçilir. Seri ve başarı yüzdesi başlıkta zorunlu değildir. “Bugün yetişmeyecek” ilgili işin menüsünde; bütün günü başarısız ilan eden düğme yok.

### 2.3 Plan ve haftalık gözden geçirme
Gün/Hafta/Ay; hafta telefonda dikey gün listesi, ayda kısa işaretler. Saatli randevular, esnek kapasite ve kesinleşmemiş tercihler ayrı etiket taşır. Cumartesi ve Salı/Perşembe aile görüşmesi tercihleri onaylanana kadar “Kesinleşmedi”; öğle görüşmesi üretilmez.
Haftayı planla: (1) görüşme/dersleri kesinleştir, (2) aktif hedefleri seç, (3) esnek işleri günlere dağıt, (4) boşluk/dinlenme ve kapasiteyi gözden geçir, (5) değişiklik özetini onayla. Otomatik öneri önizlemedir; sessiz takvim değişikliği yok. Her adımda taslak saklanır. Bilinmeyen süreler kapasite hesaplandı gibi gösterilmez.

### 2.4 Aktivite ve etkinlik
Başlık; Görev/Etkinlik; hedef bağlantısı isteğe bağlı. Zaman: saatli, gün/pencere içinde esnek veya henüz tarihsiz. Tekrar: hafta günleri, her N günde bir, haftada N oturum; başlangıç/isteğe bağlı bitiş. Fitness'ta başlangıç tarihi üzerinden 2 günlük aralık önizlemesi, haftalık sabit gün zorlaması yok.
Sinema/tiyatro/gezi: yer, tahmini harcama, hazırlık/ulaşım hatırlatması isteğe bağlı. “Bilet al” gibi bağlı görev eklenebilir. Gerçek harcama eklerken tarih/açıklama/tahmin önceden dolu gelir; kullanıcı onaylar. Dış uygulamadaki fitness/dil programını tekrar girmek gerekmez; otomatik bağlantı/senkronizasyon vaat edilmez.

### 2.5 Hızlı kayıt ve Yapmak istediklerim
Tek başlıkla kaydet; kategori/tarih zorunlu değil. Plan ve Hedefler'den erişilen tarihsiz liste; haftalık gözden geçirmede hatırlanır, bildirim yağmuru veya gecikme sayacı yok. “Göreve dönüştür / Etkinlik planla / Hedef yap” var olan yakalama kaydını hedef kayda bağlar; iki bağımsız iş üretmez. Hazırlık görevi tamamlanmadan etkinlik için yanlış hazır bilgisi verilmez.

### 2.6 Toparlama ve erteleme
İş menüsü: Başka güne taşı / Bu haftadan çıkar / Duraklat. Kaydırma öncesi etkilenen randevu ve hatırlatmalar gösterilir. “Yalnız bu sefer” ile “Tekrar düzenini değiştir” açık ayrılır. Fitness döngüsünü kaydırma politikası PRD O2 kararıdır; önizleme ve onay olmadan sonraki oturumlar değişmez. Erteleme hedefte ikinci tamamlanma yaratmaz, eski bildirimi iptal eder. İptal/atlama ile finansal vade erteleme birbirine karıştırılmaz.

### 2.7 Hedefler
Aktif / Daha sonra / Duraklatılmış / Tamamlandı. Hedef detayında sonuç/isteğe bağlı tarih, sıradaki küçük adımlar, bağlı düzenli aktiviteler ve isteğe bağlı notlar. Aktivite tamamlanınca burada görünür; ikinci onay istemez. Temel oturum sayısı MVP; detaylı öğrenme günlüğü/grafikleri v1. Hedefi duraklatırken bağlı gelecek işler ve bildirimlere etkisi özetlenip onaylanır; geçmiş silinmez.

### 2.8 Para
Özet / İşlemler / Hesaplar / Kartlar / Faturalar ve düzenli gelirler.
- Özet: bu ay gelir/gider, kategori dağılımı (sade liste), birikim planlanan/gerçekleşen, yaklaşan ödemeler ve maaşa kadar yük. Tahmini kullanılabilir para yanında dönem, seçili hesaplar ve eksik bilgiler. Tutar bilinmeyen faturalar görünür; sıfır gibi hesaplanmaz.
- Hesaplar: banka/nakit TRY, USDT, altın; bakiye, TL karşılığı, kur tarihi, transfer/dönüşüm. Likit bütçe ile toplam varlık ayrılır.
- İşlemler: ay, arama, kategori; hızlı gider tutar → kategori → kaydet; hesap/tarih geçerli varsayılanı varsa doldurulur, yoksa sorulur. Not/fiş/taksit ayrıntıları genişletilir. Silme 8 sn geri al.
- Faturalar: dönem, sağlayıcı/kategori, bilinmeyen/tahmini/kesin tutar, vade bilinmiyor/var, bekleyen/kısmi/ödendi, otomatik ödeme talimatı ayrı etiket. “Ödedim” ödeme hesabı/tutar/tarih doğrulanarak tek kayıt yaratır. Bildirimden gerekli alanlar eksikse bu form açılır. Kısmi ödemede kalan görünür; tahmini tutarlı dönem kesinleşmeden tam ödendi sayılmaz.
- Kartlar: limit, ekstre, borç, son ödeme, Taksitlerim; yansıyan taksit ve yapılan ödeme farklı etiket. Kısmi ödeme/iade/düzeltme akışı PRD O4 kararından sonra tamamlanır.

### 2.9 Birikim ve Taksitlerim
Birikim: hedef tutar, ayrılacak ve gerçekten aktarılan ayrı; “Sonra aktarırım” geçmişi değiştirmez. Transfer iki hesap/tutarla, taslak önizleme sonrası oluşur. Taksitler: ürün, ilerleme (ekstreye yansıyan), kalan, bitiş, gelecek 12 ay; son kuruş farkı önizlenir. Örnek 1.000 TL/6 = 5×166,66 + 166,70. Bekleyen satır değişikliği ile posted satır ayrılır.

### 2.10 Hatırlatma ayarları ve eylemleri
Kategori bazında açık/kapalı, uygun zaman aralığı, sessiz saatler, tekrar tercihi, kilit ekranı ayrıntısı. İzin/Android kısıtı durumu ve açıklayıcı çözüm yolu; izin reddi kullanımın geri kalanını engellemez. Örnek hatırlatma cihaz denemesi. Tamamla/sonra hatırlat/bugün olmayacak; ödeme tamamlamada doğrulama gerekir. Tamamlanan/taşınan/silinen kayıtların eski bildirimleri geçersizleşir. Tekrarlanan bildirime basış çift veri üretmez.

### 2.11 Yedek ve depolama
Son yedek, kullanım/kalıcılık; yedek al: parola iki kez, güç göstergesi, fiş dahil/hariç, paylaş/indir. Geri yükle: dosya → parola → doğrulama → özet → şifreli kurtarma yedeği önerisi → üzerine yazma açık onayı. Yanlış parola/bozuk dosya mevcut veriyi değiştirmez. Birleştirme hazır seçenek değildir (PRD O5). Yedek unutulursa verinin kurtarılamayabileceği kısa açıklanır.

### 2.12 Daha fazla ve sonraki ekranlar
Anılar, kategoriler, yedek, .ics, bildirimler, ayarlar, hakkında; v1'de ayrıntılı istatistik/bütçe/maddi hedef/kilit. Ayarlar: müsait zaman, tema, tekrar, izinler, depolama, sürüm, yalnız geliştirmede uydurma demo verisi, tüm veriyi sil (yedek önerisi + çift onay).

## 3. Kritik akışlar
A1 kısa karşılama ve ilk esnek iş; A2 ek işin farklı mesai saatinde tamamlanması; A3 hızlı gider ve aylık kategori dağılımı; A4 kart/taksit; A5 maaş ve birikim planı/transfer ayrımı; A6 toparlama, otomatik yarına yığmama; A7 yedek gidiş-dönüş; A8 kesin saatli ders çakışması; A9 çevrimdışı; A10 tercih görüşmenin birden çok kesin randevuya dönüşmesi. Ek senaryolar B1–B10, 07-TEST-PLAN.md'de.

## 4. Her ekranın durumları
Yükleniyor (300 ms altı iskelet göstermeme), boş (davet/eylem), hata (neden/çözüm), kısa başarı, geri al; kısmi bilgi/taslak ve izin reddi. Çevrimdışı normal çalışma durumudur. Yeni sürüm veya geri yükleme yarım formu kaybettirmez.
