# Инструкция по вёрстке модуля 10-History

## 1. Обзор модуля

Модуль **History** отвечает за отображение истории операций пользователя, фильтрацию и обработку ошибок загрузки данных.

**Экраны:**

| Экран | Роут | Описание |
|-------|------|----------|
| **HistoryMain** | `/history/history-main` | Главный экран с суммой расходов, прогресс-баром по категориям, кнопкой фильтров и списком операций, сгруппированных по дням. |
| **HistoryOperationsList** | `/history/history-operationsList` | Экран ошибки загрузки истории операций (fallback при неудачном запросе). |
| **HistoryFilters** | `/history/history-filters` | Экран фильтров с чипсами типов операций, датами, суммами, счетами, картами и категориями. |

---

## 2. Дизайн-система

### Типографика

| Элемент | Размер | Вес | Цвет |
|---------|--------|-----|------|
| Заголовок экрана | `text-2xl` (24px) | `font-semibold` | `#FAFAFA` |
| Заголовок секции | `text-sm` (14px) | `font-medium` | `#9A9A9A` (muted) |
| Сумма в карточке | `text-[32px]` / `leading-tight` | `font-bold` | `#FAFAFA` |
| Название операции | `text-base` (16px) | `font-medium` | `#FAFAFA` |
| Подпись операции | `text-sm` (14px) | `font-normal` | `#9A9A9A` |
| Сумма операции | `text-base` (16px) | `font-semibold` | `#FAFAFA` (расход), `#30D158` (доход) |
| Дата-разделитель | `text-sm` (14px) | `font-medium` | `#9A9A9A` |

### Цвета

| Токен | Значение | Применение |
|-------|----------|------------|
| `bg-page` | `#0A0A0A` | Фон страницы |
| `bg-card` | `#1C1C1E` | Фон карточек, инпутов, аватарок |
| `border-default` | `#3A3A3C` | Бордеры карточек, инпутов, чипсов |
| `text-primary` | `#FAFAFA` | Основной текст |
| `text-muted` | `#9A9A9A` | Вторичный текст, подписи |
| `text-success` | `#30D158` | Положительные суммы (доходы) |
| `text-error` | `#FF453A` | Иконка ошибки, крестик |
| `accent-pink` | `#FF375F` | Сегмент прогресс-бара |
| `accent-blue` | `#0A84FF` | Сегмент прогресс-бара |
| `accent-orange` | `#FF9F0A` | Сегмент прогресс-бара |
| `accent-green` | `#30D158` | Сегмент прогресс-бара |
| `btn-primary-bg` | `#E5E5E5` | Фон Primary кнопки |
| `btn-primary-text` | `#0A0A0A` | Текст Primary кнопки |

### Радиусы

| Компонент | Радиус |
|-----------|--------|
| Карточка | `rounded-2xl` (16px) |
| Кнопка Primary | `rounded-xl` (12px) |
| Input | `rounded-xl` (12px) |
| Чипс / Badge | `rounded-lg` (8px) или `rounded-full` |
| Аватарка / Иконка операции | `rounded-xl` (16px) или `rounded-2xl` |
| Прогресс-бар | `rounded-full` |

---

## 3. Компоненты UI

### 3.1. HistorySummaryCard

Карточка верхнего уровня с общей суммой расходов и многоцветным сегментированным прогресс-баром.

**Структура:**
- Обертка `Card` (`bg-card`, `border-default`, `rounded-2xl`, `p-5`).
- Внутри `flex flex-col gap-4`.
- Блок текста: сумма `Typography` (32px, bold, white), подпись `Typography` (14px, muted).
- SegmentedProgressBar: горизонтальный `flex h-3 w-full overflow-hidden rounded-full`.
  - 4 сегмента в ряд, пропорции задаются через `style={{ width: '${percent}%' }}`.
  - Цвета: `#FF375F`, `#0A84FF`, `#FF9F0A`, `#30D158`.
  - Между сегментами `gap-1` (или `space-x-1`).

**Props:**
```ts
interface HistorySummaryCardProps {
  totalAmount: string;        // "100 089 ₽"
  periodLabel: string;        // "Расходы за апрель"
  segments: { color: string; percent: number }[];
}
```

---

### 3.2. HistoryList

Группировка операций по датам с разделителями.

**Структура:**
- Внешний `div` с `flex flex-col gap-6`.
- Для каждой группы:
  - `DateDivider` — строка с датой (`Сегодня, 5 ноября`).
  - Внутри группы `flex flex-col gap-0` (или `gap-1`), каждый `HistoryItem` — это строка.
- Если список пустой — `EmptyState` с иконкой и подписью.

**Props:**
```ts
interface HistoryListProps {
  groups: {
    dateLabel: string;
    items: HistoryItemData[];
  }[];
}
```

---

### 3.3. HistoryItem

Строка одной операции.

**Структура:**
- `div` с `flex items-center gap-3 py-3` (отступы между items через `gap` или `border-b`).
- **Слева:** `TransactionIcon`.
- **Центр:** `flex flex-col gap-0.5 flex-1 min-w-0`.
  - `Typography` name — truncate, `text-base font-medium text-primary`.
  - `Typography` description — `text-sm text-muted`, возможно с `truncate`.
- **Справа:** `flex flex-col items-end gap-1`.
  - `AmountDisplay`.
  - `StatusBadge` (если есть, например `+12`).

**Props:**
```ts
interface HistoryItemProps {
  id: string;
  name: string;
  description: string;
  amount: number;
  currency: string;
  type: 'income' | 'expense' | 'neutral';
  iconType: 'avatar' | 'icon';
  iconSrc?: string;      // URL аватарки или иконка Lucide
  iconBg?: string;       // фоновый цвет для иконки
  badge?: string;        // например "+12"
}
```

---

### 3.4. TransactionIcon

Аватарка или иконка операции слева в строке.

**Структура:**
- Если `avatar`: `Avatar` из `@rollout/ui-kit`, размер `size="lg"` (~48px), `rounded-xl`.
- Если `icon`: `div` `w-12 h-12 rounded-xl flex items-center justify-center` с `bg` из пропа (`iconBg`).
  - Внутри иконка из `lucide-react` (размер 24, цвет белый).

**Props:**
```ts
interface TransactionIconProps {
  type: 'avatar' | 'icon';
  src?: string;
  icon?: React.ComponentType<{ className?: string }>;
  iconBg?: string;
}
```

---

### 3.5. AmountDisplay

Отображение суммы с правильным цветом и знаком.

**Правила:**
- `type === 'income'` → `+` + formatted, цвет `text-success` (`#30D158`).
- `type === 'expense'` → `−` + formatted (minus sign U+2212), цвет `text-primary`.
- `type === 'neutral'` → без знака, цвет `text-primary`.
- Формат числа: `Intl.NumberFormat('ru-RU', { minimumFractionDigits: 2 })` или `toLocaleString`.

**Props:**
```ts
interface AmountDisplayProps {
  amount: number;
  currency: string; // "₽", "$"
  type: 'income' | 'expense' | 'neutral';
}
```

---

### 3.6. StatusBadge

Маленький pill-Бейдж, используется для счётчиков (`+12`, `+3`) и индикатора активных фильтров.

**Структура:**
- `Badge` из `@rollout/ui-kit` или кастомный `span`.
- `bg-[#3A3A3C] text-[#FAFAFA] text-xs font-medium px-2 py-0.5 rounded-full`.
- На экране фильтров — белый фон с чёрным текстом (или наоборот, в зависимости от active).

**Props:**
```ts
interface StatusBadgeProps {
  children: React.ReactNode; // "+12" или 2
  variant?: 'default' | 'active';
}
```

---

### 3.7. SearchBar

Поле поиска по операциям (используется на будущих итерациях или при необходимости).

**Структура:**
- `Field` из `@rollout/ui-kit`.
- `Input` с `bg-card`, `border-default`, `rounded-xl`, `placeholder:text-muted`.
- Иконка `Search` (lucide-react) слева внутри `Input` (через `startAdornment` или абсолютное позиционирование).
- Иконка `X` справа для очистки, если `value.length > 0`.

**Props:**
```ts
interface SearchBarProps {
  value: string;
  onChange: (value: string) => void;
  placeholder?: string;
}
```

---

### 3.8. PeriodSelector

Поле выбора периода дат с иконкой календаря.

**Структура:**
- `Field` с `label="Дата или период"`.
- `Input` read-only, `value="10.01.2025 - 16.01.2025"`, `rounded-xl`.
- Иконка `Calendar` справа (`endAdornment` или внутри Input).
- При клике — открытие нативного date-picker или кастомного календаря (вне скоупа — оставить TODO).

**Props:**
```ts
interface PeriodSelectorProps {
  startDate?: string;
  endDate?: string;
  onChange: (range: { start: string; end: string }) => void;
}
```

---

### 3.9. FilterChip

Чипс (кнопка-фильтр) для типов операций, категорий, переводов и платежей.

**Структура:**
- `Button` из `@rollout/ui-kit` с `variant={active ? 'secondary' : 'outline'}`.
- Или кастомный `button` / `div`:
  - Неактивный: `bg-transparent border border-[#3A3A3C] text-[#FAFAFA] rounded-lg px-4 py-2 text-sm`.
  - Активный: `bg-[#FAFAFA] text-[#0A0A0A] border-transparent`.
- Располагается в `flex flex-wrap gap-2`.

**Props:**
```ts
interface FilterChipProps {
  label: string;
  active: boolean;
  onClick: () => void;
}
```

---

### 3.10. AccountSelector

Список карт / счетов с чекбоксами, используется на экране фильтров.

**Структура:**
- Секция с `Typography` заголовком (`text-sm text-muted`).
- Для каждого элемента: `div` `flex items-center gap-3 py-3`.
  - Чекбокс — `input type="checkbox"` (или кастомный через `Checkbox` из ui-kit, если есть) с `accent` стилем.
  - Иконка карты: `TransactionIcon` type="icon" с `rounded-lg` и фоном логотипа (жёлтый для T-Банк, красный/оранжевый для Mastercard).
  - Текст: `flex flex-col`.
    - `Typography` — название + маска (`Накопительный счёт · 9089`), `text-base text-primary`.
    - `Typography` — баланс (`3 675,89 ₽`), `text-sm text-muted`.
- Кнопка "Все карты" / "Все счета" с `ChevronDown`.

**Props:**
```ts
interface AccountSelectorProps {
  title: string; // "Карты" или "Счета"
  items: {
    id: string;
    name: string;
    mask: string;
    balance: string;
    iconBg: string;
    checked: boolean;
  }[];
  onToggle: (id: string) => void;
  onExpand: () => void;
}
```

---

## 4. Структура экранов

### 4.1. HistoryMain

```tsx
<main className="mx-auto flex max-w-[576px] flex-col gap-7 pt-20 pb-8">
  {/* Логотип — компонент Layout или просто img в верхнем левом углу */}
  <Logo className="self-start" />

  {/* Карточка суммы */}
  <HistorySummaryCard
    totalAmount="100 089 ₽"
    periodLabel="Расходы за апрель"
    segments={[...]}
  />

  {/* Кнопка фильтров */}
  <Button
    variant="outline"
    className="self-start gap-2 rounded-xl border-[#3A3A3C] bg-transparent text-[#FAFAFA]"
    onClick={() => navigate('/history/history-filters')}
  >
    <SlidersHorizontal size={18} />
    Фильтры
  </Button>

  {/* Список операций */}
  <HistoryList groups={historyGroups} />

  {/* Подвал */}
  <Footer />
</main>
```

**Детали:**
- Отступ между карточкой и кнопкой фильтров — `gap-7` (из контейнера).
- `HistoryList` — занимает всю ширину, внутри группы с `gap-6`.
- Каждый `HistoryItem` — без border-bottom, разделение только через даты.
- Если данных нет — показывается `HistoryOperationsList` (fallback) или `EmptyState`.

---

### 4.2. HistoryOperationsList (Error State)

```tsx
<main className="mx-auto flex max-w-[576px] flex-col items-center gap-6 pt-32 pb-8">
  <Logo className="self-start absolute top-8 left-8" />

  {/* Иконка ошибки */}
  <div className="flex h-20 w-20 items-center justify-center rounded-full bg-[#FF453A]/10">
    <X className="text-[#FF453A]" size={40} />
  </div>

  <div className="flex flex-col items-center gap-3 text-center">
    <Typography className="text-2xl font-semibold text-primary">
      Не удаётся получить историю операций
    </Typography>
    <Typography className="text-base text-muted">
      Попробуйте обновить страницу или загляните позже — мы обязательно всё починим
    </Typography>
  </div>

  <div className="flex w-full flex-col gap-3">
    <Button
      className="h-12 w-full rounded-xl bg-[#E5E5E5] text-[#0A0A0A] font-medium"
      onClick={handleRetry}
    >
      Обновить
    </Button>
    <Button
      variant="outline"
      className="h-12 w-full rounded-xl border-[#3A3A3C] bg-[#1C1C1E] text-[#FAFAFA]"
      onClick={handleLogout}
    >
      Выйти
    </Button>
  </div>

  <Footer />
</main>
```

**Детали:**
- Центрирование по вертикали: `pt-32` (или `flex-1 justify-center` внутри `min-h-screen`).
- Кнопки прибиты к нижней части контента через `mt-auto` или дополнительный `gap`.
- `Footer` — copyright и соц. иконки (X, LinkedIn, Telegram).

---

### 4.3. HistoryFilters

```tsx
<main className="mx-auto flex max-w-[576px] flex-col gap-6 pt-12 pb-8">
  {/* Шапка */}
  <div className="flex items-center gap-3">
    <Button variant="ghost" size="icon" onClick={() => navigate(-1)}>
      <ArrowLeft size={24} className="text-[#FAFAFA]" />
    </Button>
    <div className="flex items-center gap-2">
      <Typography className="text-2xl font-semibold text-primary">Фильтры</Typography>
      {activeCount > 0 && <StatusBadge variant="active">{activeCount}</StatusBadge>}
    </div>
  </div>

  {/* Тип операции */}
  <FilterSection title="Тип операции">
    <div className="flex flex-wrap gap-2">
      {operationTypes.map((t) => (
        <FilterChip key={t} label={t} active={selectedTypes.has(t)} onClick={...} />
      ))}
    </div>
  </FilterSection>

  {/* Дата или период */}
  <FilterSection title="Дата или период">
    <PeriodSelector {...periodProps} />
  </FilterSection>

  {/* Сумма */}
  <FilterSection title="Сумма">
    <div className="flex gap-3">
      <Input placeholder="0 ₽" className="bg-card border-default rounded-xl" />
      <Input placeholder="0 ₽" className="bg-card border-default rounded-xl" />
    </div>
  </FilterSection>

  {/* Переводы */}
  <FilterSection title="Переводы">
    <div className="flex flex-wrap gap-2">
      {transferTypes.map(...)} // FilterChip
    </div>
  </FilterSection>

  {/* Платежи */}
  <FilterSection title="Платежи">
    <div className="flex flex-wrap gap-2">
      {paymentTypes.map(...)} // FilterChip
    </div>
  </FilterSection>

  {/* Карты */}
  <AccountSelector title="Карты" {...cardProps} />

  {/* Счета */}
  <AccountSelector title="Счета" {...accountProps} />

  {/* Категории */}
  <FilterSection title="Категории">
    <div className="flex flex-wrap gap-2">
      {categories.map(...)} // FilterChip
    </div>
  </FilterSection>

  {/* Нижние кнопки */}
  <div className="flex gap-3 pt-4">
    <Button className="h-12 flex-1 rounded-xl bg-[#E5E5E5] text-[#0A0A0A] font-medium">
      Применить
    </Button>
    <Button
      variant="outline"
      className="h-12 flex-1 rounded-xl border-[#3A3A3C] bg-transparent text-[#FAFAFA]"
      onClick={handleReset}
    >
      Отмена
    </Button>
  </div>

  <Footer />
</main>
```

**Детали:**
- Все секции фильтров обёрнуты в `FilterSection` с `gap-6` между ними.
- `FilterSection` — `flex flex-col gap-3` (заголовок + контент).
- `Input` суммы — два поля в ряд, каждое `flex-1`.
- `AccountSelector` — чекбоксы с кастомными стилями, фон карточки `#1C1C1E`.
- Нижние кнопки sticky или просто внизу страницы.

---

## 5. Пример полной страницы (HistoryMainPage.tsx)

```tsx
import { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import { SlidersHorizontal } from 'lucide-react';
import { Button, Card, Typography } from '@rollout/ui-kit';
import { cn } from '@rollout/ui-kit';

import { HistorySummaryCard } from '@/components/HistorySummaryCard';
import { HistoryList } from '@/components/HistoryList';
import { Footer } from '@/components/Footer';

const SEGMENTS = [
  { color: '#FF375F', percent: 35 },
  { color: '#0A84FF', percent: 30 },
  { color: '#FF9F0A', percent: 25 },
  { color: '#30D158', percent: 10 },
];

const HISTORY_GROUPS = [
  {
    dateLabel: 'Сегодня, 5 ноября',
    items: [
      {
        id: '1',
        name: 'Супермаркет «Удачный»',
        description: 'Продукты',
        amount: -12867.09,
        currency: '₽',
        type: 'expense' as const,
        iconType: 'icon' as const,
        icon: 'ShoppingCart',
        iconBg: '#FF9F0A',
        badge: '+12',
      },
      {
        id: '2',
        name: 'Екатерина Ирсуевна Л.',
        description: 'Переводы • СБП • Сбер',
        amount: 10000,
        currency: '₽',
        type: 'income' as const,
        iconType: 'avatar' as const,
        iconSrc: '/avatars/ekaterina.jpg',
      },
      {
        id: '3',
        name: 'Людмила Ивановна П.',
        description: 'Переводы • СБП • Альфа-Банк',
        amount: -4675,
        currency: '₽',
        type: 'expense' as const,
        iconType: 'avatar' as const,
        iconSrc: '/avatars/ludmila.jpg',
      },
    ],
  },
  {
    dateLabel: 'Вчера, 4 ноября',
    items: [
      {
        id: '4',
        name: 'Проценты по вкладу',
        description: 'Накопления',
        amount: 234.56,
        currency: '₽',
        type: 'income' as const,
        iconType: 'icon' as const,
        icon: 'Percent',
        iconBg: '#30D158',
      },
      {
        id: '5',
        name: 'Lamoda',
        description: 'Возврат',
        amount: 16500,
        currency: '₽',
        type: 'income' as const,
        iconType: 'icon' as const,
        icon: 'Lamoda', // кастомная иконка или текст
        iconBg: '#FF375F',
      },
      {
        id: '6',
        name: 'Перевод между счетами',
        description: 'Между счетами',
        amount: 50000,
        currency: '₽',
        type: 'neutral' as const,
        iconType: 'icon' as const,
        icon: 'ArrowRightLeft',
        iconBg: '#0A84FF',
      },
    ],
  },
  {
    dateLabel: '3 ноября',
    items: [
      {
        id: '7',
        name: 'Miss you',
        description: 'Кафе и рестораны',
        amount: -1250,
        currency: '₽',
        type: 'expense' as const,
        iconType: 'icon' as const,
        icon: 'UtensilsCrossed',
        iconBg: '#FF9F0A',
        badge: '+3',
      },
      {
        id: '8',
        name: 'THE BODY',
        description: 'Красота',
        amount: -5900,
        currency: '₽',
        type: 'expense' as const,
        iconType: 'icon' as const,
        icon: 'Sparkles',
        iconBg: '#FF375F',
        badge: '+9',
      },
      {
        id: '9',
        name: 'Перевод на карту  ·· 0056',
        description: 'Переводы • Т-Банк',
        amount: -7689,
        currency: '₽',
        type: 'expense' as const,
        iconType: 'icon' as const,
        icon: 'ArrowRightLeft',
        iconBg: '#0A84FF',
      },
    ],
  },
];

export default function HistoryMainPage() {
  const navigate = useNavigate();

  return (
    <main className="mx-auto flex min-h-screen max-w-[576px] flex-col gap-7 bg-[#0A0A0A] pt-20 pb-8 px-4">
      {/* Логотип */}
      <img src="/logo.svg" alt="Rollout" className="h-8 w-8" />

      {/* Карточка расходов */}
      <HistorySummaryCard
        totalAmount="100 089 ₽"
        periodLabel="Расходы за апрель"
        segments={SEGMENTS}
      />

      {/* Фильтры */}
      <Button
        variant="outline"
        className="self-start gap-2 rounded-xl border-[#3A3A3C] bg-transparent text-[#FAFAFA] hover:bg-[#1C1C1E]"
        onClick={() => navigate('/history/history-filters')}
      >
        <SlidersHorizontal size={18} />
        Фильтры
      </Button>

      {/* Список */}
      <HistoryList groups={HISTORY_GROUPS} />

      {/* Отступ до футера */}
      <div className="mt-auto" />
      <Footer />
    </main>
  );
}
```

---

## 6. Чек-лист

### Общие требования
- [ ] Все страницы используют контейнер `mx-auto flex max-w-[576px] flex-col gap-7 pt-20 pb-8` (или `px-4` внутри).
- [ ] Фон страницы `#0A0A0A`, карточки `#1C1C1E`, бордеры `#3A3A3C`.
- [ ] Типографика через `Typography` из `@rollout/ui-kit` или кастомные классы с токенами выше.
- [ ] Иконки только из `lucide-react`.
- [ ] Все кнопки Primary имеют `bg-[#E5E5E5]`, `text-[#0A0A0A]`, `rounded-xl` (12px).
- [ ] Все Input имеют `bg-[#1C1C1E]`, `border-[#3A3A3C]`, `rounded-xl` (12px).
- [ ] Все Card имеют `bg-[#1C1C1E]`, `border-[#3A3A3C]`, `rounded-2xl` (16px).
- [ ] Используется `cn()` из `@rollout/ui-kit` для условных классов.
- [ ] Формы и фильтры управляются через `useState`.
- [ ] Роуты зарегистрированы в `react-router-dom`:
  - `/history/history-main`
  - `/history/history-operationsList`
  - `/history/history-filters`

### Компоненты
- [ ] `HistorySummaryCard` — корректная работа сегментированного прогресс-бара (сумма процентов = 100%).
- [ ] `HistoryList` — группировка по `dateLabel`, разделитель `DateDivider` с линиями по бокам.
- [ ] `HistoryItem` — корректная обработка `truncate` для длинных названий, выравнивание по базовой линии.
- [ ] `TransactionIcon` — поддержка аватарок (фото) и цветных иконок (Lucide), размер 48×48, radius 16px.
- [ ] `AmountDisplay` — формат числа `ru-RU`, 2 знака после запятой, правильный знак (`+`, `−`, без знака) и цвет.
- [ ] `StatusBadge` — pill-форма, маленький размер, используется для `+12`, `+3` и счётчика фильтров.
- [ ] `SearchBar` — иконка поиска слева, иконка очистки справа при вводе.
- [ ] `PeriodSelector` — read-only input, иконка календаря справа, формат `DD.MM.YYYY - DD.MM.YYYY`.
- [ ] `FilterChip` — переключение `active`/`inactive`, варианты для типов операций, категорий, переводов и платежей.
- [ ] `AccountSelector` — чекбокс, иконка карты/счёта, название с маской, баланс. Кнопка "Все ..." с `ChevronDown`.

### Экраны
- [ ] **HistoryMain** — отображается `HistorySummaryCard`, кнопка "Фильтры", `HistoryList` с данными по дням, подвал.
- [ ] **HistoryMain** — при пустом списке отображается `EmptyState` (или редирект на `HistoryOperationsList` при ошибке).
- [ ] **HistoryOperationsList** — центрированная иконка ошибки, заголовок, описание, кнопки "Обновить" (Primary) и "Выйти" (Outline).
- [ ] **HistoryFilters** — шапка с `ArrowLeft`, заголовок "Фильтры", бейдж с числом активных фильтров.
- [ ] **HistoryFilters** — секции: Тип операции, Дата, Сумма, Переводы, Платежи, Карты, Счета, Категории.
- [ ] **HistoryFilters** — нижние кнопки "Применить" (Primary) и "Отмена" (Outline), сброс фильтров по "Отмене".
- [ ] **HistoryFilters** — input суммы — два поля в ряд, placeholder `0 ₽`.
- [ ] **HistoryFilters** — список карт и счетов с флагами (T-Банк, Mastercard, Ozon и т.д.).

### Адаптивность и UX
- [ ] Горизонтальный скролл отсутствует (max-width 576px + responsive padding).
- [ ] Длинные названия операций обрезаются через `truncate` или `text-ellipsis`.
- [ ] Нажатия на кнопки и чипсы имеют `active:scale-[0.98]` или `hover` состояние.
- [ ] Расстояния между элементами строго по спецификации (`gap-7`, `gap-6`, `gap-3`).
- [ ] Footer (`© 2025 Rollout` + иконки соцсетей) присутствует на всех трёх экранах.
