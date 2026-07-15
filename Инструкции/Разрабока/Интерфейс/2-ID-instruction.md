# Инструкция по вёрстке модуля 2-ID

> Статус: готово к реализации  
> Скриншотов проанализировано: **16**  
> Дата: 2025-07-13

---

## 1. Обзор модуля

### Назначение
Модуль **2-ID** — это блок идентификации пользователя в финансовом приложении Rollout. Пользователь последовательно заполняет анкету из нескольких шагов: контакты, персональные данные, информация о работе, доходах, дополнительная информация, объект страхования (при необходимости), данные по ипотеке (при необходимости). По завершении заполнения отображается экран сводки (Summary) с перечислением всех разделов и кнопкой отправки заявки. Также в модуле присутствует раздел «Уровни кошелька» — таблица сравнения лимитов и возможностей для анонимного, именного и идентифицированного кошелька.

### Экраны модуля (из Figma)

| # | Страница Figma | Компонент | Файл скриншота |
|---|----------------|-----------|----------------|
| 1 | Cover | 2. ID | `78_6006.png` |
| 2 | ↳ Contacts | ContactsTemplate | `660_60934.png` |
| 3 | ↳ Personal data | PersonalDataTemplate/Default | `660_61328.png` |
| 4 | ↳ Job | JobTemplate | `660_64724.png` |
| 5 | ↳ Income | IncomeTemplate/Default | `660_71735.png` |
| 6 | ↳ Additional | AdditionalInfoTemplate/Default | `660_73588.png` |
| 7 | ↳ Object of Insurance | ObjectOfInsuranceTemplate | `941_103321.png` |
| 8 | ↳ 🚧 Mortgage | Mortgage info | `944_41155.png` |
| 9 | ↳ Loan | MortgageSummary_Template/Default | `660_60487.png` |
| 10 | ↳ Credit Card | CreditCard_Summary_Template/Default | `1007_13871.png` |
| 11 | ↳ Car Loan | CarLoan_Summary_Template | `1009_17261.png` |
| 12 | ↳ Micro Loan | MicroLoan_Summary_Template | `1011_16122.png` |
| 13 | ↳ Instalment | Instalment_Summary_Template | `1012_19602.png` |
| 14 | ↳ Mortgage | Mortgage_Summary_Template | `1055_34175.png` |
| 15 | ↳ Mortgage Insurance | Mortgage_Insurance_Summary_Template | `1055_35889.png` |
| 16 | ↳ 🚧 Identy Leveles | ID_Levels_Form_Desktop | `443_106092.png` |

### Роуты (URL-пути)

```tsx
// Forms
/id/contacts
/id/personal-data
/id/job
/id/income
/id/additional
/id/object-of-insurance
/id/mortgage

// Summary
/id/summary/loan
/id/summary/credit-card
/id/summary/car-loan
/id/summary/micro-loan
/id/summary/instalment
/id/summary/mortgage
/id/summary/mortgage-insurance

// Wallet levels
/id/wallet-levels
```

---

## 2. Дизайн-система

### Цвета

| Токен | Значение | Использование |
|-------|----------|---------------|
| `bg-primary` | `#0A0A0A` | Фон страницы |
| `bg-card` | `#1C1C1E` | Карточки, кнопки, контейнеры иконок |
| `bg-input` | `#FAFAFA` | Фон input-полей (светлые на тёмном фоне) |
| `bg-input-dark` | `#1C1C1E` | Альтернативный input на карточке |
| `border-default` | `#3A3A3C` | Границы карточек, разделители |
| `border-input` | `#E5E5E5` | Граница input-полей в состоянии default |
| `text-primary` | `#FAFAFA` | Основной текст, заголовки |
| `text-secondary` | `#8E8E93` | Подписи, placeholder в тёмных инпутах |
| `text-muted` | `#636366` | Placeholder в светлых инпутах |
| `text-inverse` | `#0A0A0A` | Текст на светлых/primary кнопках |
| `accent-success` | `#34C759` | Бейдж «Заполнен», галочки |
| `accent-purple` | `#8B5CF6` | Акцент колонки в таблице уровней |
| `accent-error` | `#FF3B30` | Крестики в таблице уровней |

### Типографика

| Элемент | Размер | Вес | Line-height | Цвет |
|---------|--------|-----|-------------|------|
| Page Title | `24px` | `600` | `1.2` | `text-primary` |
| Section Label | `14px` | `400` | `1.4` | `text-secondary` |
| Input Text | `16px` | `400` | `1.5` | `text-inverse` (в светлом input) |
| Input Placeholder | `16px` | `400` | `1.5` | `text-muted` |
| Button Text | `16px` | `500` | `1.0` | `text-primary` |
| List Item Title | `16px` | `500` | `1.3` | `text-primary` |
| List Item Subtitle | `14px` | `400` | `1.3` | `text-secondary` |
| Badge Text | `12px` | `500` | `1.0` | `accent-success` |
| Caption / Hint | `13px` | `400` | `1.4` | `text-secondary` |

> Шрифт: `Geist Variable` (уже подключён глобально).

### Спейсинг

| Токен | Значение |
|-------|----------|
| `page-padding-x` | `16px` (mobile), `24px` (desktop) |
| `section-gap` | `24px` |
| `input-gap` | `16px` |
| `input-padding` | `16px 20px` |
| `input-height` | `56px` |
| `button-height` | `56px` |
| `card-padding` | `20px` |
| `card-radius` | `16px` |
| `input-radius` | `12px` |
| `button-radius` | `12px` |
| `icon-container-size` | `48px` |
| `icon-radius` | `12px` |

### Иконки

- **Библиотека:** `lucide-react`
- **Размер:** `size-5` (20px) для inline, `size-6` (24px) для контейнеров
- **Цвет:** `currentColor` (наследует от родителя)
- **Контейнер иконки:** `w-12 h-12 rounded-xl bg-bg-card flex items-center justify-center`
- **Используемые иконки:**
  - `Mail` — контакты
  - `User` — персональные данные
  - `Phone` — телефон
  - `Users` — работа
  - `Wallet` — доход
  - `MoreHorizontal` — дополнительно
  - `Building` — ипотека / объект страхования
  - `Check` — заполнено / доступно
  - `X` — недоступно
  - `ChevronRight` — переход в раздел
  - `ChevronDown` — селект
  - `Search` — поисковые поля

---

## 3. Компоненты UI

### 3.1. PageContainer

Обертка для всех страниц модуля.

```tsx
import { cn } from "@rollout/ui-kit";

interface PageContainerProps {
  children: React.ReactNode;
  className?: string;
}

export function PageContainer({ children, className }: PageContainerProps) {
  return (
    <div
      className={cn(
        "mx-auto flex max-w-[576px] flex-col gap-7 pt-20 pb-8",
        className
      )}
    >
      {children}
    </div>
  );
}
```

> Референс shadcn: базовый layout-компонент (не требует отдельного shadcn-компонента).

---

### 3.2. FormSection

Группировка полей формы с заголовком.

```tsx
import { Typography } from "@rollout/ui-kit";
import { cn } from "@rollout/ui-kit";

interface FormSectionProps {
  title: string;
  children: React.ReactNode;
  className?: string;
}

export function FormSection({ title, children, className }: FormSectionProps) {
  return (
    <div className={cn("flex flex-col gap-4", className)}>
      <Typography variant="body" className="text-[#8E8E93]">
        {title}
      </Typography>
      <div className="flex flex-col gap-4">{children}</div>
    </div>
  );
}
```

> Референс shadcn: `Card` (как структурный контейнер), но здесь — кастомная группировка.

---

### 3.3. FormInput

Поле ввода для форм модуля 2-ID.

**Особенности:** светлый фон (`#FAFAFA`) на тёмной странице, тёмный текст, серый placeholder.

```tsx
import { Input, cn } from "@rollout/ui-kit";
import { Search } from "lucide-react";

interface FormInputProps extends React.InputHTMLAttributes<HTMLInputElement> {
  icon?: "search";
  error?: string;
}

export function FormInput({
  icon,
  error,
  className,
  ...props
}: FormInputProps) {
  return (
    <div className="flex flex-col gap-1.5">
      <div className="relative">
        {icon === "search" && (
          <Search className="absolute left-4 top-1/2 size-5 -translate-y-1/2 text-[#636366]" />
        )}
        <Input
          className={cn(
            "h-14 rounded-xl border border-[#E5E5E5] bg-[#FAFAFA] px-5 text-base text-[#0A0A0A] placeholder:text-[#636366] focus-visible:ring-1 focus-visible:ring-[#3A3A3C]",
            icon === "search" && "pl-12",
            className
          )}
          {...props}
        />
      </div>
      {error && (
        <span className="text-sm text-[#FF3B30]">{error}</span>
      )}
    </div>
  );
}
```

> Референс shadcn: `Input` → реализация через `@rollout/ui-kit` / `@base-ui/react` Field + Input.

---

### 3.4. FormSelect

Селект с выпадающим списком.

```tsx
import { Select, cn } from "@rollout/ui-kit";
import { ChevronDown } from "lucide-react";

interface FormSelectProps {
  placeholder?: string;
  value?: string;
  options: { value: string; label: string }[];
  onChange: (value: string) => void;
  className?: string;
}

export function FormSelect({
  placeholder = "Выберите",
  value,
  options,
  onChange,
  className,
}: FormSelectProps) {
  return (
    <Select value={value} onValueChange={onChange}>
      <Select.Trigger
        className={cn(
          "flex h-14 w-full items-center justify-between rounded-xl border border-[#E5E5E5] bg-[#FAFAFA] px-5 text-base focus:outline-none focus:ring-1 focus:ring-[#3A3A3C]",
          !value && "text-[#636366]",
          value && "text-[#0A0A0A]",
          className
        )}
      >
        <Select.Value placeholder={placeholder} />
        <ChevronDown className="size-5 text-[#636366]" />
      </Select.Trigger>
      <Select.Content className="rounded-xl border border-[#3A3A3C] bg-[#1C1C1E] p-1">
        {options.map((opt) => (
          <Select.Item
            key={opt.value}
            value={opt.value}
            className="rounded-lg px-4 py-3 text-[#FAFAFA] hover:bg-[#3A3A3C] focus:bg-[#3A3A3C]"
          >
            {opt.label}
          </Select.Item>
        ))}
      </Select.Content>
    </Select>
  );
}
```

> Референс shadcn: `Select` → `@rollout/ui-kit` Select.

---

### 3.5. FormCheckbox

Чекбокс с подписью.

```tsx
import { Checkbox, cn } from "@rollout/ui-kit";

interface FormCheckboxProps {
  checked: boolean;
  onCheckedChange: (checked: boolean) => void;
  label: string;
  className?: string;
}

export function FormCheckbox({
  checked,
  onCheckedChange,
  label,
  className,
}: FormCheckboxProps) {
  return (
    <label
      className={cn(
        "flex cursor-pointer items-center gap-3",
        className
      )}
    >
      <Checkbox
        checked={checked}
        onCheckedChange={onCheckedChange}
        className="size-6 rounded-md border-[#3A3A3C] data-[state=checked]:bg-[#34C759] data-[state=checked]:border-[#34C759]"
      />
      <span className="text-sm text-[#FAFAFA]">{label}</span>
    </label>
  );
}
```

> Референс shadcn: `Checkbox` → `@rollout/ui-kit` Checkbox.

---

### 3.6. PrimaryButton

Основная кнопка (full-width, тёмный фон).

```tsx
import { Button, cn } from "@rollout/ui-kit";

interface PrimaryButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  children: React.ReactNode;
  loading?: boolean;
}

export function PrimaryButton({
  children,
  loading,
  className,
  disabled,
  ...props
}: PrimaryButtonProps) {
  return (
    <Button
      className={cn(
        "h-14 w-full rounded-xl bg-[#1C1C1E] text-base font-medium text-[#FAFAFA] hover:bg-[#2C2C2E] disabled:opacity-50",
        className
      )}
      disabled={disabled || loading}
      {...props}
    >
      {loading ? "Загрузка..." : children}
    </Button>
  );
}
```

> Референс shadcn: `Button` → `@rollout/ui-kit` Button.

---

### 3.7. SecondaryButton

Вторичная кнопка (для «Заполнить через Госуслуги», «Сохранить и продолжить»).

```tsx
import { Button, cn } from "@rollout/ui-kit";

interface SecondaryButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  children: React.ReactNode;
  variant?: "outline" | "ghost";
}

export function SecondaryButton({
  children,
  variant = "outline",
  className,
  ...props
}: SecondaryButtonProps) {
  return (
    <Button
      variant={variant}
      className={cn(
        "h-14 w-full rounded-xl text-base font-medium",
        variant === "outline" &&
          "border border-[#3A3A3C] bg-transparent text-[#FAFAFA] hover:bg-[#1C1C1E]",
        variant === "ghost" &&
          "bg-transparent text-[#FAFAFA] hover:bg-[#1C1C1E]",
        className
      )}
      {...props}
    >
      {children}
    </Button>
  );
}
```

> Референс shadcn: `Button` (variant outline / ghost).

---

### 3.8. SummaryListItem

Элемент списка на экранах Summary.

```tsx
import { cn } from "@rollout/ui-kit";
import { ChevronRight } from "lucide-react";
import type { LucideIcon } from "lucide-react";

interface SummaryListItemProps {
  icon: LucideIcon;
  title: string;
  subtitle?: string;
  status?: "filled" | "empty";
  onClick?: () => void;
}

export function SummaryListItem({
  icon: Icon,
  title,
  subtitle,
  status,
  onClick,
}: SummaryListItemProps) {
  return (
    <button
      onClick={onClick}
      className="flex w-full items-center gap-4 rounded-2xl bg-[#1C1C1E] p-4 text-left transition-colors hover:bg-[#2C2C2E]"
    >
      <div className="flex h-12 w-12 shrink-0 items-center justify-center rounded-xl bg-[#2C2C2E]">
        <Icon className="size-6 text-[#FAFAFA]" />
      </div>
      <div className="flex min-w-0 flex-1 flex-col">
        <span className="text-base font-medium text-[#FAFAFA]">{title}</span>
        {subtitle && (
          <span
            className={cn(
              "text-sm",
              status === "filled" ? "text-[#34C759]" : "text-[#8E8E93]"
            )}
          >
            {subtitle}
          </span>
        )}
      </div>
      <ChevronRight className="size-5 shrink-0 text-[#8E8E93]" />
    </button>
  );
}
```

> Референс shadcn: `Card` (как контейнер) + кастомная компоновка.

---

### 3.9. IDLevelsTable

Таблица сравнения уровней кошелька.

```tsx
import { cn } from "@rollout/ui-kit";
import { Check, X } from "lucide-react";

interface LevelColumn {
  name: string;
  highlight?: boolean;
  rows: (string | boolean | React.ReactNode)[];
}

interface IDLevelsTableProps {
  rowLabels: string[];
  columns: LevelColumn[];
}

export function IDLevelsTable({ rowLabels, columns }: IDLevelsTableProps) {
  return (
    <div className="overflow-x-auto">
      <div className="min-w-[600px]">
        {/* Header */}
        <div className="grid grid-cols-4 gap-2">
          <div className="p-4" />
          {columns.map((col) => (
            <div
              key={col.name}
              className={cn(
                "rounded-t-2xl p-4 text-center text-sm font-medium",
                col.highlight
                  ? "bg-[#8B5CF6] text-white"
                  : "bg-[#1C1C1E] text-[#8E8E93]"
              )}
            >
              {col.name}
            </div>
          ))}
        </div>

        {/* Rows */}
        {rowLabels.map((label, i) => (
          <div key={label} className="grid grid-cols-4 gap-2">
            <div className="flex items-center p-4 text-sm text-[#8E8E93]">
              {label}
            </div>
            {columns.map((col) => {
              const cell = col.rows[i];
              return (
                <div
                  key={`${col.name}-${i}`}
                  className={cn(
                    "flex items-center justify-center p-4 text-sm",
                    col.highlight
                      ? "bg-[#8B5CF6]/20 text-[#FAFAFA]"
                      : "bg-[#1C1C1E] text-[#8E8E93]"
                  )}
                >
                  {typeof cell === "boolean" ? (
                    cell ? (
                      <Check className="size-5 text-[#34C759]" />
                    ) : (
                      <X className="size-5 text-[#FF3B30]" />
                    )
                  ) : (
                    cell
                  )}
                </div>
              );
            })}
          </div>
        ))}
      </div>
    </div>
  );
}
```

> Референс shadcn: `Table` → кастомная grid-реализация с карточной стилизацией.

---

### 3.10. StepProgress

Индикатор прогресса по шагам формы.

```tsx
import { cn } from "@rollout/ui-kit";

interface StepProgressProps {
  steps: { label: string; active: boolean; completed: boolean }[];
}

export function StepProgress({ steps }: StepProgressProps) {
  return (
    <div className="flex items-center gap-2">
      {steps.map((step, i) => (
        <div key={i} className="flex items-center gap-2">
          <div
            className={cn(
              "h-2 w-2 rounded-full",
              step.active && "bg-[#FAFAFA]",
              step.completed && "bg-[#34C759]",
              !step.active && !step.completed && "bg-[#3A3A3C]"
            )}
          />
          {i < steps.length - 1 && (
            <div
              className={cn(
                "h-px w-4",
                step.completed ? "bg-[#34C759]" : "bg-[#3A3A3C]"
              )}
            />
          )}
        </div>
      ))}
    </div>
  );
}
```

> Референс shadcn: `Progress` / `Stepper` → кастомная реализация.

---

## 4. Структура каждого экрана

### 4.1. Contacts (`/id/contacts`)

**Скриншот:** `660_60934.png`

**Layout:**
- Контейнер: `PageContainer`
- Шапка: стрелка назад + заголовок «Контакты» + индикатор шагов
- Форма: flex-col gap-6
- Кнопка прижата к низу (или фиксирована внизу экрана)

**Компоненты сверху вниз:**
1. `StepProgress` — шаги анкеты (активный шаг = Contacts)
2. Заголовок «Контакты» (Typography variant="h1")
3. `FormSection` «Телефон»
   - `FormInput` (type="tel", placeholder="+7 (999) 999-99-99")
4. `FormSection` «Email»
   - `FormInput` (type="email", placeholder="ivanov@mail.ru")
5. `PrimaryButton` «Продолжить»

**Состояния:**
- `loading`: кнопка disabled + спиннер
- `error`: под input красный текст ошибки
- `empty`: placeholder visible
- `success`: переход на `/id/personal-data`

**Интерактивность:**
- `onChange` → обновление `useState`
- `onSubmit` → валидация → переход

---

### 4.2. Personal Data (`/id/personal-data`)

**Скриншот:** `660_61328.png`

**Layout:**
- Длинная скроллящаяся форма
- Padding-bottom: `pb-tabbar md:pb-0`

**Компоненты сверху вниз:**
1. `StepProgress`
2. Заголовок «Личные данные»
3. `FormSection` «ФИО»
   - `FormInput` placeholder="Фамилия"
   - `FormInput` placeholder="Имя"
   - `FormInput` placeholder="Отчество"
4. `FormSection` «Паспортные данные»
   - `FormInput` placeholder="Серия и номер паспорта"
   - `FormInput` placeholder="Когда выдан"
   - `FormInput` placeholder="Кем выдан"
5. `FormSection` «Адрес регистрации»
   - `FormInput` icon="search" placeholder="г. Москва, ул. Лесная"
6. `FormSection` «Адрес проживания»
   - `FormCheckbox` label="Совпадает с адресом регистрации"
   - `FormInput` icon="search" placeholder="Населённый пункт"
7. `SecondaryButton` variant="ghost" «Заполнить через Госуслуги»
8. `SecondaryButton` variant="outline" «Сохранить и продолжить»

**Состояния:**
- `loading`: поля disabled
- `error`: подсветка невалидных полей
- `success`: переход на `/id/job`

---

### 4.3. Job (`/id/job`)

**Скриншот:** `660_64724.png`

**Компоненты сверху вниз:**
1. `StepProgress`
2. Заголовок «Работа»
3. `FormSection` «Место работы»
   - `FormInput` icon="search" placeholder="Название компании или ИНН"
   - `FormInput` icon="search" placeholder="г Москва, ул Лесная"
4. `FormInput` placeholder="+7 (000) 000-00-00"
5. `FormSelect` «Должность»
6. `FormSelect` «Стаж»
7. `FormSelect` «Тип занятости»
8. `FormInput` placeholder="Название компании"
9. `FormSelect` «Форма собственности»
10. `FormInput` placeholder="ДД.ММ.ГГГГ" (дата приёма)
11. `PrimaryButton` «Продолжить»

---

### 4.4. Income (`/id/income`)

**Скриншот:** `660_71735.png`

**Компоненты сверху вниз:**
1. `StepProgress`
2. Заголовок «Доход»
3. `FormInput` placeholder="123456 ₽" (ежемесячный доход)
4. `FormInput` placeholder="123456 ₽" (дополнительный доход)
5. `FormSelect` «Источник дополнительного дохода»
6. `FormInput` icon="search" placeholder="Сбер" (основной банк)
7. `FormCheckbox` label="Даю согласие на обработку..."
8. `FormSelect` «Тип занятости»
9. `FormInput` placeholder="10 000 ₽" (ежемесячные расходы)
10. `PrimaryButton` «Продолжить»

---

### 4.5. Additional (`/id/additional`)

**Скриншот:** `660_73588.png`

**Компоненты сверху вниз:**
1. `StepProgress`
2. Заголовок «Дополнительно»
3. `FormSelect` «Образование»
4. `FormInput` placeholder="Количество человек" (в семье)
5. `FormSelect` «Семейное положение»
6. `FormCheckbox` label="Согласие на получение СМС"
7. `FormCheckbox` label="Есть загранпаспорт"
8. `FormCheckbox` label="Водительское удостоверение"
9. `PrimaryButton` «Продолжить»

---

### 4.6. Object of Insurance (`/id/object-of-insurance`)

**Скриншот:** `941_103321.png`

**Компоненты сверху вниз:**
1. `StepProgress`
2. Заголовок «Объект страхования»
3. `FormSelect` «Тип объекта страхования»
4. `FormInput` icon="search" placeholder="г. Москва, ул. Лесная" (адрес)
5. `FormInput` placeholder="0" (площадь)
6. `FormInput` placeholder="ГГГГ" (год постройки)
7. `FormCheckbox` label="Квартира находится на первом или последнем этаже"
8. `FormCheckbox` label="Имеется автоматическая пожарная сигнализация"
9. `PrimaryButton` «Продолжить»

---

### 4.7. Mortgage (`/id/mortgage`)

**Скриншот:** `944_41155.png`

**Компоненты сверху вниз:**
1. `StepProgress`
2. Заголовок «Ипотека»
3. `FormSelect` placeholder="₽" (валюта)
4. `FormInput` placeholder="Номер ипотечного договора"
5. `FormSelect` placeholder="ДД.ММ.ГГГГ" (дата выдачи кредита)
6. `FormSelect` placeholder="ДД.ММ.ГГГГ" (дата окончания)
7. `PrimaryButton` «Продолжить»

---

### 4.8. Summary — Loan (`/id/summary/loan`)

**Скриншот:** `660_60487.png`

**Layout:**
- Список заполненных разделов
- Каждый элемент — карточка с иконкой, заголовком, статусом и стрелкой
- Кнопка «Отправить заявку» фиксирована внизу

**Компоненты сверху вниз:**
1. Заголовок «Рефинансирование кредита»
2. `SummaryListItem` icon={Mail} title="Контакты" subtitle="Заполнен" status="filled"
3. `SummaryListItem` icon={User} title="Личные данные" subtitle="Заполнен" status="filled"
4. `SummaryListItem` icon={Phone} title="Работодатель" subtitle="Заполнен" status="filled"
5. `SummaryListItem` icon={Users} title="Родственник" (без subtitle — пусто)
6. `SummaryListItem` icon={Wallet} title="Доходы" subtitle="Заполнен" status="filled"
7. `SummaryListItem` icon={MoreHorizontal} title="Дополнительно" (без subtitle)
8. `PrimaryButton` «Отправить заявку»

**Состояния:**
- `loading`: кнопка disabled
- `error`: тост / баннер с ошибкой
- `success`: редирект на страницу успеха

**Интерактивность:**
- `onClick` на `SummaryListItem` → навигация к соответствующей форме для редактирования
- `onClick` на кнопку → отправка заявки

---

### 4.9. Summary — Credit Card (`/id/summary/credit-card`)

**Скриншот:** `1007_13871.png`

**Компоненты сверху вниз:**
1. Заголовок «Кредитная карта»
2. `SummaryListItem` icon={Mail} title="Контакты"
3. `SummaryListItem` icon={User} title="Личные данные"
4. `SummaryListItem` icon={Users} title="Работа"
5. `SummaryListItem` icon={Wallet} title="Доходы"
6. `SummaryListItem` icon={MoreHorizontal} title="Дополнительно"
7. `PrimaryButton` «Отправить заявку»

---

### 4.10. Summary — Car Loan (`/id/summary/car-loan`)

**Скриншот:** `1009_17261.png`

**Компоненты:** аналогично Credit Card, но заголовок «Автокредит».

---

### 4.11. Summary — Micro Loan (`/id/summary/micro-loan`)

**Скриншот:** `1011_16122.png`

**Компоненты:**
1. Заголовок «Микрозайм»
2. `SummaryListItem` icon={Mail} title="Контакты"
3. `SummaryListItem` icon={User} title="Личные данные"
4. `SummaryListItem` icon={Wallet} title="Доходы"
5. `PrimaryButton` «Отправить заявку»

---

### 4.12. Summary — Instalment (`/id/summary/instalment`)

**Скриншот:** `1012_19602.png`

**Компоненты:**
1. Заголовок «Рассрочка»
2. `SummaryListItem` icon={Mail} title="Контакты"
3. `SummaryListItem` icon={User} title="Личные данные"
4. `SummaryListItem` icon={Wallet} title="Доходы"
5. `PrimaryButton` «Отправить заявку»

---

### 4.13. Summary — Mortgage (`/id/summary/mortgage`)

**Скриншот:** `1055_34175.png`

**Компоненты:**
1. Заголовок «Ипотека»
2. `SummaryListItem` icon={Mail} title="Контакты"
3. `SummaryListItem` icon={User} title="Личные данные"
4. `SummaryListItem` icon={Phone} title="Работодатель"
5. `SummaryListItem` icon={Users} title="Родственник"
6. `SummaryListItem` icon={Wallet} title="Доходы"
7. `SummaryListItem` icon={MoreHorizontal} title="Дополнительно"
8. `PrimaryButton` «Отправить заявку»

---

### 4.14. Summary — Mortgage Insurance (`/id/summary/mortgage-insurance`)

**Скриншот:** `1055_35889.png`

**Компоненты:**
1. Заголовок «Ипотечное страхование»
2. `SummaryListItem` icon={Mail} title="Контакты"
3. `SummaryListItem` icon={User} title="Личные данные"
4. `SummaryListItem` icon={Building} title="Данные об ипотеке"
5. `SummaryListItem` icon={Users} title="Родственник"
6. `SummaryListItem` icon={Building} title="Данные об объекте страхования"
7. `PrimaryButton` «Отправить заявку»

---

### 4.15. Wallet Levels (`/id/wallet-levels`)

**Скриншот:** `443_106092.png`

**Layout:**
- Полноширинная таблица с горизонтальным скроллом на mobile
- 4 колонки: label, Анонимный (highlight), Именной, Идентифицированный

**Компоненты:**
1. Заголовок «У вас анонимный кошелёк — это минимум»
2. Подзаголовок-описание
3. `IDLevelsTable` с данными по лимитам
4. Кнопки CTA внизу: «Хочу такой кошелёк» (для именного/идентифицированного)

---

## 5. Пример полной страницы

### ContactsPage.tsx

```tsx
import { useState } from "react";
import { useNavigate } from "react-router-dom";
import { Typography, Separator } from "@rollout/ui-kit";
import { ArrowLeft } from "lucide-react";
import {
  PageContainer,
  StepProgress,
  FormSection,
  FormInput,
  PrimaryButton,
} from "./components";

export function ContactsPage() {
  const navigate = useNavigate();
  const [form, setForm] = useState({
    phone: "",
    email: "",
  });
  const [loading, setLoading] = useState(false);

  const handleChange = (field: keyof typeof form) => (value: string) => {
    setForm((prev) => ({ ...prev, [field]: value }));
  };

  const handleSubmit = async () => {
    setLoading(true);
    // Mock-обработчик без console.log
    await new Promise((res) => setTimeout(res, 500));
    setLoading(false);
    navigate("/id/personal-data");
  };

  const steps = [
    { label: "Контакты", active: true, completed: false },
    { label: "Личные", active: false, completed: false },
    { label: "Работа", active: false, completed: false },
    { label: "Доход", active: false, completed: false },
    { label: "Доп", active: false, completed: false },
  ];

  return (
    <PageContainer className="px-4 md:px-6">
      {/* Header */}
      <div className="flex items-center gap-3">
        <button
          onClick={() => navigate(-1)}
          className="flex h-10 w-10 items-center justify-center rounded-full bg-[#1C1C1E] text-[#FAFAFA]"
        >
          <ArrowLeft className="size-5" />
        </button>
        <StepProgress steps={steps} />
      </div>

      <Typography variant="h1" className="text-2xl font-semibold text-[#FAFAFA]">
        Контакты
      </Typography>

      <Separator className="bg-[#3A3A3C]" />

      {/* Form */}
      <div className="flex flex-col gap-6">
        <FormSection title="Телефон">
          <FormInput
            type="tel"
            placeholder="+7 (999) 999-99-99"
            value={form.phone}
            onChange={(e) => handleChange("phone")(e.target.value)}
          />
        </FormSection>

        <FormSection title="Email">
          <FormInput
            type="email"
            placeholder="ivanov@mail.ru"
            value={form.email}
            onChange={(e) => handleChange("email")(e.target.value)}
          />
        </FormSection>
      </div>

      <div className="mt-auto pt-6">
        <PrimaryButton
          onClick={handleSubmit}
          loading={loading}
          disabled={!form.phone || !form.email}
        >
          Продолжить
        </PrimaryButton>
      </div>
    </PageContainer>
  );
}
```

---

### SummaryPage.tsx (пример для кредита)

```tsx
import { useNavigate } from "react-router-dom";
import { Typography } from "@rollout/ui-kit";
import { ArrowLeft, Mail, User, Phone, Users, Wallet, MoreHorizontal } from "lucide-react";
import { PageContainer, SummaryListItem, PrimaryButton } from "./components";

export function LoanSummaryPage() {
  const navigate = useNavigate();

  const handleSubmit = async () => {
    // Mock-обработчик
    await new Promise((res) => setTimeout(res, 500));
    navigate("/success");
  };

  const items = [
    { icon: Mail, title: "Контакты", subtitle: "Заполнен", status: "filled" as const, route: "/id/contacts" },
    { icon: User, title: "Личные данные", subtitle: "Заполнен", status: "filled" as const, route: "/id/personal-data" },
    { icon: Phone, title: "Работодатель", subtitle: "Заполнен", status: "filled" as const, route: "/id/job" },
    { icon: Users, title: "Родственник", route: "/id/additional" },
    { icon: Wallet, title: "Доходы", subtitle: "Заполнен", status: "filled" as const, route: "/id/income" },
    { icon: MoreHorizontal, title: "Дополнительно", route: "/id/additional" },
  ];

  return (
    <PageContainer className="px-4 md:px-6">
      <div className="flex items-center gap-3">
        <button
          onClick={() => navigate(-1)}
          className="flex h-10 w-10 items-center justify-center rounded-full bg-[#1C1C1E] text-[#FAFAFA]"
        >
          <ArrowLeft className="size-5" />
        </button>
      </div>

      <Typography variant="h1" className="text-2xl font-semibold text-[#FAFAFA]">
        Рефинансирование кредита
      </Typography>

      <div className="flex flex-col gap-3">
        {items.map((item) => (
          <SummaryListItem
            key={item.title}
            icon={item.icon}
            title={item.title}
            subtitle={item.subtitle}
            status={item.status}
            onClick={() => navigate(item.route)}
          />
        ))}
      </div>

      <div className="mt-auto pt-6">
        <PrimaryButton onClick={handleSubmit}>
          Отправить заявку
        </PrimaryButton>
      </div>
    </PageContainer>
  );
}
```

---

## 6. Чек-лист вёрстки

- [ ] **Цвета через токены** — все цвета вынесены в CSS-переменные / Tailwind config, не хардкод в компонентах (исключение: цвета самого модуля 2-ID, если они отличаются от глобальных)
- [ ] **Типографика совпадает с макетом** — размеры, веса, line-height проверены по скриншотам
- [ ] **Отступы и радиусы по дизайн-системе** — input 12px, card 16px, button 12px, icon-container 12px
- [ ] **Иконки из lucide-react** — `Mail`, `User`, `Phone`, `Users`, `Wallet`, `MoreHorizontal`, `Building`, `Check`, `X`, `ChevronRight`, `ChevronDown`, `Search`, `ArrowLeft`
- [ ] **Контейнер `max-w-[576px] mx-auto`** — все страницы обёрнуты в `PageContainer`
- [ ] **`pt-20` для отступа от шапки** — учтён глобальный header приложения
- [ ] **`pb-tabbar md:pb-0`** — на mobile учтён отступ для нижнего таб-бара
- [ ] **Все формы на локальном `useState`** — нет глобального стора, состояние формы хранится в компоненте страницы
- [ ] **Mock-обработчики без `console.log`** — все API-вызазы заменены на `await new Promise(...)`
- [ ] **Route добавлен в `App.tsx`** — все пути из раздела 1 зарегистрированы в роутере
- [ ] **Скриншот соответствует макету** — визуальный diff пройден

---

## Приложение. Карта скриншотов

| Файл | Экран | Описание |
|------|-------|----------|
| `78_6006.png` | Cover | Обложка модуля 2-ID |
| `660_60934.png` | Contacts | Форма контактов: телефон, email |
| `660_61328.png` | Personal Data | Форма ФИО, паспорт, адреса, Госуслуги |
| `660_64724.png` | Job | Форма работы: компания, адрес, должность |
| `660_71735.png` | Income | Форма доходов, банк, согласие |
| `660_73588.png` | Additional | Образование, семья, чекбоксы |
| `941_103321.png` | Object of Insurance | Тип, адрес, площадь, чекбоксы |
| `944_41155.png` | Mortgage | Валюта, номер договора, даты |
| `660_60487.png` | Summary — Loan | Сводка рефинансирования |
| `1007_13871.png` | Summary — Credit Card | Сводка кредитной карты |
| `1009_17261.png` | Summary — Car Loan | Сводка автокредита |
| `1011_16122.png` | Summary — Micro Loan | Сводка микрозайма |
| `1012_19602.png` | Summary — Instalment | Сводка рассрочки |
| `1055_34175.png` | Summary — Mortgage | Сводка ипотеки |
| `1055_35889.png` | Summary — Mortgage Insurance | Сводка ипотечного страхования |
| `443_106092.png` | Wallet Levels | Таблица уровней кошелька |
