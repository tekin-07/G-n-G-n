@AGENTS.md

## Claude Code'a özel notlar

- Her faza başlarken önce plan modunda çalış: belgeleri oku, planı çıkar, kullanıcı onaylamadan dosya değiştirme.
- Fazlar arasında bağlamı temizle (`/clear`); yeni oturumda yalnızca AGENTS.md + ilgili belgeleri oku. Kalıcı kararlar belgelere yazılır, sohbet hafızasına güvenilmez.
- Faz kapısında, işi yapan oturumdan bağımsız bir gözden geçirme alt ajanı (reviewer) çalıştır: "docs/03-DESIGN.md bölüm 3 ve AGENTS.md bölüm 5'e aykırı bir şey var mı, test eksikliği var mı?" Bulguları kullanıcıya düz dille özetle.
- Uzun komut çıktılarını sohbete yapıştırma; özetle.
- Bir komut kullanıcı verisini veya depoyu geri alınamaz şekilde değiştirecekse (silme, force push, reset --hard) çalıştırmadan önce açıkça onay iste.
