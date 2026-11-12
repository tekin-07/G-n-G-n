# 01 - Ürün Gereksinim Dokümanı (PRD): Gün Gün

Sürüm: 1.0, 31 Ekim 2026. Durum: onaylı kapsam. Bu belge "ne yapılacak" sorusunun tek kaynağıdır. Önceliklerde çelişki olursa sıra: AGENTS.md > bu belge > 02 > 03 > 04 > 05 > 06.

## 1. Amaç

Kullanıcının hayatını planlı ve derli toplu yaşamasına yardım eden, günlük planı, hedefleri ve parayı tek yerde tutan, kullanması yormayan bir kişisel uygulama. Plansız kaçamaklar doğaldır; uygulama bunları suçlamaz, genel gidişatı hedefler doğrultusunda tutar.

İlkeler:
1. Yormasın: sık yapılan işlem 2-3 dokunuş; ekranda az yazı.
2. Suçlamasın: kaçırılan gün için yargılayıcı dil yok; "yeniden başla" tonu.
3. Veri sende: veri yalnızca telefondadır; sunucu, hesap, reklam, analitik yok.
4. Genişlesin: yeni kategori, hedef, gelir, gider, düzenli ve günübirlik etkinlik sonradan eklenebilir.
5. Sade ve ferah: turkuaz, şeftali, beyaz, açık mavi; açık ve koyu tema. "Yapay zekâ şablonu" görünümü yasak (bkz. 03-DESIGN.md).

## 2. Kullanıcı

Tek kullanıcı, Türkçe, Android telefon. Devlet dairesinde hafta içi 08:00-17:00 çalışır; iş yeri öğle arasında görüşebileceği mesafededir. Küçük yaşta bir oğlu var. Düzenli spor yapmak istiyor; Aralık'ta boks ve salsa kursu başlayacak. Ek iş yapıyor. Birikim yapmaya başlıyor (TL, USDT, gram altın). Yazılım bilgisi yok. Kişisel değerler (maaş tutarı vb.) `docs/PROFIL.local.md` dosyasındadır ve depoya girmez.

## 3. Kapsam özeti

| Sürüm | İçerik |
|---|---|
| MVP | Onboarding, Bugün ekranı, Plan (gün/hafta/ay), düzenli ve günübirlik etkinlikler, haftalık otomatik plan + çakışma uyarısı, oğul zamanı (öğle arası + hafta sonu) ve notu, ek iş kontrol kartı, plansız gün + joker, hedefler + günlük hedef notu (hedef günlüğü), kategoriler (hepsinde özelleştirilebilir), hesaplar (TL, USDT, altın) + elle kur, gelir/gider kaydı, gelir kaynakları + maaş otomasyonu, sabit giderler + vade uyarısı, kredi kartı + taksit + ekstre, aylık birikim kartı, şifreli yedek/geri yükle, kalıcı depolama, takvime aktar (.ics), fiş fotoğrafı ekleme, açık/koyu tema, PWA kurulumu, çevrimdışı |
| v1 | İstatistikler (gün, hafta sonu, ay), aylık bütçe, grafikler, maddi hedefler, PIN/parmak izi kilidi |
| v2 | Gerçek zamanlı bildirimler, fişten otomatik okuma, kural tabanlı akıllı öneriler (isteğe bağlı yapay zekâ) |
| Ultimate | Şifreli senkronizasyon, ana ekran widget'ı, Play Store, canlı kur (isteğe bağlı), Google Drive'a otomatik yedek |

Kapsam dışı: çok kullanıcı, banka bağlantısı/açık bankacılık, iOS'a özel çalışma, Türkçe dışı arayüz, sosyal özellikler, reklam.

## 4. Özellikler (MVP)

Her özellik için ölçüt, ROADMAP'te faz görevlerine bölünmüştür. Burada davranış kuralları yazılıdır.

### F1. Karşılama (onboarding)
6 kısa adım, her biri atlanabilir ve sonra Ayarlar'dan değiştirilebilir: (1) hoş geldin + veri telefonda kalır notu + ana ekrana ekleme yardımı, (2) uyanış (hafta içi/hafta sonu), uyku, iş ve öğle saatleri (varsayılanlar dolu), (3) spor günleri ve saati, ek iş süreleri, oğul günü seçimi, (4) maaş günü ve tutarı, ilk hesaplar (TL banka, nakit, USDT, altın; başlangıç bakiyesi 0), (5) sabit gider önerileri (kira, elektrik, su, doğalgaz, internet, telefon hattı, AI aboneliği: işaretle + tutar), (6) özet ve "Bugün'e git". Bittiğinde haftalık plan ve sistem rutinleri üretilir.

### F2. Bugün
Bkz. 02-SCREENS.md. Üstte tarih ve tek cümle mini motivasyon; "Şimdi / Sıradaki" kartı; bugünün dikey zaman çizgisi; zamanı gelen kontrol kartları; tek satır para özeti; uyarı kartları; "Ekle" düğmesi (gider, gelir, etkinlik, hedef notu).

### F3. Plan
Gün, hafta ve ay görünümü. Düzenli etkinlikler kurallarına göre, günübirlik etkinlikler tarihlerine göre görünür. Çakışmalar sakin bir işaretle gösterilir; dokununca öneri sunulur (uygulanması onay ister). Haftalık "oğul günü" seçimi (Cmt/Paz) Plan'dadır.

### F4. Etkinlikler: düzenli ve günübirlik
- Düzenli: ad, kategori, günler (çoklu), saat (veya esnek), süre, başlangıç tarihi (Aralık'ta başlayan boks gibi), isteğe bağlı bitiş, haftalık hedef, hatırlatma. Kullanıcı sınırsız sayıda ekleyebilir (boks, salsa, yeni alışkanlıklar).
- Günübirlik: konser, sinema, tiyatro, arkadaşlarla çıkış vb. Ad, kategori, tarih, saat, yer, tahmini harcama (isteğe bağlı: kaydedilince gidere dönüştürme önerisi).
- Tamamlama: "Yaptım / Atla / Ertele" tek dokunuş; süre ve not isteğe bağlı.

### F5. Haftalık otomatik plan
Varsayılan rutini ayarlardan üretir (05-DATA-MODEL.md, 4.6). Kurallar:
- Hafta içi: iş 08:00-17:00; her gün 12:00-13:00 oğlunla öğle arası (yemek işte, 12:00'den önce); iş saatleri içinde 1 saat esnek ek iş, 15:00'te "Bugün için gerekenleri yaptın mı?" kartı; spor 20:00'de 35-40 dk (ev), haftada 4 gün (hafta sonu sabahları dahil).
- Hafta sonu: en geç 08:00 uyanış; 2 saat ek iş; spor ve yemek; hepsi 13:00'e kadar biter; 12:00 civarı ek iş kontrol kartı. Oğul günü (varsayılan Cumartesi, haftalık Pazar'a değişebilir): 13:00-18:00 oğlunla blok (en az 4 saat). Diğer gün serbest/sosyal.
- Her şey kural olarak veridir; kullanıcı her bloğu düzenleyebilir.

### F6. Oğlunla zaman
Öğle arası ve hafta sonu bloklarında isteğe bağlı "bugün ne yaptık" notu (anı defteri). Haftalık toplam saat sayacı: hafta içi hedef en az 1 saat/gün, hafta sonu en az 4 saat. Notlar "Anılar" listesinde tarih sırasıyla okunur (v1'de dışa aktarılabilir).

### F7. Plansız gün ve joker
Bugün ekranında "Bugün plansız geçti" düğmesi. Seri kuralları 05-DATA-MODEL.md 4.5'tedir: haftada 1 joker (ayarlanabilir 0-3), joker varken seri bozulmaz; yoksa seri sıfırlanır ama dil yargılamaz.

### F8. Hedefler
Kategorili yaşam hedefleri (tatil yurt içi/yurt dışı, ev, araba, İngilizce gelişimi...). Kategori ve hedef sonradan eklenir. Her akşam (varsayılan 21:30) "Hedefler için bugün ne yaptın?" kartı: tüm aktif hedefler tek ekranda, her biri için kısa not alanı, boş bırakılabilir. Notlar hedefin günlüğünde birikir; hedef detayında zaman çizgisi olarak görünür. Durum: aktif, duraklatıldı, tamamlandı.

### F9. Kategoriler
Hedef, aktivite, gelir, gider ve sabit gider için ayrı kategori listeleri. Ad, ikon (sabit set, emoji yok) ve renk (8 renk) seçilir; sıralanır; arşivlenir. Sistem kategorileri silinemez, yeniden adlandırılabilir.

### F10. Hesaplar ve birikim
Hesap türleri: banka (TL), nakit (TL), kripto (USDT), altın (gram). Başlangıç bakiyesi 0. Elle kur girişi (USDT/TL, gram altın/TL) ve "kur X gün önce güncellendi" etiketi. Toplam birikim TL karşılığıyla görünür. Hesaplar arası aktarım/dönüşüm (TL -> USDT gibi) iki tutarla kaydedilir.

### F11. Gelir ve gider
Elle ekleme (tutar, kategori, hesap veya kart, tarih, not, isteğe bağlı fiş fotoğrafı). Hızlı ekleme: tutar -> kategori -> bitti (hesap/tarih varsayılanlı). Ek gelirler gelir kaynağı olarak eklenir (tek seferlik veya aylık). Liste, ay filtresi, arama.

### F12. Maaş
Aylık otomatik gelir (varsayılan ayın 15'i; tutar `docs/PROFIL.local.md`'dedir ve onboarding'de girilir). Zam olursa "şu aydan itibaren yeni tutar" eklenir; geçmiş değişmez. Gün gelince kayıt otomatik oluşur; uygulama o gün açılmadıysa açılışta telafi edilir.

### F13. Sabit giderler
Kira, elektrik, su, doğalgaz, internet, telefon hattı, AI aboneliği ve kullanıcının eklediği her şey: aylık veya yıllık, sabit veya değişken tutar, vade günü, ödeme kaynağı. Vadeye `fixedExpenseWarnDays` (3) gün kala uyarı kartı. "Ödedim" -> işlem oluşur (değişkende gerçek tutar sorulur). İsteğe bağlı otomatik kayıt. Her sabit gider kendi kategorisinde gruplanır.

### F14. Kredi kartı ve taksit
- Kart: ad, limit, hesap kesim günü, son ödeme günü. Nakit/kart etiketi gider eklerken görünür.
- Taksitli alım: ürün adı, kart, toplam tutar (faiz dahil), taksit sayısı, başlangıç ayı, kategori; isteğe bağlı peşin fiyat (vade farkı gösterilir). Başlangıç ve bitiş ayı, aylık taksit, kuruş farkı son taksitte.
- "Taksitlerim": kaç üründen taksit ödeniyor, bu ayki toplam taksit, ürün başına ilerleme çubuğu (3/6), kalan tutar, bitiş ayı, önümüzdeki 12 ay taksit yükü.
- Kart ekstresi: o ay tek çekim + taksitler, kesim ve son ödeme tarihi, son ödemeye yaklaşınca uyarı. "Ekstreyi ödedim" -> `card_payment`.

### F15. Aylık birikim kartı
Maaş gününde: "Bu ay ne kadar biriktirdin/biriktireceksin?" Tutar ayarlarda sabittir; kart onayla/değiştir/sonra seçenekleriyle çıkar. Onayda hangi hesaba aktarıldığı sorulur (transfer oluşur).

### F16. Yedekleme ve geri yükleme
Tek dokunuşla şifreli `.gungun` dosyası (parola zorunlu, güç göstergesi). Paylaş menüsüyle Drive/WhatsApp/dosyalar'a kaydedilir. "Fişleri de ekle" anahtarı. Haftalık hatırlatma kartı (ilk 3 haftada daha sık). Geri yükleme: dosya seç -> parola -> özet gösterilir -> "üzerine yaz / birleştir" onayı. Parola unutulursa yedek açılamaz uyarısı her yedekte görünür.

### F17. Hatırlatmalar (MVP sınırı)
Bildirim yerine kartlar ve "Takvime aktar" (.ics). Güvenilir saatli bildirim v2'dedir; bu sınır Ayarlar'da açıkça yazılır.

### F18. Tema ve erişilebilirlik
Açık, koyu, sistem. Dokunma hedefi en az 48 dp. Metin büyütmeye uyar. Hareket azaltma tercihine uyar. WCAG AA kontrast (03-DESIGN.md'deki token'lar doğrulanmıştır).

### F19. Fiş fotoğrafı ekleme
Gider eklerken kamera veya galeriden fotoğraf; küçültülüp telefonda saklanır; işlemde görünür; yedeğe isteğe bağlı girer. Otomatik okuma v2.

## 5. Özellikler (v1)

- F20 İstatistik: Gün (tamamlama, seri), hafta sonu özeti, ay (spor sayısı ve dakika, oğlunla saat, ek iş yapılan gün, hedef notu girilen gün, plansız gün, tamamlama yüzdesi). Para: aylık gelir/gider, kategori dağılımı, birikim oranı, net değer geçmişi.
- F21 Aylık bütçe: genel ve kategori limitleri, harcama ilerlemesi, limit aşımına sakin uyarı.
- F22 Grafikler: özel SVG çubuk, halka, çizgi.
- F23 Maddi hedefler: tutar, tarih, bağlı hesaplar, "aylık ne kadar ayırmalı" hesabı.
- F24 Kilit: PIN + (varsa) parmak izi, finans ekranları için ayrı açılabilir. Sınırlar dürüstçe yazılır.

## 6. Özellikler (v2 ve sonrası) - yalnızca yön
- F25 Bildirimler: Capacitor/Android yerel bildirim (önerilen) veya Web Push (04-ARCHITECTURE.md bölüm 7).
- F26 Fiş okuma: önce e-Arşiv/e-Fatura QR kodunu okuma (QR içinde tarih ve toplam tutar vardır), olmazsa OCR; cihaz içi OCR tercih edilir; sunucu/AI kullanımı her fiş için ayrı açık onaya bağlıdır. 20 gerçek fişle ölçüm yapılır, toplam tutar doğruluğu >= %90 olmadan "otomatik" denmez.
- F27 Akıllı öneriler: önce kural tabanlı ("Cuma spor çakışıyor, Perşembe'ye kaydıralım mı?"). Üretken yapay zekâ ancak isteğe bağlı, veri minimizasyonuyla ve açık onayla.

## 7. Fonksiyonel olmayan gereksinimler

- Gizlilik: MVP/v1'de ağ isteği yoktur (uygulamanın kendi dosyaları hariç). Analitik/reklam/üçüncü taraf betik yok.
- Güvenlik: bkz. 04-ARCHITECTURE.md bölüm 8.
- Çevrimdışı: tüm MVP işlevleri internetsiz çalışır.
- Performans: bkz. 04-ARCHITECTURE.md bölüm 10.
- Yerelleştirme: Türkçe, `tr-TR` sayı/tarih biçimi, TL, hafta Pazartesi'den başlar, saat dilimi `Europe/Istanbul`.
- Uyumluluk: Android'de Chrome'un son 2 majör sürümü; kurulu PWA olarak.
- Dayanıklılık: yarım kayıt yok (transaction), geri alınabilir silme, migrasyon testleri.
- Bakım: bağımlılık güncellemeleri haftalık gruplu PR; sürüm kaydı.

## 8. Metin ve ton kuralları (kısa)
Cümle düzeni, sade fiiller, ünlem yok, "Harika!" tarzı süs yok, hata mesajı neyin ters gittiğini ve nasıl düzeltileceğini söyler, boş ekran bir davettir. Ayrıntı ve örnekler 03-DESIGN.md'dedir.

## 9. Başarı ölçütleri (kişisel kullanım)
- İlk gün: karşılamayı 3 dakikada bitirip Bugün ekranını görmek.
- Günlük gider ekleme: ortalama <= 10 saniye.
- 4 haftada veri kaybı: 0 (yedek alışkanlığı oturdu).
- Kullanıcı 4. haftada hâlâ açıyor ve "yoruyor" demiyor (öznel kontrol, faz sonlarında sorulur).

## 10. Varsayımlar ve açık noktalar (kullanıcı onayına açık)

| # | Varsayım | Kim karar verdi | Değiştirmek için |
|---|---|---|---|
| 1 | Hafta içi uyanış 06:30, uyku 23:00 | Tur 1 varsayımı, kullanıcı "tamam" dedi | Ayarlar |
| 2 | Spor günleri varsayılan Pzt, Çar (20:00) + Cmt, Paz sabah (11:00) | Claude; kullanıcı 4 gün ve 20:00 dedi, günleri belirtmedi | Onboarding/Ayarlar |
| 3 | Hafta sonu ek iş kontrol kartı 12:00 | Claude | Ayarlar |
| 4 | Oğul günü bloğu 13:00-18:00 (5 saat; en az 4) | Claude | Plan |
| 5 | Taksitler tek tek "ödendi" işaretlenmez; ekstre ödemesi kaydedilir, ilerleme çubuğu ekstreye yansıyan taksitleri sayar | Claude (çift sayımı önlemek için). Önceki konuşmada "ödendi işaretlenebilir" denmişti | Faz 5'te kullanıcıya sorulur; istenirse taksit satırına "ödendi" bayrağı eklenir |
| 6 | Seri eşiği %60, joker haftada 1 | Claude | Ayarlar |
| 7 | Yazı tipi çifti Nunito (başlık) + Figtree (gövde); Türkçe harf testinden geçmezse yedek aday | Claude | Faz 0 sonunda ekran görüntüsüyle onay |
| 8 | Depo herkese açık (Free GitHub planı) | Claude | Özel depo istenirse Cloudflare Pages vb. araştırılır |
| 9 | Yedek parolası en az 10 karakter | Claude | - |
| 10 | Maaş gün/ay yoksa ayın son gününe sıkıştırılır; hafta sonuna denk gelirse yine o tarihte yazılır | Claude | Düzenlenebilir |
