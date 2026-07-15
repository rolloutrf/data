# Инструкция по вёрстке модуля 9-Checkout

> **Целевой путь:** `src/modules/checkout/`  
> **Стек:** Node 22, pnpm 10.32.1, Vite 8, Tailwind v4, @base-ui/react, @rollout/ui-kit, react-router-dom 6, lucide-react, Geist Variable

---

## 1. Обзор модуля

Модуль отвечает за финальный этап оформления заказа / заявки / бронирования. Всего 12 категорий чекаута, каждая со своей спецификой отображения данных.

| Категория | Роут | Тип чекаута | Особенности |
|---|---|---|---|
| Electronics | `/checkout/electronics` | Товарный | Список товаров, счётчик, корзина, рекомендации |
| Cloth | `/checkout/cloth` | Товарный | + цвет/размер под фото |
| Cosmetics | `/checkout/cosmetics` | Товарный | + объём/оттенок |
| Flower | `/checkout/flower` | Товарный | + высота/цвет букета |
| HouseGoods | `/checkout/housegoods` | Товарный | + размеры |
| Food | `/checkout/food` | Товарный | + вес/граммовка |
| Restaurant | `/checkout/restaurant` | Товарный | + вес блюда |
| Auto | `/checkout/auto` | Авто + опции | Основной товар + чекбокс-опции, кнопка «Забронировать» |
| Travel | `/checkout/travel` | Бронирование | Гости (взрослые/дети), даты, стоимость за ночи |
| RealEstate | `/checkout/realestate` | Запись на просмотр | Контакт, дата/время, объект недвижимости |
| Services | `/checkout/services` | Заявка на услугу | Контакт, время/место, комментарий |
| Jobs | `/checkout/jobs` | Отклик на вакансию | Резюме, сопроводительное, чекбокс согласия |

**Общая структура:**
- Хедер: логотип + стрелка назад + заголовок страницы
- Контент: список позиций / форма → блок итогов → CTA-кнопка
- Футер: © 2025 Rollout + иконки соцсетей (X, LinkedIn, Telegram)

---

## 2. Дизайн-система

### 2.1 Цвета

| Токен | Значение | Tailwind |
|---|---|---|
| `--bg-page` | `#0A0A0A` | `bg-[#0A0A0A]` |
| `--bg-card` | `#1C1C1E` | `bg-[#1C1C1E]` |
| `--bg-input` | `#1C1C1E` | `bg-[#1C1C1E]` |
| `--text-primary` | `#FAFAFA` | `text-[#FAFAFA]` |
| `--text-secondary` | `#A1A1AA` | `text-[#A1A1AA]` |
| `--text-muted` | `#71717A` | `text-[#71717A]` |
| `--border` | `#3A3A3C` | `border-[#3A3A3C]` |
| `--primary-btn-bg` | `#E5E5EA` | `bg-[#E5E5EA]` |
| `--primary-btn-text` | `#0A0A0A` | `text-[#0A0A0A]` |
| `--accent-red` | `#EF4444` | `text-red-500` |
| `--accent-blue` | `#3B82F6` | `text-blue-500` |
| `--segmented-bg` | `#1C1C1E` | `bg-[#1C1C1E]` |
| `--segmented-active` | `#3A3A3C` | `bg-[#3A3A3C]` |

### 2.2 Типографика

| Элемент | Размер | Вес | Цвет |
|---|---|---|---|
| Заголовок страницы | `text-xl` (20px) | 600 | `#FAFAFA` |
| Название товара | `text-sm` (14px) | 500 | `#FAFAFA` |
| Варианты товара | `text-xs` (12px) | 400 | `#A1A1AA` |
| Цена | `text-base` (16px) | 600 | `#FAFAFA` |
| Цена зачёркнутая | `text-sm` | 400 | `#71717A` line-through |
| Скидка | `text-sm` | 500 | `#EF4444` |
| Подзаголовок секции | `text-lg` (18px) | 600 | `#FAFAFA` |
| Текст кнопки | `text-base` | 600 | `#0A0A0A` |
| Плейсхолдер | `text-sm` | 400 | `#71717A` |
| Футер | `text-xs` | 400 | `#71717A` |

### 2.3 Спейсинг

```
Контейнер: mx-auto flex max-w-[576px] flex-col gap-7 pt-20 pb-8 px-4
Секции внутри: gap-6
Элементы карточки: gap-3
Отступ между товарами: gap-4
```

### 2.4 Радиусы

```
Карточка: rounded-2xl (16px)
Кнопка primary: rounded-xl (12px)
Input: rounded-xl (12px)
Товарное изображение: rounded-lg (8px)
Счётчик: rounded-xl (12px)
```

---

## 3. Компоненты UI

### 3.1 CheckoutLayout

Обёртка страницы чекаута с хедером и футером.

```tsx
import { useNavigate } from "react-router-dom";
import { ArrowLeft } from "lucide-react";
import { Typography } from "@rollout/ui-kit";

interface CheckoutLayoutProps {
  title: string;
  children: React.ReactNode;
  onBack?: () => void;
}

export function CheckoutLayout({ title, children, onBack }: CheckoutLayoutProps) {
  const navigate = useNavigate();

  return (
    <div className="mx-auto flex min-h-screen max-w-[576px] flex-col gap-7 bg-[#0A0A0A] pt-20 pb-8 text-[#FAFAFA]">
      {/* Header */}
      <header className="flex items-center gap-3 px-4">
        <button
          onClick={onBack ?? (() => navigate(-1))}
          className="flex h-10 w-10 items-center justify-center rounded-full transition hover:bg-[#1C1C1E]"
          aria-label="Назад"
        >
          <ArrowLeft className="size-5" />
        </button>
        <Typography variant="h1" className="text-xl font-semibold">
          {title}
        </Typography>
      </header>

      {/* Content */}
      <main className="flex flex-col gap-6 px-4">{children}</main>

      {/* Footer */}
      <footer className="mt-auto flex items-center justify-between px-4 text-xs text-[#71717A]">
        <span>© 2025 Rollout</span>
        <div className="flex items-center gap-3">
          <a href="#" className="hover:text-[#FAFAFA]" aria-label="X"><XIcon /></a>
          <a href="#" className="hover:text-[#FAFAFA]" aria-label="LinkedIn"><LinkedinIcon /></a>
          <a href="#" className="hover:text-[#FAFAFA]" aria-label="Telegram"><TelegramIcon /></a>
        </div>
      </footer>
    </div>
  );
}
```

**Shadcn-референс:** `Card` как обёртка, но здесь кастомный layout с fixed-логикой хедера.

---

### 3.2 ProductCheckoutItem

Товарная позиция с чекбоксом, фото, счётчиком и ценой.

```tsx
import { useState } from "react";
import { Trash2, Minus, Plus } from "lucide-react";
import { cn } from "@rollout/ui-kit";

interface ProductCheckoutItemProps {
  id: string;
  image: string;
  title: string;
  variant?: string; // "Цвет: Чёрный • Размер: XL"
  weight?: string;  // "170 гр"
  price: number;
  initialQuantity?: number;
  checked?: boolean;
  onToggle?: (id: string) => void;
  onQuantityChange?: (id: string, qty: number) => void;
  onRemove?: (id: string) => void;
}

export function ProductCheckoutItem({
  id,
  image,
  title,
  variant,
  weight,
  price,
  initialQuantity = 1,
  checked = true,
  onToggle,
  onQuantityChange,
  onRemove,
}: ProductCheckoutItemProps) {
  const [qty, setQty] = useState(initialQuantity);

  const updateQty = (delta: number) => {
    const next = Math.max(1, qty + delta);
    setQty(next);
    onQuantityChange?.(id, next);
  };

  return (
    <div className="flex flex-col gap-3">
      <div className="flex items-start gap-3">
        {/* Checkbox */}
        <button
          onClick={() => onToggle?.(id)}
          className={cn(
            "mt-1 flex h-5 w-5 shrink-0 items-center justify-center rounded border transition",
            checked
              ? "border-[#E5E5EA] bg-[#E5E5EA]"
              : "border-[#3A3A3C] bg-transparent"
          )}
        >
          {checked && <CheckIcon className="size-3.5 text-[#0A0A0A]" />}
        </button>

        {/* Image */}
        <div className="h-14 w-14 shrink-0 overflow-hidden rounded-lg bg-[#1C1C1E]">
          <img src={image} alt={title} className="h-full w-full object-cover" />
        </div>

        {/* Info */}
        <div className="flex min-w-0 flex-1 flex-col gap-0.5">
          <span className="text-sm font-medium leading-snug text-[#FAFAFA]">{title}</span>
          {variant && <span className="text-xs text-[#A1A1AA]">{variant}</span>}
          {weight && <span className="text-xs text-[#A1A1AA]">{weight}</span>}
        </div>
      </div>

      {/* Counter + Price */}
      <div className="flex items-center justify-between pl-8">
        <div className="flex items-center overflow-hidden rounded-xl border border-[#3A3A3C] bg-[#1C1C1E]">
          <button
            onClick={() => onRemove?.(id)}
            className="flex h-9 w-10 items-center justify-center text-[#A1A1AA] transition hover:text-[#FAFAFA]"
            aria-label="Удалить"
          >
            <Trash2 className="size-4" />
          </button>
          <div className="flex h-9 w-12 items-center justify-center border-x border-[#3A3A3C] text-sm text-[#FAFAFA]">
            {qty} шт
          </div>
          <button
            onClick={() => updateQty(+1)}
            className="flex h-9 w-10 items-center justify-center text-[#A1A1AA] transition hover:text-[#FAFAFA]"
            aria-label="Добавить"
          >
            <Plus className="size-4" />
          </button>
        </div>
        <span className="text-base font-semibold text-[#FAFAFA]">
          {price.toLocaleString("ru-RU")} ₽
        </span>
      </div>
    </div>
  );
}
```

**Shadcn-референс:** Составной компонент на базе `Checkbox` + `Button` (icon) + `Badge`.

---

### 3.3 OrderSummary

Блок подытогов корзины.

```tsx
interface OrderSummaryProps {
  itemCount: number;
  subtotal: number;
  discount: number;
  total: number;
  label?: string; // "Итого за 3 товара"
}

export function OrderSummary({ itemCount, subtotal, discount, total, label }: OrderSummaryProps) {
  return (
    <div className="flex flex-col gap-3">
      <h2 className="text-lg font-semibold text-[#FAFAFA]">Ваша корзина</h2>
      <div className="flex flex-col gap-2">
        <div className="flex items-center justify-between text-sm">
          <span className="text-[#A1A1AA]">{itemCount} товара</span>
          <span className="text-[#FAFAFA]">{subtotal.toLocaleString("ru-RU")} ₽</span>
        </div>
        {discount > 0 && (
          <div className="flex items-center justify-between text-sm">
            <span className="text-[#A1A1AA]">Скидка</span>
            <span className="font-medium text-red-500">
              -{discount.toLocaleString("ru-RU")} ₽
            </span>
          </div>
        )}
        {discount === 0 && (
          <div className="flex items-center justify-between text-sm">
            <span className="text-[#A1A1AA]">Скидка</span>
            <span className="font-medium text-red-500">0 ₽</span>
          </div>
        )}
        <div className="mt-1 flex items-center justify-between text-base font-semibold">
          <span className="text-[#FAFAFA]">{label ?? "Итого"}</span>
          <span className="text-[#FAFAFA]">{total.toLocaleString("ru-RU")} ₽</span>
        </div>
      </div>
    </div>
  );
}
```

**Shadcn-референс:** `Card` с `CardContent`, строки через `Separator`.

---

### 3.4 CheckoutButton

Главная CTA-кнопка чекаута.

```tsx
import { Button } from "@rollout/ui-kit";

interface CheckoutButtonProps {
  children: React.ReactNode;
  onClick?: () => void;
  disabled?: boolean;
  loading?: boolean;
}

export function CheckoutButton({ children, onClick, disabled, loading }: CheckoutButtonProps) {
  return (
    <Button
      onClick={onClick}
      disabled={disabled || loading}
      className="h-[52px] w-full rounded-xl bg-[#E5E5EA] text-base font-semibold text-[#0A0A0A] transition hover:bg-[#D4D4D8] active:scale-[0.99] disabled:opacity-50"
    >
      {loading ? "Загрузка..." : children}
    </Button>
  );
}
```

**Shadcn-референс:** `Button` с `size="lg"` + `className="w-full"`.

---

### 3.5 QuantityStepper

Счётчик количества (используется в Travel для гостей).

```tsx
import { useState } from "react";
import { Minus, Plus } from "lucide-react";

interface QuantityStepperProps {
  label: string;
  description?: string;
  min?: number;
  max?: number;
  initial?: number;
  onChange?: (val: number) => void;
}

export function QuantityStepper({
  label,
  description,
  min = 0,
  max = 99,
  initial = 1,
  onChange,
}: QuantityStepperProps) {
  const [value, setValue] = useState(initial);

  const update = (delta: number) => {
    const next = Math.max(min, Math.min(max, value + delta));
    setValue(next);
    onChange?.(next);
  };

  return (
    <div className="flex items-center justify-between">
      <div className="flex flex-col">
        <span className="text-base font-medium text-[#FAFAFA]">{label}</span>
        {description && <span className="text-sm text-[#A1A1AA]">{description}</span>}
      </div>
      <div className="flex items-center overflow-hidden rounded-xl border border-[#3A3A3C] bg-[#1C1C1E]">
        <button
          onClick={() => update(-1)}
          disabled={value <= min}
          className="flex h-10 w-10 items-center justify-center text-[#A1A1AA] transition hover:text-[#FAFAFA] disabled:opacity-30"
        >
          <Minus className="size-4" />
        </button>
        <div className="flex h-10 w-12 items-center justify-center text-sm text-[#FAFAFA]">{value}</div>
        <button
          onClick={() => update(+1)}
          disabled={value >= max}
          className="flex h-10 w-10 items-center justify-center text-[#A1A1AA] transition hover:text-[#FAFAFA] disabled:opacity-30"
        >
          <Plus className="size-4" />
        </button>
      </div>
    </div>
  );
}
```

**Shadcn-референс:** Кастомный компонент на базе `Button` (ghost, icon).

---

### 3.6 ContactInfoRow

Строка контактной информации с переходом (используется в RealEstate, Services).

```tsx
import { ChevronRight } from "lucide-react";

interface ContactInfoRowProps {
  title: string;
  name: string;
  subtitle?: string;
  onClick?: () => void;
}

export function ContactInfoRow({ title, name, subtitle, onClick }: ContactInfoRowProps) {
  return (
    <div className="flex flex-col gap-2">
      <span className="text-base font-medium text-[#FAFAFA]">{title}</span>
      <button
        onClick={onClick}
        className="flex items-center justify-between rounded-xl bg-[#1C1C1E] p-4 transition hover:bg-[#27272A]"
      >
        <div className="flex flex-col items-start text-left">
          <span className="text-sm text-[#FAFAFA]">{name}</span>
          {subtitle && <span className="text-sm text-[#A1A1AA]">{subtitle}</span>}
        </div>
        <ChevronRight className="size-5 text-[#71717A]" />
      </button>
    </div>
  );
}
```

**Shadcn-референс:** `Button` variant ghost + `Card`.

---

### 3.7 FileUploadZone

Зона загрузки файла (для Jobs — резюме).

```tsx
import { Upload } from "lucide-react";

interface FileUploadZoneProps {
  label: string;
  onUpload?: (file: File) => void;
}

export function FileUploadZone({ label, onUpload }: FileUploadZoneProps) {
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const file = e.target.files?.[0];
    if (file) onUpload?.(file);
  };

  return (
    <div className="flex flex-col gap-2">
      <span className="text-base font-medium text-[#FAFAFA]">{label}</span>
      <label className="flex h-24 cursor-pointer flex-col items-center justify-center gap-2 rounded-xl border border-dashed border-[#3A3A3C] bg-[#1C1C1E] transition hover:border-[#A1A1AA]">
        <Upload className="size-5 text-[#A1A1AA]" />
        <input type="file" className="hidden" onChange={handleChange} accept=".pdf,.doc,.docx" />
      </label>
    </div>
  );
}
```

**Shadcn-референс:** `Input` type file с кастомным стилем-обёрткой.

---

### 3.8 InfoRow

Простая строка «label — value» (для Travel, итоговых блоков).

```tsx
interface InfoRowProps {
  label: string;
  value: string;
  bold?: boolean;
}

export function InfoRow({ label, value, bold }: InfoRowProps) {
  return (
    <div className="flex items-center justify-between text-sm">
      <span className={bold ? "text-base font-semibold text-[#FAFAFA]" : "text-[#A1A1AA]"}>
        {label}
      </span>
      <span className={bold ? "text-base font-semibold text-[#FAFAFA]" : "text-[#FAFAFA]"}>
        {value}
      </span>
    </div>
  );
}
```

**Shadcn-референс:** `div` с `justify-between`, можно обернуть в `CardContent`.

---

### 3.9 CheckboxOption

Чекбокс-опция с изображением (для Auto доп. опций).

```tsx
import { useState } from "react";

interface CheckboxOptionProps {
  id: string;
  image?: string;
  label: string;
  price: number | string;
  checked?: boolean;
  onToggle?: (id: string) => void;
}

export function CheckboxOption({ id, image, label, price, checked = false, onToggle }: CheckboxOptionProps) {
  return (
    <div className="flex flex-col gap-3">
      <div className="flex items-center gap-3">
        <button
          onClick={() => onToggle?.(id)}
          className={cn(
            "flex h-5 w-5 shrink-0 items-center justify-center rounded border transition",
            checked
              ? "border-[#E5E5EA] bg-[#E5E5EA]"
              : "border-[#3A3A3C] bg-transparent"
          )}
        >
          {checked && <CheckIcon className="size-3.5 text-[#0A0A0A]" />}
        </button>
        {image && (
          <div className="h-12 w-12 shrink-0 overflow-hidden rounded-lg bg-[#1C1C1E]">
            <img src={image} alt={label} className="h-full w-full object-cover" />
          </div>
        )}
        <span className="text-sm text-[#FAFAFA]">{label}</span>
      </div>
      {checked && (
        <div className="pl-8">
          <ProductCheckoutItem id={`${id}-qty`} title="" price={typeof price === "number" ? price : 0} variant="" />
          {/* или отдельный inline-counter */}
        </div>
      )}
    </div>
  );
}
```

**Shadcn-референс:** `Checkbox` + `Label` + `Card`.

---

### 3.10 RecommendationCard

Карточка товара в блоке «Подобрали для вас» (2 колонки).

```tsx
import { Heart } from "lucide-react";
import { Button, Badge } from "@rollout/ui-kit";

interface RecommendationCardProps {
  image: string;
  title: string;
  price: number;
  oldPrice?: number;
  discount?: string;
  bonus?: string;
  delivery?: string;
  rating?: number;
  reviews?: number;
  store?: string;
  actionLabel?: string;
}

export function RecommendationCard({
  image,
  title,
  price,
  oldPrice,
  discount,
  bonus,
  delivery,
  rating,
  reviews,
  store,
  actionLabel = "В корзину",
}: RecommendationCardProps) {
  return (
    <div className="flex flex-col gap-2">
      <div className="relative aspect-square overflow-hidden rounded-xl bg-[#1C1C1E]">
        <img src={image} alt={title} className="h-full w-full object-cover" />
      </div>
      <span className="line-clamp-2 text-sm font-medium text-[#FAFAFA]">{title}</span>
      <div className="flex items-center gap-2">
        <span className="text-base font-semibold text-[#FAFAFA]">{price.toLocaleString("ru-RU")} ₽</span>
        {oldPrice && <span className="text-sm text-[#71717A] line-through">{oldPrice.toLocaleString("ru-RU")} ₽</span>}
        {discount && <span className="text-sm text-red-500">{discount}</span>}
      </div>
      {bonus && <span className="text-xs text-blue-500">{bonus}</span>}
      {delivery && <span className="text-xs text-[#A1A1AA]">{delivery}</span>}
      {rating !== undefined && (
        <div className="flex items-center gap-1 text-xs text-[#A1A1AA]">
          <span>{store}</span>
          <span className="text-yellow-500">★</span>
          <span>{rating}</span>
          <span>({reviews})</span>
        </div>
      )}
      <div className="flex items-center gap-2">
        <Button
          variant="outline"
          className="h-9 flex-1 rounded-xl border-[#3A3A3C] bg-transparent text-sm text-[#FAFAFA] hover:bg-[#1C1C1E]"
        >
          {actionLabel}
        </Button>
        <button className="flex h-9 w-9 items-center justify-center rounded-xl border border-[#3A3A3C] text-[#A1A1AA] transition hover:text-red-500">
          <Heart className="size-4" />
        </button>
      </div>
    </div>
  );
}
```

**Shadcn-референс:** `Card` + `CardContent` + `Button` outline.

---

## 4. Структура экранов

### 4.1 Товарные чекауты (Electronics, Cloth, Cosmetics, Flower, HouseGoods, Food, Restaurant)

**Layout:**
```
CheckoutLayout
├── ProductCheckoutItem[]
├── OrderSummary
├── CheckoutButton
└── RecommendationsGrid (2 колонки, RecommendationCard[])
```

**Состояния:**
- `items: CartItem[]` — товары с qty/checked
- `recommendations: Product[]`
- `isSubmitting: boolean`

**Особенности по категориям:**
| Категория | Поле variant | Поле weight | Рекомендации |
|---|---|---|---|
| Electronics | — | — | телефоны, срок доставки, рассрочка |
| Cloth | Цвет • Размер | — | + палитра цветов, размерный ряд |
| Cosmetics | Цвет • Размер | — | + доставка 1 день |
| Flower | Цвет • Размер | — | + доставка 30 мин |
| HouseGoods | Цвет • Размер | — | + бонусы, доставка 1-2 дня |
| Food | — | 170 гр | + бонусы |
| Restaurant | — | 170 гр | + бонусы, доставка за час |

### 4.2 Auto Checkout

**Layout:**
```
CheckoutLayout
├── ProductCheckoutItem (основной товар)
├── h2: Дополнительные опции
├── CheckboxOption[]
├── OrderSummary
└── CheckoutButton (Забронировать)
```

**Состояния:**
- `mainItem: CartItem`
- `options: Option[]` — id, label, image, price, checked, qty

### 4.3 Travel Checkout

**Layout:**
```
CheckoutLayout
├── ProductCard (отель: фото + название)
├── h2: Гости
│   ├── QuantityStepper (Взрослые, от 18 лет)
│   └── QuantityStepper (Дети, до 18 лет)
├── h2: Даты
│   └── span: 26 фев — 3 марта
├── h2: Стоимость бронирования
│   ├── InfoRow (6 ночей → 325 780 ₽)
│   ├── InfoRow (1 человек → 0 ₽)
│   └── InfoRow (Итого → 325 780 ₽, bold)
└── CheckoutButton (Оформить)
```

**Состояния:**
- `adults: number`
- `children: number`
- `dates: { from: string; to: string }`
- `pricePerNight: number`

### 4.4 RealEstate Checkout

**Layout:**
```
CheckoutLayout (title: Запись на просмотр)
├── ContactInfoRow (Контактное лицо)
├── ContactInfoRow (Дата и время просмотра)
├── h2: Ваш заказ
│   └── ProductCheckoutItem (упрощённый: без счётчика)
└── CheckoutButton (Записаться на просмотр)
```

**Состояния:**
- `contact: { name; phone }`
- `viewingDate: string`
- `property: PropertyItem`

### 4.5 Services Checkout

**Layout:**
```
CheckoutLayout (title: Оформление заявки)
├── ProductCard (услуга: фото + название)
├── ContactInfoRow (Контактная информация)
├── ContactInfoRow (Время визита)
├── ContactInfoRow (Адрес)
├── TextArea (Комментарий)
└── CheckoutButton (Оставить заявку)
```

**Состояния:**
- `service: ServiceItem`
- `contact: Contact`
- `datetime: string`
- `address: string`
- `comment: string`

### 4.6 Jobs Checkout

**Layout:**
```
CheckoutLayout (title: Отклик на вакансию)
├── ProductCard (вакансия: фото + название)
├── ContactInfoRow (Контакты)
├── FileUploadZone (Загрузите ваше резюме)
├── TextArea (Сопроводительное письмо)
├── Checkbox (Я согласен на обработку персональных данных)
└── CheckoutButton (Откликнуться)
```

**Состояния:**
- `vacancy: Vacancy`
- `contact: Contact`
- `resume: File | null`
- `coverLetter: string`
- `agreed: boolean`

---

## 5. Пример полной страницы

### ElectronicsCheckoutPage.tsx

```tsx
import { useState } from "react";
import { CheckoutLayout } from "../components/CheckoutLayout";
import { ProductCheckoutItem } from "../components/ProductCheckoutItem";
import { OrderSummary } from "../components/OrderSummary";
import { CheckoutButton } from "../components/CheckoutButton";
import { RecommendationCard } from "../components/RecommendationCard";

interface CartItem {
  id: string;
  image: string;
  title: string;
  price: number;
  qty: number;
  checked: boolean;
}

const MOCK_ITEMS: CartItem[] = [
  {
    id: "1",
    image: "/images/iphone16.jpg",
    title: "Apple Смартфон iPhone 16 Pro Max Black Titanium, nano SIM + eSIM сим-карты 256 ГБ",
    price: 115_675,
    qty: 1,
    checked: true,
  },
  {
    id: "2",
    image: "/images/iphone16.jpg",
    title: "Apple Смартфон iPhone 16 Pro Max Black Titanium, nano SIM + eSIM сим-карты 256 ГБ",
    price: 115_675,
    qty: 1,
    checked: true,
  },
  {
    id: "3",
    image: "/images/iphone16.jpg",
    title: "Apple Смартфон iPhone 16 Pro Max Black Titanium, nano SIM + eSIM сим-карты 256 ГБ",
    price: 115_675,
    qty: 1,
    checked: true,
  },
];

const MOCK_RECOMMENDATIONS = [
  {
    image: "/images/rec1.jpg",
    title: "Смартфон Phone 16 Pro Max Black Titanium",
    price: 69_500,
    oldPrice: 79_500,
    discount: "-10 %",
    delivery: "3-5 дней",
    rating: 4.2,
    reviews: 19,
    store: "Alexstore",
  },
  // ... ещё 3 карточки
];

export default function ElectronicsCheckoutPage() {
  const [items, setItems] = useState(MOCK_ITEMS);
  const [isSubmitting, setIsSubmitting] = useState(false);

  const toggleItem = (id: string) => {
    setItems((prev) =>
      prev.map((i) => (i.id === id ? { ...i, checked: !i.checked } : i))
    );
  };

  const updateQty = (id: string, qty: number) => {
    setItems((prev) => prev.map((i) => (i.id === id ? { ...i, qty } : i)));
  };

  const removeItem = (id: string) => {
    setItems((prev) => prev.filter((i) => i.id !== id));
  };

  const checkedItems = items.filter((i) => i.checked);
  const subtotal = checkedItems.reduce((s, i) => s + i.price * i.qty, 0);
  const discount = 52_250; // mock
  const total = subtotal - discount;

  const handleSubmit = () => {
    setIsSubmitting(true);
    setTimeout(() => setIsSubmitting(false), 2000);
  };

  return (
    <CheckoutLayout title="Оформление заказа">
      {/* Items */}
      <div className="flex flex-col gap-4">
        {items.map((item) => (
          <ProductCheckoutItem
            key={item.id}
            id={item.id}
            image={item.image}
            title={item.title}
            price={item.price}
            initialQuantity={item.qty}
            checked={item.checked}
            onToggle={toggleItem}
            onQuantityChange={updateQty}
            onRemove={removeItem}
          />
        ))}
      </div>

      {/* Order Summary */}
      <OrderSummary
        itemCount={checkedItems.reduce((s, i) => s + i.qty, 0)}
        subtotal={subtotal}
        discount={discount}
        total={total}
        label={`Итого за ${checkedItems.length} товара`}
      />

      {/* CTA */}
      <CheckoutButton onClick={handleSubmit} loading={isSubmitting}>
        Оформить
      </CheckoutButton>

      {/* Recommendations */}
      <div className="flex flex-col gap-4">
        <h2 className="text-lg font-semibold text-[#FAFAFA]">Подобрали для вас</h2>
        <div className="grid grid-cols-2 gap-4">
          {MOCK_RECOMMENDATIONS.map((rec, idx) => (
            <RecommendationCard key={idx} {...rec} />
          ))}
        </div>
      </div>
    </CheckoutLayout>
  );
}
```

---

## 6. Чек-лист

### Общие требования
- [ ] Все 12 роутов зарегистрированы в `react-router-dom`
- [ ] Контейнер `max-w-[576px] mx-auto` на всех страницах
- [ ] Тёмная тема: фон `#0A0A0A`, текст `#FAFAFA`
- [ ] Шрифт Geist Variable подключён глобально
- [ ] Иконки lucide-react, размер `size-5` (20px) для навигации, `size-4` для контролов
- [ ] Все кнопки и интерактивные элементы имеют `hover` и `active` состояния
- [ ] Футер с © 2025 Rollout и соц. иконками на каждой странице

### Компоненты
- [ ] `CheckoutLayout` — переиспользуемая обёртка с хедером/футером
- [ ] `ProductCheckoutItem` — чекбокс, фото, счётчик, цена
- [ ] `OrderSummary` — подытог, скидка, итог
- [ ] `CheckoutButton` — primary CTA, ~52px высота
- [ ] `QuantityStepper` — для Travel (гости) и счётчиков количества
- [ ] `ContactInfoRow` — строка с чевроном для RealEstate/Services
- [ ] `FileUploadZone` — dashed border, скрытый input type file
- [ ] `InfoRow` — label/value строка
- [ ] `CheckboxOption` — чекбокс с изображением для Auto
- [ ] `RecommendationCard` — 2-колоночная карточка товара

### Товарные чекауты (7 шт.)
- [ ] Electronics — список товаров, корзина, рекомендации
- [ ] Cloth — + цвет/размер под названием
- [ ] Cosmetics — + оттенок/объём
- [ ] Flower — + параметры букета
- [ ] HouseGoods — + размеры
- [ ] Food — + вес (гр)
- [ ] Restaurant — + вес блюда, бонусы

### Специализированные чекауты (5 шт.)
- [ ] Auto — основной товар + доп. опции с чекбоксами, кнопка «Забронировать»
- [ ] Travel — гости (взрослые/дети), даты, стоимость за ночи
- [ ] RealEstate — контакт, дата/время просмотра, объект, кнопка «Записаться на просмотр»
- [ ] Services — контакт, время/место, комментарий, кнопка «Оставить заявку»
- [ ] Jobs — контакт, загрузка резюме, сопроводительное, чекбокс согласия, кнопка «Откликнуться»

### Состояния и обработка
- [ ] `useState` для всех форм и списков
- [ ] Mock-обработчики без `console.log`
- [ ] Управление checked-состоянием товаров
- [ ] Управление quantity (минимум 1)
- [ ] Удаление товара из списка
- [ ] Состояние загрузки (loading) на CTA-кнопках

### Адаптив и accessibility
- [ ] `aria-label` на всех иконках-кнопках
- [ ] `line-clamp-2` на длинных названиях товаров
- [ ] `disabled` состояния на кнопках при недопустимых значениях
- [ ] Hover-эффекты на интерактивных элементах
