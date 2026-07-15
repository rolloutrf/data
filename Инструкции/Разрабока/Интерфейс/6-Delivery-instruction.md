# Модуль 6-Delivery — Инструкция по вёрстке на React

> Файл: `6-Delivery.json`  
> Figma: `v5UoHLSS3oHElCwNmFotbX`  
> Скриншоты: `6-Delivery_screenshots/`  
> Дата: 2026-06-21

---

## 1. Обзор модуля

### Назначение
Модуль **6-Delivery** отвечает за выбор способа доставки (пункт выдачи / курьер), выбор конкретного пункта на карте или ввод адреса курьерской доставки, а также оформление заказа с выбором временного слота.

### Экраны модуля (из Figma)

| # | Страница Figma | Ключевые скриншоты | Назначение |
|---|----------------|-------------------|------------|
| 1 | Cover | `cover.png` | Обложка модуля |
| 2 | Choosing Point | `choosing_point.png` | Карта с выбором пункта выдачи |
| 3 | Delivery Method | `delivery_method_1-14.png` | Способы доставки (14 состояний) |
| 4 | Making an Order | `making_an_order.png` | Оформление заказа |
| 5 | Delivery | — | Пустая страница (нет фреймов) |

### Уникальные экраны (сгруппировано по состояниям)

1. **ChoosingPointPage** — Карта с фильтрами пунктов выдачи (`choosing_point.png`)
2. **DeliveryMethodCourierPage** — Карта с курьерской доставкой (`delivery_method_8-11.png`)
3. **DeliveryMethodTooltipPage** — Карта с тултипами пунктов (`delivery_method_3.png`)
4. **PickupListSheet** — Bottom Sheet: список пунктов выдачи (`delivery_method_4.png`)
5. **PickupDetailSheet** — Bottom Sheet: детали пункта выдачи (`delivery_method_5.png`)
6. **DeliveryEmptySheet** — Bottom Sheet: "Куда доставить?" (`delivery_method_6.png`)
7. **AddressSearchSheet** — Bottom Sheet: поиск адреса (`delivery_method_7.png`, `delivery_method_12.png`)
8. **AddressFormSheet** — Bottom Sheet: форма адреса доставки (`delivery_method_13.png`, `delivery_method_14.png`)
9. **OrderCheckoutPage** — Оформление заказа (`making_an_order.png`)

### Роуты (URL-пути)

```
/delivery/choosing-point          → ChoosingPointPage
/delivery/courier                 → DeliveryMethodCourierPage
/delivery/order                   → OrderCheckoutPage
```

Bottom Sheets открываются как модальные окна/шторки поверх текущего экрана:

```
/delivery/choosing-point?sheet=pickup-list
/delivery/choosing-point?sheet=pickup-detail&id=123
/delivery/choosing-point?sheet=address-search
/delivery/courier?sheet=address-form
```

---

## 2. Дизайн-система

### 2.1 Цвета

| Токен | Значение | Использование |
|-------|----------|---------------|
| `--bg-primary` | `#0A0A0A` | Фон страницы |
| `--bg-card` | `#1C1C1E` | Фон карточек, bottom sheet, input, чипов, табов |
| `--bg-active` | `#3A3A3C` | Активный таб, бордер, фон badge, трек слайдера |
| `--text-primary` | `#FAFAFA` | Заголовки, основной текст, иконки |
| `--text-secondary` | `#8E8E93` | Подзаголовки, описания, placeholder |
| `--text-muted` | `#636366` | Placeholder в input, disabled текст |
| `--border-default` | `#3A3A3C` | Бордеры карточек, input, чипов, радио-кнопок |
| `--border-selected` | `#FAFAFA` | Выбранная радио-карточка, активный чип |
| `--button-primary-bg` | `#E5E5E5` | Фон primary кнопки |
| `--button-primary-text` | `#0A0A0A` | Текст primary кнопки |
| `--button-secondary-bg` | `transparent` | Фон secondary кнопки |
| `--button-secondary-border` | `#3A3A3C` | Бордер secondary кнопки |
| `--accent-orange` | `#FF6B00` | Метка курьера на карте (оранжевая) |
| `--accent-blue` | `#007AFF` | Метки пунктов на карте (синие) |
| `--sheet-handle` | `#3A3A3C` | Ручка сверху bottom sheet |

### 2.2 Типографика

| Элемент | Размер | Вес | Line-height | Цвет |
|---------|--------|-----|-------------|------|
| Page Title | 28px | 700 (bold) | 1.2 | `#FAFAFA` |
| Page Subtitle | 16px | 400 | 1.4 | `#8E8E93` |
| Section Title | 20px | 600 | 1.3 | `#FAFAFA` |
| Card Title | 18px | 600 | 1.3 | `#FAFAFA` |
| Body | 16px | 400 | 1.5 | `#FAFAFA` |
| Body Secondary | 14px | 400 | 1.4 | `#8E8E93` |
| Caption | 13px | 400 | 1.3 | `#636366` |
| Button | 16px | 600 | 1 | `#0A0A0A` (primary) / `#FAFAFA` (secondary) |
| Badge / Chip | 14px | 500 | 1 | `#FAFAFA` |
| Input Value | 16px | 400 | 1.5 | `#FAFAFA` |
| Input Placeholder | 16px | 400 | 1.5 | `#636366` |
| List Item Title | 16px | 600 | 1.3 | `#FAFAFA` |
| List Item Subtitle | 14px | 400 | 1.4 | `#8E8E93` |
| Tooltip | 14px | 500 | 1.3 | `#0A0A0A` |

Шрифт: `Geist Variable` (default в проекте).

### 2.3 Спейсинг

| Параметр | Значение |
|----------|----------|
| Контейнер max-width | `576px` |
| Контейнер padding-top | `pt-20` (отступ от шапки) |
| Контейнер padding-bottom | `pb-8` (desktop) / `pb-tabbar` (mobile) |
| Контейнер gap | `gap-7` (28px) |
| Карточка padding | `p-5` (20px) |
| Карточка border-radius | `rounded-2xl` (16px) |
| Input border-radius | `rounded-xl` (12px) |
| Button border-radius | `rounded-3xl` (24px) |
| Tab border-radius | `rounded-full` (24px) |
| Chip border-radius | `rounded-2xl` (16px) |
| Gap между секциями | `gap-6` (24px) |
| Gap внутри карточки | `gap-4` (16px) |
| Gap между списком | `gap-0` (разделитель 1px) |
| Bottom Sheet handle | `w-10 h-1 rounded-full` (40×4px) |
| Bottom Sheet top-radius | `rounded-t-2xl` (16px) |
| Map corner radius | `rounded-2xl` (16px) |

### 2.4 Иконки

- Иконки из `lucide-react`
- Размер: `size-5` (20px) для inline, `size-6` (24px) для кнопок/экшенов
- Цвет: наследуется от текста (`currentColor`)
- Ключевые иконки:
  - `ArrowLeft` — кнопка назад
  - `MapPin` — метка пункта/адреса
  - `ChevronRight` — переход в списке
  - `Navigation` — кнопка геолокации на карте
  - `Search` — иконка поиска в input
  - `Mic` — голосовой ввод в input
  - `User` — получатель заказа
  - `Clock` — временной слот
  - `RadioCircle` (custom) — радио-кнопка в слоте
  - `X`, `Linkedin`, `Send` (Telegram) — социконки в футере

---

## 3. Компоненты UI

### 3.1 PageHeader

Название: **PageHeader**  
Описание: Шапка страницы с кнопкой назад, заголовком и подзаголовком.

```typescript
interface PageHeaderProps {
  title: string;
  subtitle?: string;
  onBack?: () => void;
}
```

```tsx
import { ArrowLeft } from "lucide-react";
import { Button, Typography } from "@rollout/ui-kit";

export function PageHeader({ title, subtitle, onBack }: PageHeaderProps) {
  return (
    <div className="flex flex-col gap-2">
      <div className="flex items-center gap-3">
        {onBack && (
          <Button variant="ghost" size="icon" onClick={onBack}>
            <ArrowLeft className="size-6 text-[#FAFAFA]" />
          </Button>
        )}
        <div className="flex flex-col">
          <Typography variant="h1" className="text-[28px] font-bold text-[#FAFAFA]">
            {title}
          </Typography>
          {subtitle && (
            <Typography variant="body" className="text-[16px] text-[#8E8E93]">
              {subtitle}
            </Typography>
          )}
        </div>
      </div>
    </div>
  );
}
```

> **Shadcn ref:** `Button` (variant="ghost", size="icon")

---

### 3.2 DeliveryTabs

Название: **DeliveryTabs**  
Описание: Segmented tabs для переключения между "Пункт выдачи" и "Курьер".

```typescript
interface DeliveryTabsProps {
  activeTab: "pickup" | "courier";
  onTabChange: (tab: "pickup" | "courier") => void;
}
```

```tsx
import { Tabs, cn } from "@rollout/ui-kit";

export function DeliveryTabs({ activeTab, onTabChange }: DeliveryTabsProps) {
  return (
    <div className="bg-[#1C1C1E] rounded-full p-1 flex">
      <button
        onClick={() => onTabChange("pickup")}
        className={cn(
          "flex-1 py-3 px-4 rounded-full text-[16px] font-medium transition-colors",
          activeTab === "pickup"
            ? "bg-[#3A3A3C] text-[#FAFAFA]"
            : "text-[#8E8E93] hover:text-[#FAFAFA]"
        )}
      >
        Пункт выдачи
      </button>
      <button
        onClick={() => onTabChange("courier")}
        className={cn(
          "flex-1 py-3 px-4 rounded-full text-[16px] font-medium transition-colors",
          activeTab === "courier"
            ? "bg-[#3A3A3C] text-[#FAFAFA]"
            : "text-[#8E8E93] hover:text-[#FAFAFA]"
        )}
      >
        Курьер
      </button>
    </div>
  );
}
```

> **Shadcn ref:** `Tabs` (custom segmented variant)

---

### 3.3 FilterChips

Название: **FilterChips**  
Описание: Горизонтальный скроллящийся список фильтров-провайдеров доставки.

```typescript
interface FilterChipsProps {
  filters: { id: string; label: string }[];
  activeFilter: string | null;
  onFilterChange: (id: string) => void;
}
```

```tsx
import { cn } from "@rollout/ui-kit";

export function FilterChips({ filters, activeFilter, onFilterChange }: FilterChipsProps) {
  return (
    <div className="flex gap-2 overflow-x-auto scrollbar-hide pb-1">
      {filters.map((filter) => (
        <button
          key={filter.id}
          onClick={() => onFilterChange(filter.id)}
          className={cn(
            "shrink-0 px-4 py-2 rounded-2xl text-[14px] font-medium border transition-colors",
            activeFilter === filter.id
              ? "bg-[#3A3A3C] border-[#3A3A3C] text-[#FAFAFA]"
              : "bg-[#1C1C1E] border-[#3A3A3C] text-[#FAFAFA]"
          )}
        >
          {filter.label}
        </button>
      ))}
    </div>
  );
}
```

> **Shadcn ref:** `Badge` (custom chip variant)

---

### 3.4 DeliveryMap

Название: **DeliveryMap**  
Описание: Контейнер для карты с оверлеями (метки, тултипы, кнопка геолокации). В MVP — мок-карта (img или div).

```typescript
interface DeliveryMapProps {
  mode: "pickup" | "courier";
  markers?: MapMarker[];
  onLocateClick?: () => void;
}

interface MapMarker {
  id: string;
  lat: number;
  lng: number;
  type: "postamat" | "cdek" | "yandex" | "ozon" | "courier";
  label?: string;
  tooltip?: string;
}
```

```tsx
import { Navigation } from "lucide-react";
import { Button } from "@rollout/ui-kit";

export function DeliveryMap({ mode, markers, onLocateClick }: DeliveryMapProps) {
  return (
    <div className="relative w-full aspect-[3/4] bg-[#1C1C1E] rounded-2xl overflow-hidden">
      {/* Мок карты — в реализации заменить на Map API */}
      <div className="absolute inset-0 bg-[#0A0A0A] flex items-center justify-center text-[#636366]">
        [Map Placeholder]
      </div>
      
      {/* Метки */}
      {markers?.map((marker) => (
        <div
          key={marker.id}
          className="absolute"
          style={{ left: `${marker.lng}%`, top: `${marker.lat}%` }}
        >
          <div className={cn(
            "w-8 h-8 rounded-full flex items-center justify-center text-white text-sm font-bold",
            marker.type === "courier" ? "bg-[#FF6B00]" : "bg-[#007AFF]"
          )}>
            {marker.type === "courier" ? "📍" : marker.label?.[0]}
          </div>
          {marker.tooltip && (
            <div className="absolute -top-10 left-1/2 -translate-x-1/2 bg-[#FAFAFA] text-[#0A0A0A] px-3 py-1.5 rounded-xl text-[14px] font-medium whitespace-nowrap shadow-lg">
              {marker.tooltip}
            </div>
          )}
        </div>
      ))}
      
      {/* Кнопка геолокации */}
      <Button
        variant="secondary"
        size="icon"
        className="absolute bottom-4 right-4 bg-[#1C1C1E] border-[#3A3A3C]"
        onClick={onLocateClick}
      >
        <Navigation className="size-5 text-[#FAFAFA]" />
      </Button>
    </div>
  );
}
```

> **Shadcn ref:** `Button` (variant="secondary", size="icon")

---

### 3.5 PickupListItem

Название: **PickupListItem**  
Описание: Элемент списка пункта выдачи с адресом, ценой и сроком.

```typescript
interface PickupListItemProps {
  title: string;
  address: string;
  price: string;
  duration: string;
  onClick?: () => void;
}
```

```tsx
import { ChevronRight } from "lucide-react";
import { Card, Typography } from "@rollout/ui-kit";

export function PickupListItem({ title, address, price, duration, onClick }: PickupListItemProps) {
  return (
    <button
      onClick={onClick}
      className="w-full text-left py-4 flex items-center justify-between group"
    >
      <div className="flex flex-col gap-1">
        <Typography variant="body" className="text-[16px] font-semibold text-[#FAFAFA]">
          {title}
        </Typography>
        <Typography variant="body" className="text-[14px] text-[#8E8E93]">
          {address}
        </Typography>
        <Typography variant="body" className="text-[14px] text-[#8E8E93]">
          {price}, {duration}
        </Typography>
      </div>
      <ChevronRight className="size-5 text-[#8E8E93] group-hover:text-[#FAFAFA] transition-colors" />
    </button>
  );
}
```

> **Shadcn ref:** `Card` (custom row variant)

---

### 3.6 PickupDetailSheet

Название: **PickupDetailSheet**  
Описание: Bottom Sheet с деталями пункта выдачи и кнопкой подтверждения.

```typescript
interface PickupDetailSheetProps {
  isOpen: boolean;
  onClose: () => void;
  onConfirm: () => void;
  pickup: {
    title: string;
    address: string;
    price: string;
    deliveryDate: string;
    storageDays: string;
    workingHours: string;
  };
}
```

```tsx
import { Sheet, Button, Separator, Typography } from "@rollout/ui-kit";

export function PickupDetailSheet({ isOpen, onClose, onConfirm, pickup }: PickupDetailSheetProps) {
  return (
    <Sheet open={isOpen} onOpenChange={onClose}>
      <Sheet.Content className="bg-[#1C1C1E] rounded-t-2xl p-6">
        {/* Handle */}
        <div className="w-10 h-1 bg-[#3A3A3C] rounded-full mx-auto mb-6" />
        
        <Typography variant="h3" className="text-[20px] font-semibold text-[#FAFAFA] mb-2">
          {pickup.title}
        </Typography>
        <Typography variant="body" className="text-[14px] text-[#8E8E93] mb-6">
          {pickup.address}
        </Typography>
        
        <div className="flex flex-col gap-4 mb-6">
          <div className="flex justify-between">
            <Typography className="text-[16px] text-[#FAFAFA]">Стоимость</Typography>
            <Typography className="text-[16px] text-[#8E8E93]">{pickup.price}</Typography>
          </div>
          <Separator className="bg-[#3A3A3C]" />
          <div className="flex justify-between">
            <Typography className="text-[16px] text-[#FAFAFA]">Доставка</Typography>
            <Typography className="text-[16px] text-[#8E8E93]">{pickup.deliveryDate}</Typography>
          </div>
          <Separator className="bg-[#3A3A3C]" />
          <div className="flex justify-between">
            <Typography className="text-[16px] text-[#FAFAFA]">Срок хранения</Typography>
            <Typography className="text-[16px] text-[#8E8E93]">{pickup.storageDays}</Typography>
          </div>
          <Separator className="bg-[#3A3A3C]" />
          <div className="flex justify-between">
            <Typography className="text-[16px] text-[#FAFAFA]">Ежедневно</Typography>
            <Typography className="text-[16px] text-[#8E8E93]">{pickup.workingHours}</Typography>
          </div>
        </div>
        
        <Button
          className="w-full h-14 rounded-3xl bg-[#E5E5E5] text-[#0A0A0A] font-semibold text-[16px]"
          onClick={onConfirm}
        >
          Заберу отсюда
        </Button>
      </Sheet.Content>
    </Sheet>
  );
}
```

> **Shadcn ref:** `Sheet`, `Button`, `Separator`

---

### 3.7 AddressSearchInput

Название: **AddressSearchInput**  
Описание: Поле поиска адреса с иконкой лупы и микрофона.

```typescript
interface AddressSearchInputProps {
  value: string;
  onChange: (value: string) => void;
  onVoiceClick?: () => void;
  placeholder?: string;
}
```

```tsx
import { Search, Mic } from "lucide-react";
import { Input, Button } from "@rollout/ui-kit";

export function AddressSearchInput({ value, onChange, onVoiceClick, placeholder = "Введите адрес" }: AddressSearchInputProps) {
  return (
    <div className="relative">
      <Search className="absolute left-4 top-1/2 -translate-y-1/2 size-5 text-[#636366]" />
      <Input
        value={value}
        onChange={(e) => onChange(e.target.value)}
        placeholder={placeholder}
        className="w-full h-14 pl-12 pr-12 bg-[#1C1C1E] border-[#3A3A3C] rounded-xl text-[16px] text-[#FAFAFA] placeholder:text-[#636366]"
      />
      <Button
        variant="ghost"
        size="icon"
        className="absolute right-2 top-1/2 -translate-y-1/2"
        onClick={onVoiceClick}
      >
        <Mic className="size-5 text-[#636366]" />
      </Button>
    </div>
  );
}
```

> **Shadcn ref:** `Input` + `Button` (variant="ghost")

---

### 3.8 AddressSuggestionList

Название: **AddressSuggestionList**  
Описание: Список подсказок адресов с иконкой MapPin.

```typescript
interface AddressSuggestionListProps {
  suggestions: { id: string; title: string; subtitle: string }[];
  onSelect: (id: string) => void;
}
```

```tsx
import { MapPin } from "lucide-react";
import { Typography } from "@rollout/ui-kit";

export function AddressSuggestionList({ suggestions, onSelect }: AddressSuggestionListProps) {
  return (
    <div className="flex flex-col">
      {suggestions.map((item) => (
        <button
          key={item.id}
          onClick={() => onSelect(item.id)}
          className="flex items-start gap-3 py-4 text-left hover:bg-[#3A3A3C]/30 transition-colors"
        >
          <MapPin className="size-5 text-[#8E8E93] mt-0.5 shrink-0" />
          <div className="flex flex-col gap-0.5">
            <Typography className="text-[16px] font-medium text-[#FAFAFA]">
              {item.title}
            </Typography>
            <Typography className="text-[14px] text-[#8E8E93]">
              {item.subtitle}
            </Typography>
          </div>
        </button>
      ))}
    </div>
  );
}
```

---

### 3.9 AddressFormSheet

Название: **AddressFormSheet**  
Описание: Bottom Sheet с формой ввода деталей адреса доставки (курьер).

```typescript
interface AddressFormSheetProps {
  isOpen: boolean;
  onClose: () => void;
  onSubmit: (data: AddressFormData) => void;
  initialData?: Partial<AddressFormData>;
}

interface AddressFormData {
  street: string;
  apartment: string;
  intercom: string;
  entrance: string;
  floor: string;
  comment: string;
}
```

```tsx
import { Sheet, Input, Button, Typography, Field } from "@rollout/ui-kit";
import { useState } from "react";

export function AddressFormSheet({ isOpen, onClose, onSubmit, initialData }: AddressFormSheetProps) {
  const [form, setForm] = useState<AddressFormData>({
    street: initialData?.street || "",
    apartment: initialData?.apartment || "",
    intercom: initialData?.intercom || "",
    entrance: initialData?.entrance || "",
    floor: initialData?.floor || "",
    comment: initialData?.comment || "",
  });

  const handleChange = (field: keyof AddressFormData, value: string) => {
    setForm((prev) => ({ ...prev, [field]: value }));
  };

  return (
    <Sheet open={isOpen} onOpenChange={onClose}>
      <Sheet.Content className="bg-[#1C1C1E] rounded-t-2xl p-6">
        <div className="w-10 h-1 bg-[#3A3A3C] rounded-full mx-auto mb-6" />
        
        <Typography variant="h3" className="text-[20px] font-semibold text-[#FAFAFA] mb-6">
          Адрес доставки
        </Typography>
        
        <div className="flex flex-col gap-5">
          <Field label="Улица и дом">
            <Input
              value={form.street}
              onChange={(e) => handleChange("street", e.target.value)}
              placeholder="Москва, улица Крылатские Холмы, 27к2"
              className="h-14 bg-[#1C1C1E] border-[#3A3A3C] rounded-xl text-[16px] text-[#FAFAFA] placeholder:text-[#636366]"
            />
          </Field>
          
          <div className="grid grid-cols-2 gap-4">
            <Field label="Квартира">
              <Input
                value={form.apartment}
                onChange={(e) => handleChange("apartment", e.target.value)}
                placeholder="Номер квартиры"
                className="h-14 bg-[#1C1C1E] border-[#3A3A3C] rounded-xl text-[16px] text-[#FAFAFA] placeholder:text-[#636366]"
              />
            </Field>
            <Field label="Домофон">
              <Input
                value={form.intercom}
                onChange={(e) => handleChange("intercom", e.target.value)}
                placeholder="Код домофона"
                className="h-14 bg-[#1C1C1E] border-[#3A3A3C] rounded-xl text-[16px] text-[#FAFAFA] placeholder:text-[#636366]"
              />
            </Field>
          </div>
          
          <div className="grid grid-cols-2 gap-4">
            <Field label="Подъезд">
              <Input
                value={form.entrance}
                onChange={(e) => handleChange("entrance", e.target.value)}
                placeholder="Подъезд"
                className="h-14 bg-[#1C1C1E] border-[#3A3A3C] rounded-xl text-[16px] text-[#FAFAFA] placeholder:text-[#636366]"
              />
            </Field>
            <Field label="Этаж">
              <Input
                value={form.floor}
                onChange={(e) => handleChange("floor", e.target.value)}
                placeholder="Этаж"
                className="h-14 bg-[#1C1C1E] border-[#3A3A3C] rounded-xl text-[16px] text-[#FAFAFA] placeholder:text-[#636366]"
              />
            </Field>
          </div>
          
          <Field label="Комментарий">
            <Input
              value={form.comment}
              onChange={(e) => handleChange("comment", e.target.value)}
              placeholder="Комментарий для курьера"
              className="h-14 bg-[#1C1C1E] border-[#3A3A3C] rounded-xl text-[16px] text-[#FAFAFA] placeholder:text-[#636366]"
            />
          </Field>
        </div>
        
        <Button
          className="w-full h-14 mt-6 rounded-3xl bg-[#E5E5E5] text-[#0A0A0A] font-semibold text-[16px]"
          onClick={() => onSubmit(form)}
        >
          Продолжить
        </Button>
      </Sheet.Content>
    </Sheet>
  );
}
```

> **Shadcn ref:** `Sheet`, `Input`, `Button`, `Field`

---

### 3.10 DeliveryInfoCard

Название: **DeliveryInfoCard**  
Описание: Карточка с информацией о выбранном способе доставки (иконка + текст + стрелка).

```typescript
interface DeliveryInfoCardProps {
  icon: React.ReactNode;
  title: string;
  subtitle: string;
  extra?: string;
  onClick?: () => void;
}
```

```tsx
import { ChevronRight } from "lucide-react";
import { Card, Typography } from "@rollout/ui-kit";

export function DeliveryInfoCard({ icon, title, subtitle, extra, onClick }: DeliveryInfoCardProps) {
  return (
    <Card
      className="bg-[#1C1C1E] border-[#3A3A3C] rounded-2xl p-4 flex items-center gap-4 cursor-pointer"
      onClick={onClick}
    >
      <div className="text-[#FAFAFA]">{icon}</div>
      <div className="flex-1 flex flex-col gap-0.5">
        <Typography className="text-[16px] font-semibold text-[#FAFAFA]">
          {title}
        </Typography>
        <Typography className="text-[14px] text-[#8E8E93]">
          {subtitle}
        </Typography>
        {extra && (
          <Typography className="text-[14px] text-[#8E8E93]">
            {extra}
          </Typography>
        )}
      </div>
      <ChevronRight className="size-5 text-[#8E8E93]" />
    </Card>
  );
}
```

> **Shadcn ref:** `Card`

---

### 3.11 DateSelector

Название: **DateSelector**  
Описание: Горизонтальный скролл чипов с выбором даты доставки.

```typescript
interface DateSelectorProps {
  options: { id: string; label: string }[];
  selectedId: string;
  onSelect: (id: string) => void;
}
```

```tsx
import { cn } from "@rollout/ui-kit";

export function DateSelector({ options, selectedId, onSelect }: DateSelectorProps) {
  return (
    <div className="flex gap-2 overflow-x-auto scrollbar-hide pb-1">
      {options.map((option) => (
        <button
          key={option.id}
          onClick={() => onSelect(option.id)}
          className={cn(
            "shrink-0 px-5 py-3 rounded-2xl text-[16px] font-medium border transition-colors",
            selectedId === option.id
              ? "bg-[#3A3A3C] border-[#3A3A3C] text-[#FAFAFA]"
              : "bg-[#1C1C1E] border-[#3A3A3C] text-[#FAFAFA]"
          )}
        >
          {option.label}
        </button>
      ))}
    </div>
  );
}
```

> **Shadcn ref:** `Badge` (custom chip variant)

---

### 3.12 TimeSlotCard

Название: **TimeSlotCard**  
Описание: Радио-карточка для выбора временного слота доставки.

```typescript
interface TimeSlotCardProps {
  price: string;
  timeRange: string;
  selected: boolean;
  onSelect: () => void;
}
```

```tsx
import { cn } from "@rollout/ui-kit";

export function TimeSlotCard({ price, timeRange, selected, onSelect }: TimeSlotCardProps) {
  return (
    <button
      onClick={onSelect}
      className={cn(
        "flex items-center gap-3 p-4 rounded-2xl border transition-colors min-w-[160px]",
        selected
          ? "bg-[#1C1C1E] border-[#FAFAFA]"
          : "bg-[#1C1C1E] border-[#3A3A3C]"
      )}
    >
      <div className={cn(
        "w-5 h-5 rounded-full border-2 flex items-center justify-center",
        selected ? "border-[#FAFAFA]" : "border-[#3A3A3C]"
      )}>
        {selected && <div className="w-2.5 h-2.5 rounded-full bg-[#FAFAFA]" />}
      </div>
      <div className="flex flex-col gap-1">
        <span className="text-[16px] font-semibold text-[#FAFAFA]">{price}</span>
        <span className="text-[14px] text-[#8E8E93]">{timeRange}</span>
      </div>
    </button>
  );
}
```

> **Shadcn ref:** `RadioGroup` (custom card variant)

---

### 3.13 Footer

Название: **Footer**  
Описание: Футер с копирайтом и социальными иконками.

```tsx
import { X, Linkedin, Send } from "lucide-react";
import { Typography } from "@rollout/ui-kit";

export function Footer() {
  return (
    <footer className="flex items-center justify-between py-4 mt-auto">
      <Typography className="text-[14px] text-[#8E8E93]">
        © 2025 Rollout
      </Typography>
      <div className="flex items-center gap-4">
        <a href="#" className="text-[#8E8E93] hover:text-[#FAFAFA] transition-colors">
          <X className="size-5" />
        </a>
        <a href="#" className="text-[#8E8E93] hover:text-[#FAFAFA] transition-colors">
          <Linkedin className="size-5" />
        </a>
        <a href="#" className="text-[#8E8E93] hover:text-[#FAFAFA] transition-colors">
          <Send className="size-5" />
        </a>
      </div>
    </footer>
  );
}
```

---

## 4. Структура каждого экрана

### 4.1 ChoosingPointPage (delivery/choosing-point)

**Скриншот:** `choosing_point.png` / `delivery_method_1.png`

**Layout:**
- Контейнер: `mx-auto flex max-w-[576px] flex-col gap-7 pt-20 pb-8`
- Фон: `#0A0A0A`
- Карта занимает всю ширину контейнера, aspect-ratio 3:4

**Компоненты сверху вниз:**
1. `PageHeader` (title="Доставка еды", subtitle="Укажите адрес и способ получения")
2. `DeliveryTabs` (active: "pickup")
3. `FilterChips` (Постаматы, СДЭК, Почта, OZON, Яндекс...)
4. `DeliveryMap` (mode="pickup", markers с номерами)
5. `Footer`

**Состояния:**
- `loading` — скелетон карты и чипов
- `error` — тост/alert с ошибкой загрузки пунктов
- `empty` — карта с центром по умолчанию, кнопка "Показать ближайшие пункты"
- `success` — карта с метками, чипы активны

**Интерактивность:**
- `onTabChange` → навигация на `/delivery/courier`
- `onFilterChange` → фильтрация меток на карте
- `onMarkerClick` → открытие `PickupDetailSheet`
- `onLocateClick` → геолокация пользователя
- `onBack` → навигация назад

---

### 4.2 DeliveryMethodCourierPage (delivery/courier)

**Скриншот:** `delivery_method_8-11.png`

**Layout:**
- Контейнер: `mx-auto flex max-w-[576px] flex-col gap-7 pt-20 pb-8`
- Фон: `#0A0A0A`

**Компоненты сверху вниз:**
1. `PageHeader` (title="Доставка еды", subtitle="Укажите адрес и способ получения")
2. `DeliveryTabs` (active: "courier")
3. `FilterChips` (Дом, Работа, Осенний бульвар, 4...)
4. `DeliveryMap` (mode="courier", оранжевая метка)
5. `Footer`

**Состояния:**
- `loading` — скелетон
- `error` — alert
- `empty` — карта с кнопкой "Показать ближайшие пункты"
- `success` — карта с оранжевой меткой адреса

**Интерактивность:**
- `onTabChange` → `/delivery/choosing-point`
- `onAddressChipClick` → открытие `AddressFormSheet` с предзаполненными данными
- `onMapClick` → открытие `AddressSearchSheet`
- `onLocateClick` → геолокация

---

### 4.3 PickupListSheet

**Скриншот:** `delivery_method_4.png`

**Layout:**
- Bottom Sheet, `rounded-t-2xl`, `bg-[#1C1C1E]`
- Внутри: `p-6`, gap между элементами

**Компоненты:**
1. Handle bar (сверху)
2. Title: "Выберите пункт выдачи" (20px semibold)
3. Список `PickupListItem`

**Состояния:**
- `loading` — скелетон-строки (3 шт.)
- `empty` — "Пункты выдачи не найдены"
- `success` — список с данными

**Интерактивность:**
- `onItemClick` → открытие `PickupDetailSheet`
- `onClose` → закрытие шторки

---

### 4.4 PickupDetailSheet

**Скриншот:** `delivery_method_5.png`

**Layout:**
- Bottom Sheet, `rounded-t-2xl`, `bg-[#1C1C1E]`, `p-6`

**Компоненты:**
1. Handle bar
2. Title + Address
3. Row: "Стоимость" / "300₽"
4. Separator
5. Row: "Доставка" / "19 июня"
6. Separator
7. Row: "Срок хранения" / "7 дней"
8. Separator
9. Row: "Ежедневно" / "10:00–20:00"
10. Button "Заберу отсюда"

**Состояния:**
- `success` — данные загружены

**Интерактивность:**
- `onConfirm` → сохранение пункта, переход к `OrderCheckoutPage`
- `onClose` → закрытие

---

### 4.5 DeliveryEmptySheet

**Скриншот:** `delivery_method_6.png`

**Layout:**
- Bottom Sheet с заголовком и кнопкой

**Компоненты:**
1. Handle bar
2. Title: "Куда доставить?"
3. Subtitle: "Выберите пункт выдачи заказа на карте"
4. Button: "Показать ближайшие пункты"

**Интерактивность:**
- `onButtonClick` → геолокация + показ ближайших пунктов

---

### 4.6 AddressSearchSheet

**Скриншот:** `delivery_method_7.png`, `delivery_method_12.png`

**Layout:**
- Bottom Sheet, `rounded-t-2xl`, `bg-[#1C1C1E]`, `p-6`

**Компоненты:**
1. Handle bar
2. Title: "Адрес" или "Куда доставить?"
3. `AddressSearchInput` (с Search и Mic)
4. `AddressSuggestionList` (список подсказок)
5. Button "Показать ближайшие пункты"

**Состояния:**
- `empty` — поле пустое, нет подсказок
- `typing` — поле заполнено, подсказки загружаются
- `success` — список подсказок отображается

**Интерактивность:**
- `onInputChange` → debounce запрос на геокодинг
- `onSuggestionSelect` → заполнение адреса, переход к `AddressFormSheet`
- `onVoiceClick` — placeholder (mock)

---

### 4.7 AddressFormSheet

**Скриншот:** `delivery_method_13.png`, `delivery_method_14.png`

**Layout:**
- Bottom Sheet, `rounded-t-2xl`, `bg-[#1C1C1E]`, `p-6`
- Scrollable content (если не влезает)

**Компоненты:**
1. Handle bar
2. Title: "Адрес доставки"
3. Field: "Улица и дом" (Input, full width)
4. Grid 2 cols: "Квартира" / "Домофон"
5. Grid 2 cols: "Подъезд" / "Этаж"
6. Field: "Комментарий" (Input, full width)
7. Button "Продолжить"

**Состояния:**
- `empty` — все поля пустые
- `partial` — часть полей заполнена
- `valid` — улица заполнена (минимум для кнопки)
- `submitting` — кнопка в loading state

**Интерактивность:**
- `onSubmit` → валидация, сохранение адреса, переход к `OrderCheckoutPage`
- `onClose` → закрытие шторки

---

### 4.8 OrderCheckoutPage (delivery/order)

**Скриншот:** `making_an_order.png`

**Layout:**
- Контейнер: `mx-auto flex max-w-[576px] flex-col gap-7 pt-20 pb-8`
- Фон: `#0A0A0A`
- Структура: вертикальная, gap-7 между секциями

**Компоненты сверху вниз:**
1. `PageHeader` (title="Оформление заказа", subtitle не нужен)
2. `DeliveryTabs` (active: текущий выбор)
3. `DeliveryInfoCard` (icon=MapPin, title="Постамат", subtitle=address, extra="Срок хранения заказа – 7 дней")
4. `DeliveryInfoCard` (icon=User, title="Марина Чернявцева", subtitle="+79265753505")
5. Section "Когда доставить":
   - `DateSelector` (Сегодня, Завтра, Послезавтра, до 3 дней)
6. Section "Временные слоты":
   - `TimeSlotCard` (300₽, 10:00–12:00)
   - `TimeSlotCard` (150₽, 12:00–14:00)
   - Горизонтальный скролл
7. Button "Оформить заказ" (Primary, full width)
8. `Footer`

**Состояния:**
- `loading` — скелетон карточек и кнопок
- `error` — alert с ошибкой загрузки данных
- `empty` — нет выбранного способа доставки (редирект на ChoosingPoint)
- `success` — все данные загружены, форма активна
- `submitting` — кнопка "Оформить заказ" в loading

**Интерактивность:**
- `onTabChange` — переключение способа (редирект на соответствующий экран)
- `onDeliveryCardClick` — редактирование адреса/пункта
- `onDateSelect` — обновление доступных слотов
- `onTimeSlotSelect` — выбор времени
- `onSubmit` — отправка заказа, редирект на success page
- `onBack` — навигация назад

---

## 5. Пример полной страницы

### OrderCheckoutPage.tsx

```tsx
import { useState } from "react";
import { useNavigate } from "react-router-dom";
import { MapPin, User } from "lucide-react";
import {
  Button,
  Card,
  Separator,
  Typography,
} from "@rollout/ui-kit";
import {
  PageHeader,
  DeliveryTabs,
  DeliveryInfoCard,
  DateSelector,
  TimeSlotCard,
  Footer,
} from "@/components/delivery";

interface TimeSlot {
  id: string;
  price: string;
  timeRange: string;
}

export function OrderCheckoutPage() {
  const navigate = useNavigate();
  const [activeTab, setActiveTab] = useState<"pickup" | "courier">("pickup");
  const [selectedDate, setSelectedDate] = useState("today");
  const [selectedSlot, setSelectedSlot] = useState<string | null>(null);
  const [isSubmitting, setIsSubmitting] = useState(false);

  const dateOptions = [
    { id: "today", label: "Сегодня" },
    { id: "tomorrow", label: "Завтра" },
    { id: "after_tomorrow", label: "Послезавтра" },
    { id: "3days", label: "до 3 дней" },
  ];

  const timeSlots: TimeSlot[] = [
    { id: "slot1", price: "300 ₽", timeRange: "10:00 – 12:00" },
    { id: "slot2", price: "150 ₽", timeRange: "12:00 – 14:00" },
  ];

  const handleTabChange = (tab: "pickup" | "courier") => {
    setActiveTab(tab);
    if (tab === "courier") {
      navigate("/delivery/courier");
    }
  };

  const handleSubmit = () => {
    if (!selectedSlot) return;
    setIsSubmitting(true);
    // Mock submit
    setTimeout(() => {
      setIsSubmitting(false);
      navigate("/order/success");
    }, 1500);
  };

  const handleEditDelivery = () => {
    navigate("/delivery/choosing-point");
  };

  const handleEditRecipient = () => {
    // Mock edit recipient
  };

  return (
    <div className="mx-auto flex max-w-[576px] flex-col gap-7 pt-20 pb-8 px-4 min-h-screen bg-[#0A0A0A]">
      <PageHeader
        title="Оформление заказа"
        onBack={() => navigate(-1)}
      />

      <DeliveryTabs activeTab={activeTab} onTabChange={handleTabChange} />

      <DeliveryInfoCard
        icon={<MapPin className="size-6" />}
        title="Постамат"
        subtitle="Москва, Крылатские холмы, 43, магазин «Пятёрочка»"
        extra="Срок хранения заказа – 7 дней"
        onClick={handleEditDelivery}
      />

      <DeliveryInfoCard
        icon={<User className="size-6" />}
        title="Марина Чернявцева"
        subtitle="+79265753505"
        onClick={handleEditRecipient}
      />

      <div className="flex flex-col gap-3">
        <Typography className="text-[16px] font-semibold text-[#FAFAFA]">
          Когда доставить
        </Typography>
        <DateSelector
          options={dateOptions}
          selectedId={selectedDate}
          onSelect={setSelectedDate}
        />
      </div>

      <div className="flex flex-col gap-3">
        <div className="flex gap-3 overflow-x-auto scrollbar-hide pb-1">
          {timeSlots.map((slot) => (
            <TimeSlotCard
              key={slot.id}
              price={slot.price}
              timeRange={slot.timeRange}
              selected={selectedSlot === slot.id}
              onSelect={() => setSelectedSlot(slot.id)}
            />
          ))}
        </div>
      </div>

      <Button
        className="w-full h-14 rounded-3xl bg-[#E5E5E5] text-[#0A0A0A] font-semibold text-[16px] disabled:opacity-50"
        disabled={!selectedSlot || isSubmitting}
        onClick={handleSubmit}
      >
        {isSubmitting ? "Оформление..." : "Оформить заказ"}
      </Button>

      <Footer />
    </div>
  );
}
```

---

## 6. Чек-лист вёрстки

- [ ] **Все цвета через токены** — не хардкод, использовать CSS-переменные или константы (`#0A0A0A`, `#1C1C1E`, `#3A3A3C`, `#FAFAFA`, `#8E8E93`, `#636366`, `#E5E5E5`)
- [ ] **Типографика совпадает с макетом** — размеры 28/20/18/16/14/13px, веса 700/600/500/400, line-height 1.2–1.5
- [ ] **Отступы и радиусы по дизайн-системе** — контейнер `max-w-[576px]`, gap-7 (28px), карточки rounded-2xl (16px), кнопки rounded-3xl (24px), input rounded-xl (12px)
- [ ] **Иконки из lucide-react** — ArrowLeft, MapPin, ChevronRight, Navigation, Search, Mic, User, X, Linkedin, Send
- [ ] **Контейнер `max-w-[576px] mx-auto`** — на всех страницах модуля
- [ ] **Отступ от шапки `pt-20`** — для всех страниц
- [ ] **Мобильный отступ `pb-tabbar md:pb-0`** — для нижнего таббара на мобильных
- [ ] **Все формы на локальном `useState`** — AddressFormSheet, DateSelector, TimeSlotCard
- [ ] **Mock-обработчики без `console.log`** — использовать `navigate`, `setState`, `setTimeout` для имитации API
- [ ] **Route добавлен в App.tsx** — `/delivery/choosing-point`, `/delivery/courier`, `/delivery/order`
- [ ] **Скриншот соответствует макету** — визуальный diff по цветам, отступам, шрифтам, иконкам
- [ ] **Bottom Sheet handle** — серый бар `w-10 h-1 bg-[#3A3A3C] rounded-full mx-auto mb-6`
- [ ] **Separator** — `bg-[#3A3A3C]` между строками в деталях пункта
- [ ] **Map placeholder** — в MVP использовать div с `[Map Placeholder]`, в реализации — интеграция с Map API
- [ ] **Footer** — © 2025 Rollout + X, LinkedIn, Telegram иконки, цвет `#8E8E93`
- [ ] **Radio-карточки** — внутренняя точка `w-2.5 h-2.5 rounded-full bg-[#FAFAFA]`, бордер `border-[#FAFAFA]` при выбранном
- [ ] **No deep imports** — импортировать всё из `@rollout/ui-kit` (не `@rollout/ui-kit/button`)
- [ ] **No radix-ui** — не использовать radix напрямую, всё через @rollout/ui-kit
- [ ] **No shadcn add** — не использовать CLI shadcn для добавления компонентов
- [ ] **Geist Variable** — шрифт подключён глобально в проекте
