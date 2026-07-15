# Инструкция по вёрстке модуля 11-Finance

## 1. Обзор модуля

### Назначение
Модуль **11-Finance** — это финансовый раздел мобильного приложения Rollout. Он объединяет управление счетами и картами, аналитику расходов/доходов, переводы по СБП/QR, оплату услуг, курсы валют и металлов, финансовые продукты (кредиты, вклады, ипотека) и настройки СБП.

### Экраны модуля (список страниц из Figma)

| # | Страница Figma | Описание |
|---|----------------|----------|
| 1 | **Cover** | Обложка модуля |
| 2 | **Finance** | Главный экран с общей суммой и карточками счетов |
| 3 | **Analytics** | Аналитика финансов (донат-чарт, категории) |
| 4 | **Cards** | Список карт стопкой (stack) |
| 5 | **AllAccounts** | Все счета с группировкой |
| 6 | **CardSelection1-3** | Выбор основной карты для переводов |
| 7 | **Pay** | Платёжный хаб (QR, телефон, карта, Bluetooth) |
| 8 | **QrCode** | Генерация QR-кода для получения переводов |
| 9 | **PhoneNumber** | Привязка номера телефона |
| 10 | **ShareQrCode** | Поделиться QR-кодом |
| 11 | **AddingPhoneNumber** | Добавление номера телефона |
| 12 | **Сamera / CameraScans** | Сканирование QR-кодов |
| 13 | **PaymentBluetooth 1-5** | Оплата через NFC/Bluetooth |
| 14 | **Payment** | Экран подтверждения платежа |
| 15 | **InvoicesForPayment** | Кошелёк / выбор счёта для оплаты |
| 16 | **FinancialProducts** | Список финансовых продуктов (каталог) |
| 17 | **FinancialCatalog** | Каталог продуктов (микрокредит, ипотека и т.д.) |
| 18 | **FinancialOnboarding** | Онбординг для кредита/продукта |
| 19 | **FinancialForm** | Анкета/форма заявки |
| 20 | **FinancialOffers** | Предложения от банков |
| 21 | **FinancialDetails** | Детали условий кредита (bottom sheet) |
| 22 | **FinancialManagement** | Управление финансовыми продуктами |
| 23 | **FundsMovement** | Заказ справки о движении средств |
| 24 | **AccountDetails** | Детали счёта |
| 25 | **AvailableFunds** | Доступные средства |
| 26 | **Incomes** | Доходы |
| 27 | **CivilServants** | Справка для госслужащих |
| 28 | **ExchangeRate / ExchangeRateFull** | Курсы валют (таблица) |
| 29 | **CurrencyAlert** | Уведомление об отсутствии счёта в металле |
| 30 | **MetalSelection** | Выбор металла (Au, Ag, Pt, Pd) |
| 31 | **CurrenciesMetals** | Валюты и металлы |
| 32 | **AccountOpening 1-4** | Открытие валютного/металлического счёта |
| 33 | **MetalDrawer 1-5** | Пошаговое открытие металлического счёта |
| 34 | **CurrencyAccount 2,4,5** | Открытие валютного счёта |
| 35 | **BuyCurrency** | Покупка валюты |
| 36 | **CurrencyMap 1-6** | Карта валют (курсы, графики) |
| 37 | **PaymentsServices** | Сервисы оплаты |
| 38 | **ProvidersList** | Список поставщиков |
| 39 | **PaymentProcessStep1-2** | Процесс оплаты (штрафы ГАИ и т.д.) |
| 40 | **PaymentProcessSearchResult** | Результаты поиска |
| 41 | **PaymentProcessDataCheck** | Проверка данных платежа |
| 42 | **SettingsSBPAccount** | Настройки СБП — приоритетный счёт |
| 43 | **SettingsSBPTransfer** | Настройки переводов по СБП |
| 44 | **SettingsSBPPayment** | Настройки оплаты по СБП |
| 45 | **SettingsSBPLimits** | Лимиты СБП |
| 46 | **SettingsSBPTariff** | Тарифы СБП |
| 47 | **SettingsSBPTerms** | Условия СБП |

### Роуты (URL-пути)

```
/finance                    → Finance (главный)
/finance/analytics          → Analytics
/finance/cards              → Cards
/finance/accounts           → AllAccounts
/finance/account/:id        → AccountDetails
/finance/pay                → Pay (платёжный хаб)
/finance/pay/qr             → QrCode
/finance/pay/phone          → PhoneNumber
/finance/pay/share-qr       → ShareQrCode
/finance/pay/bluetooth      → PaymentBluetooth
/finance/pay/camera         → Camera
/finance/pay/card-select    → CardSelection
/finance/pay/invoice        → InvoicesForPayment
/finance/pay/process        → PaymentProcessStep1
/finance/exchange           → ExchangeRate
/finance/exchange/full      → ExchangeRateFull
/finance/exchange/buy       → BuyCurrency
/finance/exchange/metals    → MetalSelection
/finance/exchange/open      → AccountOpening
/finance/products           → FinancialCatalog
/finance/products/:id       → FinancialOnboarding
/finance/products/apply     → FinancialForm
/finance/products/offers    → FinancialOffers
/finance/products/manage    → FinancialManagement
/finance/statement          → FundsMovement
/finance/statement/civil    → CivilServants
/finance/settings/sbp       → SettingsSBPAccount
/finance/settings/sbp-transfer → SettingsSBPTransfer
/finance/settings/sbp-payment  → SettingsSBPPayment
/finance/settings/sbp-limits   → SettingsSBPLimits
```

---

## 2. Дизайн-система (из макетов)

### 2.1 Цвета

| Токен | Значение | Использование |
|-------|----------|---------------|
| `--bg-primary` | `#0A0A0A` | Основной фон страницы |
| `--bg-card` | `#1C1C1E` | Фон карточек, инпутов, боттом-шитов |
| `--bg-card-elevated` | `#2C2C2E` | Наведение/активное состояние карточки |
| `--border-default` | `#3A3A3C` | Бордеры карточек, инпутов, сепараторов |
| `--border-active` | `#8E8E93` | Активный бордер (focus) |
| `--text-primary` | `#FAFAFA` | Основной текст, заголовки |
| `--text-secondary` | `#8E8E93` | Вторичный текст, placeholders, подписи |
| `--text-tertiary` | `#636366` | Disabled, hint text |
| `--button-primary-bg` | `#E5E5E5` | Primary button background |
| `--button-primary-text` | `#0A0A0A` | Primary button text |
| `--button-secondary-bg` | `transparent` | Secondary button background |
| `--button-secondary-border` | `#3A3A3C` | Secondary button border |
| `--accent-green` | `#34C759` | Рост курса, положительная динамика |
| `--accent-red` | `#FF3B30` | Падение курса, ошибка |
| `--accent-blue` | `#0A84FF` | Информация, ссылки |
| `--tab-active` | `#3A3A3C` | Активный таб (segmented) |
| `--tab-inactive` | `transparent` | Неактивный таб |
| `--bottom-sheet-handle` | `#3A3A3C` | Ручка боттом-шита |

### 2.2 Типографика

Шрифт: **Geist Variable** (подключён глобально через `font-[family-name:var(--font-geist)]` или `font-sans` если настроен в Tailwind).

| Элемент | Размер | Вес | Line-height | Letter-spacing |
|---------|--------|-----|-------------|----------------|
| H1 (страница) | `28px` | `600` | `1.2` | `-0.02em` |
| H2 (секция) | `22px` | `600` | `1.25` | `-0.01em` |
| H3 (карточка) | `18px` | `600` | `1.3` | `0` |
| Body | `16px` | `400` | `1.5` | `0` |
| Body Medium | `16px` | `500` | `1.5` | `0` |
| Caption | `14px` | `400` | `1.4` | `0` |
| Caption Medium | `14px` | `500` | `1.4` | `0` |
| Small | `12px` | `400` | `1.3` | `0.01em` |
| Button | `16px` | `500` | `1` | `0` |
| Large Amount | `32px` | `600` | `1.1` | `-0.02em` |

### 2.3 Спейсинг

| Токен | Значение | Использование |
|-------|----------|---------------|
| `--page-padding-x` | `16px` | Горизонтальные отступы контейнера |
| `--page-gap` | `28px` | Gap между секциями на странице |
| `--card-padding` | `16px` | Padding внутри карточки |
| `--card-gap` | `12px` | Gap между элементами внутри карточки |
| `--card-radius` | `16px` | Радиус карточки |
| `--input-radius` | `12px` | Радиус инпута |
| `--button-radius` | `12px` | Радиус кнопки |
| `--chip-radius` | `8px` | Радиус чипа/бейджа |
| `--tab-radius` | `8px` | Радиус таба (segmented) |
| `--avatar-radius` | `50%` | Аватар (круг) |
| `--item-gap` | `16px` | Gap между item-ами в списке |
| `--icon-size-sm` | `20px` | Маленькая иконка |
| `--icon-size-md` | `24px` | Средняя иконка |
| `--icon-size-lg` | `28px` | Большая иконка |

### 2.4 Иконки

- **Библиотека:** `lucide-react`
- **Размеры:** `size-5` (20px), `size-6` (24px)
- **Цвет:** `text-primary` или `text-secondary` в зависимости от контекста
- **Стиль:** Outline (не solid), stroke-width по умолчанию (2)
- **Кастомные:** Некоторые иконки (3D-иллюстрации продуктов) — это PNG/SVG ассеты, не lucide. Для MVP заменяем на `lucide` аналоги или `img`.

### 2.5 Тени / Elevations

В тёмной теме тени не используются. Вместо этого — различие через границы (`border`) и фон (`bg-card`).

---

## 3. Компоненты UI (с примерами кода)

### 3.1 PageHeader

**Назначение:** Заголовок страницы с кнопкой "Назад", названием и опциональным action.

**Props:**

```typescript
interface PageHeaderProps {
  title: string;
  backTo?: string;          // URL для navigate(-1) если не указан
  onBack?: () => void;      // Кастомный обработчик
  action?: React.ReactNode; // Иконка/кнопка справа (search, info, close)
}
```

**Код:**

```tsx
import { Button } from "@rollout/ui-kit";
import { ArrowLeft } from "lucide-react";
import { useNavigate } from "react-router-dom";

export function PageHeader({ title, backTo, onBack, action }: PageHeaderProps) {
  const navigate = useNavigate();

  const handleBack = () => {
    if (onBack) onBack();
    else if (backTo) navigate(backTo);
    else navigate(-1);
  };

  return (
    <div className="flex items-center gap-3 pt-4 pb-2">
      <Button
        variant="ghost"
        size="icon"
        onClick={handleBack}
        className="shrink-0 text-[#FAFAFA] hover:bg-[#1C1C1E]"
      >
        <ArrowLeft className="size-6" />
      </Button>
      <h1 className="flex-1 text-[22px] font-semibold leading-[1.25] tracking-[-0.01em] text-[#FAFAFA]">
        {title}
      </h1>
      {action && <div className="shrink-0">{action}</div>}
    </div>
  );
}
```

**Shadcn ref:** `Button` (variant ghost, size icon).

---

### 3.2 AccountCard

**Назначение:** Карточка счёта/карты с балансом и номером. Используется на Finance и Cards.

**Props:**

```typescript
interface AccountCardProps {
  balance: string;
  currency: string;
  name: string;
  number: string;
  type: "debit" | "credit" | "savings";
  isPrimary?: boolean;
}
```

**Код:**

```tsx
import { Card, Badge } from "@rollout/ui-kit";
import { CreditCard } from "lucide-react";

export function AccountCard({ balance, currency, name, number, isPrimary }: AccountCardProps) {
  return (
    <Card className="min-w-[260px] shrink-0 bg-[#1C1C1E] border border-[#3A3A3C] rounded-2xl p-4">
      <div className="flex flex-col gap-3">
        <div className="flex items-center justify-between">
          <span className="text-lg font-semibold text-[#FAFAFA]">
            {balance} {currency}
          </span>
          {isPrimary && (
            <Badge className="bg-[#3A3A3C] text-[#FAFAFA] text-xs px-2 py-0.5 rounded-full">
              Основной
            </Badge>
          )}
        </div>
        <div className="text-sm text-[#8E8E93]">{name}</div>
        <div className="flex items-center gap-2">
          <CreditCard className="size-5 text-[#8E8E93]" />
          <span className="text-sm font-medium text-[#FAFAFA]">•• {number}</span>
        </div>
      </div>
    </Card>
  );
}
```

**Shadcn ref:** `Card`, `Badge`.

---

### 3.3 SegmentedTabs

**Назначение:** Переключение табов (Расходы/Доходы, Валюты/Металлы и т.д.).

**Props:**

```typescript
interface SegmentedTabsProps<T extends string> {
  tabs: { value: T; label: string }[];
  value: T;
  onChange: (value: T) => void;
}
```

**Код:**

```tsx
import { Tabs, TabsList, TabsTrigger } from "@rollout/ui-kit";
import { cn } from "@rollout/ui-kit";

export function SegmentedTabs<T extends string>({ tabs, value, onChange }: SegmentedTabsProps<T>) {
  return (
    <Tabs value={value} onValueChange={(v) => onChange(v as T)} className="w-full">
      <TabsList className="grid grid-cols-2 w-full bg-[#1C1C1E] rounded-xl p-1">
        {tabs.map((tab) => (
          <TabsTrigger
            key={tab.value}
            value={tab.value}
            className={cn(
              "rounded-lg text-sm font-medium transition-colors",
              "data-[state=active]:bg-[#3A3A3C] data-[state=active]:text-[#FAFAFA]",
              "data-[state=inactive]:text-[#8E8E93] data-[state=inactive]:bg-transparent"
            )}
          >
            {tab.label}
          </TabsTrigger>
        ))}
      </TabsList>
    </Tabs>
  );
}
```

**Shadcn ref:** `Tabs`, `TabsList`, `TabsTrigger`.

---

### 3.4 CategoryItem

**Назначение:** Элемент списка категорий в аналитике (иконка + название + операции + сумма).

**Props:**

```typescript
interface CategoryItemProps {
  icon: React.ReactNode; // или React.ElementType для lucide
  iconBg: string;        // hex цвет фона иконки
  name: string;
  operations: number;
  amount: string;
  currency: string;
}
```

**Код:**

```tsx
import { Card } from "@rollout/ui-kit";

export function CategoryItem({ icon, iconBg, name, operations, amount, currency }: CategoryItemProps) {
  return (
    <div className="flex items-center gap-4 py-3">
      <div
        className="flex items-center justify-center size-10 rounded-xl shrink-0"
        style={{ backgroundColor: iconBg }}
      >
        {icon}
      </div>
      <div className="flex flex-col flex-1 min-w-0">
        <span className="text-base font-medium text-[#FAFAFA] truncate">{name}</span>
        <span className="text-sm text-[#8E8E93]">{operations} операций</span>
      </div>
      <span className="text-base font-medium text-[#FAFAFA] shrink-0">
        {amount} {currency}
      </span>
    </div>
  );
}
```

**Shadcn ref:** нет прямого аналога, кастомный компонент.

---

### 3.5 AccountListItem

**Назначение:** Элемент списка счетов (аватар + название + номер + баланс). Используется на AllAccounts, CardSelection.

**Props:**

```typescript
interface AccountListItemProps {
  avatar?: React.ReactNode; // или цвет/буква
  name: string;
  number: string;
  balance: string;
  currency: string;
  selected?: boolean;
  onClick?: () => void;
}
```

**Код:**

```tsx
import { Card, Avatar, AvatarFallback } from "@rollout/ui-kit";
import { CreditCard, Check } from "lucide-react";
import { cn } from "@rollout/ui-kit";

export function AccountListItem({ avatar, name, number, balance, currency, selected, onClick }: AccountListItemProps) {
  return (
    <Card
      onClick={onClick}
      className={cn(
        "flex items-center gap-4 p-4 rounded-2xl border transition-colors cursor-pointer",
        selected
          ? "bg-[#2C2C2E] border-[#8E8E93]"
          : "bg-[#1C1C1E] border-[#3A3A3C] hover:bg-[#2C2C2E]"
      )}
    >
      <div className="shrink-0">
        {avatar || (
          <Avatar className="size-10 bg-[#3A3A3C]">
            <AvatarFallback className="bg-[#3A3A3C] text-[#FAFAFA]">
              <CreditCard className="size-5" />
            </AvatarFallback>
          </Avatar>
        )}
      </div>
      <div className="flex flex-col flex-1 min-w-0">
        <span className="text-base font-medium text-[#FAFAFA] truncate">{name}</span>
        <span className="text-sm text-[#8E8E93]">•• {number}</span>
      </div>
      <div className="flex items-center gap-2 shrink-0">
        <span className="text-base font-medium text-[#FAFAFA]">
          {balance} {currency}
        </span>
        {selected && <Check className="size-5 text-[#FAFAFA]" />}
      </div>
    </Card>
  );
}
```

**Shadcn ref:** `Card`, `Avatar`, `AvatarFallback`.

---

### 3.6 ExchangeRateRow

**Назначение:** Строка курса валюты (флаг + код + название + покупка + продажа + тренд).

**Props:**

```typescript
interface ExchangeRateRowProps {
  flagUrl: string;   // или ReactNode с флагом
  code: string;
  name: string;
  buy: string;
  sell: string;
  trend?: "up" | "down" | "neutral";
}
```

**Код:**

```tsx
import { TrendingUp, TrendingDown, Minus } from "lucide-react";
import { Separator } from "@rollout/ui-kit";

export function ExchangeRateRow({ flagUrl, code, name, buy, sell, trend }: ExchangeRateRowProps) {
  const TrendIcon = trend === "up" ? TrendingUp : trend === "down" ? TrendingDown : Minus;
  const trendColor = trend === "up" ? "text-[#34C759]" : trend === "down" ? "text-[#FF3B30]" : "text-[#8E8E93]";

  return (
    <div className="flex flex-col">
      <div className="flex items-center gap-3 py-3">
        <div className="size-10 rounded-full overflow-hidden shrink-0 bg-[#1C1C1E]">
          <img src={flagUrl} alt={code} className="size-full object-cover" />
        </div>
        <div className="flex flex-col flex-1 min-w-0">
          <span className="text-base font-medium text-[#FAFAFA]">{code}</span>
          <span className="text-sm text-[#8E8E93]">{name}</span>
        </div>
        <div className="flex items-center gap-1 shrink-0">
          <TrendIcon className={`size-4 ${trendColor}`} />
          <span className="text-base font-medium text-[#FAFAFA]">{buy}</span>
        </div>
        <div className="flex items-center gap-1 shrink-0 w-[80px] justify-end">
          <TrendIcon className={`size-4 ${trendColor}`} />
          <span className="text-base font-medium text-[#FAFAFA]">{sell}</span>
        </div>
      </div>
      <Separator className="bg-[#3A3A3C]" />
    </div>
  );
}
```

**Shadcn ref:** `Separator`.

---

### 3.7 ProductCard

**Назначение:** Карточка финансового продукта в каталоге (2 в ряд).

**Props:**

```typescript
interface ProductCardProps {
  title: string;
  image: string; // URL 3D-иллюстрации или placeholder
  onClick?: () => void;
}
```

**Код:**

```tsx
import { Card } from "@rollout/ui-kit";
import { ArrowRight } from "lucide-react";

export function ProductCard({ title, image, onClick }: ProductCardProps) {
  return (
    <Card
      onClick={onClick}
      className="relative flex flex-col justify-between bg-[#1C1C1E] border border-[#3A3A3C] rounded-2xl p-4 min-h-[160px] cursor-pointer hover:bg-[#2C2C2E] transition-colors"
    >
      <span className="text-lg font-semibold text-[#FAFAFA] leading-tight z-10">
        {title}
      </span>
      <img
        src={image}
        alt={title}
        className="absolute bottom-2 right-2 w-20 h-20 object-contain opacity-90"
      />
    </Card>
  );
}
```

**Shadcn ref:** `Card`.

---

### 3.8 BottomSheet

**Назначение:** Нижний выезжающий панель (выбор металла, детали кредита, кошелёк).

**Props:**

```typescript
interface BottomSheetProps {
  open: boolean;
  onClose: () => void;
  title: string;
  children: React.ReactNode;
  footer?: React.ReactNode; // Кнопка внизу
}
```

**Код:**

```tsx
import { Sheet, SheetContent, SheetHeader, SheetTitle, SheetTrigger } from "@rollout/ui-kit";
import { Button } from "@rollout/ui-kit";

export function BottomSheet({ open, onClose, title, children, footer }: BottomSheetProps) {
  return (
    <Sheet open={open} onOpenChange={(v) => !v && onClose()}>
      <SheetContent
        side="bottom"
        className="bg-[#1C1C1E] border-t border-[#3A3A3C] rounded-t-3xl px-4 pb-6 pt-2 max-w-[576px] mx-auto"
      >
        {/* Handle */}
        <div className="flex justify-center pt-2 pb-4">
          <div className="w-10 h-1 bg-[#3A3A3C] rounded-full" />
        </div>
        <SheetHeader className="pb-4">
          <SheetTitle className="text-xl font-semibold text-[#FAFAFA] text-left">
            {title}
          </SheetTitle>
        </SheetHeader>
        <div className="flex flex-col gap-4">
          {children}
        </div>
        {footer && (
          <div className="mt-6">
            {footer}
          </div>
        )}
      </SheetContent>
    </Sheet>
  );
}
```

**Shadcn ref:** `Sheet`, `SheetContent`, `SheetHeader`, `SheetTitle`.

---

### 3.9 FormField

**Назначение:** Поле ввода с лейблом (для форм заявок, настроек).

**Props:**

```typescript
interface FormFieldProps {
  label: string;
  value: string;
  placeholder?: string;
  readOnly?: boolean;
  onChange?: (value: string) => void;
  onClick?: () => void;
  suffix?: React.ReactNode; // ChevronDown, Calendar и т.д.
}
```

**Код:**

```tsx
import { Input, Field } from "@rollout/ui-kit";
import { ChevronDown } from "lucide-react";

export function FormField({ label, value, placeholder, readOnly, onChange, onClick, suffix }: FormFieldProps) {
  return (
    <Field label={label} className="flex flex-col gap-2">
      <div
        onClick={onClick}
        className="relative flex items-center bg-[#1C1C1E] border border-[#3A3A3C] rounded-xl px-4 py-3 cursor-pointer"
      >
        <Input
          value={value}
          placeholder={placeholder}
          readOnly={readOnly}
          onChange={(e) => onChange?.(e.target.value)}
          className="bg-transparent border-0 p-0 text-base text-[#FAFAFA] placeholder:text-[#636366] focus-visible:ring-0 focus-visible:ring-offset-0"
        />
        {suffix && (
          <div className="ml-2 shrink-0 text-[#8E8E93]">
            {suffix}
          </div>
        )}
      </div>
    </Field>
  );
}
```

**Shadcn ref:** `Input`, `Field`.

---

### 3.10 ChipGroup

**Назначение:** Горизонтальная группа чипов (годы 2026, 2025, 2024...).

**Props:**

```typescript
interface ChipGroupProps {
  options: string[];
  value: string;
  onChange: (value: string) => void;
}
```

**Код:**

```tsx
import { cn } from "@rollout/ui-kit";

export function ChipGroup({ options, value, onChange }: ChipGroupProps) {
  return (
    <div className="flex flex-wrap gap-2">
      {options.map((opt) => {
        const isActive = opt === value;
        return (
          <button
            key={opt}
            onClick={() => onChange(opt)}
            className={cn(
              "px-4 py-2 rounded-lg border text-sm font-medium transition-colors",
              isActive
                ? "bg-[#2C2C2E] border-[#FAFAFA] text-[#FAFAFA]"
                : "bg-transparent border-[#3A3A3C] text-[#FAFAFA] hover:bg-[#1C1C1E]"
            )}
          >
            {opt}
          </button>
        );
      })}
    </div>
  );
}
```

**Shadcn ref:** нет прямого аналога, кастомный.

---

### 3.11 InfoBlock

**Назначение:** Блок с иконкой и текстом (подсказки, предупреждения).

**Props:**

```typescript
interface InfoBlockProps {
  icon: React.ReactNode;
  title: string;
  description: string;
  action?: { label: string; onClick: () => void };
}
```

**Код:**

```tsx
export function InfoBlock({ icon, title, description, action }: InfoBlockProps) {
  return (
    <div className="flex items-start gap-3 py-2">
      <div className="shrink-0 mt-0.5 text-[#8E8E93]">{icon}</div>
      <div className="flex flex-col gap-1">
        <span className="text-sm text-[#FAFAFA]">{title}</span>
        <span className="text-sm text-[#8E8E93]">{description}</span>
        {action && (
          <button
            onClick={action.onClick}
            className="text-sm text-[#8E8E93] underline underline-offset-2 text-left mt-1"
          >
            {action.label}
          </button>
        )}
      </div>
    </div>
  );
}
```

---

### 3.12 Footer

**Назначение:** Футер на всех страницах (© 2025 Rollout + социальные иконки).

**Код:**

```tsx
import { Separator } from "@rollout/ui-kit";

export function Footer() {
  return (
    <footer className="mt-auto pt-8 pb-6">
      <Separator className="bg-[#3A3A3C] mb-4" />
      <div className="flex items-center justify-between">
        <span className="text-sm text-[#8E8E93]">© 2025 Rollout</span>
        <div className="flex items-center gap-4">
          {/* X (Twitter) */}
          <a href="https://x.com" target="_blank" rel="noopener noreferrer" className="text-[#8E8E93] hover:text-[#FAFAFA]">
            <svg className="size-5" fill="currentColor" viewBox="0 0 24 24"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>
          </a>
          {/* LinkedIn */}
          <a href="https://linkedin.com" target="_blank" rel="noopener noreferrer" className="text-[#8E8E93] hover:text-[#FAFAFA]">
            <svg className="size-5" fill="currentColor" viewBox="0 0 24 24"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 01-2.063-2.065 2.064 2.064 0 112.063 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>
          </a>
          {/* Telegram */}
          <a href="https://telegram.org" target="_blank" rel="noopener noreferrer" className="text-[#8E8E93] hover:text-[#FAFAFA]">
            <svg className="size-5" fill="currentColor" viewBox="0 0 24 24"><path d="M11.944 0A12 12 0 0 0 0 12a12 12 0 0 0 12 12 12 12 0 0 0 12-12A12 12 0 0 0 12 0a12 12 0 0 0-.056 0zm4.962 7.224c.1-.002.321.023.465.14a.506.506 0 0 1 .171.325c.016.093.036.306.02.472-.18 1.898-.962 6.502-1.36 8.627-.168.9-.499 1.201-.82 1.23-.696.065-1.225-.46-1.9-.902-1.056-.693-1.653-1.124-2.678-1.8-1.185-.78-.417-1.21.258-1.91.177-.184 3.247-2.977 3.307-3.23.007-.032.014-.15-.056-.212s-.174-.041-.249-.024c-.106.024-1.793 1.14-5.061 3.345-.48.33-.913.49-1.3.48-.428-.008-1.252-.241-1.865-.44-.752-.245-1.349-.374-1.297-.789.027-.216.325-.437.893-.663 3.498-1.524 5.83-2.529 6.998-3.014 3.332-1.386 4.025-1.627 4.476-1.635z"/></svg>
          </a>
        </div>
      </div>
    </footer>
  );
}
```

**Shadcn ref:** `Separator`.

---

## 4. Структура каждого экрана

### 4.1 Finance (Главный экран)

**Скриншот:** `Finance_0.png`

**Layout:**
```
<Container max-w-[576px] mx-auto flex flex-col gap-7 pt-20 pb-8>
  <Logo />
  
  <Section>
    <Label text-secondary>Общая сумма</Label>
    <LargeAmount>3 675,89 ₽</LargeAmount>
  </Section>
  
  <Section>
    <HorizontalScroll gap-3>
      <AccountCard balance="3 675,89" currency="₽" name="Debit Card" number="3273" />
      <AccountCard balance="3 675,89" currency="₽" name="Debit Card" number="2371" />
    </HorizontalScroll>
  </Section>
  
  <Footer />
</Container>
```

**Компоненты сверху вниз:**
1. Logo (Z-образный логотип Rollout)
2. Label + LargeAmount (общая сумма)
3. HorizontalScroll с AccountCard
4. Footer

**Состояния:**
- `loading` — скелетоны карточек
- `error` — пустой экран с retry
- `empty` — нет счетов, кнопка "Добавить счёт"
- `success` — отображение данных

**Интерактивность:**
- `onClick` карточки → переход на AccountDetails
- Свайп карточек влево/вправо (horizontal scroll)

---

### 4.2 Analytics (Аналитика финансов)

**Скриншот:** `Analytics_0.png`

**Layout:**
```
<Container>
  <PageHeader title="Аналитика финансов" action={<SearchIcon />} />
  
  <SegmentedTabs tabs={['Расходы', 'Доходы']} />
  
  <FilterRow>
    <Button variant="outline" size="sm"><Calendar /> Период</Button>
    <Select placeholder="Счета и карты" />
    <Button variant="outline" size="sm">Неделя</Button>
  </FilterRow>
  
  <ChartToggleRow>
    <IconButton icon={<PieChart />} active />
    <IconButton icon={<BarChart3 />} />
  </ChartToggleRow>
  
  <DonutChart>
    <CenterLabel>655 345 ₽<br/>Расходы</CenterLabel>
  </DonutChart>
  
  <Section>
    <H2>Категории</H2>
    <CategoryItem icon={<Pill />} iconBg="#FF3B30" name="Аптеки" operations={14} amount="58 888" />
    <CategoryItem icon={<Plane />} iconBg="#FF9500" name="Путешествия и туры" ... />
    <CategoryItem icon={<Ticket />} iconBg="#5E5CE6" name="Авиабилеты" ... />
    <CategoryItem icon={<Utensils />} iconBg="#FFCC00" name="Доставка еды" ... />
    <CategoryItem icon={<ShoppingCart />} iconBg="#FF9500" name="Супермаркеты" ... />
  </Section>
  
  <Footer />
</Container>
```

**Состояния:**
- `loading` — скелетон донат-чарта и списка
- `empty` — нет операций, показать EmptyState
- `error` — toast + retry

**Интерактивность:**
- Переключение табов Расходы/Доходы → перестроение графика
- `onClick` категории → детализация по категории
- Фильтры по периоду и счетам

---

### 4.3 AllAccounts (Все счета)

**Скриншот:** `AllAccounts_0.png`

**Layout:**
```
<Container>
  <PageHeader title="Label" />
  
  <SectionLabel>Label</SectionLabel>
  <AccountListItem name="Main account" number="9089" balance="3 675,89" />
  ... (повтор)
  
  <SectionLabel>Label</SectionLabel>
  <AccountListItem name="Main account" number="9089" balance="3 675,89" />
  ... (повтор)
  
  <Button className="w-full">Добавить новый <Plus /></Button>
  
  <Footer />
</Container>
```

**Состояния:**
- `loading` — скелетон списка
- `empty` — нет счетов
- `success` — список с группировкой

**Интерактивность:**
- `onClick` item → AccountDetails
- `onClick` "Добавить новый" → AccountOpening

---

### 4.4 Cards (Список карт стопкой)

**Скриншот:** `Cards_0.png`

**Layout:**
```
<Container>
  <Logo />
  
  <StackedCards>
    <CardItem name="Debit Card · 9089" balance="$135,000" />
    <CardItem name="Debit Card · 9089" balance="$135,000" />
    <CardItem name="Debit Card · 9089" balance="$135,000" />
    <CardItem name="Debit Card · 9089" balance="$135,000" />
    <CardItem name="Debit Card · 9089" balance="$135,000" />
  </StackedCards>
  
  <Footer />
</Container>
```

**Особенности:**
- Карты расположены стопкой (stack) с отрицательным margin-top
- Каждая следующая карта имеет меньший z-index и более светлый фон (#2C2C2E → #3A3A3C → #48484A → #636366 → #8E8E93)
- `border-radius: 24px` для верхних углов

---

### 4.5 Pay / QrCode (QR-код для переводов)

**Скриншот:** `QrCode_0.png` (dark), `Pay_0.png` (light)

**Layout:**
```
<Container>
  <PageHeader title="QR-код" />
  
  <Section className="text-center">
    <Body>Поделитесь QR-кодом для получения переводов</Body>
    <QRCode value="..." size={240} className="mx-auto my-6" />
    
    <Label text-secondary>Телефон, который увидит отправитель</Label>
    <Select value="+7 927 007-75-88" className="mt-2" />
    
    <InfoBlock
      icon={<CreditCard />}
      title="Чтобы перевод поступил именно на эту карту, установите её как основную."
      action={{ label: "Выбрать основную карту", onClick: () => navigate('/finance/pay/card-select') }}
    />
  </Section>
  
  <Button className="w-full">Поделитесь QR-кодом</Button>
  
  <Footer />
</Container>
```

**Состояния:**
- `loading` — генерация QR
- `error` — не удалось загрузить QR

**Интерактивность:**
- `onClick` "Поделитесь QR-кодом" → ShareQrCode (share API)
- `onClick` Select → PhoneNumber (bottom sheet)
- `onClick` "Выбрать основную карту" → CardSelection

---

### 4.6 ExchangeRate (Курсы валют)

**Скриншот:** `ExchangeRate_0.png`

**Layout:**
```
<Container>
  <Section className="flex items-center justify-between">
    <H1>Курсы валют</H1>
    <ArrowRight className="size-6" />
  </Section>
  
  <SegmentedTabs tabs={['Валюты', 'Металлы']} />
  
  <HeaderRow>
    <span className="flex-1" />
    <span className="w-[80px] text-right text-secondary">Купить</span>
    <span className="w-[80px] text-right text-secondary">Продать</span>
  </HeaderRow>
  
  <ExchangeRateRow flag="🇨🇳" code="CNY" name="Юань" buy="11,30" sell="11,03" trend="neutral" />
  <ExchangeRateRow flag="🇺🇸" code="USD" name="Доллар" buy="79,40" sell="73,80" trend="neutral" />
  <ExchangeRateRow flag="🇦🇪" code="AED" name="Дирхам" buy="21,94" sell="20,74" trend="up" />
  <ExchangeRateRow flag="🇰🇿" code="100 KZT" name="Тенге" buy="12,98" sell="14,82" trend="down" />
  
  <Button variant="ghost" className="text-[#FAFAFA]">
    Ещё <ChevronDown />
  </Button>
  
  <Footer />
</Container>
```

**Состояния:**
- `loading` — скелетон строк
- `error` — retry
- `empty` — нет данных

**Интерактивность:**
- `onClick` "Ещё" → ExchangeRateFull (полный список)
- `onClick` строка → детали валюты/покупка

---

### 4.7 FinancialCatalog (Финансовые продукты)

**Скриншот:** `FinancialCatalog_0.png`

**Layout:**
```
<Container>
  <PageHeader title="Финансовые продукты" action={<SearchIcon />} />
  
  <Grid cols-2 gap-3>
    <ProductCard title="Микро\nкредит" image="/assets/micro-loan.png" />
    <ProductCard title="Ипотека" image="/assets/mortgage.png" />
    <ProductCard title="Кредиты" image="/assets/loans.png" />
    <ProductCard title="Кредитные\nкарты" image="/assets/credit-cards.png" />
    <ProductCard title="Рассрочка" image="/assets/installment.png" />
    <ProductCard title="Вклады" image="/assets/deposits.png" />
    <ProductCard title="ОСАГО" image="/assets/osago.png" />
    <ProductCard title="Страхование\nипотеки" image="/assets/mortgage-insurance.png" />
    <ProductCard title="Кредит\nна авто" image="/assets/car-loan.png" />
    <ProductCard title="Все продукты" image={null} withArrow />
  </Grid>
  
  <Footer />
</Container>
```

**Состояния:**
- `loading` — скелетон карточек
- `error` — retry

**Интерактивность:**
- `onClick` карточка → FinancialOnboarding / FinancialForm
- `onClick` "Все продукты" → полный каталог

---

### 4.8 FundsMovement (Заказ справки)

**Скриншот:** `FundsMovement_0.png`

**Layout:**
```
<Container>
  <PageHeader title="Заказ справки" />
  
  <H2>Справка для госслужащих</H2>
  
  <SectionLabel>Отчётный период</SectionLabel>
  <ChipGroup options={['2026', '2025', '2024', '2023']} value={period} onChange={setPeriod} />
  <Button variant="outline">Другой период</Button>
  
  <FormField
    label="Отчётная дата"
    value="1 января 2026"
    readOnly
    onClick={() => openCalendar()}
    suffix={<ChevronDown />}
  />
  
  <Button className="w-full mt-auto">Заказать</Button>
  
  <Footer />
</Container>
```

**Состояния:**
- `idle` — форма не заполнена
- `filled` — форма заполнена, кнопка активна
- `submitting` — лоадер на кнопке
- `success` — bottom sheet с подтверждением
- `error` — toast ошибка

**Интерактивность:**
- `onClick` чип → выбор года
- `onClick` "Другой период" → открыть range picker
- `onClick` поле даты → Calendar bottom sheet
- `onSubmit` → mock API call

---

### 4.9 PaymentProcessStep1 (Оплата штрафов ГАИ)

**Скриншот:** `PaymentProcessStep1_1.png`

**Layout:**
```
<Container>
  <PageHeader title="Штрафы ГАИ" />
  
  <AccountListItem
    icon={<div className="size-10 rounded-lg bg-[#1C1C1E]" />}
    name="По СТС и госномеру"
    description="Поиск штрафов с камер"
    withArrow
  />
  
  <AccountListItem
    name="Поиск по водительским правам"
    description="Поиск штрафов от сотрудников ГАИ"
    withArrow
  />
  
  <AccountListItem
    name="По номеру постановления (УИН)"
    description="Оплата квитанции"
    withArrow
  />
  
  <Button className="w-full">Продолжить</Button>
  
  <Footer />
</Container>
```

**Состояния:**
- `idle` — ничего не выбрано, кнопка disabled
- `selected` — выбран пункт, кнопка активна
- `submitting` — лоадер

---

### 4.10 BuyCurrency (Покупка валюты)

**Скриншот:** `BuyCurrency_0.png`

**Layout:**
```
<Container>
  <PageHeader title="Купить валюту" action={<InfoIcon />} />
  
  <div className="grid grid-cols-2 gap-3">
    <Card className="p-4">
      <Label text-secondary>Купить</Label>
      <AmountInput value={buyAmount} onChange={setBuyAmount} suffix="$" />
    </Card>
    <Card className="p-4">
      <Label text-secondary>Списать</Label>
      <AmountInput value={sellAmount} onChange={setSellAmount} suffix="₽" />
    </Card>
  </div>
  
  <Card className="p-4">
    <div className="flex justify-between items-center">
      <div>
        <Label text-secondary>Откуда</Label>
        <div className="text-[#FAFAFA]">Сберегательный счёт ••2539</div>
      </div>
      <div className="text-[#FAFAFA]">20 450,70 ₽</div>
    </div>
  </Card>
  
  <div className="text-sm text-[#8E8E93]">
    1 $ = 80,40 ₽
  </div>
  
  <Button className="w-full">Перевести {buyAmount} $</Button>
  
  <Footer />
</Container>
```

**Состояния:**
- `idle` — сумма 0, кнопка disabled
- `filled` — сумма > 0, кнопка активна
- `insufficient` — сумма > баланса, кнопка disabled + error
- `submitting` — лоадер

---

### 4.11 CurrencyAlert (Уведомление)

**Скриншот:** `CurrencyAlert_0.png`

**Layout:**
```
<Container>
  <PageHeader title="Курсы валют" action={<InfoIcon />} />
  
  <Card className="flex flex-col items-center text-center gap-4 p-6 mt-4">
    <H2>Нет счёта в металле</H2>
    <Body text-secondary>
      Для совершения операции необходимо открыть счёт в металле. Это бесплатно
    </Body>
    <Button className="w-full" onClick={() => navigate('/finance/exchange/open')}>
      Открыть счёт
    </Button>
  </Card>
  
  <Footer />
</Container>
```

---

### 4.12 MetalSelection (Bottom Sheet)

**Скриншот:** `MetalSelection_0.png`

**Layout:**
```
<BottomSheet open={open} onClose={onClose} title="Выберите металл">
  <div className="flex flex-col">
    <div className="flex items-center justify-between py-3">
      <div className="flex items-center gap-3">
        <WeightIcon className="size-6 text-[#FAFAFA]" />
        <span className="text-base text-[#FAFAFA]">Золото</span>
      </div>
      <span className="text-sm text-[#8E8E93]">Au</span>
    </div>
    <Separator />
    <div className="flex items-center justify-between py-3">
      <div className="flex items-center gap-3">
        <WeightIcon className="size-6 text-[#FAFAFA]" />
        <span className="text-base text-[#FAFAFA]">Серебро</span>
      </div>
      <span className="text-sm text-[#8E8E93]">Ag</span>
    </div>
    <Separator />
    ... (Платина, Палладий)
  </div>
</BottomSheet>
```

---

## 5. Пример полной страницы

### FinancePage.tsx

```tsx
import { useState } from "react";
import { useNavigate } from "react-router-dom";
import { Button, Card, Badge, Tabs, TabsList, TabsTrigger } from "@rollout/ui-kit";
import { ArrowLeft, CreditCard, Plus } from "lucide-react";
import { cn } from "@rollout/ui-kit";

// --- Компоненты ---
function PageHeader({ title }: { title: string }) {
  const navigate = useNavigate();
  return (
    <div className="flex items-center gap-3 pt-4 pb-2">
      <Button variant="ghost" size="icon" onClick={() => navigate(-1)} className="shrink-0 text-[#FAFAFA] hover:bg-[#1C1C1E]">
        <ArrowLeft className="size-6" />
      </Button>
      <h1 className="flex-1 text-[22px] font-semibold leading-[1.25] tracking-[-0.01em] text-[#FAFAFA]">
        {title}
      </h1>
    </div>
  );
}

function AccountCard({ balance, currency, name, number }: { balance: string; currency: string; name: string; number: string }) {
  return (
    <Card className="min-w-[260px] shrink-0 bg-[#1C1C1E] border border-[#3A3A3C] rounded-2xl p-4">
      <div className="flex flex-col gap-3">
        <div className="flex items-center justify-between">
          <span className="text-lg font-semibold text-[#FAFAFA]">{balance} {currency}</span>
        </div>
        <div className="text-sm text-[#8E8E93]">{name}</div>
        <div className="flex items-center gap-2">
          <CreditCard className="size-5 text-[#8E8E93]" />
          <span className="text-sm font-medium text-[#FAFAFA]">•• {number}</span>
        </div>
      </div>
    </Card>
  );
}

function Footer() {
  return (
    <footer className="mt-auto pt-8 pb-6">
      <div className="h-px bg-[#3A3A3C] mb-4" />
      <div className="flex items-center justify-between">
        <span className="text-sm text-[#8E8E93]">© 2025 Rollout</span>
        <div className="flex items-center gap-4 text-[#8E8E93]">
          {/* X */}
          <svg className="size-5" fill="currentColor" viewBox="0 0 24 24"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>
          {/* LinkedIn */}
          <svg className="size-5" fill="currentColor" viewBox="0 0 24 24"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 01-2.063-2.065 2.064 2.064 0 112.063 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>
          {/* Telegram */}
          <svg className="size-5" fill="currentColor" viewBox="0 0 24 24"><path d="M11.944 0A12 12 0 0 0 0 12a12 12 0 0 0 12 12 12 12 0 0 0 12-12A12 12 0 0 0 12 0a12 12 0 0 0-.056 0zm4.962 7.224c.1-.002.321.023.465.14a.506.506 0 0 1 .171.325c.016.093.036.306.02.472-.18 1.898-.962 6.502-1.36 8.627-.168.9-.499 1.201-.82 1.23-.696.065-1.225-.46-1.9-.902-1.056-.693-1.653-1.124-2.678-1.8-1.185-.78-.417-1.21.258-1.91.177-.184 3.247-2.977 3.307-3.23.007-.032.014-.15-.056-.212s-.174-.041-.249-.024c-.106.024-1.793 1.14-5.061 3.345-.48.33-.913.49-1.3.48-.428-.008-1.252-.241-1.865-.44-.752-.245-1.349-.374-1.297-.789.027-.216.325-.437.893-.663 3.498-1.524 5.83-2.529 6.998-3.014 3.332-1.386 4.025-1.627 4.476-1.635z"/></svg>
        </div>
      </div>
    </footer>
  );
}

// --- Страница ---
export default function FinancePage() {
  const navigate = useNavigate();
  const [accounts] = useState([
    { id: "1", balance: "3 675,89", currency: "₽", name: "Debit Card", number: "3273" },
    { id: "2", balance: "3 675,89", currency: "₽", name: "Debit Card", number: "2371" },
  ]);

  const totalBalance = "3 675,89";

  return (
    <div className="mx-auto flex max-w-[576px] flex-col gap-7 pt-20 pb-8 px-4 min-h-screen bg-[#0A0A0A]">
      {/* Logo */}
      <div className="flex items-center">
        <svg className="size-8 text-[#FAFAFA]" viewBox="0 0 32 32" fill="currentColor">
          <path d="M4 4h10v10H4zM18 4h10v10H18zM4 18h10v10H4zM18 18h10v10H18z" />
        </svg>
      </div>

      {/* Total Balance */}
      <section className="flex flex-col gap-1">
        <span className="text-sm text-[#8E8E93]">Общая сумма</span>
        <span className="text-[32px] font-semibold leading-[1.1] tracking-[-0.02em] text-[#FAFAFA]">
          {totalBalance} ₽
        </span>
      </section>

      {/* Cards Horizontal Scroll */}
      <section className="flex gap-3 overflow-x-auto -mx-4 px-4 pb-2 scrollbar-hide">
        {accounts.map((acc) => (
          <div key={acc.id} onClick={() => navigate(`/finance/account/${acc.id}`)} className="cursor-pointer">
            <AccountCard {...acc} />
          </div>
        ))}
      </section>

      <Footer />
    </div>
  );
}
```

---

## 6. Чек-лист вёрстки

- [ ] **Все цвета через токены** — не хардкод. Использовать CSS-переменные или константы в теме.
- [ ] **Типографика совпадает с макетом** — Geist Variable, размеры, веса, line-height по спецификации §2.2.
- [ ] **Отступы и радиусы по дизайн-системе** — padding, gap, radius по §2.3.
- [ ] **Иконки из lucide-react** — `size-5` (20px) или `size-6` (24px). Не кастомные SVG без согласования.
- [ ] **Контейнер** — `mx-auto flex max-w-[576px] flex-col gap-7 pt-20 pb-8 px-4` на каждой странице.
- [ ] **pt-20** — отступ от шапки (header приложения фиксированный ~80px).
- [ ] **pb-tabbar md:pb-0** — на мобильном добавить отступ для таб-бара (~64px), на десктопе убрать.
- [ ] **Все формы на локальном useState** — без подключения к API, mock-данные в компоненте.
- [ ] **Mock-обработчики без console.log** — использовать `() => {}` или `alert()` для демо, не оставлять `console.log`.
- [ ] **Route добавлен в App.tsx** — каждая страница зарегистрирована в роутере с правильным path.
- [ ] **Скриншот соответствует макету** — визуальный diff: фон, цвета, шрифты, отступы, скругления.
- [ ] **Не использовать radix-ui напрямую** — всё импортировать из `@rollout/ui-kit`.
- [ ] **Не использовать `shadcn add`** — компоненты уже доступны через `@rollout/ui-kit`.
- [ ] **Импортировать всё из `@rollout/ui-kit`** — не deep imports (`import { Button } from "@rollout/ui-kit"`).
- [ ] **Футер на всех страницах** — © 2025 Rollout + X, LinkedIn, Telegram.
- [ ] **BottomSheet с handle** — серая полоска `w-10 h-1 bg-[#3A3A3C] rounded-full` сверху.
- [ ] **Primary Button** — `bg-[#E5E5E5] text-[#0A0A0A]`, rounded-xl, font-medium.
- [ ] **Secondary Button / Outline** — `bg-transparent border-[#3A3A3C] text-[#FAFAFA]`.
- [ ] **Input Field** — `bg-[#1C1C1E] border-[#3A3A3C] rounded-xl`, placeholder `text-[#636366]`.
- [ ] **Card** — `bg-[#1C1C1E] border-[#3A3A3C] rounded-2xl`.
- [ ] **Tabs** — segmented, `bg-[#1C1C1E]`, active `bg-[#3A3A3C]`.
- [ ] **Avatar** — круглый, `bg-[#3A3A3C]`, fallback с иконкой.
- [ ] **Separator** — `bg-[#3A3A3C]`, тонкая линия между item-ами.
- [ ] **Scroll** — `overflow-x-auto` для карточек, `overflow-y-auto` для списков, скрыть скроллбар если нужно.
- [ ] **Dark theme only** — не реализовывать light mode, тёмная тема по умолчанию.
- [ ] **Tailwind v4** — использовать современный синтаксис (`bg-[#0A0A0A]` вместо `bg-background`).
- [ ] **Geist Variable** — шрифт подключён глобально, использовать `font-sans`.

---

*Инструкция составлена на основе анализа Figma-макетов модуля 11-Finance.*
*108 скриншотов загружено и проанализировано.*
*Финальный файл: `/Users/mokoloskov/Documents/kimi/workspace/11-Finance-instruction.md`*
