# Инструкция по вёрстке модуля 12-Loyalty

## 1. Обзор модуля

Модуль **Loyalty** объединяет кэшбэк, реферальную программу, офферы-партнёры, компенсацию платежей и выбор категорий кэшбэка.

### Экраны

| Экран | Роут | Назначение |
|-------|------|------------|
| Main | `/loyalty/main` | Главная: баланс, бонусы, рефералы, повышенный кэшбэк, FAQ |
| Referal1 | `/loyalty/referal1` | История рекомендаций с таб-фильтрами |
| Referal2 | `/loyalty/referal2` | Лотерея/призы реферальной программы |
| Offerwall | `/loyalty/offerwall` | Детальная карточка партнёрского оффера |
| Compensation | `/loyalty/compensation` | Выбор платежа и форма компенсации |
| Categories | `/loyalty/catergories` | Выбор 3 категорий повышенного кэшбэка |

### Глобальные правила
- Все страницы обёрнуты в контейнер: `mx-auto flex max-w-[576px] flex-col gap-7 pt-20 pb-8`
- Тёмная тема по умолчанию, фон `#0A0A0A`
- Верхняя навигация: кнопка «Назад» + заголовок + иконка поиска (на Main)
- Футер: `© 2025 Rollout` + иконки соцсетей (X, LinkedIn, Telegram)

---

## 2. Дизайн-система

### Цвета

```css
--bg-primary: #0A0A0A
--bg-card: #1C1C1E
--text-primary: #FAFAFA
--text-secondary: #A1A1AA
--border: #3A3A3C
--success: #22C55E
--accent: #E5E5E5        /* Primary button bg */
```

### Типографика
- Шрифт: `Geist Variable`
- Заголовки экранов: `text-xl font-semibold`
- Подзаголовки: `text-lg font-medium`
- Описания: `text-sm text-[#A1A1AA]`
- Цифры/баланс: `text-3xl font-bold`

### Радиусы
- Primary button: `rounded-xl` (12px)
- Input: `rounded-xl` (12px)
- Card: `rounded-2xl` (16px)
- Avatar: `rounded-full`
- Badge: `rounded-full`

### Spacing
- Gap между секциями: `gap-7` (28px)
- Gap внутри карточек: `gap-4` (16px)
- Padding карточек: `p-4` или `p-5`
- Margin от хедера: `pt-20` (80px)

---

## 3. Компоненты UI

### 3.1 LoyaltyCard

Карточка баланса или информационного блока. Используется на Main для «Накопили в этом месяце» и «Кэшбэк дня».

```tsx
import { Card } from "@rollout/ui-kit";

interface LoyaltyCardProps {
  title: string;
  value?: string;
  subtitle?: string;
  action?: React.ReactNode;
  children?: React.ReactNode;
  className?: string;
}

export const LoyaltyCard = ({ title, value, subtitle, action, children, className }: LoyaltyCardProps) => (
  <Card className={cn("bg-[#1C1C1E] border-[#3A3A3C] rounded-2xl p-5", className)}>
    <div className="flex flex-col gap-2">
      <Typography className="text-sm text-[#A1A1AA]">{title}</Typography>
      {value && <Typography className="text-3xl font-bold text-[#FAFAFA]">{value}</Typography>}
      {subtitle && <Typography className="text-sm text-[#A1A1AA]">{subtitle}</Typography>}
      {action && <div className="mt-3">{action}</div>}
      {children}
    </div>
  </Card>
);
```

### 3.2 CashbackCategory

Карточка категории в горизонтальном скролле. Используется на Main в секции «Повышенный кэшбэк».

```tsx
interface CashbackCategoryProps {
  image: string;
  discount: string;
  name: string;
}

export const CashbackCategory = ({ image, discount, name }: CashbackCategoryProps) => (
  <div className="flex flex-col gap-2 min-w-[140px]">
    <div className="relative rounded-2xl overflow-hidden aspect-square bg-[#1C1C1E]">
      <img src={image} alt={name} className="w-full h-full object-cover" />
      <Badge className="absolute bottom-2 left-2 bg-[#0A0A0A]/80 text-[#FAFAFA] rounded-full px-2 py-1 text-xs">
        {discount}
      </Badge>
    </div>
    <Typography className="text-sm text-[#FAFAFA] truncate">{name}</Typography>
  </div>
);
```

### 3.3 ReferralProgress

Прогресс-бар с шагами реферальной программы. Используется на Referal экранах.

```tsx
interface Step {
  label: string;
  status: "completed" | "active" | "pending";
  reward?: string;
}

interface ReferralProgressProps {
  steps: Step[];
}

export const ReferralProgress = ({ steps }: ReferralProgressProps) => (
  <div className="flex flex-col gap-4">
    {steps.map((step, idx) => (
      <div key={idx} className="flex items-center gap-3">
        <div className={cn(
          "w-6 h-6 rounded-full flex items-center justify-center text-xs",
          step.status === "completed" && "bg-[#22C55E] text-white",
          step.status === "active" && "bg-[#3A3A3C] text-[#FAFAFA] border border-[#FAFAFA]",
          step.status === "pending" && "bg-[#3A3A3C] text-[#A1A1AA]"
        )}>
          {step.status === "completed" ? <Check className="w-4 h-4" /> : <Plus className="w-4 h-4" />}
        </div>
        <Typography className={cn(
          "text-sm flex-1",
          step.status === "pending" ? "text-[#A1A1AA]" : "text-[#FAFAFA]"
        )}>
          {step.label}
        </Typography>
        {step.reward && (
          <Typography className="text-sm text-[#A1A1AA]">{step.reward}</Typography>
        )}
      </div>
    ))}
  </div>
);
```

### 3.4 QRCodeDisplay

Блок с QR-кодом для реферальной программы. Используется на Referal экранах.

```tsx
interface QRCodeDisplayProps {
  value: string;
  label?: string;
}

export const QRCodeDisplay = ({ value, label }: QRCodeDisplayProps) => (
  <div className="flex flex-col items-center gap-4 p-5 bg-[#1C1C1E] border border-[#3A3A3C] rounded-2xl">
    {label && <Typography className="text-sm text-[#A1A1AA]">{label}</Typography>}
    <div className="w-48 h-48 bg-white rounded-xl p-3">
      {/* QR-code placeholder — использовать qrcode.react или img */}
      <img src={`/qr/${value}.png`} alt="QR Code" className="w-full h-full" />
    </div>
    <ShareButton value={value} />
  </div>
);
```

### 3.5 FAQAccordion

Аккордеон вопросов и ответов. Используется на Main и Referal экранах.

```tsx
import { Accordion } from "@base-ui/react";
import { ChevronDown } from "lucide-react";

interface FAQItem {
  question: string;
  answer: string;
}

interface FAQAccordionProps {
  items: FAQItem[];
}

export const FAQAccordion = ({ items }: FAQAccordionProps) => (
  <Accordion.Root className="flex flex-col gap-0" type="multiple">
    {items.map((item, idx) => (
      <Accordion.Item key={idx} value={`item-${idx}`} className="border-b border-[#3A3A3C]">
        <Accordion.Trigger className="flex items-center justify-between w-full py-4 text-left text-[#FAFAFA] text-sm">
          {item.question}
          <ChevronDown className="w-4 h-4 text-[#A1A1AA] transition-transform group-data-[state=open]:rotate-180" />
        </Accordion.Trigger>
        <Accordion.Content className="text-sm text-[#A1A1AA] pb-4">
          {item.answer}
        </Accordion.Content>
      </Accordion.Item>
    ))}
  </Accordion.Root>
);
```

### 3.6 OfferItem

Карточка оффера-партнёра. Используется на Main (горизонтальный скролл) и Offerwall (детальная).

```tsx
interface OfferItemProps {
  logo: string;
  discount: string;
  title: string;
  description: string;
  deadline?: string;
  image?: string;
  promoCode?: string;
}

export const OfferItem = ({ logo, discount, title, description, deadline, image, promoCode }: OfferItemProps) => (
  <Card className="bg-[#1C1C1E] border-[#3A3A3C] rounded-2xl overflow-hidden">
    <div className="p-5 flex items-start justify-between">
      <div className="flex flex-col gap-1">
        <Typography className="text-2xl font-bold text-[#FAFAFA]">{discount}</Typography>
        <Typography className="text-sm text-[#A1A1AA]">{description}</Typography>
      </div>
      {deadline && (
        <Badge className="bg-[#FAFAFA] text-[#0A0A0A] rounded-full px-3 py-1 text-xs">
          {deadline}
        </Badge>
      )}
    </div>
    {image && <img src={image} alt={title} className="w-full aspect-[16/9] object-cover" />}
    <div className="p-5 flex items-center gap-3">
      <img src={logo} alt={title} className="w-10 h-10 rounded-full" />
      <div className="flex flex-col">
        <Typography className="text-sm font-medium text-[#FAFAFA]">{title}</Typography>
        <Typography className="text-xs text-[#A1A1AA] line-clamp-2">{description}</Typography>
      </div>
    </div>
    {promoCode && (
      <div className="px-5 pb-5">
        <div className="flex items-center gap-2 bg-[#1C1C1E] border border-[#3A3A3C] rounded-xl px-4 py-3">
          <Copy className="w-4 h-4 text-[#A1A1AA]" />
          <Typography className="text-sm text-[#FAFAFA] flex-1 text-center">{promoCode}</Typography>
        </div>
      </div>
    )}
  </Card>
);
```

### 3.7 CompensationForm

Форма компенсации со слайдером и выбором карты. Используется на Compensation (детальная страница).

```tsx
import { useState } from "react";
import { Button, Card } from "@rollout/ui-kit";
import { Slider } from "@base-ui/react";

interface CompensationFormProps {
  maxBonus: number;
  exchangeRate: number; // бонусов за 1 рубль
  cardLabel: string;
  cardMask: string;
  onSubmit: (amount: number) => void;
}

export const CompensationForm = ({ maxBonus, exchangeRate, cardLabel, cardMask, onSubmit }: CompensationFormProps) => {
  const [amount, setAmount] = useState(maxBonus / exchangeRate / 2);
  const rubAmount = Math.floor(amount);
  const bonusAmount = rubAmount * exchangeRate;

  return (
    <div className="flex flex-col gap-5">
      <Card className="bg-[#1C1C1E] border-[#3A3A3C] rounded-2xl p-5">
        <Typography className="text-base text-[#FAFAFA]">
          Доступно бонусов: {maxBonus.toLocaleString("ru-RU", { minimumFractionDigits: 2 })}
        </Typography>
        <Typography className="text-sm text-[#A1A1AA]">
          Обмен: {exchangeRate} бонуса = 1 ₽
        </Typography>
      </Card>

      <Card className="bg-[#1C1C1E] border-[#3A3A3C] rounded-2xl p-5 flex flex-col gap-4">
        <Typography className="text-2xl font-bold text-[#FAFAFA]">
          {rubAmount.toLocaleString("ru-RU", { minimumFractionDigits: 2 })} ₽
        </Typography>
        <Typography className="text-sm text-[#A1A1AA]">Сумма к зачислению</Typography>
        <Slider.Root
          value={rubAmount}
          min={0}
          max={maxBonus / exchangeRate}
          step={1}
          onValueChange={setAmount}
          className="relative flex items-center w-full h-5"
        >
          <Slider.Track className="h-2 w-full rounded-full bg-[#3A3A3C]">
            <Slider.Thumb className="w-5 h-5 rounded-full bg-[#FAFAFA] border-2 border-[#3A3A3C]" />
          </Slider.Track>
        </Slider.Root>
      </Card>

      <div className="flex flex-col gap-2">
        <Typography className="text-lg text-[#FAFAFA]">
          {bonusAmount.toLocaleString("ru-RU", { minimumFractionDigits: 2 })} бонуса
        </Typography>
        <Typography className="text-sm text-[#A1A1AA]">Будет списано</Typography>
      </div>

      <Separator className="bg-[#3A3A3C]" />

      <Card className="bg-[#1C1C1E] border-[#3A3A3C] rounded-2xl p-5 flex items-center justify-between">
        <div className="flex flex-col">
          <Typography className="text-sm text-[#FAFAFA]">Зачислим на</Typography>
          <Typography className="text-sm text-[#A1A1AA]">{cardLabel} {cardMask}</Typography>
        </div>
        <div className="w-12 h-8 bg-white rounded flex items-center justify-center">
          <Typography className="text-xs font-bold text-[#1A1F71]">VISA</Typography>
        </div>
      </Card>

      <Button
        onClick={() => onSubmit(rubAmount)}
        className="w-full h-14 bg-[#E5E5E5] text-[#0A0A0A] rounded-xl font-medium"
      >
        Зачислить {rubAmount.toLocaleString("ru-RU", { minimumFractionDigits: 2 })} ₽
      </Button>
    </div>
  );
};
```

### 3.8 CategoryGrid

Сетка категорий с чекбоксами. Используется на Categories.

```tsx
import { useState } from "react";
import { Button, Checkbox } from "@rollout/ui-kit";

interface Category {
  id: string;
  icon: React.ReactNode;
  color: string;
  label: string;
  description: string;
}

interface CategoryGridProps {
  categories: Category[];
  maxSelection?: number;
  onConfirm: (selected: string[]) => void;
}

export const CategoryGrid = ({ categories, maxSelection = 3, onConfirm }: CategoryGridProps) => {
  const [selected, setSelected] = useState<string[]>([]);

  const toggle = (id: string) => {
    setSelected(prev =>
      prev.includes(id) ? prev.filter(x => x !== id)
      : prev.length < maxSelection ? [...prev, id]
      : prev
    );
  };

  return (
    <div className="flex flex-col gap-6">
      <div className="flex flex-col gap-4">
        {categories.map(cat => (
          <div
            key={cat.id}
            onClick={() => toggle(cat.id)}
            className="flex items-center gap-4 cursor-pointer"
          >
            <Checkbox.Root
              checked={selected.includes(cat.id)}
              className={cn(
                "w-6 h-6 rounded-md border-2 flex items-center justify-center transition-colors",
                selected.includes(cat.id)
                  ? "bg-[#FAFAFA] border-[#FAFAFA]"
                  : "bg-transparent border-[#3A3A3C]"
              )}
            >
              {selected.includes(cat.id) && <Check className="w-4 h-4 text-[#0A0A0A]" />}
            </Checkbox.Root>
            <div className={cn("w-12 h-12 rounded-xl flex items-center justify-center text-white", cat.color)}>
              {cat.icon}
            </div>
            <div className="flex flex-col flex-1">
              <Typography className="text-base text-[#FAFAFA]">{cat.label}</Typography>
              <Typography className="text-sm text-[#A1A1AA]">{cat.description}</Typography>
            </div>
          </div>
        ))}
      </div>
      <Button
        disabled={selected.length !== maxSelection}
        onClick={() => onConfirm(selected)}
        className="w-full h-14 bg-[#E5E5E5] text-[#0A0A0A] rounded-xl font-medium disabled:opacity-40"
      >
        Подтвердить
      </Button>
    </div>
  );
};
```

### 3.9 PointsDisplay

Отображение баллов/бонусов. Используется на Referal (drawer, лотерея) и Compensation.

```tsx
interface PointsDisplayProps {
  value: number;
  label: string;
  size?: "sm" | "md" | "lg";
}

export const PointsDisplay = ({ value, label, size = "md" }: PointsDisplayProps) => {
  const sizeMap = {
    sm: "text-lg",
    md: "text-2xl",
    lg: "text-4xl"
  };

  return (
    <div className="flex flex-col gap-1">
      <Typography className="text-sm text-[#A1A1AA]">{label}</Typography>
      <Typography className={cn("font-bold text-[#FAFAFA]", sizeMap[size])}>
        {value.toLocaleString("ru-RU")} Б
      </Typography>
    </div>
  );
};
```

### 3.10 ShareButton

Кнопка шеринга с выпадающим списком соцсетей.

```tsx
import { useState } from "react";
import { Button } from "@rollout/ui-kit";
import { Share2, Copy, MessageCircle, Link } from "lucide-react";

interface ShareButtonProps {
  value: string;
  variant?: "icon" | "button";
}

export const ShareButton = ({ value, variant = "button" }: ShareButtonProps) => {
  const [open, setOpen] = useState(false);

  const handleCopy = () => {
    navigator.clipboard.writeText(value);
    setOpen(false);
  };

  return (
    <div className="relative">
      {variant === "button" ? (
        <Button onClick={() => setOpen(!open)} className="bg-[#E5E5E5] text-[#0A0A0A] rounded-xl h-12">
          <Share2 className="w-4 h-4 mr-2" /> Поделиться
        </Button>
      ) : (
        <button onClick={() => setOpen(!open)} className="p-2">
          <Share2 className="w-5 h-5 text-[#FAFAFA]" />
        </button>
      )}
      {open && (
        <div className="absolute bottom-full mb-2 right-0 bg-[#1C1C1E] border border-[#3A3A3C] rounded-xl p-2 flex flex-col gap-1 min-w-[160px] z-50">
          <button onClick={handleCopy} className="flex items-center gap-2 px-3 py-2 text-sm text-[#FAFAFA] hover:bg-[#3A3A3C] rounded-lg">
            <Copy className="w-4 h-4" /> Копировать ссылку
          </button>
          <button className="flex items-center gap-2 px-3 py-2 text-sm text-[#FAFAFA] hover:bg-[#3A3A3C] rounded-lg">
            <MessageCircle className="w-4 h-4" /> Telegram
          </button>
          <button className="flex items-center gap-2 px-3 py-2 text-sm text-[#FAFAFA] hover:bg-[#3A3A3C] rounded-lg">
            <Link className="w-4 h-4" /> WhatsApp
          </button>
        </div>
      )}
    </div>
  );
};
```

### 3.11 ReferralContact

Список контактов для приглашения с поиском. Используется на Referal экранах.

```tsx
import { useState } from "react";
import { Input, Avatar } from "@rollout/ui-kit";
import { Search } from "lucide-react";

interface Contact {
  id: string;
  name: string;
  avatar?: string;
  selected?: boolean;
}

interface ReferralContactProps {
  contacts: Contact[];
  onSelect: (id: string) => void;
}

export const ReferralContact = ({ contacts, onSelect }: ReferralContactProps) => {
  const [query, setQuery] = useState("");
  const filtered = contacts.filter(c => c.name.toLowerCase().includes(query.toLowerCase()));

  return (
    <div className="flex flex-col gap-4">
      <div className="relative">
        <Search className="absolute left-4 top-1/2 -translate-y-1/2 w-4 h-4 text-[#A1A1AA]" />
        <Input
          value={query}
          onChange={e => setQuery(e.target.value)}
          placeholder="Поиск по имени"
          className="pl-10 h-12 bg-[#1C1C1E] border-[#3A3A3C] rounded-xl text-[#FAFAFA] placeholder:text-[#A1A1AA]"
        />
      </div>
      <div className="flex flex-col gap-2">
        {filtered.map(contact => (
          <div
            key={contact.id}
            onClick={() => onSelect(contact.id)}
            className="flex items-center gap-3 p-3 rounded-xl hover:bg-[#1C1C1E] cursor-pointer"
          >
            <Avatar src={contact.avatar} fallback={contact.name[0]} className="w-10 h-10" />
            <Typography className="text-sm text-[#FAFAFA]">{contact.name}</Typography>
          </div>
        ))}
      </div>
    </div>
  );
};
```

### 3.12 SocialMediaGroup

Горизонтальная группа иконок соцсетей для шеринга.

```tsx
interface Social {
  id: string;
  icon: React.ReactNode;
  label: string;
  onClick: () => void;
}

interface SocialMediaGroupProps {
  items: Social[];
  vertical?: boolean;
}

export const SocialMediaGroup = ({ items, vertical = false }: SocialMediaGroupProps) => (
  <div className={cn(
    "flex gap-4",
    vertical ? "flex-col" : "flex-row"
  )}>
    {items.map(s => (
      <button
        key={s.id}
        onClick={s.onClick}
        className={cn(
          "flex items-center gap-2 text-[#FAFAFA] hover:bg-[#1C1C1E] rounded-xl transition-colors",
          vertical ? "px-3 py-2" : "flex-col gap-1"
        )}
      >
        <div className="w-10 h-10 rounded-full bg-[#3A3A3C] flex items-center justify-center">
          {s.icon}
        </div>
        <Typography className="text-xs text-[#A1A1AA]">{s.label}</Typography>
      </button>
    ))}
  </div>
);
```

### 3.13 LoyaltyDrawer

Drawer/bottom-sheet с информацией о баллах и шагах. Используется на Referal.

```tsx
interface LoyaltyDrawerProps {
  pointsYou: number;
  pointsFriend: number;
  steps: { number: number; title: string; description: string }[];
}

export const LoyaltyDrawer = ({ pointsYou, pointsFriend, steps }: LoyaltyDrawerProps) => (
  <div className="bg-[#1C1C1E] border border-[#3A3A3C] rounded-2xl p-5 flex flex-col gap-5">
    <div className="flex gap-8">
      <PointsDisplay value={pointsYou} label="Вам" />
      <PointsDisplay value={pointsFriend} label="Другу" />
    </div>
    <Button variant="outline" className="w-fit h-10 rounded-full bg-[#3A3A3C] text-[#FAFAFA] border-none">
      Вопросы и ответы <ChevronRight className="w-4 h-4 ml-1" />
    </Button>
    <div className="flex flex-col gap-4">
      {steps.map(step => (
        <div key={step.number} className="flex gap-4">
          <div className="w-8 h-8 rounded-full bg-[#3A3A3C] flex items-center justify-center text-sm text-[#FAFAFA] font-medium shrink-0">
            {step.number}
          </div>
          <div className="flex flex-col">
            <Typography className="text-sm text-[#FAFAFA]">{step.title}</Typography>
            <Typography className="text-xs text-[#A1A1AA]">{step.description}</Typography>
          </div>
        </div>
      ))}
    </div>
  </div>
);
```

### 3.14 LotteryCard

Карточка приза в лотерее. Используется на Referal2 (призы).

```tsx
interface LotteryCardProps {
  image: string;
  discount: string;
  title: string;
}

export const LotteryCard = ({ image, discount, title }: LotteryCardProps) => (
  <div className="flex flex-col gap-2">
    <div className="rounded-2xl overflow-hidden aspect-square bg-[#1C1C1E]">
      <img src={image} alt={title} className="w-full h-full object-cover" />
    </div>
    <Typography className="text-lg font-bold text-[#FAFAFA]">{discount}</Typography>
    <Typography className="text-sm text-[#A1A1AA]">{title}</Typography>
  </div>
);
```

### 3.15 PromoCodeInput

Поле с промокодом и кнопкой копирования. Используется на Offerwall.

```tsx
import { useState } from "react";
import { Copy, Check } from "lucide-react";

interface PromoCodeInputProps {
  code: string;
}

export const PromoCodeInput = ({ code }: PromoCodeInputProps) => {
  const [copied, setCopied] = useState(false);

  const handleCopy = () => {
    navigator.clipboard.writeText(code);
    setCopied(true);
    setTimeout(() => setCopied(false), 2000);
  };

  return (
    <button
      onClick={handleCopy}
      className="flex items-center gap-3 w-full bg-[#1C1C1E] border border-[#3A3A3C] rounded-xl px-4 py-3"
    >
      {copied ? <Check className="w-4 h-4 text-[#22C55E]" /> : <Copy className="w-4 h-4 text-[#A1A1AA]" />}
      <Typography className="text-sm text-[#FAFAFA] flex-1 text-center">{code}</Typography>
    </button>
  );
};
```

---

## 4. Структура экранов

### 4.1 Main (`/loyalty/main`)

**Секции (сверху вниз):**
1. **Header** — `<` Назад + «Кэшбэк и бонусы» + 🔍
2. **BalanceCard** — `LoyaltyCard`: «Накопили в этом месяце» / 532 ₽ / «Зачислим 31 января»
3. **ActionCards** — 2 колонки:
   - «Повышенный кэшбэк в январе» + кнопка «Выбрать»
   - «Бонусы от партнёров» + аватарки партнёров (Globus, XFit, Yves Rocher)
4. **ReferralBlock** — «Приглашайте друзей» + 2 колонки:
   - «Делитесь промокодом» / «Получите 500 ₽» + аватар
   - «Другу — кофе» / «Вам скидка» + аватар
5. **ProductButton** — Full-width кнопка «Выбрать продукт»
6. **CashbackDay** — `LoyaltyCard`: «Кэшбэк дня» / «Выгодное сегодня» + аватарки партнёров
7. **ElevatedCashback** — «Повышенный кэшбэк» + `>` + горизонтальный скролл `CashbackCategory`
   - Пятёрочка: -10%
   - Глобус: -15%
   - Yves Rocher: -25%
8. **FAQ** — «Часто задаваемые вопросы» + `FAQAccordion` + кнопка «Смотреть все»
9. **Footer** — копирайт + соцсети

### 4.2 Referal1 (`/loyalty/referal1`)

**Секции:**
1. **Header** — `<` + «История рекомендаций»
2. **Tabs** — `Tabs` с 3 вкладками: «В ожидании», «Успешно» (с Badge count=3), «Отклонено»
3. **ReferralList** — список `ReferralContact`-строк:
   - Аватар + «От [Имя]» + тип карты + баллы (зелёным)
4. **Footer**

**Данные строки:**
- Аватар 40px, border-radius full
- Имя: `text-sm font-medium text-[#FAFAFA]`
- Подпись: `text-sm text-[#A1A1AA]`
- Баллы: `text-sm text-[#22C55E]` (справа)
- Фон строки: прозрачный или `#1C1C1E` rounded-2xl

### 4.3 Referal2 (`/loyalty/referal2`)

**Секции:**
1. **Header** — `<` + «Призы»
2. **PointsTabs** — `Tabs`: «40 баллов» | «Призы»
3. **LotterySection** — Изображение конфет/призов + заголовок + описание
4. **PrizesGrid** — Сетка 2×N `LotteryCard` с товарами (15% скидка на электронику)
5. **CTA** — Sticky bottom кнопка «Крутить барабан»
6. **Footer**

### 4.4 Offerwall (`/loyalty/offerwall`)

**Секции:**
1. **Header** — `<` + «Yves Rocher»
2. **OfferHeader** — 25% / «Кэшбэк на покупки онлайн» + Badge «До 15 января»
3. **HeroImage** — Баннер магазина, aspect-ratio 16:9, rounded-2xl
4. **BrandInfo** — Логотип + название + описание
5. **HowToBuy** — «Как купить со скидкой» + `PromoCodeInput`
6. **Steps** — 2 шага с иконками:
   - Выберите товар в онлайн-магазине
   - Введите промокод при оплате
7. **CTA** — Full-width кнопка «К покупкам»
8. **Footer**

### 4.5 Compensation (`/loyalty/compensation`)

**Состояния:**

#### A. Список платежей (`/loyalty/compensation`)
1. **Header** — `<` + «Выбор платежа»
2. **Title** — «Выберите платёж, чтобы вернуть деньги»
3. **InfoLink** — `?` + «Как это работает»
4. **PaymentList** — список карточек платежей:
   - Дата группировки (10 января, Сб)
   - Иконка категории (40px, `#3A3A3C` bg, rounded-xl) + название + категория + сумма справа
   - Подпись: «Компенсация доступна до [дата]»
5. **Footer**

#### B. Форма компенсации (`/loyalty/compensation/:id` или drawer)
1. **Header** — `<` + «Компенсация по платежу»
2. **PaymentInfo** — название / сумма / срок
3. **CompensationForm** — см. компонент выше
4. **Footer**

### 4.6 Categories (`/loyalty/catergories`)

1. **Header** — `<` + «На декабрь»
2. **Title** — «Выберите 3 категории»
3. **Subtitle** — «После подтверждения изменить категории можно только в следующем месяце»
4. **CategoryGrid** — список с чекбоксами + цветные иконки:
   - Супермаркеты (жёлтый, 🛒)
   - Доставка еды (оранжевый, 🍔)
   - Кафе и рестораны (оранжевый, 🍴)
   - Такси и каршеринг (фиолетовый, 🚕)
   - Путешествие (зелёный, 🧳)
   - Коммунальные услуги (голубой, 🏠)
   - Автосервис (синий, 🔧)
   - Дом и ремонт (тёмно-синий, 🛠️)
   - Зоотовары (розовый, 🐾)
5. **CTA** — Sticky bottom кнопка «Подтвердить» (disabled до выбора 3)
6. **Footer**

---

## 5. Пример полной страницы (`LoyaltyMainPage.tsx`)

```tsx
import { useState } from "react";
import { useNavigate } from "react-router-dom";
import { Button, Card, Badge, Tabs, Typography, cn } from "@rollout/ui-kit";
import { ArrowLeft, Search, ChevronRight, Copy } from "lucide-react";
import { LoyaltyCard } from "../components/LoyaltyCard";
import { CashbackCategory } from "../components/CashbackCategory";
import { FAQAccordion } from "../components/FAQAccordion";

const FAQ_ITEMS = [
  { question: "Как получить кэшбэк?", answer: "Кэшбэк начисляется автоматически после совершения покупки в выбранной категории." },
  { question: "С какой даты будет начисляться кэшбэк?", answer: "Кэшбэк начисляется с 1-го числа каждого месяца." },
  { question: "За что начисляется кэшбэк?", answer: "За покупки в выбранных категориях и у партнёров программы." },
  { question: "Сколько кэшбэка можно получить за месяц?", answer: "Максимальный размер кэшбэка зависит от вашей категории." },
];

const CASHBACK_ITEMS = [
  { image: "/brands/pyaterochka.png", discount: "-10%", name: "Пятёрочка дост..." },
  { image: "/brands/globus.png", discount: "-15%", name: "Глобус" },
  { image: "/brands/yves-rocher.png", discount: "-25%", name: "Yves Rocher" },
];

export const LoyaltyMainPage = () => {
  const navigate = useNavigate();

  return (
    <div className="mx-auto flex max-w-[576px] flex-col gap-7 pt-20 pb-8 px-4">
      {/* Header */}
      <div className="flex items-center justify-between">
        <button onClick={() => navigate(-1)} className="p-2">
          <ArrowLeft className="w-6 h-6 text-[#FAFAFA]" />
        </button>
        <Typography className="text-xl font-semibold text-[#FAFAFA]">Кэшбэк и бонусы</Typography>
        <button className="p-2">
          <Search className="w-6 h-6 text-[#FAFAFA]" />
        </button>
      </div>

      {/* Balance Card */}
      <LoyaltyCard
        title="Накопили в этом месяце"
        value="532 ₽"
        subtitle="Зачислим 31 января"
      />

      {/* Action Cards */}
      <div className="grid grid-cols-2 gap-4">
        <Card className="bg-[#1C1C1E] border-[#3A3A3C] rounded-2xl p-5 flex flex-col justify-between min-h-[140px]">
          <Typography className="text-base font-medium text-[#FAFAFA]">
            Повышенный кэшбэк в январе
          </Typography>
          <Button className="w-fit h-10 bg-[#E5E5E5] text-[#0A0A0A] rounded-xl text-sm font-medium">
            Выбрать
          </Button>
        </Card>
        <Card className="bg-[#1C1C1E] border-[#3A3A3C] rounded-2xl p-5 flex flex-col justify-between min-h-[140px]">
          <Typography className="text-base font-medium text-[#FAFAFA]">
            Бонусы от партнёров
          </Typography>
          <div className="flex -space-x-2">
            <img src="/brands/globus.png" alt="" className="w-8 h-8 rounded-full border-2 border-[#1C1C1E]" />
            <img src="/brands/xfit.png" alt="" className="w-8 h-8 rounded-full border-2 border-[#1C1C1E]" />
            <img src="/brands/yves-rocher.png" alt="" className="w-8 h-8 rounded-full border-2 border-[#1C1C1E]" />
          </div>
        </Card>
      </div>

      {/* Referral Block */}
      <div className="flex flex-col gap-4">
        <Typography className="text-lg font-medium text-[#FAFAFA]">Приглашайте друзей</Typography>
        <div className="grid grid-cols-2 gap-4">
          <Card className="bg-[#1C1C1E] border-[#3A3A3C] rounded-2xl p-5 flex flex-col gap-2">
            <Typography className="text-base font-medium text-[#FAFAFA]">Делитесь промокодом</Typography>
            <Typography className="text-sm text-[#A1A1AA]">Получите 500 ₽</Typography>
            <img src="/avatars/user1.png" alt="" className="w-10 h-10 rounded-full mt-2" />
          </Card>
          <Card className="bg-[#1C1C1E] border-[#3A3A3C] rounded-2xl p-5 flex flex-col gap-2">
            <Typography className="text-base font-medium text-[#FAFAFA]">Другу — кофе</Typography>
            <Typography className="text-sm text-[#A1A1AA]">Вам скидка</Typography>
            <img src="/avatars/user2.png" alt="" className="w-10 h-10 rounded-full mt-2" />
          </Card>
        </div>
      </div>

      <Button className="w-full h-14 bg-[#E5E5E5] text-[#0A0A0A] rounded-xl font-medium">
        Выбрать продукт
      </Button>

      {/* Cashback Day */}
      <LoyaltyCard
        title="Кэшбэк дня"
        subtitle="Выгодное сегодня"
      >
        <div className="flex -space-x-2 mt-2">
          <img src="/brands/globus.png" alt="" className="w-8 h-8 rounded-full border-2 border-[#1C1C1E]" />
          <img src="/brands/xfit.png" alt="" className="w-8 h-8 rounded-full border-2 border-[#1C1C1E]" />
          <img src="/brands/yves-rocher.png" alt="" className="w-8 h-8 rounded-full border-2 border-[#1C1C1E]" />
          <img src="/brands/more.png" alt="" className="w-8 h-8 rounded-full border-2 border-[#1C1C1E]" />
        </div>
      </LoyaltyCard>

      {/* Elevated Cashback */}
      <div className="flex flex-col gap-4">
        <div className="flex items-center justify-between">
          <Typography className="text-lg font-medium text-[#FAFAFA]">Повышенный кэшбэк</Typography>
          <ChevronRight className="w-5 h-5 text-[#A1A1AA]" />
        </div>
        <div className="flex gap-4 overflow-x-auto pb-2 -mx-4 px-4 scrollbar-hide">
          {CASHBACK_ITEMS.map((item, idx) => (
            <CashbackCategory key={idx} {...item} />
          ))}
        </div>
      </div>

      {/* FAQ */}
      <div className="flex flex-col gap-4">
        <Typography className="text-lg font-medium text-[#FAFAFA]">Часто задаваемые вопросы</Typography>
        <FAQAccordion items={FAQ_ITEMS} />
        <Button variant="outline" className="w-full h-14 bg-[#1C1C1E] border-[#3A3A3C] text-[#FAFAFA] rounded-xl">
          Смотреть все
        </Button>
      </div>

      {/* Footer */}
      <div className="flex items-center justify-between pt-4 border-t border-[#3A3A3C]">
        <Typography className="text-xs text-[#A1A1AA]">© 2025 Rollout</Typography>
        <div className="flex gap-4">
          <a href="#" className="text-[#A1A1AA] hover:text-[#FAFAFA]">
            <svg className="w-5 h-5" fill="currentColor" viewBox="0 0 24 24"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>
          </a>
          <a href="#" className="text-[#A1A1AA] hover:text-[#FAFAFA]">
            <svg className="w-5 h-5" fill="currentColor" viewBox="0 0 24 24"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 01-2.063-2.065 2.064 2.064 0 112.063 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>
          </a>
          <a href="#" className="text-[#A1A1AA] hover:text-[#FAFAFA]">
            <svg className="w-5 h-5" fill="currentColor" viewBox="0 0 24 24"><path d="M11.944 0A12 12 0 0 0 0 12a12 12 0 0 0 12 12 12 12 0 0 0 12-12A12 12 0 0 0 12 0a12 12 0 0 0-.056 0zm4.962 7.224c.1-.002.321.023.465.14a.506.506 0 0 1 .171.325c.016.093.036.306.02.472-.18 1.898-.962 6.502-1.36 8.627-.168.9-.499 1.201-.82 1.23-.696.065-1.225-.46-1.9-.902-1.056-.693-1.653-1.124-2.678-1.8-1.185-.78-.417-1.21.258-1.91.177-.184 3.247-2.977 3.307-3.23.007-.032.014-.15-.056-.212s-.174-.041-.249-.024c-.106.024-1.793 1.14-5.061 3.345-.479.33-.913.49-1.302.48-.428-.008-1.252-.241-1.865-.44-.752-.245-1.349-.374-1.297-.789.027-.216.325-.437.893-.663 3.498-1.524 5.83-2.529 6.998-3.014 3.332-1.386 4.025-1.627 4.476-1.635z"/></svg>
          </a>
        </div>
      </div>
    </div>
  );
};
```

---

## 6. Чек-лист

### Роутинг
- [ ] `/loyalty/main` — главная страница кэшбэка
- [ ] `/loyalty/referal1` — история рекомендаций
- [ ] `/loyalty/referal2` — лотерея/призы
- [ ] `/loyalty/offerwall` — детальная страница оффера
- [ ] `/loyalty/compensation` — выбор платежа для компенсации
- [ ] `/loyalty/catergories` — выбор категорий (опечатка в роуте сохранена)

### Компоненты
- [ ] `LoyaltyCard` — универсальная карточка баланса/инфо
- [ ] `CashbackCategory` — карточка категории в скролле
- [ ] `ReferralProgress` — прогресс-бар с шагами
- [ ] `QRCodeDisplay` — QR-код с кнопкой шеринга
- [ ] `FAQAccordion` — аккордеон вопросов
- [ ] `OfferItem` — карточка партнёрского оффера
- [ ] `CompensationForm` — форма со слайдером и выбором карты
- [ ] `CategoryGrid` — сетка категорий с чекбоксами (max 3)
- [ ] `PointsDisplay` — отображение баллов
- [ ] `ShareButton` — кнопка шеринга с дропдауном
- [ ] `ReferralContact` — список контактов с поиском
- [ ] `SocialMediaGroup` — группа иконок соцсетей
- [ ] `LoyaltyDrawer` — drawer с баллами и шагами
- [ ] `LotteryCard` — карточка приза лотереи
- [ ] `PromoCodeInput` — поле промокода с копированием

### Стилистика
- [ ] Фон `#0A0A0A` на всех страницах
- [ ] Карточки `#1C1C1E` с бордером `#3A3A3C`, radius 16px
- [ ] Primary кнопка: светло-серый `#E5E5E5`, radius 12px
- [ ] Input: `#1C1C1E` bg, `#3A3A3C` border, radius 12px
- [ ] Текст `#FAFAFA` primary, `#A1A1AA` secondary
- [ ] Контейнер `max-w-[576px] mx-auto`
- [ ] Горизонтальный скролл категорий: `overflow-x-auto` + `scrollbar-hide`
- [ ] Зелёный акцент `#22C55E` для баллов и успешных статусов
- [ ] Badge с deadline: белый bg, чёрный текст, rounded-full
- [ ] Avatar: 40px, rounded-full, возможно с border

### Функциональность
- [ ] useState для управления формами (компенсация, категории, поиск)
- [ ] useState для открытия/закрытия аккордеона
- [ ] useState для копирования промокода (feedback: Check иконка)
- [ ] useState для dropdown шеринга
- [ ] Tabs для фильтрации рефералов (3 состояния)
- [ ] Tabs для лотереи (баллы/призы)
- [ ] Checkbox: max 3 выбора на Categories, disabled CTA до лимита
- [ ] Slider для Compensation (связь бонусов и рублей)
- [ ] Кнопка «Подтвердить» на Categories: disabled state
- [ ] Sticky bottom кнопки на Categories и Referal2
- [ ] Footer с соцсетями на всех экранах
- [ ] Иконки из `lucide-react` (ArrowLeft, Search, ChevronRight, ChevronDown, Copy, Check, Share2, MessageCircle, Link, Plus, Home, Smartphone, Search)

### Адаптивность
- [ ] Grid 2 колонки для карточек действий и рефералов
- [ ] Grid 2 колонки для LotteryCard на Referal2
- [ ] Горизонтальный скролл для CashbackCategory
- [ ] Корректное отображение на мобильных (max-width 576px centered)

### Импорты
- [ ] UI-кит: `Button, Input, Field, Select, Tabs, Card, Badge, Avatar, Separator, Typography, cn`
- [ ] Base UI: `Accordion, Slider` (при необходимости)
- [ ] Lucide иконки
- [ ] `react-router-dom` для навигации
