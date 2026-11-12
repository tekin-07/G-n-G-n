# 06 - Yol Haritası ve Faz Planı (Gün Gün)

Çalışma biçimi: her seferinde TEK faz. Faz bitmeden sonrakine geçilmez. Her fazın sonunda "faz kapısı" geçilir: `pnpm check` yeşil + kabul ölçütleri kanıtlı + kullanıcı kontrolü + etiket (`git tag faz-N`). Boyutlar göreceli (S küçük, M orta, L büyük); süre sözü verilmez.

Görev durumu bu dosyada işaretlenir: `[ ]` yapılmadı, `[x]` bitti (ajan, kanıtla birlikte işaretler).

## Faz kapısı (her faz için ortak)
1. `pnpm check` (biome + typecheck + vitest + depcheck + build) yeşil.
2. Yeni davranışlar için test var; test çıktısı özetlendi.
3. Mobil 360/412 px, açık ve koyu tema ekran görüntüsü + 03-DESIGN.md bölüm 11 öz-eleştirisi.
4. `axe` taraması: kritik/ciddi ihlal 0. Emoji ve sabit hex taraması temiz.
5. Çevrimdışı akış çalışıyor (Playwright `setOffline`).
6. Değişen belgeler güncellendi (veri modeli/mimari/PRD).
7. Kullanıcıya düz Türkçe özet + "nasıl kontrol edersin" adımları verildi; kullanıcı onayladı.
8. `git tag faz-N`, Pages'e dağıtım doğrulandı (varsa).

---

## MVP

### Faz 0 - İskele, CI, yayın, tema (M)
Amaç: boş ama kurulabilir, test edilen, canlı bir kabuk.
- [ ] 0.1 `.gitignore` İLK commit'ten önce: `node_modules`, `dist`, `*.gungun`, `.env*`, `docs/PROFIL.local.md`, `CLAUDE.local.md`, `playwright-report`, `test-results`, `coverage`. `git check-ignore -v docs/PROFIL.local.md` çıktısı gösterilir.
- [ ] 0.2 pnpm workspace; `tsconfig.base.json` (strict, noUncheckedIndexedAccess); Biome; dependency-cruiser (04 bölüm 4 kuralları); script'ler: `dev`, `build`, `check`, `lint`, `typecheck`, `test`, `test:e2e`, `depcheck`.
- [ ] 0.3 `packages/core` ve `packages/data` iskeleti, her birinde 1 çalışan test.
- [ ] 0.4 `apps/web`: Vite + React, hash router, 5 sekmeli kabuk (boş durum ekranlarıyla), `i18n/tr.ts`, ikon kayıt defteri, token'lar (açık/koyu/sistem), Nunito + Figtree self-host.
- [ ] 0.5 Türkçe harf ve tabular rakam test sayfası (geçici; faz sonunda kaldırılır); ekran görüntüsü kullanıcıya gösterilir (yazı tipi onayı, PRD varsayım 7).
- [ ] 0.6 Kontrast testi (03-DESIGN.md tabloları): her çift için oran >= eşik (otomatik test).
- [ ] 0.7 PWA: manifest (shortcuts dahil), geçici basit geometrik ikonlar (192/512/maskable; marka kopyası yok), service worker, `prompt` güncelleme bildirimi. CSP meta etiketi (04 bölüm 8) eklenir; bir alt sayfa (diyalog) ve bir görsel (`blob:`) CSP altında çalışır, tarayıcı konsolunda CSP ihlali 0.
- [ ] 0.8 CI (`ci.yml`) ve Pages dağıtımı (`deploy.yml`); `VITE_BASE`; `README.md` (kullanıcı için kısa); `docs/VERSIONS.md` (kurulan sürümler).
- [ ] 0.9 Playwright: uygulama açılır, 5 sekme gezilir, çevrimdışıyken yeniden açılır, `axe` temiz.
Kabul: Pages adresi açılır; Android Chrome'dan "Uygulamayı yükle" ile ana ekrana eklenir; uçak modunda açılır; açık/koyu tema geçişi çalışır.
Kullanıcı kontrolü: "Telefonda adresi aç, ana ekrana ekle, uçak moduna al, uygulamayı aç. Ekranda Türkçe harfler (ğ, ş, ı, İ) doğru mu? Renkler hoşuna gidiyor mu?"
Başlatma: "docs/06-ROADMAP.md'deki Faz 0'ı uygula. Önce plan çıkar, onayımı bekle."

### Faz 1 - Çekirdek, veri katmanı, ayarlar, karşılama (L)
- [ ] 1.1 `core/time`, `core/money` (varlıklar, biçimlendirme, BigInt değerleme, yuvarlama) + 05'teki test vektörleri.
- [ ] 1.2 Zod şemaları (tüm MVP varlıkları) ve türleri; `Result` türü.
- [ ] 1.3 `ports` + Dexie `db.ts` (sürüm 1, 05'teki şema) + depo uygulamaları; soft delete + geri al altyapısı; tek transaction yardımcıları.
- [ ] 1.4 `storage`: `persist()`/`estimate()`; Ayarlar > Depolama göstergesi.
- [ ] 1.5 Ayarlar deposu + varsayılanlar; tohum kategoriler (Türkçe, ikon + renk anahtarı).
- [ ] 1.6 `core` rutin üreteci (`seedRoutines(settings)`) -> sistem aktiviteleri (05, tablo "Tohum sistem rutinleri").
- [ ] 1.7 Karşılama ekranları (F1, 6 adım), tema ayarı kalıcı.
- [ ] 1.8 Migrasyon test iskeleti (fixture v1).
Kabul: birim testler 05 vektörlerini geçer; sayfa yenilense/uygulama kapansa veri kalır; karşılama sonunda sistem rutinleri oluşmuştur (veritabanında kontrol testi); çevrimdışı.
Kullanıcı kontrolü: "Karşılamayı doldur, uygulamayı kapat-aç, bilgilerin duruyor mu? Ayarlar'dan bir saati değiştir."

### Faz 2 - Plan ve Bugün (L)
- [ ] 2.1 `core/schedule`: `expandOccurrences`, `autoplan`, `conflicts`, `weekChoice` + testler (05, 4.6; hafta içi/sonu, oğul günü Pazar'a geçiş, Aralık'ta boks çakışması).
- [ ] 2.2 Etkinlik ekle/düzenle (düzenli + günübirlik), aktivite kategorileri yönetimi.
- [ ] 2.3 Plan ekranı: gün/hafta/ay, hafta şeridi, oğul günü seçici, çakışma sayfası + öneri uygulama.
- [ ] 2.4 Bugün: Şimdi/Sıradaki, zaman çizgisi, blok ayrıntısı (Yaptım/Atla/Ertele/Not).
- [ ] 2.5 Kontrol kartları: ek iş (15:00 hafta içi, 12:00 hafta sonu), oğul notu; `FeatureModule` + `todayCards` kaydı.
- [ ] 2.6 Oğlunla haftalık saat sayacı + Anılar listesi.
- [ ] 2.7 `core/streak` + "Plansız gün" + joker + günlük halka (05, 4.5; testli).
- [ ] 2.8 Mini motivasyon sağlayıcısı (03, bölüm 10).
- [ ] 2.9 Playwright senaryoları A1, A2, A6, A8, A10.
Kabul: A1, A2, A6, A8, A10 geçer; çakışma önerileri onaysız uygulanmaz.
Kullanıcı kontrolü: "Haftanın planı sana doğru mu geliyor? Boks diye yeni bir etkinlik ekle (Aralık, haftada 1 gün 1 saat), spor günüyle çakışırsa öneri çıkıyor mu? Cumartesi/Pazar oğul gününü değiştir."

### Faz 3 - Hedefler (M)
- [ ] 3.1 Hedef kategorileri + hedef CRUD (yaşam hedefi), durumlar.
- [ ] 3.2 Akşam kartı (21:30, ayarlanabilir): tüm aktif hedefler tek sayfada, hedef başına not; hedef günlüğü zaman çizgisi.
- [ ] 3.3 Hedef detay, düzenle/duraklat/tamamla; kategori ekle/düzenle (renk + ikon).
- [ ] 3.4 Playwright: hedef ekle -> akşam notu -> detayda görünür.
Kabul: notlar tarih sırasıyla hedef günlüğünde; kategori sonradan eklenebilir.
Kullanıcı kontrolü: "Üç hedef ekle (tatil, ev, İngilizce), bir kategori yarat, akşam kartına not yaz."

### Faz 4 - Para 1: hesaplar, işlemler, gelir, sabit gider (L)
- [ ] 4.1 Hesaplar (banka, nakit, USDT, altın) + elle kur + "kur X gün önce" etiketi.
- [ ] 4.2 Hızlı gider/gelir ekleme (3 adım), düzenle, sil + geri al, liste, ay filtresi, arama.
- [ ] 4.3 Gelir kaynakları + maaş otomasyonu (`runDailyJobs`, 04 bölüm 5 ve 05 madde 4.7: telafi + idempotent `autoKey`) + zam (amounts).
- [ ] 4.4 Sabit giderler: tohum öneriler (kira, elektrik, su, doğalgaz, internet, telefon hattı, AI aboneliği), vade uyarısı (3 gün), "Ödedim", değişken tutar.
- [ ] 4.5 Aktarım/dönüşüm (TL -> USDT/altın) + "kuru güncelle?".
- [ ] 4.6 Para > Özet: toplam birikim (TL karşılığı), bu ay, yaklaşan ödemeler.
- [ ] 4.7 Testler: bakiye/net değer vektörleri, telafi (3 ay açılmamış), çift kayıt olmaması, sabit gider ay sonu sıkıştırma.
Kabul: A3, A5 (birikim kartı hariç) geçer; 125,5 USDT = 5.189,43 TL vektörü arayüzde de görünür.
Kullanıcı kontrolü: "Üç hesap aç, birkaç gider ekle, maaş otomatik düşmüş mü bak, kira için sabit gider kur."

### Faz 5 - Kredi kartı ve taksit (M)
- [ ] 5.1 Kart CRUD; `core/finance/cardStatements` (05, 4.2 vektörleri).
- [ ] 5.2 Taksitli alım: önizleme (kuruş farkı son taksitte), peşin fiyat -> vade farkı, satırların tek transaction'da üretilmesi.
- [ ] 5.3 "Taksitlerim" (ürün sayısı, bu ay toplamı, ilerleme çubuğu, kalan, bitiş ayı) + gelecek 12 ay listesi.
- [ ] 5.4 Kart detayı: ekstre ayı seçici, kalemler, "Ekstreyi ödedim" (card_payment), son ödeme uyarısı.
- [ ] 5.5 Düzenleme/silme kuralları (yalnızca bekleyen taksitler değişir) ve testleri.
- [ ] 5.6 KULLANICIYA SOR: taksitler için tek tek "ödendi" bayrağı ister mi? (PRD varsayım 5.)
Kabul: A4 geçer; 12.999 TL/6 -> 6 x 2.166,50; 1.000 TL/6 -> 5 x 166,66 + 166,70.
Kullanıcı kontrolü: "Buzdolabını 6 taksitle gir; Taksitlerim'de başlangıç/bitiş, bu ayki taksit toplamı ve ilerleme doğru mu?"

### Faz 6 - Birikim kartı, yedek, takvim, fiş (L)
- [ ] 6.1 Birikim kartı (maaş günü, sabit tutar, transfer).
- [ ] 6.2 Şifreli yedek dışa/içe aktarma (04, bölüm 8 belirtimi): gidiş-dönüş, yanlış parola, bozuk dosya, eski sürüm fixture, "üzerine yaz/birleştir", geri yükleme öncesi kurtarma önerisi.
- [ ] 6.3 Yedek hatırlatma kartı (ilk 3 hafta sık, sonra haftalık) ve "son yedek" göstergesi.
- [ ] 6.4 Kalıcı depolama akışı: ilk anlamlı veriden sonra iste, reddedilirse uyarı + yedek önerisi.
- [ ] 6.5 "Takvime aktar": düzenli etkinlikler için RRULE + VALARM + sabit +03:00 VTIMEZONE `.ics`; test dosyayı ayrıştırıp doğrular (geliştirme bağımlılığı olarak ayrıştırıcı için izin istenir).
- [ ] 6.6 Fiş fotoğrafı: seç/çek, küçült (<= 250 KB), işleme bağla, görüntüle, sil; yedeğe isteğe bağlı.
Kabul: A7 geçer (yedek al -> tüm veriyi sil -> geri yükle -> aynı); .ics Google Takvim'e içe aktarılabilir (kullanıcı kontrolü).
Kullanıcı kontrolü: "Yedek al, dosyayı Drive'a kaydet; Ayarlar'dan tüm veriyi sil; yedeği geri yükle."

### Faz 7 - MVP cilası ve yayın (M)
- [ ] 7.1 Erişilebilirlik: axe, yazı %130, TalkBack kısa tur, odak sırası, dokunma hedefleri.
- [ ] 7.2 Performans bütçesi (04, bölüm 10) ölçümü ve rapor.
- [ ] 7.3 Playwright: A1-A10 tam set; çevrimdışı set.
- [ ] 7.4 Gerçek cihaz kontrol listesi (07-TEST-PLAN.md) kullanıcı ile yürütülür.
- [ ] 7.5 Uygulama içi gizlilik notu ("Verin yalnızca bu telefonda"), Hakkında, sürüm, `CHANGELOG.md`, `v0.1.0` etiketi.
- [ ] 7.6 7 günlük gerçek kullanım denemesi; geri bildirim listesi -> Faz 7b düzeltmeleri.
Kabul: kontrol listesi tamam; veri kaybı senaryoları geçer; kullanıcı "yormuyor" der.

---

## v1

### Faz 8 - İstatistikler (M)
`core/stats` (gün, hafta sonu, ay: spor sayısı/dakika, oğlunla saat, ek iş günü, hedef notu günü, plansız gün, tamamlama %, seri) + para istatistikleri (gelir/gider, kategori dağılımı, birikim oranı, net değer geçmişi: `snapshots`). Sayı önce, grafik sonra, altında okunur özet.
### Faz 9 - Aylık bütçe ve grafikler (M)
Genel ve kategori limitleri, ilerleme, sakin aşım uyarısı; özel SVG grafik bileşenleri (çubuk, halka, çizgi) ve dataviz renk kuralları (kategori renk çiftleri).
### Faz 10 - Maddi hedefler (S)
Hedef tutar/tarih, bağlı hesaplar, "aylık ne kadar ayırmalı", ilerleme.
### Faz 11 - Kilit (S-M)
PIN (PBKDF2 hash + tuz + yanlış deneme gecikmesi) + WebAuthn parmak izi doğrulaması; "kapı" olduğu dürüstçe yazılır; finans ekranları için ayrı anahtar.

## v2

### Faz 12 - Bildirimler (M-L)
Karar noktası (04, bölüm 7): Capacitor Android + yerel bildirim (önerilen) veya Web Push + küçük sunucu. İlk iş kısa bir "spike": gerçek telefonda 15:00 bildirimi 7 gün güvenilir geldi mi? `core/reminders` tek kaynaktır.
### Faz 13 - Fiş okuma (L)
Önce QR (e-Arşiv/e-Fatura), sonra cihaz içi OCR; kullanıcının 20 gerçek fişiyle (yalnızca kendi cihazında) ölçüm; toplam tutar doğruluğu >= %90 değilse "öneri" olarak sunulur, otomatik kayıt yapılmaz. Sunucu/AI kullanımı her fiş için açık onay ister; `apps/server` bu fazda doğar.
### Faz 14 - Akıllı öneriler (M)
Kural tabanlı öneriler (çakışma, bütçe, birikim hedefi). Üretken yapay zekâ isteğe bağlı, kapalı başlar, veri minimizasyonu ve açık onay.

## Ultimate (sırası belirsiz)
Şifreli senkronizasyon (veri katmanı değişimi, ports sayesinde), widget, Play Store, canlı kur (isteğe bağlı), Drive'a otomatik yedek, Argon2id'ye KDF yükseltmesi, anı defteri dışa aktarma, çoklu para birimi (EUR vb.).

---

## Faz başlatma şablonu (kopyala-yapıştır)
"docs/06-ROADMAP.md'deki Faz N'yi uygula. Önce AGENTS.md ve ilgili belgeleri oku, sonra kısa bir plan çıkar (adımlar + her adımın doğrulama yolu) ve onayımı bekle. Onaydan sonra adım adım ilerle; her görevden sonra `pnpm check` çalıştır; faz sonunda faz kapısı kontrol listesini kanıtlarıyla göster."

## Değişiklik talepleri
Yeni istek gelirse: (1) hangi fazda ve hangi belgede olduğunu söyle, (2) kapsam dışıysa "Yol haritasına ekleyelim mi, hangi faza?" diye sor, (3) onay olmadan mevcut fazın işini genişletme.
