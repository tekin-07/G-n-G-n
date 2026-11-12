# 04 - Mimari ve Teknoloji Seçimleri (Gün Gün)

Durum: onaylı taslak, 31 Ekim 2026. Bu belge "nasıl yapılır" sorusunun tek kaynağıdır. Değişiklik gerekirse önce bu belge güncellenir (bkz. AGENTS.md, "Karar değişikliği").

## 1. Temel karar: "local-first" ve backend sorusu

Gün Gün finans, aile ve rutin verisi tutar. Bu veriyi bir sunucuda saklamak; hesap açma, kimlik doğrulama, güvenlik açığı yönetimi, KVKK sorumluluğu ve işletme maliyeti demektir. Tek kullanıcılı kişisel bir uygulama için bu risk, getirdiği faydadan büyüktür.

Karar: İlk sürümlerde (MVP, v1) veri yalnızca kullanıcının telefonunda durur. Sunucu yoktur. "Profesyonel backend" ihtiyacı, sunucu olmadan da karşılanır: backend'in işlevi olan iş kuralları ve veri erişimi, arayüzden tamamen ayrı iki pakete konur.

| Paket | Görev | Backend'deki karşılığı |
|---|---|---|
| `packages/core` | Saf TypeScript: para, tarih, tekrar kuralları, plan üretimi, taksit/kart/ekstre hesapları, seri (streak), istatistik, doğrulama şemaları, depo arayüzleri (ports) | Servis ve domain katmanı |
| `packages/data` | Dexie (IndexedDB) şeması, depo uygulamaları, migrasyonlar, yedek/şifreleme, günlük işler (otomatik kayıtlar) | Veri erişimi ve veritabanı katmanı |
| `apps/web` | React arayüzü, PWA, ekranlar | Frontend |
| `apps/server` | YALNIZCA v2'de: fiş okuma ve (seçilirse) push bildirim için küçük servis | Gerçek backend |

Bu ayrım sayesinde: (a) hesaplar arayüzden bağımsız test edilir, (b) veri katmanı ileride SQLite/senkronizasyon ile değiştirilebilir, (c) aynı çekirdek sunucuda ve Android uygulamasında yeniden kullanılır.

## 2. Teknoloji tablosu

Sürüm politikası: "en son kararlı sürüm" Faz 0'da kurulur, kilit dosyasıyla sabitlenir, kurulan sürümler `docs/VERSIONS.md` dosyasına yazılır. Aşağıdaki majör sürümler Ekim 2026 araştırmasına dayanır; kurulum sırasında `npm view <paket> version` ile doğrulanır.

| Alan | Seçim | Gerekçe |
|---|---|---|
| Dil | TypeScript, `strict` açık. TypeScript 7 (Go tabanlı derleyici, 8 Temmuz 2026'dan beri kararlı) | Tür güvenliği, ajanın hata yapmasını azaltır. TS API'sine ihtiyaç duyan bir araç bozulursa uyumluluk paketi `@typescript/typescript6` kullanılır |
| Paket yönetimi | pnpm (workspaces) | Katı bağımlılık çözümü, katman ihlallerini erken yakalar. Node 25 ve sonrası Corepack'i artık içermez; kurulum `npm install -g pnpm` ile yapılır |
| Derleme/dev sunucu | Vite 8 (Rolldown tabanlı) + `@vitejs/plugin-react` | Hızlı, PWA eklentisi olgun |
| UI | React 19 | Ajanların en iyi bildiği ekosistem; bileşen/erişilebilirlik kütüphaneleri geniş |
| Yönlendirme | React Router, `createHashRouter` | GitHub Pages'te derin bağlantı 404 sorunu yaşamaz; PWA'da kararlı |
| Stil | Tailwind CSS v4, token'lar CSS değişkeni olarak (`@theme`) | Tutarlı tasarım dili, açık/koyu tema kolay |
| Erişilebilir bileşenler | Radix UI primitive'leri (Dialog, Popover, Tabs, Switch, Toast, ToggleGroup...) | Klavye/ekran okuyucu desteği hazır; görünüm bizim token'larımızla |
| İkonlar | `@phosphor-icons/react` (regular + fill) | Tek set, tutarlı çizgi; yumuşak karakter. Emoji yasak |
| Formlar | react-hook-form + zod (`@hookform/resolvers`) | Şemalar `core`'dan gelir, tek doğrulama kaynağı |
| UI durumu | Zustand (sadece geçici UI durumu) | Küçük ve basit |
| Veri | Dexie 4 + `dexie-react-hooks` (`useLiveQuery`) | IndexedDB için olgun sarmalayıcı; canlı sorgular arayüzü otomatik günceller |
| Doğrulama | Zod 4 | Yedek içe aktarma ve form doğrulaması |
| Tarih/saat | Temporal API; `temporal-polyfill` yalnızca `core/time` içinde | Chrome 144 ve Firefox 139'dan beri yerel destek var, Safari henüz yok. Polyfill ile her ortamda (Node testleri dahil) aynı davranış |
| Kimlik | ULID (`ulid`) | Zaman sıralı, ileride senkronizasyona uygun |
| PWA | `vite-plugin-pwa` (Workbox), `registerType: 'prompt'` | Çevrimdışı çalışma; güncelleme kullanıcıya sorulur, yarım form kaybolmaz |
| Grafik (v1) | Özel SVG bileşenleri + `d3-scale`/`d3-shape` | Hazır grafik kütüphanelerinin "şablon" görünümünden kaçınır, küçük |
| Test | Vitest, Testing Library, `fake-indexeddb`, `fast-check`, Playwright, `@axe-core/playwright` | Katmanlara göre bölüm 11'de |
| Lint/format | Biome | Tek araç, hızlı, TS 7 ile sorunsuz (TS API'sine bağımlı değil) |
| Mimari denetim | `dependency-cruiser` | Katman kurallarını CI'da zorlar |
| CI/CD | GitHub Actions, GitHub Pages | Ücretsiz; Free planda Pages için depo herkese açık olmalıdır (bkz. bölüm 9) |
| Şifreleme | Web Crypto: AES-256-GCM + PBKDF2-HMAC-SHA256 (en az 600.000 yineleme) | Ek bağımlılık yok; OWASP PBKDF2 için bu eşiği önerir. Üstü Argon2id'dir, başlıkta `kdf` alanı olduğu için ileride yükseltilir |

Yeni bağımlılık eklemek için izin gerekir (AGENTS.md). Yukarıdaki liste "izinli bağımlılıklar" listesidir.

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
        schedule/    expandOccurrences, autoplan, conflicts, weekChoice
        streak/      gün değerlendirme, joker, seri
        finance/     balances, netWorth, installments, cardStatements, fixedExpenses, savings, budget
        stats/       günlük/hafta sonu/aylık özetler
        reminders/   ics üretimi
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
        app/         main.tsx, router.tsx, providers.tsx, composition.ts, registry.ts, theme/
        components/ui/   Button, Sheet, Chip, Card, Field, Money, EmptyState ... (paylaşılan)
        features/
          onboarding/  today/  plan/  goals/  money/  stats/  settings/  backup/
            (her biri: components/, hooks/, routes.tsx, index.ts, todayCards.ts?)
        i18n/        tr.ts (tüm arayüz metinleri)
        lib/         küçük yardımcılar
        pwa/         kayıt, güncelleme bildirimi, kurulum yardımı
      e2e/           Playwright
    server/          (v2'ye kadar boş; yalnızca README)
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
1. Zamanı gelmiş otomatik kayıtlar: maaş/gelir kaynakları, `autoPost=true` sabit giderler, ekstre tarihi geçmiş taksit satırları (planned -> posted). Uygulama günlerce açılmadıysa kaçan aylar tarihleriyle telafi edilir.
2. Haftalık net değer anlık görüntüsü (`snapshots`).
3. Süresi geçmiş geçici durumlar için temizlik (silinenler > 30 gün).
4. Uyarı kartlarının girdileri hesaplanır (kaydedilmez, ekranda anlık üretilir): sabit gider yaklaşıyor, ekstre son ödeme günü, kur eski, yedek zamanı.

### Eklenti benzeri genişleme (modülerlik)
Her özellik klasörü bir `FeatureModule` dışa aktarır:
```ts
interface FeatureModule {
  id: string;
  routes: RouteObject[];
  nav?: { label: string; icon: IconKey; order: number };
  todayCards?: (ctx: TodayContext) => TodayCard[];   // Bugün ekranına kart ekler
  settingsSections?: SettingsSection[];
  backupTables?: string[];                           // yedeğe dahil tablolar
}
```
`app/registry.ts` modülleri toplar. Yeni bir özellik (örn. "Su içme takibi") eklemek: yeni klasör + registry'ye bir satır; Bugün, Ayarlar veya yedek kodu elle değiştirilmez.
Başka genişleme noktaları: yeni kategori (veri), yeni düzenli aktivite (veri), yeni hesap türü/varlık (`core/money/assets.ts` içine bir `AssetDef` + test), yeni ikon (`iconRegistry.ts` içine bir satır).

## 6. PWA ve çevrimdışı

- Manifest: `name` "Gün Gün", `short_name` "Gün Gün", `lang` "tr", `display` "standalone", `start_url` ve `scope` Vite `base` ile uyumlu göreli (`./`), simgeler 192 ve 512 (maskable dahil), `theme_color`/`background_color` açık temadan. `shortcuts`: "Gider ekle" ve "Bugün" (Android uzun basma kısayolu).
- Service worker: uygulama kabuğu ön belleğe alınır (precache), yazı tipleri dahil. Dış istek yok. `registerType: 'prompt'`: yeni sürüm hazır olunca "Yeni sürüm hazır, yenile" bildirimi; kullanıcı onaylamadan sayfa yenilenmez.
- Vite `base`: GitHub Pages proje sitesi için `/<depo-adı>/`; `VITE_BASE` ortam değişkeniyle ayarlanır.
- Kurulum yardımı: `beforeinstallprompt` yakalanır; karşılamada ve Ayarlar'da "Ana ekrana ekle" adımları gösterilir.
- Kalıcı depolama: ilk anlamlı veri girildikten sonra `navigator.storage.persist()` istenir; sonuç ve `estimate()` kullanımı Ayarlar > Depolama'da gösterilir. Reddedilirse ekranda açık uyarı ve yedek önerisi çıkar.
- Çevrimdışı test zorunludur (Playwright `setOffline`): uygulama açılır, tüm MVP işlevleri çalışır.

## 7. Bildirim ve hatırlatma stratejisi (kritik kısıt)

Araştırma sonucu: web uygulamasının, sunucusuz ve uygulama kapalıyken kesin saatte yerel bildirim planlaması güvenilir değildir. Chrome'un Notification Triggers API'si (TimestampTrigger) deneme (origin trial) aşamasını tamamladı ama yayına alınmadı; Periodic Background Sync yalnızca Chrome/Android'de, kurulu ve etkileşimi yeterli PWA'lar için ve kaba aralıklarla çalışır. Bu yüzden saatli bildirim vaat edilmez.

| Sürüm | Yöntem | Güvenilirlik |
|---|---|---|
| MVP | (1) Bugün ekranında kartlar (15:00 ek iş sorusu, 21:30 hedef notu). (2) "Takvime aktar": tüm düzenli hatırlatmaları tekrarlı `.ics` olarak dışa aktarır (VEVENT + RRULE + VALARM, `Europe/Istanbul` sabit +03:00 VTIMEZONE); Google Takvim/Android bildirimi bunları güvenilir çalıştırır. (3) Uygulama açıkken yerel `Notification` | Kartlar: uygulama açılınca. Takvim bildirimi: güvenilir |
| v2, seçenek A (önerilen) | Capacitor ile Android sarmalayıcı + `@capacitor/local-notifications`: sunucusuz, veri telefonda kalır, gerçek zamanlı yerel bildirim. Android 12+ için tam zamanlı alarm izni (`SCHEDULE_EXACT_ALARM`), Android 13+ için bildirim izni gerekir. APK GitHub Releases'ten indirilebilir | Yüksek |
| v2, seçenek B | Web Push + küçük sunucu (VAPID). Sunucu yalnızca abonelik ve saat bilir, finans verisi bilmez | Orta (ağ/Doze gecikmesi) |

Karar Faz 12'de, MVP kullanım deneyimine göre verilir. Her iki seçenekte de zamanlama mantığı `core/reminders` içindedir (tek kaynak; `.ics`, Capacitor ve push aynı kuralı kullanır).

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
Geri yükleme: parola ile çöz -> Zod ile doğrula -> `schemaVersion` eski ise migrasyonla yükselt -> "mevcut verinin üzerine yaz" veya "birleştir" onayı -> tek transaction. Başarısız olursa mevcut veri değişmez. Geri yükleme öncesi otomatik "kurtarma yedeği" (şifresiz, yalnızca bellekte değil, indirilebilir) önerilir.

## 9. Yayın

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

- `core`: birim + özellik tabanlı (`fast-check`) testler. Para, taksit, tekrar kuralı, seri, kart ekstresi için kapsam >= %90 satır. Test vektörleri 05-DATA-MODEL.md'dedir.
- `data`: `fake-indexeddb` ile depo, migrasyon (eski sürüm fixture -> yeni), yedek gidiş-dönüş testleri.
- `web`: Testing Library ile form ve kart bileşenleri; Playwright (Pixel 7 emülasyonu) ile ana akışlar, çevrimdışı senaryo, `axe` erişilebilirlik taraması.
- Her görev "kanıtla bitti" kuralına bağlıdır: test çıktısı olmadan tamamlandı denmez.

## 12. Risk kaydı

| Risk | Olasılık | Etki | Önlem |
|---|---|---|---|
| Kullanıcı yedek almaz, veri kaybolur | Yüksek | Çok yüksek | Haftalık hatırlatma, Bugün'de kart, ilk 3 haftada daha sık hatırlatma, kolay tek dokunuşla yedek |
| Saatli bildirim beklentisi karşılanmaz | Yüksek | Orta | Baştan dürüst söylemek, `.ics`, v2 kararı |
| Kapsam şişmesi | Yüksek | Yüksek | Faz kapıları; her fazda yalnızca o fazın işi; ROADMAP dışına çıkmak yasak |
| Ajan "ortalama" (slop) tasarım üretir | Orta | Orta | 03-DESIGN.md yasak listesi, token'lar, ekran görüntüsüyle öz-eleştiri adımı |
| Yeni majör sürüm uyumsuzluğu (Vite 8, TS 7 yeni) | Orta | Düşük | Kilit dosyası, CI, sürüm kaydı; sorun çıkarsa bir önceki majör sürüme dönülür ve not düşülür |
| Kur elle girildiği için eski kalır | Yüksek | Düşük | "Kur X gün önce güncellendi" etiketi, 7 günden eskiyse kart |

## 13. Kaynaklar
Ayrıntılı bağlantılar `docs/09-KAYNAKLAR.md` dosyasındadır.
