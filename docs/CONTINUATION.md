# Devam Notu

Güncellendi: 12 Kasım 2026. Yeni Codex CLI oturumunda [`README.md`](../README.md), [`AGENTS.md`](../AGENTS.md) ve [`PROJECT-CONTEXT.md`](PROJECT-CONTEXT.md) sonrasında bu notu oku; sonra yalnız ilgili asıl kaynak bölümlerine git.

## Hedef ve aşama

Gün Gün; günlük planı, toparlanmayı ve kişisel parayı sade biçimde bir araya getiren tek kullanıcılı Türkçe uygulama fikridir. Şu an yalnız planlama/tasarım aşamasındayız. Uygulama kodu, proje bağımlılıkları, tamamlanmış faz, uygulama test/build sonucu veya dağıtım yok. Doküman ve bağlantı tutarlılığı incelendi; bu, uygulama testi değildir.

Kullanıcı ikinci konsept için **“Daha da yaklaştın”** dedi: ilk görsele göre daha yakın buldu. Bu son tasarıma onay değildir. İkinci yön kullanıcı incelemesine açık öneri olarak kalır. Faz 0, kodlama veya yeni ürün kapsamı otomatik başlamaz.

## Sonraki iş

İkinci konseptin görsel dilini rafine et ve gerçek haftaya dayalı normal/yoğun gün, plan değişikliği, fitness ertelemesi, görüşme tercihi ve fatura ödeme akışlarını somutlaştır. Özellikle Bugün satırlarının rahat taranması, haftanın anlaşılır görünmesi, kesin randevu ile tercihin ayrılması ve bilinmeyen fatura tutarının dürüst sunulması değerlendirilsin. Tasarım/akış önerilerini kabul edilmiş ürün kurallarıyla karıştırma. Kullanıcıya anlaşılır görsel/örnek sun; teknik uzmanlık gerektiren seçimleri ajan hazırlar. Kodlamaya ancak kullanıcı kodlama istediğinde ve uygun faz için ayrıca ilerlenir.

## Tamamlanan çalışma

Kabul edilen planlama değişiklikleri 01–09 kaynaklarına ve bağlama işlendi; [`10-DECISIONS.md`](10-DECISIONS.md) eklendi. İlk telefon konsepti hazırlandı, ardından resmî depo görselleri incelenerek araştırma notu ve ikinci konsept oluşturuldu. İkinci konsept kullanıcıya gösterildi; mevcut geri bildirim yukarıda kayıtlıdır. Bu işler planlama ve dokümantasyon kapsamındadır; uygulama geliştirme başlamadı.

## Tasarım varlıkları ve araştırma

- Güncel öneri: [`ikinci ekran taslakları`](assets/ikinci-ekran-taslaklari.png). İlk taslak yalnız karşılaştırma için: [`ilk ekran taslakları`](assets/ilk-ekran-taslaklari.png). Depoda yalnız bu iki kendi ürettiğimiz konsept varlığı bulunur.
- Üç resmî kaynak deposundan erişilen ekran görselleri ve kaynak commit'leri [`12-VISUAL-RESEARCH.md`](12-VISUAL-RESEARCH.md)'de belgelenmiştir. Bunlar resmî depo görselleridir; uygulamaların bugün kurulan sürümleri oldukları doğrulanmadı. Tasarım örnekleri indirilen dosya olarak depoya eklenmez.
- İkinci konsept görsel üretim aracıyla hazırlandı; Sleek kullanılmadı (hesap/API bağlı değildi). Kullanıcının verdiği `design-mobile-apps` ve `find-skills` yönergeleri incelendi; global skill kurulumu yapılmadı.
- Maketler raster öneridir; gerçek ekran, font/kontrast, 48 dp hedef, koyu tema veya 360/412 px doğrulaması değildir.

## Kabul edilen kararlar ve açık konular

Kabul edilen ürün kararları [`10-DECISIONS.md`](10-DECISIONS.md)'de ve kaynak belgelere işlenmiştir: mesai içi esnek ek iş, kesinleşmemiş görüşme tercihleri, gün aşırı fitness, sakin Bugün listeleri, hızlı tek satır kayıt/toparlanma, temel aylık para dağılımı ve faturalar, temel hatırlatmanın MVP gereksinimi, gerçek veri öncesi şifreli yedek. Eski sabit öğle görüşmesi, 10:00/15:00 ek iş saati, haftada dört sabit spor günü ve zorunlu seri varsayımlarını geri getirme.

Tek açık karar listesi [`01-PRD.md` bölüm 10](01-PRD.md#10-varsayımlar-ve-açık-kararlar)'dadır:

- **O1:** platform, bildirim teslimi ve kurulum/güncelleme.
- **O2:** fitness ertelemesinin gün aşırı döngüye etkisi.
- **O3:** kişisel kesin saatler ve süreler.
- **O4:** tahmini kullanılabilir para, kart ödemesi dağılımı, iade/düzeltme.
- **O5:** yedek birleştirme çatışma/silinme politikası.
- **O6:** bildirim tekrarı, sessiz saat ve kilit ekranı mahremiyeti.
- **O7:** font, araç sürümü ve ilgili doğrulama.

İkinci tasarımın yerleşimleri, renk vurguları veya etkileşimleri henüz ürün kararı değildir. Maketlerdeki gün/tutarlar uydurmadır; kişisel profil veya gerçek tutar kullanma.

## Oturum ve araç kullanımı

Root ajanı işi koordine eder; yalnız gerçekten bağımsız ve sınırlı işler için alt ajan kullanır. Rutin küçük görevlerde `gpt-6-luna`, karmaşık tasarım/mimari/incelemede `gpt-6.1-sol` tercih edilir. CLI'da model/alt ajan yoksa mevcut araçla sürdür ve kısıtı açıkça bildir; kullanmadığın aracı kullanmış gibi gösterme. Bağlam maliyetini azaltmak için aynı belgeleri gereksiz yere tekrar okuma. Soruları en fazla üç kısa Türkçe soruda grupla; kullanıcıdan kodlama/tasarım uzmanlığı bekleme.

Bu depo belgeden ibarettir; `package.json` ve npm/pnpm uygulama komutları yoktur. Uygulama testi veya build geçtiğini söyleme. Belgelerde değişiklik yaparken içerik, bağlantılar ve diff tutarlılığını kontrol et; kişisel dosyaları, `.env*` ve `*.gungun` yedeklerini dışarıda tut.
