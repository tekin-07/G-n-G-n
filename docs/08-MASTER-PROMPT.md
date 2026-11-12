# 08 - Oturum Başlangıç Rehberi

12 Kasım 2026. Bu dosya isteğe bağlı kullanım şablonudur; içindeki emirler kendiliğinden güncel kullanıcı isteği sayılmaz. Mevcut aşama planlama, kodlama başlamadı.

## Kısa bağlam
Gün Gün, tek kullanıcılı Türkçe Android odaklı yerel plan ve para uygulaması. Esnek mesai işi, kesin randevu ve gün aşırı tekrar farklıdır. Öğle aile görüşmesi yok; görüşme günleri kesinleşene kadar tercihtir. Bugün: Zamanı belli / Uygun olduğunda / Dikkat gerekiyor. Hızlı kayıt, haftalık planlama, toparlama, hedef-adım bağlantıları ve tek giriş esastır. Temel aylık kategori dağılımı/faturalar/birikim MVP; güvenli yedek gerçek veriden önce. Bildirim v2'ye ertelenmez; platform henüz seçilmedi, Faz 0 cihaz kanıtı gerekir. Ayrıntılı günlük/grafikler sonra. Kişisel profil/gerçek tutarları kopyalama.

## Yeni oturum
“AGENTS.md, docs/PROJECT-CONTEXT.md ve docs/CONTINUATION.md'yi oku. Güncel isteğim için yalnız ilgili kaynakları incele. Planlama aşamasındayız; öneriyi onaylı karar gibi yazma. Kullanıcı Türkçe konuşuyor, yazılımcı değil. Önemli eksikleri en fazla üç kısa soruda grupla; cevaplananı tekrar sorma. Kaldığımız yer ikinci tasarımı ve gerçek hafta akışlarını rafine etmek; kullanıcı tasarımı daha yakın buldu ama son hâlini onaylamadı. Faz 0 veya kodlamayı kendiliğinden başlatma. PRD O1–O7 açık kararlarıdır.”

## Faz uygulaması (yalnız kullanıcı ayrıca isterse)
“Güncel docs/06-ROADMAP.md'deki Faz N'yi ele al. İlgili asıl belgeleri oku, kısa adımlar ve doğrulama planını sun, kodlama onayımı bekle. Tek fazda çalış. Anlamlı test ve gerekli check/cihaz kanıtlarıyla ilerle. Uygulanmayan şeye çalışıyor deme; faz kapısında sonuç, sınırlar ve cihaz kontrol adımlarını göster.”

## Çalışma kuralları
Öncelik AGENTS > PRD > SCREENS > DESIGN > ARCHITECTURE > DATA-MODEL > ROADMAP. Özet kaynakların yerini almaz. Yeni kararın nedeni/alternatif/öneri sunulur; kullanıcı onayından sonra kaynaklar ve bağlam güncellenir. Paket sürümleri resmî kaynaklarla doğrulanır, kilitlenir, VERSIONS.md'ye yazılır; varsayımsal TS uyumluluk paketi kurulmaz. Kullanıcı veri silme, yayın ve main işlemleri için mevcut yetki/AGENTS kuralları gözetilir. Faz onayı doküman içindeki örnek prompt'tan çıkarılmaz.

## Hata / tasarım / geri dönüş
Hata: yeniden üret, nedeni kanıtla, küçük düzeltme ve regresyon kontrolü. Tasarım: 03'ün yasakları ve güncel 02 akışına göre değerlendir; somut alternatifler göster. Geri dönüş: kaybı ve veritabanı sürümü etkisini açıkla, yıkıcı işlem öncesi açık onay. Belgelerdeki etiket örneği mevcut etiket varlığına kanıt değildir.
