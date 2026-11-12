# 05 - Veri Modeli ve Hesap Kuralları (Gün Gün)

12 Kasım 2026 güncellemesi. Henüz uygulama/veritabanı yoktur; aşağıdaki yapı planlama şemasıdır, çalıştırılmış migrasyon değildir. PRD O1–O7 açık kararlarına bağlı alanlar ilgili fazın tasarımında tamamlanır.

Bu belge veri yapısının ve tüm hesapların tek kaynağıdır. `packages/core/src/schemas` bu belgenin Zod karşılığıdır. Şema değişirse: bu belge + Zod şeması + Dexie migrasyonu + migrasyon testi birlikte güncellenir. Yeni tablolardaki alanlar, ekran taslağı ve doğrulama kurallarıyla birlikte Faz 1/ilgili fazda tamamlanır; bu dokümandaki TypeScript gösterimi çalıştırılabilir tam Zod şeması değildir.

## 1. Temel türler ve kurallar

```ts
type Id = string;            // ULID
type LocalDate  = string;    // "2026-10-31"
type LocalTime  = string;    // "20:00"
type YearMonth  = string;    // "2026-10"
type WeekKey    = string;    // ISO hafta: "2026-W44" (hafta Pazartesi başlar)
type Weekday    = 1|2|3|4|5|6|7;   // 1 = Pazartesi ... 7 = Pazar
type Minor      = number;    // tam sayı (Number.isSafeInteger); varlığın en küçük birimi
type EpochMs    = number;    // UTC anlık zaman

interface Base { id: Id; createdAt: EpochMs; updatedAt: EpochMs; deletedAt?: EpochMs }
```

Para kuralları:
- Varlıklar ve ölçekleri: `TRY` ölçek 2 (kuruş), `USDT` ölçek 6, `XAU_G` (gram altın) ölçek 3 (miligram). Yeni varlık = `core/money/assets.ts` içine bir satır + test.
- Tutarlar daima pozitif saklanır; yön işlem türünden gelir.
- TL karşılığı hesabı (`valueInTRY`): `balanceMinor * tryPerUnitKurus / 10^scale`, `BigInt` ile, sonuç yarım yukarı yuvarlanır (half-up).
- Kur: `tryPerUnitKurus` = 1 tam birimin kuruş cinsinden değeri (1 USDT = 41,35 TL -> 4135).
- Biçimlendirme: `tr-TR` (`1.234,56 TL`), tablo hizası için tabular rakam.

Soft delete: tüm kullanıcı varlıklarında `deletedAt`. Depolar varsayılan olarak silinmişleri döndürmez.

## 2. Tablolar (Dexie şeması)

```ts
db.version(1).stores({
  settings:         'id',
  categories:       'id, kind, archived, sortOrder',
  activities:       'id, kind, categoryId, goalId, parentActivityId, systemKey, archived',
  activityLogs:     'id, &occurrenceKey, date, activityId',
  occurrenceOverrides: 'occurrenceKey, activityId, scheduledDate',
  captures:         'id, status, createdAt',
  weekPlans:        'weekKey',
  meetingPreferences: 'id',
  reminderIntents:  'id, entityId, status',
  goals:            'id, categoryId, status, type',
  goalLogs:         'id, goalId, date',
  accounts:         'id, assetCode, archived, sortOrder',
  rates:            'id, assetCode, at',
  transactions:     'id, date, type, status, accountId, cardId, categoryId, [date+type], installmentPlanId, fixedExpenseId, billId, incomeSourceId, &autoKey, &commandId',
  incomeSources:    'id, kind, active',
  fixedExpenses:    'id, active, categoryId',
  bills:            'id, &periodKey, fixedExpenseId, dueDate',
  creditCards:      'id, archived',
  installmentPlans: 'id, cardId, startMonth',
  savingsMonths:    'month',
  budgets:          'id, month, categoryId',     // v1
  receipts:         'id, transactionId',
  snapshots:        'date',
  meta:             'key',
});
```
`&autoKey` benzersiz ve seyrektir (yalnızca otomatik kayıtlarda dolu). Örnek: `inc:<kaynakId>:2026-11`, `ins:<planId>:3`. Fatura dönemindeki `fix:<id>:2026-11` transactions.autoKey değil bills.periodKey alanındadır. Manuel ödeme/dönüşüm tekrarları için ayrıca benzersiz commandId kullanılır.

## 3. Varlıklar

### settings (tek satır, `id: 'main'`)
```ts
interface Settings {
  id: 'main';
  displayName?: string;
  theme: 'light' | 'dark' | 'system';
  timezone: string;                    // "Europe/Istanbul"
  weekStartsOn: 1;
  wakeWeekday?: LocalTime;
  wakeWeekend?: LocalTime;
  bedtime?: LocalTime;                // henüz kesinleşmedi
  workStart: LocalTime; workEnd: LocalTime; // 08:00–17:00
  eveningAvailableFrom?: LocalTime;   // kullanıcı tercihi, yaklaşık 18:30
  sideJobTargetMin?: number;          // esnek süre hedefi; sabit saat değildir
  notificationPreferences: {
    enabledKinds: string[];
    quietHours?: { from: LocalTime; to: LocalTime };
    detailOnLockScreen: 'minimal' | 'full';
  };
  // İzin/OS durumu anlık platform gözlemidir, yedekten hazır kabul edilmez.

  savingsMonthlyTargetMinor?: Minor;   // aylık sabit birikim tutarı (TRY kuruş)
  fixedExpenseWarnDays: number;        // 3
  rateStaleDays: number;               // 7
  backupReminderDays: number;          // 7
  onboardingDone: boolean;
  lastBackupAt?: EpochMs;
  persistGranted?: boolean;
  lock?: { enabled: boolean; pinHash?: string; salt?: string; iter?: number; biometric?: boolean }; // v1
  appSchemaVersion: number;
}
```

### categories
```ts
interface Category extends Base {
  kind: 'activity' | 'goal' | 'income' | 'expense' | 'fixed';
  name: string; iconKey: IconKey; colorKey: ColorKey;   // emoji YOK
  sortOrder: number; archived: boolean; isSystem: boolean;
}
type ColorKey = 'turkuaz'|'seftali'|'gok'|'nane'|'kum'|'mercan'|'lacivert'|'limon';  // 03-DESIGN.md
```
Tohum kategoriler (Türkçe): aktivite: Spor, İş, Aile, Sosyal, Kurs, Kişisel gelişim; hedef: Sağlık, Kariyer, Finans, Aile, Gelişim, Seyahat, Ev ve araç; gelir: Maaş, Ek gelir, Diğer gelir; gider: Market, Yeme içme, Ulaşım, Sağlık, Eğlence, Giyim, Eğitim, Ev, Fatura, Abonelik, Çocuk, Diğer; sabit gider: Kira, Fatura, Abonelik, Haberleşme.

### activities, oluşumlar ve hızlı kayıt
```ts
type Recurrence =
  | { type: 'weekdays'; daysOfWeek: Weekday[] }
  | { type: 'intervalDays'; every: number; anchorDate: LocalDate }
  | { type: 'weeklyFrequency'; targetCount: number };
type Timing =
  | { type: 'fixed'; start: LocalTime; durationMin: number }
  | { type: 'flexible'; window?: { from: LocalTime; to: LocalTime }; durationMin?: number };
interface Activity extends Base {
  kind: 'task' | 'event'; title: string; categoryId?: Id; note?: string;
  goalId?: Id; parentActivityId?: Id; // hedef adımı veya etkinlik hazırlığı
  systemKey?: 'work' | 'side-job' | 'workout' | 'family-meeting' | 'bedtime';
  date?: LocalDate;                  // tek seferlik; yoksa henüz planlanmamış
  recurrence?: Recurrence; startDate?: LocalDate; endDate?: LocalDate;
  timing?: Timing; place?: string; costMinor?: Minor; // tahmini TRY harcama
  confirmation: 'tentative' | 'confirmed';
  reminder?: { enabled: boolean; minutesBefore?: number; preferredTime?: LocalTime };
  paused: boolean; archived: boolean;
}
interface ActivityLog extends Base {
  occurrenceKey: string; activityId: Id; date: LocalDate;
  status: 'done' | 'skipped'; minutes?: number; note?: string;
}
interface OccurrenceOverride {
  occurrenceKey: string; activityId: Id;
  scheduledDate?: LocalDate; timing?: Timing;
  state: 'scheduled' | 'removedFromWeek'; updatedAt: EpochMs;
}
interface Capture extends Base {
  text: string; status: 'inbox' | 'converted';
  convertedTo?: { type: 'activity' | 'goal'; id: Id };
}
interface WeekPlan {
  weekKey: WeekKey; status: 'draft' | 'confirmed';
  activeGoalIds: Id[];
  assignments: { occurrenceKey: string; date: LocalDate; timing?: Timing }[];
  updatedAt: EpochMs;
}
interface MeetingPreference extends Base {
  preferredDays: Weekday[]; preferredWindow?: { from: LocalTime; to: LocalTime };
  note?: string; // tercih; kesin görüşme kaydı değildir
}
```
- Aynı olayın taşınması kimliğini değiştirmez: tarihe bağlı tekrarlar için ilk planlanan tarih üzerinden sabit `activityId:originalDate`; haftalık sıklık için `activityId:weekKey:slot`, tek görev için `activityId:once`. Haftalık slot sayıları sonradan değişirse mevcut log/kimlik korunur; gerçekleşmiş geçmiş yeniden yazılmaz.
- Erteleme `OccurrenceOverride` ve plan atamasını tek transaction'da değiştirir; eski bildirim iptali için niyet bırakır. Tamamlama yalnız `ActivityLog`'dadır. Hedef ilerlemesi buradan türetilir, ikinci “tamamlandı” kaydı tutulmaz. Geri alma ilgili türetilmiş ilerlemeyi de geri alır.
- Saat/gün kesin olmayan tercih kapasiteyi kesin randevu gibi bloke etmez. Plan/timing verilmemiş iş gecikmiş sayılmaz. `fixed` zamanlama ancak gerçek saat ve süre belli olduğunda kullanılır.
- Ek iş hafta içi mesai penceresinde esnektir, saat 10:00 yerleşimi/15:00 zorunlu sorgusu yok. Süre hedefi ayarlanabilir. Hafta sonu ayrı hedef ancak kullanıcı seçerse eklenir.
- Fitness `intervalDays: 2`, kullanıcı başlangıç tarihine göre; hafta sınırından bağımsızdır. Tek oluşumu taşımak mı döngüyü kaydırmak mı PRD O2'de açık; kural değişimi geçmişi koruyan sürüm/etki tasarımı gerektirir.
- Öğle görüşmesi, haftalık sabit dört spor günü veya mecburi aile saat kotası üretilmez. Görüşme tercihleri kesinleşince birden fazla tarihli etkinlik oluşabilir. Onaylanan görüşmenin hazırlanma/ulaşım payı kullanıcı verisidir.
- Hızlı kayıttan dönüşüm kaynak kaydı ve hedef bağlantısını atomik günceller; tekrar dönüşüm var olan hedefi döndürür. Görev/etkinlik bağlantıları döngü oluşturamaz. Arşiv/duraklatma geçmiş logları silmez.
- Tohum kayıtlar yalnız kullanıcının seçtiği düzen için üretilir; eksik saat/süre/maaş uydurulmaz.

### reminderIntents (platformdan bağımsız yeniden planlama niyeti)
```ts
interface ReminderIntent {
  id: string; entityId: Id; occurrenceKey?: string; revision: number;
  dueAt?: EpochMs; status: 'needsReconcile' | 'scheduled' | 'cancelled';
}
```
Bu, nihai OS API şeması değildir. Tercihler, sessiz saatler ve kayıt sürümü core'da uygulanır; adaptör izin ve kuyruk durumunu gözler. Veri yazma ile platform alarmı atomik değildir; yeniden deneme/iptal ve eski eylem kontrolü gerekir. OS tanıtıcıları cihaz yerelidir, yedekten aynen kullanılmaz. Bildirim erteleme faturanın `dueDate` alanını değiştirmez. Kesin tekrar politikası PRD O6'dadır.

### goals, goalLogs
```ts
interface Goal extends Base {
  categoryId: Id; title: string; why?: string;
  type: 'life' | 'financial';
  targetDate?: LocalDate; status: 'active' | 'later' | 'paused' | 'done';
  financial?: { targetMinor: Minor; linkedAccountIds: Id[]; manualSavedMinor?: Minor }; // v1 (tip TRY)
}
interface GoalLog extends Base { goalId: Id; date: LocalDate; text: string }
```

### accounts, rates
```ts
interface Account extends Base {
  name: string; type: 'bank' | 'cash' | 'crypto' | 'gold'; assetCode: 'TRY'|'USDT'|'XAU_G';
  openingMinor: Minor; sortOrder: number; archived: boolean; iconKey: IconKey; colorKey: ColorKey;
}
interface Rate { id: Id; assetCode: 'USDT'|'XAU_G'; tryPerUnitKurus: Minor; at: EpochMs; source: 'manual' | 'transfer' }
```
Güncel kur = `assetCode` için en yeni `at`. TRY için kur yoktur (1).

### transactions
```ts
interface Transaction extends Base {
  type: 'income' | 'expense' | 'transfer' | 'card_payment';
  date: LocalDate; status: 'planned' | 'posted';
  note?: string; categoryId?: Id; receiptIds?: Id[];
  // income
  accountId?: Id; amountMinor?: Minor; incomeSourceId?: Id;
  // expense: ya hesap ya kart
  pay?: { kind: 'account'; accountId: Id } | { kind: 'card'; cardId: Id };
  // transfer (farklı varlık = dönüşüm): TRY -> USDT gibi
  fromAccountId?: Id; toAccountId?: Id; fromMinor?: Minor; toMinor?: Minor; savingsMonth?: YearMonth;
  // card_payment
  cardId?: Id;                                // ödemeyi alan kart; fromAccountId + amountMinor
  // bağlantılar
  fixedExpenseId?: Id; billId?: Id; activityId?: Id; occurrenceKey?: string;
  installmentPlanId?: Id; installmentNo?: number;
  autoKey?: string; commandId?: string; origin: 'manual' | 'auto';
}
```
Doğrulama: `income` için `accountId` ve `amountMinor` zorunlu; `expense` için `pay` ve tutar; `transfer` için from/to ve iki tutar; `card_payment` için `cardId`, `fromAccountId`, `amountMinor`. Tutar > 0. Gider/gelir tutarı hesabın varlık biriminde (TRY dışı hesaba gider yazılabilir; TL karşılığı güncel kurla gösterilir).

### incomeSources
```ts
interface IncomeSource extends Base {
  name: string; kind: 'salary' | 'extra' | 'other'; categoryId: Id; accountId: Id;
  recurrence: { type: 'monthly'; day: number } | { type: 'oneoff'; date: LocalDate };
  amounts: { from: YearMonth; amountMinor: Minor }[];   // zam olunca yeni satır; en yeni <= ay geçerli
  active: boolean;
}
```
Maaş: `monthly`, `day: 15`. Gün ayda yoksa (31) ayın son gününe sıkıştırılır. Hafta sonuna denk gelse bile kayıt o tarihte oluşur (kullanıcı düzenler).

### fixedExpenses ve bills
Düzenli gider tanımı ile dönem faturası ayrı varlıklardır; bir fatura harcama kaydının kendisi değildir.
```ts
interface FixedExpense extends Base {
  name: string; categoryId: Id;
  frequency: { type: 'monthly'; day?: number } | { type: 'yearly'; month: number; day?: number };
  amountMode: 'fixed' | 'variable';
  amounts: { from: YearMonth; amountMinor: Minor }[]; // boş olabilir, tahminin geçmişi
  pay?: { kind: 'account'; accountId: Id } | { kind: 'card'; cardId: Id };
  automaticPaymentInstruction: boolean; // bankada talimat var; ödendi demek değil
  warnDays?: number; startMonth: YearMonth; endMonth?: YearMonth; active: boolean;
}
interface Bill extends Base {
  fixedExpenseId: Id; periodKey: string; // örn. fix:<id>:2026-11, unique
  dueDate?: LocalDate;
  amountState: 'unknown' | 'estimated' | 'confirmed';
  amountMinor?: Minor;              // bu fatura akışında TRY kuruş
}
```
- `unknown` tutar yokluğu, sıfır ödeme değildir. `estimated`/`confirmed` pozitif tutar ister. Vade bilinmiyorsa ayrı eksik bilgi kartı vardır; sahte son ödeme tarihi/bildirimi yok.
- Yaklaşan iki dönem hazırlanır, kaçan dönemler idempotent telafi edilir. `Bill` beklenen yükümlülüktür; ayrıca aynı tutarda planned expense yaratılıp iki kez sayılmaz.
- Ödemeler `transactions.billId` bağlantılı posted giderlerdir. İşlem tutarı/hesabı onaylanmadan gider oluşmaz; bu taslak fatura örnekleri TRY üzerindendir. Farklı varlıkla ödeme eşlemesi O4 finans tasarımında netleşmeden farklı birimdeki tutarlar doğrudan toplanmaz.
- Ödeme durumu türetilir: ödemeler yoksa bekliyor; ödeme var ama kesin tutara ulaşmıyorsa kısmi; kesin tutar karşılandıysa ödendi. Tutar tahminiyse “tutarı kesinleştir” durumu kalır, tam ödeme uydurulmaz. Fazla ödeme farkı gizlenmez; düzeltme gerekir.
- Aynı ödeme eylemi sabit komut kimliğiyle bir kez işlenir; ayrı kısmi ödemeler ayrı kimliktir. Transaction ödeme kaydı + yeniden bildirim niyeti birlikte yazılır. Tam ödenince eski uyarılar iptal edilir; geri al/düzeltme bakiye ve kalan uyarısını yeniden hesaplar.
- `automaticPaymentInstruction` veya vade tarihi asla kendiliğinden posted ödeme yaratmaz. Tahmini tutar ve vade değişiklikleri gerçekleşmiş ödemeleri geriye dönük değiştirmez.
- Etkinlikten gider oluşturma da kullanıcı onayı ve komut kimliğiyle yapılır; tahmini maliyet kendi başına işlem değildir. Aynı etkinlik için kullanıcı farklı harcamaları bilinçli ekleyebilir.

Kart kısmi ödemesinin hangi ekstreyi kapattığı, iade ve düzeltme muhasebesi PRD O4'te açık kapıdır. Bunlar tanımlanmadan yalnız genel kart borcuna bakılarak “bu ekstre ödendi” çıkarılamaz.

### creditCards, installmentPlans
```ts
interface CreditCard extends Base { name: string; limitMinor: Minor; statementDay: number; dueDay: number; payFromAccountId?: Id; archived: boolean; colorKey: ColorKey }
interface InstallmentPlan extends Base {
  title: string; cardId: Id; categoryId: Id;
  totalMinor: Minor;            // karttan çıkacak TOPLAM (faiz/vade farkı dahil)
  count: number;                // 2..36
  startMonth: YearMonth;        // ilk taksidin yansıdığı ekstre ayı
  cashPriceMinor?: Minor;       // peşin fiyat (varsa vade farkı = total - cash)
  purchaseDate: LocalDate; note?: string;
}
```
Plan oluşturulunca `count` adet `expense` işlemi (`status: 'planned'`, `pay: {kind:'card'}`, `installmentNo`, `autoKey: ins:<planId>:<no>`) tek transaction'da üretilir. Plan düzenlenirse yalnızca henüz `planned` olanlar yeniden üretilir; `posted` olanlar korunur.

### savingsMonths, budgets (v1), receipts, snapshots, meta
```ts
interface SavingsMonth { month: YearMonth; plannedMinor: Minor; actualMinor?: Minor; confirmedAt?: EpochMs }
// Ayarlarda sabit: savingsMonthlyTargetMinor (tutar sabit; SavingsMonth.plannedMinor onun kopyası)
interface Budget extends Base { month: YearMonth | 'default'; categoryId?: Id; limitMinor: Minor }
interface Receipt { id: Id; transactionId?: Id; blob: Blob; mime: string; width: number; height: number; bytes: number; createdAt: EpochMs }
interface Snapshot { date: LocalDate; assetsKurus: Minor; cardDebtKurus: Minor }
```

## 4. Hesap kuralları (test vektörleriyle)

### 4.1 Taksit
`base = floor(total / n)`, `kalan = total - base * n`; ilk n-1 taksit `base`, SON taksit `base + kalan`.
- 1.000,00 TL / 6: toplam 100000 kuruş -> base 16666, kalan 4 -> 5 x 166,66 + 1 x 166,70 (toplam 1000,00).
- 12.999,00 TL / 6: 1299900 -> base 216650, kalan 0 -> 6 x 2.166,50.
- Değişmez: taksitlerin toplamı = total; hepsi > 0; sayı = n.
- Taksit k'nin ayı: `addMonths(startMonth, k-1)`; satırın `date` değeri o ayın ekstre kesim günüdür (ay sonuna sıkıştırılır).
- Vade farkı: `cashPriceMinor` varsa `total - cash` (negatif olamaz; uyarı verilir).
- Taksit satırı `planned` -> `posted`: `date <= bugün` olduğunda günlük iş otomatik yapar (ekstreye yansıdı). Taksitler tek tek "ödendi" işaretlenmez; ekstre ödemesi `card_payment` ile kaydedilir.
- "Taksitlerim" özeti: aktif plan sayısı (kalan taksidi > 0), bu ayki taksit toplamı (ay bazında), kalan toplam, bitiş ayı = `addMonths(startMonth, n-1)`, ilerleme = `posted` taksit / n. Önümüzdeki 12 ay taksit yükü = ay bazlı `planned+posted` toplamı.

### 4.2 Kart ekstresi
Kesim günü `S`, son ödeme günü `D` (ay uzunluğuna sıkıştırılır).
- Bir harcamanın ekstre kapanış tarihi: gün <= S ise o ayın S'si, değilse sonraki ayın S'si.
- Son ödeme tarihi: kapanıştan sonraki ilk `D`. `D > S` ise aynı ay, `D <= S` ise sonraki ay.
- Örnek S=20, D=5: 20 Mart alışveriş -> kapanış 20 Mart, son ödeme 5 Nisan. 21 Mart -> kapanış 20 Nisan, son ödeme 5 Mayıs. S=10, D=25: 10 Mart -> kapanış 10 Mart, son ödeme 25 Mart.
- Ekstre tutarı = kapanışı o tarih olan tüm kart giderleri (tek çekim + o aya düşen taksitler), `planned` taksitler dahil gösterilir ("ekstreye yansıyacak").
- Kart borcu = `posted` kart giderleri toplamı - `card_payment` toplamı (negatifse "alacak" olarak gösterilir).
- Son ödeme uyarısı ilgili ekstrenin kalan ödemesine göre olmalıdır; genel kart borcu tek başına yeterli değildir. Ödeme dağıtımı ve iade/düzeltme modeli PRD O4 kapsamında Faz 5 kapısından önce tamamlanır.

### 4.3 Gider/gelir sayımı (istatistik ve bütçe)
- Aylık gider = o ay `date`'li `posted` `expense` işlemleri. Tek çekim kart harcaması satın alma tarihinde; taksit, kendi taksit ayında sayılır.
- `transfer` ve `card_payment` gider değildir (çifte sayımı önler).
- Aylık gelir = o ay `posted` `income`. Farklı varlık tutarları ham minor birimleriyle toplanmaz; TRY değerleme ve kur tarihi açıklanır.
- "Beklenen" = ilgili dönem planned taksit/işlemler + bills kalanları; aynı faturanın ödeme işlemi yeniden yükümlülük diye sayılmaz. Bilinmeyen tutarlar ayrı sayılır; toplam eksik/tahmini etiket taşır.
- Aylık gelir/gider ve kategori dağılımı MVP'dir; bütçe limiti/ileri grafik v1'dir. Kullanılabilir para hesabı nakit akışıdır; aylık gider toplamıyla aynı değildir (4.9).

### 4.4 Bakiye ve net değer
- Hesap bakiyesi = `openingMinor` + posted income + gelen transfer (`toMinor`) - giden transfer (`fromMinor`) - hesaptan ödenen posted gider - `card_payment`.
- Toplam varlık (TL) = Σ `valueInTRY(bakiye, güncel kur)`. Net değer = toplam varlık - kart borcu (ekranda "kart borcunu düş" anahtarı).
- Test vektörü: 125,5 USDT, kur 41,35 TL -> `125500000 * 4135 / 10^6 = 518942,5` -> yuvarla -> 518943 kuruş = 5.189,43 TL. 3,250 g altın, gram 4.850,25 TL -> `3250 * 485025 / 1000 = 1.576.331,25` -> 1576331 kuruş = 15.763,31 TL.
- Transfer (dönüşüm) kaydedilince örtük kur = `fromMinor`/`toMinor`; kullanıcıya "kuru güncelle?" önerilir (`Rate.source: 'transfer'`).

### 4.5 Tamamlama, ilerleme ve toparlama
Tek log bir oluşumun tamamlanmasını temsil eder; hedefte/Plan'da/Bugün'de türetilir. Not, dakika ve puan zorunlu değildir. Tekrar eden bildirim veya iki ekrandan işlem ikinci log yaratmaz. Tamamlanmayan iş otomatik yarına eklenmez. Günü %60 eşiğiyle başarılı/başarısız sayan zorunlu seri/joker kuralı kaldırılmıştır; olası isteğe bağlı göstergeler v1'de ayrı tasarlanır.

### 4.6 Haftalık plan (açıklanabilir; AI şart değil)
1. Görüşme tercihleri ve kesin randevular ayrılır; önce kesin zamanlı olaylar yerleşir. Öğle aile görüşmesi üretilmez.
2. Aktif hedeflerin küçük adımları, mesai içi ek iş ve gün aşırı fitness ayrı zamanlama kurallarına sahiptir. Haftada N oturum için N sabit kimlikli slot vardır; onaylanan gün ataması gerekir.
3. Esnek iş pencere içinde görünür; kullanıcı seçmedikçe sahte saate yerleşmez. İş mesaisi ek iş için izinli pencere olabilir, buna gereksiz çakışma çıkarılmaz.
4. Bilinen sürelerle kapasite/ulaşım/dinlenme payı önizlenir; bilinmeyen süreler açık gösterilir. Bütün boşlukları doldurma zorunluluğu yoktur.
5. Kullanıcı haftayı onaylamadan öneriler kayda kesin randevu olarak geçmez. Sonraki değişiklikler eski bildirimleri geçersizleştirir.
6. Bir iş bu haftadan çıkarılabilir veya duraklatılabilir; oluşum taşıma ve seri kuralı değişimi ayrıdır. Fitness döngü erteleme politikası O2 tamamlanmadan uygulanmaz.

### 4.7 Otomatik kayıtlar ve telafi
`runDailyJobs(now)`: gelir/taksit için `autoKey`, fatura dönemi için `periodKey` ile eksikler idempotent üretilir; zaten varsa atlanır. Maaş gününde otomatik muhasebe kaydı banka kanıtı değildir; kullanıcı gerçekleşmeyen geliri düzeltebilir. Fatura vadesi ödeme yaratmaz. Uygulama kapalıyken bu fonksiyonun çalışacağı varsayılmaz; OS bildirim zamanlaması ayrıdır.

### 4.8 Birikim kartı
Maaş günü (ayın `incomeSources[salary].day`) ve sonraki 3 gün boyunca Bugün'de: "Bu ay ne kadar biriktirdin/biriktireceksin?" Varsayılan tutar `savingsMonthlyTargetMinor`. Kullanıcı kaynak/hedef hesap ve tutarları onaylayınca `transfer` (savingsMonth etiketli) oluşur. "Sonra aktarırım" yalnız planlanan tutarı tutar, `actualMinor` yaratmaz. Gerçekleşen birikim etiketli transferlerden hesaplanır; varsa `actualMinor` yalnız türetilmiş önbellektir.

### 4.9 Tahmini kullanılabilir para (tasarım kapısı: O4)
Seçili likit TRY hesapları, açıkça belirtilen dönem, o dönemin ödenmemiş yükümlülükleri ve birikim planı temel girdilerdir. Altın/USDT varlık değerini otomatik nakit sayma. Gerçekleşmiş birikim transferini bakiye zaten içerdiği için tekrar düşme. Kart satın alımı harcama istatistiği, kart ödemesi nakit çıkışıdır; aynı borcu iki kez düşme. Bilinmeyen tutar/vade/kur ayrı eksik bilgi olarak görünür; sıfıra çevrilmez. Formül, ödeme dağıtımı ve sınır vektörleri Faz 3/5 tasarımında onaylanıp test edilmeden kesin kullanılabilir tutar gösterilmez.

## 5. Yedek kapsamı
Kullanıcı verisi tabloları (yakalamalar, hedef bağlantıları, tercihler, planlar, faturalar ve loglar dahil), `receipts` isteğe bağlı; `schemaVersion` zorunludur. Soft-deleted kayıtlar dışa aktarılmaz; dışarıda kalan kayıtlara referanslar doğrulanır. `installId`, OS alarm kimlikleri ve gözlenen izin durumu taşınmaz. Hatırlatma tercihleri taşınır; yeni cihaz alarm niyetleri yeniden üretilir. Üzerine yazma öncesi şifreli kurtarma yedeği önerilir. Birleştirme için çatışma/tombstone politikası belirlenmedi; PRD O5 kapalıdır.

## 6. Migrasyon politikası
Her şema değişikliği: yeni `db.version(n)` + `upgrade` fonksiyonu + `packages/data/test/fixtures/v(n-1).json` -> v(n) testi + yedek içe aktarmada eski sürümü yükselten dönüştürücü. Geriye dönük uyumsuz değişiklik yasaktır; alan eklemek serbesttir, alan kaldırmak iki adımda (önce kullanımdan kaldır, sonraki sürümde sil).

## 7. Örnek (uydurma) veri
Testler ve demo "örnek verileri yükle" düğmesi YALNIZCA uydurma değerler kullanır (örn. maaş 50.000 TL). Gerçek kişisel değerler asla koda, test veya commit edilen belgeye girmez (bkz. `docs/PROFIL.local.md`).
