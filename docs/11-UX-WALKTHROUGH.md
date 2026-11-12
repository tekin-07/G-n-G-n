# 11 — Günlük Kullanım ve İlk Ekran Taslakları

12 Kasım 2026. **İnceleme taslağıdır, yeni ürün kararı veya uygulama kodu değildir.** Kullanıcı ekran maketleriyle ilerlemeyi seçti. Kaynak: 01-PRD, 02-SCREENS ve 03-DESIGN. Aşağıdaki örnek tutar/görüşmeler uydurmadır, kişisel takvim değildir. Taslak kabul edilirse ilgili asıl belgelere işlenir.

## İlk görsel turunun amacı
Üç ekranın bilgi sırasını değerlendirmek: Bugün, Haftayı planla, Para. Telefon maketleri çalışır ekran veya doğrulanmış erişilebilirlik/ölçü kanıtı değildir. İlk tur açık tema; koyu tema ve 360/412 px incelemeleri ayrıntılı UI aşamasındadır. Ek özellik veya yeni kategori sistemi önerilmiyor.

Seçilen mevcut yön: Zamanı belli / Uygun olduğunda / Dikkat gerekiyor listeleri; bütün günü saatli çizelgeye dönüştürme yok. Alternatif bir ilk görünüm olarak tam hafta takvimi düşünülebilir, fakat günlük seçim yükünü azaltma ihtiyacı için Bugün başlangıcını öneriyoruz. Haftalık plan ayrı alanda kalır. Görsel alışkanlık ve finans işlevlerini aynı renkle/ikonla belirsizleştirmemeli.

## 1. Normal iş günü
- Açılış: Bugün. Üstte tarih, hızlı Ekle; kesinleşmiş saatli olaylar kısa listede, mesai içi ek iş Uygun olduğunda bölümünde.
- Kullanıcı 10:00 veya 15:00'te boş kaldığında işi açar/tamamlar. Başlat sayacı veya süre girmek zorunlu değil; pencere içinde gecikme yok.
- Tek Tamamla eylemi kaydı Tamamlananlar'a taşır, bağlı hedefin ilerlemesi ve eski hatırlatma birlikte güncellenir. Geri al erişilir.
- Akşam seçilen gündeki fitness/okuma ve kesinleşmiş görüşme görünür. Süre/saat bilinmiyorsa uydurulmaz.
- Maketteki başlıklar ve yerleşim prototip önerisidir; ek iş içeriği makete taşınmaz.

## 2. Yoğun veya planı değişen gün
- İlgili iş > Bugün yetişmeyecek > Başka güne taşı / Bu haftadan çıkar / Duraklat.
- Taşıma önizlemesinde hedef gün ve etkilenebilecek işler gösterilir; bütün işlerin ertesi güne eklenmesi yok.
- Kaydetmeden vazgeçince mevcut plan kalır. Kaydedince eski hatırlatma iptal/yeniden planlanır; geçmiş tamamlamalar korunur.
- Finansal vadesi olan bir faturada bu menü borcun tarihini değiştirmez; yalnız hatırlatma ertelenebilir.

## 3. Haftalık plan ve görüşme değişikliği
- Plan > Haftayı planla: kesin işler, görüşme tercihleri, esnek hedef işleri ve boşluk gözden geçirilir.
- Görüşme tercihi “Kesinleşmedi” etiketlidir. Anlaşılan tarih/saat girildiğinde kullanıcı “Kesinleştir” ile onaylar; bu bir mesajlaşma veya karşı taraftan onay toplama sistemi değildir.
- Örnek makette tek bir gün bölümü genişletilmiş, diğer günler kısa özetlenmiştir. Tüm haftayı tek ekrana sıkıştırmak amaç değildir.
- Değişiklik özeti > Onayla. Uyumsuz zamanlar için öneri, otomatik taşıma yok. Süresi bilinmeyen işlerle kesin “bu haftaya sığıyor” sonucu verilmez.
- Oğulla öğle görüşmesi yok; belirlenmemiş görüşme saatleri makette gerçekmiş gibi kullanılmaz.

## 4. Ertelenen fitness
- Gün aşırı döngü hafta sınırında sürer. “Bu seferi taşı” ile “Sonraki günleri de değiştir” etkisi ayrı gösterilir.
- **O2 açık:** hangi davranışın varsayılan olacağı henüz kararlaştırılmadı. Maket bu kararı kendiliğinden kapatmaz; hareket/dinlenme günlerinin önizlemesi sonraki turda karşılaştırılır.
- OpenGym program içeriği kopyalanmaz veya entegrasyon yapılmış gösterilmez. Gün Gün planlama ve tek dokunuş tamamlamayı sağlar.

## 5. Yeni fatura ve para kontrolü
- Para > Özet: bu ay gelir/gider/birikim durumu; sade kategori dağılımı; yaklaşan ödemeler ve eksik bilgi.
- “Tutar bekleniyor” ayrı görünür; uydurma 0 TL veya kesin kullanılabilir para sunulmaz. Makette kullanılabilir tutar O4 çözülmeden hesaplanmış rakam olarak gösterilmez.
- Fatura geldiğinde dönem/tutar/vade kesinleşir. Bankadaki otomatik ödeme talimatı ayrıca görünür, uygulama bunu gerçek ödeme saymaz.
- Ödedim > tutar/hesap/tarih doğrulaması > kaydet: tek harcama, kalan borç ve uyarı tutarlı güncellenir. Eksik alan varsa tek dokunuşta ödeme varmış sayılmaz.
- Finans maketindeki rakamlar yalnız uydurma tasarım verisidir. Kart iadesi/kısmi ekstre dağıtımı bu makette çözülmüş değildir.

## İlk maketler için içerik ve stil
- Sakin, ferah, günlük kullanım odaklı. Serin beyaz zemin, turkuaz ana eylem, az şeftali uyarı; Nunito başlık/Figtree gövde hissi. Gerçek font/ölçü/kontrast doğrulaması uygulama aşamasındadır.
- Renk token'ları, köşe çeşitleri, 48 dp hedefler ve sade ikonlar 03-DESIGN'den. Emoji, gradient, cam, eşit kart ızgarası, reklam cümlesi veya baskın seri yok.
- Üç ayrı telefon ekranı; her ekranda tek baskın eylem ve okunabilir metin. Alt gezinmede Bugün / Plan / Hedefler / Para / Daha fazla.
- Bugün: örnek “Dil dersi 19:00”, “Ek iş — Mesai içinde”, “Kitap oku — Akşam”, “Elektrik — Tutar bekleniyor”. Bunlar bir gerçek günün atamaları değildir.
- Haftayı planla: tercihler/kesinleşme ve esnek iş dağılımı; “Hafta taslağı” ve “Planı onayla”. Onaylanmamış görüşme gerçek randevu gibi boyanmaz.
- Para: gelir/gider/birikim ayrımı; 24.000 TL örnek gelir, 9.200 TL örnek gider, 2.000 TL örnek ayrılan birikim; gider dağılımı örnek 4.000 Market +3.000 Ev +1.200 Ulaşım +1.000 Diğer =9.200. Bu rakamlardan ayrıca “kalan harcanabilir” hesaplanmaz.

## Skill incelemesi ve üretim durumu
Kullanıcının paylaştığı [design-mobile-apps](https://www.skills.sh/designed-by-ai/skills/design-mobile-apps) ve [find-skills](https://www.skills.sh/vercel-labs/skills/find-skills) kaynak yönergeleri GitHub'dan okundu; skills.sh sayfaları bu ortamdan HTTP 403 döndürdü.
- [Mobil tasarım SKILL.md](https://github.com/designed-by-ai/skills/blob/main/skills/design-mobile-apps/SKILL.md) ve [designing.md](https://github.com/designed-by-ai/skills/blob/main/skills/design-mobile-apps/designing.md): Sleek API projesi, tasarım yönü, ekran üretimi ve ekran görüntüsü inceleme akışı. `SLEEK_API_KEY` veya hesap bağlama gerektirir. Ortamda bu bağlantı yok ve Sleek mevcut izinli ağ hedefleri arasında değil. API projesi oluşturulmadı; ücret/kredi harcanmadı. Anahtar sohbette istenmez; gerektiğinde güvenli bağlama/ortam ayarı kullanılır.
- [find-skills SKILL.md](https://github.com/vercel-labs/skills/blob/main/skills/find-skills/SKILL.md): ihtiyaca göre arama ve kaynak/kalite incelemesi. Şimdilik ek bir arama/kurulum gerekli görülmedi; skill CLI veya global paket kurulmadı.
- İlk görsel tur için burada görsel üretim aracıyla konsept maket hazırlanır; Sleek çıktısı diye sunulmaz. Dış tasarım brief'inde kişisel PROFIL.local veya repo içindeki özel kullanıcı bilgileri paylaşılmaz; yalnız anonim ürün kuralları ve uydurma örnekler kullanılır.

## İlk görsel çıktı ve öz değerlendirme
[İlk üç ekran maketi](assets/ilk-ekran-taslaklari.png) görsel üretim aracıyla oluşturuldu ve kullanıcıya gösterildi. Sleek çıktısı değildir; uydurma verili raster konsepttir. Bugün/Haftayı planla/Para yan yana, açık tema. İlk görsel yalnız karşılaştırma için korunur; güncel tasarım önerisi [ikinci konsepttir](assets/ikinci-ekran-taslaklari.png) ve kullanıcı onayı değildir.

İçerik kontrolü: esnek iş saatli görünmüyor; görüşmede kesinleşmeme etiketi var; bilinmeyen faturaya sıfır/kalan bakiye uydurulmadı; gider dağılımının metinsel tutarları 9.200 TL'ye eşit. Bu kontrol uygulama hesabı veya test sonucu değildir.

Görsel öz eleştiri ve sonraki tur önerileri (henüz onaylı karar değil):
- Gün Gün markası ile büyük ekran başlığının birlikte kullanımı dikey alanı fazla tüketiyor; tekrar eden marka küçültülebilir.
- Bugün listesi kalın başlıklar ve çok sayıda çerçeveyle beklenenden yoğun; çerçeveleri azaltıp ağırlık hiyerarşisini sakinleştirmek değerlendirilmeli.
- Üretilen görselin düğme/yüzeylerinde hafif ton geçişi/doku var. 03'ün düz renk kuralıyla birebir eşleşmiyor; son tasarım düz token renkleri kullanmalı. Görselden gerçek font, kontrast, piksel ölçüsü veya dokunma hedefi doğrulanamaz.
- Para çubukları yalnız görsel yön gösterir; oranlar kodlama sırasında veriden hesaplanır. Kesin font/360 ve 412 px/koyu tema kontrolleri henüz yapılmadı.
- Haftayı onaylamak, kesinleşmemiş görüşmeyi otomatik kesinleştirmemeli. Şu maketteki buton yalnız plan taslağını kaydetme/inceleme adayı; bu ayrım sonraki etkileşim taslağında açık anlatılmalı.

## Sonraki değerlendirme
Kullanıcı ikinci turu ilkine göre daha yakın buldu, son tasarım olarak onaylamadı. İkinci konseptte Bugün satırlarının bilgi sırası/tek eylemi, haftanın okunması, görüşme tercihi ile kesin randevu ayrımı ve Para'daki bilinmeyen tutar sunumu rafine edilecek; ardından normal ve aksayan gerçek hafta akışları gösterilecek. Onaylanmış kararlarla görsel önerileri ayrı tut. Görsel tura devam etmek kodlama/platform/yayın onayı değildir. Uygulama ve gerçek cihaz testleri yapılmadı.
