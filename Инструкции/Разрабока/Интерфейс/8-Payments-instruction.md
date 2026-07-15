# Инструкция по вёрстке модуля 8-Payments

> **Стек:** Node 22, pnpm 10.32.1, Vite 8, Tailwind v4, `@base-ui/react`, `@rollout/ui-kit`, `react-router-dom 6`, `lucide-react`, Geist Variable  
> **Тема:** тёмная по умолчанию  
> **Контейнер:** `mx-auto flex max-w-[576px] flex-col gap-7 pt-20 pb-8`  
> **Дата:** 2025-07-13

---

## 1. Обзор модуля

### Назначение
Модуль **8-Payments** отвечает за всё, что связано с платежами в приложении Rollout: выбор способа оплаты, привязка карт, оплата через СБП, переводы по реквизитам, управление картами (блокировка, лимиты, ПИН, перевыпуск), заказ новых карт и доставка.

### Экраны (страницы из Figma)

| # | Страница Figma | Ключевые фреймы | Назначение |
|---|----------------|-----------------|------------|
| 1 | **PaymentPage** ✅ | layout ×2 | Выбор способа оплаты (кошелёк, банки, карты, СБП) |
| 2 | **PaymentPageSingleChoose** ✅ | layout | Форма платежа с одной выбранной картой |
| 3 | **CardForm** ✅ | layout ×2 | Добавление новой карты (номер, срок, CVC) |
| 4 | **BankList** ✅ | layout | Список банков для выбора через СБП |
| 5 | **M2M** ✅ | layout | Статус запроса в банк (Merchant-to-Merchant) |
| 6 | **C2C** ✅ | layout | Ожидание пополнения через СБП (C2C) |
| 7 | **3Ds** ✅ | layout | 3D Secure: ввод SMS-кода |
| 8 | **Polling** ✅ | layout | Пolling-экран ожидания ответа банка |
| 9 | **Error** ✅ | Status | Экран ошибки (не удалось загрузить список банков) |
| 10 | **Success** ✅ | Status | Успешный перевод/возврат |
| 11 | **PaymentServices** | layout ×19 | Оплата по реквизитам (M2M перевод) |
| 12 | **Card Details** | layout | Информация о карте, заморозка |
| 13 | **CardTariffsandConditions** | CardTariffsandConditions | Тарифы и условия обслуживания |
| 14 | **ContactlessPayment** | CardContactlessPayment | Подключение RolloutPay (Mir Pay) |
| 15 | **СardBlock** | CardBlocked, CardBlockedSuccess | Блокировка карты |
| 16 | **ReplaceСard** | CardReplace, SuccessScreen | Перевыпуск карты |
| 17 | **CardPinCode** | CreatePIN, EnterPIN | Создание/смена ПИН-кода |
| 18 | **CardLimits** | CardLimitsMain, CardLimitsSet | Управление лимитами карты |
| 19 | **RegistrationNewCard** | Select card to order, Order card | Выбор и заказ новой карты |
| 20 | **СardAllProducts** | CardAllProduct | Выбор типа продукта (дебетовая, кредитная, детская, счёт) |
| 21 | **CardForSomeone** | CardForSomeone | Для кого оформить карту (себе/другому) |
| 22 | **CardDebit** | layout, Tabs | Заказ дебетовой карты (Premium/Base) |
| 23 | **CardDesign** | layout | Выбор дизайна и цвета карты |
| 24 | **CardSearch** ⚙️ | DelivryStepSearch, DeliveryStepMapContent | Поиск пункта выдачи на карте |
| 25 | **CardDeliveryDetails** | layout ×3, DeliveryDate | Детали доставки (дата, время, адрес) |
| 26 | **CardDelivery** | layout | Выбор способа доставки |
| 27 | **CardСonfirmation** | СonfirmationContent/view2 | Подтверждение заказа карты |
| 28 | **CardSuccess** ✅ | CardSuccess | Успешное оформление карты |
| 29 | **BNPL** | layout | Buy Now Pay Later (слот для контента) |

### Роуты (URL-пути)

```ts
const PAYMENT_ROUTES = {
  // Основные платежи
  paymentPage: '/payments',
  paymentSingle: '/payments/single',
  cardForm: '/payments/card/new',
  bankList: '/payments/banks',
  
  // Статусы платежа
  paymentM2M: '/payments/m2m',
  paymentC2C: '/payments/c2c',
  payment3Ds: '/payments/3ds',
  paymentPolling: '/payments/polling',
  paymentError: '/payments/error',
  paymentSuccess: '/payments/success',
  
  // Оплата по реквизитам
  paymentServices: '/payments/services',
  
  // Управление картой
  cardDetails: '/card/:id',
  cardTariffs: '/card/:id/tariffs',
  cardContactless: '/card/:id/contactless',
  cardBlock: '/card/:id/block',
  cardReplace: '/card/:id/replace',
  cardPin: '/card/:id/pin',
  cardLimits: '/card/:id/limits',
  
  // Заказ новой карты
  registrationNewCard: '/cards/order',
  cardAllProducts: '/cards/products',
  cardForSomeone: '/cards/for-someone',
  cardDebit: '/cards/debit',
  cardDesign: '/cards/design',
  cardSearch: '/cards/delivery/search',
  cardDeliveryDetails: '/cards/delivery/details',
  cardDelivery: '/cards/delivery',
  cardConfirmation: '/cards/confirmation',
  cardSuccess: '/cards/success',
  
  // BNPL
  bnpl: '/bnpl',
};
```

---

## 2. Дизайн-система (из макетов)

### 2.1. Цвета

| Токен | Значение | Использование |
|-------|----------|---------------|
| `--bg-primary` | `#0A0A0A` | Фон всей страницы |
| `--bg-card` | `#1C1C1E` | Фон карточек, инпутов, шторок |
| `--bg-card-hover` | `#2C2C2E` | Hover на карточках |
| `--border-card` | `#3A3A3C` | Бордер карточек, разделителей |
| `--text-primary` | `#FAFAFA` | Основной текст, заголовки |
| `--text-secondary` | `#8E8E93` | Подписи, описания, placeholder |
| `--text-muted` | `#636366` | Disabled, placeholder в инпутах |
| `--button-primary-bg` | `#E5E5E5` | Primary кнопка (светло-серый) |
| `--button-primary-text` | `#0A0A0A` | Текст primary кнопки |
| `--button-secondary-bg` | `transparent` | Secondary кнопка |
| `--button-secondary-border` | `#3A3A3C` | Бордер secondary кнопки |
| `--success` | `#34C759` | Success-статус, галочка |
| `--error` | `#FF3B30` | Error-статус, крестик |
| `--accent` | `#007AFF` | Ссылки, активные элементы |
| `--tabs-bg` | `#1C1C1E` | Фон табов |
| `--tabs-active` | `#3A3A3C` | Активный таб |
| `--slider-track` | `#3A3A3C` | Трек слайдера |
| `--slider-fill` | `#E5E5E5` | Заполнение слайдера |
| `--badge-bg` | `#3A3A3C` | Фон бейджа |
| `--overlay` | `rgba(0,0,0,0.7)` | Оверлей под шторками |

**Важно:** не хардкодить цвета, использовать CSS-переменные или Tailwind через `theme()`.

### 2.2. Типографика

| Элемент | Размер | Вес | Line-height | Letter-spacing |
|---------|--------|-----|-------------|----------------|
| H1 (заголовок экрана) | 28px | 600 | 1.2 | -0.02em |
| H2 (раздел) | 22px | 600 | 1.3 | -0.01em |
| H3 (подзаголовок) | 20px | 600 | 1.3 | -0.01em |
| Body | 17px | 400 | 1.5 | 0 |
| Body Medium | 17px | 500 | 1.5 | 0 |
| Caption | 15px | 400 | 1.4 | 0 |
| Caption Medium | 15px | 500 | 1.4 | 0 |
| Button | 17px | 500 | 1.2 | 0 |
| Small | 13px | 400 | 1.4 | 0.01em |
| Amount/Big Number | 34px | 700 | 1.1 | -0.02em |
| Pin Code | 28px | 400 | 1.2 | 0.1em |

```css
/* Tailwind примеры */
.text-h1 { font-size: 28px; font-weight: 600; line-height: 1.2; letter-spacing: -0.02em; }
.text-h2 { font-size: 22px; font-weight: 600; line-height: 1.3; letter-spacing: -0.01em; }
.text-body { font-size: 17px; font-weight: 400; line-height: 1.5; }
.text-caption { font-size: 15px; font-weight: 400; line-height: 1.4; }
.text-button { font-size: 17px; font-weight: 500; line-height: 1.2; }
.text-amount { font-size: 34px; font-weight: 700; line-height: 1.1; letter-spacing: -0.02em; }
```

### 2.3. Спейсинг

| Токен | Значение | Использование |
|-------|----------|---------------|
| `--gap-xs` | 4px | Минимальные отступы |
| `--gap-sm` | 8px | Отступы внутри компонентов |
| `--gap-md` | 12px | Между элементами в списке |
| `--gap-lg` | 16px | Padding внутри карточек |
| `--gap-xl` | 20px | Между секциями |
| `--gap-2xl` | 24px | Большие отступы |
| `--gap-3xl` | 32px | Отступы между блоками |
| `--radius-sm` | 8px | Маленькие элементы |
| `--radius-md` | 12px | Инпуты, бейджи |
| `--radius-lg` | 16px | Карточки, кнопки, шторки |
| `--radius-xl` | 20px | Модальные окна, большие карточки |
| `--radius-full` | 9999px | Круглые элементы (аватар, радио) |

**Контейнер:** `mx-auto flex max-w-[576px] flex-col gap-7 pt-20 pb-8`  
**Мобильный отступ от таббара:** `pb-tabbar md:pb-0`

### 2.4. Иконки

- **Библиотека:** `lucide-react`
- **Размеры:** `size-5` (20px) для inline, `size-6` (24px) для кнопок/навигации
- **Цвет:** наследуется от текста (`currentColor`)
- **Stroke-width:** 2px по умолчанию

| Иконка | Lucide-имя | Использование |
|--------|------------|---------------|
| ← Назад | `ArrowLeft` | Навигация |
| ✕ Закрыть | `X` | Закрытие шторки |
| 🔍 Поиск | `Search` | Поле поиска |
| ▼ Раскрыть | `ChevronDown` | Селект/раскрытие |
| ▶ Перейти | `ChevronRight` | Переход вперёд |
| ✓ Галочка | `Check` | Checkbox, success |
| + Добавить | `Plus` | Новая карта |
| ? Помощь | `HelpCircle` | Подсказка CVC |
| 🔒 Блокировка | `Lock` | Безопасность |
| 💳 Карта | `CreditCard` | Платёжные методы |
| 📦 Доставка | `Package` | Пункт выдачи |
| 🚚 Курьер | `Truck` | Курьерская доставка |
| ✉️ Почта | `Mail` | Почта РФ |
| 🔔 Уведомления | `Bell` | Тарифы |
| 💰 Кэшбэк | `Sparkles` | Преимущества карты |
| 📱 NFC | `Smartphone` | Бесконтактная оплата |
| 🏧 Банкомат | `Banknote` | Снятие наличных |
| ↔️ Перевод | `ArrowLeftRight` | Переводы |
| 📋 Копировать | `Copy` | Копирование номера |
| 🔄 Обновить | `RefreshCw` | Повторить |

### 2.5. Компоненты из @rollout/ui-kit

```tsx
import {
  Button,
  Input,
  Field,
  Select,
  Tabs,
  Card,
  Badge,
  Avatar,
  Separator,
  Typography,
  cn,
} from '@rollout/ui-kit';
```

---

## 3. Компоненты UI (с примерами кода)

### 3.1. PaymentMethodCard

Выбор способа оплаты (кошелёк, банк, карта, СБП).

```tsx
import { Card, Avatar, Typography, cn } from '@rollout/ui-kit';
import { Check } from 'lucide-react';

interface PaymentMethodCardProps {
  id: string;
  title: string;
  subtitle: string;
  icon: React.ReactNode;
  selected?: boolean;
  onSelect: (id: string) => void;
}

export function PaymentMethodCard({
  id,
  title,
  subtitle,
  icon,
  selected,
  onSelect,
}: PaymentMethodCardProps) {
  return (
    <Card
      onClick={() => onSelect(id)}
      className={cn(
        'flex items-center gap-4 p-4 cursor-pointer transition-colors',
        selected ? 'border-[#3A3A3C] bg-[#2C2C2E]' : 'border-[#3A3A3C] bg-[#1C1C1E]'
      )}
    >
      <Avatar className="size-12 rounded-xl bg-[#3A3A3C]">
        {icon}
      </Avatar>
      <div className="flex-1 min-w-0">
        <Typography className="text-[17px] font-medium text-[#FAFAFA]">
          {title}
        </Typography>
        <Typography className="text-[15px] text-[#8E8E93]">
          {subtitle}
        </Typography>
      </div>
      <div
        className={cn(
          'size-6 rounded-full border-2 flex items-center justify-center',
          selected
            ? 'border-[#E5E5E5] bg-[#E5E5E5]'
            : 'border-[#3A3A3C]'
        )}
      >
        {selected && <Check className="size-4 text-[#0A0A0A]" />}
      </div>
    </Card>
  );
}
```

**Референс shadcn:** `Card` + `Avatar` + кастомный radio-индикатор.

---

### 3.2. CardFormInput

Поле ввода для формы карты (номер, срок, CVC).

```tsx
import { Input, Field, cn } from '@rollout/ui-kit';
import { HelpCircle } from 'lucide-react';

interface CardFormInputProps {
  label?: string;
  placeholder: string;
  value: string;
  onChange: (value: string) => void;
  maxLength?: number;
  helperIcon?: boolean;
  className?: string;
}

export function CardFormInput({
  label,
  placeholder,
  value,
  onChange,
  maxLength,
  helperIcon,
  className,
}: CardFormInputProps) {
  return (
    <Field className={className}>
      {label && (
        <label className="text-[15px] font-medium text-[#FAFAFA] mb-2 block">
          {label}
        </label>
      )}
      <div className="relative">
        <Input
          value={value}
          onChange={(e) => onChange(e.target.value)}
          placeholder={placeholder}
          maxLength={maxLength}
          className={cn(
            'w-full h-14 px-4 bg-[#1C1C1E] border border-[#3A3A3C] rounded-xl',
            'text-[17px] text-[#FAFAFA] placeholder:text-[#636366]',
            'focus:outline-none focus:border-[#8E8E93]',
            'transition-colors'
          )}
        />
        {helperIcon && (
          <button className="absolute right-4 top-1/2 -translate-y-1/2 text-[#636366]">
            <HelpCircle className="size-5" />
          </button>
        )}
      </div>
    </Field>
  );
}
```

**Референс shadcn:** `Input` внутри `Field`.

---

### 3.3. BankListItem

Элемент списка банков.

```tsx
import { Card, Avatar, Typography, cn } from '@rollout/ui-kit';
import { ChevronRight } from 'lucide-react';

interface BankListItemProps {
  name: string;
  icon: React.ReactNode;
  onClick: () => void;
}

export function BankListItem({ name, icon, onClick }: BankListItemProps) {
  return (
    <Card
      onClick={onClick}
      className="flex items-center gap-4 p-4 cursor-pointer border-[#3A3A3C] bg-[#1C1C1E] hover:bg-[#2C2C2E] transition-colors"
    >
      <Avatar className="size-12 rounded-xl bg-[#3A3A3C] overflow-hidden">
        {icon}
      </Avatar>
      <Typography className="flex-1 text-[17px] font-medium text-[#FAFAFA]">
        {name}
      </Typography>
      <ChevronRight className="size-5 text-[#8E8E93]" />
    </Card>
  );
}
```

**Референс shadcn:** `Card` + `Avatar`.

---

### 3.4. StatusScreen

Экран статуса (success, error, loading, polling).

```tsx
import { Card, Button, Typography, cn } from '@rollout/ui-kit';
import { Check, X, Loader2 } from 'lucide-react';

interface StatusScreenProps {
  variant: 'success' | 'error' | 'loading' | 'polling';
  title: string;
  description?: string;
  details?: { label: string; value: string }[];
  primaryAction?: { label: string; onClick: () => void };
  secondaryAction?: { label: string; onClick: () => void };
}

export function StatusScreen({
  variant,
  title,
  description,
  details,
  primaryAction,
  secondaryAction,
}: StatusScreenProps) {
  const iconMap = {
    success: (
      <div className="size-16 rounded-full bg-[#34C759] flex items-center justify-center">
        <Check className="size-8 text-white" />
      </div>
    ),
    error: (
      <div className="size-16 rounded-full bg-[#FF3B30] flex items-center justify-center">
        <X className="size-8 text-white" />
      </div>
    ),
    loading: (
      <div className="size-16 rounded-full bg-[#3A3A3C] flex items-center justify-center">
        <Loader2 className="size-8 text-[#FAFAFA] animate-spin" />
      </div>
    ),
    polling: (
      <div className="size-16 rounded-full bg-[#3A3A3C] flex items-center justify-center">
        <Loader2 className="size-8 text-[#FAFAFA] animate-spin" />
      </div>
    ),
  };

  return (
    <div className="flex flex-col items-center text-center gap-6 py-12">
      {iconMap[variant]}
      <div className="space-y-2">
        <Typography className="text-[28px] font-semibold text-[#FAFAFA]">
          {title}
        </Typography>
        {description && (
          <Typography className="text-[17px] text-[#8E8E93] max-w-[320px]">
            {description}
          </Typography>
        )}
      </div>
      {details && (
        <div className="w-full space-y-3">
          {details.map((detail) => (
            <div
              key={detail.label}
              className="flex justify-between border-b border-dashed border-[#3A3A3C] pb-2"
            >
              <Typography className="text-[15px] text-[#8E8E93]">
                {detail.label}
              </Typography>
              <Typography className="text-[15px] text-[#FAFAFA]">
                {detail.value}
              </Typography>
            </div>
          ))}
        </div>
      )}
      <div className="w-full space-y-3 mt-4">
        {primaryAction && (
          <Button
            onClick={primaryAction.onClick}
            className="w-full h-14 rounded-2xl bg-[#E5E5E5] text-[#0A0A0A] text-[17px] font-medium hover:bg-[#D4D4D4]"
          >
            {primaryAction.label}
          </Button>
        )}
        {secondaryAction && (
          <Button
            onClick={secondaryAction.onClick}
            variant="outline"
            className="w-full h-14 rounded-2xl border-[#3A3A3C] text-[#FAFAFA] text-[17px] font-medium hover:bg-[#2C2C2E]"
          >
            {secondaryAction.label}
          </Button>
        )}
      </div>
    </div>
  );
}
```

**Референс shadcn:** `Card` + `Button` + кастомные иконки.

---

### 3.5. PinCodePad

Клавиатура для ввода ПИН-кода / 3D Secure кода.

```tsx
import { Typography, cn } from '@rollout/ui-kit';
import { Delete } from 'lucide-react';

interface PinCodePadProps {
  length: number;
  value: string;
  onChange: (value: string) => void;
  title: string;
  subtitle?: string;
}

const KEYS = ['1', '2', '3', '4', '5', '6', '7', '8', '9', '', '0', 'backspace'];

export function PinCodePad({ length, value, onChange, title, subtitle }: PinCodePadProps) {
  const handleKey = (key: string) => {
    if (key === 'backspace') {
      onChange(value.slice(0, -1));
    } else if (key && value.length < length) {
      onChange(value + key);
    }
  };

  return (
    <div className="flex flex-col items-center gap-8">
      <div className="text-center space-y-2">
        <Typography className="text-[22px] font-semibold text-[#FAFAFA]">
          {title}
        </Typography>
        {subtitle && (
          <Typography className="text-[15px] text-[#8E8E93]">
            {subtitle}
          </Typography>
        )}
      </div>
      <div className="flex gap-3">
        {Array.from({ length }).map((_, i) => (
          <div
            key={i}
            className={cn(
              'size-4 rounded-full transition-colors',
              i < value.length ? 'bg-[#FAFAFA]' : 'bg-[#3A3A3C]'
            )}
          />
        ))}
      </div>
      <div className="grid grid-cols-3 gap-x-8 gap-y-6 w-full max-w-[280px]">
        {KEYS.map((key, i) => (
          <button
            key={i}
            onClick={() => handleKey(key)}
            disabled={!key}
            className={cn(
              'h-16 flex items-center justify-center text-[28px] font-normal text-[#FAFAFA]',
              'active:opacity-60 transition-opacity',
              !key && 'pointer-events-none'
            )}
          >
            {key === 'backspace' ? (
              <Delete className="size-7" />
            ) : (
              key
            )}
          </button>
        ))}
      </div>
    </div>
  );
}
```

**Референс shadcn:** Кастомный компонент (нет прямого аналога).

---

### 3.6. SelectCardField

Поле выбора карты с выпадающим селектом.

```tsx
import { Select, Card, Avatar, Typography, cn } from '@rollout/ui-kit';
import { ChevronDown } from 'lucide-react';

interface CardOption {
  id: string;
  title: string;
  subtitle: string;
  icon: React.ReactNode;
}

interface SelectCardFieldProps {
  label: string;
  value: string;
  options: CardOption[];
  onChange: (value: string) => void;
}

export function SelectCardField({ label, value, options, onChange }: SelectCardFieldProps) {
  const selected = options.find((o) => o.id === value);

  return (
    <div className="space-y-2">
      <Typography className="text-[15px] text-[#8E8E93]">{label}</Typography>
      <Select value={value} onValueChange={onChange}>
        <Select.Trigger
          className={cn(
            'w-full h-16 px-4 bg-[#1C1C1E] border border-[#3A3A3C] rounded-xl',
            'flex items-center gap-3 text-left',
            'focus:outline-none focus:border-[#8E8E93]'
          )}
        >
          {selected && (
            <>
              <Avatar className="size-10 rounded-lg bg-[#3A3A3C]">
                {selected.icon}
              </Avatar>
              <div className="flex-1 min-w-0">
                <Typography className="text-[17px] font-medium text-[#FAFAFA]">
                  {selected.title}
                </Typography>
                <Typography className="text-[15px] text-[#8E8E93]">
                  {selected.subtitle}
                </Typography>
              </div>
            </>
          )}
          <ChevronDown className="size-5 text-[#8E8E93]" />
        </Select.Trigger>
        <Select.Content className="bg-[#1C1C1E] border border-[#3A3A3C] rounded-xl">
          {options.map((option) => (
            <Select.Item
              key={option.id}
              value={option.id}
              className="flex items-center gap-3 px-4 py-3 cursor-pointer hover:bg-[#2C2C2E]"
            >
              <Avatar className="size-10 rounded-lg bg-[#3A3A3C]">
                {option.icon}
              </Avatar>
              <div>
                <Typography className="text-[17px] font-medium text-[#FAFAFA]">
                  {option.title}
                </Typography>
                <Typography className="text-[15px] text-[#8E8E93]">
                  {option.subtitle}
                </Typography>
              </div>
            </Select.Item>
          ))}
        </Select.Content>
      </Select>
    </div>
  );
}
```

**Референс shadcn:** `Select` с кастомным триггером.

---

### 3.7. SegmentedTabs

Сегментированные табы (Base / Premium, Материал / Цвет).

```tsx
import { Tabs, cn } from '@rollout/ui-kit';

interface SegmentedTabsProps {
  options: { value: string; label: string }[];
  value: string;
  onChange: (value: string) => void;
}

export function SegmentedTabs({ options, value, onChange }: SegmentedTabsProps) {
  return (
    <Tabs value={value} onValueChange={onChange}>
      <Tabs.List className="flex bg-[#1C1C1E] rounded-2xl p-1 gap-1">
        {options.map((option) => (
          <Tabs.Trigger
            key={option.value}
            value={option.value}
            className={cn(
              'flex-1 h-11 rounded-xl text-[15px] font-medium transition-all',
              'data-[state=active]:bg-[#3A3A3C] data-[state=active]:text-[#FAFAFA]',
              'data-[state=inactive]:text-[#8E8E93] data-[state=inactive]:hover:text-[#FAFAFA]'
            )}
          >
            {option.label}
          </Tabs.Trigger>
        ))}
      </Tabs.List>
    </Tabs>
  );
}
```

**Референс shadcn:** `Tabs` (segmented style).

---

### 3.8. StepIndicator

Индикатор шагов (Шаг 1 из 4, Шаг 2 из 4 и т.д.).

```tsx
import { Typography } from '@rollout/ui-kit';

interface StepIndicatorProps {
  current: number;
  total: number;
  title: string;
}

export function StepIndicator({ current, total, title }: StepIndicatorProps) {
  return (
    <div className="text-center space-y-1">
      <Typography className="text-[20px] font-semibold text-[#FAFAFA]">
        {title}
      </Typography>
      <Typography className="text-[15px] text-[#8E8E93]">
        Шаг {current} из {total}
      </Typography>
    </div>
  );
}
```

---

### 3.9. DeliveryOptionCard

Карточка выбора способа доставки.

```tsx
import { Card, Avatar, Typography, cn } from '@rollout/ui-kit';
import { ChevronRight } from 'lucide-react';

interface DeliveryOptionCardProps {
  icon: React.ReactNode;
  title: string;
  selected?: boolean;
  onClick: () => void;
}

export function DeliveryOptionCard({ icon, title, selected, onClick }: DeliveryOptionCardProps) {
  return (
    <Card
      onClick={onClick}
      className={cn(
        'flex items-center gap-4 p-4 cursor-pointer transition-colors',
        selected ? 'bg-[#2C2C2E] border-[#3A3A3C]' : 'bg-[#1C1C1E] border-[#3A3A3C]'
      )}
    >
      <Avatar className="size-12 rounded-xl bg-[#3A3A3C] flex items-center justify-center">
        {icon}
      </Avatar>
      <Typography className="flex-1 text-[17px] font-medium text-[#FAFAFA]">
        {title}
      </Typography>
      <ChevronRight className="size-5 text-[#8E8E93]" />
    </Card>
  );
}
```

---

### 3.10. CardPreview

Превью банковской карты.

```tsx
import { Card, Typography } from '@rollout/ui-kit';

interface CardPreviewProps {
  name: string;
  system: 'mir' | 'visa' | 'mastercard';
  design?: 'default' | 'premium' | 'color';
  color?: string;
}

export function CardPreview({ name, system, design = 'default', color }: CardPreviewProps) {
  const systemLogo = {
    mir: 'МИР',
    visa: 'VISA',
    mastercard: 'Mastercard',
  };

  return (
    <div
      className="relative w-full aspect-[1.586] rounded-2xl overflow-hidden p-6 flex flex-col justify-between"
      style={{
        background: color || (design === 'premium'
          ? 'linear-gradient(135deg, #8E8E93 0%, #E5E5E5 100%)'
          : '#2C2C2E'),
      }}
    >
      <div className="flex items-start justify-between">
        <div className="w-10 h-8 rounded-md bg-[#E5E5E5]/20" /> {/* Чип */}
      </div>
      <div className="flex items-end justify-between">
        <Typography className="text-[15px] font-medium text-white/90 uppercase tracking-wider">
          {name}
        </Typography>
        <Typography className="text-[20px] font-bold text-white/90 italic">
          {systemLogo[system]}
        </Typography>
      </div>
    </div>
  );
}
```

---

### 3.11. LimitSlider

Слайдер для установки лимита карты.

```tsx
import { Slider, Typography, Card } from '@rollout/ui-kit';

interface LimitSliderProps {
  label: string;
  value: number;
  max: number;
  onChange: (value: number) => void;
}

export function LimitSlider({ label, value, max, onChange }: LimitSliderProps) {
  return (
    <Card className="p-4 space-y-4 bg-[#1C1C1E] border-[#3A3A3C]">
      <div className="flex items-center justify-between">
        <Typography className="text-[17px] font-medium text-[#FAFAFA]">
          {label}
        </Typography>
        <button className="text-[15px] text-[#8E8E93]">Изменить</button>
      </div>
      <Typography className="text-[28px] font-bold text-[#FAFAFA]">
        {value.toLocaleString('ru-RU')} ₽
      </Typography>
      <Slider
        value={[value]}
        max={max}
        step={1000}
        onValueChange={(v) => onChange(v[0])}
        className="w-full"
      />
    </Card>
  );
}
```

**Референс shadcn:** `Slider` (track: `#3A3A3C`, fill: `#E5E5E5`, thumb: 24px circle).

---

### 3.12. RadioGroupCard

Радио-группа для выбора варианта (для кого карта).

```tsx
import { Card, Typography, cn } from '@rollout/ui-kit';

interface RadioOption {
  value: string;
  title: string;
  description: string;
}

interface RadioGroupCardProps {
  options: RadioOption[];
  value: string;
  onChange: (value: string) => void;
}

export function RadioGroupCard({ options, value, onChange }: RadioGroupCardProps) {
  return (
    <div className="space-y-3">
      {options.map((option) => (
        <Card
          key={option.value}
          onClick={() => onChange(option.value)}
          className={cn(
            'p-4 cursor-pointer transition-colors flex items-start gap-3',
            value === option.value ? 'bg-[#2C2C2E] border-[#3A3A3C]' : 'bg-[#1C1C1E] border-[#3A3A3C]'
          )}
        >
          <div
            className={cn(
              'mt-0.5 size-5 rounded-full border-2 flex-shrink-0 flex items-center justify-center',
              value === option.value ? 'border-[#E5E5E5]' : 'border-[#3A3A3C]'
            )}
          >
            {value === option.value && (
              <div className="size-2.5 rounded-full bg-[#E5E5E5]" />
            )}
          </div>
          <div className="space-y-1">
            <Typography className="text-[17px] font-medium text-[#FAFAFA]">
              {option.title}
            </Typography>
            <Typography className="text-[15px] text-[#8E8E93]">
              {option.description}
            </Typography>
          </div>
        </Card>
      ))}
    </div>
  );
}
```

**Референс shadcn:** `RadioGroup` (кастомная стилизация).

---

### 3.13. InfoRow

Строка с информацией (тарифы, реквизиты).

```tsx
import { Typography, Separator } from '@rollout/ui-kit';
import { Switch } from '@base-ui/react';

interface InfoRowProps {
  title: string;
  value?: string;
  description?: string;
  hasSwitch?: boolean;
  switchValue?: boolean;
  onSwitchChange?: (value: boolean) => void;
}

export function InfoRow({
  title,
  value,
  description,
  hasSwitch,
  switchValue,
  onSwitchChange,
}: InfoRowProps) {
  return (
    <div className="py-3 space-y-1">
      <div className="flex items-center justify-between">
        <div>
          <Typography className="text-[17px] text-[#FAFAFA]">{title}</Typography>
          {description && (
            <Typography className="text-[15px] text-[#8E8E93]">{description}</Typography>
          )}
        </div>
        {hasSwitch ? (
          <Switch
            checked={switchValue}
            onCheckedChange={onSwitchChange}
            className="data-[state=checked]:bg-[#34C759]"
          />
        ) : (
          value && (
            <Typography className="text-[17px] text-[#FAFAFA]">{value}</Typography>
          )
        )}
      </div>
      <Separator className="bg-[#3A3A3C]" />
    </div>
  );
}
```

---

### 3.14. SearchInput

Поле поиска с иконкой.

```tsx
import { Input, cn } from '@rollout/ui-kit';
import { Search } from 'lucide-react';

interface SearchInputProps {
  placeholder: string;
  value: string;
  onChange: (value: string) => void;
}

export function SearchInput({ placeholder, value, onChange }: SearchInputProps) {
  return (
    <div className="relative">
      <Search className="absolute left-4 top-1/2 -translate-y-1/2 size-5 text-[#636366]" />
      <Input
        value={value}
        onChange={(e) => onChange(e.target.value)}
        placeholder={placeholder}
        className={cn(
          'w-full h-12 pl-12 pr-4 bg-[#1C1C1E] border border-[#3A3A3C] rounded-xl',
          'text-[17px] text-[#FAFAFA] placeholder:text-[#636366]',
          'focus:outline-none focus:border-[#8E8E93]'
        )}
      />
    </div>
  );
}
```

---

### 3.15. CheckboxField

Чекбокс с подписью.

```tsx
import { Checkbox, Typography, cn } from '@rollout/ui-kit';
import { Check } from 'lucide-react';

interface CheckboxFieldProps {
  label: string;
  checked: boolean;
  onChange: (checked: boolean) => void;
}

export function CheckboxField({ label, checked, onChange }: CheckboxFieldProps) {
  return (
    <label className="flex items-center gap-3 cursor-pointer">
      <Checkbox
        checked={checked}
        onCheckedChange={onChange}
        className={cn(
          'size-6 rounded-md border-2 flex items-center justify-center transition-colors',
          checked ? 'bg-[#E5E5E5] border-[#E5E5E5]' : 'border-[#3A3A3C] bg-transparent'
        )}
      >
        {checked && <Check className="size-4 text-[#0A0A0A]" />}
      </Checkbox>
      <Typography className="text-[17px] text-[#FAFAFA]">{label}</Typography>
    </label>
  );
}
```

**Референс shadcn:** `Checkbox`.

---

## 4. Структура каждого экрана

### 4.1. PaymentPage — «Счета и карты»

**Скриншот:** `8-Payments_screenshots/↪_PaymentPage_✅_layout.png`

**Layout:**
- Шторка снизу (`bottom-sheet`) с заголовком «Счета и карты»
- Контент внутри шторки с отступами `gap-4`

**Компоненты сверху вниз:**
1. **Handle bar** — серый прямоугольник `w-10 h-1 rounded-full bg-[#3A3A3C]`
2. **Заголовок** «Счета и карты» — H2, center
3. **PaymentMethodCard** ×5:
   - Кошелёк (иконка + «Быстро пополнить через СБП»)
   - Т-Банк ×2 (иконка + «Мгновенно через СБП»)
   - МИР ••4444 (иконка + номер карты)
   - СБП (иконка + «Оплата в приложении вашего банка»)
   - Новая карта (+ иконка + «Visa, Mastercard, МИР»)
4. **Primary Button** «Выбрать» — на всю ширину

**Состояния:**
- `default` — ничего не выбрано, кнопка disabled
- `selected` — выбран метод, кнопка active
- `loading` — кнопка в состоянии загрузки

**Интерактивность:**
- `onSelect` — выбор метода (radio-логика)
- `onSubmit` — навигация к платежу

---

### 4.2. PaymentPageSingleChoose — «Платёж»

**Скриншот:** `8-Payments_screenshots/↪_PaymentPageSingleChoose_✅_layout.png`

**Layout:**
- Хедер с закрытием (✕ + «Платёж»)
- Сумма крупным шрифтом (34px)
- Контент с gap-7

**Компоненты сверху вниз:**
1. **Header** — `X` + «Платёж» (H1)
2. **Amount** — `19 000 ₽` (text-amount)
3. **Label** «Откуда» (caption)
4. **SelectCardField** — выбор карты (МИР ••4444)
5. **Security info** — текст про SSL и 3D Secure + ссылка
6. **Primary Button** «Оплатить 0 ₽»

**Состояния:**
- `loading` — кнопка с спиннером
- `error` — показать ошибку под кнопкой
- `disabled` — сумма 0, кнопка неактивна

---

### 4.3. CardForm — «Новая карта»

**Скриншот:** `8-Payments_screenshots/↪_CardForm_✅_layout.png`

**Layout:**
- Шторка снизу с handle bar
- Форма внутри шторки

**Компоненты сверху вниз:**
1. **Handle bar**
2. **Заголовок** «Новая карта» (H2, center)
3. **CardFormInput** «Номер карты» (full width)
4. **Row** с двумя инпутами: «ММ/ГГ» (flex-1) + «CVC» (flex-1) с иконкой help
5. **CheckboxField** «Запомнить карту»
6. **Primary Button** «Сохранить»

**Состояния:**
- `empty` — placeholder
- `valid` — валидные данные, кнопка active
- `invalid` — ошибка валидации (красный border)
- `loading` — кнопка в спиннере

---

### 4.4. BankList — «Выбор банка»

**Скриншот:** `8-Payments_screenshots/↪_BankList_✅_layout.png`

**Layout:**
- Полноэкранный список
- Футер с © 2025 Rollout + соц. иконки

**Компоненты сверху вниз:**
1. **Logo** (Rollout)
2. **Header** — `ArrowLeft` + «Выбор банка» (H1)
3. **SearchInput** «Название банка»
4. **BankListItem** ×N (Кошелек, ВТБ, Т-Банк, Альфа-Банк, Райффайзен, Ак Барс, Газпромбанк, Хоум Банк...)
5. **Footer** — © 2025 Rollout + X, LinkedIn, Telegram

**Состояния:**
- `loading` — спиннер вместо списка
- `error` — StatusScreen (error variant)
- `empty` — «Ничего не найдено»
- `filtered` — результаты поиска

**Интерактивность:**
- `onSearch` — фильтрация списка
- `onSelect` — выбор банка, навигация назад

---

### 4.5. M2M — «Пополнение кошелька»

**Скриншот:** `8-Payments_screenshots/↪_M2M_✅_layout.png`

**Layout:**
- Центрированный контент
- Статус-экран

**Компоненты сверху вниз:**
1. **Header** — `ArrowLeft` + «Пополнение кошелька»
2. **Bank icon** — логотип банка с лоадером (48px)
3. **Title** — «Отправили запрос на 13 629 ₽ в Т-Банк» (H2, center)
4. **Description** — «Отправили запрос на 13 629 ₽ в Т-Банк» (body, center)
5. **Primary Button** «Хорошо»

**Состояния:**
- `loading` — спиннер на иконке банка
- `success` — галочка, переход
- `error` — ошибка, кнопка «Повторить»

---

### 4.6. C2C — «Ждём пополнение через СБП»

**Скриншот:** `8-Payments_screenshots/↪_C2C_✅_layout.png`

**Layout:**
- Центрированный контент с инструкцией

**Компоненты сверху вниз:**
1. **Header** — `ArrowLeft` + «Пополнение кошелька»
2. **Bank icon** — логотип СБП с лоадером
3. **Title** — «Ждём пополнение через СБП» (H2, center)
4. **Description** — «На балансе для покупок 1 000 ₽. Можно пополнить ещё на 25 000 ₽ — это ваш лимит.» (body, center)
5. **Instruction list** (1-4 шага)
6. **Secondary Button** «Пополнить другим способом»

---

### 4.7. 3Ds — «Пополнение кошелька» (SMS-код)

**Скриншот:** `8-Payments_screenshots/↪_3Ds_✅_layout.png`

**Layout:**
- Форма с 5 инпутами-клетками

**Компоненты сверху вниз:**
1. **Header** — `ArrowLeft` + «Пополнение кошелька»
2. **Amount** — «19 000 ₽» (text-amount)
3. **Description** — «Код отправлен на +7 912 3•••84. Введите его и подтвердите оплату»
4. **PinCodePad** — 5 клеток для ввода кода
5. **Timer** — «Получить код снова можно через 59 с» (caption, muted)
6. **Link** — «Отменить платёж» (underline)

**Состояния:**
- `typing` — заполнение клеток
- `valid` — код заполнен, авто-отправка
- `invalid` — ошибка, тряска клеток
- `timer` — обратный отсчёт

---

### 4.8. Polling — «Ждём ответ от банка»

**Скриншот:** `8-Payments_screenshots/↪_Polling_✅_layout.png`

**Layout:**
- Центрированный статус

**Компоненты сверху вниз:**
1. **Header** — `ArrowLeft` + (пусто)
2. **Loader** — спиннер (large)
3. **Title** — «Ждём ответ от банка» (H2, center)
4. **Description** — «Ждём платёжную страницу» (body, center)
5. **Secondary Button** «Выбрать другую карту»

**Состояния:**
- `polling` — спиннер крутится
- `timeout` — «Повторить»
- `success` — редирект
- `error` — показать ошибку

---

### 4.9. Error — «Не удалось загрузить»

**Скриншот:** `8-Payments_screenshots/↪_Error_✅_Status.png`

**Layout:**
- Центрированный статус-экран

**Компоненты сверху вниз:**
1. **Logo** + `X` (закрыть)
2. **StatusScreen** (error variant):
   - Иконка: красный круг с крестиком
   - Title: «Не удалось загрузить список банков»
   - Primary Button: «Обновить»

**Состояния:**
- `error` — показать ошибку
- `retrying` — кнопка в спиннере

---

### 4.10. Success — «Вернули деньги на карту»

**Скриншот:** `8-Payments_screenshots/↪_Success_✅_Status.png`

**Layout:**
- Центрированный статус-экран с деталями

**Компоненты сверху вниз:**
1. **Logo** + `X`
2. **StatusScreen** (success variant):
   - Иконка: зелёный круг с галочкой
   - Title: «Вернули деньги на карту»
   - Details: Номер получателя, Имя, Банк (dotted border)
   - Primary Button: «Хорошо»

---

### 4.11. PaymentServices — «Оплатить» (реквизиты)

**Скриншот:** `8-Payments_screenshots/↪_PaymentServices_layout.png`

**Layout:**
- Форма с полями ввода

**Компоненты сверху вниз:**
1. **Header** — `ArrowLeft` + «Оплатить» (H1)
2. **Label** «Откуда» + **SelectCardField**
3. **Label** «Куда» + **SelectCardField**
4. **Label** «Расчётный счёт» + **CardFormInput** (с иконкой scanner)
5. **Label** «БИК» + **CardFormInput** (с иконкой scanner)
6. **Label** «ФИО получателя» + **CardFormInput**
7. **Label** «Комментарий» + **CardFormInput** (с иконкой scanner)
8. **Security info** — текст про SSL + ссылка
9. **Primary Button** «Продолжить»

**Состояния:**
- `empty` — placeholder
- `filled` — кнопка active
- `loading` — спиннер
- `error` — ошибка под полем

---

### 4.12. CardDetails — «Хотите заморозить карту?»

**Скриншот:** `8-Payments_screenshots/↪_Card_Details_layout.png`

**Layout:**
- Шторка снизу (confirmation dialog)

**Компоненты сверху вниз:**
1. **Handle bar**
2. **Title** — «Хотите заморозить карту?» (H2, bold)
3. **Description** — текст про защиту
4. **Primary Button** «Заморозить»
5. **Secondary Button** «Отмена»

---

### 4.13. CardDebit — «Заказ карты» (Шаг 1)

**Скриншот:** `8-Payments_screenshots/↪_CardDebit_layout.png`

**Layout:**
- Длинный скроллящийся экран

**Компоненты сверху вниз:**
1. **Header** — `ArrowLeft` + «Заказ карты» + StepIndicator «Шаг 1 из 4»
2. **CardPreview** — превью карты (Premium)
3. **Color selector** — ряд цветных кружков (радиус 9999px)
4. **Price** — «Premium · 499 ₽» (H2, center)
5. **Description** — «Элегантная карта с металлическим сиянием...»
6. **SegmentedTabs** — «Базовая» / «Premium»
7. **Title** — «Дебетовая карта Premium» (H2)
8. **Subtitle** — «Покупайте всё, что нужно»
9. **Feature list** — 4 пункта с иконками (кэшбэк, повышенный кэшбэк, Mir+СБП, Mir Pay)
10. **Link** — «Подробнее о карте» (accent color)
11. **Primary Button** «Продолжить»

---

### 4.14. CardDesign — «Дизайн карты»

**Скриншот:** `8-Payments_screenshots/↪_CardDesign_layout.png`

**Layout:**
- Превью карты + выбор параметров

**Компоненты сверху вниз:**
1. **Header** — `ArrowLeft` + «Дизайн карты» (H1)
2. **CardPreview** — большая превью карты (Visa, серебристая)
3. **Price** — «Premium · 499 ₽» (H2, center)
4. **Description** — «Элегантная серая карта с перламутровым сиянием...»
5. **SegmentedTabs** — «Материал» / «Цвет»
6. **Color circles** — 4 цвета (радиус 9999px, size-12)
7. **Primary Button** «Подтвердить»

---

### 4.15. CardSearch — «Куда доставить карту»

**Скриншот:** `8-Payments_screenshots/↪_CardSearch_⚙️_layout.png`

**Layout:**
- Карта на весь экран (светлая тема карты)
- Поиск и фильтры сверху

**Компоненты сверху вниз:**
1. **Header** — `ArrowLeft` + «Куда доставить карту» (H1, dark text)
2. **Label** — «Найти по адресу или названию» (dark text)
3. **SearchInput** — «Найти пункт выдачи» (light theme)
4. **Filter chips** — СДЭК, OZON, Wildberries, DPD, Пятёрочка (scroll horizontal)
5. **Map** — интерактивная карта с маркерами
6. **Footer** — © 2025 Rollout + соц. иконки

**Примечание:** карта — внешний компонент (Yandex/Google Maps), не вёрстается вручную.

---

### 4.16. CardDelivery — «Заказ карты» (Шаг 2)

**Скриншот:** `8-Payments_screenshots/↪_CardDelivery_layout.png`

**Layout:**
- Выбор способа доставки

**Компоненты сверху вниз:**
1. **Header** — `ArrowLeft` + «Заказ карты» + StepIndicator «Шаг 2 из 4»
2. **Address** — «г. Москва, Московский проспект, д. 37, кв. 55» с `ChevronRight`
3. **Title** — «Выберите способ доставки» (H2)
4. **DeliveryOptionCard** ×3:
   - Курьерская доставка (Truck)
   - Доставка в пункт выдачи (Package)
   - Доставка Почтой РФ (Mail)
5. **Primary Button** «Продолжить»

---

### 4.17. CardDeliveryDetails — «Заказ карты» (Шаг 3)

**Скриншот:** `8-Payments_screenshots/↪_CardDeliveryDetails_layout.png`

**Layout:**
- Форма деталей доставки

**Компоненты сверху вниз:**
1. **Header** — `ArrowLeft` + «Заказ карты» + StepIndicator «Шаг 3 из 4»
2. **Address** — «г. Москва...» (readonly)
3. **Title** — «Доставим карту курьером» (H2)
4. **Subtitle** — «Выберите удобную дату и время доставки»
5. **Label** «Дата и время доставки»
6. **Date chips** — «Сегодня», «Завтра», «Послезавтра», «до 3 дней» (scroll horizontal)
7. **Time slots** — «300 ₽ 10:00-12:00», «150 ₽ 12:00-14:00» (radio cards)
8. **Label** «Подъезд» + **CardFormInput** (значение «3»)
9. **Label** «Этаж» + **CardFormInput** (значение «5»)
10. **Label** «Домофон» + **CardFormInput** (значение «*5555»)
11. **CardFormInput** — «Как добраться и когда вам позвонить» (textarea)
12. **Primary Button** «Продолжить»

---

### 4.18. CardSuccess — «Карта оформлена»

**Скриншот:** `8-Payments_screenshots/↪_CardSuccess✅_CardSuccess.png`

**Layout:**
- Центрированный статус-экран

**Компоненты сверху вниз:**
1. **Logo** + `X`
2. **StatusScreen** (success variant):
   - Иконка: зелёный круг с галочкой
   - Title: «Карта оформлена»
   - Description: «Ваша карта оформлена и станет доступна для операций в ближайшее время»
   - Primary Button: «Вернуться на главный»

---

### 4.19. CardBlock — «Блокировка карты»

**Скриншот:** `8-Payments_screenshots/↪_СardBlock_CardBlocked.png`

**Layout:**
- Форма подтверждения блокировки

**Компоненты сверху вниз:**
1. **Header** — `ArrowLeft` + «Блокировка карты» (H1)
2. **Description** — «После блокировки расплатиться картой будет нельзя, но вы сможете совершать платежи и переводы в приложении»
3. **Card info** — иконка карты + «МИР ROLLOUT ••5555» + «Карта, которую заблокируем»
4. **Primary Button** «Заблокировать»
5. **Secondary Button** «Отменить»

---

### 4.20. ReplaceCard — «Перевыпуск»

**Скриншот:** `8-Payments_screenshots/↪_ReplaceСard_CardReplace.png`

**Layout:**
- Информация о перевыпуске

**Компоненты сверху вниз:**
1. **Header** — `ArrowLeft` (пусто)
2. **Title** — «Что изменится после перевыпуска» (H1)
3. **Info list** ×3 с иконками:
   - Заблокируем старую карту
   - Номер карты изменится
   - Картой можно пользоваться сразу после перевыпуска
4. **Primary Button** «Перевыпустить»
5. **Secondary Button** «Отменить»

---

### 4.21. CardPinCode — «Пин-код»

**Скриншот:** `8-Payments_screenshots/↪_CardPinCode_CreatePIN.png`

**Layout:**
- Центрированный экран с клавиатурой

**Компоненты сверху вниз:**
1. **Header** — `ArrowLeft` + «Пин-код» (H1)
2. **Logo** — Rollout (48px, center)
3. **Title** — «Придумайте ПИН-код» (H2, center)
4. **Pin dots** — 4 точки (заполненные/пустые)
5. **Subtitle** — «Для снятия наличных и оплаты покупок»
6. **PinCodePad** — цифровая клавиатура 3×4

**Состояния:**
- `create` — «Придумайте ПИН-код»
- `confirm` — «Повторите ПИН-код»
- `error` — «ПИН-коды не совпадают»
- `success` — редирект

---

### 4.22. CardLimits — «Лимиты»

**Скриншот:** `8-Payments_screenshots/↪_CardLimits_.CardLocal.png`

**Layout:**
- Карточка с лимитом

**Компоненты сверху вниз:**
1. **LimitSlider** — «Лимиты в феврале» + «10 000 ₽» + слайдер + «Изменить»

**Состояния:**
- `view` — readonly слайдер
- `edit` — активный слайдер с кнопкой «Сохранить»

---

### 4.23. CardContactlessPayment — «Бесконтактная оплата»

**Скриншот:** `8-Payments_screenshots/↪_ContactlessPayment_CardContactlessPayment.png`

**Layout:**
- Превью карты + CTA

**Компоненты сверху вниз:**
1. **Header** — `ArrowLeft` + «Бесконтактная оплата» (H1)
2. **CardPreview** — превью карты (Mir)
3. **Title** — «Подключите RolloutPay» (H2, center)
4. **Description** — «Платите смартфоном не открывая приложение» (center)
5. **Primary Button** «Подключить»

---

### 4.24. CardTariffsandConditions — «Тарифы и условия»

**Скриншот:** `8-Payments_screenshots/↪_CardTariffsandConditions_CardTariffsandConditions.png`

**Layout:**
- Скроллящийся список секций

**Компоненты сверху вниз:**
1. **Header** — `ArrowLeft` + «Тарифы и условия» (H1)
2. **Section** «Обслуживание»:
   - InfoRow: «Обслуживание карты» — «Бесплатно»
   - InfoRow: «Уведомления об операциях» — «99 ₽ в месяц» + Switch
3. **Section** «Снятие наличных»:
   - InfoRow: «Бесплатно» + список (банкоматы, условия)
   - InfoRow: «С комиссией» + список
4. **Section** «Переводы» — 3 InfoRow
5. **Section** «Пополнение» — 2 InfoRow

---

### 4.25. CardAllProducts — «Оформить продукт»

**Скриншот:** `8-Payments_screenshots/↪_СardAllProducts_CardAllProduct.png`

**Layout:**
- Список продуктов

**Компоненты сверху вниз:**
1. **Header** — `ArrowLeft` + «Оформить продукт» (H1)
2. **Section** «Карту»:
   - PaymentMethodCard ×3: Дебетовая, Кредитная, Детская
3. **Section** «Счёт»:
   - PaymentMethodCard: Платёжный счёт

---

### 4.26. CardForSomeone — «Оформление заказа»

**Скриншот:** `8-Payments_screenshots/↪_CardForSomeone_CardForSomeone.png`

**Layout:**
- Радио-группа

**Компоненты сверху вниз:**
1. **Header** — `ArrowLeft` + «Оформление заказа» (H1)
2. **Title** — «Для кого оформить карту» (H2)
3. **RadioGroupCard**:
   - «Для себя» — «Например, для покупок и повседневных расходов»
   - «Для другого человека» — «Совместные расходы с уведомлениями о тратах»
4. **Primary Button** «Продолжить»

---

### 4.27. BNPL — «Buy Now Pay Later»

**Скриншот:** `8-Payments_screenshots/↪_BNPL_layout.png`

**Layout:**
- Слот для контента (пустой экран с placeholder)

**Компоненты сверху вниз:**
1. **Placeholder** — «Slot (swap it with your content)» в рамке
2. **Footer** — © 2025 Rollout + соц. иконки

---

## 5. Пример полной страницы

### PaymentPage.tsx — страница выбора способа оплаты

```tsx
import { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import {
  Button,
  Card,
  Avatar,
  Typography,
  cn,
} from '@rollout/ui-kit';
import {
  Check,
  Plus,
  Wallet,
  Building2,
  CreditCard,
  Banknote,
} from 'lucide-react';

// --- Types ---
interface PaymentMethod {
  id: string;
  title: string;
  subtitle: string;
  icon: React.ReactNode;
}

// --- Mock Data ---
const PAYMENT_METHODS: PaymentMethod[] = [
  {
    id: 'wallet',
    title: 'Кошелёк',
    subtitle: 'Быстро пополнить через СБП',
    icon: <Wallet className="size-6 text-[#FAFAFA]" />,
  },
  {
    id: 'tbank-1',
    title: 'Т-Банк',
    subtitle: 'Мгновенно через СБП',
    icon: <Building2 className="size-6 text-[#FAFAFA]" />,
  },
  {
    id: 'mir',
    title: 'МИР',
    subtitle: '••4444',
    icon: <CreditCard className="size-6 text-[#FAFAFA]" />,
  },
  {
    id: 'sbp',
    title: 'СБП',
    subtitle: 'Оплата в приложении вашего банка',
    icon: <Banknote className="size-6 text-[#FAFAFA]" />,
  },
  {
    id: 'new-card',
    title: 'Новая карта',
    subtitle: 'Visa, Mastercard, МИР',
    icon: <Plus className="size-6 text-[#FAFAFA]" />,
  },
];

// --- Components ---
function PaymentMethodCard({
  method,
  selected,
  onSelect,
}: {
  method: PaymentMethod;
  selected: boolean;
  onSelect: () => void;
}) {
  return (
    <Card
      onClick={onSelect}
      className={cn(
        'flex items-center gap-4 p-4 cursor-pointer transition-colors border',
        selected
          ? 'bg-[#2C2C2E] border-[#3A3A3C]'
          : 'bg-[#1C1C1E] border-[#3A3A3C] hover:bg-[#2C2C2E]'
      )}
    >
      <Avatar className="size-12 rounded-xl bg-[#3A3A3C] flex items-center justify-center">
        {method.icon}
      </Avatar>
      <div className="flex-1 min-w-0">
        <Typography className="text-[17px] font-medium text-[#FAFAFA]">
          {method.title}
        </Typography>
        <Typography className="text-[15px] text-[#8E8E93]">
          {method.subtitle}
        </Typography>
      </div>
      <div
        className={cn(
          'size-6 rounded-full border-2 flex items-center justify-center transition-colors',
          selected ? 'border-[#E5E5E5] bg-[#E5E5E5]' : 'border-[#3A3A3C]'
        )}
      >
        {selected && <Check className="size-4 text-[#0A0A0A]" />}
      </div>
    </Card>
  );
}

// --- Page ---
export default function PaymentPage() {
  const navigate = useNavigate();
  const [selectedId, setSelectedId] = useState<string>('');
  const [isLoading, setIsLoading] = useState(false);

  const handleSelect = () => {
    if (!selectedId) return;
    setIsLoading(true);
    // Mock handler
    setTimeout(() => {
      setIsLoading(false);
      navigate('/payments/single');
    }, 800);
  };

  return (
    <div className="min-h-screen bg-[#0A0A0A] flex items-end justify-center">
      {/* Bottom Sheet */}
      <div className="w-full max-w-[576px] bg-[#1C1C1E] rounded-t-[20px] p-6 space-y-6">
        {/* Handle */}
        <div className="flex justify-center">
          <div className="w-10 h-1 rounded-full bg-[#3A3A3C]" />
        </div>

        {/* Title */}
        <Typography className="text-[22px] font-semibold text-[#FAFAFA] text-center">
          Счета и карты
        </Typography>

        {/* Methods List */}
        <div className="space-y-3">
          {PAYMENT_METHODS.map((method) => (
            <PaymentMethodCard
              key={method.id}
              method={method}
              selected={selectedId === method.id}
              onSelect={() => setSelectedId(method.id)}
            />
          ))}
        </div>

        {/* Submit Button */}
        <Button
          onClick={handleSelect}
          disabled={!selectedId || isLoading}
          className={cn(
            'w-full h-14 rounded-2xl text-[17px] font-medium transition-colors',
            selectedId && !isLoading
              ? 'bg-[#E5E5E5] text-[#0A0A0A] hover:bg-[#D4D4D4]'
              : 'bg-[#3A3A3C] text-[#636366]'
          )}
        >
          {isLoading ? (
            <span className="animate-pulse">Загрузка...</span>
          ) : (
            'Выбрать'
          )}
        </Button>
      </div>
    </div>
  );
}
```

---

## 6. Чек-лист вёрстки

- [ ] **Все цвета через токены** — не хардкод, использовать CSS-переменные или Tailwind `theme()`
- [ ] **Типографика совпадает с макетом** — размеры, веса, line-height, letter-spacing по разделу 2.2
- [ ] **Отступы и радиусы по дизайн-системе** — gap, padding, radius из раздела 2.3
- [ ] **Иконки из lucide-react** — `size-5` / `size-6`, stroke-width 2px, `currentColor`
- [ ] **Контейнер** — `max-w-[576px] mx-auto` для всего контента
- [ ] **Отступ от шапки** — `pt-20` для всех страниц
- [ ] **Мобильный отступ от таббара** — `pb-tabbar md:pb-0`
- [ ] **Все формы на локальном `useState`** — без внешних библиотек форм
- [ ] **Mock-обработчики** — без `console.log`, использовать `setTimeout` + `navigate`
- [ ] **Route добавлен в `App.tsx`** — все пути из раздела 1.3
- [ ] **Скриншот соответствует макету** — визуальный diff, проверить padding, gaps, цвета
- [ ] **Не использовать `radix-ui`** — только `@base-ui/react` и `@rollout/ui-kit`
- [ ] **Не использовать `shadcn add`** — компоненты из `@rollout/ui-kit`
- [ ] **Не deep imports** — `import { ... } from '@rollout/ui-kit'`
- [ ] **Футер** — © 2025 Rollout + иконки X, LinkedIn, Telegram (lucide: `Twitter` → `X`, `Linkedin`, `Send` или `MessageCircle`)
- [ ] **Шрифт** — Geist Variable (подключен глобально)
- [ ] **Тёмная тема** — фон `#0A0A0A`, текст `#FAFAFA`
- [ ] **Primary Button** — `bg-[#E5E5E5] text-[#0A0A0A]`
- [ ] **Secondary Button** — `border-[#3A3A3C] bg-transparent text-[#FAFAFA]`
- [ ] **Input** — `bg-[#1C1C1E] border-[#3A3A3C] rounded-xl placeholder-[#636366]`
- [ ] **Card** — `bg-[#1C1C1E] border-[#3A3A3C] rounded-2xl`
- [ ] **Tabs** — segmented, `bg-[#1C1C1E]`, active tab `bg-[#3A3A3C]`
- [ ] **Slider** — track `#3A3A3C`, fill `#E5E5E5`, thumb 24px circle
- [ ] **Badge** — small circle, `bg-[#3A3A3C]`
- [ ] **Success** — `#34C759`, **Error** — `#FF3B30`
- [ ] **Accessibility** — focus states, aria-labels на кнопках и инпутах
- [ ] **Responsive** — работает на 320px до 576px

---

## Скриншоты

Все скриншоты сохранены в `/Users/mokoloskov/Documents/kimi/workspace/8-Payments_screenshots/`:

1. `Cover_8._Payments.png` — обложка модуля
2. `↪_PaymentPage_✅_layout.png` — выбор способа оплаты
3. `↪_PaymentPageSingleChoose_✅_layout.png` — платёж с одной картой
4. `↪_CardForm_✅_layout.png` — форма новой карты
5. `↪_BankList_✅_layout.png` — список банков
6. `↪_M2M_✅_layout.png` — статус M2M
7. `↪_C2C_✅_layout.png` — ожидание C2C
8. `↪_3Ds_✅_layout.png` — 3D Secure код
9. `↪_Polling_✅_layout.png` — polling банка
10. `↪_Error_✅_Status.png` — ошибка
11. `↪_Success_✅_Status.png` — успех
12. `↪_PaymentServices_layout.png` — оплата по реквизитам
13. `↪_Card_Details_layout.png` — детали карты
14. `↪_CardTariffsandConditions_CardTariffsandConditions.png` — тарифы
15. `↪_ContactlessPayment_CardContactlessPayment.png` — бесконтактная оплата
16. `↪_СardBlock_CardBlocked.png` — блокировка
17. `↪_ReplaceСard_CardReplace.png` — перевыпуск
18. `↪_CardPinCode_CreatePIN.png` — ПИН-код
19. `↪_CardLimits_.CardLocal.png` — лимиты
20. `↪_RegistrationNewCard_Select_card_to_order.png` — выбор карты
21. `↪_СardAllProducts_CardAllProduct.png` — все продукты
22. `↪_CardForSomeone_CardForSomeone.png` — для кого карта
23. `↪_CardDebit_layout.png` — заказ дебетовой
24. `↪_CardDesign_layout.png` — дизайн карты
25. `↪_CardSearch_⚙️_layout.png` — поиск пункта выдачи
26. `↪_CardDeliveryDetails_layout.png` — детали доставки
27. `↪_CardDelivery_layout.png` — способы доставки
28. `↪_CardСonfirmation_СonfirmationContent_view2.png` — подтверждение
29. `↪_CardSuccess✅_CardSuccess.png` — успех заказа
30. `↪_BNPL_layout.png` — BNPL слот

**Всего скриншотов:** 30
