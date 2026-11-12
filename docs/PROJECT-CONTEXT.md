# Gün Gün — Kısa Proje Bağlamı

Güncellendi: 12 Kasım 2026. Bu dosya yön bulma içindir; kararların kaynağı değildir. Ayrıntı için [`README.md`](../README.md), [`AGENTS.md`](../AGENTS.md), [`docs/CONTINUATION.md`](CONTINUATION.md) ve aşağıdaki asıl belgelere bak.

## Şu anki durum

- Yalnız planlama ve tasarım çalışması var. Uygulama, `package.json`, kurulu bağımlılık, tamamlanmış faz, test/build sonucu veya dağıtım yok.
- Kullanıcı ikinci tasarım turu için “Daha da yaklaştın” dedi: önceki tura göre daha yakın buldu, son tasarım olarak onaylamadı.
- Sıradaki iş ikinci görsel yönü ve gerçek normal/aksayan hafta akışlarını rafine etmek. Faz 0 veya kodlama kendiliğinden başlamaz.
- Tasarım maketleri uygulama ekranı ve ölçü/erişilebilirlik kanıtı değildir. [`docs/11-UX-WALKTHROUGH.md`](11-UX-WALKTHROUGH.md) ve [`docs/12-VISUAL-RESEARCH.md`](12-VISUAL-RESEARCH.md) öneri/inceleme belgeleridir.

## Kabul edilmiş ürün çerçevesi

Tek kullanıcılı Türkçe, Android odaklı plan ve para uygulaması; amaç daha az karar yükü, kolay başlama/toparlanma ve görünür ödemelerdir. Kullanıcı yazılımcı değildir; Türkçe, kısa ve sade iletişim kullan. Teknik alternatifleri ajan somutlaştırır; kullanıcıdan tasarım veya kod uzmanlığı beklenmez.

- Kesin randevu, esnek zaman penceresi ve tekrar döngüsü farklı kavramlardır. Mesai içi ek iş 08:00–17:00 arasında uygun zamanda; sabit 10:00/15:00 yok.
- Öğle aile görüşmesi yok. Cumartesi ile Salı/Perşembe tercihleri kesinleşmiş randevu değildir. Evde fitness gün aşırı; OpenGym içeriği Gün Gün'e kopyalanmaz. Boks saati/tarihi bilinmiyor.
- Sakin Bugün listeleri, haftalık onaylı plan, tek satır yakalama ve iş yığmadan toparlama hedeflenir. Zorunlu günlük not, başarı yüzdesi, joker ve seri baskısı yok.
- Fatura tanımı, dönem ve gerçekleşmiş ödeme ayrıdır. Bilinmeyen tutar sıfır sayılamaz. Para transferi, planlanan birikim ve gerçek birikim farklıdır; açık finans kararları çözülmeden kesin harcanabilir tutar gösterilmez.
- Temel hatırlatma MVP ihtiyacıdır; platform seçimi ve gerçek cihaz kanıtı Faz 0 kapısıdır. Yerel veri, şifreli yedek ve gizlilik sınırları kaynak belgelerde tanımlıdır.
- Kişisel `docs/PROFIL.local.md`, `.env*` ve `*.gungun` depoya girmez. Örneklerde yalnız uydurma veri kullanılır.

## Kaynaklar ve açık kararlar

`docs/01-PRD.md` kapsamı ve tek açık karar listesi O1–O7'yi; `02-SCREENS.md` akışları; `03-DESIGN.md` görsel sistemi; `04-ARCHITECTURE.md` teknik sınırları; `05-DATA-MODEL.md` veri/hesap kurallarını; `06-ROADMAP.md` fazları; `07-TEST-PLAN.md` gelecekteki kanıt ölçütlerini tanımlar. `docs/10-DECISIONS.md` kabul edilen değişiklik kaydıdır. Sıra ve çatışma kuralı için `AGENTS.md` geçerlidir.

O1 platform/bildirim teslimi; O2 fitness erteleme döngüsü; O3 kesin kişisel saatler ve süreler; O4 tahmini para, kart ödemesi, iade ve düzeltme; O5 yedek birleştirme; O6 bildirim tekrar/sessiz saat/mahremiyet; O7 font ve araç sürümü doğrulaması. Detay ve çözüm aşamaları PRD 10'dadır.

## Araçlar ve çalışma biçimi

Kullanıcı `design-mobile-apps` ve `find-skills` kaynaklarını verdi; kaynak yönergeleri incelendi. Sleek API/hesabı bu ortamda bağlı değil ve kullanılmadı. İki raster konsept görsel üretim aracıyla hazırlandı; `docs/12-VISUAL-RESEARCH.md` resmî repo görsellerine dayalı sınırlı karşılaştırmayı ve kaynakları açıklar. Global skill kurulumu yapılmadı.

İş akışında root ajanı koordine eder; uygunsa bağımsız ve sınırlı işleri alt ajanlara verir. Rutin küçük işlerde gpt-6-luna, karmaşık tasarım/mimari/incelemede gpt-6.1-sol tercih edilir; araçlar yoksa mevcut olanla sürdürülür ve bu belirtilir. Gereksiz tekrar okuma/delegasyondan kaçın. Bu depo belgeden ibarettir; uygulama komutları henüz yoktur.
