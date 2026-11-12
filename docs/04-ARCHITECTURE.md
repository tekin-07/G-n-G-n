# 04 - Mimari ve Teknoloji Seçimleri (Gün Gün)

Durum: 12 Kasım 2026 planlama güncellemesi; platform/bildirim seçimi PRD O1 kapsamında açık. Bu belge "nasıl yapılır" sorusunun tek kaynağıdır. Değişiklik gerekirse önce bu belge güncellenir (bkz. AGENTS.md, "Karar değişikliği").

## 1. Temel karar: "local-first" ve backend sorusu

Gün Gün finans, aile ve rutin verisi tutar. Bu veriyi bir sunucuda saklamak; hesap açma, kimlik doğrulama, güvenlik açığı yönetimi, KVKK sorumluluğu ve işletme maliyeti demektir. Tek kullanıcılı kişisel bir uygulama için bu risk, getirdiği faydadan büyüktür.

Karar: İlk sürümlerde (MVP, v1) veri yalnızca kullanıcının telefonunda durur. Sunucu yoktur. "Profesyonel backend" ihtiyacı, sunucu olmadan da karşılanır: backend'in işlevi olan iş kuralları ve veri erişimi, arayüzden tamamen ayrı iki pakete konur.

| Paket | Görev | Backend'deki karşılığı |
|---|---|---|
| `packages/core` | Saf TypeScript: para, tarih, tekrar kuralları, plan üretimi, taksit/kart/ekstre hesapları, tamamlama/ilerleme, istatistik, doğrulama şemaları, depo arayüzleri (ports) | Servis ve domain katmanı |
| `packages/data` | Dexie (IndexedDB) şeması, depo uygulamaları, migrasyonlar, yedek/şifreleme, günlük işler (otomatik kayıtlar) | Veri erişimi ve veritabanı katmanı |
| `apps/web` | React arayüzü, web/PWA kabuğu; seçilirse Android sarmalayıcısında ortak UI | Frontend |
| `apps/server` | Ancak ayrı ihtiyaç/onay sonrası; MVP/v1 için yok | İsteğe bağlı sonraki servis |

Bu ayrım sayesinde: (a) hesaplar arayüzden bağımsız test edilir, (b) veri katmanı ileride SQLite/senkronizasyon ile değiştirilebilir, (c) aynı çekirdek sunucuda ve Android uygulamasında yeniden kullanılır.

## 2. Teknoloji tablosu

Sürüm politikası: "en son kararlı sürüm" Faz 0'da kurulur, kilit dosyasıyla sabitlenir, kurulan sürümler `docs/VERSIONS.md` dosyasına yazılır. Eski araştırmadaki majör sürüm/tarih iddiaları bu oturumda doğrulanmadı; kesin teknik karar sayılmaz. Kurulumda resmî sürüm notları, `npm view <paket> version` ve araç uyumluluğu kontrol edilir; sonuçlar VERSIONS.md'ye yazılır.

| Alan | Seçim | Gerekçe |
|---|---|---|
| Dil | TypeScript, `strict` açık; kararlı uyumlu sürüm kurulumda doğrulanır | Tür güvenliği; uyumluluk paketi veya sürüm düşürme ancak resmî destek/uyumluluk doğrulanarak kararlaştırılır |
| Paket yönetimi | pnpm (workspaces) | Katı bağımlılık çözümü, katman ihlallerini erken yakalar. Node 25 ve sonrası Corepack'i artık içermez; kurulum `npm install -g pnpm` ile yapılır |
| Derleme/dev sunucu | Vite + `@vitejs/plugin-react` (uyumlu kararlı sürümler doğrulanır) | Hızlı, PWA eklentisi olgun |
| UI | React 19 | Ajanların en iyi bildiği ekosistem; bileşen/erişilebilirlik kütüphaneleri geniş |
| Yönlendirme | React Router, `createHashRouter` | GitHub Pages'te derin bağlantı 404 sorunu yaşamaz; PWA'da kararlı |
| Stil | Tailwind CSS v4, token'lar CSS değişkeni olarak (`@theme`) | Tutarlı tasarım dili, açık/koyu tema kolay |
| Erişilebilir bileşenler | Radix UI primitive'leri (Dialog, Popover, Tabs, Switch, Toast, ToggleGroup...) | Klavye/ekran okuyucu desteği hazır; görünüm bizim token'larımızla |
| İkonlar | `@phosphor-icons/react` (regular + fill) | Tek set, tutarlı çizgi; yumuşak karakter. Emoji yasak |
| Formlar | react-hook-form + zod (`@hookform/resolvers`) | Şemalar `core`'dan gelir, tek doğrulama kaynağı |
| UI durumu | Zustand (sadece geçici UI durumu) | Küçük ve basit |
| Veri | Dexie 4 + `dexie-react-hooks` (`useLiveQuery`) | IndexedDB için olgun sarmalayıcı; canlı sorgular arayüzü otomatik günceller |
| Doğrulama | Zod 4 | Yedek içe aktarma ve form doğrulaması |
| Tarih/saat | Temporal API; `temporal-polyfill` yalnızca `core/time` içinde | Tarayıcı/Node desteği kurulumda doğrulanır; core/time sınırında aynı davranış sağlanır |
| Kimlik | ULID (`ulid`) | Zaman sıralı, ileride senkronizasyona uygun |
| PWA | `vite-plugin-pwa` (Workbox), `registerType: 'prompt'` | Çevrimdışı çalışma; güncelleme kullanıcıya sorulur, yarım form kaybolmaz |
| Grafik (v1) | Özel SVG bileşenleri + `d3-scale`/`d3-shape` | Hazır grafik kütüphanelerinin "şablon" görünümünden kaçınır, küçük |
| Test | Vitest, Testing Library, `fake-indexeddb`, `fast-check`, Playwright, `@axe-core/playwright` | Katmanlara göre bölüm 11'de |
| Lint/format | Biome | Tek araç; seçilen TypeScript sürümüyle uyumluluğu doğrulanır |
| Mimari denetim | `dependency-cruiser` | Katman kurallarını CI'da zorlar |
| CI/CD | GitHub Actions, GitHub Pages | Ücretsiz; Free planda Pages için depo herkese açık olmalıdır (bkz. bölüm 9) |
| Şifreleme | Web Crypto: AES-256-GCM + PBKDF2-HMAC-SHA256 (en az 600.000 yineleme) | Ek bağımlılık yok; OWASP PBKDF2 için bu eşiği önerir. Üstü Argon2id'dir, başlıkta `kdf` alanı olduğu için ileride yükseltilir |

Capacitor ve yerel bildirim eklentisi Faz 0 değerlendirme adaylarıdır; üretim bağımlılığı olarak seçilmiş değildir. Bildirim adayı için deneme onayı, üretim seçimi anlamına gelmez. Yeni bağımlılık eklemek için izin gerekir (AGENTS.md). Yukarıdaki liste "izinli bağımlılıklar" listesidir.

## 3. Depo yapısı

```
gun-gun/
  AGENTS.md  CLAUDE.md  README.md  .gitignore  biome.json  pnpm-workspace.yaml
  .dependency-cruiser.cjs  package.json  tsconfig.base.json
  docs/                      (bu belgeler; PROFIL.local.md git'e GİRMEZ)
  .github/workflows/         ci.yml  deploy.yml
  packages/
    core/
      src/
        time/        LocalDate, LocalTime, YearMonth, WeekKey, hafta/ay yardımcıları (Temporal burada)
        money/       Minor, AssetDef, formatTRY, valuation (BigInt), yuvarlama
        schemas/     Zod şemaları (tüm varlıklar) + türler
        ports/       Repository arayüzleri (ActivityRepo, TransactionRepo ...)
        schedule/    expandOccurrences, autoplan, conflicts, weeklyPlan
        progress/    tamamlanan işler; isteğe bağlı göstergeler ayrı onayla
        finance/     balances, netWorth, installments, cardStatements, fixedExpenses, savings, budget
        stats/       günlük/hafta sonu/aylık özetler
        reminders/   hatırlatma kuralları/niyetleri, yardımcı ics üretimi
        jobs/        runDailyJobs (saf mantık; yazma portlar üzerinden)
        index.ts     tek giriş: yalnızca buradan dışa aktarılanlar kullanılabilir
      test/
    data/
      src/
        db.ts        Dexie şeması + migrasyon sürümleri
        repos/       ports'un Dexie uygulamaları
        backup/      serialize, encrypt, decrypt, restore, validate
        receipts/    görsel küçültme + saklama
        storage/     persist(), estimate()
        index.ts
      test/ (fixtures: sürüm başına sahte veri JSON)
  apps/
    web/
      public/        ikonlar (192, 512, maskable), yazı tipi dosyaları (self-host)
      src/
        app/         main.tsx, router.tsx, providers.tsx, composition.ts, theme/
        components/ui/   Button, Sheet, Chip, Card, Field, Money, EmptyState ... (paylaşılan)
        features/
          onboarding/  today/  plan/  goals/  money/  stats/  settings/  backup/
            (her biri: components/, hooks/, routes.tsx, index.ts, reminders.ts?)
        i18n/        tr.ts (tüm arayüz metinleri)
        lib/         küçük yardımcılar
        pwa/         kayıt, güncelleme bildirimi, kurulum yardımı
      e2e/           Playwright
    server/          (ihtiyaç/onay olmadan klasör oluşturulmaz)
```

## 4. Katman kuralları (CI'da `dependency-cruiser` ile zorlanır)

1. `core` hiçbir şeyi `data` veya `web`'den içe aktarmaz. DOM, React, Dexie, `window`, `localStorage`, `fetch` yoktur. Saat `now()` parametre olarak verilir (test edilebilirlik).
2. `data`, `core`'u kullanır; `web`'i bilmez. React yoktur.
3. `web`, `core` ve `data`'yı yalnızca `index.ts` girişlerinden içe aktarır (derin içe aktarma yasak).
4. Depo örnekleri yalnızca `apps/web/src/app/composition.ts` içinde oluşturulur; özellikler bunları bağlam (context) kancalarıyla alır.
5. `features/X`, başka bir `features/Y`'nin iç dosyalarını içe aktaramaz. Ortak bir şey gerekiyorsa `core`'a (mantık) veya `components/ui`'ya (görünüm) taşınır.
6. Kullanıcıya görünen tüm metin `i18n/tr.ts` içindedir. Bileşenlerde sabit Türkçe metin yasak.
7. Para her yerde tam sayı "minor unit"tir. `number` ile ondalık para hesabı yapılamaz. Kur çarpımı `BigInt` ile yapılır.
8. Tarih ve saat alanları düz dize (`LocalDate`, `LocalTime`) olarak saklanır; anlık zaman damgaları epoch ms (UTC). Türkiye'de yaz saati yoktur (UTC+3 sabit) ama kod saat dilimini sabit varsaymaz: `Europe/Istanbul` ayarlardan gelir.

## 5. Veri akışı

Okuma: `useLiveQuery(() => repo.xxx())` -> bileşen. Veri değişince arayüz kendiliğinden güncellenir.
Yazma: bileşen -> özellik servisi (`features/X/hooks`) -> `core` mantığı (hesap/doğrulama) -> depo (`data`), tek Dexie transaction içinde. Birden fazla tabloyu etkileyen işlemler (taksit planı oluşturma, transfer, yedek geri yükleme) tek transaction'dır; yarım kayıt oluşmaz.
Silme: "yumuşak silme" (`deletedAt`). Ekranda 8 saniyelik "Geri al" çubuğu çıkar. 30 gün sonra temizlenir. Depolar silinmişleri varsayılan olarak gizler.

### Günlük işler (`runDailyJobs`)
Uygulama açılışında ve `visibilitychange` ile ön plana gelince çalışır. Her biri idempotenttir (tekrar çalışması zarar vermez; `autoKey` benzersiz indeksi ile korunur):
1. Zamanı gelmiş gelirler, fatura dönemleri ve taksitlerin ekstreye yansıması telafi edilir. Faturanın vadesi veya otomatik ödeme talimatı, ödeme kanıtı değildir; kendiliğinden posted gider/ödendi durumu üretmez. Gelir otomasyonu banka doğrulaması değildir; kullanıcı düzeltmesi desteklenir.
2. Haftalık net değer anlık görüntüsü (`snapshots`).
3. Süresi geçmiş geçici durumlar için temizlik (silinenler > 30 gün).
4. Uyarı kartlarının girdileri hesaplanır (kaydedilmez, ekranda anlık üretilir): sabit gider yaklaşıyor, ekstre son ödeme günü, kur eski, yedek zamanı.

### Ölçülü genişleme ve tek işlem yolu
Özellik klasörleri korunur; baştan genel `FeatureModule`/eklenti/registry çerçevesi zorunlu değildir. Basit açık import ve composition yeterlidir; tekrar eden somut ihtiyaç çıkarsa genelleştirilir. Tablo yedekleme kapsamı merkezi ve sürümlü veri şemasından gelir; bir modül unutulduğunda veri kaybı olmamalıdır.

UI ve bildirim eylemleri aynı domain komutlarını kullanır. Tek tamamlama kaydı hedef/Plan/Bugün'e yansır. Yakalama dönüşümü, fatura ödemesi ve etkinlik-gider bağlantısı idempotent anahtarlarla korunur. OS bildirimi ile Dexie atomik tek işlem olamayacağı için değişiklik transaction'ında yeniden planlama ihtiyacı işaretlenir; adaptör başarılı olana kadar uzlaştırılır. Eski bildirim eylemi güncel kayıt/sürümü doğrular; silinmiş iş veya ödenmiş faturayı tekrar işletmez.

`core/reminders` zaman ve içerik kurallarını üretir; platform adaptörü planlar/iptal eder ve izin durumunu raporlar. Platform tanıtıcıları domain kimlikleri yerine geçmez. Cihaz bildirim kuyruğu yedekten körlemesine yüklenmez; yeni cihazda izinler kontrol edilip yeniden hesaplanır. Ayrıntılı API ve OS desteği Faz 0 kanıtından sonra yazılır.

## 6. Web/PWA kabuğu ve çevrimdışı

Platform kararı açık: aşağıdakiler web/PWA kabuğu içindir. Android sarmalayıcı seçilirse kalıcılık, uygulama kimliği, kurulum/güncelleme, paket imzalama ve yedek dosya paylaşımı ayrıca doğrulanır. Web ile yerel uygulamanın depoları aynı varsayılmaz; taşınma gerekiyorsa şifreli yedek kullanılır.

- Manifest: `name` "Gün Gün", `short_name` "Gün Gün", `lang` "tr", `display` "standalone", `start_url` ve `scope` Vite `base` ile uyumlu göreli (`./`), simgeler 192 ve 512 (maskable dahil), `theme_color`/`background_color` açık temadan. `shortcuts`: "Gider ekle" ve "Bugün" (Android uzun basma kısayolu).
- Service worker: uygulama kabuğu ön belleğe alınır (precache), yazı tipleri dahil. Dış istek yok. `registerType: 'prompt'`: yeni sürüm hazır olunca "Yeni sürüm hazır, yenile" bildirimi; kullanıcı onaylamadan sayfa yenilenmez.
- Vite `base`: GitHub Pages proje sitesi için `/<depo-adı>/`; `VITE_BASE` ortam değişkeniyle ayarlanır.
- Kurulum yardımı: `beforeinstallprompt` yakalanır; karşılamada ve Ayarlar'da "Ana ekrana ekle" adımları gösterilir.
- Kalıcı depolama: ilk anlamlı veri girildikten sonra `navigator.storage.persist()` istenir; sonuç ve `estimate()` kullanımı Ayarlar > Depolama'da gösterilir. Reddedilirse ekranda açık uyarı ve yedek önerisi çıkar.
- Çevrimdışı test zorunludur (Playwright `setOffline`): uygulama açılır, tüm MVP işlevleri çalışır.

## 7. Bildirim ve hatırlatma stratejisi (ilk sürüm kapısı)
Hatırlatma temel kullanım ihtiyacıdır; v2'ye ertelenmez. Web uygulaması kapalıyken sunucusuz kesin zamanlı bildirim güvenilir kabul edilmez. Açılışta/visibilitychange'de çalışan `runDailyJobs` kapalı uygulama alarmı değildir.

| Aday | Fayda | Sınır / karar |
|---|---|---|
| Web/PWA + uygulama içi kart + .ics | Basit dağıtım, çevrimdışı plan/para | Kapalı uygulamada eylemli hatırlatma ihtiyacını tek başına karşıladığı varsayılmaz |
| Capacitor/Android yerel bildirim | Aynı arayüz ve yerel veriyle OS zamanlaması adayı | Kurulum, izin, pil, yeniden başlatma ve Android sürümü etkileri gerçek cihazda ölçülecek; henüz seçilmedi |
| Web Push + servis | Alternatif olarak değerlendirilebilir | Ağ/sunucu gerektirir; MVP/v1 yerel/sunucusuz kararıyla uyumsuz olduğu için ayrıca kapsam/gizlilik onayı olmadan seçilemez |

Faz 0 deneyi: uygulama kapalı ve ekran kilitli, çevrimdışı, yeniden başlatma ve pil kısıtı, izin reddi/geri alma; planlama/iptal/değiştirme, bildirimden eylem ve tekrar dokunma. Planlanan/gerçek teslim gözlemleri ve başarısız koşullar raporlanır. Tam zamanlı alarm/bildirim izin gereksinimleri güncel Android ve eklenti belgeleriyle doğrulanır. Yedi günlük cihaz gözlemi hedeflenir; deney tamamlanmadan başarı iddiası yok.

Sessiz saatler ve kategori tercihleri önce uygulanır. Vade ile hatırlatma zamanı farklıdır. Tamamlanan, taşınan, duraklatılan veya silinen işin eski alarmı iptal edilir. Tutar/hesap eksik fatura bildirimi ödeme formunu açar, otomatik gider üretmez. Platform kuyruğunun kapasite ve ileriye planlama sınırları ölçülür; tekrar ve yeniden planlama stratejisi bununla belgelenir. Teslim edilemeyen izin durumunu UI açık gösterir. .ics yardımcı aktarım; sağlayıcıya bağlı bildirim/tekrar içe aktarma davranışı garantilenmez.

## 8. Güvenlik ve gizlilik

Tehdit modeli (açık yazılmıştır, abartı yok):

| Tehdit | Önlem | Kalan risk |
|---|---|---|
| Telefon kaybolur/çalınır | Android ekran kilidi birincil koruma. v1'de uygulama PIN/parmak izi kilidi (WebAuthn doğrulaması + PIN). Kilit bir "kapı"dır; veri uygulama içinde şifrelenmez | Uygulama kilidi, cihaza fiziksel ve teknik erişimi olan birine karşı tam koruma değildir. Belgede ve arayüzde bu dürüstçe yazılır |
| Tarayıcı verisi silinir / uygulama kaldırılır | Kalıcı depolama isteği, haftalık yedek hatırlatması, şifreli yedek dosyası | Yedek alınmadıysa veri kaybolur. Bu en olası risktir |
| Yedek dosyası sızar | AES-256-GCM, rastgele tuz ve IV, parola ile türetilmiş anahtar. En az 10 karakter + güç göstergesi | Zayıf parola kaba kuvvetle kırılabilir; parola unutulursa yedek açılamaz (uyarı gösterilir) |
| Bağımlılık zinciri saldırısı | Az bağımlılık, kilit dosyası, `pnpm audit` CI'da, Dependabot/Renovate, yeni bağımlılık için izin | Sıfırlanmaz, azaltılır |
| XSS | `dangerouslySetInnerHTML` yasak, CSP meta etiketi (`default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; img-src 'self' data: blob:; font-src 'self'; connect-src 'self'; object-src 'none'; base-uri 'self'; form-action 'none'`), kullanıcı metni her zaman metin olarak basılır. Not: `blob:` fiş önizlemesi için, `'unsafe-inline'` yalnızca stil için (Radix gibi kütüphaneler çalışma anında stil ekler); betikte satır içi izin YOKTUR. Faz 0'da CSP ile uygulama açılıp bir diyalog ve bir görsel denenir | Düşük |
| Depodan kişisel veri sızar | Depo herkese açık olacaktır. Koda/test/belgelere gerçek kişisel veri girmez; `docs/PROFIL.local.md` ve `*.gungun` `.gitignore`'dadır; testlerde uydurma veri | Kullanıcı hatası: ilk commit'ten önce `.gitignore` kontrol edilir |
| Gizli anahtar sızıntısı | MVP/v1'de hiçbir API anahtarı yoktur. v2 sunucusunda anahtarlar yalnızca sunucu ortam değişkenindedir, depoda asla | - |

Ek kurallar: analitik, reklam, üçüncü taraf betik, CDN yazı tipi yok. Finans verisi `console`'a yazılmaz. Hata mesajlarında tutar/not yer almaz.

### Yedek dosyası belirtimi (`.gungun`)
```
bayt 0-6    : "GUNGUN\0" (sihirli)
bayt 7      : biçim sürümü (1)
JSON başlık : { kdf: "pbkdf2-sha256", iter: 600000+, salt: b64(16), iv: b64(12), schemaVersion, exportedAt, includeReceipts }
şifreli gövde: AES-256-GCM( gzip( JSON{ meta, tables{...}, receipts?[...] } ) ), başlık AAD olarak doğrulanır
```
Geri yükleme: parola ile çöz -> Zod ile doğrula -> `schemaVersion` eski ise migrasyonla yükselt -> kayıt özeti -> üzerine yazma açık onayı -> tek transaction. Başarısız olursa mevcut veri değişmez. Önce indirilebilir şifreli kurtarma yedeği önerilir; sessiz şifresiz kopya oluşturulmaz. Birleştirme PRD O5 politikası ve testleri onaylanana kadar sunulmaz. Yedek/geri yükleme ilk gerçek veri kullanımından önce gereklidir. Başlık uzunluğu/çerçeveleme, boyut ve KDF üst sınırları Faz 1 güvenlik tasarımında tanımlanır; mevcut gösterim tam ikili format belirtimi sayılmaz.

## 9. Yayın

Aşağıdaki Pages akışı web/PWA içindir. Android seçilirse imzalama/dağıtım/güncelleme yolu ve gizli imza anahtarı saklama kuralları ayrıca kararlaştırılır; Pages APK güncelleme sistemi değildir. Bu belge yayın yapıldığı anlamına gelmez.

- Free GitHub planında GitHub Pages yalnızca herkese açık depodan yayın yapar; özel depodan yayın için ücretli plan gerekir (Pro/Team). Bu proje için karar: depo herkese açık (kod zaten gizli değil, veri depoda yok). Özel depo istenirse Cloudflare Pages gibi alternatifler ayrıca değerlendirilir.
- `deploy.yml`: `main`'e birleşince `pnpm check` -> `pnpm build` -> `actions/upload-pages-artifact` -> `actions/deploy-pages`. Ayarlarda Pages kaynağı "GitHub Actions" seçilir.
- Her faz sonunda `git tag faz-N` ve (MVP için) `v0.1.0`. Geri dönüş: etikete dön.
- Sürüm numarası arayüzde (Ayarlar > Hakkında) görünür; `CHANGELOG.md` tutulur.

## 10. Performans bütçesi

- İlk yükleme (gzip) JS <= 250 KB, yazı tipleri hariç. Rota bazlı kod bölme: Para ve İstatistik ayrı parça.
- Orta seviye Android'de (CPU 4x yavaşlatma, hızlı 4G benzetimi) açılıştan etkileşime <= 2,5 sn.
- 5.000 işlem + 2 yıllık aktivite kaydıyla liste ekranları 60 fps kaydırır; sanal liste yalnızca gerekirse.
- Fiş görseli <= 250 KB (uzun kenar 1600 px, WebP kalite ~0,7; WebP yoksa JPEG).

## 11. Test stratejisi (özet; ayrıntı 07-TEST-PLAN.md)

- `core`: birim + özellik tabanlı (`fast-check`) testler. Para, taksit, tekrar kuralı, plan toparlama, kart ekstresi için kapsam >= %90 satır. Test vektörleri 05-DATA-MODEL.md'dedir.
- `data`: `fake-indexeddb` ile depo, migrasyon (eski sürüm fixture -> yeni), yedek gidiş-dönüş testleri.
- `web`: Testing Library ile form ve kart bileşenleri; Playwright (Pixel 7 emülasyonu) ile ana akışlar, çevrimdışı senaryo, `axe` erişilebilirlik taraması.
- Her görev "kanıtla bitti" kuralına bağlıdır: test çıktısı olmadan tamamlandı denmez.

## 12. Risk kaydı

| Risk | Olasılık | Etki | Önlem |
|---|---|---|---|
| Kullanıcı yedek almaz, veri kaybolur | Yüksek | Çok yüksek | Haftalık hatırlatma, Bugün'de kart, ilk 3 haftada daha sık hatırlatma, kolay tek dokunuşla yedek |
| Saatli bildirim beklentisi karşılanmaz | Yüksek | Yüksek | Faz 0 platform/cihaz kapısı; izin durumu; dürüst sınırlar; v2'ye ertelememe |
| Kapsam şişmesi | Yüksek | Yüksek | Faz kapıları; her fazda yalnızca o fazın işi; ROADMAP dışına çıkmak yasak |
| Ajan "ortalama" (slop) tasarım üretir | Orta | Orta | 03-DESIGN.md yasak listesi, token'lar, ekran görüntüsüyle öz-eleştiri adımı |
| Araç sürümü/uyumluluğu belirsizliği | Orta | Orta | Resmî doğrulama, kilit dosyası, CI, sürüm kaydı; sessiz uyumluluk paketi eklememe |
| Kur elle girildiği için eski kalır | Yüksek | Düşük | "Kur X gün önce güncellendi" etiketi, 7 günden eskiyse kart |

## 13. Kaynaklar
Ayrıntılı bağlantılar `docs/09-KAYNAKLAR.md` dosyasındadır.
