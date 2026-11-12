# 05 - Veri Modeli ve Hesap Kuralları (Gün Gün)

Bu belge veri yapısının ve tüm hesapların tek kaynağıdır. `packages/core/src/schemas` bu belgenin Zod karşılığıdır. Şema değişirse: bu belge + Zod şeması + Dexie migrasyonu + migrasyon testi birlikte güncellenir.

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
  activities:       'id, kind, categoryId, systemKey, archived',
  activityLogs:     'id, &[activityId+date], date, activityId',
  dayRecords:       'date',
  weekChoices:      'weekKey',
  goals:            'id, categoryId, status, type',
  goalLogs:         'id, goalId, date',
  accounts:         'id, assetCode, archived, sortOrder',
  rates:            'id, assetCode, at',
  transactions:     'id, date, type, status, accountId, cardId, categoryId, [date+type], installmentPlanId, fixedExpenseId, incomeSourceId, &autoKey',
  incomeSources:    'id, kind, active',
  fixedExpenses:    'id, active, categoryId',
  creditCards:      'id, archived',
  installmentPlans: 'id, cardId, startMonth',
  savingsMonths:    'month',
  budgets:          'id, month, categoryId',     // v1
  receipts:         'id, transactionId',
  snapshots:        'date',
  meta:             'key',
});
```
`&autoKey` benzersiz ve seyrektir (yalnızca otomatik kayıtlarda dolu). Örnek: `inc:<kaynakId>:2026-11`, `fix:<id>:2026-11`, `ins:<planId>:3`.

## 3. Varlıklar

### settings (tek satır, `id: 'main'`)
```ts
interface Settings {
  id: 'main';
  displayName?: string;
  theme: 'light' | 'dark' | 'system';
  timezone: string;                    // "Europe/Istanbul"
  weekStartsOn: 1;
  wakeWeekday: LocalTime;              // varsayılan "06:30"
  wakeWeekend: LocalTime;              // varsayılan "08:00" (en geç)
  bedtime: LocalTime;                  // "23:00"
  workStart: LocalTime; workEnd: LocalTime;   // "08:00" "17:00" (yalnızca hafta içi)
  lunchStart: LocalTime; lunchEnd: LocalTime; // "12:00" "13:00"
  sideJobCheckInWeekday: LocalTime;    // "15:00"
  sideJobCheckInWeekend: LocalTime;    // "12:00"
  weekendFreeBy: LocalTime;            // "13:00" (hafta sonu sabah işleri bu saate kadar biter)
  goalCardTime: LocalTime;             // "21:30"
  jokerPerWeek: number;                // 1 (0-3)
  weekendSonMinHours: number;          // 4
  defaultSonDay: 'sat' | 'sun';        // 'sat'
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

### activities (düzenli + günübirlik; sistem rutinleri de buradadır)
```ts
interface Activity extends Base {
  kind: 'regular' | 'oneoff';
  title: string; categoryId: Id; note?: string;
  systemKey?: 'work' | 'son-lunch' | 'side-job' | 'workout' | 'son-weekend' | 'bedtime';
  schedule: RegularSchedule | OneOffSchedule;
  checkIn?: { time: LocalTime; question: string };      // örn. 15:00 "Bugün için gerekenleri yaptın mı?"
  countsForStreak: boolean;                              // düzenli: true, günübirlik: false (varsayılan)
  reminderMinutesBefore?: number;                        // .ics VALARM
  archived: boolean;
}
interface RegularSchedule {
  type: 'regular';
  daysOfWeek: Weekday[];
  start: LocalTime | null;                // null = esnek
  durationMin: number;
  flexWindow?: { from: LocalTime; to: LocalTime };       // esnekse (örn. ek iş 08:00-17:00)
  overrides?: Partial<Record<Weekday, { start?: LocalTime; durationMin?: number }>>;  // hafta sonu 2 saat gibi
  startDate: LocalDate;                   // Aralık'ta başlayan boks: 2026-12-xx
  endDate?: LocalDate;
  weeklyTarget?: number;                  // örn. spor haftada 4
}
interface OneOffSchedule { type: 'oneoff'; date: LocalDate; start?: LocalTime; durationMin?: number; place?: string; costMinor?: Minor }
```
Tohum sistem rutinleri (onboarding'de ayarlara göre üretilir; kullanıcı düzenleyebilir):
| systemKey | Günler | Zaman | Notlar |
|---|---|---|---|
| work | Pzt-Cum | 08:00-17:00 | Çakışma hesabında "meşgul" ama ek iş ve öğle arası bu blok içinde serbesttir |
| son-lunch | Pzt-Cum | 12:00-13:00 | Her gün oğlunla; not alanı ("bugün ne yaptık") |
| side-job | Pzt-Cum 60 dk esnek (08:00-17:00 penceresi); Cmt-Paz 120 dk | Hafta içi checkIn 15:00; hafta sonu checkIn 12:00 | Hafta sonu en geç `weekendFreeBy`'a kadar biter. Varsayılan yerleşim: hafta içi 10:00, hafta sonu 09:30 |
| workout | Seçilen 4 gün (varsayılan Pzt, Çar, Cmt, Paz) | Hafta içi 20:00, hafta sonu sabah (varsayılan 11:00) | 35-40 dk, ev. Kurslar gelince çakışma önerisi |
| son-weekend | Seçilen "oğul günü" (varsayılan Cmt) | 13:00-18:00 | En az `weekendSonMinHours` saat; haftalık seçim `weekChoices` ile değişir |
| bedtime | Her gün | `bedtime` | Yalnızca hatırlatma, varsayılan kapalı |

### activityLogs
```ts
interface ActivityLog extends Base {
  activityId: Id; date: LocalDate;
  status: 'done' | 'skipped' | 'moved';
  minutes?: number; note?: string;           // oğul notu burada
  movedToDate?: LocalDate;
}
```
Benzersiz `[activityId+date]`. Kayıt yoksa: geçmiş günde "kaçırıldı" olarak yorumlanır (kaydedilmez).

### dayRecords, weekChoices
```ts
interface DayRecord { date: LocalDate; unplanned?: boolean; jokerUsed?: boolean; note?: string; updatedAt: EpochMs }
interface WeekChoice { weekKey: WeekKey; sonDay: 'sat' | 'sun'; updatedAt: EpochMs }
```

### goals, goalLogs
```ts
interface Goal extends Base {
  categoryId: Id; title: string; why?: string;
  type: 'life' | 'financial';
  targetDate?: LocalDate; status: 'active' | 'paused' | 'done';
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
  fixedExpenseId?: Id; installmentPlanId?: Id; installmentNo?: number;
  autoKey?: string; origin: 'manual' | 'auto';
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

### fixedExpenses
```ts
interface FixedExpense extends Base {
  name: string; categoryId: Id;
  frequency: { type: 'monthly'; day: number } | { type: 'yearly'; month: number; day: number };
  amountMode: 'fixed' | 'variable';
  amounts: { from: YearMonth; amountMinor: Minor }[];   // 'variable' ise tahmin (estimate)
  pay: { kind: 'account'; accountId: Id } | { kind: 'card'; cardId: Id };
  autoPost: boolean;            // true = vade gününde otomatik posted
  warnDays?: number;            // ayarlardaki varsayılanı ezer
  startMonth: YearMonth; endMonth?: YearMonth; active: boolean;
}
```
Her ay için `planned` işlem üretilir (`autoKey: fix:<id>:<YYYY-MM>`), yalnızca gelecek 2 ay önceden. Kullanıcı "Ödedim" (değişkense gerçek tutarı girerek) -> `posted`.

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
- Son ödeme uyarısı: son ödeme gününe `fixedExpenseWarnDays` (varsayılan 3) gün kala ve kart borcu > 0 ise.

### 4.3 Gider/gelir sayımı (istatistik ve bütçe)
- Aylık gider = o ay `date`'li `posted` `expense` işlemleri. Tek çekim kart harcaması satın alma tarihinde; taksit, kendi taksit ayında sayılır.
- `transfer` ve `card_payment` gider değildir (çifte sayımı önler).
- Aylık gelir = o ay `posted` `income`.
- "Beklenen" = o ay `planned` işlemler (sabit gider, taksit).
- Kalan = gelir (beklenen maaş dahil) - posted gider - planned gider (bilgi satırı, bakiye değildir).

### 4.4 Bakiye ve net değer
- Hesap bakiyesi = `openingMinor` + posted income + gelen transfer (`toMinor`) - giden transfer (`fromMinor`) - hesaptan ödenen posted gider - `card_payment`.
- Toplam birikim (TL) = Σ `valueInTRY(bakiye, güncel kur)`. Net değer = toplam birikim - kart borcu (ekranda "kart borcunu düş" anahtarı).
- Test vektörü: 125,5 USDT, kur 41,35 TL -> `125500000 * 4135 / 10^6 = 518942,5` -> yuvarla -> 518943 kuruş = 5.189,43 TL. 3,250 g altın, gram 4.850,25 TL -> `3250 * 485025 / 1000 = 1.576.331,25` -> 1576331 kuruş = 15.763,31 TL.
- Transfer (dönüşüm) kaydedilince örtük kur = `fromMinor`/`toMinor`; kullanıcıya "kuru güncelle?" önerilir (`Rate.source: 'transfer'`).

### 4.5 Gün değerlendirme ve seri (streak)
- Bir günün "gereken" öğeleri: o gün oluşan `countsForStreak=true` aktivite oluşumları + (varsa) `checkIn`'ler.
- Gün "tutuldu": gereken öğelerin `done` oranı >= %60 (ayarlanabilir, varsayılan 60) VEYA gün `unplanned` işaretli ve o hafta joker hakkı varsa (`jokerUsed=true` yapılır).
- Joker: hafta (Pzt-Paz) başına `jokerPerWeek` (varsayılan 1) "plansız gün" koruması. Hakkı biten haftada ikinci plansız gün "tutulmadı" sayılır ama arayüz yargılamaz ("Yeniden başlamak için yarın uygun").
- Seri = geçmişe doğru art arda "tutuldu" gün sayısı. Bugün henüz bitmediyse seriyi kırmaz (gün sonunda değerlendirilir). Gereken öğesi olmayan gün seriyi korur (nötr).
- Test: 3 gün tutuldu + 1 plansız (joker var) + 1 tutuldu -> seri 5. Aynı hafta 2. plansız -> seri 0'a döner.

### 4.6 Haftalık otomatik plan (açıklanabilir, AI değil)
1. Sabit bloklar yerleşir: iş, öğle (oğlunla), oğul günü bloğu (hafta sonu seçimine göre).
2. Esnek bloklar tercih saatine yerleşir: ek iş, spor.
3. Her çakışma `Conflict { a, b, kind }` olarak raporlanır ve en yakın boş aralık/uygun gün önerilir (aynı gün +-, sonra diğer günler). Öneri kullanıcı onayı olmadan uygulanmaz.
4. Hafta sonu kuralı: ek iş + spor + yemek, `weekendFreeBy` (13:00) saatine kadar sığmıyorsa uyarı (kaç dakika taştığı yazılır).
5. Aralık'ta boks/salsa (haftada 1 gün, 1 saat) eklenince spor günüyle çakışma kontrolü yapılır; çakışırsa "Spor'u X gününe kaydır" önerisi.
6. Haftalık "oğlunla zaman" toplamı (öğle arası + hafta sonu bloğu) hedefin (5 x 1 saat + en az 4 saat) altındaysa Plan ekranında sakin bir bilgi gösterilir.

### 4.7 Otomatik kayıtlar ve telafi
`runDailyJobs(now)`: her gelir kaynağı/sabit gider/taksit için `lastProcessed`'dan bugüne kadar eksik her ay/tarih `autoKey` ile kontrol edilerek eksikler üretilir; zaten varsa atlanır. Maaş, gün geldiğinde `posted` olarak kaydedilir.

### 4.8 Birikim kartı
Maaş günü (ayın `incomeSources[salary].day`) ve sonraki 3 gün boyunca Bugün'de: "Bu ay ne kadar biriktirdin/biriktireceksin?" Varsayılan tutar `savingsMonthlyTargetMinor`. Onayda kullanıcı hangi hesaba aktardığını seçer; bir `transfer` (savingsMonth etiketli) oluşturulur veya yalnızca `SavingsMonth.actualMinor` kaydedilir ("sonra aktarırım").

## 5. Yedek kapsamı
Tüm tablolar (meta hariç `installId`), `receipts` isteğe bağlı ("fişleri de ekle" anahtarı; boyut tahmini gösterilir). `schemaVersion` zorunludur.

## 6. Migrasyon politikası
Her şema değişikliği: yeni `db.version(n)` + `upgrade` fonksiyonu + `packages/data/test/fixtures/v(n-1).json` -> v(n) testi + yedek içe aktarmada eski sürümü yükselten dönüştürücü. Geriye dönük uyumsuz değişiklik yasaktır; alan eklemek serbesttir, alan kaldırmak iki adımda (önce kullanımdan kaldır, sonraki sürümde sil).

## 7. Örnek (uydurma) veri
Testler ve demo "örnek verileri yükle" düğmesi YALNIZCA uydurma değerler kullanır (örn. maaş 50.000 TL). Gerçek kişisel değerler asla koda, test veya commit edilen belgeye girmez (bkz. `docs/PROFIL.local.md`).
