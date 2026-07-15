# Инструкция по вёрстке модуля 5-ItemCard

> **Файл:** `/Users/mokoloskov/Documents/kimi/workspace/5-ItemCard-instruction.md`  
> **Дата:** 2025-07-13  
> **Figma-файл:** `5. ItemCard` (`eiItTzL0Q2QFL5S8XQXFui`)

---

## 1. Обзор модуля

### Назначение
Модуль **ItemCard** отвечает за отображение карточки товара/услуги, систему рейтингов и отзывов, выбор вариантов (размер, цвет), а также за форму добавления отзыва. Является ключевым модулем e-commerce части приложения.

### Экраны модуля (страницы Figma)

| # | Страница Figma | Описание | Наличие скриншота |
|---|----------------|----------|-------------------|
| 1 | **Cover** | Обложка модуля | ✅ |
| 2 | **Common** | Общие компоненты (категории, рекомендации, вопросы, отзывы, список) | ✅ |
| 3 | **Crads** | Заглушка для карточек | ❌ (пустая) |
| 4 | **↪ ElectronicsCard** | Карточка электроники | ❌ (пустая) |
| 5 | **↪ ClothCard** | Карточка одежды | ❌ (пустая) |
| 6 | **↪ CosmeticsCard** | Карточка косметики (селектор размера/цвета) | ✅ |
| 7 | **↪ HouseCard** | Карточка товаров для дома (селектор цвета) | ✅ |
| 8 | **↪ TravelCard** | Карточка путешествий | ❌ (пустая) |
| 9 | **↪ FoodCard** | Карточка еды | ❌ (пустая) |
| 10 | **↪ RestaurantCard** | Карточка ресторана | ❌ (пустая) |
| 11 | **↪ AutoCard** | Карточка авто | ❌ (пустая) |
| 12 | **↪ RealEstateCard** | Карточка недвижимости | ❌ (пустая) |
| 13 | **↪ ServicesCard** | Карточка услуг | ❌ (пустая) |
| 14 | **↪ JobsCard** | Карточка вакансий | ❌ (пустая) |
| 15 | **↪ FlowerCard** | Карточка цветов (селектор цвета) | ✅ |
| 16 | **Comparing** | Сравнение товаров | ❌ (пустая) |
| 17 | **Feedback → Rating and reviews** | Рейтинг и список отзывов | ✅ |
| 18 | **Feedback → Experience** | Форма написания отзыва | ✅ |
| 19 | **Feedback → Sorting** | Сортировка отзывов | ✅ |

### Роуты (URL-пути)

```tsx
// Предполагаемые роуты на основе анализа структуры
const routes = [
  { path: '/item/:id', element: <ItemCardPage /> },           // Основная карточка товара
  { path: '/item/:id/reviews', element: <RatingAndReviewsPage /> }, // Рейтинг и отзывы
  { path: '/item/:id/reviews/new', element: <WriteReviewPage /> },  // Написать отзыв
  { path: '/item/:id/reviews/sort', element: <ReviewSortingPage /> }, // Сортировка отзывов
];
```

---

## 2. Дизайн-система (из макетов)

### 2.1 Цвета

```tsx
// tailwind.config.ts — extend theme
colors: {
  background: '#0A0A0A',        // Фон страницы
  card: '#1C1C1E',              // Фон карточек, инпутов
  border: '#3A3A3C',            // Бордеры карточек, инпутов
  'border-subtle': '#2C2C2E',   // Тонкие разделители
  foreground: '#FAFAFA',        // Основной текст
  'foreground-muted': '#8E8E93', // Вторичный текст (даты, плейсхолдеры)
  'foreground-subtle': '#636366', // Плейсхолдеры инпутов
  primary: '#E5E5E5',           // Primary кнопка (bg), заполненные звёзды
  'primary-foreground': '#0A0A0A', // Текст на primary кнопке
  secondary: 'transparent',     // Secondary кнопка — прозрачный + border
  'secondary-foreground': '#FAFAFA',
  muted: '#3A3A3C',             // Фон бейджей, неактивных элементов
  'muted-foreground': '#8E8E93',
  accent: '#3A3A3C',            // Акцентный фон (hover, active tab)
  destructive: '#FF3B30',       // Красный для ошибок/жалоб
  ring: '#3A3A3C',              // Фокус-ринг
}
```

| Элемент | Значение |
|---------|----------|
| Фон страницы | `#0A0A0A` |
| Фон карточек / инпутов | `#1C1C1E` |
| Бордер карточек / инпутов | `#3A3A3C` |
| Основной текст | `#FAFAFA` |
| Вторичный текст (даты, метки) | `#8E8E93` |
| Плейсхолдер | `#636366` |
| Primary кнопка bg | `#E5E5E5` |
| Primary кнопка текст | `#0A0A0A` |
| Secondary кнопка bg | `transparent` |
| Secondary кнопка border | `#3A3A3C` |
| Звёзды заполненные | `#E5E5E5` |
| Звёзды пустые / бордер | `#3A3A3C` |
| Прогресс-бар track | `#3A3A3C` |
| Прогресс-бар fill | `#E5E5E5` |
| Аватар бордер | `#3A3A3C` |
| Меню/поповер bg | `#1C1C1E` |
| Меню/поповер border | `#3A3A3C` |

### 2.2 Типографика

| Элемент | Размер | Вес | Line-height | Цвет |
|---------|--------|-----|-------------|------|
| Заголовок страницы (H1) | `text-2xl` (24px) | 600 | 1.2 | `#FAFAFA` |
| Заголовок секции (H2) | `text-lg` (18px) | 600 | 1.3 | `#FAFAFA` |
| Рейтинг (число) | `text-5xl` (48px) | 700 | 1.0 | `#FAFAFA` |
| Body | `text-base` (16px) | 400 | 1.5 | `#FAFAFA` |
| Body secondary | `text-sm` (14px) | 400 | 1.5 | `#8E8E93` |
| Caption | `text-xs` (12px) | 500 | 1.4 | `#8E8E93` |
| Button | `text-base` (16px) | 500 | 1.0 | `#0A0A0A` / `#FAFAFA` |
| Label | `text-sm` (14px) | 500 | 1.4 | `#FAFAFA` |
| Badge | `text-xs` (12px) | 500 | 1.0 | `#FAFAFA` |
| Placeholder | `text-base` (16px) | 400 | 1.5 | `#636366` |

> Шрифт: `Geist Variable` (уже подключён глобально через `font-sans`)

### 2.3 Спейсинг

| Элемент | Значение |
|---------|----------|
| Контейнер max-width | `max-w-[576px]` |
| Контейнер margin | `mx-auto` |
| Page padding top (под шапку) | `pt-20` |
| Page padding bottom | `pb-8` (desktop) / `pb-tabbar` (mobile) |
| Page horizontal padding | `px-4` |
| Gap между секциями | `gap-7` (28px) |
| Gap внутри карточек | `gap-4` (16px) |
| Card padding | `p-4` (16px) |
| Card border-radius | `rounded-2xl` (16px) |
| Button border-radius | `rounded-xl` (12px) |
| Input border-radius | `rounded-xl` (12px) |
| Avatar border-radius | `rounded-full` |
| Image thumbnail radius | `rounded-xl` (12px) |
| Badge border-radius | `rounded-full` |
| Popover border-radius | `rounded-xl` (12px) |
| Bottom sheet radius (top) | `rounded-t-2xl` (16px) |
| Segmented control radius | `rounded-xl` (12px) |

### 2.4 Иконки

- **Библиотека:** `lucide-react`
- **Размеры:**
  - Навигация (стрелка, поиск, сортировка): `size-6` (24px)
  - Интерактивные (лайк, дизлайк, комментарий, меню): `size-5` (20px)
  - Звёзды рейтинга: `size-6` (24px) — кастомные или `Star` / `StarOff`
  - Соц. иконки (футер): `size-5` (20px)
- **Цвет:** `#FAFAFA` (основной), `#8E8E93` (неактивные)

---

## 3. Компоненты UI

### 3.1 StarRating

**Описание:** Компонент отображения рейтинга в виде 5 звёзд. Поддерживает режимы просмотра (read-only) и интерактивный (выбор оценки).

**Props-интерфейс:**

```tsx
interface StarRatingProps {
  value: number;           // Текущее значение (0–5)
  max?: number;            // Максимум звёзд (default: 5)
  interactive?: boolean;   // Можно ли кликать (default: false)
  size?: 'sm' | 'md' | 'lg'; // Размер (default: 'md')
  onChange?: (value: number) => void;
}
```

**Shadcn-референс:** нет прямого аналога — кастомный компонент.

**Реализация через @rollout/ui-kit:**

```tsx
import { useState } from 'react';
import { Star } from 'lucide-react';
import { cn } from '@rollout/ui-kit';

export function StarRating({
  value,
  max = 5,
  interactive = false,
  size = 'md',
  onChange,
}: StarRatingProps) {
  const [hoverValue, setHoverValue] = useState(0);

  const sizeClasses = {
    sm: 'size-4',
    md: 'size-6',
    lg: 'size-8',
  };

  return (
    <div className="flex items-center gap-1">
      {Array.from({ length: max }, (_, i) => {
        const starValue = i + 1;
        const isFilled = interactive
          ? starValue <= (hoverValue || value)
          : starValue <= value;

        return (
          <button
            key={i}
            type="button"
            disabled={!interactive}
            className={cn(
              'transition-colors',
              interactive && 'cursor-pointer hover:scale-110'
            )}
            onMouseEnter={() => interactive && setHoverValue(starValue)}
            onMouseLeave={() => interactive && setHoverValue(0)}
            onClick={() => interactive && onChange?.(starValue)}
          >
            <Star
              className={cn(
                sizeClasses[size],
                isFilled ? 'fill-[#E5E5E5] text-[#E5E5E5]' : 'fill-transparent text-[#3A3A3C]'
              )}
            />
          </button>
        );
      })}
    </div>
  );
}
```

---

### 3.2 RatingBreakdown

**Описание:** Блок с числовым рейтингом, количеством оценок и горизонтальными прогресс-барами распределения по звёздам.

**Props-интерфейс:**

```tsx
interface RatingBreakdownProps {
  rating: number;          // Средний рейтинг (например, 4.9)
  totalReviews: number;    // Всего оценок
  distribution: {          // Распределение по звёздам (5 → 1)
    stars: number;
    percentage: number;
  }[];
}
```

**Shadcn-референс:** `Progress` — используем как референс для прогресс-баров.

**Реализация:**

```tsx
import { Progress } from '@rollout/ui-kit';

export function RatingBreakdown({ rating, totalReviews, distribution }: RatingBreakdownProps) {
  return (
    <div className="flex flex-col gap-4">
      {/* Заголовок рейтинга */}
      <div className="flex items-end gap-4">
        <span className="text-5xl font-bold text-[#FAFAFA]">{rating}</span>
        <div className="flex flex-col gap-1 pb-1">
          <StarRating value={Math.round(rating)} />
          <span className="text-sm text-[#8E8E93]">{totalReviews} оценок</span>
        </div>
      </div>

      {/* Прогресс-бары */}
      <div className="flex flex-col gap-2">
        {distribution.map((item) => (
          <div key={item.stars} className="flex items-center gap-3">
            <Progress
              value={item.percentage}
              className="h-2 flex-1 rounded-full bg-[#3A3A3C]"
            />
            <span className="w-10 text-right text-sm text-[#8E8E93]">
              {item.percentage}%
            </span>
          </div>
        ))}
      </div>
    </div>
  );
}
```

---

### 3.3 ReviewCard

**Описание:** Карточка отдельного отзыва с аватаром, именем, датой, бейджем, фото, текстом и кнопками взаимодействия.

**Props-интерфейс:**

```tsx
interface ReviewCardProps {
  review: {
    id: string;
    author: {
      name: string;
      avatar?: string;
    };
    date: string;
    rating: number;
    isVerifiedBuyer: boolean;
    images?: string[];
    text: string;
    likes: number;
    dislikes: number;
    comments: number;
  };
  onLike?: (id: string) => void;
  onDislike?: (id: string) => void;
  onComment?: (id: string) => void;
  onMenu?: (id: string) => void;
  onReadMore?: (id: string) => void;
}
```

**Shadcn-референс:** `Card` — как контейнер.

**Реализация:**

```tsx
import { Card, Avatar, Badge, Separator } from '@rollout/ui-kit';
import { ThumbsUp, ThumbsDown, MessageCircle, MoreHorizontal } from 'lucide-react';
import { useState } from 'react';

export function ReviewCard({ review, onLike, onDislike, onComment, onMenu, onReadMore }: ReviewCardProps) {
  const [isExpanded, setIsExpanded] = useState(false);
  const maxLength = 200;
  const isLong = review.text.length > maxLength;
  const displayText = isExpanded || !isLong ? review.text : review.text.slice(0, maxLength) + '...';

  return (
    <div className="flex flex-col gap-4 py-4">
      {/* Шапка отзыва */}
      <div className="flex items-start gap-3">
        <Avatar className="size-10">
          <Avatar.Image src={review.author.avatar} alt={review.author.name} />
          <Avatar.Fallback className="bg-[#3A3A3C] text-[#FAFAFA]">
            {review.author.name.slice(0, 2).toUpperCase()}
          </Avatar.Fallback>
        </Avatar>

        <div className="flex flex-col gap-1">
          <span className="text-base font-medium text-[#FAFAFA]">{review.author.name}</span>
          <span className="text-sm text-[#8E8E93]">{review.date}</span>
          {review.isVerifiedBuyer && (
            <Badge variant="secondary" className="w-fit gap-1 bg-[#3A3A3C] text-[#FAFAFA]">
              <Check className="size-3" />
              проверенный покупатель
            </Badge>
          )}
        </div>
      </div>

      {/* Звёзды */}
      <StarRating value={review.rating} size="sm" />

      {/* Фото */}
      {review.images && review.images.length > 0 && (
        <div className="flex gap-2 overflow-x-auto">
          {review.images.map((img, idx) => (
            <img
              key={idx}
              src={img}
              alt={`Review image ${idx + 1}`}
              className="size-20 rounded-xl object-cover"
            />
          ))}
        </div>
      )}

      {/* Текст */}
      <div className="flex flex-col gap-1">
        <p className="text-base leading-relaxed text-[#FAFAFA]">{displayText}</p>
        {isLong && (
          <button
            onClick={() => setIsExpanded(!isExpanded)}
            className="self-end text-sm text-[#8E8E93] hover:text-[#FAFAFA]"
          >
            {isExpanded ? 'Свернуть' : 'Читать ещё'}
          </button>
        )}
      </div>

      {/* Кнопки */}
      <div className="flex items-center justify-between">
        <div className="flex items-center gap-2">
          <button
            onClick={() => onLike?.(review.id)}
            className="flex items-center gap-1.5 rounded-lg bg-[#1C1C1E] px-3 py-2 text-sm text-[#FAFAFA] hover:bg-[#2C2C2E]"
          >
            <ThumbsUp className="size-5" />
            {review.likes}
          </button>
          <button
            onClick={() => onDislike?.(review.id)}
            className="flex items-center gap-1.5 rounded-lg bg-[#1C1C1E] px-3 py-2 text-sm text-[#FAFAFA] hover:bg-[#2C2C2E]"
          >
            <ThumbsDown className="size-5" />
            {review.dislikes}
          </button>
        </div>

        <div className="flex items-center gap-2">
          <button
            onClick={() => onComment?.(review.id)}
            className="flex items-center gap-1.5 rounded-lg bg-[#1C1C1E] px-3 py-2 text-sm text-[#FAFAFA] hover:bg-[#2C2C2E]"
          >
            <MessageCircle className="size-5" />
            {review.comments}
          </button>
          <button
            onClick={() => onMenu?.(review.id)}
            className="rounded-lg bg-[#1C1C1E] p-2 text-[#FAFAFA] hover:bg-[#2C2C2E]"
          >
            <MoreHorizontal className="size-5" />
          </button>
        </div>
      </div>

      <Separator className="bg-[#3A3A3C]" />
    </div>
  );
}
```

---

### 3.4 ReviewMenuPopover

**Описание:** Поповер с действиями для отзыва (пожаловаться, скрыть, заблокировать).

**Props-интерфейс:**

```tsx
interface ReviewMenuPopoverProps {
  reviewId: string;
  isOpen: boolean;
  onClose: () => void;
  onReportReview?: (id: string) => void;
  onHideReview?: (id: string) => void;
  onReportUser?: (id: string) => void;
  onBlockUser?: (id: string) => void;
}
```

**Shadcn-референс:** `Popover` / `DropdownMenu`

**Реализация через @rollout/ui-kit:**

```tsx
import { Popover, PopoverTrigger, PopoverContent } from '@rollout/ui-kit';

export function ReviewMenuPopover({
  reviewId,
  isOpen,
  onClose,
  onReportReview,
  onHideReview,
  onReportUser,
  onBlockUser,
}: ReviewMenuPopoverProps) {
  const items = [
    { label: 'Пожаловаться на отзыв', action: onReportReview },
    { label: 'Скрыть отзыв', action: onHideReview },
    { label: 'Пожаловаться на пользователя', action: onReportUser },
    { label: 'Заблокировать пользователя', action: onBlockUser },
  ];

  return (
    <Popover open={isOpen} onOpenChange={(open) => !open && onClose()}>
      <PopoverContent
        className="w-64 border-[#3A3A3C] bg-[#1C1C1E] p-2"
        align="end"
      >
        <div className="flex flex-col">
          {items.map((item, idx) => (
            <button
              key={idx}
              onClick={() => {
                item.action?.(reviewId);
                onClose();
              }}
              className="rounded-lg px-3 py-2.5 text-left text-sm text-[#FAFAFA] hover:bg-[#2C2C2E]"
            >
              {item.label}
            </button>
          ))}
        </div>
      </PopoverContent>
    </Popover>
  );
}
```

---

### 3.5 SegmentedControl

**Описание:** Сегментированный переключатель (Tabs) для выбора вариантов: размеры, цвета и т.д.

**Props-интерфейс:**

```tsx
interface SegmentedControlProps<T extends string> {
  options: { value: T; label: string; icon?: React.ReactNode }[];
  value: T;
  onChange: (value: T) => void;
}
```

**Shadcn-референс:** `Tabs`

**Реализация через @rollout/ui-kit:**

```tsx
import { Tabs, TabsList, TabsTrigger } from '@rollout/ui-kit';

export function SegmentedControl<T extends string>({
  options,
  value,
  onChange,
}: SegmentedControlProps<T>) {
  return (
    <Tabs value={value} onValueChange={(v) => onChange(v as T)}>
      <TabsList className="h-auto w-full gap-1 rounded-xl bg-[#1C1C1E] p-1">
        {options.map((option) => (
          <TabsTrigger
            key={option.value}
            value={option.value}
            className="flex-1 rounded-lg px-3 py-2.5 text-sm font-medium text-[#8E8E93] data-[state=active]:bg-[#3A3A3C] data-[state=active]:text-[#FAFAFA]"
          >
            <span className="flex items-center gap-2">
              {option.icon}
              {option.label}
            </span>
          </TabsTrigger>
        ))}
      </TabsList>
    </Tabs>
  );
}
```

---

### 3.6 ColorSelector

**Описание:** Селектор цвета в виде пилюлек с цветным кружком и подписью.

**Props-интерфейс:**

```tsx
interface ColorOption {
  value: string;
  label: string;
  color: string; // hex цвет кружка
}

interface ColorSelectorProps {
  options: ColorOption[];
  value: string;
  onChange: (value: string) => void;
}
```

**Реализация:**

```tsx
import { cn } from '@rollout/ui-kit';

export function ColorSelector({ options, value, onChange }: ColorSelectorProps) {
  return (
    <div className="flex flex-wrap gap-2">
      {options.map((option) => {
        const isSelected = option.value === value;
        return (
          <button
            key={option.value}
            onClick={() => onChange(option.value)}
            className={cn(
              'flex items-center gap-2 rounded-xl border px-4 py-2.5 text-sm font-medium transition-colors',
              isSelected
                ? 'border-[#3A3A3C] bg-[#3A3A3C] text-[#FAFAFA]'
                : 'border-[#3A3A3C] bg-transparent text-[#FAFAFA] hover:bg-[#1C1C1E]'
            )}
          >
            <span
              className="size-4 rounded-full border border-[#3A3A3C]"
              style={{ backgroundColor: option.color }}
            />
            {option.label}
          </button>
        );
      })}
    </div>
  );
}
```

---

### 3.7 ImageUploader

**Описание:** Компонент загрузки изображений для формы отзыва. Показывает превью с возможностью удаления и кнопку добавления нового.

**Props-интерфейс:**

```tsx
interface ImageUploaderProps {
  images: string[];
  onAdd: () => void;
  onRemove: (index: number) => void;
  maxImages?: number;
}
```

**Реализация:**

```tsx
import { Upload, X } from 'lucide-react';
import { cn } from '@rollout/ui-kit';

export function ImageUploader({ images, onAdd, onRemove, maxImages = 5 }: ImageUploaderProps) {
  const canAdd = images.length < maxImages;

  return (
    <div className="flex flex-wrap gap-3">
      {images.map((img, idx) => (
        <div key={idx} className="relative size-20">
          <img
            src={img}
            alt={`Upload ${idx + 1}`}
            className="size-full rounded-xl object-cover"
          />
          <button
            onClick={() => onRemove(idx)}
            className="absolute -right-1.5 -top-1.5 flex size-6 items-center justify-center rounded-full bg-[#3A3A3C] text-[#FAFAFA] hover:bg-[#48484A]"
          >
            <X className="size-3.5" />
          </button>
        </div>
      ))}

      {canAdd && (
        <button
          onClick={onAdd}
          className={cn(
            'flex size-20 flex-col items-center justify-center gap-1 rounded-xl border-2 border-dashed border-[#3A3A3C] text-[#8E8E93] transition-colors hover:border-[#636366] hover:text-[#FAFAFA]'
          )}
        >
          <Upload className="size-6" />
        </button>
      )}
    </div>
  );
}
```

---

### 3.8 SortSheet

**Описание:** Bottom sheet / модалка для выбора сортировки отзывов.

**Props-интерфейс:**

```tsx
type SortOption = 'newest' | 'positive' | 'negative' | 'rating';

interface SortSheetProps {
  isOpen: boolean;
  onClose: () => void;
  value: SortOption;
  onChange: (value: SortOption) => void;
  onApply: () => void;
  onReset: () => void;
}
```

**Shadcn-референс:** `Sheet` (боковая панель) или кастомный Bottom Sheet

**Реализация:**

```tsx
import { Sheet, SheetContent, SheetHeader, SheetTitle } from '@rollout/ui-kit';
import { Check } from 'lucide-react';
import { cn } from '@rollout/ui-kit';

const sortOptions: { value: SortOption; label: string }[] = [
  { value: 'newest', label: 'Новые' },
  { value: 'positive', label: 'Положительные' },
  { value: 'negative', label: 'Отрицательные' },
  { value: 'rating', label: 'По рейтингу' },
];

export function SortSheet({ isOpen, onClose, value, onChange, onApply, onReset }: SortSheetProps) {
  return (
    <Sheet open={isOpen} onOpenChange={(open) => !open && onClose()}>
      <SheetContent
        side="bottom"
        className="h-auto rounded-t-2xl border-t border-[#3A3A3C] bg-[#0A0A0A] p-0"
      >
        {/* Индикатор свайпа */}
        <div className="flex justify-center pt-3 pb-1">
          <div className="h-1 w-10 rounded-full bg-[#3A3A3C]" />
        </div>

        <SheetHeader className="px-4 pb-4">
          <SheetTitle className="text-left text-lg font-semibold text-[#FAFAFA]">
            Здесь вы можете отсортировать отзывы
          </SheetTitle>
          <p className="text-sm text-[#8E8E93]">Показать сначала</p>
        </SheetHeader>

        <div className="flex flex-col px-4">
          {sortOptions.map((option) => {
            const isSelected = option.value === value;
            return (
              <button
                key={option.value}
                onClick={() => onChange(option.value)}
                className="flex items-center justify-between py-3.5 text-left"
              >
                <span className={cn('text-base', isSelected ? 'font-medium text-[#FAFAFA]' : 'text-[#FAFAFA]')}>
                  {option.label}
                </span>
                {isSelected && <Check className="size-5 text-[#FAFAFA]" />}
              </button>
            );
          })}
        </div>

        <div className="flex flex-col gap-2 p-4">
          <Button onClick={onApply} className="w-full rounded-xl bg-[#E5E5E5] py-3 text-base font-medium text-[#0A0A0A] hover:bg-[#D4D4D4]">
            Показать
          </Button>
          <Button
            onClick={onReset}
            variant="outline"
            className="w-full rounded-xl border-[#3A3A3C] py-3 text-base font-medium text-[#FAFAFA] hover:bg-[#1C1C1E]"
          >
            Сбросить
          </Button>
        </div>
      </SheetContent>
    </Sheet>
  );
}
```

---

### 3.9 PageHeader

**Описание:** Универсальная шапка страницы с кнопкой назад, заголовком и дополнительными действиями.

**Props-интерфейс:**

```tsx
interface PageHeaderProps {
  title: string;
  onBack?: () => void;
  actions?: React.ReactNode;
}
```

**Реализация:**

```tsx
import { ArrowLeft } from 'lucide-react';

export function PageHeader({ title, onBack, actions }: PageHeaderProps) {
  return (
    <div className="flex items-center justify-between py-4">
      <div className="flex items-center gap-3">
        {onBack && (
          <button
            onClick={onBack}
            className="flex size-10 items-center justify-center rounded-xl text-[#FAFAFA] hover:bg-[#1C1C1E]"
          >
            <ArrowLeft className="size-6" />
          </button>
        )}
        <h1 className="text-xl font-semibold text-[#FAFAFA]">{title}</h1>
      </div>
      {actions && <div className="flex items-center gap-2">{actions}</div>}
    </div>
  );
}
```

---

### 3.10 AppFooter

**Описание:** Футер приложения с копирайтом и социальными иконками.

**Реализация:**

```tsx
import { Linkedin, Send } from 'lucide-react';

// Кастомная иконка X (Twitter) — в lucide-react нет, используем SVG
function XIcon({ className }: { className?: string }) {
  return (
    <svg className={className} viewBox="0 0 24 24" fill="currentColor">
      <path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z" />
    </svg>
  );
}

export function AppFooter() {
  return (
    <footer className="flex items-center justify-between py-6">
      <span className="text-sm text-[#8E8E93]">© 2025 Rollout</span>
      <div className="flex items-center gap-4">
        <a href="#" className="text-[#8E8E93] hover:text-[#FAFAFA]">
          <XIcon className="size-5" />
        </a>
        <a href="#" className="text-[#8E8E93] hover:text-[#FAFAFA]">
          <Linkedin className="size-5" />
        </a>
        <a href="#" className="text-[#8E8E93] hover:text-[#FAFAFA]">
          <Send className="size-5" />
        </a>
      </div>
    </footer>
  );
}
```

---

## 4. Структура каждого экрана

### 4.1 Экран: Рейтинг и отзывы (`/item/:id/reviews`)

**Скриншоты:**
- [`Rating_and_reviews_layout_1.png`](./5-ItemCard_screenshots/Rating_and_reviews_layout_1.png) — основной вид
- [`Rating_and_reviews_layout_2.png`](./5-ItemCard_screenshots/Rating_and_reviews_layout_2.png) — с открытым меню отзыва

**Layout:**
```tsx
<div className="mx-auto flex max-w-[576px] flex-col gap-7 pt-20 pb-8">
  <PageHeader title="Рейтинг и отзывы" onBack={...} actions={...} />
  <RatingBreakdown ... />
  <Button variant="primary">Написать отзыв</Button>
  <div className="flex flex-col">
    {reviews.map(review => <ReviewCard key={review.id} ... />)}
  </div>
  <AppFooter />
</div>
```

**Компоненты сверху вниз:**
1. `PageHeader` — стрелка назад, заголовок, иконки поиска и сортировки
2. `RatingBreakdown` — число 4.9, звёзды, 20 оценок, 5 прогресс-баров
3. `Button` — "Написать отзыв" (primary, полная ширина)
4. Список `ReviewCard` — карточки отзывов
5. `AppFooter`

**Состояния:**
- **loading** — скелетоны для рейтинга и списка отзывов
- **empty** — "Пока нет отзывов" с иконкой и кнопкой "Написать первый отзыв"
- **error** — тост/баннер с ошибкой загрузки
- **success** — отображение списка

**Интерактивность:**
- Клик по стрелке назад → навигация на страницу товара
- Клик по поиску → открытие поиска по отзывам
- Клик по сортировке → открытие `SortSheet`
- Клик "Написать отзыв" → переход на `/item/:id/reviews/new`
- Лайк/дизлайк → обновление счётчика с optimistic update
- Меню (⋮) → открытие `ReviewMenuPopover`
- "Читать ещё" → разворачивание текста

---

### 4.2 Экран: Поделитесь опытом (`/item/:id/reviews/new`)

**Скриншоты:**
- [`Experience_layout_1.png`](./5-ItemCard_screenshots/Experience_layout_1.png) — форма
- [`Experience_layout_2.png`](./5-ItemCard_screenshots/Experience_layout_2.png) — с загруженным фото

**Layout:**
```tsx
<div className="mx-auto flex max-w-[576px] flex-col gap-7 pt-20 pb-8">
  <PageHeader title="Поделитесь опытом" onBack={...} />
  <StarRating interactive value={rating} onChange={setRating} />
  <p className="text-center text-sm text-[#8E8E93]">Оценка полученной услуги</p>
  
  <div className="flex flex-col gap-4">
    <label className="text-lg font-semibold text-[#FAFAFA]">Общее впечатление</label>
    <Input placeholder="Общее впечатление" />
  </div>
  
  <div className="flex flex-col gap-4">
    <label className="text-lg font-semibold text-[#FAFAFA]">Достоинства</label>
    <Input placeholder="Расскажите, что понравилось" />
  </div>
  
  <div className="flex flex-col gap-4">
    <label className="text-lg font-semibold text-[#FAFAFA]">Недостатки</label>
    <Input placeholder="Расскажите, что не так" />
  </div>
  
  <ImageUploader ... />
  
  <div className="flex items-center justify-between">
    <span className="text-base font-medium text-[#FAFAFA]">Анонимность</span>
    <Switch checked={isAnonymous} onCheckedChange={setIsAnonymous} />
  </div>
  
  <Button onClick={handleSubmit}>Отправить</Button>
  <AppFooter />
</div>
```

**Компоненты сверху вниз:**
1. `PageHeader` — стрелка назад, заголовок
2. `StarRating` (interactive) — 5 звёзд для выбора оценки
3. Подпись "Оценка полученной услуги"
4. Поле "Общее впечатление" — Input
5. Поле "Достоинства" — Input
6. Поле "Недостатки" — Input
7. `ImageUploader` — кнопка "Добавить медиа" / превью загруженных
8. `Switch` — "Анонимность"
9. `Button` — "Отправить" (primary, полная ширина)
10. `AppFooter`

**Состояния:**
- **initial** — пустая форма, звёзды не выбраны
- **filled** — пользователь заполнил поля
- **submitting** — кнопка в состоянии загрузки
- **success** — редирект на список отзывов + тост "Спасибо за отзыв!"
- **error** — валидация полей (звёзды обязательны)

**Интерактивность:**
- Звёзды — hover эффект, клик фиксирует оценку
- Поля — onChange обновляет локальный state
- "Добавить медиа" — вызывает file picker (mock)
- Тоггл анонимности — переключает state
- "Отправить" — валидация и submit

---

### 4.3 Экран: Сортировка отзывов (Bottom Sheet)

**Скриншоты:**
- [`Sorting_layout_2.png`](./5-ItemCard_screenshots/Sorting_layout_2.png) — боковая панель
- [`Sorting_layout_3.png`](./5-ItemCard_screenshots/Sorting_layout_3.png) — bottom sheet

**Layout:**
```tsx
<SortSheet
  isOpen={isSortOpen}
  onClose={() => setIsSortOpen(false)}
  value={sortBy}
  onChange={setSortBy}
  onApply={handleApplySort}
  onReset={handleResetSort}
/>
```

**Компоненты сверху вниз:**
1. Индикатор свайпа (серый бар)
2. Заголовок "Здесь вы можете отсортировать отзывы"
3. Подпись "Показать сначала"
4. Список опций с радио-выбором + галочка
5. Кнопка "Показать" (primary)
6. Кнопка "Сбросить" (secondary/outline)

**Состояния:**
- **open** — шит открыт
- **closed** — шит закрыт

**Интерактивность:**
- Свайп вниз / клик вне → закрытие
- Клик по опции → выбор (без закрытия)
- "Показать" → применение сортировки и закрытие
- "Сбросить" → сброс на дефолтное значение

---

### 4.4 Экран: Селектор вариантов (Cosmetics / House / Flower)

**Скриншоты:**
- [`CosmeticsCard_CosmeticsCard_Selection__scn.png`](./5-ItemCard_screenshots/CosmeticsCard_CosmeticsCard_Selection__scn.png) — размер + цвет
- [`HouseCard_HomeCard_Selection_scn.png`](./5-ItemCard_screenshots/HouseCard_HomeCard_Selection_scn.png) — цвет
- [`FlowerCard_FlowerCard_SelectionColor__scn.png`](./5-ItemCard_screenshots/FlowerCard_FlowerCard_SelectionColor__scn.png) — цвет

**Layout:**
```tsx
<div className="flex flex-col gap-4">
  {/* Сегментированный селектор размера (только для Cosmetics) */}
  <SegmentedControl
    options={[
      { value: '30ml', label: '30 мл' },
      { value: '60ml', label: '60 мл' },
      { value: '90ml', label: '90 мл' },
      { value: '120ml', label: '120 мл' },
      { value: '250ml', label: '250 мл' },
    ]}
    value={selectedSize}
    onChange={setSelectedSize}
  />
  
  {/* Селектор цвета */}
  <ColorSelector
    options={[
      { value: 'blue', label: 'Синий', color: '#007AFF' },
      { value: 'red', label: 'Красный', color: '#FF3B30' },
      { value: 'gray', label: 'Серый', color: '#8E8E93' },
      { value: 'black', label: 'Черный', color: '#1C1C1E' },
    ]}
    value={selectedColor}
    onChange={setSelectedColor}
  />
</div>
```

**Компоненты сверху вниз:**
1. `SegmentedControl` — размеры (только Cosmetics)
2. `ColorSelector` — цвета

**Состояния:**
- **default** — первый вариант выбран по умолчанию
- **selected** — пользователь выбрал другой вариант

**Интерактивность:**
- Клик по сегменту → выбор размера
- Клик по цвету → выбор цвета

---

## 5. Пример полной страницы

### Страница: Рейтинг и отзывы

```tsx
// apps/demo/src/pages/item/ReviewsPage.tsx

import { useState } from 'react';
import { useNavigate, useParams } from 'react-router-dom';
import { Search, ArrowUpDown } from 'lucide-react';
import { Button, Input } from '@rollout/ui-kit';
import { PageHeader } from '@/components/PageHeader';
import { AppFooter } from '@/components/AppFooter';
import { StarRating } from '@/components/StarRating';
import { RatingBreakdown } from '@/components/RatingBreakdown';
import { ReviewCard } from '@/components/ReviewCard';
import { ReviewMenuPopover } from '@/components/ReviewMenuPopover';
import { SortSheet } from '@/components/SortSheet';
import type { SortOption } from '@/components/SortSheet';

const mockReviews = [
  {
    id: '1',
    author: { name: 'Маргарита Ромашкина', avatar: '/avatars/user1.jpg' },
    date: '24 Октября 2025',
    rating: 5,
    isVerifiedBuyer: true,
    images: ['/review/1.jpg', '/review/2.jpg', '/review/3.jpg', '/review/4.jpg'],
    text: 'Сотрудники всегда приветливы, компетентны и готовы помочь в решении любых вопросов. Особенно хочу отметить оперативность работы менеджеров при оформлении документов. Рекомендую!',
    likes: 10,
    dislikes: 10,
    comments: 10,
  },
  {
    id: '2',
    author: { name: 'Маргарита Ромашкина', avatar: '/avatars/user1.jpg' },
    date: '24 Октября 2025',
    rating: 3,
    isVerifiedBuyer: false,
    text: 'Сотрудники всегда приветливы, компетентны и готовы помочь в решении любых вопросов. Особенно хочу отметить оперативность работы менеджеров при оформлении документов.',
    likes: 10,
    dislikes: 10,
    comments: 10,
  },
];

const mockDistribution = [
  { stars: 5, percentage: 100 },
  { stars: 4, percentage: 75 },
  { stars: 3, percentage: 50 },
  { stars: 2, percentage: 25 },
  { stars: 1, percentage: 0 },
];

export default function ReviewsPage() {
  const navigate = useNavigate();
  const { id } = useParams<{ id: string }>();
  
  const [activeMenuId, setActiveMenuId] = useState<string | null>(null);
  const [isSortOpen, setIsSortOpen] = useState(false);
  const [sortBy, setSortBy] = useState<SortOption>('newest');

  const handleLike = (reviewId: string) => {
    // Mock handler
  };

  const handleDislike = (reviewId: string) => {
    // Mock handler
  };

  const handleComment = (reviewId: string) => {
    // Mock handler
  };

  const handleReportReview = (reviewId: string) => {
    // Mock handler
  };

  const handleApplySort = () => {
    setIsSortOpen(false);
    // Mock: применить сортировку
  };

  const handleResetSort = () => {
    setSortBy('newest');
  };

  return (
    <div className="mx-auto flex min-h-screen max-w-[576px] flex-col gap-7 px-4 pt-20 pb-8">
      {/* Шапка */}
      <PageHeader
        title="Рейтинг и отзывы"
        onBack={() => navigate(`/item/${id}`)}
        actions={
          <>
            <button className="flex size-10 items-center justify-center rounded-xl text-[#FAFAFA] hover:bg-[#1C1C1E]">
              <Search className="size-6" />
            </button>
            <button
              onClick={() => setIsSortOpen(true)}
              className="flex size-10 items-center justify-center rounded-xl text-[#FAFAFA] hover:bg-[#1C1C1E]"
            >
              <ArrowUpDown className="size-6" />
            </button>
          </>
        }
      />

      {/* Рейтинг */}
      <RatingBreakdown
        rating={4.9}
        totalReviews={20}
        distribution={mockDistribution}
      />

      {/* Кнопка написать отзыв */}
      <Button
        onClick={() => navigate(`/item/${id}/reviews/new`)}
        className="w-full rounded-xl bg-[#E5E5E5] py-3.5 text-base font-medium text-[#0A0A0A] hover:bg-[#D4D4D4]"
      >
        Написать отзыв
      </Button>

      {/* Список отзывов */}
      <div className="flex flex-col">
        {mockReviews.map((review) => (
          <ReviewCard
            key={review.id}
            review={review}
            onLike={handleLike}
            onDislike={handleDislike}
            onComment={handleComment}
            onMenu={(id) => setActiveMenuId(id)}
          />
        ))}
      </div>

      {/* Поповер меню */}
      {activeMenuId && (
        <ReviewMenuPopover
          reviewId={activeMenuId}
          isOpen={true}
          onClose={() => setActiveMenuId(null)}
          onReportReview={handleReportReview}
        />
      )}

      {/* Сортировка */}
      <SortSheet
        isOpen={isSortOpen}
        onClose={() => setIsSortOpen(false)}
        value={sortBy}
        onChange={setSortBy}
        onApply={handleApplySort}
        onReset={handleResetSort}
      />

      {/* Футер */}
      <AppFooter />
    </div>
  );
}
```

---

## 6. Чек-лист вёрстки

- [ ] **Все цвета через токены** — не хардкод `#FAFAFA`, использовать `text-foreground` / `bg-background` из темы
- [ ] **Типографика совпадает с макетом** — проверить размеры, веса, line-height для всех уровней
- [ ] **Отступы и радиусы по дизайн-системе** — `gap-7` между секциями, `rounded-2xl` для карточек, `rounded-xl` для кнопок и инпутов
- [ ] **Иконки из lucide-react** — `size-5` для интерактивных, `size-6` для навигации
- [ ] **Контейнер** — `max-w-[576px] mx-auto` на каждой странице
- [ ] **Отступ от шапки** — `pt-20` для всех страниц модуля
- [ ] **Мобильный отступ** — `pb-tabbar md:pb-0` для нижнего отступа
- [ ] **Все формы на локальном useState** — никаких глобальных сторов для форм отзыва
- [ ] **Mock-обработчики** — обработчики событий без `console.log`, с TODO-комментариями
- [ ] **Route добавлен в App.tsx** — все роуты модуля зарегистрированы
- [ ] **Скриншот соответствует макету** — визуальный diff пройден
- [ ] **StarRating** — корректное отображение заполненных и пустых звёзд
- [ ] **ImageUploader** — превью с крестиком удаления, пунктирная рамка для добавления
- [ ] **ReviewMenuPopover** — позиционируется корректно относительно кнопки меню
- [ ] **SortSheet** — bottom sheet с индикатором свайпа, корректная анимация
- [ ] **Анонимность Switch** — корректное toggle состояние
- [ ] **Footer** — © 2025 Rollout + иконки X, LinkedIn, Telegram (Send)
- [ ] **Нет radix-ui** — всё через `@rollout/ui-kit` на `@base-ui/react`
- [ ] **Нет deep imports** — `import { Button, Input } from '@rollout/ui-kit'`
- [ ] **Нет shadcn add** — использовать только `@rollout/ui-kit`

---

## Приложение: Скриншоты

| Файл | Описание | Размер |
|------|----------|--------|
| `Cover_5_ItemCard.png` | Обложка модуля | 2.9 MB |
| `Common_ItemCard_categories.png` | Общие компоненты (категории) | 5 KB |
| `CosmeticsCard_CosmeticsCard_Selection__scn.png` | Селектор размера/цвета (косметика) | 24 KB |
| `HouseCard_HomeCard_Selection_scn.png` | Селектор цвета (дом) | 15 KB |
| `FlowerCard_FlowerCard_SelectionColor__scn.png` | Селектор цвета (цветы) | 11 KB |
| `Rating_and_reviews_layout_1.png` | Рейтинг и отзывы (основной) | 153 KB |
| `Rating_and_reviews_layout_2.png` | Рейтинг и отзывы (меню) | 159 KB |
| `Experience_layout_1.png` | Форма отзыва | 87 KB |
| `Experience_layout_2.png` | Форма отзыва (с фото) | 113 KB |
| `Sorting_layout_1.png` | Сортировка (список) | 109 KB |
| `Sorting_layout_2.png` | Сортировка (панель) | 158 KB |
| `Sorting_layout_3.png` | Сортировка (bottom sheet) | 37 KB |

**Всего скачано скриншотов: 12**
