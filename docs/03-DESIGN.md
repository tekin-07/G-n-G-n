# 03 - Tasarım Sistemi ve "Şablon Görünümüne Karşı" Kurallar (Gün Gün)

Bu belge görsel ve metin kararlarının tek kaynağıdır. Ajan yeni bir görsel karar vermeden önce buraya bakar; burada olmayan bir şeyi "varsayılan" diye eklemez, sorar.

## 1. Neden bu belge var (araştırma özeti)

Yönlendirilmeyen yapay zekâ, gördüğü tüm arayüzlerin ortalamasını üretir. Bu "ortalama"nın bilinen imzası: mor-indigo gradient, Inter/Roboto yazı tipi, ortalanmış büyük başlık, ikonlu üç yuvarlak kart, ikon yerine emoji, buzlu cam (glassmorphism), her şeye aynı köşe yarıçapı ve aynı yumuşak gölge, abartılı reklam dili ("Effortlessly unleash..."). Düzeltme "mor yerine turkuaz gradient" değildir; o da yeni şablondur. Düzeltme, her seçimin yazılı bir nedeni olmasıdır. Bu yüzden bu belge hem yön hem yasak listesidir.

Referans uygulamalardan alınan dersler:
- Tiimo: günü renkli dikey zaman çizgisi olarak göstermek, suçlamayan sakin ton. Alınan: zaman çizgisi, nazik geri bildirim.
- Structured: tek dikey çizgide gün. Alınan: sadelik.
- Finch: plan dağılsa bile yargılamayan günlük temas. Alınan: "plansız gün" tonu.
- Cashew (bütçe): Material You sadeliği, sistem renklerine uyum, açık/koyu tema, hızlı işlem girişi. Alınan: hızlı gider girişi, tok ve temiz liste.

## 2. Tasarım yönü (tek paragraf)

"Sabah ışığı, sakin kırtasiye": serin beyaz bir zemin üstünde turkuaz ana renk, şeftali yalnızca küçük sıcak vurgu; bol boşluk, ince çizgiler, az gölge. Hiçbir ekran bir "panel" gibi görünmez; bir planlayıcı defteri gibi okunur. Hatırlanır tek unsur: günün zaman çizgisi ve gün kapanınca dolan ince halka. Geri kalan her şey sessizdir.

Neden bu seçimler: kullanıcı ferah, ciddi olmayan, yormayan bir görünüm istedi (turkuaz, şeftali, beyaz, açık mavi). Zemin bilinçli olarak krem değil serin beyazdır; krem + kil/terrakota ikilisi de bugünün "otomatik tasarım" imzalarından biridir.

## 3. Yasaklar (kod incelemesinde reddedilir)

1. Emoji, hiçbir yerde (arayüz, tohum veri, testlerin görünür metni). İkon = `@phosphor-icons/react`, tek set, tek kalınlık (regular), seçili durumda fill.
2. Gradient (arka plan, düğme, metin, halka), buzlu cam, bulanık arka plan, neon, parlama.
3. Mor/indigo/menekşe tonları; Inter, Roboto, Arial, system-ui'yi başlık yazısı olarak kullanmak.
4. Aynı boyutta eşit kartlardan oluşan ızgara ("üç kutu") ve ortalanmış hero blokları. Her ekranda tek baskın öğe, gerisi sakin liste.
5. Tüm öğelerde tek köşe yarıçapı ve tek gölge. Yarıçap öğe boyutuna göre değişir (bölüm 6); gölge en çok iki seviye.
6. Her başlığın üstünde büyük harf (ALL CAPS) etiket, orta nokta ile birleştirilmiş meta dizeleri ("A · B · C"), düğmelere "→", ardışık sayılı süs ("01/02/03", içerik gerçekten sıra değilse), tek kelimesi vurgulu başlıklar.
7. Her bölümde kayan/solarak gelen giriş animasyonları, her kartta hover geçişi. Hareket yalnızca kullanıcının eylemine yanıt verir.
8. Aşırı metin: bir ekranda başlık + 1 yardımcı cümle; ikon tek başına anlam taşımaz (yanında yazı).
9. Rastgele görsel/illüstrasyon, stok fotoğraf, "yapay zekâ blob" figürler. İllüstrasyon gerekirse tek çizgi stilinde elle çizilir (SVG) veya hiç kullanılmaz.
10. Hype kopyası: "Süper güç", "Hayatını dönüştür", ünlem yağmuru, uydurma yorum/istatistik.
11. Kayıt dışı renk: tüm renkler token'dan gelir; bileşende sabit hex yok.

## 4. Renk token'ları (WCAG AA doğrulandı, 31 Ekim 2026)

Tüm metin/zemin çiftleri >= 4,5:1, arayüz bileşeni sınırları >= 3:1 olacak şekilde hesaplanmıştır (aşağıdaki oranlar betikle ölçüldü). CSS değişkeni adı = `--color-<token>`.

### Açık tema
| Token | Hex | Kullanım |
|---|---|---|
| bg | #F5FAFA | Sayfa zemini (serin beyaz) |
| surface | #FFFFFF | Kart/sayfa yüzeyi |
| sunken | #E8F2F2 | Girdi alanı, çökük yüzey |
| line | #C9DDDE | İnce çizgi (yalnızca ayırıcı; bileşen sınırı için `primary` veya `muted`) |
| text | #17282B | Ana metin (bg üzerinde 14,5:1) |
| muted | #4A6165 | İkincil metin (bg üzerinde 6,25:1; sunken üzerinde 5,77:1) |
| primary | #0B7C83 | Ana düğme dolgusu, aktif sekme (beyaz yazı 4,97:1; yüzey üstünde 4,97:1) |
| on-primary | #FFFFFF | primary üstündeki yazı |
| primary-ink | #065A60 | Açık turkuaz zemin üstünde yazı/ikon (7,96:1 beyazda) |
| primary-soft | #D5F0EF | Seçili çip, vurgulu satır (primary-ink ile 6,65:1) |
| peach | #FFB59A | Şeftali dolgu (yalnızca dolgu; metin/ikon olarak beyaz üstünde KULLANILMAZ, 1,7:1) |
| on-peach | #3B1C0E | peach üstündeki yazı (9,08:1) |
| peach-soft | #FFE7DD | Sıcak uyarı/vurgu zemini |
| peach-ink | #9A3D14 | peach-soft/beyaz üstünde yazı (5,81:1 / 6,88:1) |
| sky-soft | #DCEBFA | Bilgi zemini, grafik |
| sky-ink | #1E5C99 | sky-soft üstünde yazı (5,68:1) |
| success-soft / ink | #DDF3E6 / #1B6E45 | Başarı (5,36:1) |
| danger-soft / ink | #FBE4E1 / #A7302A | Hata/silme (5,58:1) |

### Koyu tema
| Token | Hex | Not |
|---|---|---|
| bg | #0E181A | |
| surface | #152326 | |
| raised | #1C2F33 | Açılır sayfalar |
| line | #2D464B | |
| text | #E6F2F2 | surface üstünde 14,11:1 |
| muted | #9FB7BA | raised üstünde 6,62:1 |
| primary | #5CCFC9 | on-primary #06292B ile 8,25:1 |
| primary-ink | #8EE3DE | primary-soft (#133A3C) üstünde 8,36:1 |
| peach / on-peach | #FFB59A / #3B1C0E | 9,08:1 |
| peach-ink / soft | #FFC4AE / #3A2A22 | 8,97:1 |
| sky-ink / soft | #9BCBF5 / #14303F | 8,04:1 |
| success-ink / soft | #86DDAB / #133524 | 8,28:1 |
| danger-ink / soft | #FF9A8E / #3E1F1C | 7,24:1 |

### Kategori renkleri (8; ad = ColorKey)
Her biri `soft` (zemin) + `ink` (yazı/ikon) çiftidir; çift yalnızca birlikte kullanılır.
| ColorKey | Açık soft / ink | Koyu soft / ink |
|---|---|---|
| turkuaz | #D5F0EF / #065A60 | #133A3C / #8EE3DE |
| seftali | #FFE7DD / #9A3D14 | #3A2A22 / #FFC4AE |
| gok | #DCEBFA / #1E5C99 | #14303F / #9BCBF5 |
| nane | #DDF3E6 / #1B6E45 | #133524 / #86DDAB |
| kum | #F3EBD9 / #6B5316 | #3A3220 / #E6D08F |
| mercan | #FBE4E1 / #A7302A | #3E1F1C / #FF9A8E |
| lacivert | #E3E8F4 / #34456E | #1E2740 / #A9BBEA |
| limon | #EEF3C9 / #55610F | #2F3410 / #D2E27A |
Hepsi >= 5,3:1 (açık) ve >= 7,2:1 (koyu). Bilgi yalnızca renkle verilmez: yanında ikon veya metin vardır (renk körlüğü).
Yeni renk eklemek: önce kontrast betiğinden geçer (`apps/web/src/app/theme/contrast.test.ts` Faz 0'da yazılır ve bu tabloları doğrular).

## 5. Tipografi

- Başlıklar: Nunito (700/800), sıcak ve yuvarlak hatlı. Gövde: Figtree (400/500/600). İki aile açıkça farklıdır; üçüncü yazı tipi yok. Hepsi self-host (Fontsource ve `latin` + `latin-ext` alt kümeleri), CDN yok.
- Faz 0'da Türkçe harf testi zorunlu: `ğ Ğ ş Ş ı İ ö Ö ü Ü ç Ç` her ağırlıkta doğru görünür; para için tabular rakam (`font-variant-numeric: tabular-nums`) çalışıyor mu denenir. Başarısız olursa sırayla Nunito Sans, DM Sans, Atkinson Hyperlegible denenir; seçim ekran görüntüsüyle kullanıcıya gösterilir.
- Ölçek (px, satır yüksekliği): display 32/38 (yalnızca büyük rakam), title 24/30, heading 19/26, body 16/24, small 14/20, caption 12/16 (alt sınır 12; altı yasak). Satır uzunluğu 80 karakterin altında.
- Büyük tutarlar `display`, listede tutarlar `body` yarı kalın, sağa hizalı.
- Cümle düzeni (ilk harf büyük). Türkçe büyük/küçük harf için `toLocaleUpperCase('tr-TR')` kullanılır (i/İ, ı/I). Sabit ALL CAPS yok.

## 6. Biçim: boşluk, köşe, yükseklik, çizgi

- Izgara 4 pt; yatay kenar boşluğu 16; bölümler arası 24; kart içi 16.
- Köşe yarıçapı öğe boyutuna göre: çip/küçük düğme 10, girdi 12, kart/liste grubu 16, alt sayfa üst köşe 24, büyük "Şimdi" kartı 20, halka/avatar tam yuvarlak. Tek yarıçapı her yere uygulamak yasaktır.
- Yükseklik: seviye 0 düz (çizgili), seviye 1 yalnızca alt sayfa ve yüzen düğme için tek ince gölge. Kartların çoğu gölgesizdir; ayrım `line` ve zemin farkıyla yapılır.
- Dokunma hedefi >= 48x48 dp; komşu hedefler arası >= 8 dp. Odak halkası görünür (2 px, primary, 2 px boşluklu).

## 7. Bileşen envanteri (`components/ui`)

Button (birincil dolgu, ikincil çizgi, metin), IconButton (her zaman `aria-label`), Chip (seçilebilir), Card (düz), Field (etiket üstte, hata altta), MoneyInput (büyük sayı tuş takımı), MoneyText (tr-TR, işaretli), TimeField, DayChips, Sheet (alt sayfa), Toast/UndoBar, Tabs (alt çubuk), ProgressBar (taksit), Ring (gün halkası), EmptyState, SectionHeader, ListRow, Switch, Segmented.
Her bileşen: 4 durum (normal, odak, basılı, devre dışı), açık/koyu tema, Testing Library erişilebilirlik sorgusuyla test.

## 8. Hareket

- Yalnızca eyleme yanıt: onay işareti dolması 150 ms, alt sayfa 220 ms ease-out, silme çubuğu 120 ms.
- Tek "anılacak an": gün tamamlanınca Bugün'deki ince halka dolar ve kısaca sessizce durur (ses/konfeti yok).
- `prefers-reduced-motion: reduce` ise tüm hareket anlık geçişe döner.

## 9. Ekran düzeni taslakları

Bugün:
```
[Cumartesi, 31 Ekim            (seri: 4)]
Bugün spor günü, 35 dakika yeter.

+----------------------------------------+
| Şimdi                         10:30-12:30 |
| Ek iş                                    |
| Kalan 1 sa 20 dk                         |
| [ Yaptım ]            [ Ertele ]         |
+----------------------------------------+

 08:00  Uyanış
 09:30  | Ek iş (2 sa)        <- şimdi çizgisi
 11:00  | Spor (35 dk)
 13:00  | Oğlunla zaman (5 sa)
 ...
Toplam birikim 12.480 TL  >
[ Kart son ödeme: 3 gün ]      (uyarı kartı)
                 Bugün plansız geçti
[Bugün][Plan][Hedefler][Para][Daha fazla]            ( + Ekle )
```
Para > Özet:
```
Toplam birikim
12.480,00 TL           [x] Kart borcunu düş
Bu ay   Gelir 50.000   Gider 31.200   Beklenen 18.300   (uydurma örnek rakamlar)
Yaklaşan ödemeler: Kira - 1 Kas - 15.000 TL ...
```

## 10. Metin ve ton

Genel: kısa, sade fiil, ilk harf büyük, ünlem yok, "Harika/Tebrikler" yok. Düğme ne olacağını söyler ("Kaydet", "Yedek al", "Ödedim"); aynı eylemin adı akış boyunca aynı kalır ("Kaydet" -> "Kaydedildi").
Hata: neyin ters gittiği + ne yapılacağı, özür yok. Örnek: "Tutar sıfırdan büyük olmalı."
Boş ekran davettir: "Henüz hedef yok. Mesela bir tatil ya da İngilizce hedefi ekleyebilirsin."
Plansız/kaçırılan gün: "Plansız gün de olur. Yarın yeniden başlarız." Asla "başaramadın", "seriyi bozdun".
Mini motivasyon kuralları: günde en fazla bir cümle, duruma bağlı (spor günü, hafta sonu, maaş günü, seri...), tekrarlamayan döngü, ton sakin ve samimi, abartı ve ünlem yok.
Örnek banka (30 satırdan fazlası yazılabilir; `i18n/tr.ts` içinde kategori etiketli):
- Genel: "Bugün küçük bir adım yeter." / "Planın hazır, sen sadece başla." / "Bugünün en önemli işi belli."
- Spor günü: "Bugün spor günü, 35 dakika yeter." / "Isınmak bile bir başlangıç."
- Hafta sonu: "Hafta sonu sabahı, acele yok." / "Öğleden sonra oğlunla zaman var."
- Maaş günü: "Maaş günü. Birikim için güzel bir gün."
- Seri: "Dört gündür düzenlisin." (yalnızca 3, 7, 14, 30 gün gibi eşiklerde)
- Plansız gün sonrası: "Dün plansızdı, bugün yeni bir gün."
- Akşam: "Gün bitiyor, hedeflerine bir satır yazabilirsin."
Yasak: karşılaştırma ("%80 başarılısın"), suçlama, korkutma ("hedefinin gerisindesin" gibi ifadeler MVP'de yok).

## 11. Kalite kapısı (her fazın görsel bitişi)

1. Mobil 360 px ve 412 px, açık ve koyu tema ekran görüntüsü alınır, ajan "bu ekran bölüm 3 yasaklarından hangisini çiğniyor?" diye öz-eleştiri yapar ve bulduğunu düzeltir.
2. Ekran görüntüleri kullanıcıya gösterilir; kullanıcı onayı olmadan faz kapanmaz.
3. `axe` taraması: ciddi ve kritik ihlal 0.
4. Yazı büyütme %130'da taşma yok. Hareket azaltma testi geçer.
5. Tüm metin `i18n/tr.ts` içinde ve emoji taraması (regex) temiz.
