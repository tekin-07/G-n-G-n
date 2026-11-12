# 02 - Ekranlar ve Kullanıcı Akışları (Gün Gün)

Görsel kurallar 03-DESIGN.md'dedir. Metinler örnektir; nihai metinler `i18n/tr.ts` içinde tutulur, cümle düzeniyle (ilk harf büyük, ünlemsiz) yazılır. Ekranlar mobil öncelikli, 360-412 dp genişliğe göre tasarlanır.

## 1. Gezinme

Alt çubuk, 5 sekme (ikon + metin, ikonlar tek başına kullanılmaz): Bugün, Plan, Hedefler, Para, Daha fazla.
Sağ altta yüzen "Ekle" düğmesi yalnızca Bugün, Plan ve Para'da görünür; açılan alt sayfa: Gider ekle, Gelir ekle, Etkinlik ekle, Hedefe not.
Tüm düzenleme ve ekleme formları alt sayfa (bottom sheet) veya tam ekran sayfa olarak açılır; geri tuşu bir adım geri gider; yarım form için "Vazgeç?" onayı.
Rotalar (hash): `#/bugun`, `#/plan`, `#/hedefler`, `#/para`, `#/daha-fazla`, alt rotalar özellik klasöründe tanımlanır.

## 2. Ekran listesi

### 2.1 Karşılama (onboarding)
6 adım (PRD F1). Üstte adım çubuğu (adım sayısı yazı ile: "3 / 6"). Her adımda tek karar grubu. Varsayılanlar dolu, "Devam" büyük düğme, "Atla" ikincil. Son adım özet kartları ve "Bugün'e git".
Boş durum: yok. Hata: geçersiz saat/tutar satır altında açıklanır.

### 2.2 Bugün
Yukarıdan aşağıya:
1. Başlık satırı: "Cumartesi, 31 Ekim" + sağda seri göstergesi (halka içinde sayı).
2. Mini motivasyon: tek cümle, en fazla günde bir (03-DESIGN.md banka).
3. "Şimdi / Sıradaki" büyük kart: aktif veya sıradaki blok (ad, saat aralığı, kalan süre), tek ana düğme ("Yaptım" veya "Başladım"), ikincil "Ertele".
4. Bugünün zaman çizgisi: dikey, saat etiketli; geçmiş bloklar soluk, "şimdi" çizgisi belirgin; blok = kategori rengi şeridi + ad + saat; dokununca ayrıntı sayfası (Yaptım, Atla, Ertele, Not, Düzenle).
5. Kontrol kartları (zamanı gelince): ek iş sorusu (15:00 hafta içi, 12:00 hafta sonu): "Bugün için gerekenleri yaptın mı?" [Evet] [Hayır]; hedef notu kartı (21:30): "Hedefler için bugün ne yaptın?" -> alt sayfa: aktif hedefler listesi, her birinde tek satırlık not alanı, [Kaydet]; oğul notu: öğle arası/hafta sonu bloğu bitince "Bugün neler yaptınız?" (isteğe bağlı).
6. Para özeti (tek satır, dokununca Para'ya): "Toplam birikim 12.480 TL, bu ay beklenen gider 18.300 TL" (rakamlar örnek).
7. Uyarı kartları (en fazla 3, öncelik sırasıyla): kart son ödeme (3 gün kala), sabit gider vadesi (3 gün kala), birikim kartı (maaş günü), kur eski (7 gün), yedek zamanı.
8. Altta tek bağlantı: "Bugün plansız geçti" (ikincil metin düğmesi). Joker kalmadıysa yanında sakin bir not.

Boş gün: "Bugün için planlı bir şey yok. İstersen bir etkinlik ekle." Çevrimdışı: göstergesi yok (uygulama hep çevrimdışıdır).

### 2.3 Plan
Üstte görünüm anahtarı: Gün | Hafta | Ay. Hafta şeridi (Pzt-Paz, bugün vurgulu, oğul günü küçük etiket). 
- Gün: Bugün zaman çizgisinin aynısı, seçilen tarih için.
- Hafta: 7 sütun yerine dikey gün listesi (telefonda yatay sütun okunmaz): her gün altında blokların kısa satırları; çakışma işareti.
- Ay: ay ızgarası, günlerde en fazla 3 nokta (kategori renginde); güne dokununca gün görünümü.
Üstte "Bu hafta oğul günü: Cumartesi [Değiştir]". Haftalık oğlunla toplam saat bilgisi.
Çakışma: satırda "Çakışıyor" etiketi; dokununca sayfa: iki blok ve öneriler ("Spor'u Perşembe 20:00'ye kaydır"), [Uygula] [Böyle kalsın].
Filtre: kategori çipleri.

### 2.4 Etkinlik ekle/düzenle
Üstte iki sekme: Düzenli | Günübirlik.
- Düzenli: ad, kategori, günler (Pzt..Paz çipleri), saat veya "Esnek", süre, başlangıç tarihi, bitiş (isteğe bağlı), haftalık hedef, hatırlatma. Alt kısım: "Çakışma kontrolü" canlı sonuç.
- Günübirlik: ad, kategori, tarih, saat, yer, tahmini harcama (isteğe bağlı).
Kaydedince Plan'a döner, yeni blok kısaca vurgulanır.

### 2.5 Hedefler
Liste: kategoriye göre gruplu kartlar (hedef adı, kategori çipi, son not tarihi, hedef tarihi varsa kalan gün). Üstte "Bugünün notları" bağlantısı (21:30 kartının aynısı). Boş durum: "Henüz hedef yok. Mesela bir tatil, ev ya da İngilizce hedefi ekleyebilirsin." [Hedef ekle].
Hedef detayı: başlık, neden (isteğe bağlı), durum, günlük zaman çizgisi (tarih + not), [Not ekle], düzenle/duraklat/tamamla.
Hedef ekle: ad, kategori (yeni kategori ekleme dahil), tür (Yaşam hedefi | Maddi hedef [v1]), hedef tarihi.
Kategori yönetimi: Daha fazla > Kategoriler.

### 2.6 Para
Üstte segment: Özet | İşlemler | Hesaplar | Kartlar | Sabit.
- Özet: büyük rakam "Toplam birikim" (+ "kart borcunu düş" anahtarı); altında bu ay: gelir, gider, beklenen; yaklaşan ödemeler listesi (sabit gider vadeleri, kart son ödeme, taksit); birikim kartı.
- İşlemler: ay seçici, arama, kategori çipleri; liste (kategori ikonu rengi, ad/not, hesap/kart, tutar sağda; gelir pozitif renkle ama renk tek başına bilgi taşımaz: "+" ve "-" işaretleri). Satır kaydırma: sola kaydır sil (geri al çubuğu).
- Hesaplar: kartlar (ad, tür, bakiye, TL karşılığı); [Hesap ekle], [Aktarım/dönüşüm]; kur kartı ("1 USDT = 41,35 TL, 2 gün önce" [Güncelle]); hesap detayı: işlem listesi.
- Kartlar: kart listesi (limit, kesim, son ödeme, güncel ekstre, kart borcu), [Taksitlerim], [Kart ekle]. Kart detayı: ekstre ayı seçici, kalemler (tek çekim + taksit), [Ekstreyi ödedim].
- Sabit: gelir kaynakları ve sabit giderler iki bölüm; vade günü, tutar, durum (bekliyor/ödendi), [Ödedim]; [Ekle].

Gider ekle (hızlı akış, 3 adım): tutar (büyük sayı tuş takımı) -> kategori (en son kullanılan 6 çip + "Tümü") -> [Kaydet]. "Daha fazla ayrıntı" açılırsa: hesap/kart (varsayılan son kullanılan), tarih (bugün), not, fiş fotoğrafı, taksit anahtarı (kart seçiliyse).
Taksitli alım: tutar (toplam), taksit sayısı çipleri (2,3,6,9,12 + diğer), başlangıç ayı (varsayılan o ekstre), ürün adı, kategori, "peşin fiyat" isteğe bağlı; canlı önizleme: "6 taksit: 5 x 166,66 TL ve son taksit 166,70 TL. Bitiş: Nisan 2027".
Gelir ekle: tutar, kaynak (maaş/ek gelir/diğer + yeni), hesap, tarih, not.
Aktarım/dönüşüm: kaynak hesap + tutar, hedef hesap + tutar, örtük kur gösterilir, "Kuru güncelle" önerisi.

### 2.7 Taksitlerim
Üstte iki sayı: "Taksit ödenen ürün: 3" ve "Bu ay taksit toplamı: X TL". Altında ürün kartları: ad, kart, ilerleme çubuğu "3/6", kalan tutar, bitiş ayı. En altta "Önümüzdeki 12 ay": ay başına çubuk (v1 grafik; MVP'de liste) ve en yüksek ay etiketi. Ürüne dokununca detay: tüm taksitler (ay, tutar, durum: ekstreye yansıdı/bekliyor), düzenle/sil (silinirse yalnızca bekleyenler).

### 2.8 Birikim kartı
Maaş gününden itibaren 3 gün Bugün'de ve Para > Özet'te: "Bu ay biriktireceğin tutar: 5.000 TL" (ayarlardaki sabit tutar; örnek) [Biriktirdim] [Tutarı değiştir] [Sonra]. [Biriktirdim] -> hangi hesaba aktarıldığı (varsayılan son kullanılan) -> transfer oluşur.

### 2.9 Daha fazla
Liste: İstatistik (v1), Anılar (oğul notları), Kategoriler, Yedekle ve geri yükle, Takvime aktar, Ayarlar, Hakkında.

### 2.10 İstatistik (v1)
Üstte Gün | Hafta sonu | Ay. Sayılar önce, grafik ikincil; her grafiğin altında okunur özet cümle ("Bu ay 9 gün spor yaptın"). Para bölümü: gelir/gider, kategori dağılımı, birikim, net değer çizgisi.

### 2.11 Yedekle ve geri yükle
Son yedek tarihi büyük; [Yedek al] -> parola iki kez + güç göstergesi + "Fişleri de ekle" anahtarı + boyut -> paylaş menüsü. Bölüm "Geri yükle": dosya seç -> parola -> özet (tablo satır sayıları, yedek tarihi) -> [Üzerine yaz] / [Birleştir] / [Vazgeç]. Sabit uyarı: "Parolayı unutursan yedek açılamaz."

### 2.12 Ayarlar
Tema, plan kuralları (saatler, spor günleri/saati, ek iş, oğul varsayılanı), joker sayısı, birikim tutarı, uyarı gün sayıları, depolama (kalıcı depolama durumu + kullanım), sürüm, "Örnek veri yükle" (yalnızca geliştirme/demo; uydurma), "Tüm veriyi sil" (çift onay + yedek önerisi).

## 3. Kritik akışlar (kabul testlerine dönüşür)

A1. İlk açılış -> karşılama -> Bugün ekranında haftanın planı hazır.
A2. Hafta içi 15:00 -> ek iş kartı -> Evet -> kart kaybolur, gün özetinde "ek iş yapıldı".
A3. Gider ekleme: Bugün > Ekle > Gider > 125 yaz > Market > Kaydet (<= 4 dokunuş), liste ve özet güncellenir.
A4. Taksitli buzdolabı: 12.999 TL, 6 taksit, kart, Kasım başlangıç -> önizleme -> Kaydet -> Taksitlerim'de 1 ürün, bu ayki toplam doğru.
A5. Maaş günü: açılışta maaş kaydı oluşmuş, birikim kartı görünür; [Biriktirdim] -> transfer.
A6. Plansız gün: düğme -> joker kullanıldı, seri korunur; aynı hafta ikinci kez -> sakin açıklama, seri sıfırlanır.
A7. Yedek al -> uygulamayı sil/yeniden kur -> yedek yükle -> tüm veri geri gelir.
A8. Aralık'ta boks eklenir (haftada 1 gün, 1 saat) -> spor günüyle çakışırsa öneri çıkar.
A9. Çevrimdışı (uçak modu): tüm yukarıdakiler çalışır.
A10. Oğul günü Pazar'a çevrilir -> hafta planı yeniden dağılır, Cumartesi serbest.

## 4. Durumlar (her ekran için zorunlu)
Yükleniyor (iskelet, 300 ms'den kısa sürede gösterme), boş (davet + eylem), hata (ne oldu + ne yapılır), başarı (kısa onay, uzun kutlama yok), geri al (silmede 8 sn).
