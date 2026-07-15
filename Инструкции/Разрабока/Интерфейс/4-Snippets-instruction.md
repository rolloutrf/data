# Инструкция по вёрстке модуля 4-Snippets

> **Файл:** `/Users/mokoloskov/Documents/kimi/workspace/4-Snippets-instruction.md`  
> **Скриншоты:** `/Users/mokoloskov/Documents/kimi/workspace/4-Snippets_screenshots/` (51 скриншот)  
> **Figma-file:** `Zy2zp3A6V5RHH8OQxt19Ow`  
> **Дата:** 2026-06-17

---

## 1. Обзор модуля

### 1.1 Назначение
Модуль **4-Snippets** — это центральный каталог (marketplace) приложения Rollout. Пользователь видит:
- Главную страницу с рекомендациями товаров (горизонтальная карусель + вертикальная сетка).
- Страницу «Супер-апп» — сетку категорий (Электроника, Одежда, Косметика, Цветы, Доставка еды, Авто, Бронирование и т.д.).
- Отдельные вертикали: Недвижимость, Еда, Рестораны, Услуги, Работа, Авто, Путешествия, Электроника, Одежда, Косметика, Товары для дома, Цветы.
- Экраны фильтров с чекбоксами, чипами, сегментированными переключателями, слайдером цены и выпадающими списками.

### 1.2 Экраны (страницы Figma)

| # | Страница Figma | layout-фреймы | Назначение |
|---|----------------|---------------|------------|
| 1 | **Cover** | 1 | Обложка файла |
| 2 | **Snippets** | 3 | Главная: карусель рекомендаций + сетка товаров |
| 3 | **Filter** | 8 + 2 Sheet | Фильтры: категории, чекбоксы, чипы, слайдер цены, выпадающие списки |
| 4 | **SupperApp** | 10 | «Супер-апп»: сетка категорий + поиск |
| 5 | **Electronics** | 2 | Вертикаль Электроника |
| 6 | **Cloth** | 2 | Вертикаль Одежда |
| 7 | **Cosmetics** | 2 | Вертикаль Косметика |
| 8 | **Houses Goods** | 2 | Вертикаль Товары для дома |
| 9 | **Flower** | 2 | Вертикаль Цветы |
| 10 | **Food** | 2 | Вертикаль Доставка еды |
| 11 | **Restaurant** | 2 | Вертикаль Рестораны |
| 12 | **RealEstate** | 6 | Вертикаль Недвижимость: категории, сервисы, кнопка «Разместить объявление» |
| 13 | **Travel** | 2 | Вертикаль Путешествия |
| 14 | **Auto** | 2 | Вертикаль Авто |
| 15 | **Services** | 2 | Вертикаль Услуги: поиск, фильтры, категории |
| 16 | **Jobs** | 2 | Вертикаль Работа: табы, поиск, фильтры, категории |
| 17 | **Common** | 13 | Переиспользуемые компоненты (категории, сервисы, поиск, toggle-группа) |

> Пустые страницы (`------`, `---`, `-----`, `SERP`) не содержат layout-фреймов и в вёрстке не нуждаются.

### 1.3 Роуты (URL-пути)

```
/snippets                 → Snippets (главная)
/snippets/filter          → Filter
/snippets/supperapp       → SupperApp
/snippets/electronics     → Electronics
/snippets/cloth           → Cloth
/snippets/cosmetics       → Cosmetics
/snippets/houses-goods    → Houses Goods
/snippets/flowers         → Flowers
/snippets/food            → Food
/snippets/restaurants     → Restaurants
/snippets/real-estate     → Real Estate
/snippets/travel          → Travel
/snippets/auto            → Auto
/snippets/services        → Services
/snippets/jobs            → Jobs
```

---

## 2. Дизайн-система (из макетов)

### 2.1 Цветовая палитра

| Токен | Значение | Использование |
|-------|----------|---------------|
| `bg-primary` | `#0A0A0A` | Фон всего приложения |
| `bg-card` | `#1C1C1E` | Фон карточек, input, tabs, badges |
| `bg-elevated` | `#2C2C2E` | Поднятые элементы (hover-фон кнопок) |
| `border-default` | `#3A3A3C` | Границы карточек, input, чипов |
| `text-primary` | `#FAFAFA` | Основной текст, заголовки |
| `text-secondary` | `#E5E5E5` | Вторичный текст, активные элементы |
| `text-muted` | `#8E8E93` | Подписи, описания |
| `text-placeholder` | `#636366` | Placeholder в input |
| `accent-red` | `#FF453A` | Скидки, цена со скидкой (crossed-out), ошибки |
| `accent-yellow` | `#FFD60A` | Звёзды рейтинга |
| `bg-button-primary` | `#E5E5E5` | Primary кнопка (светло-серый) |
| `text-button-primary` | `#0A0A0A` | Текст primary кнопки |
| `bg-button-secondary` | `transparent` | Secondary кнопка |
| `border-button-secondary` | `#3A3A3C` | Border secondary кнопки |
| `slider-track` | `#3A3A3C` | Track слайдера |
| `slider-fill` | `#E5E5E5` | Fill слайдера |
| `white` | `#FFFFFF` | Логотип, иконки |

### 2.2 Типографика (шрифт Geist Variable)

| Элемент | Размер | Вес | Line-height | Letter-spacing |
|---------|--------|-----|-------------|----------------|
| H1 (заголовок страницы) | 32px / `text-[32px]` | 600 | 1.1 | -0.02em |
| H2 (секция) | 24px / `text-2xl` | 600 | 1.2 | -0.01em |
| H3 (карточка, название) | 18px / `text-lg` | 500 | 1.3 | 0 |
| Body | 16px / `text-base` | 400 | 1.5 | 0 |
| Caption | 14px / `text-sm` | 400 | 1.4 | 0 |
| Caption-small | 12px / `text-xs` | 400 | 1.3 | 0.01em |
| Button | 16px / `text-base` | 500 | 1 | 0 |
| Price (жирный) | 20px / `text-xl` | 700 | 1.2 | -0.01em |
| Price-old (зачёркнутый) | 16px / `text-base` | 400 | 1.2 | 0 |

### 2.3 Спейсинг, радиусы, размеры

| Элемент | Значение |
|---------|----------|
| Контейнер max-width | `576px` |
| Контейнер padding-x | `16px` (`px-4`) |
| Page gap (между секциями) | `28px` (`gap-7`) |
| Card padding | `16px` (`p-4`) |
| Card border-radius | `16px` (`rounded-2xl`) |
| Input border-radius | `12px` (`rounded-xl`) |
| Chip border-radius | `24px` (`rounded-full`) |
| Button border-radius | `12px` (`rounded-xl`) |
| Category-card border-radius | `16px` (`rounded-2xl`) |
| Icon size (inline) | `20px` (`size-5`) |
| Icon size (button) | `24px` (`size-6`) |
| Tab-bar height | `64px` |
| Header pt | `80px` (`pt-20`) |
| Bottom pb (mobile) | `tabbar-height` (`pb-tabbar`) |
| Grid gap (categories) | `12px` (`gap-3`) |
| Grid gap (products) | `12px` (`gap-3`) |
| Carousel peek | `16px` (видна часть следующей карточки) |

### 2.4 Иконки

- **Библиотека:** `lucide-react`
- **Размеры:** `size-5` (20px) для inline, `size-6` (24px) для кнопок
- **Цвет:** `text-[#FAFAFA]` (primary) или `text-[#8E8E93]` (muted)
- **Стиль:** outline (не filled)

Частые иконки:
- `Search`, `SlidersHorizontal`, `ArrowUpDown`, `ChevronDown`, `ChevronLeft`, `Heart`, `ShoppingCart`, `User`, `Settings`, `Star`, `Truck`, `Clock`, `Check`, `X`, `MapPin`, `Briefcase`, `Home`, `Car`, `UtensilsCrossed`, `Plane`, `Flower2`, `Shirt`, `Sparkles`, `Wrench`

---

## 3. Компоненты UI

### 3.1 Logo

```tsx
import { RolloutLogo } from "@rollout/ui-kit";

// Или кастомный SVG, если не экспортирован
export function AppLogo() {
  return (
    <div className="flex items-center gap-2">
      {/* Rollout logo — белый абстрактный символ */}
      <svg width="32" height="32" viewBox="0 0 32 32" fill="none">
        <path d="M..." fill="white" />
      </svg>
    </div>
  );
}
```

> shadcn-референс: нет (кастомный SVG).  
> Реализация: кастомный компонент с SVG.

---

### 3.2 SearchInput

Поле поиска с иконкой лупы. Используется почти на всех экранах.

```tsx
import { Input } from "@rollout/ui-kit";
import { Search } from "lucide-react";

interface SearchInputProps {
  value: string;
  onChange: (value: string) => void;
  placeholder?: string;
}

export function SearchInput({ value, onChange, placeholder = "Поиск" }: SearchInputProps) {
  return (
    <div className="relative">
      <Search className="absolute left-4 top-1/2 size-5 -translate-y-1/2 text-[#8E8E93]" />
      <Input
        type="text"
        value={value}
        onChange={(e) => onChange(e.target.value)}
        placeholder={placeholder}
        className="h-12 w-full rounded-xl border border-[#3A3A3C] bg-[#1C1C1E] pl-11 pr-4 text-base text-[#FAFAFA] placeholder:text-[#636366] focus:border-[#E5E5E5] focus:outline-none"
      />
    </div>
  );
}
```

> shadcn-референс: `Input`  
> Реализация: `@rollout/ui-kit` → `Input` + `lucide-react` иконка

---

### 3.3 CategoryCard

Карточка категории с изображением и подписью. Используется в SupperApp и вертикалях.

```tsx
import { Card } from "@rollout/ui-kit";
import { cn } from "@rollout/ui-kit";

interface CategoryCardProps {
  image: string;
  title: string;
  onClick?: () => void;
  className?: string;
}

export function CategoryCard({ image, title, onClick, className }: CategoryCardProps) {
  return (
    <button
      onClick={onClick}
      className={cn(
        "flex flex-col items-center gap-2 rounded-2xl bg-[#1C1C1E] p-3 transition-colors hover:bg-[#2C2C2E]",
        className
      )}
    >
      <div className="aspect-square w-full overflow-hidden rounded-xl">
        <img src={image} alt={title} className="h-full w-full object-cover" />
      </div>
      <span className="text-sm font-medium text-[#FAFAFA]">{title}</span>
    </button>
  );
}
```

> shadcn-референс: `Card`  
> Реализация: `@rollout/ui-kit` → `Card` или кастомный `button` + `cn`

---

### 3.4 CategoryRowCard

Горизонтальная карточка категории (иконка слева + текст справа). Используется в RealEstate, Services.

```tsx
interface CategoryRowCardProps {
  icon: React.ReactNode;
  title: string;
  onClick?: () => void;
}

export function CategoryRowCard({ icon, title, onClick }: CategoryRowCardProps) {
  return (
    <button
      onClick={onClick}
      className="flex items-center gap-3 rounded-2xl border border-[#3A3A3C] bg-[#1C1C1E] p-4 text-left transition-colors hover:bg-[#2C2C2E]"
    >
      <div className="flex size-12 shrink-0 items-center justify-center rounded-xl bg-[#2C2C2E]">
        {icon}
      </div>
      <span className="text-base font-medium text-[#FAFAFA]">{title}</span>
    </button>
  );
}
```

---

### 3.5 ProductCard

Карточка товара с изображением, названием, ценой, скидкой, доставкой, рейтингом и кнопками.

```tsx
import { Button, Badge } from "@rollout/ui-kit";
import { Heart, ShoppingCart, Truck, Clock, Star } from "lucide-react";
import { cn } from "@rollout/ui-kit";

interface ProductCardProps {
  id: string;
  image: string;
  title: string;
  price: number;
  oldPrice?: number;
  discount?: string;
  colors?: string[];
  deliveryDays?: string;
  installment?: boolean;
  storeName: string;
  rating: number;
  reviewCount: number;
  createdAt: string;
  onAddToCart?: (id: string) => void;
  onFavorite?: (id: string) => void;
}

export function ProductCard({
  image,
  title,
  price,
  oldPrice,
  discount,
  colors = [],
  deliveryDays,
  installment,
  storeName,
  rating,
  reviewCount,
  createdAt,
  onAddToCart,
  onFavorite,
}: ProductCardProps) {
  return (
    <div className="flex flex-col gap-3 rounded-2xl bg-[#1C1C1E] p-3">
      {/* Image */}
      <div className="relative aspect-square overflow-hidden rounded-xl bg-[#2C2C2E]">
        <img src={image} alt={title} className="h-full w-full object-cover" />
      </div>

      {/* Title */}
      <h3 className="line-clamp-2 text-base font-medium text-[#FAFAFA]">{title}</h3>

      {/* Price */}
      <div className="flex items-baseline gap-2">
        <span className="text-xl font-bold text-[#FAFAFA]">{price.toLocaleString("ru-RU")} ₽</span>
        {oldPrice && (
          <span className="text-base text-[#8E8E93] line-through">
            {oldPrice.toLocaleString("ru-RU")} ₽
          </span>
        )}
        {discount && <span className="text-sm font-medium text-[#FF453A]">{discount}</span>}
      </div>

      {/* Colors */}
      {colors.length > 0 && (
        <div className="flex items-center gap-1.5">
          {colors.slice(0, 4).map((color, i) => (
            <div
              key={i}
              className="size-5 rounded-full border border-[#3A3A3C]"
              style={{ backgroundColor: color }}
            />
          ))}
          {colors.length > 4 && (
            <span className="text-sm text-[#8E8E93]">+{colors.length - 4} цвета</span>
          )}
        </div>
      )}

      {/* Meta */}
      <div className="flex flex-col gap-1 text-sm text-[#8E8E93]">
        {deliveryDays && (
          <div className="flex items-center gap-1.5">
            <Truck className="size-4" />
            <span>{deliveryDays}</span>
          </div>
        )}
        {installment && (
          <div className="flex items-center gap-1.5">
            <Clock className="size-4" />
            <span>Можно в рассрочку</span>
          </div>
        )}
      </div>

      {/* Store & Rating */}
      <div className="flex items-center gap-2">
        <span className="text-sm font-semibold text-[#FAFAFA]">{storeName}</span>
        <div className="flex items-center gap-0.5">
          <Star className="size-4 fill-[#FFD60A] text-[#FFD60A]" />
          <span className="text-sm text-[#FAFAFA]">{rating}</span>
          <span className="text-sm text-[#8E8E93]">({reviewCount})</span>
        </div>
      </div>

      {/* Created at */}
      <span className="text-xs text-[#8E8E93]">{createdAt}</span>

      {/* Actions */}
      <div className="flex gap-2">
        <Button
          onClick={() => onAddToCart?.(id)}
          className="flex-1 rounded-xl bg-[#E5E5E5] text-[#0A0A0A] hover:bg-white"
        >
          <ShoppingCart className="mr-2 size-5" />
          В корзину
        </Button>
        <Button
          variant="outline"
          onClick={() => onFavorite?.(id)}
          className="size-11 rounded-xl border-[#3A3A3C] bg-transparent p-0 hover:bg-[#2C2C2E]"
        >
          <Heart className="size-5 text-[#FAFAFA]" />
        </Button>
      </div>
    </div>
  );
}
```

> shadcn-референс: `Card`, `Button`, `Badge`  
> Реализация: `@rollout/ui-kit` → `Button`, `Badge`, `cn`

---

### 3.6 FilterCheckbox

Чекбокс для фильтров. Может быть вложенным (indent).

```tsx
import { Checkbox } from "@rollout/ui-kit";
import { cn } from "@rollout/ui-kit";

interface FilterCheckboxProps {
  label: string;
  checked: boolean;
  onChange: (checked: boolean) => void;
  indent?: boolean;
  count?: number;
}

export function FilterCheckbox({ label, checked, onChange, indent = false, count }: FilterCheckboxProps) {
  return (
    <label
      className={cn(
        "flex cursor-pointer items-center gap-3 py-2",
        indent && "pl-8"
      )}
    >
      <Checkbox
        checked={checked}
        onCheckedChange={onChange}
        className="size-5 rounded border-[#3A3A3C] bg-[#1C1C1E] data-[state=checked]:border-[#E5E5E5] data-[state=checked]:bg-[#E5E5E5] data-[state=checked]:text-[#0A0A0A]"
      />
      <span className="text-base text-[#FAFAFA]">{label}</span>
      {count !== undefined && (
        <span className="ml-auto text-sm text-[#8E8E93]">{count}</span>
      )}
    </label>
  );
}
```

> shadcn-референс: `Checkbox`  
> Реализация: `@rollout/ui-kit` → `Checkbox`

---

### 3.7 FilterChip

Кнопка-фильтр (чип) для выбора одного или нескольких значений.

```tsx
import { cn } from "@rollout/ui-kit";

interface FilterChipProps {
  label: string;
  active?: boolean;
  onClick?: () => void;
  fullWidth?: boolean;
}

export function FilterChip({ label, active = false, onClick, fullWidth = false }: FilterChipProps) {
  return (
    <button
      onClick={onClick}
      className={cn(
        "rounded-full border px-5 py-2.5 text-base font-medium transition-colors",
        active
          ? "border-[#E5E5E5] bg-[#E5E5E5] text-[#0A0A0A]"
          : "border-[#3A3A3C] bg-[#1C1C1E] text-[#FAFAFA] hover:bg-[#2C2C2E]",
        fullWidth && "w-full"
      )}
    >
      {label}
    </button>
  );
}
```

> shadcn-референс: `Badge` / `Toggle`  
> Реализация: кастомный `button` + `cn`

---

### 3.8 FilterToggleGroup

Сегментированный переключатель (Tabs в стиле iOS).

```tsx
import { Tabs } from "@rollout/ui-kit";
import { cn } from "@rollout/ui-kit";

interface FilterToggleGroupProps {
  options: { value: string; label: string }[];
  value: string;
  onChange: (value: string) => void;
}

export function FilterToggleGroup({ options, value, onChange }: FilterToggleGroupProps) {
  return (
    <div className="flex rounded-xl bg-[#1C1C1E] p-1">
      {options.map((opt) => (
        <button
          key={opt.value}
          onClick={() => onChange(opt.value)}
          className={cn(
            "flex-1 rounded-lg py-2.5 text-base font-medium transition-colors",
            value === opt.value
              ? "bg-[#3A3A3C] text-[#FAFAFA]"
              : "text-[#8E8E93] hover:text-[#E5E5E5]"
          )}
        >
          {opt.label}
        </button>
      ))}
    </div>
  );
}
```

> shadcn-референс: `Tabs`  
> Реализация: `@rollout/ui-kit` → `Tabs` или кастомный

---

### 3.9 SelectDropdown

Выпадающий список для фильтров (например, «Профессия», «Овощи»).

```tsx
import { Select } from "@rollout/ui-kit";
import { ChevronDown } from "lucide-react";

interface SelectDropdownProps {
  value: string;
  onChange: (value: string) => void;
  options: { value: string; label: string }[];
  placeholder?: string;
}

export function SelectDropdown({ value, onChange, options, placeholder }: SelectDropdownProps) {
  return (
    <Select value={value} onValueChange={onChange}>
      <Select.Trigger className="flex h-11 items-center gap-2 rounded-xl border border-[#3A3A3C] bg-[#1C1C1E] px-4 text-base text-[#FAFAFA]">
        <Select.Value placeholder={placeholder} />
        <ChevronDown className="size-5 text-[#8E8E93]" />
      </Select.Trigger>
      <Select.Content className="rounded-xl border border-[#3A3A3C] bg-[#1C1C1E]">
        {options.map((opt) => (
          <Select.Item
            key={opt.value}
            value={opt.value}
            className="text-base text-[#FAFAFA] focus:bg-[#2C2C2E] focus:text-[#FAFAFA]"
          >
            {opt.label}
          </Select.Item>
        ))}
      </Select.Content>
    </Select>
  );
}
```

> shadcn-референс: `Select`  
> Реализация: `@rollout/ui-kit` → `Select`

---

### 3.10 SliderRange

Двойной слайдер для диапазона цен.

```tsx
import { Slider } from "@rollout/ui-kit";

interface SliderRangeProps {
  min: number;
  max: number;
  value: [number, number];
  onChange: (value: [number, number]) => void;
}

export function SliderRange({ min, max, value, onChange }: SliderRangeProps) {
  return (
    <div className="px-2 py-4">
      <Slider
        value={value}
        min={min}
        max={max}
        step={1000}
        onValueChange={(v) => onChange(v as [number, number])}
        className="w-full"
      />
      <div className="mt-3 flex justify-between text-sm text-[#8E8E93]">
        <span>от {value[0].toLocaleString("ru-RU")} ₽</span>
        <span>до {value[1].toLocaleString("ru-RU")} ₽</span>
      </div>
    </div>
  );
}
```

> shadcn-референс: `Slider`  
> Реализация: `@rollout/ui-kit` → `Slider`. Стилизация track `#3A3A3C`, fill `#E5E5E5`, thumb `24px` круг.

---

### 3.11 ServiceCard

Карточка сервиса с иконкой, заголовком и описанием.

```tsx
interface ServiceCardProps {
  icon: React.ReactNode;
  title: string;
  description: string;
  onClick?: () => void;
}

export function ServiceCard({ icon, title, description, onClick }: ServiceCardProps) {
  return (
    <button
      onClick={onClick}
      className="flex items-start gap-4 rounded-2xl bg-[#1C1C1E] p-4 text-left transition-colors hover:bg-[#2C2C2E]"
    >
      <div className="flex size-12 shrink-0 items-center justify-center rounded-xl bg-[#2C2C2E]">
        {icon}
      </div>
      <div className="flex flex-col gap-1">
        <span className="text-base font-medium text-[#FAFAFA]">{title}</span>
        <span className="text-sm text-[#8E8E93]">{description}</span>
      </div>
    </button>
  );
}
```

---

### 3.12 TabBar

Нижняя навигация (для мобильной вёрстки).

```tsx
import { User, Heart, CreditCard, ShoppingCart } from "lucide-react";
import { cn } from "@rollout/ui-kit";

interface TabBarProps {
  activeTab?: string;
  onTabChange?: (tab: string) => void;
}

const tabs = [
  { id: "profile", icon: User, label: "Профиль" },
  { id: "favorites", icon: Heart, label: "Избранное" },
  { id: "orders", icon: CreditCard, label: "Заказы" },
  { id: "cart", icon: ShoppingCart, label: "Корзина" },
];

export function TabBar({ activeTab = "profile", onTabChange }: TabBarProps) {
  return (
    <nav className="fixed bottom-0 left-0 right-0 z-50 border-t border-[#3A3A3C] bg-[#0A0A0A]/95 backdrop-blur-md md:hidden">
      <div className="mx-auto flex max-w-[576px] justify-around py-2">
        {tabs.map((tab) => {
          const Icon = tab.icon;
          const isActive = activeTab === tab.id;
          return (
            <button
              key={tab.id}
              onClick={() => onTabChange?.(tab.id)}
              className="flex flex-col items-center gap-0.5 p-2"
            >
              <Icon
                className={cn(
                  "size-6 transition-colors",
                  isActive ? "text-[#FAFAFA]" : "text-[#8E8E93]"
                )}
              />
              <span
                className={cn(
                  "text-[10px] transition-colors",
                  isActive ? "text-[#FAFAFA]" : "text-[#8E8E93]"
                )}
              >
                {tab.label}
              </span>
            </button>
          );
        })}
      </div>
    </nav>
  );
}
```

---

### 3.13 CarouselDots

Индикаторы точек для горизонтальной карусели.

```tsx
import { cn } from "@rollout/ui-kit";

interface CarouselDotsProps {
  total: number;
  active: number;
}

export function CarouselDots({ total, active }: CarouselDotsProps) {
  return (
    <div className="flex justify-center gap-1.5 py-3">
      {Array.from({ length: total }).map((_, i) => (
        <div
          key={i}
          className={cn(
            "size-1.5 rounded-full transition-colors",
            i === active ? "bg-[#FAFAFA]" : "bg-[#3A3A3C]"
          )}
        />
      ))}
    </div>
  );
}
```

---

### 3.14 ColorFilterItem

Элемент фильтра по цвету (круг с цветом + label + radio/checkbox).

```tsx
import { Checkbox } from "@rollout/ui-kit";
import { cn } from "@rollout/ui-kit";

interface ColorFilterItemProps {
  color: string;
  label: string;
  checked: boolean;
  onChange: (checked: boolean) => void;
}

export function ColorFilterItem({ color, label, checked, onChange }: ColorFilterItemProps) {
  const isWhite = color.toLowerCase() === "#ffffff" || color.toLowerCase() === "white";
  
  return (
    <label className="flex cursor-pointer items-center gap-3 py-2">
      <Checkbox
        checked={checked}
        onCheckedChange={onChange}
        className="size-5 rounded border-[#3A3A3C]"
      />
      <div
        className={cn(
          "size-6 rounded-full",
          isWhite && "border border-[#3A3A3C]"
        )}
        style={{ backgroundColor: color }}
      />
      <span className="text-base text-[#FAFAFA]">{label}</span>
    </label>
  );
}
```

---

## 4. Структура каждого экрана

### 4.1 Snippets (Главная)

**Скриншот:** `Snippets_1941_12184.png`, `Snippets_2328_57312.png`, `Snippets_4330_27809.png`

| Позиция | Компонент | Описание |
|---------|-----------|----------|
| 1 | `AppLogo` | Логотип Rollout (верхний левый угол) |
| 2 | H1 «Рекомендации» | Заголовок страницы |
| 3 | `ProductCard` (carousel) | Горизонтальная карусель карточек товаров (peek: видна часть следующей) |
| 4 | `CarouselDots` | Точки-индикаторы под каруселью |
| 5 | `ProductCard` (grid) | Сетка карточек товаров (2 колонки) с ценой, скидкой, кнопками |

**Состояния:**
- `loading` — скелетоны карточек (серые плейсхолдеры с анимацией pulse)
- `empty` — «Нет рекомендаций» + иконка
- `error` — «Не удалось загрузить» + кнопка «Повторить»

**Интерактивность:**
- Свайп карусели (горизонтальный scroll-snap)
- `onAddToCart` — добавление в корзину
- `onFavorite` — добавление в избранное
- Клик по карточке — переход на детальную страницу товара

---

### 4.2 SupperApp (Супер-апп)

**Скриншот:** `SupperApp_4163_10137.png`, `SupperApp2_4163_12834.png`

| Позиция | Компонент | Описание |
|---------|-----------|----------|
| 1 | `AppLogo` | Логотип |
| 2 | `SearchInput` | Поле поиска с placeholder «Поиск» |
| 3 | `CategoryCard` (grid 3 cols) | Сетка категорий: Электроника, Одежда, Косметика, Цветы, Товары для дома, Доставка еды, Авто, Бронирование, Недвижимость, Услуги, Работа |
| 4 | `TabBar` | Нижняя навигация |

**Состояния:**
- `loading` — скелетоны категорий
- `empty` — «Нет доступных категорий»

**Интерактивность:**
- Клик по `CategoryCard` — навигация в соответствующую вертикаль
- Поиск — фильтрация категорий по вводу

---

### 4.3 Filter (Фильтры)

**Скриншот:** `Filter_1786_41981.png`, `Filter_6511_33258.png`, `Filter2_6511_33882.png`, `Filter2_7008_22222.png`, `Filter2_7008_22758.png`

| Позиция | Компонент | Описание |
|---------|-----------|----------|
| 1 | `AppLogo` + H1 «Фильтры» | Заголовок |
| 2 | Секция «Категории» | `FilterCheckbox` с иерархией (parent + indent children) |
| 3 | Секция «Тип недвижимости» | `FilterChip` (горизонтальная/вертикальная обёртка) |
| 4 | Секция «Тип жилья» | `FilterChip` |
| 5 | Секция «До метро» | `FilterToggleGroup` («Пешком» / «На транспорте») + `FilterChip` (5/10/15/20/25/30 мин.) |
| 6 | Секция «Цена» | `FilterToggleGroup` («Цена за всё» / «Цена за м²») + `SliderRange` + быстрые `FilterChip` |
| 7 | Секция «Цвет» | `SearchInput` («поиск по списку») + `ColorFilterItem` |
| 8 | Секция «Регион» | `SelectDropdown` |
| 9 | Секция «Рейтинг» | `SelectDropdown` |
| 10 | Кнопка «Применить» | Primary Button на фиксированной нижней панели |

**Состояния:**
- `loading` — скелетоны чекбоксов
- `empty` — «Нет доступных фильтров»

**Интерактивность:**
- Чекбокс — toggle значения
- Чип — toggle active
- Слайдер — drag для диапазона
- Select — открытие dropdown
- «Применить» — submit формы, переход назад с query-параметрами
- Кнопка «Сбросить» (при наличии выбранных фильтров)

---

### 4.4 Real Estate (Недвижимость)

**Скриншот:** `RealEstate_1423_4610.png`, `RealEstate_6676_7888.png`

| Позиция | Компонент | Описание |
|---------|-----------|----------|
| 1 | `AppLogo` + H1 «Недвижимость» | Заголовок |
| 2 | `SearchInput` | Поиск |
| 3 | `CategoryRowCard` (grid 2 cols) | Новостройки, Вторичка, Комнаты, Дома/дачи, Машиноместа, Коммерческая |
| 4 | Primary Button | «Разместить объявление» (светло-серый фон, тёмный текст) |
| 5 | H2 «Сервисы» | Заголовок секции |
| 6 | `ServiceCard` | Подобрать агента, Оценка, Ипотека, Страхование |

**Интерактивность:**
- Клик по категории — переход в фильтр/список
- «Разместить объявление» — открытие формы

---

### 4.5 Food (Доставка еды)

**Скриншот:** `Food_6430_31908.png`, `Food_6765_34099.png`

| Позиция | Компонент | Описание |
|---------|-----------|----------|
| 1 | `AppLogo` + H1 «Еда» | Заголовок |
| 2 | `SearchInput` | Поиск |
| 3 | Адрес + время | «2-я Брестская улица, 37с1», «сегодня, в 22:45-23:00» |
| 4 | Banner | Промо-баннер с изображением еды |
| 5 | `FilterChip` + `SelectDropdown` | Сортировка (стрелки), «Фильтр», «Овощи», «Молочные продукты»… |
| 6 | `CategoryCard` (grid 3 cols) | Готовая еда, Овощи и фрукты, Молочные продукты… |

---

### 4.6 Services (Услуги)

**Скриншот:** `Services_2063_46420.png`, `Services_6765_50782.png`

| Позиция | Компонент | Описание |
|---------|-----------|----------|
| 1 | `AppLogo` + H1 «Услуги» | Заголовок |
| 2 | `SearchInput` | Поиск |
| 3 | `SelectDropdown` (row) | «Регион», «Рейтинг» |
| 4 | `CategoryRowCard` (grid 2 cols) | Отдых, Ремонт, Забота о животных, Красота… |

---

### 4.7 Jobs (Работа)

**Скриншот:** `Jobs_2096_7984.png`, `Jobs_6768_100183.png`

| Позиция | Компонент | Описание |
|---------|-----------|----------|
| 1 | `AppLogo` + H1 «Работа» | Заголовок |
| 2 | `FilterToggleGroup` | «Ищу работу» / «Ищу сотрудника» |
| 3 | `SearchInput` | Поиск |
| 4 | Icon Button + `FilterChip` + `SelectDropdown` | Сортировка, «Фильтр», «Профессия» |
| 5 | Primary Button | «Создать резюме» |
| 6 | H2 «Популярные категории» | Заголовок |
| 7 | `CategoryCard` | Категории вакансий |

---

### 4.8 Electronics, Cloth, Cosmetics, Houses Goods, Flower, Restaurant, Travel, Auto

Эти вертикали следуют **одному шаблону**:
1. `AppLogo` + H1 (название вертикали)
2. `SearchInput`
3. `FilterChip` / `SelectDropdown` (горизонтальный scroll)
4. Banner (опционально, промо)
5. `CategoryCard` grid или `CategoryRowCard` grid

---

## 5. Пример полной страницы

### SupperAppPage.tsx

```tsx
import { useState } from "react";
import { Button, Input, Card, cn } from "@rollout/ui-kit";
import { Search } from "lucide-react";

// --- Types ---
interface Category {
  id: string;
  title: string;
  image: string;
  route: string;
}

// --- Mock Data ---
const categories: Category[] = [
  { id: "1", title: "Электроника", image: "/img/cat-electronics.png", route: "/snippets/electronics" },
  { id: "2", title: "Одежда", image: "/img/cat-cloth.png", route: "/snippets/cloth" },
  { id: "3", title: "Косметика", image: "/img/cat-cosmetics.png", route: "/snippets/cosmetics" },
  { id: "4", title: "Цветы", image: "/img/cat-flowers.png", route: "/snippets/flowers" },
  { id: "5", title: "Товары для дома", image: "/img/cat-house.png", route: "/snippets/houses-goods" },
  { id: "6", title: "Доставка еды", image: "/img/cat-food.png", route: "/snippets/food" },
  { id: "7", title: "Авто", image: "/img/cat-auto.png", route: "/snippets/auto" },
  { id: "8", title: "Бронирование", image: "/img/cat-travel.png", route: "/snippets/travel" },
  { id: "9", title: "Недвижимость", image: "/img/cat-realestate.png", route: "/snippets/real-estate" },
  { id: "10", title: "Услуги", image: "/img/cat-services.png", route: "/snippets/services" },
  { id: "11", title: "Работа", image: "/img/cat-jobs.png", route: "/snippets/jobs" },
  { id: "12", title: "Рестораны", image: "/img/cat-restaurant.png", route: "/snippets/restaurants" },
];

// --- Components ---
function AppLogo() {
  return (
    <svg width="32" height="32" viewBox="0 0 32 32" fill="none">
      <path
        d="M16 2L4 14l12 12 12-12L16 2z"
        fill="white"
        fillOpacity="0.9"
      />
      <circle cx="24" cy="24" r="6" fill="white" />
    </svg>
  );
}

function SearchInput({
  value,
  onChange,
  placeholder = "Поиск",
}: {
  value: string;
  onChange: (v: string) => void;
  placeholder?: string;
}) {
  return (
    <div className="relative">
      <Search className="absolute left-4 top-1/2 size-5 -translate-y-1/2 text-[#8E8E93]" />
      <Input
        type="text"
        value={value}
        onChange={(e) => onChange(e.target.value)}
        placeholder={placeholder}
        className="h-12 w-full rounded-xl border border-[#3A3A3C] bg-[#1C1C1E] pl-11 pr-4 text-base text-[#FAFAFA] placeholder:text-[#636366] focus:border-[#E5E5E5] focus:outline-none"
      />
    </div>
  );
}

function CategoryCard({
  category,
  onClick,
}: {
  category: Category;
  onClick: () => void;
}) {
  return (
    <button
      onClick={onClick}
      className="flex flex-col items-center gap-2 rounded-2xl bg-[#1C1C1E] p-3 transition-colors hover:bg-[#2C2C2E]"
    >
      <div className="aspect-square w-full overflow-hidden rounded-xl">
        <img
          src={category.image}
          alt={category.title}
          className="h-full w-full object-cover"
          loading="lazy"
        />
      </div>
      <span className="text-sm font-medium text-[#FAFAFA]">{category.title}</span>
    </button>
  );
}

// --- Page ---
export default function SupperAppPage() {
  const [search, setSearch] = useState("");

  const filtered = categories.filter((c) =>
    c.title.toLowerCase().includes(search.toLowerCase())
  );

  const handleCategoryClick = (route: string) => {
    // Mock navigation — заменить на react-router navigate(route)
    window.location.href = route;
  };

  return (
    <div className="mx-auto flex min-h-screen max-w-[576px] flex-col gap-7 bg-[#0A0A0A] pt-20 pb-8">
      {/* Logo */}
      <div className="px-4">
        <AppLogo />
      </div>

      {/* Search */}
      <div className="px-4">
        <SearchInput value={search} onChange={setSearch} />
      </div>

      {/* Categories Grid */}
      <div className="px-4">
        {filtered.length === 0 ? (
          <div className="flex flex-col items-center gap-4 py-12 text-[#8E8E93]">
            <Search className="size-12" />
            <p className="text-base">Ничего не найдено</p>
          </div>
        ) : (
          <div className="grid grid-cols-3 gap-3">
            {filtered.map((cat) => (
              <CategoryCard
                key={cat.id}
                category={cat}
                onClick={() => handleCategoryClick(cat.route)}
              />
            ))}
          </div>
        )}
      </div>

      {/* Footer */}
      <footer className="mt-auto px-4 pt-8 text-center">
        <p className="text-xs text-[#8E8E93]">© 2025 Rollout</p>
      </footer>
    </div>
  );
}
```

---

### JobsPage.tsx (пример с tabs и фильтрами)

```tsx
import { useState } from "react";
import { Button, Input, Select, cn } from "@rollout/ui-kit";
import { Search, SlidersHorizontal, ArrowUpDown, ChevronDown } from "lucide-react";

export default function JobsPage() {
  const [tab, setTab] = useState<"seeker" | "employer">("seeker");
  const [search, setSearch] = useState("");
  const [profession, setProfession] = useState("");

  const professions = [
    { value: "it", label: "IT / Разработка" },
    { value: "design", label: "Дизайн" },
    { value: "marketing", label: "Маркетинг" },
    { value: "sales", label: "Продажи" },
  ];

  return (
    <div className="mx-auto flex min-h-screen max-w-[576px] flex-col gap-7 bg-[#0A0A0A] pt-20 pb-8">
      {/* Logo + Title */}
      <div className="px-4">
        <h1 className="text-[32px] font-semibold leading-tight tracking-tight text-[#FAFAFA]">
          Работа
        </h1>
      </div>

      {/* Tabs */}
      <div className="px-4">
        <div className="flex rounded-xl bg-[#1C1C1E] p-1">
          {[
            { key: "seeker" as const, label: "Ищу работу" },
            { key: "employer" as const, label: "Ищу сотрудника" },
          ].map((t) => (
            <button
              key={t.key}
              onClick={() => setTab(t.key)}
              className={cn(
                "flex-1 rounded-lg py-2.5 text-base font-medium transition-colors",
                tab === t.key
                  ? "bg-[#3A3A3C] text-[#FAFAFA]"
                  : "text-[#8E8E93] hover:text-[#E5E5E5]"
              )}
            >
              {t.label}
            </button>
          ))}
        </div>
      </div>

      {/* Search */}
      <div className="px-4">
        <div className="relative">
          <Search className="absolute left-4 top-1/2 size-5 -translate-y-1/2 text-[#8E8E93]" />
          <Input
            type="text"
            value={search}
            onChange={(e) => setSearch(e.target.value)}
            placeholder="Поиск"
            className="h-12 w-full rounded-xl border border-[#3A3A3C] bg-[#1C1C1E] pl-11 pr-4 text-base text-[#FAFAFA] placeholder:text-[#636366]"
          />
        </div>
      </div>

      {/* Filter Bar */}
      <div className="flex gap-2 overflow-x-auto px-4 pb-1">
        <button className="flex size-11 shrink-0 items-center justify-center rounded-xl border border-[#3A3A3C] bg-[#1C1C1E] text-[#FAFAFA]">
          <ArrowUpDown className="size-5" />
        </button>
        <button className="flex shrink-0 items-center gap-2 rounded-xl border border-[#3A3A3C] bg-[#1C1C1E] px-4 py-2.5 text-base text-[#FAFAFA]">
          <SlidersHorizontal className="size-5" />
          Фильтр
        </button>
        <Select value={profession} onValueChange={setProfession}>
          <Select.Trigger className="flex h-11 shrink-0 items-center gap-2 rounded-xl border border-[#3A3A3C] bg-[#1C1C1E] px-4 text-base text-[#FAFAFA]">
            <Select.Value placeholder="Профессия" />
            <ChevronDown className="size-5 text-[#8E8E93]" />
          </Select.Trigger>
          <Select.Content className="rounded-xl border border-[#3A3A3C] bg-[#1C1C1E]">
            {professions.map((p) => (
              <Select.Item
                key={p.value}
                value={p.value}
                className="text-base text-[#FAFAFA] focus:bg-[#2C2C2E]"
              >
                {p.label}
              </Select.Item>
            ))}
          </Select.Content>
        </Select>
      </div>

      {/* CTA */}
      <div className="px-4">
        <Button className="h-12 w-full rounded-xl bg-[#E5E5E5] text-base font-medium text-[#0A0A0A] hover:bg-white">
          Создать резюме
        </Button>
      </div>

      {/* Popular Categories */}
      <div className="px-4">
        <h2 className="mb-3 text-xl font-semibold text-[#FAFAFA]">Популярные категории</h2>
        <div className="grid grid-cols-3 gap-3">
          {professions.map((p) => (
            <div
              key={p.value}
              className="flex flex-col items-center gap-2 rounded-2xl bg-[#1C1C1E] p-3"
            >
              <div className="aspect-square w-full rounded-xl bg-[#2C2C2E]" />
              <span className="text-sm text-[#FAFAFA]">{p.label}</span>
            </div>
          ))}
        </div>
      </div>

      {/* Footer */}
      <footer className="mt-auto px-4 pt-8 text-center">
        <p className="text-xs text-[#8E8E93]">© 2025 Rollout</p>
      </footer>
    </div>
  );
}
```

---

## 6. Чек-лист вёрстки

- [ ] **Все цвета через токены** (не хардкод) — используй CSS-переменные или константы из `@rollout/ui-kit`
- [ ] **Типографика совпадает с макетом** — Geist Variable, размеры, веса, line-height по таблице §2.2
- [ ] **Отступы и радиусы по дизайн-системе** — `px-4`, `gap-7`, `rounded-2xl`, `rounded-xl`
- [ ] **Иконки из `lucide-react`** — `size-5` / `size-6`, цвет `text-[#FAFAFA]` или `text-[#8E8E93]`
- [ ] **Контейнер** `max-w-[576px] mx-auto`
- [ ] **Отступ от шапки** `pt-20`
- [ ] **Мобильный отступ снизу** `pb-tabbar md:pb-0`
- [ ] **Все формы на локальном `useState`** — нет завязки на бэкенд
- [ ] **Mock-обработчики без `console.log`** — либо пустые функции, либо `// TODO: API integration`
- [ ] **Route добавлен в `App.tsx`** — все 15 роутов из §1.3
- [ ] **Скриншот соответствует макету** — визуальный diff: цвета, отступы, шрифты, иконки
- [ ] **Не использовать `radix-ui`** напрямую — только `@rollout/ui-kit`
- [ ] **Не хардкодить цвета** — использовать токены дизайн-системы
- [ ] **Импортировать всё из `@rollout/ui-kit`** — не deep imports
- [ ] **Footer** на каждой странице: `© 2025 Rollout`
- [ ] **Скелетоны** для loading-состояний (серые блоки с `animate-pulse`)

---

## Приложение: Список скачанных скриншотов

| # | Файл | Страница | Размер |
|---|------|----------|--------|
| 1 | `Snippets_1941_12184.png` | Snippets | 212 KB |
| 2 | `Snippets_2328_57312.png` | Snippets | 305 KB |
| 3 | `Snippets_4330_27809.png` | Snippets | 472 KB |
| 4 | `Filter_1786_41981.png` | Filter | 285 KB |
| 5 | `Filter_6511_33258.png` | Filter | 54 KB |
| 6 | `Filter_6511_34163.png` | Filter | 38 KB |
| 7 | `Filter_6511_34263.png` | Filter | 51 KB |
| 8 | `Filter_6511_33882.png` | Filter | 48 KB |
| 9 | `Filter_6511_45913.png` | Filter | 51 KB |
| 10 | `Filter2_6511_33882.png` | Filter | 48 KB |
| 11 | `Filter2_6511_45913.png` | Filter | 51 KB |
| 12 | `Filter_7008_22222.png` | Filter | 2.2 MB |
| 13 | `Filter_7008_22758.png` | Filter | 2.2 MB |
| 14 | `Filter2_7008_22222.png` | Filter | 2.2 MB |
| 15 | `Filter2_7008_22758.png` | Filter | 2.2 MB |
| 16 | `SupperApp_4163_10137.png` | SupperApp | 442 KB |
| 17 | `SupperApp_6774_45511.png` | SupperApp | 414 KB |
| 18 | `SupperApp_6769_123887.png` | SupperApp | 469 KB |
| 19 | `SupperApp_6774_45512.png` | SupperApp | 438 KB |
| 20 | `SupperApp2_4163_12834.png` | SupperApp | 204 KB |
| 21 | `SupperApp2_6770_134855.png` | SupperApp | 232 KB |
| 22 | `SupperApp2_4163_10409.png` | SupperApp | 2.6 MB |
| 23 | `SupperApp2_6774_45935.png` | SupperApp | 2.7 MB |
| 24 | `SupperApp_6769_119014.png` | SupperApp | 7.0 MB |
| 25 | `SupperApp_6774_48417.png` | SupperApp | 6.9 MB |
| 26 | `Electronics_6678_14303.png` | Electronics | 1.2 MB |
| 27 | `Electronics_6678_14689.png` | Electronics | 554 KB |
| 28 | `Cloth_6048_26932.png` | Cloth | 2.2 MB |
| 29 | `Cloth_6707_10488.png` | Cloth | 890 KB |
| 30 | `Cosmetics_6088_53831.png` | Cosmetics | 1.3 MB |
| 31 | `Cosmetics_6708_25738.png` | Cosmetics | 799 KB |
| 32 | `HousesGoods_5675_12387.png` | Houses Goods | 1.2 MB |
| 33 | `HousesGoods_6765_31448.png` | Houses Goods | 635 KB |
| 34 | `Flower_6415_17488.png` | Flower | 694 KB |
| 35 | `Flower_6765_32364.png` | Flower | 1.4 MB |
| 36 | `Food_6430_31908.png` | Food | 777 KB |
| 37 | `Food_6765_34099.png` | Food | 1.8 MB |
| 38 | `Restaurant_4333_38060.png` | Restaurant | 1.2 MB |
| 39 | `Restaurant_6770_134467.png` | Restaurant | 3.0 MB |
| 40 | `RealEstate_1423_4610.png` | RealEstate | 607 KB |
| 41 | `RealEstate_6676_7888.png` | RealEstate | 1.4 MB |
| 42 | `RealEstate_1961_24237.png` | RealEstate | 577 KB |
| 43 | `RealEstate_1965_15667.png` | RealEstate | 584 KB |
| 44 | `RealEstate_2042_16057.png` | RealEstate | 894 KB |
| 45 | `RealEstate_6680_5568.png` | RealEstate | 1.6 MB |
| 46 | `Travel_6685_17974.png` | Travel | 2.2 MB |
| 47 | `Travel_6686_20948.png` | Travel | 805 KB |
| 48 | `Auto_6765_41361.png` | Auto | 1.5 MB |
| 49 | `Auto_6765_42356.png` | Auto | 621 KB |
| 50 | `Services_2063_46420.png` | Services | 1.1 MB |
| 51 | `Services_6765_50782.png` | Services | 2.6 MB |
| 52 | `Jobs_2096_7984.png` | Jobs | 581 KB |
| 53 | `Jobs_6768_100183.png` | Jobs | 1.4 MB |

**Итого: 53 скриншота** загружены в `/Users/mokoloskov/Documents/kimi/workspace/4-Snippets_screenshots/`.
