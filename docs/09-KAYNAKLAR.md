# 09 - Kaynaklar ve Araştırma Notları (31 Ekim 2026)

12 Kasım 2026 notu: Aşağıdaki bağlantılar önceki araştırma arşividir; bu planlama güncellemesinde internet araştırması veya sürüm doğrulaması yapılmadı. Başlıklardaki tarih/majör sürüm/uyumluluk iddiaları kesin güncel gerçek olarak kullanılmaz. Platform kararı PRD O1 ve ARCHITECTURE bölüm 7'ye göre yeniden değerlendirilecek; güncel resmî Android/Capacitor/TypeScript/Vite belgeleri ve gerçek cihaz kanıtı kuruluma başlamadan kayda alınacak.

Bu liste önceki kararların dayanağıdır. Üçüncü taraf blog/özet sayfaları ikincil kaynaktır; kurulum sırasında resmî dokümanla doğrulanır.

## Tasarım ve "AI slop"
- Neden yapay zekâ tasarımı jenerik görünür (mor gradient, Inter, üç kart): https://www.superdesign.dev/blog/why-ai-design-looks-generic
- Yapay zekâ üretimi tasarımın 10 işareti (emoji ikon, buzlu cam, gradient): https://pub.towardsai.net/10-signs-of-ai-generated-design-and-how-to-make-yours-not-look-like-it-482ff7da81fc
- Jenerik yapay zekâ uygulaması ve gerçek bir tasarımı hedef verme: https://vp0.com/blogs/why-does-my-ai-app-look-generic
- DeSlop (rengi değiştirmek yeni slop; çözüm yazılı tasarım yönü): https://github.com/nate-zz/DeSlop

## Benzer uygulamalar
- Tiimo (renkli zaman çizgisi, suçlamayan ton): https://twit.tv/posts/tech/how-tiimo-makes-task-and-time-management-easier-neurodiverse-users
- Tiimo, Structured, Finch karşılaştırması: https://www.getinflow.io/post/best-alternatives-tiimo-adhd
- Cashew (Material You bütçe uygulaması, açık kaynak): https://converter.brightcoding.dev/blog/jameskokoskacashew-open-source-flutter-budget-tracker-for-all-platforms ; web sürümü: https://budget-track.web.app/

## Teknoloji sürümleri
- Vite 8 (12 Mart 2026): https://github.com/objectstack-ai/framework/pull/924
- TypeScript 7.0 (8 Temmuz 2026, Go tabanlı): https://coddy.tech/docs/typescript/typescript-7 ve https://adtmag.com/articles/2026/07/10/microsoft-bets-typescript-future-on-a-native-compiler.aspx
- React 19.x: https://versionlog.com/react/
- Dexie 4.4.x: https://github.com/dexie/Dexie.js/releases/tag/v4.4.4
- vite-plugin-pwa (Workbox): https://github.com/vite-pwa/vite-plugin-pwa
- Temporal API tarayıcı desteği (Chrome 144, Firefox 139, Safari henüz yok - Mart 2026 kaynağı): https://www.w3schools.com/JS//js_temporal.asp
- Node 25+ ile Corepack'in gelmemesi, pnpm kurulumu, Node 26 LTS tarihi: https://www.issoh.co.jp/md/tech/2765

## Bildirim ve PWA kısıtları
- Notification Triggers API (origin trial tamamlandı, yayına alınmadı): https://developer.chrome.com/docs/web-platform/notification-triggers
- Periodic Background Sync desteği (Chrome/Android, kurulu ve etkileşimi yüksek PWA): https://openpwa.net/compatibility/periodic-background-sync/
- PWA'nın hatırlatıcı bildirimlerde sınırları, Android'de Capacitor ile OS düzeyinde planlama: https://dev.to/nishchaldev/when-notifications-matter-pwas-start-to-show-their-limits-5ebl
- Capacitor Local Notifications (Android 12+ tam zamanlı alarm izni, Android 13 bildirim izni): https://npmjs.com/package/@capacitor/local-notifications

## Yayın
- GitHub Pages: Free planda yalnızca herkese açık depo; özel depo için Pro/Team vb.: https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site ve https://docs.github.com/en/enterprise-server@3.15/pages/getting-started-with-github-pages/what-is-github-pages

## Güvenlik
- OWASP parola saklama (Argon2id tercih; PBKDF2-HMAC-SHA256 için en az 600.000 yineleme): https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html ; özet karşılaştırma: https://mojoauth.com/security-guides/argon2-vs-pbkdf2

## Fiş okuma (v2)
- Geleneksel OCR ve belge VLM'leri fiş kıyası (VLM'ler kendiliğinden daha iyi değil): https://imagetotable.ai/references/traditional-ocr-vs-document-parsing-vlm
- OCR hattı ve VLM tez çalışması (hibrit mimari): https://escholarship.org/uc/item/28j4w273
- e-Arşiv/e-Fatura QR kod içeriği (toplam tutar, tarih vb.): https://www.parasut.com/blog/fatura-ve-belgelerde-karekod-kullanimi-zorunlulugu

## Ajan araçları
- Claude Code kurulum ve hesap şartları (ücretsiz Claude planı dahil değil): https://code.claude.com/docs/en/setup.md
- CLAUDE.md içinde `@AGENTS.md` içe aktarma: https://travis.media/blog/claude-md-import-agents-md
- Codex CLI kurulum ve AGENTS.md: https://github.com/openai/codex ve https://dev.to/owen_fox/how-to-install-codex-cli-the-complete-official-setup-guide-32le

## Doğrulanmamış / dikkat
- Kur ve ücretli plan fiyatları değişebilir; abonelik sayfasından bakın.
- Blog kaynaklarındaki doğruluk yüzdeleri (örn. Tesseract %80-90) kaba göstergedir; karar Faz 13'te kendi fişlerinizle ölçümle verilecek.
- Phosphor ikon adları ve Fontsource paketlerindeki Türkçe harf kapsamı Faz 0'da kurulumda doğrulanır.
