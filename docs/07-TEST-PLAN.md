# 07 - Test ve Kalite Planı (Gün Gün)

Amaç: veri kaybı olmaması, hesapların doğru çıkması, uygulamanın gerçek telefonda yormayan şekilde çalışması. Kural: kanıtsız "bitti" yok; her görev test çıktısıyla kapanır.

## 1. Seviyeler ve araçlar

| Seviye | Araç | Neyi korur | Hedef |
|---|---|---|---|
| Birim (core) | Vitest, fast-check | Para, taksit, ekstre, tekrar kuralı, çakışma, seri, değerleme | `core/money`, `finance`, `schedule`, `streak` satır kapsamı >= %90 |
| Veri (data) | Vitest + `fake-indexeddb` | Depolar, transaction bütünlüğü, migrasyon, yedek gidiş-dönüş, `autoKey` benzersizliği | Her depo için CRUD + silme/geri alma; her şema sürümü için migrasyon testi |
| Bileşen | Testing Library | Formlar, kartlar, hata mesajları, erişilebilir adlar | Ana formların hepsi |
| Uçtan uca | Playwright (Pixel 7 emülasyonu) | A1-A10 akışları, çevrimdışı, tema | Her faz kapısında ilgili senaryolar |
| Erişilebilirlik | `@axe-core/playwright` | Kontrast, ad/rol, odak | Kritik/ciddi 0 |
| Statik | Biome, `tsc`, dependency-cruiser, emoji/hex taraması | Katman ihlali, sabit renk, emoji | CI'da zorunlu |

## 2. Özellik tabanlı (property) değişmezler
- Taksit: Σ taksit = toplam; her taksit > 0; sayı = n; yalnızca son taksit diğerlerinden büyük olabilir ve fark < n kuruş.
- Para: `valueInTRY` negatif bakiye için simetrik; sıfır kur reddedilir; BigInt sonucu `Number.isSafeInteger`.
- Tekrar: bir haftalık `expandOccurrences` sonuçları sıralı, tekrarsız; gün sayısı kural günleriyle eşit.
- Çakışma: sonuç simetrik (a,b çakışıyorsa b,a da); kendisiyle çakışmaz.
- Telafi: `runDailyJobs` iki kez art arda çalışınca ikinci çalışma 0 kayıt üretir.
- Yedek: `restore(backup(db))` -> veri tablo tablo eşit (`receipts` dahil/hariç).

## 3. Sabit test vektörleri (05-DATA-MODEL.md'dekiler birebir)
- 1.000,00 TL / 6 -> 5 x 166,66 + 166,70. 12.999,00 / 6 -> 6 x 2.166,50.
- 125,5 USDT x 41,35 -> 5.189,43 TL. 3,250 g x 4.850,25 -> 15.763,31 TL.
- Ekstre: S=20, D=5: 20 Mart -> kapanış 20 Mart, son ödeme 5 Nisan; 21 Mart -> 20 Nisan / 5 Mayıs. S=10, D=25: 10 Mart -> 10 Mart / 25 Mart. Şubat'ta S=31 -> 28/29.
- Seri: 3 tutuldu + 1 plansız (joker var) + 1 tutuldu = 5; aynı hafta ikinci plansız -> 0.
- Maaş: 15'i; uygulama 3 ay açılmadı -> 3 kayıt, tarihleri 15'i, ikinci açılışta yeni kayıt yok. 31'inde olan sabit gider Şubat'ta 28/29'a düşer.
- Aralık boks: haftada 1 gün 1 saat; spor Çarşamba 20:00, boks Çarşamba 20:00 -> çakışma + "spor Perşembe 20:00" önerisi.

## 4. Kabul senaryoları (Given / When / Then) - 02-SCREENS.md bölüm 3'ün kısaltılmışı
A1 İlk açılış: Given boş veritabanı; When karşılama varsayılanlarla bitirilir; Then Bugün'de haftanın rutinleri (iş, öğle, ek iş, spor) görünür ve `settings.onboardingDone=true`.
A2 Ek iş kartı: Given hafta içi 15:01; When Evet; Then log `done`, kart gizlenir, gün özetinde işaretli.
A3 Hızlı gider: When Ekle>Gider>125>Market>Kaydet; Then işlem posted, Para'daki "bu ay gider" 125 TL artar; <= 4 dokunuş.
A4 Taksit: When 12.999 TL, 6 taksit, kart, Kasım; Then 6 planned satır, Taksitlerim'de 1 ürün, bu ay 2.166,50 TL.
A5 Maaş + birikim: Given 15'i geçti; When uygulama açılır; Then maaş kaydı var, birikim kartı görünür; [Biriktirdim] -> transfer.
A6 Joker: When Plansız gün; Then seri korunur; When aynı hafta ikinci; Then seri 0 ve yargısız mesaj.
A7 Yedek: When yedek al, tüm veriyi sil, geri yükle; Then tablo satır sayıları ve toplam birikim aynı.
A8 Boks çakışması: yukarıdaki vektör.
A9 Çevrimdışı: Given `setOffline(true)`; When A1-A8 işlemleri; Then hepsi çalışır, ağ isteği 0.
A10 Oğul günü: When Pazar seçilir; Then 13:00-18:00 bloğu Pazar'a geçer, Cumartesi "serbest".

## 5. Veri güvenliği testleri (sıfır tolerans)
- Transaction ortasında hata: yarım kayıt kalmaz (taksit planı, transfer, geri yükleme).
- Yanlış parola/bozuk yedek: mevcut veri değişmez, anlaşılır hata.
- Eski şema fixture'ı yeni sürüme yükselir; hiçbir kayıt kaybolmaz.
- Aynı `autoKey` ikinci kez eklenemez.
- Silme geri alınır; 30 gün sonra temizlenir; yedek silinmiş kayıtları taşımaz.
- Hiçbir ağ isteği (Playwright `route` ile dış isteği başarısız say).

## 6. Gerçek Android cihaz kontrol listesi (Faz 0, 2, 4, 6, 7'de kullanıcıyla)
[ ] Pages adresinden "Uygulamayı yükle" -> ana ekran simgesi, tam ekran açılış.
[ ] Uçak modunda aç ve tüm temel işlemleri yap.
[ ] Geri tuşu: sayfa/alt sayfa kapanır, uygulamadan istemsiz çıkmaz.
[ ] Klavye açıkken alanlar kapanmaz; sayı tuş takımı tutar girişinde açılır.
[ ] Koyu tema + sistem teması otomatik; yazı boyutu %130.
[ ] Yedek: Paylaş menüsü Drive/Dosyalar'a kaydediyor mu; geri yükle dosya seçiciden açılıyor mu.
[ ] Fiş: kamera izni ve galeri; fotoğraf küçülüyor mu (Ayarlar > Depolama).
[ ] Kalıcı depolama durumu Ayarlar'da görünüyor.
[ ] Chrome "site verilerini temizle" TEST EDİLMEZ (veri silinir); yerine yedek-geri yükleme testi.
[ ] Yeni sürüm yayınlanınca "Yeni sürüm hazır" bildirimi geliyor, onaylayınca güncelleniyor, yarım form kaybolmuyor.
[ ] TalkBack ile Bugün ve gider ekleme akışı anlaşılır.
[ ] Pil/performans: ekranlar akıcı, açılış 3 sn altında.

## 7. Tanım: "bitti"
Kod + test + belge güncel + `pnpm check` yeşil + ekran görüntüsü (UI ise) + kullanıcıya düz dille anlatım. Biri eksikse bitmemiştir.

## 8. Hata ayıklama disiplini
Önce hatayı yeniden üreten test yaz (veya adımları yaz), tek cümlelik varsayım, en küçük düzeltme, testi geçir, regresyon testi bırak. "Çalışıyor gibi" yok; gözle ve çıktıyla göster.
