# AGENTS.md - Gün Gün için yapay zekâ geliştirici kuralları

Bu dosya Codex, Claude Code ve benzeri araçlar içindir. Her oturumun başında oku.

## 1. Kullanıcı ve iletişim
- Kullanıcı yazılımcı DEĞİLDİR. Türkçe, kısa, sade cevap ver. Jargon kullanma; kullanırsan bir cümleyle açıkla.
- Sohbette kod yığma. "Ne yaptım, nasıl kontrol edersin, sırada ne var" üçlüsünü yaz. Dosya adlarını ve komutları yalnızca gerektiğinde ver.
- Gerekli soruları en fazla 3 kısa soruda grupla; tek tek uzun soru zinciri kurma, cevaplananları tekrar sorma. Düşük etkili seçimleri yapıp belirt; önemli öneriyi onaylı karar gibi yazma. Teknik seçenekleri ajan somutlaştırır, kullanıcıdan geliştirici uzmanlığı beklenmez.
- Kullanıcıya terminal komutu yazdırman gerekirse kopyala-yapıştır hazır ver ve ne işe yaradığını tek cümleyle söyle.
- Hata olunca: ne oldu (düz dille), ne denedin, sıradaki adım. Suçlama/özür yağmuru yok.

## 2. Belgeler ve öncelik
Her oturumda `docs/PROJECT-CONTEXT.md` ve güncel görev devri için `docs/CONTINUATION.md` dosyalarını hızlı başvuru olarak oku; ardından yalnız göreve ve aktif aşamaya uygun asıl belge bölümlerini incele. Özet, asıl belgelerin yerine geçmez. Ürün/kapsam için `docs/01-PRD.md`; ekran için `docs/02-SCREENS.md`; görünüm için `docs/03-DESIGN.md`; mimari için `docs/04-ARCHITECTURE.md`; veri/hesap için `docs/05-DATA-MODEL.md`; sıra için `docs/06-ROADMAP.md`; kalite için `docs/07-TEST-PLAN.md`. Faz başında, karar değişikliğinde veya kaynaklar arasında çelişki şüphesinde ilgili asıl belgelerin tamamını oku. `docs/PROFIL.local.md` varsa kişisel değerler içindir (bölüm 8).
Çelişki sırası: bu dosya > PRD > SCREENS > DESIGN > ARCHITECTURE > DATA-MODEL > ROADMAP. Çelişki bulursan DUR, kullanıcıya göster.
Belgede olmayan bir şeyi uydurma. Eksikse sor veya "varsayım" diye yazıp `docs/01-PRD.md` bölüm 10'a ekle.

### Karar değişikliği
Bir karar değişecekse: önce kullanıcıya sebep + alternatif + öneri sun; onayla; SONRA ilgili belgeyi güncelle; sonra kod. Belge güncellenmeden kodda sessiz sapma yapma.

## 3. Çalışma yöntemi
1. Tek seferde tek faz (`docs/06-ROADMAP.md`). Faz dışına çıkma; fikirlerini "öneri listesi"ne yaz, uygulama.
2. Faz başında: ilgili belgeleri oku -> kısa plan (adım + her adımın doğrulaması) -> kullanıcı onayı -> uygula.
3. Küçük adımlar. Her görevden sonra `pnpm check` çalıştır. Kırıksa düzelt, sonra devam et.
4. Önce basit çözüm. İstenmeyen soyutlama, "belki lazım olur" kodu, kullanılmayan yapılandırma yazma. 200 satır yazıp 50'ye inebilecekse indir.
5. Cerrahi değişiklik: yalnızca görevle ilgili satırlara dokun; komşu kodu "güzelleştirme". Kendi değişikliğinin yetim bıraktığı import/değişkenleri temizle, eskilerini bırak (ama bildir).
6. Doğrulanabilir hedef: "çalışıyor" yerine test/çıktı göster. Hata düzeltirken önce yeniden üreten test yaz.
7. Faz sonunda ROADMAP'teki faz kapısı listesini kanıtla göster ve kullanıcı onayı bekle.

## 4. Komutlar (Faz 0'da oluşturulur)
`pnpm install`, `pnpm dev` (geliştirme sunucusu), `pnpm check` (biome + tsc + vitest + dependency-cruiser + build), `pnpm test`, `pnpm test:e2e`, `pnpm build`.
Kod/uygulama fazında `pnpm check` yeşil değilken tamamlandı deme. Yalnız belge işinde içerik, bağlantı ve diff tutarlılığı kontrol edilir; mevcut olmayan komutlar/testler çalıştı denmez. Şu anda yalnız planlama belgeleri vardır.

## 5. Kod kuralları
- TypeScript `strict`; `any` yok; `as` dönüşümü yalnızca doğrulanmış sınırlarda ve yorumla.
- Katman kuralları 04-ARCHITECTURE.md bölüm 4'tedir ve CI'da zorlanır. `core` saf; DOM/Dexie/React yok. Derin içe aktarma yok.
- Para tam sayı minor unit; `number` ile ondalık para hesabı yok; kur çarpımı BigInt. Tarih/saat `core/time` üzerinden; ham `Date` ile iş mantığı yok.
- Her yazma bir transaction; yarım kayıt yok. Silme = yumuşak silme + geri al.
- Her şema/Zod/Dexie değişikliği için migrasyon + test.
- Kullanıcı metinleri yalnızca `i18n/tr.ts`. Kod, dosya ve değişken adları İngilizce; yorumlar İngilizce, kısa ve "neden"i anlatır.
- Dosya <= 300 satır, fonksiyon <= 50 satır ana kural; aşılırsa bölmenin neden zor olduğunu yaz.
- `console.log` yok (geliştirmede bile commit'e girmez). Finans verisi loga yazılmaz.
- Yeni bağımlılık: 04-ARCHITECTURE.md listesinde yoksa ÖNCE kullanıcıdan izin iste (ne, neden, alternatifi, boyutu, bakımı).
- Erişilebilirlik: etiketsiz ikon düğmesi yok, dokunma hedefi >= 48 dp, odak görünür, hareket azaltma desteği.

## 6. Tasarım kuralları (kısa; asıl kaynak 03-DESIGN.md)
Emoji yok. Gradient/buzlu cam yok. Mor/indigo yok. Inter/Roboto başlık yok. Eşit kartlı "üçlü ızgara" yok. Tek köşe yarıçapı her yere yok. Renk yalnızca token'dan. Her UI işinin sonunda 360 ve 412 px, açık ve koyu ekran görüntüsü al ve 03-DESIGN.md bölüm 3'e karşı öz-eleştiri yap. Marka/logo/karakter kopyalama yok.

## 7. Güvenlik ve gizlilik
- MVP/v1 kullanıcı verisi yerel; uygulama dosyalarının ilk kurulumu/güncellemesi dışında dış servis isteği yok (analitik, CDN, harici yazı tipi, üçüncü taraf betik yok). Bildirim platformu açık karardır; sunucu/AI eklemek ayrıca onay gerektirir.
- API anahtarı/sır asla depoya girmez. `.env*` git'e girmez.
- `dangerouslySetInnerHTML` yok. Kullanıcı metni her zaman metin.
- Yedek dosyaları (`*.gungun`) ve kişisel dosyalar git'e girmez.
- Kullanıcıya ait veriyi silecek/üzerine yazacak her işlem açık onay ister.

## 8. Kişisel veri ve açık depo
Depo herkese açık olacaktır. `docs/PROFIL.local.md` yalnızca yerelde durur ve `.gitignore`'dadır.
- Her commit'ten önce `git check-ignore -v docs/PROFIL.local.md` ile korunduğunu doğrula; değilse commit yapma.
- O dosyadaki değerleri (maaş tutarı vb.) kodun, testin, fixture'ın, ekran görüntüsünün, commit mesajının veya belgenin içine KOPYALAMA. Testlerde ve demo verisinde uydurma değerler kullan (örn. maaş 50.000 TL).
- Kullanıcı değerleri uygulamaya yalnızca çalışma anında (karşılama ekranı) girilir.

## 9. Git
- Her faz için dal: `faz-N-kisa-ad`. Küçük, anlamlı commit'ler (Conventional Commits: `feat:`, `fix:`, `test:`, `docs:`, `chore:`).
- `main`'e birleştirme ve itme yalnızca faz kapısı geçildikten ve kullanıcı "onayla" dedikten sonra. Zorla itme (`--force`) yok. Etiket: `faz-N`.
- Kullanıcının açıkça istediği belge güncelleme/yayımlama izni yalnızca belgeleri kapsar; uygulama faz kapısı, kodlama veya uygulama yayını izni sayılmaz.
- Commit'ler `main`'e bozuk gitmez: CI yeşil olmalı.
- Geri dönüş istenirse etikete dönerek yap, kullanıcıya düz dille anlat.

## 10. Yapma listesi
Kapsam genişletme. Belgeye uymayan "iyileştirme". İzinsiz bağımlılık. Kod davranışını uygun doğrulama olmadan tamamlandı saymak. Kullanıcı verisini silen/geçersiz kılan değişiklik (migrasyonsuz). Gerçek kişisel veri girmek. Tasarım yasaklarını çiğnemek. Çalışmayan bir şeyi "çalışıyor" diye raporlamak. Bilmediğin bir şeyi (sürüm, API) uydurmak: önce doğrula (paket sürümü için `npm view`, belge için resmî doküman).

## 11. Bakım
Haftalık bağımlılık güncelleme PR'ları (gruplu) ayrı bir bakım görevidir; `pnpm audit` yüksek/kritik bulgu varsa kullanıcıya düz dille bildir. Kurulan sürümler `docs/VERSIONS.md`'ye yazılır (Faz 0'da sen oluşturursun, 06-ROADMAP.md görev 0.8); güncelleme yaptıkça bu dosyayı da güncelle.

## 12. Güncel planlama ilkeleri
Saatli, esnek ve döngüsel işleri ayır; tercihleri kesin randevu yapma. Öğle görüşmesi/sabit dört spor günü/15:00 ek iş sorgusu eski varsayımdır. Bildirim kararı erken, yedek gerçek veri öncesi, temel aylık para dağılımı MVP. Tek giriş, kademeli onboarding ve toparlama; zorunlu başarı yüzdesi/joker/seri yok. Ürün ayrıntıları PRD, açık kararlar PRD 10, değişiklik izi docs/10-DECISIONS.md. Kabul edilen değişiklikte bağlı kaynakları ve PROJECT-CONTEXT.md'yi aynı işte eşitle.
