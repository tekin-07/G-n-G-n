# 10 - Planlama Karar Kaydı

12 Kasım 2026. Kullanıcı önerileri kabul etti ve eski belgelerin yenileriyle güncellenmesine izin verdi. Bu kayıt ürün kararlarını izler; kodlama, platform seçimi veya yayın onayı değildir. Özel profil dosyasındaki değerler kopyalanmadı. Teknik araştırma bu güncellemede yeniden yapılmadı.

## Kabul edilip kaynaklara işlenen değişiklikler
| Eski yaklaşım | Güncel karar | Asıl kaynak |
|---|---|---|
| Her gün öğle aile görüşmesi, hafta sonu tek seçim | Öğle görüşmesi yok; Cumartesi ve Salı/Perşembe tercihleri, haftada birden fazla kesinleşebilir görüşme | PRD 2/F7, Screens 2.3, Data activities/weekPlans |
| Ek işi 10:00'a yerleştir, 15:00 kontrol et | Mesai 08:00–17:00 içinde uygun anda; zorunlu başlangıç/kontrol saati yok | PRD 2/F3, Data 4.6 |
| Spor haftanın dört sabit günü | Evde gün aşırı fitness; içerik OpenGym'de, plan/tamamlama Gün Gün'de | PRD 2/F4, Data Recurrence |
| Boks/salsa için kesin Aralık rutini | Boks takvimi bilinmiyor; kayıt hazırlığı ve daha sonra kesin etkinlik; salsa otomatik tohumlanmaz | PRD 2, Screens 2.4 |
| Dolgun Bugün paneli, baskın seri | Üç sakin liste, tamamlananlar ve toparlama; zorunlu %60/joker kaldırıldı | PRD F2/F6, Screens 2.2/2.6, Design 9 |
| Her akşam hedef notu | Hedefe bağlı küçük adımlar/aktiviteler; tek tamamlama, isteğe bağlı not; derin günlük sonra | PRD F4/F8, Roadmap 2/4/8 |
| Yeni fikir için baştan tam form | Tek satır yakalama, tarihsiz liste, idempotent dönüşüm, etkinliğe hazırlık görevi | PRD F5, Data captures |
| Plan bozulunca biriken işler | Taşı/haftadan çıkar/duraklat; otomatik yarına yığma yok; etki önizlemesi | PRD F6, Screens 2.6 |
| Bildirim v2 | Temel bildirim MVP; platform/gerçek cihaz kanıtı Faz 0 | PRD F13/O1, Architecture 7 |
| Kategori dağılımı v1 | Temel aylık dağılım, birikim ve ödeme görünümü MVP | PRD F11, Roadmap 3 |
| Vadesi gelen sabit gider otomatik posted olabilir | Fatura dönemi ve gerçekleşen ödeme ayrı; talimat ödeme kanıtı değil; bilinmeyen tutar destekli | PRD F10, Data bills |
| Yedek ilk kez Faz 6 | Temel şifreli yedek/restore Faz 1, gerçek veri öncesi; dosya/fiş genişlemesi Faz 6 | PRD F14, Roadmap 1/6 |
| Hazır birleştir seçeneği / şifresiz kurtarma önerisi | Birleştirme politika/test kapısına bağlı; şifreli kurtarma yedeği | PRD O5, Architecture 8 |
| Genel eklenti altyapısı ilk günden | Sade özellik sınırları/composition, ihtiyaç çıkınca genelleştirme | Architecture 5 |
| Uzun karşılamayı tamamlama yükü | Kademeli başlangıç, ihtiyaç anında detay, taslak koruma | PRD F1, Screens 2.1 |

## Karar olmayan ayrıntılar
PRD O1–O7 tek açık karar listesidir. Android/Capacitor seçilmedi; boks/görüşme saatleri kesinleşmedi; uyku/fitness süresi uydurulmadı; fitness erteleme döngüsü, finans tahmini/iade/ödeme dağıtımı, yedek birleştirme ve bildirim tekrarı ayrıntıları ilgili tasarım kapılarında çözülecek. Veri alanları bu davranışların teknik taslağıdır, üretimde mevcut şema değildir.

## Faz eşlemesi ve devam
Eski Faz 3 hedeflerin kapsamı yeni Faz 2/4'e; eski Faz 4 temel para yeni Faz 3'e; eski Faz 6 yedek yeni Faz 1'e; eski Faz 12 temel bildirim kararı yeni Faz 0'a taşındı. Güncel görevler için yalnız 06-ROADMAP kullanılır; eski sohbet numaraları otomatik görev emri değildir. Ayrıntılı istatistik/hedef günlüğü v1, OCR/AI sonraki sürüm olarak korunur.

Sonraki görüşme: gerçek bir haftanın ekran ve eylemlerini somutlaştırmak. Amaç yeni özellik yığmak değil; normal ve aksayan günlerde plan/para/hatırlatma akışının az çabayla işlemesini değerlendirmek. Kullanıcıya sorular kısa toplu gruplarla; uzmanlık gerektiren teknik çözüm seçenekleri ajan tarafından somutlaştırılır.

## Güncel tasarım durumu

Kullanıcı ikinci görsel konsept için “Daha da yaklaştın” dedi: ilk tura göre daha yakın buldu, ancak son tasarım olarak onaylamadı. İkinci konsept inceleme önerisi olarak kalır. Devam adımı gerçek haftanın normal/aksayan gün akışlarıyla bu yönü rafine etmektir; kodlama veya Faz 0 onayı değildir. Oturum devri: [`docs/CONTINUATION.md`](CONTINUATION.md).
