# Инструкция по вёрстке модуля 3-Profile (React + @rollout/ui-kit)

> Статус: готово к вёрстке  
> Дата анализа: 2025-07-13  
> Скриншотов скачано: 27  
> Стек: Node 22, pnpm 10.32.1, Vite 8, Tailwind v4, @base-ui/react, @rollout/ui-kit, react-router-dom 6, lucide-react, Geist Variable

---

## 1. Обзор модуля

### 1.1 Назначение
Модуль **Profile** — личный кабинет пользователя. Объединяет управление аккаунтом, финансами, заказами, избранным, личными данными, уведомлениями, безопасностью и отзывами. Является центральной точкой входа для всех пользовательских настроек и истории активности.

### 1.2 Экраны модуля (страницы Figma)

| # | Страница Figma | Экран(ы) | Статус |
|---|----------------|----------|--------|
| 1 | Cover | Обложка модуля | — |
| 2 | Profile ☑️ | Главный профиль (`/profile`) | ✅ |
| 3 | PersonalData ☑️ | Личные данные (`/profile/personal-data`) | ✅ |
|  |  | Данные для таможни (`/profile/customs-data`) | ✅ |
| 4 | Notifications ☑️ | Настройка уведомлений (`/profile/notifications/settings`) | ✅ |
|  |  | Список уведомлений (`/profile/notifications`) | ✅ |
| 5 | Security ☑️ | Безопасность (`/profile/security`) | ✅ |
| 6 | Orders ☑️ | Заказы — Актуальные (`/profile/orders`) | ✅ |
|  |  | Заказы — Завершённые (`/profile/orders?tab=completed`) | ✅ |
| 7 | Favourites ☑️ | Избранное — Все (`/profile/favourites`) | ✅ |
|  |  | Избранное — Подборки (`/profile/favourites?tab=collections`) | ✅ |
| 8 | Feedback ☑️ | Отзывы и вопросы (`/profile/feedback`) | ✅ |
| 9 | PromoСode ☑️ | Промокоды (`/profile/promo-codes`) | ✅ |
| 10 | Certificate ☑️ | Сертификаты (`/profile/certificates`) | ✅ |
| 11 | Property 🛠️ | Автомобили (`/profile/cars`) | 🛠️ |
|  |  | Недвижимость, Прочее имущество | 🛠️ |

### 1.3 Роуты (URL-пути)

```tsx
// App.tsx — раздел profile
<Route path="/profile" element={<ProfilePage />} />
<Route path="/profile/personal-data" element={<PersonalDataPage />} />
<Route path="/profile/customs-data" element={<CustomsDataPage />} />
<Route path="/profile/notifications" element={<NotificationsListPage />} />
<Route path="/profile/notifications/settings" element={<NotificationsSettingsPage />} />
<Route path="/profile/security" element={<SecurityPage />} />
<Route path="/profile/orders" element={<OrdersPage />} />
<Route path="/profile/favourites" element={<FavouritesPage />} />
<Route path="/profile/feedback" element={<FeedbackPage />} />
<Route path="/profile/promo-codes" element={<PromoCodesPage />} />
<Route path="/profile/certificates" element={<CertificatesPage />} />
<Route path="/profile/cars" element={<CarsPage />} />
```

---

## 2. Дизайн-система (из макетов)

### 2.1 Цвета

| Токен | Значение (hex) | Применение |
|-------|----------------|------------|
| `--bg-page` | `#0A0A0A` | Фон всей страницы |
| `--bg-card` | `#1C1C1E` | Карточки, input-поля, ячейки списка |
| `--bg-elevated` | `#2C2C2E` | Поднятые элементы, hover-фон |
| `--border-default` | `#3A3A3C` | Границы input, карточек, разделителей |
| `--border-subtle` | `#48484A` | Тонкие разделители внутри списков |
| `--text-primary` | `#FAFAFA` | Основной текст, заголовки |
| `--text-secondary` | `#8E8E93` | Подписи, placeholder, даты, описания |
| `--text-tertiary` | `#636366` | Placeholder в input, disabled |
| `--accent-primary` | `#E5E5E5` | Primary button bg, активный tab, toggle ON |
| `--accent-primary-text` | `#0A0A0A` | Текст на primary button |
| `--accent-danger` | `#FF453A` | Ошибки, удалить, опасные действия |
| `--accent-success` | `#30D158` | Успешные статусы, текущий сеанс |
| `--badge-bg` | `#3A3A3C` | Фон badge/чипа |
| `--badge-text` | `#FAFAFA` | Текст badge |
| `--overlay` | `rgba(0,0,0,0.7)` | Затемнение под модальными окнами |
| `--toggle-off` | `#3A3A3C` | Toggle в выключенном состоянии |
| `--toggle-on` | `#E5E5E5` | Toggle во включённом состоянии |
| `--toggle-knob` | `#FAFAFA` | Кружок toggle |

### 2.2 Типографика (Geist Variable)

| Элемент | Размер | Вес | Line-height | Letter-spacing |
|---------|--------|-----|-------------|----------------|
| Page title (H1) | 28px | 600 | 1.2 | -0.02em |
| Section title (H2) | 20px | 600 | 1.3 | -0.01em |
| Card title | 17px | 500 | 1.3 | 0 |
| Body | 16px | 400 | 1.5 | 0 |
| Body medium | 16px | 500 | 1.5 | 0 |
| Caption | 13px | 400 | 1.4 | 0.01em |
| Caption medium | 13px | 500 | 1.4 | 0.01em |
| Button text | 16px | 500 | 1.0 | 0 |
| Section header label | 13px | 500 | 1.3 | 0.02em |
| Price | 17px | 600 | 1.2 | -0.01em |
| Price old | 15px | 400 | 1.2 | 0 |
| Discount | 13px | 500 | 1.2 | 0 |

### 2.3 Спейсинг, padding, gap, радиусы

| Параметр | Значение |
|----------|----------|
| Page container | `max-w-[576px] mx-auto` |
| Page top padding | `pt-20` (отступ от шапки) |
| Page bottom padding | `pb-8` (desktop), `pb-tabbar` (mobile) |
| Page horizontal padding | `px-4` (16px) |
| Section gap | `gap-7` (28px) между секциями |
| Inner section gap | `gap-3` (12px) |
| Card padding | `p-4` (16px) или `p-5` (20px) |
| Card border-radius | `rounded-2xl` (16px) |
| Input border-radius | `rounded-xl` (12px) |
| Button border-radius | `rounded-xl` (12px) или `rounded-full` (для pill) |
| Small chip border-radius | `rounded-lg` (8px) |
| Avatar border-radius | `rounded-full` (круг) |
| List item gap | `gap-3` (12px) между иконкой и текстом |
| List item padding | `py-3.5 px-4` |
| Tabs border-radius | `rounded-full` (pill) |
| Toggle border-radius | `rounded-full` |
| Toggle width | 51px, height 31px |
| Toggle knob | 27px circle |

### 2.4 Иконки

- **Библиотека:** `lucide-react`
- **Размеры:** `size-5` (20px) для inline-иконок, `size-6` (24px) для навигации/крупных
- **Цвет:** по умолчанию `text-primary` (#FAFAFA), для secondary — `text-secondary` (#8E8E93)
- **Stroke width:** 1.5 (lucide default)

**Ключевые иконки по экранам:**

| Экран | Иконки (lucide-react) |
|-------|----------------------|
| Profile | `Bell`, `User`, `CreditCard`, `Ticket`, `Gift`, `Package`, `RotateCcw`, `Heart`, `BarChart3`, `MessageCircleQuestion`, `HelpCircle`, `MessageSquare`, `Shield`, `Trash2`, `LogOut` |
| PersonalData | `ArrowLeft`, `Search` (в input адреса) |
| Orders | `ChevronRight`, `ChevronDown` |
| Favourites | `Search`, `ArrowUpDown`, `SlidersHorizontal`, `ChevronDown`, `Heart`, `Phone`, `ShoppingCart` |
| Feedback | `Star`, `ThumbsUp`, `ThumbsDown`, `MessageCircle`, `MoreHorizontal`, `ChevronRight` |
| Security | `X` (удаление устройства), `ChevronRight` |
| PromoCodes | `Gift`, `ChevronRight` |
| Notifications | `ChevronDown`, `ChevronUp` (accordion) |

---

## 3. Компоненты UI (с примерами кода)

### 3.1 PageHeader

**Описание:** Шапка страницы с заголовком и опциональными кнопками.

**Props:**
```tsx
interface PageHeaderProps {
  title: string;
  backHref?: string;
  rightAction?: React.ReactNode;
  className?: string;
}
```

**Код:**
```tsx
import { Button } from "@rollout/ui-kit";
import { ArrowLeft } from "lucide-react";
import { useNavigate } from "react-router-dom";

export function PageHeader({ title, backHref, rightAction, className }: PageHeaderProps) {
  const navigate = useNavigate();
  return (
    <div className={cn("flex items-center gap-3 py-4", className)}>
      {backHref && (
        <Button
          variant="ghost"
          size="icon"
          className="shrink-0 text-primary"
          onClick={() => navigate(backHref)}
        >
          <ArrowLeft className="size-6" />
        </Button>
      )}
      <Typography variant="h1" className="flex-1">
        {title}
      </Typography>
      {rightAction && <div className="shrink-0">{rightAction}</div>}
    </div>
  );
}
```

**Shadcn-референс:** нет прямого аналога; комбинация `Button` + `Typography`.

---

### 3.2 SectionLabel

**Описание:** Мелкая серая подпись над группой элементов (например, «Аккаунт», «Финансы»).

**Props:**
```tsx
interface SectionLabelProps {
  children: React.ReactNode;
  className?: string;
}
```

**Код:**
```tsx
import { Typography } from "@rollout/ui-kit";

export function SectionLabel({ children, className }: SectionLabelProps) {
  return (
    <Typography
      variant="caption"
      className={cn("text-secondary font-medium tracking-wide", className)}
    >
      {children}
    </Typography>
  );
}
```

**Shadcn-референс:** `Label` (адаптированный).

---

### 3.3 ProfileCard

**Описание:** Карточка профиля с аватаром, именем и email. Кликабельная, ведёт на редактирование личных данных.

**Props:**
```tsx
interface ProfileCardProps {
  name: string;
  email: string;
  avatarUrl?: string;
  onClick?: () => void;
}
```

**Код:**
```tsx
import { Card, Avatar, Typography } from "@rollout/ui-kit";
import { ChevronRight } from "lucide-react";

export function ProfileCard({ name, email, avatarUrl, onClick }: ProfileCardProps) {
  return (
    <Card
      className="flex items-center gap-3 p-4 cursor-pointer bg-card border-border-default"
      onClick={onClick}
    >
      <Avatar className="size-12" src={avatarUrl} fallback={name.slice(0, 2)} />
      <div className="flex-1 min-w-0">
        <Typography variant="body" className="font-medium text-primary">
          {name}
        </Typography>
        <Typography variant="caption" className="text-secondary">
          {email}
        </Typography>
      </div>
      <ChevronRight className="size-5 text-secondary shrink-0" />
    </Card>
  );
}
```

**Shadcn-референс:** `Card` + `Avatar`.

---

### 3.4 BalanceCard

**Описание:** Карточка баланса кошелька с суммой и кнопкой «Пополнить».

**Props:**
```tsx
interface BalanceCardProps {
  balance: string;
  currency: string;
  onTopUp?: () => void;
}
```

**Код:**
```tsx
import { Card, Button, Typography } from "@rollout/ui-kit";

export function BalanceCard({ balance, currency, onTopUp }: BalanceCardProps) {
  return (
    <Card className="p-5 bg-card border-border-default rounded-2xl">
      <Typography variant="h2" className="text-primary">
        {balance} {currency}
      </Typography>
      <Typography variant="caption" className="text-secondary mt-1">
        На кошельке
      </Typography>
      <Button
        className="w-full mt-4 bg-accent-primary text-accent-primary-text rounded-xl h-12 font-medium"
        onClick={onTopUp}
      >
        Пополнить
      </Button>
    </Card>
  );
}
```

**Shadcn-референс:** `Card` + `Button`.

---

### 3.5 MenuListItem

**Описание:** Элемент списка меню с иконкой слева, текстом и стрелкой справа. Основной навигационный элемент профиля.

**Props:**
```tsx
interface MenuListItemProps {
  icon: React.ReactNode;
  label: string;
  onClick?: () => void;
  href?: string;
  destructive?: boolean;
}
```

**Код:**
```tsx
import { ChevronRight } from "lucide-react";

export function MenuListItem({ icon, label, onClick, href, destructive }: MenuListItemProps) {
  const content = (
    <div className="flex items-center gap-3 py-3.5 px-4 cursor-pointer">
      <div className={cn("shrink-0", destructive ? "text-danger" : "text-primary")}>
        {icon}
      </div>
      <span className={cn("flex-1 text-base font-medium", destructive ? "text-danger" : "text-primary")}>
        {label}
      </span>
      <ChevronRight className="size-5 text-secondary shrink-0" />
    </div>
  );

  if (href) {
    return (
      <a href={href} className="block rounded-xl hover:bg-elevated transition-colors">
        {content}
      </a>
    );
  }
  return (
    <div className="rounded-xl hover:bg-elevated transition-colors" onClick={onClick}>
      {content}
    </div>
  );
}
```

**Shadcn-референс:** нет; кастомный на `div` + `Typography`.

---

### 3.6 SegmentedTabs

**Описание:** Сегментированные табы (pill-стиль). Активный таб — светлый фон, неактивный — прозрачный.

**Props:**
```tsx
interface SegmentedTabsProps {
  tabs: { id: string; label: string; badge?: number }[];
  activeTab: string;
  onChange: (id: string) => void;
}
```

**Код:**
```tsx
import { Tabs, Badge } from "@rollout/ui-kit";
import { cn } from "@rollout/ui-kit";

export function SegmentedTabs({ tabs, activeTab, onChange }: SegmentedTabsProps) {
  return (
    <div className="flex gap-1 p-1 bg-card rounded-full border border-border-default">
      {tabs.map((tab) => (
        <button
          key={tab.id}
          onClick={() => onChange(tab.id)}
          className={cn(
            "flex-1 relative flex items-center justify-center gap-1.5 py-2.5 px-4 text-sm font-medium rounded-full transition-colors",
            activeTab === tab.id
              ? "bg-elevated text-primary"
              : "text-secondary hover:text-primary"
          )}
        >
          {tab.label}
          {tab.badge !== undefined && tab.badge > 0 && (
            <Badge className="bg-badge-bg text-badge-text text-xs px-1.5 py-0">
              {tab.badge}
            </Badge>
          )}
        </button>
      ))}
    </div>
  );
}
```

**Shadcn-референс:** `Tabs` (адаптированный под pill-стиль).

---

### 3.7 CategoryFilterChips

**Описание:** Горизонтальный скролл с чипами фильтрации (категории заказов, фильтры избранного).

**Props:**
```tsx
interface CategoryFilterChipsProps {
  categories: { id: string; label: string }[];
  activeId: string;
  onChange: (id: string) => void;
}
```

**Код:**
```tsx
export function CategoryFilterChips({ categories, activeId, onChange }: CategoryFilterChipsProps) {
  return (
    <div className="flex gap-2 overflow-x-auto no-scrollbar">
      {categories.map((cat) => (
        <button
          key={cat.id}
          onClick={() => onChange(cat.id)}
          className={cn(
            "shrink-0 py-2 px-4 rounded-full text-sm font-medium border transition-colors",
            activeId === cat.id
              ? "bg-elevated text-primary border-border-default"
              : "bg-transparent text-secondary border-border-default hover:text-primary"
          )}
        >
          {cat.label}
        </button>
      ))}
    </div>
  );
}
```

**Shadcn-референс:** `Badge` (адаптированный).

---

### 3.8 FormField

**Описание:** Лейбл + input в стиле дизайн-системы. Используется на всех формах (личные данные, данные для таможни, промокоды, сертификаты).

**Props:**
```tsx
interface FormFieldProps {
  label: string;
  placeholder?: string;
  value: string;
  onChange: (value: string) => void;
  type?: "text" | "email" | "date" | "search";
  icon?: React.ReactNode;
  hint?: string;
}
```

**Код:**
```tsx
import { Input, Field, Typography } from "@rollout/ui-kit";

export function FormField({ label, placeholder, value, onChange, type = "text", icon, hint }: FormFieldProps) {
  return (
    <Field>
      <Typography variant="body" className="font-medium text-primary mb-2">
        {label}
      </Typography>
      <div className="relative">
        {icon && (
          <div className="absolute left-4 top-1/2 -translate-y-1/2 text-tertiary">
            {icon}
          </div>
        )}
        <Input
          type={type}
          value={value}
          onChange={(e) => onChange(e.target.value)}
          placeholder={placeholder}
          className={cn(
            "w-full h-12 bg-card border border-border-default rounded-xl text-primary placeholder:text-tertiary focus:border-border-subtle focus:ring-0",
            icon ? "pl-11" : "px-4"
          )}
        />
      </div>
      {hint && (
        <Typography variant="caption" className="text-secondary mt-1.5">
          {hint}
        </Typography>
      )}
    </Field>
  );
}
```

**Shadcn-референс:** `Input` + `Label` (через `Field`).

---

### 3.9 SelectField

**Описание:** Лейбл + select/dropdown для выбора из списка (пол, страна, язык, валюта, часовой пояс).

**Props:**
```tsx
interface SelectFieldProps {
  label: string;
  value: string;
  options: { value: string; label: string }[];
  onChange: (value: string) => void;
}
```

**Код:**
```tsx
import { Select, Field, Typography } from "@rollout/ui-kit";
import { ChevronDown } from "lucide-react";

export function SelectField({ label, value, options, onChange }: SelectFieldProps) {
  return (
    <Field>
      <Typography variant="body" className="font-medium text-primary mb-2">
        {label}
      </Typography>
      <Select
        value={value}
        onValueChange={onChange}
        className="w-full h-12 bg-card border border-border-default rounded-xl text-primary"
      >
        <Select.Trigger className="flex items-center justify-between px-4 h-12 bg-card border border-border-default rounded-xl text-primary focus:ring-0 focus:border-border-subtle">
          <Select.Value />
          <ChevronDown className="size-5 text-secondary" />
        </Select.Trigger>
        <Select.Content className="bg-card border border-border-default rounded-xl">
          {options.map((opt) => (
            <Select.Item
              key={opt.value}
              value={opt.value}
              className="px-4 py-2.5 text-primary hover:bg-elevated cursor-pointer rounded-lg"
            >
              {opt.label}
            </Select.Item>
          ))}
        </Select.Content>
      </Select>
    </Field>
  );
}
```

**Shadcn-референс:** `Select`.

---

### 3.10 ToggleSwitch

**Описание:** iOS-стильный переключатель для настроек уведомлений.

**Props:**
```tsx
interface ToggleSwitchProps {
  checked: boolean;
  onChange: (checked: boolean) => void;
  label?: string;
}
```

**Код:**
```tsx
export function ToggleSwitch({ checked, onChange, label }: ToggleSwitchProps) {
  return (
    <label className="flex items-center justify-between py-3 cursor-pointer">
      {label && <span className="text-base font-medium text-primary">{label}</span>}
      <button
        role="switch"
        aria-checked={checked}
        onClick={() => onChange(!checked)}
        className={cn(
          "relative inline-flex h-[31px] w-[51px] shrink-0 rounded-full transition-colors focus:outline-none",
          checked ? "bg-accent-primary" : "bg-toggle-off"
        )}
      >
        <span
          className={cn(
            "inline-block size-[27px] rounded-full bg-toggle-knob shadow transition-transform",
            checked ? "translate-x-[22px]" : "translate-x-0.5"
          )}
        />
      </button>
    </label>
  );
}
```

**Shadcn-референс:** `Switch` (но в @rollout/ui-kit может отсутствовать, поэтому кастомный).

---

### 3.11 OrderCard

**Описание:** Карточка заказа с датой доставки, адресом и миниатюрами товаров. Различается для активных и завершённых заказов.

**Props:**
```tsx
interface OrderCardProps {
  status: "active" | "completed";
  deliveryDate: string;
  address: string;
  items: { image: string; name: string }[];
  onClick?: () => void;
  onRate?: () => void;
  onReturn?: () => void;
}
```

**Код:**
```tsx
import { Card, Typography } from "@rollout/ui-kit";
import { ChevronRight } from "lucide-react";

export function OrderCard({ status, deliveryDate, address, items, onClick, onRate, onReturn }: OrderCardProps) {
  return (
    <Card className="p-4 bg-card border-border-default rounded-2xl">
      <div className="flex items-start justify-between cursor-pointer" onClick={onClick}>
        <div>
          <Typography variant="body" className="font-medium text-primary">
            {status === "active" ? "Доставим" : "Доставлен"} — {deliveryDate}
          </Typography>
          <Typography variant="caption" className="text-secondary mt-1">
            {address}
          </Typography>
        </div>
        <ChevronRight className="size-5 text-secondary shrink-0 mt-0.5" />
      </div>
      <div className="flex gap-2 mt-3">
        {items.map((item, i) => (
          <img
            key={i}
            src={item.image}
            alt={item.name}
            className="size-14 rounded-lg object-cover bg-elevated"
          />
        ))}
      </div>
      {status === "completed" && (onRate || onReturn) && (
        <div className="flex gap-2 mt-3">
          {onRate && (
            <Button
              className="flex-1 h-11 bg-accent-primary text-accent-primary-text rounded-xl font-medium"
              onClick={onRate}
            >
              Оценить
            </Button>
          )}
          {onReturn && (
            <Button
              variant="outline"
              className="flex-1 h-11 bg-transparent border-border-default text-primary rounded-xl font-medium"
              onClick={onReturn}
            >
              Вернуть
            </Button>
          )}
        </div>
      )}
    </Card>
  );
}
```

**Shadcn-референс:** `Card` + `Button`.

---

### 3.12 ProductCard

**Описание:** Карточка товара в избранном (2-колоночная сетка). Содержит изображение, название, цену, старую цену, скидку, теги, продавца и кнопки действий.

**Props:**
```tsx
interface ProductCardProps {
  image: string;
  title: string;
  price: string;
  oldPrice?: string;
  discount?: string;
  tags: string[];
  seller: string;
  sellerRating: string;
  sellerReviews: number;
  actionButton: "cart" | "call";
  onAction?: () => void;
  onToggleFavorite?: () => void;
  isFavorite?: boolean;
}
```

**Код:**
```tsx
import { Card, Button, Badge, Typography } from "@rollout/ui-kit";
import { Heart, Phone, ShoppingCart, Star } from "lucide-react";

export function ProductCard({
  image, title, price, oldPrice, discount, tags, seller, sellerRating, actionButton, onAction, onToggleFavorite, isFavorite
}: ProductCardProps) {
  return (
    <Card className="bg-transparent border-0 p-0">
      <div className="relative aspect-square rounded-2xl overflow-hidden bg-elevated">
        <img src={image} alt={title} className="w-full h-full object-cover" />
        <div className="absolute bottom-2 left-1/2 -translate-x-1/2 flex gap-1">
          <div className="size-1.5 rounded-full bg-primary/60" />
          <div className="size-1.5 rounded-full bg-primary/30" />
          <div className="size-1.5 rounded-full bg-primary/30" />
        </div>
      </div>
      <div className="mt-2">
        <Typography variant="body" className="text-primary font-medium line-clamp-2 text-sm leading-tight">
          {title}
        </Typography>
        <div className="flex items-center gap-2 mt-1">
          <Typography variant="price" className="text-primary font-semibold">
            {price}
          </Typography>
          {oldPrice && (
            <Typography variant="caption" className="text-secondary line-through">
              {oldPrice}
            </Typography>
          )}
          {discount && (
            <Typography variant="caption" className="text-danger font-medium">
              {discount}
            </Typography>
          )}
        </div>
        <div className="flex flex-wrap gap-1 mt-1.5">
          {tags.map((tag, i) => (
            <Badge key={i} className="bg-badge-bg text-badge-text text-xs px-1.5 py-0.5 rounded">
              {tag}
            </Badge>
          ))}
        </div>
        <div className="flex items-center gap-1 mt-1.5">
          <Typography variant="caption" className="text-primary font-medium">
            {seller}
          </Typography>
          <Star className="size-3.5 fill-accent-primary text-accent-primary" />
          <Typography variant="caption" className="text-secondary">
            {sellerRating}
          </Typography>
        </div>
        <div className="flex gap-2 mt-2">
          <Button
            className="flex-1 h-9 bg-accent-primary text-accent-primary-text rounded-lg text-sm font-medium"
            onClick={onAction}
          >
            {actionButton === "cart" ? (
              <>
                <ShoppingCart className="size-4 mr-1" /> В корзину
              </>
            ) : (
              <>
                <Phone className="size-4 mr-1" /> Позвонить
              </>
            )}
          </Button>
          <Button
            size="icon"
            variant="outline"
            className="h-9 w-9 rounded-lg border-border-default bg-transparent"
            onClick={onToggleFavorite}
          >
            <Heart className={cn("size-4", isFavorite ? "fill-danger text-danger" : "text-primary")} />
          </Button>
        </div>
      </div>
    </Card>
  );
}
```

**Shadcn-референс:** `Card` + `Badge` + `Button`.

---

### 3.13 NotificationItem

**Описание:** Элемент списка уведомлений с иконкой, заголовком, описанием и временем.

**Props:**
```tsx
interface NotificationItemProps {
  icon: React.ReactNode;
  title: string;
  description: string;
  time: string;
  onClick?: () => void;
}
```

**Код:**
```tsx
import { Card, Typography } from "@rollout/ui-kit";

export function NotificationItem({ icon, title, description, time, onClick }: NotificationItemProps) {
  return (
    <Card
      className="flex items-start gap-3 p-4 bg-card border-border-default rounded-2xl cursor-pointer"
      onClick={onClick}
    >
      <div className="flex items-center justify-center size-10 rounded-xl bg-elevated shrink-0">
        {icon}
      </div>
      <div className="flex-1 min-w-0">
        <div className="flex items-start justify-between gap-2">
          <Typography variant="body" className="font-medium text-primary">
            {title}
          </Typography>
          <Typography variant="caption" className="text-secondary shrink-0">
            {time}
          </Typography>
        </div>
        <Typography variant="caption" className="text-secondary mt-1 line-clamp-2">
          {description}
        </Typography>
      </div>
    </Card>
  );
}
```

**Shadcn-референс:** `Card`.

---

### 3.14 PromoCodeItem

**Описание:** Элемент списка промокодов/сертификатов с иконкой подарка, названием, описанием и кнопкой «Применить».

**Props:**
```tsx
interface PromoCodeItemProps {
  title: string;
  description: string;
  onApply?: () => void;
}
```

**Код:**
```tsx
import { Button, Typography } from "@rollout/ui-kit";
import { Gift } from "lucide-react";

export function PromoCodeItem({ title, description, onApply }: PromoCodeItemProps) {
  return (
    <div className="flex items-center gap-3 py-3">
      <div className="flex items-center justify-center size-12 rounded-xl bg-elevated shrink-0">
        <Gift className="size-6 text-primary" />
      </div>
      <div className="flex-1 min-w-0">
        <Typography variant="body" className="font-medium text-primary">
          {title}
        </Typography>
        <Typography variant="caption" className="text-secondary mt-0.5">
          {description}
        </Typography>
      </div>
      <Button
        variant="outline"
        className="h-9 px-4 rounded-lg bg-transparent border-border-default text-primary text-sm font-medium shrink-0"
        onClick={onApply}
      >
        Применить
      </Button>
    </div>
  );
}
```

**Shadcn-референс:** `Button` + `Card`.

---

### 3.15 ReviewCard

**Описание:** Карточка отзыва с рейтингом (звёзды), названием товара, датой, достоинствами, недостатками и кнопками реакций.

**Props:**
```tsx
interface ReviewCardProps {
  productImage: string;
  productName: string;
  rating: number;
  publishedAt: string;
  pros: string;
  cons: string;
  likes: number;
  dislikes: number;
  comments: number;
  onEdit?: () => void;
  onDelete?: () => void;
  onShare?: () => void;
  onReport?: () => void;
}
```

**Код:**
```tsx
import { Card, Typography, Button, Separator } from "@rollout/ui-kit";
import { Star, ThumbsUp, ThumbsDown, MessageCircle, MoreHorizontal } from "lucide-react";

export function ReviewCard({ productImage, productName, rating, publishedAt, pros, cons, likes, dislikes, comments }: ReviewCardProps) {
  return (
    <Card className="p-4 bg-card border-border-default rounded-2xl">
      <div className="flex gap-1 mb-2">
        {Array.from({ length: 5 }).map((_, i) => (
          <Star
            key={i}
            className={cn(
              "size-4",
              i < rating ? "fill-accent-primary text-accent-primary" : "text-secondary"
            )}
          />
        ))}
      </div>
      <div className="flex items-center gap-3">
        <img src={productImage} alt={productName} className="size-12 rounded-lg object-cover bg-elevated" />
        <Typography variant="body" className="font-medium text-primary line-clamp-2">
          {productName}
        </Typography>
      </div>
      <Typography variant="caption" className="text-secondary mt-2">
        Опубликован {publishedAt}
      </Typography>
      <div className="mt-3">
        <Typography variant="body" className="font-semibold text-primary">
          Достоинства
        </Typography>
        <Typography variant="body" className="text-primary mt-1">
          {pros}
        </Typography>
      </div>
      <div className="mt-3">
        <Typography variant="body" className="font-semibold text-primary">
          Недостатки
        </Typography>
        <Typography variant="body" className="text-primary mt-1">
          {cons}
        </Typography>
      </div>
      <Separator className="my-3 bg-border-default" />
      <div className="flex items-center gap-2">
        <Button variant="ghost" size="sm" className="h-8 px-3 rounded-lg text-primary">
          <ThumbsUp className="size-4 mr-1.5" /> {likes}
        </Button>
        <Button variant="ghost" size="sm" className="h-8 px-3 rounded-lg text-primary">
          <ThumbsDown className="size-4 mr-1.5" /> {dislikes}
        </Button>
        <Button variant="ghost" size="sm" className="h-8 px-3 rounded-lg text-primary">
          <MessageCircle className="size-4 mr-1.5" /> {comments}
        </Button>
        <Button variant="ghost" size="icon" className="h-8 w-8 rounded-lg text-primary ml-auto">
          <MoreHorizontal className="size-4" />
        </Button>
      </div>
    </Card>
  );
}
```

**Shadcn-референс:** `Card` + `Separator` + `Button`.

---

### 3.16 DeviceCard

**Описание:** Карточка авторизованного устройства с названием, браузером, IP, датой и кнопкой удаления.

**Props:**
```tsx
interface DeviceCardProps {
  name: string;
  browser: string;
  ip: string;
  location: string;
  date: string;
  isCurrent?: boolean;
  onRemove?: () => void;
}
```

**Код:**
```tsx
import { Card, Button, Typography } from "@rollout/ui-kit";
import { X } from "lucide-react";

export function DeviceCard({ name, browser, ip, location, date, isCurrent, onRemove }: DeviceCardProps) {
  return (
    <Card className="flex items-center gap-3 p-4 bg-card border-border-default rounded-2xl">
      <div className="flex-1 min-w-0">
        <Typography variant="body" className="font-medium text-primary">
          {name}
        </Typography>
        <Typography variant="caption" className={cn("mt-0.5", isCurrent ? "text-success" : "text-secondary")}>
          {isCurrent ? "Текущий сеанс" : `${browser} • ${ip}`}
        </Typography>
        {!isCurrent && (
          <Typography variant="caption" className="text-secondary">
            {date} {location}
          </Typography>
        )}
        {isCurrent && (
          <Typography variant="caption" className="text-success">
            {ip}
          </Typography>
        )}
      </div>
      <Button
        variant="outline"
        size="icon"
        className="h-10 w-10 rounded-xl bg-transparent border-border-default shrink-0"
        onClick={onRemove}
      >
        <X className="size-5 text-primary" />
      </Button>
    </Card>
  );
}
```

**Shadcn-референс:** `Card` + `Button`.

---

### 3.17 AccordionFAQ

**Описание:** Аккордеон с частыми вопросами (используется в сертификатах).

**Props:**
```tsx
interface AccordionFAQProps {
  items: { question: string; answer: string }[];
}
```

**Код:**
```tsx
import { useState } from "react";
import { ChevronDown, ChevronUp } from "lucide-react";
import { Typography, Separator } from "@rollout/ui-kit";

export function AccordionFAQ({ items }: AccordionFAQProps) {
  const [openIndex, setOpenIndex] = useState<number | null>(null);

  return (
    <div className="divide-y divide-border-default">
      {items.map((item, i) => (
        <div key={i} className="py-4">
          <button
            className="flex items-center justify-between w-full text-left"
            onClick={() => setOpenIndex(openIndex === i ? null : i)}
          >
            <Typography variant="body" className="font-medium text-primary pr-4">
              {item.question}
            </Typography>
            {openIndex === i ? (
              <ChevronUp className="size-5 text-secondary shrink-0" />
            ) : (
              <ChevronDown className="size-5 text-secondary shrink-0" />
            )}
          </button>
          {openIndex === i && (
            <Typography variant="body" className="text-primary mt-3 leading-relaxed">
              {item.answer}
            </Typography>
          )}
        </div>
      ))}
    </div>
  );
}
```

**Shadcn-референс:** `Accordion` (кастомная реализация через useState).

---

### 3.18 Footer

**Описание:** Футер всех страниц профиля. © 2025 Rollout + социальные иконки.

**Код:**
```tsx
export function Footer() {
  return (
    <footer className="flex items-center justify-between py-6 mt-auto">
      <Typography variant="caption" className="text-secondary">
        © 2025 Rollout
      </Typography>
      <div className="flex items-center gap-4">
        <a href="https://x.com/rollout" target="_blank" rel="noopener noreferrer" className="text-secondary hover:text-primary transition-colors">
          <svg className="size-5" viewBox="0 0 24 24" fill="currentColor">
            <path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z" />
          </svg>
        </a>
        <a href="https://linkedin.com/company/rollout" target="_blank" rel="noopener noreferrer" className="text-secondary hover:text-primary transition-colors">
          <svg className="size-5" viewBox="0 0 24 24" fill="currentColor">
            <path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 0 1-2.063-2.065 2.064 2.064 0 1 1 2.063 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z" />
          </svg>
        </a>
        <a href="https://t.me/rollout" target="_blank" rel="noopener noreferrer" className="text-secondary hover:text-primary transition-colors">
          <svg className="size-5" viewBox="0 0 24 24" fill="currentColor">
            <path d="M11.944 0A12 12 0 0 0 0 12a12 12 0 0 0 12 12 12 12 0 0 0 12-12A12 12 0 0 0 12 0a12 12 0 0 0-.056 0zm4.962 7.224c.1-.002.321.023.465.14a.506.506 0 0 1 .171.325c.016.093.036.306.02.472-.18 1.898-.962 6.502-1.36 8.627-.168.9-.499 1.201-.82 1.23-.696.065-1.225-.46-1.9-.902-1.056-.693-1.653-1.124-2.678-1.8-1.185-.78-.417-1.21.258-1.91.177-.184 3.247-2.977 3.307-3.23.007-.032.014-.15-.056-.212s-.174-.041-.249-.024c-.106.024-1.793 1.14-5.061 3.345-.479.33-.913.49-1.302.48-.428-.008-1.252-.241-1.865-.44-.752-.245-1.349-.374-1.297-.789.027-.216.325-.437.893-.663 3.498-1.524 5.83-2.529 6.998-3.014 3.332-1.386 4.025-1.627 4.476-1.635z" />
          </svg>
        </a>
      </div>
    </footer>
  );
}
```

---

## 4. Структура каждого экрана

### 4.1 Profile (Главный профиль)

**Скриншот:** `Profile_☑️_Profile_1494_19919.png`  
**Скриншот (Property-версия):** `Property_🛠️_layout_2589_815.png`

**Layout:**
```
<PageContainer>
  <PageHeader title="Профиль" rightAction={<BellIcon />} />
  
  <SectionLabel>Аккаунт</SectionLabel>
  <ProfileCard name="Анжела К." email="akondrateva@gmail.ru" />
  
  <SectionLabel>Финансы</SectionLabel>
  <BalanceCard balance="10 350" currency="₽" />
  <MenuListItem icon={<CreditCard />} label="Способы оплаты" href="/profile/payment-methods" />
  <MenuListItem icon={<Ticket />} label="Промокоды" href="/profile/promo-codes" />
  <MenuListItem icon={<Gift />} label="Сертификаты" href="/profile/certificates" />
  
  <SectionLabel>Мои покупки</SectionLabel>
  <MenuListItem icon={<Package />} label="Заказы" href="/profile/orders" />
  <MenuListItem icon={<RotateCcw />} label="Возвраты" href="/profile/returns" />
  <MenuListItem icon={<Heart />} label="Избранное" href="/profile/favourites" />
  <MenuListItem icon={<BarChart3 />} label="Сравнение" href="/profile/compare" />
  <MenuListItem icon={<MessageCircleQuestion />} label="Отзывы и вопросы" href="/profile/feedback" />
  
  <SectionLabel>Настройки</SectionLabel>
  <MenuListItem icon={<HelpCircle />} label="FAQ" href="/profile/faq" />
  <MenuListItem icon={<MessageSquare />} label="Чат с поддержкой" href="/profile/support" />
  
  <SectionLabel>Поддержка</SectionLabel>
  <MenuListItem icon={<Bell />} label="Уведомления" href="/profile/notifications" />
  <MenuListItem icon={<Shield />} label="Безопасность" href="/profile/security" />
  <MenuListItem icon={<Trash2 />} label="Удалить профиль" destructive />
  
  <Button variant="secondary" className="w-full h-12 mt-2">
    Выход из профиля
  </Button>
  
  <Footer />
</PageContainer>
```

**Состояния:**
- `loading`: скелетоны для ProfileCard и BalanceCard, серые плейсхолдеры для MenuListItem
- `error`: toast-уведомление с ошибкой загрузки профиля
- `empty`: не применимо (профиль всегда есть у авторизованного пользователя)

**Интерактивность:**
- Клик на ProfileCard → `/profile/personal-data`
- Клик на MenuListItem → навигация по href
- Клик на «Выход из профиля» → открытие Dialog подтверждения, затем logout
- Клик на колокольчик → `/profile/notifications`

---

### 4.2 PersonalData (Личные данные)

**Скриншот:** `PersonalData_☑️_layout_1494_29910.png` (пустая форма), `PersonalData_☑️_layout_1494_30840.png` (заполненная)

**Layout:**
```
<PageContainer>
  <PageHeader title="Личные данные" backHref="/profile" />
  
  <Button variant="outline" className="w-full h-12 rounded-xl border-border-default">
    Заполнить через Госуслуги
  </Button>
  
  <FormField label="ФИО" placeholder="Фамилия Имя Отчество" />
  <Typography variant="caption" className="text-secondary">
    Если отчества нет, оставьте поле пустым
  </Typography>
  
  <FormField label="Адрес" placeholder="Город, улица, дом" icon={<Search />} />
  <FormField label="Дата рождения" placeholder="00.00.0000" type="date" />
  <SelectField label="Пол" options={["Мужской", "Женский"]} />
  <FormField label="Эл. почта" placeholder="username@mail.com" type="email" />
  <SelectField label="Страна" options={["Россия", "Казахстан", "Беларусь"]} />
  <SelectField label="Язык" options={["Русский", "English"]} />
  <SelectField label="Валюта" options={["Российский рубль", "Доллар США"]} />
  <SelectField label="Часовой пояс" options={["(UTC+03:00) Москва"]} />
  
  <MenuListItem
    icon={<Box />}
    label="Данные для таможни"
    description="Для оформления посылок"
    href="/profile/customs-data"
  />
  
  <Footer />
</PageContainer>
```

**Состояния:**
- `loading`: скелетоны для полей формы
- `error`: inline-ошибки под полями (красный текст)
- `success`: toast «Данные сохранены»
- `empty`: форма с placeholder'ами

**Интерактивность:**
- `onChange` каждого поля → локальный `useState`
- Клик на «Госуслуги» → открытие OAuth-флоу (mock)
- Клик на «Данные для таможни» → навигация

---

### 4.3 CustomsData (Данные для таможни)

**Скриншот:** `PersonalData_☑️_layout_1494_31327.png`

**Layout:**
```
<PageContainer>
  <PageHeader title="Данные для таможни" backHref="/profile/personal-data" />
  
  <FormField label="Серия и номер паспорта" placeholder="0000 000000" />
  <FormField label="Код подразделения" placeholder="000-000" />
  <FormField label="Кем выдан" placeholder="Название подразделения" />
  <FormField label="Дата выдачи" placeholder="ДД.ММ.ГГГГ" type="date" />
  <FormField label="Место рождения" placeholder="Населённый пункт" />
  <FormField label="Адрес регистрации" placeholder="Город, улица, дом" icon={<Search />} />
  <FormField label="ИНН" placeholder="000000000" />
  
  <Footer />
</PageContainer>
```

---

### 4.4 NotificationsSettings (Настройка уведомлений)

**Скриншот:** `Notifications_☑️_layout_1447_33570.png`

**Layout:**
```
<PageContainer>
  <PageHeader title="Настройка уведомлений" backHref="/profile" />
  
  <SectionLabel>Куда отправляются</SectionLabel>
  <Typography variant="caption" className="text-secondary">
    Места, где удобнее получать уведомления
  </Typography>
  <ToggleSwitch label="Пуш" checked={true} />
  <ToggleSwitch label="Эл. почта" checked={false} />
  <ToggleSwitch label="Колокольчик" checked={false} />
  
  <SectionLabel>По какому поводу</SectionLabel>
  <Typography variant="caption" className="text-secondary">
    На какое событие хотите получать уведомления
  </Typography>
  <ToggleSwitch label="Подборки и рекомендации" checked={true} />
  <ToggleSwitch label="Акции и скидки" checked={false} />
  <ToggleSwitch label="Новости" checked={false} />
  <ToggleSwitch label="Сообщения" checked={false} />
  <ToggleSwitch label="Участие в исследованиях" checked={true} />
  <ToggleSwitch label="Избранные" checked={true} />
  <ToggleSwitch label="Отзывы" checked={false} />
  <ToggleSwitch label="Статус заказа" checked={false} />
  
  <Footer />
</PageContainer>
```

---

### 4.5 NotificationsList (Список уведомлений)

**Скриншот:** `Notifications_☑️_layout_1535_73093.png`

**Layout:**
```
<PageContainer>
  <PageHeader title="Уведомления" backHref="/profile" />
  
  <SegmentedTabs
    tabs={[
      { id: "all", label: "Все" },
      { id: "discounts", label: "Скидки и акции", badge: 3 },
      { id: "new", label: "Новинки" }
    ]}
  />
  
  <div className="flex flex-col gap-2 mt-4">
    <NotificationItem
      icon={<Info className="size-5 text-primary" />}
      title="Новый счёт"
      description="Мы вынуждены отталкиваться от того, что семантический разб..."
      time="3:45"
    />
    {/* ... ещё элементы */}
  </div>
  
  <Footer />
</PageContainer>
```

---

### 4.6 Security (Безопасность)

**Скриншот:** `Security_☑️_layout_1501_60680.png`

**Layout:**
```
<PageContainer>
  <PageHeader title="Безопасность" backHref="/profile" />
  
  <SectionLabel>Настройки</SectionLabel>
  <MenuListItem
    icon={null}
    label="Пароль"
    description="Менялся последний раз 15.05.2025, 10:21"
    href="/profile/security/password"
  />
  <MenuListItem
    icon={null}
    label="Подключить 2FA"
    description="Двухфакторная аутентификация"
    href="/profile/security/2fa"
  />
  <MenuListItem
    icon={null}
    label="Подключить FaceID"
    description="Вход с помощью распознавания лица"
    href="/profile/security/faceid"
  />
  
  <SectionLabel>Авторизованные устройства</SectionLabel>
  <div className="flex flex-col gap-2">
    <DeviceCard
      name="iPhone 16 Pro"
      ip="92.100.248.164"
      isCurrent={true}
    />
    <DeviceCard
      name="Desktop Mac OS X"
      browser="Chrome 135.0"
      ip="178.67.246.192"
      location="Россия, Санкт..."
      date="13 мая 2025 в 12:05"
    />
  </div>
  
  <Button variant="secondary" className="w-full h-12 mt-4">
    Разлогиниться на всех устройствах
  </Button>
  
  <Footer />
</PageContainer>
```

---

### 4.7 Orders (Заказы)

**Скриншот:** `Orders_☑️_layout_1503_66384.png` (активные), `Orders_☑️_layout_1503_66758.png` (завершённые)

**Layout:**
```
<PageContainer>
  <PageHeader title="Заказы" backHref="/profile" />
  
  <SegmentedTabs tabs={[
    { id: "active", label: "Актуальные" },
    { id: "completed", label: "Завершённые" }
  ]} />
  
  <CategoryFilterChips categories={[
    { id: "all", label: "Все" },
    { id: "products", label: "Продукты" },
    { id: "auto", label: "Авто" },
    { id: "flowers", label: "Цветы" },
    { id: "restaurants", label: "Рестораны" }
  ]} />
  
  <div className="flex flex-col gap-4 mt-4">
    <OrderCard
      status="active"
      deliveryDate="20 сентября"
      address="ПВЗ, Санкт-Петербург, пр. Невский, 28"
      items={[...]}
    />
    {/* ... ещё карточки */}
  </div>
  
  <Footer />
</PageContainer>
```

**Состояния:**
- `loading`: скелетоны OrderCard (3 штуки)
- `error`: пустой экран с сообщением и кнопкой «Повторить»
- `empty`: иллюстрация «Нет заказов» + кнопка «Перейти к покупкам»

---

### 4.8 Favourites (Избранное)

**Скриншот:** `Favourites_☑️_layout_1317_30681.png` (все), `Favourites_☑️_layout_1317_32208.png` (подборки)

**Layout:**
```
<PageContainer>
  <PageHeader title="Избранное" backHref="/profile" rightAction={<SearchIcon />} />
  
  <SegmentedTabs tabs={[
    { id: "all", label: "Все", badge: 25 },
    { id: "collections", label: "Подборки", badge: 2 }
  ]} />
  
  <div className="flex gap-2 mt-4">
    <Button size="icon" variant="outline" className="h-9 w-9 rounded-lg">
      <ArrowUpDown className="size-4" />
    </Button>
    <Button variant="outline" className="h-9 rounded-lg">
      <SlidersHorizontal className="size-4 mr-1" /> Фильтр
    </Button>
    <Button variant="outline" className="h-9 rounded-lg">
      Цена <ChevronDown className="size-4 ml-1" />
    </Button>
    <Button variant="outline" className="h-9 rounded-lg">
      Рейтинг <ChevronDown className="size-4 ml-1" />
    </Button>
  </div>
  
  <div className="grid grid-cols-2 gap-3 mt-4">
    <ProductCard {...} />
    <ProductCard {...} />
    {/* ... */}
  </div>
  
  <Button className="w-full h-12 mt-4 bg-accent-primary text-accent-primary-text rounded-xl">
    Показать все
  </Button>
  
  <Footer />
</PageContainer>
```

---

### 4.9 Feedback (Отзывы и вопросы)

**Скриншот:** `Feedback__☑️_layout_1542_22708.png` (ждут отзыва), `Feedback__☑️_layout_1542_21752.png` (отзывы с меню)

**Layout:**
```
<PageContainer>
  <PageHeader title="Отзывы и вопросы" backHref="/profile" />
  
  <SegmentedTabs tabs={[
    { id: "pending", label: "Ждут отзыва", badge: 8 },
    { id: "reviews", label: "Отзывы" },
    { id: "questions", label: "Вопросы" },
    { id: "answers", label: "Ответы" }
  ]} />
  
  {/* Tab: Ждут отзыва */}
  <div className="flex flex-col gap-4 mt-4">
    <div className="flex items-center gap-3 p-4 bg-card rounded-2xl">
      <img src="..." className="size-14 rounded-lg object-cover" />
      <Typography variant="body" className="flex-1 font-medium text-primary line-clamp-2">
        Смартфон Phone Pro Max, nano SIM + eSIM сим-карт 256 ГБ
      </Typography>
      <Button className="h-9 px-4 rounded-lg bg-elevated text-primary text-sm">
        Оценить
      </Button>
    </div>
  </div>
  
  {/* Tab: Отзывы */}
  <div className="flex flex-col gap-4 mt-4">
    <ReviewCard {...} />
  </div>
  
  <Footer />
</PageContainer>
```

---

### 4.10 PromoCodes (Промокоды)

**Скриншот:** `PromoСode_☑️_layout_1317_34279.png`

**Layout:**
```
<PageContainer>
  <PageHeader title="Промокоды" backHref="/profile" />
  
  <FormField
    label=""
    placeholder="Введите промокод"
    value={code}
    onChange={setCode}
  />
  <Button className="w-full h-12 bg-accent-primary text-accent-primary-text rounded-xl font-medium">
    Активировать
  </Button>
  
  <SectionLabel>Ваши промокоды</SectionLabel>
  <div className="flex flex-col">
    <PromoCodeItem title="Товары за 1 ₽" description="до 28 октября" />
    <Separator className="bg-border-default" />
    <PromoCodeItem title="-10% на товары" description="Item description goes here" />
    <Separator className="bg-border-default" />
    <PromoCodeItem title="-90% на товары" description="до 27 декабря 12:00" />
    <Separator className="bg-border-default" />
    <PromoCodeItem title="Товары за 1 ₽" description="до 23 декабря 10:46" />
  </div>
  
  <Footer />
</PageContainer>
```

---

### 4.11 Certificates (Сертификаты)

**Скриншот:** `Certificate_☑️_layout_1532_42701.png` (список + модалка), `Certificate_☑️_layout_1532_61545.png` (FAQ), `Certificate_☑️_layout_1445_1959.png`

**Layout:**
```
<PageContainer>
  <PageHeader title="Сертификаты" backHref="/profile" />
  
  <div className="flex flex-col">
    <PromoCodeItem title="Товары за 1 ₽" description="до 28 октября" />
    <Separator className="bg-border-default" />
    <PromoCodeItem title="-90% на товары" description="до 23 декабря 10:46" />
  </div>
  
  {/* FAQ Accordion — может быть в модальном окне или на отдельной странице */}
  <Dialog>
    <Dialog.Trigger asChild>
      <Button variant="ghost" className="w-full mt-4 text-primary">
        Частые вопросы
      </Button>
    </Dialog.Trigger>
    <Dialog.Content className="bg-page border-border-default rounded-2xl p-6">
      <Dialog.Header>
        <Dialog.Title className="text-center text-primary text-xl font-semibold">
          Частые вопросы
        </Dialog.Title>
        <Dialog.Description className="text-center text-secondary mt-1">
          Ответы на частые вопросы
        </Dialog.Description>
      </Dialog.Header>
      <AccordionFAQ items={faqItems} />
      <Button className="w-full h-12 mt-6 bg-accent-primary text-accent-primary-text rounded-xl">
        Закрыть
      </Button>
    </Dialog.Content>
  </Dialog>
  
  <Footer />
</PageContainer>
```

---

### 4.12 Cars (Автомобили) — из Property 🛠️

**Скриншот:** `Property_🛠️_layout_2624_20655.png`

**Layout:**
```
<PageContainer>
  <PageHeader title="Автомобили" backHref="/profile" />
  
  <div className="flex flex-col gap-2">
    <Card className="flex items-center gap-3 p-4 bg-card border-border-default rounded-2xl">
      <img src="..." className="size-12 rounded-lg object-cover" />
      <div className="flex-1">
        <Typography variant="body" className="font-medium text-primary">
          Citroen C3 • A123OA 197
        </Typography>
        <Typography variant="caption" className="text-secondary">
          без оценки
        </Typography>
      </div>
      <ChevronRight className="size-5 text-secondary" />
    </Card>
  </div>
  
  <Button className="w-full h-12 mt-4 bg-secondary text-primary rounded-xl">
    Добавить авто
  </Button>
  
  <Footer />
</PageContainer>
```

---

## 5. Пример полной страницы

### 5.1 PromoCodesPage.tsx

```tsx
import { useState } from "react";
import { useNavigate } from "react-router-dom";
import {
  Button,
  Input,
  Field,
  Typography,
  Separator,
  Card,
} from "@rollout/ui-kit";
import { ArrowLeft, Gift, ChevronRight } from "lucide-react";
import { cn } from "@rollout/ui-kit";
import { Footer } from "@/components/Footer";

// --- Mock data ---
const mockPromoCodes = [
  { id: "1", title: "Товары за 1 ₽", description: "до 28 октября" },
  { id: "2", title: "-10% на товары", description: "Item description goes here" },
  { id: "3", title: "-90% на товары", description: "до 27 декабря 12:00" },
  { id: "4", title: "Товары за 1 ₽", description: "до 23 декабря 10:46" },
];

// --- Components ---
function PromoCodeItem({ title, description }: { title: string; description: string }) {
  return (
    <div className="flex items-center gap-3 py-3">
      <div className="flex items-center justify-center size-12 rounded-xl bg-elevated shrink-0">
        <Gift className="size-6 text-primary" />
      </div>
      <div className="flex-1 min-w-0">
        <Typography variant="body" className="font-medium text-primary">
          {title}
        </Typography>
        <Typography variant="caption" className="text-secondary mt-0.5">
          {description}
        </Typography>
      </div>
      <Button
        variant="outline"
        className="h-9 px-4 rounded-lg bg-transparent border-border-default text-primary text-sm font-medium shrink-0"
      >
        Применить
      </Button>
    </div>
  );
}

// --- Page ---
export function PromoCodesPage() {
  const navigate = useNavigate();
  const [code, setCode] = useState("");
  const [isLoading, setIsLoading] = useState(false);

  const handleActivate = () => {
    if (!code.trim()) return;
    setIsLoading(true);
    // Mock API call
    setTimeout(() => {
      setIsLoading(false);
      setCode("");
    }, 1000);
  };

  return (
    <div className="mx-auto flex max-w-[576px] flex-col gap-7 pt-20 pb-8 px-4 min-h-screen">
      {/* Header */}
      <div className="flex items-center gap-3 py-4">
        <Button
          variant="ghost"
          size="icon"
          className="shrink-0 text-primary"
          onClick={() => navigate("/profile")}
        >
          <ArrowLeft className="size-6" />
        </Button>
        <Typography variant="h1" className="flex-1">
          Промокоды
        </Typography>
      </div>

      {/* Input + Activate */}
      <div className="flex flex-col gap-3">
        <Field>
          <Input
            value={code}
            onChange={(e) => setCode(e.target.value)}
            placeholder="Введите промокод"
            className="w-full h-12 bg-card border border-border-default rounded-xl text-primary placeholder:text-tertiary focus:border-border-subtle focus:ring-0 px-4"
          />
        </Field>
        <Button
          className="w-full h-12 bg-accent-primary text-accent-primary-text rounded-xl font-medium"
          onClick={handleActivate}
          disabled={isLoading || !code.trim()}
        >
          {isLoading ? "Активация..." : "Активировать"}
        </Button>
      </div>

      {/* List */}
      <div className="flex flex-col">
        <Typography variant="body" className="font-medium text-primary mb-2">
          Ваши промокоды
        </Typography>
        <div className="flex flex-col">
          {mockPromoCodes.map((item, index) => (
            <div key={item.id}>
              <PromoCodeItem title={item.title} description={item.description} />
              {index < mockPromoCodes.length - 1 && (
                <Separator className="bg-border-default" />
              )}
            </div>
          ))}
        </div>
      </div>

      <Footer />
    </div>
  );
}
```

**Импорты:**
- Все компоненты UI из `@rollout/ui-kit`
- Иконки из `lucide-react`
- `useState` для локального состояния формы
- `useNavigate` для навигации
- Mock-обработчики без `console.log`

---

## 6. Чек-лист вёрстки

- [ ] Все цвета через токены (не хардкод) — используются `bg-page`, `bg-card`, `text-primary`, `text-secondary`, `border-border-default`, `bg-accent-primary`, `text-accent-primary-text`
- [ ] Типографика совпадает с макетом — Geist Variable, размеры, веса, line-height по таблице §2.2
- [ ] Отступы и радиусы по дизайн-системе — `gap-7`, `rounded-2xl` (16px) для карточек, `rounded-xl` (12px) для input и кнопок
- [ ] Иконки из `lucide-react` — `size-5` (20px) для inline, `size-6` (24px) для навигации
- [ ] Контейнер `max-w-[576px] mx-auto` на всех страницах
- [ ] `pt-20` для отступа от шапки на всех страницах
- [ ] `pb-tabbar md:pb-0` для мобильного отступа (добавить класс для safe-area)
- [ ] Все формы на локальном `useState` — без подключения к API
- [ ] Mock-обработчики без `console.log` — используются `setTimeout` или `alert` для демо
- [ ] Route добавлен в `App.tsx` — все 11 маршрутов из §1.3
- [ ] Скриншот соответствует макету (визуальный diff) — проверить по каждому экрану из §4
- [ ] Footer присутствует на всех страницах — © 2025 Rollout + социальные иконки (X, LinkedIn, Telegram)
- [ ] ToggleSwitch реализован в iOS-стиле — 51×31px, круглый knob 27px
- [ ] SegmentedTabs — pill-стиль, `rounded-full`, активный tab с `bg-elevated`
- [ ] CategoryFilterChips — горизонтальный скролл без scrollbar
- [ ] OrderCard — группировка по дате, миниатюры товаров, кнопки «Оценить»/«Вернуть» только для завершённых
- [ ] ProductCard — 2-колоночная сетка, бейджи тегов, цена со скидкой, кнопки «В корзину»/«Позвонить» + Heart
- [ ] ReviewCard — звёзды рейтинга, достоинства/недостатки, реакции (like/dislike/comment/more)
- [ ] NotificationItem — иконка в круглом фоне, title + description + time
- [ ] DeviceCard — текущий сеанс выделен зелёным (`text-success`)
- [ ] AccordionFAQ — раскрытие по клику, ChevronDown/ChevronUp
- [ ] Dialog/Modal для FAQ — затемнение overlay, кнопка «Закрыть» primary
- [ ] Не использовать `radix-ui` напрямую, не `shadcn add`, не хардкод цвета
- [ ] Импортировать всё из `@rollout/ui-kit` (не deep imports)
- [ ] Не использовать `console.log` в production-коде
- [ ] Все ссылки и кнопки имеют `cursor-pointer` и hover-эффекты
- [ ] Line-clamp для длинных текстов (title, description) — `line-clamp-2`
- [ ] Placeholder'ы в input — цвет `text-tertiary` (#636366)
- [ ] Avatar — `rounded-full`, fallback с инициалами
- [ ] Badge на табах — small circle, `bg-badge-bg`, `text-badge-text`
- [ ] Separator между элементами списка — `bg-border-default`, 1px
- [ ] Деструктивные действия (удалить профиль) — цвет `text-danger` (#FF453A)

---

## Приложение: Список скачанных скриншотов

| Файл | Страница | Описание |
|------|----------|----------|
| `Cover_3._Profile_1926_2.png` | Cover | Обложка модуля |
| `Profile_☑️_Profile_1494_19919.png` | Profile ☑️ | Главный профиль (краткая версия) |
| `PersonalData_☑️_layout_1494_29910.png` | PersonalData ☑️ | Личные данные — пустая форма |
| `PersonalData_☑️_layout_1494_30840.png` | PersonalData ☑️ | Личные данные — заполненная |
| `PersonalData_☑️_layout_1494_31327.png` | PersonalData ☑️ | Данные для таможни |
| `PersonalData_☑️_layout_1494_31455.png` | PersonalData ☑️ | Личные данные (доп. layout) |
| `Notifications_☑️_layout_1447_33570.png` | Notifications ☑️ | Настройка уведомлений (toggles) |
| `Notifications_☑️_layout_1535_73093.png` | Notifications ☑️ | Список уведомлений (tabs) |
| `Security_☑️_layout_1501_60680.png` | Security ☑️ | Безопасность (пароль, 2FA, устройства) |
| `Orders_☑️_layout_1503_66384.png` | Orders ☑️ | Заказы — Актуальные |
| `Orders_☑️_layout_1503_66758.png` | Orders ☑️ | Заказы — Завершённые |
| `Orders_☑️_layout_1503_66848.png` | Orders ☑️ | Заказы (доп. layout) |
| `Favourites_☑️_layout_1317_30681.png` | Favourites ☑️ | Избранное — Все (grid) |
| `Favourites_☑️_layout_1317_32208.png` | Favourites ☑️ | Избранное — Подборки |
| `Favourites_☑️_layout_1445_18876.png` | Favourites ☑️ | Избранное (доп.) |
| `Favourites_☑️_layout_1445_19151.png` | Favourites ☑️ | Выбор товаров для возврата (checkbox) |
| `Feedback__☑️_layout_1542_22708.png` | Feedback ☑️ | Ждут отзыва |
| `Feedback__☑️_layout_1319_28908.png` | Feedback ☑️ | Отзывы (доп.) |
| `Feedback__☑️_layout_1542_21752.png` | Feedback ☑️ | Отзывы с меню действий |
| `PromoСode_☑️_layout_1317_34279.png` | PromoСode ☑️ | Промокоды — список |
| `PromoСode_☑️_layout_1528_12642.png` | PromoСode ☑️ | Промокоды (доп.) |
| `Certificate_☑️_layout_1532_42701.png` | Certificate ☑️ | Сертификаты + модалка добавления |
| `Certificate_☑️_layout_1532_61545.png` | Certificate ☑️ | Частые вопросы (FAQ accordion) |
| `Certificate_☑️_layout_1445_1959.png` | Certificate ☑️ | Сертификаты (доп.) |
| `Property_🛠️_layout_2589_815.png` | Property 🛠️ | Профиль с Property-разделами |
| `Property_🛠️_layout_2624_20655.png` | Property 🛠️ | Автомобили |
| `Property_🛠️_layout_2659_11089.png` | Property 🛠️ | Недвижимость (доп.) |

**Всего скачано:** 27 скриншотов  
**Путь к скриншотам:** `/Users/mokoloskov/Documents/kimi/workspace/3-Profile_screenshots/`  
**Путь к инструкции:** `/Users/mokoloskov/Documents/kimi/workspace/3-Profile-instruction.md`
