# Gün Gün - Başla Buradan (yazılım bilmeden kurulum rehberi)

Bu paket, Gün Gün uygulamasını bir yapay zekâ kodlama aracıyla (Claude Code veya Codex) adım adım yaptırman için hazırlandı. Kod yazmayacaksın; ajana işi verecek, her aşamada telefonda sonucu deneyip onaylayacaksın.

## 1. Pakette ne var

| Dosya | Ne işe yarar |
|---|---|
| `AGENTS.md` | Ajanın uyacağı kurallar (ajan her oturumda okur) |
| `CLAUDE.md` | Claude Code için küçük ek (AGENTS.md'yi içine alır) |
| `docs/01-PRD.md` | Ne yapılacak (özellikler, kurallar, varsayımlar) |
| `docs/02-SCREENS.md` | Ekranlar ve akışlar |
| `docs/03-DESIGN.md` | Renkler, yazı, "yapay zekâ şablonu görünümüne karşı" kurallar |
| `docs/04-ARCHITECTURE.md` | Teknolojiler ve yapı |
| `docs/05-DATA-MODEL.md` | Veri ve hesap kuralları (taksit, ekstre, seri...) |
| `docs/06-ROADMAP.md` | Faz faz yapılacaklar ve kabul ölçütleri |
| `docs/07-TEST-PLAN.md` | Test ve gerçek telefon kontrol listesi |
| `docs/08-MASTER-PROMPT.md` | Ajana yapıştıracağın tek parça ana prompt |
| `docs/09-KAYNAKLAR.md` | Araştırma kaynakları |
| `docs/PROFIL.local.md` | Sana ait kişisel değerler. GitHub'a GİRMEZ |
| `.gitignore` | Kişisel dosyaların yanlışlıkla yüklenmesini engeller |

Okuman gereken: yalnızca bu dosya ve `docs/01-PRD.md` bölüm 10 (varsayımlar). Gerisini ajan okur; merak edersen bakarsın.

## 2. Ön şartlar

1. Bir bilgisayar (Windows veya Mac). Ajan araçları telefonda değil, bilgisayarda çalışır.
2. Bir ajan aboneliği. İkisinden biri yeter:
   - Claude Code: ücretsiz Claude planı Claude Code'u içermez. Pro, Max, Team, Enterprise veya Anthropic Console hesabı gerekir.
   - Codex: ChatGPT planlarından (Plus, Pro, Business, Edu, Enterprise) biri veya OpenAI API anahtarı. Windows'ta Codex'in WSL ile kullanılması öneriliyor; Windows desteği deneysel olarak anılıyor.
   Kararsızsan Claude Code ile başla; bu paket ikisi için de hazır.
3. Ücretsiz bir GitHub hesabı (github.com).
4. Android telefon ve Chrome.

## 3. Bilgisayara kurulacaklar (bir kerelik)

1. Git. Windows: git-scm.com/download/win (Claude Code Windows'ta Git for Windows ister). Mac: Terminal'de `git --version` yaz, yoksa kurmayı önerir.
2. Node.js (LTS sürümü): nodejs.org. Uygulamanın geliştirme araçları için gerekir.
3. pnpm: Terminal veya PowerShell'de:
   ```
   npm install -g pnpm
   ```
   (Node 25 ve sonrası artık Corepack'i içermiyor, bu yüzden pnpm'i böyle kuruyoruz.)
4. Ajan aracı:
   - Claude Code, Windows PowerShell:
     ```
     irm https://claude.ai/install.ps1 | iex
     ```
     Mac/Linux:
     ```
     curl -fsSL https://claude.ai/install.sh | bash
     ```
     Kurulumdan sonra `claude doctor` ile kontrol edebilirsin. Güncel adımlar: code.claude.com/docs/en/setup
   - Codex: `npm install -g @openai/codex` (Node 22+ gerekir), sonra `codex` yaz ve "Sign in with ChatGPT" seç. Güncel adımlar: github.com/openai/codex
Takılırsan hata mesajını olduğu gibi bir yapay zekâ sohbetine yapıştır; çoğu kurulum hatası kısa bir düzeltmedir.

## 4. Proje klasörünü hazırla

1. Bu paketin içeriğini bilgisayarında yeni bir klasöre aç: örneğin `Belgelerim\gun-gun`. (Zip'ten çıkar; `AGENTS.md`, `docs`, `.gitignore` bu klasörün içinde olsun.)
2. O klasörde terminal aç. Windows: klasörü Dosya Gezgini'nde aç, üstteki adres çubuğuna `powershell` yazıp Enter. Mac: klasöre sağ tık, "Klasörde Yeni Terminal".
3. Git'i başlat ve KORUMAYI KONTROL ET (ilk commit'ten önce!):
   ```
   git init
   git check-ignore -v docs/PROFIL.local.md
   ```
   İkinci komut bir satır yazdırmalı (örneğin `.gitignore:2:docs/PROFIL.local.md ...`). Hiçbir şey yazdırmazsa DUR, depoya yüklemeyin; ajana "PROFIL.local.md korunmuyor, düzelt" de.
4. İlk kayıt:
   ```
   git add .
   git commit -m "docs: ilk belgeler"
   git branch -M main
   ```

## 5. GitHub deposu

1. github.com'da yeni depo oluştur: adı `gun-gun`, görünürlük Public. (Ücretsiz GitHub planında Pages yalnızca herkese açık depodan yayın yapar. Depoda kişisel verin olmayacak: maaş, oğlunun yaşı vb. yalnızca senin bilgisayarındaki `PROFIL.local.md`'de.)
2. Depo sayfasının gösterdiği iki komutu çalıştır (kullanıcı adını kendininkiyle değiştir):
   ```
   git remote add origin https://github.com/KULLANICI_ADIN/gun-gun.git
   git push -u origin main
   ```
   Tarayıcıda giriş isteyebilir; onayla.
3. Depo ayarlarında (Settings > Pages) kaynak olarak "GitHub Actions" seçilecek; bunu Faz 0'da ajan adım adım söyler.

## 6. Ajanı başlat

1. Proje klasöründeki terminalde `claude` (veya `codex`) yaz.
2. `docs/08-MASTER-PROMPT.md` dosyasını aç, kutunun içindeki metni kopyala, ajana yapıştır, Enter.
3. Ajan belgeleri okuyup Faz 0 için plan sunar. Planı oku: anlamadığın yeri sor ("bunu sade anlat"). Uygunsa "onaylıyorum, başla" de.

## 7. Fazlarla ilerleme: senin işin ne?

Her faz: ajan yapar -> sana "nasıl kontrol edersin" der -> sen telefonda dener -> "onayla" veya "şunu beğenmedim" dersin.
- Faz 0: boş ama kurulabilir uygulama. Telefonda deneyeceksin (aşağıda).
- Faz 1: karşılama ve ayarlar. Doldur, uygulamayı kapat-aç, bilgilerin duruyor mu?
- Faz 2: plan ve Bugün. Haftanın planı sana doğru mu geliyor?
- Faz 3: hedefler. Akşam notu rahat mı?
- Faz 4-5: para, kart, taksit. Gerçek bir alışverişi gir, rakamlar doğru mu?
- Faz 6: yedek. Yedek al, sil, geri yükle.
- Faz 7: MVP bitişi; bir hafta gerçekten kullan.
Her yeni faza yeni bir ajan oturumu aç (kapat, yeniden `claude`) ve kısa başlatma prompt'unu kullan (`docs/08-MASTER-PROMPT.md` sonunda).
Ekran görüntülerini ajan sana gösterecek. Beğenmediğin her şeyi çekinmeden söyle; "mor/şablon" gibi görünen bir şey varsa özellikle söyle.

## 8. Telefonda deneme

1. Faz 0 sonunda ajan sana bir adres verir: `https://KULLANICI_ADIN.github.io/gun-gun/`
2. Telefonda Chrome'la aç. Menü (üç nokta) > "Uygulamayı yükle" veya "Ana ekrana ekle".
3. Ana ekrandaki simgeden aç. Uçak modunu açıp tekrar dene: çalışmalı.
Not: Kurulabilir (PWA) deneme için adres https olmalı; bu yüzden bilgisayardaki geliştirme sunucusu yerine yayındaki adresi kullanırız.

## 9. Altın kurallar

1. Yedek al. Verin yalnızca telefonda. Chrome verilerini silersen veya uygulamayı kaldırırsan veri gider. Yedek dosyasını (`.gungun`) Drive'a/başka yere kaydet. Yedek parolasını unutursan yedek açılamaz; parolayı bir yere yaz.
2. Parola, API anahtarı, banka bilgisi asla ajana veya GitHub'a yapıştırma.
3. `docs/PROFIL.local.md` GitHub'a girmez; ajan da değerlerini koda koymaz. Bunu bozmasını fark edersen hemen söyle.
4. Ajan "bitti" derse kanıt iste: "pnpm check sonucunu ve test özetini göster".
5. Ajana büyük, belirsiz istek verme. Küçük ve net iste. Yol haritasında olmayan bir fikrin olursa "yol haritasına ekleyelim mi?" diye ona sor.
6. Her faz etiketlidir. Bozulursa: "Son çalışan faz etiketine dön" de (`docs/08-MASTER-PROMPT.md`).

## 10. Sık sorulanlar

Neden sunucu/backend yok? Maaş, harcama, borç gibi hassas veriyi bir sunucuya koymak güvenlik ve sorumluluk yükü getirir. Gün Gün'de "backend işi" telefonun içinde, ayrı ve testli iki pakette (core + data) yapılır; sunucu yalnızca ileride gerçekten gerekirse (fiş okuma, bildirim) eklenir. Yani yapı profesyonel ve genişlemeye açık, ama verin sende.

Saat gelince bildirim gelir mi? İlk sürümde güvenilir saatli bildirim vaat edilmez; web uygulamaları kapalıyken kesin saatte bildirimi güvenilir yapamıyor. Bunun yerine Bugün'de kartlar var ve "Takvime aktar" ile hatırlatmaları telefonun takvimine tekrarlı ekleyebilirsin. Gerçek bildirim v2'de (Android uygulaması olarak sarmak önerilen yol).

Fişten otomatik okuma? v2. MVP'de fiş fotoğrafını kayda ekleyebilirsin.

Depo herkese açık olunca kodumu herkes görür mü? Evet, kodu görür; verini göremez, çünkü veri depoda değil telefonunda. İleride özel depo istersen yayın için başka bir servis gerekir.

Bu paketteki varsayımlar yanlışsa? `docs/01-PRD.md` bölüm 10'daki tabloyu oku; değiştirmek istediğini ajana söyle, o belgeyi günceller.

Ne kadar sürer? Süre sözü vermiyoruz; fazlar küçük tutuldu. Her faz sonunda elinde çalışan bir şey olur.
