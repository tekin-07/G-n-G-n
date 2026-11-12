# 07 - Test ve Kalite Planı

12 Kasım 2026 güncellemesi. Aşağıdakiler gelecekteki uygulamanın kabul şartlarıdır; bu oturumda çalıştırılmış test sonuçları değildir. Yalnız belge işinde kaynak tutarlılığı ve bağlantılar kontrol edilir.

## 1. Katmanlar
- core: Vitest/fast-check; money, finance, schedule, progress/tamamlama için >=%90 satır kapsam hedefi. Tarih/saat enjekte edilir.
- data: fake-indexeddb; depolar, atomik işlem, idempotence, soft-delete/geri alma, migrasyon fixture'ları ve yedek.
- UI: Testing Library; ana formlar, hızlı kayıt, taslak, bildirim izni ve eksik bilgi durumları.
- Web: Playwright Pixel 7; A/B akışları, çevrimdışı, iki tema, axe ciddi/kritik 0. Statik: Biome/tsc/dependency-cruiser, emoji ve bileşende sabit hex taraması.
- Android: gerçek cihaz; bildirim teslimi/iptal/eylem ve OS yaşam döngüsü. Web testi veya mock adaptör, OS teslim kanıtının yerine geçmez.

## 2. Değişmezler
- Taksit toplamı tam tutar; sayı n; hepsi pozitif; kalan yalnız sonda ve n kuruştan küçük.
- Değerleme BigInt, safe integer, sıfır kur reddi, negatif bakiye için simetrik yuvarlama. Eksik kur sıfır değer değildir.
- Tekrar sıralı/tekrarsız; gün aşırı plan hafta/ay/yıl sınırında döngüsünü korur. Haftalık sıklık N benzersiz oluşumdur; onaysız gün ataması yok.
- Oluşum taşıma kimliği korur; tamamlama, hedef ilerlemesi ve bildirim eylemi çift kayıt üretmez. Tekrar kuralı değişikliği geçmişi yeniden yazmaz.
- Çakışma simetrik, kendisiyle yok; izinli mesai-ek iş iç içeliği hatalı çakışma değil. Bilinmeyen süreyle kesin kapasite iddiası yok.
- Günlük işler tekrar çalışınca yeni kopya yok; fatura dönemi unique. Vade/talimat otomatik ödeme kaydı üretmez.
- Birikim planı gerçekleşen transfer değildir. Kart ödemesi/transfer gider değildir. Fatura dönemi ve bağlı ödemesi çift gider/yükümlülük sayılmaz.
- Restore(backup(db)) aktif kullanıcı verisini, bağlantıları ve isteğe bağlı fişleri korur. OS alarm/izinleri yeni cihazda yeniden değerlendirilir.

## 3. Korunan finans vektörleri
- 1.000 TL /6 → 5×166,66 +166,70; 12.999 TL/6 → 6×2.166,50.
- 125,5 USDT ×41,35 → 5.189,43 TL; 3,250 g ×4.850,25 → 15.763,31 TL.
- Kesim S=20, vade D=5: 20 Mart →20 Mart/5 Nisan; 21 Mart →20 Nisan/5 Mayıs. S=10,D=25: 10 Mart →10 Mart/25 Mart. S=31 Şubat'ta 28/29'a sıkışır.
- Maaş 15'inde, üç ay açılmadı → eksik üç dönem kaydı; tekrar açınca kopya yok. Varsayılan otomatik gelir banka doğrulaması değildir; düzeltme geçmişi tutarlı değiştirir.
- Fatura vektörü (uydurma): 900 TL kesin fatura, 300 TL kayıtlı ödeme →600 TL kalan, kısmi. 600 TL daha →ödendi; aynı eylem tekrar gönderilirse yeni gider yok. Ödeme geri alınırsa kalan ve uyarı döner.
- Tutarı bilinmeyen fatura →0 TL diye görünmez, kullanılabilir para eksik bilgi uyarısı taşır. Tahmini tutara ödeme kaydı gelmesi kesin tam ödendi çıkarımı yapmaz.
- Tahmini kullanılabilir para, kart iadesi/kısmi ödeme dağıtımı için O4 kararı sonrası örnekler eklenmeden Faz 3/5 ilgili kapısı kapanmaz; burada formül uydurulmaz.

## 4. Kabul akışları A1–A10 (güncel anlamları)
A1 Boş veritabanı →kısa onboarding/atla →ilk esnek iş; eksik finans/uyku uydurulmaz, taslak kapat/açta kalır.
A2 Ek iş 08:00–17:00 penceresinde; farklı günlerde 10:00 ve 15:00'te tamamlanır →tek log, hedefe yansıma, sabit saat gecikmesi yok.
A3 Ekle>Gider>125>Market>Kaydet →posted gider, kategori ve aylık toplam +125; gezinme/onay adımı <=4, sayı tuşları bu ölçüme dahil değil.
A4 Kasım başlangıç 12.999 TL/6 taksit →altı satır, aylık 2.166,50; yansımak ve ödenmek farklı. İşlem tarihi ilerledikçe uygun satırlar posted olur.
A5 Maaş telafisi →birikim planı; “Sonra aktarırım” gerçekleşmiş birikim değildir; transfer onaylanınca hesaplar ve birikim aynı kaynaktan güncellenir.
A6 Yetişmeyen iş →başka güne taşı/bu haftadan çıkar/duraklat; otomatik ertesi güne yığma, zorunlu %60/joker/seri sıfırlama yok; geçmiş tamamlamalar korunur.
A7 Şifreli yedek →yalnız test verisiyle sil/onay →geri yükle; tablo içerikleri ve ilişkiler/finans toplamları aynı. Yanlış parola/bozuk dosyada mevcut veri aynı.
A8 Kesinleşmiş boks saati başka randevuyla çakışır →öneri; onay öncesi takvim değişmez. Bilinmeyen boks günü/tarihi kendi kendine üretilmez.
A9 İlk kurulum/önbellekten sonra çevrimdışı ana akışlar; uygulama dosyaları hariç dış servis trafiği 0; yerel veri/eylem devam eder. OS bildirim ayrıca gerçek cihazda doğrulanır.
A10 Görüşme tercihleri →haftalık birden çok kesin randevu; öğle görüşmesi üretilmez, değişiklik eski alarmı iptal eder. Tercih kesin randevu gibi görünmez.

## 5. Yeni bütünleşik akışlar
B1 Fitness iki gün aralığı →hafta/ay sınırında doğru çalışma/dinlenme. O2 seçildikten sonra tek oluşum/seri kaydırma önizlemesi ve geçmiş koruma testi.
B2 Haftalık taslak →kesin işler/hedefler/esnek atamalar/boşluk →onay. Dolu/eksik süreli haftada sessiz taşma veya kesin kapasite iddiası yok.
B3 “Salonu ara” hızlı kayıt →göreve dönüştür; tekrar tıklama ikinci görev üretmez. Tarihsiz fikir gecikmiş görünmez.
B4 Dil hedefi +haftada üç oturum →bir oturum tamamla →hedefte bir ilerleme; geri al →ilerleme geri döner. Not/süre/puan zorunlu değil. Duraklatma geçmişi silmez.
B5 Fatura bilinmiyor →tahmin →kesin; talimat var olsa da ödeme bekler. Gerçek ödeme tek harcama oluşturur. Kısmi ödeme, düzeltme, kalan ve uyarı tutarlı.
B6 Tiyatro fikri →bilet hazırlığı →kesin etkinlik →gider formuna hazır tarih/not/tahmin →onayla kaydet. Tahmini harcama kendiliğinden posted gider olmaz; ikinci basış kopya üretmez.
B7 Birikim transferi ve kart nakit çıkışı hesaplarda/özetlerde çift düşülmez. Eksik kur/fatura/vade uygun bilgi gösterir; formül O4'e göre doğrulanır.
B8 Tamamla/ertele eylemi uygulamadan ve bildirimden tekrar gelir →tek sonuç; eski/silinmiş/duraklatılmış kayıt eylemi güvenle reddedilir. Eksik ödeme alanı form açar.
B9 Uygulama aylarca açılmaz →fatura/gelir/taksit telafisi tekrar üretmez; fatura kendiliğinden ödenmez. Bildirim kuyruğunun ufuk sınırı saklanmaz, açıklanır.
B10 Yeni sürüm/geri yükleme →yarım form korunur veya açık onay alınır; ilişkiler ve plan kimlikleri korunur, OS planlaması uzlaştırılır.

## 6. Sıfır veri kaybı kontrolleri
Transaction ortasında hata: taksit/ödeme/yakalama dönüşümü/restore yarım kalmaz. Eski fixture yükselir; silme geri alınır; 30 gün temizlik bağlantıları bozmaz. Yedek aktif veriyi içerir, silinmişleri canlandırmaz. Birleştirme O5 onayı/testi olmadan UI'da sunulmaz. İndirme başarısı/parola doğrulaması olmadan son yedek durumu yanıltılmaz. Şifresiz kurtarma dosyası otomatik oluşturulmaz. Domain işlemi başarılı, OS alarmı başarısız olduğunda yeniden uzlaştırma ve görünür durum vardır.

## 7. Gerçek Android kontrolü
- [ ] Seçilen dağıtım yolu ile kurulum/güncelleme; veri kalıcılığı ve sürüm görünümü.
- [ ] Kapalı uygulama, kilit ekranı, çevrimdışı, yeniden başlatma, pil kısıtı: planlanan/gerçek teslimi kaydet; ölçülmeyen koşula garanti verme.
- [ ] İzin reddi/geri alma, sessiz saat, kategori kapatma, kilit ekranı ayrıntısı; hatalı izin durumunu UI anlatır.
- [ ] Bildirim tamamla/ertele, uygulama içi taşı/öde/sil →eski alarm yok, çift eylem yok. Kuvvetle durdurma gibi OS sınırları açıklanır.
- [ ] Geri tuşu, klavye, sayı girişi, iki tema, %130 yazı, TalkBack, 48 dp.
- [ ] Yedek paylaş/indir/geri yükle; kamera/galeri ve küçültme; yalnız uydurma veride kayıp/restore deneyi, gerçek veriyle kasıtlı tarayıcı temizliği yok.
- [ ] Uçak modunda plan/para; güncelleme yarım formu kaybettirmez; web/native verisinin otomatik ortak olduğu varsayılmaz.

## 8. Bitti tanımı ve hata ayıklama
İlgili kabul ölçütleri, gerçek çıktılar ve kaynak belgeler güncel; check yeşil; UI/OS varsa kendi kanıtı; kullanıcıya kısa sonuç ve sınırlar. Hata önce yeniden üretilir, en küçük düzeltme yapılır, anlamlı regresyon kontrolü bırakılır. Planlanan test, geçmiş test veya sıfır test koşusu geçmiş başarı değildir.
