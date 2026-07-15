# Инструкция по вёрстке модуля 1-Authorization (React + @rollout/ui-kit)

> **Дата:** 2026-07-13  
> **Файл Figma:** `1. Authorization` (`QsetmskH8yfvRAfVHkh6Qx`)  
> **Скриншоты:** см. папку `1-Authorization_screenshots/`

---

## 1. Обзор модуля

### 1.1 Назначение
Модуль **Authorization** отвечает за вход, регистрацию, восстановление пароля и выбор профиля пользователя в приложении Rollout. Это единая точка входа в продукт, реализованная в тёмной теме с фирменным фиолетово-оранжевым градиентом бренда.

### 1.2 Экраны модуля
| # | Экран | Страница Figma | Описание |
|---|-------|----------------|----------|
| 1 | **Authorization** (Welcome) | `Authorization` | Главный экран: слот для контента, кнопки «Зарегистрироваться» / «Войти», социальные ID, футер |
| 2 | **Login** | `Login` | Вход по телефону / почте / карте, табы, цифровая клавиатура, умная камера для сканирования карты |
| 3 | **Registration** | `Registration` | Регистрация по телефону / почте, шаг с созданием пароля |
| 4 | **Password Recovery** | `Rassword Recovery` | Восстановление пароля: email → новый пароль |
| 5 | **Select Profile** | `Select Profile` | Выбор существующего профиля или создание нового, ввод пароля для выбранного профиля |
| 6 | **Authorization Social ID** | `Authorization Social ID` | Референсы внешних социальных авторизаций (Госуслуги, VK ID, Яндекс ID, Сбер ID, Т-Банк ID) |

### 1.3 Роуты (URL-пути)
```
/auth                 → Authorization (Welcome)
/login                → Login
/register             → Registration
/recover-password     → Password Recovery
/select-profile       → Select Profile
/smart-camera         → Умная камера (сканирование карты)
```

---

## 2. Дизайн-система

### 2.1 Цвета
Все значения взяты из макетов. **Не хардкодить** — использовать CSS-переменные / токены темы.

| Токен | Значение | Применение |
|-------|----------|------------|
| `--bg-primary` | `#0A0A0A` | Фон страницы |
| `--bg-card` | `#1C1C1E` | Фон карточек, инпутов, табов |
| `--bg-card-accent` | `#1C1C1E` → фиолетовый | Слот-контент на Welcome (граница `dashed` `#7C3AED` / `#A855F7`) |
| `--border-default` | `#3A3A3C` | Бордеры карточек, инпутов, табов |
| `--text-primary` | `#FAFAFA` | Заголовки, основной текст |
| `--text-secondary` | `#E5E5E5` | Body, описания |
| `--text-muted` | `#636366` | Placeholder, caption |
| `--btn-primary-bg` | `#E5E5E5` | Primary кнопка (светло-серый) |
| `--btn-primary-text` | `#0A0A0A` | Текст primary кнопки |
| `--btn-secondary-bg` | `transparent` | Secondary кнопка |
| `--btn-secondary-border` | `#3A3A3C` | Рамка secondary кнопки |
| `--btn-secondary-text` | `#FAFAFA` | Текст secondary кнопки |
| `--input-bg` | `#1C1C1E` | Фон инпута |
| `--input-border` | `#3A3A3C` | Бордер инпута |
| `--input-placeholder` | `#636366` | Placeholder инпута |
| `--tab-active-bg` | `#3A3A3C` | Активный таб |
| `--tab-inactive-bg` | `#1C1C1E` | Неактивный таб |
| `--footer-text` | `#636366` | Текст футера |
| `--social-bg` | `#1C1C1E` | Фон кнопок соц. сетей |

### 2.2 Типографика (Geist Variable)
| Элемент | Размер | Вес | Line-height | Letter-spacing |
|---------|--------|-----|-------------|----------------|
| H1 (заголовок страницы) | 28 px | 700 | 1.2 | -0.02em |
| H3 (подзаголовок, label) | 17 px | 400 | 1.4 | -0.01em |
| H4 (caption, hint) | 15 px | 400 | 1.4 | 0 |
| Body | 16 px | 400 | 1.5 | 0 |
| Button | 17 px | 500 | 1.2 | 0 |
| Caption / Footer | 13 px | 400 | 1.4 | 0 |
| Input text | 16 px | 400 | 1.5 | 0 |
| Placeholder | 16 px | 400 | 1.5 | 0 |

### 2.3 Спейсинг и радиусы
| Параметр | Значение |
|----------|----------|
| Container | `max-w-[576px] mx-auto` |
| Page padding top | `pt-20` (отступ от шапки) |
| Page padding bottom | `pb-8` (десктоп) / `pb-tabbar` (мобайл) |
| Content gap | `gap-7` (28 px) между основными блоками |
| Card padding | `p-6` (24 px) |
| Card border-radius | `rounded-2xl` (16 px) |
| Input border-radius | `rounded-xl` (12 px) |
| Button border-radius | `rounded-2xl` (16 px) — или `rounded-full` для pill-формы |
| Tab border-radius | `rounded-xl` (12 px) для каждого таба, `rounded-2xl` для контейнера |
| Avatar border-radius | `rounded-full` |
| Social button border-radius | `rounded-2xl` (16 px) или `rounded-full` (круглые иконки) |
| Icon size | `size-5` (20 px) для caption, `size-6` (24 px) для action |

### 2.4 Иконки
- **Библиотека:** `lucide-react`
- **Размеры:** `size-5` (20 px) для текстовых иконок (глаз, очищение), `size-6` (24 px) для навигации (стрелка назад, чеврон)
- **Цвет:** наследует `--text-primary` или `--text-muted` в зависимости от состояния
- **Футер соц. иконки:** SVG-иконки X (Twitter), LinkedIn, Telegram (размер ~24 px)

---

## 3. Компоненты UI

### 3.1 AuthButton

**Описание:** Основная кнопка авторизации. Два варианта: `primary` (светлый фон, тёмный текст) и `secondary` (прозрачный фон, рамка, светлый текст).

**Props:**
```ts
interface AuthButtonProps {
  variant?: "primary" | "secondary";
  children: React.ReactNode;
  onClick?: () => void;
  disabled?: boolean;
  type?: "button" | "submit";
  className?: string;
}
```

**Пример кода:**
```tsx
import { Button, cn } from "@rollout/ui-kit";

export function AuthButton({
  variant = "primary",
  children,
  onClick,
  disabled,
  type = "button",
  className,
}: AuthButtonProps) {
  return (
    <Button
      type={type}
      disabled={disabled}
      onClick={onClick}
      className={cn(
        "h-14 w-full rounded-2xl text-[17px] font-medium transition-colors",
        variant === "primary" &&
          "bg-[var(--btn-primary-bg)] text-[var(--btn-primary-text)] hover:bg-white",
        variant === "secondary" &&
          "border border-[var(--border-default)] bg-transparent text-[var(--text-primary)] hover:bg-[var(--bg-card)]",
        className,
      )}
    >
      {children}
    </Button>
  );
}
```

**Shadcn-референс:** `Button`  
**Реализация через @rollout/ui-kit:** `Button` + `cn`

---

### 3.2 AuthInputField

**Описание:** Поле ввода с поддержкой иконок (очистка, показать/скрыть пароль), placeholder и валидации.

**Props:**
```ts
interface AuthInputFieldProps {
  value: string;
  onChange: (value: string) => void;
  placeholder?: string;
  type?: "text" | "password" | "email" | "tel";
  label?: string;
  error?: string;
  showClear?: boolean;
  showTogglePassword?: boolean;
  className?: string;
}
```

**Пример кода:**
```tsx
import { useState } from "react";
import { Input, Field, cn } from "@rollout/ui-kit";
import { Eye, EyeOff, X } from "lucide-react";

export function AuthInputField({
  value,
  onChange,
  placeholder,
  type = "text",
  label,
  error,
  showClear,
  showTogglePassword,
  className,
}: AuthInputFieldProps) {
  const [showPassword, setShowPassword] = useState(false);
  const inputType = showTogglePassword && showPassword ? "text" : type;

  return (
    <Field className={cn("w-full", className)}>
      {label && (
        <label className="mb-2 block text-[15px] text-[var(--text-primary)]">
          {label}
        </label>
      )}
      <div className="relative">
        <Input
          type={inputType}
          value={value}
          onChange={(e) => onChange(e.target.value)}
          placeholder={placeholder}
          className={cn(
            "h-14 w-full rounded-xl border bg-[var(--input-bg)] px-4 text-[16px] text-[var(--text-primary)] placeholder:text-[var(--input-placeholder)]",
            "border-[var(--input-border)] focus:border-[var(--text-primary)] focus:outline-none focus:ring-1 focus:ring-[var(--text-primary)]",
            error && "border-red-500",
            (showClear || showTogglePassword) && "pr-12",
          )}
        />
        <div className="absolute right-3 top-1/2 flex -translate-y-1/2 items-center gap-2">
          {showClear && value && (
            <button
              type="button"
              onClick={() => onChange("")}
              className="text-[var(--text-muted)] hover:text-[var(--text-primary)]"
            >
              <X className="size-5" />
            </button>
          )}
          {showTogglePassword && (
            <button
              type="button"
              onClick={() => setShowPassword((s) => !s)}
              className="text-[var(--text-muted)] hover:text-[var(--text-primary)]"
            >
              {showPassword ? (
                <Eye className="size-5" />
              ) : (
                <EyeOff className="size-5" />
              )}
            </button>
          )}
        </div>
      </div>
      {error && (
        <p className="mt-1 text-[13px] text-red-400">{error}</p>
      )}
    </Field>
  );
}
```

**Shadcn-референс:** `Input` + `Label`  
**Реализация через @rollout/ui-kit:** `Input`, `Field`, `cn`

---

### 3.3 SegmentedTabs

**Описание:** Сегментированное переключение (Телефон / Почта / Карта). Контейнер — тёмный pill, активный таб — подсвеченный.

**Props:**
```ts
interface SegmentedTabsProps {
  tabs: { id: string; label: string }[];
  activeId: string;
  onChange: (id: string) => void;
  className?: string;
}
```

**Пример кода:**
```tsx
import { Tabs, cn } from "@rollout/ui-kit";

export function SegmentedTabs({ tabs, activeId, onChange, className }: SegmentedTabsProps) {
  return (
    <div
      className={cn(
        "flex rounded-2xl bg-[var(--bg-card)] p-1",
        className,
      )}
    >
      {tabs.map((tab) => (
        <button
          key={tab.id}
          onClick={() => onChange(tab.id)}
          className={cn(
            "flex-1 rounded-xl py-3 text-[16px] font-medium transition-colors",
            activeId === tab.id
              ? "bg-[var(--tab-active-bg)] text-[var(--text-primary)]"
              : "text-[var(--text-muted)] hover:text-[var(--text-secondary)]",
          )}
        >
          {tab.label}
        </button>
      ))}
    </div>
  );
}
```

**Shadcn-референс:** `Tabs` (с `TabsList` + `TabsTrigger`)  
**Реализация через @rollout/ui-kit:** `Tabs` (или кастомная обёртка над `Tabs`)

---

### 3.4 ProfileCard

**Описание:** Карточка профиля с аватаром, именем, email и стрелкой. Используется на экране «Выберите профиль».

**Props:**
```ts
interface ProfileCardProps {
  name: string;
  email: string;
  avatarUrl?: string;
  onClick?: () => void;
  className?: string;
}
```

**Пример кода:**
```tsx
import { Card, Avatar, cn } from "@rollout/ui-kit";
import { ChevronRight } from "lucide-react";

export function ProfileCard({ name, email, avatarUrl, onClick, className }: ProfileCardProps) {
  return (
    <Card
      onClick={onClick}
      className={cn(
        "flex cursor-pointer items-center gap-4 rounded-2xl border border-[var(--border-default)] bg-[var(--bg-card)] p-4",
        "hover:bg-[var(--tab-active-bg)] transition-colors",
        className,
      )}
    >
      <Avatar src={avatarUrl} fallback={name[0]} className="size-12" />
      <div className="flex-1">
        <p className="text-[16px] font-medium text-[var(--text-primary)]">{name}</p>
        <p className="text-[14px] text-[var(--text-muted)]">{email}</p>
      </div>
      <ChevronRight className="size-6 text-[var(--text-muted)]" />
    </Card>
  );
}
```

**Shadcn-референс:** `Card` + `Avatar`  
**Реализация через @rollout/ui-kit:** `Card`, `Avatar`, `cn`

---

### 3.5 SocialAuthButton

**Описание:** Кнопка авторизации через социальные сети. Круглая или rounded-2xl, содержит SVG-логотип провайдера.

**Props:**
```ts
interface SocialAuthButtonProps {
  provider: "gosuslugi" | "sber" | "vk" | "yandex" | "tinkoff";
  onClick?: () => void;
  className?: string;
}
```

**Пример кода:**
```tsx
import { Button, cn } from "@rollout/ui-kit";

const providerStyles: Record<string, string> = {
  gosuslugi: "bg-white text-[#0055A4]",
  sber: "bg-[#21A038] text-white",
  vk: "bg-[#0077FF] text-white",
  yandex: "bg-[#FC3F1D] text-white",
  tinkoff: "bg-[#FFDD2D] text-black",
};

export function SocialAuthButton({ provider, onClick, className }: SocialAuthButtonProps) {
  return (
    <Button
      onClick={onClick}
      className={cn(
        "flex size-14 items-center justify-center rounded-2xl transition-transform active:scale-95",
        providerStyles[provider],
        className,
      )}
      aria-label={`Войти через ${provider}`}
    >
      {/* SVG-логотип провайдера */}
      <SocialIcon provider={provider} className="size-6" />
    </Button>
  );
}
```

**Shadcn-референс:** `Button`  
**Реализация через @rollout/ui-kit:** `Button`, `cn`

---

### 3.6 PageHeader

**Описание:** Шапка страницы с кнопкой «Назад» и заголовком. На экране ввода пароля добавляется иконка поиска (лупа).

**Props:**
```ts
interface PageHeaderProps {
  title: string;
  showBack?: boolean;
  onBack?: () => void;
  showSearch?: boolean;
  className?: string;
}
```

**Пример кода:**
```tsx
import { ArrowLeft, Search } from "lucide-react";
import { cn } from "@rollout/ui-kit";

export function PageHeader({ title, showBack = true, onBack, showSearch, className }: PageHeaderProps) {
  return (
    <div className={cn("flex items-center gap-4 py-4", className)}>
      {showBack && (
        <button
          onClick={onBack}
          className="flex size-10 items-center justify-center text-[var(--text-primary)]"
        >
          <ArrowLeft className="size-6" />
        </button>
      )}
      <h1 className="flex-1 text-[28px] font-bold leading-tight text-[var(--text-primary)]">
        {title}
      </h1>
      {showSearch && (
        <button className="flex size-10 items-center justify-center text-[var(--text-primary)]">
          <Search className="size-6" />
        </button>
      )}
    </div>
  );
}
```

**Shadcn-референс:** —  
**Реализация через @rollout/ui-kit:** `cn`, `Typography` (опционально)

---

### 3.7 Footer

**Описание:** Футер всех экранов авторизации: слева © 2025 Rollout, справа иконки X, LinkedIn, Telegram.

**Props:**
```ts
interface FooterProps {
  className?: string;
}
```

**Пример кода:**
```tsx
import { Separator, cn } from "@rollout/ui-kit";

export function Footer({ className }: FooterProps) {
  return (
    <footer className={cn("mt-auto flex items-center justify-between py-6", className)}>
      <span className="text-[13px] text-[var(--footer-text)]">© 2025 Rollout</span>
      <div className="flex items-center gap-4">
        <a href="#" aria-label="X" className="text-[var(--footer-text)] hover:text-[var(--text-primary)]">
          <XIcon className="size-5" />
        </a>
        <a href="#" aria-label="LinkedIn" className="text-[var(--footer-text)] hover:text-[var(--text-primary)]">
          <LinkedInIcon className="size-5" />
        </a>
        <a href="#" aria-label="Telegram" className="text-[var(--footer-text)] hover:text-[var(--text-primary)]">
          <TelegramIcon className="size-5" />
        </a>
      </div>
    </footer>
  );
}
```

**Shadcn-референс:** `Separator`  
**Реализация через @rollout/ui-kit:** `Separator`, `cn`

---

### 3.8 DividerWithText

**Описание:** Горизонтальная линия с текстом посередине («или»). Используется на Welcome-экране между кнопками и соц. авторизацией.

**Props:**
```ts
interface DividerWithTextProps {
  text: string;
  className?: string;
}
```

**Пример кода:**
```tsx
import { Separator, cn } from "@rollout/ui-kit";

export function DividerWithText({ text, className }: DividerWithTextProps) {
  return (
    <div className={cn("flex items-center gap-4", className)}>
      <Separator className="flex-1 bg-[var(--border-default)]" />
      <span className="text-[14px] text-[var(--text-muted)]">{text}</span>
      <Separator className="flex-1 bg-[var(--border-default)]" />
    </div>
  );
}
```

**Shadcn-референс:** `Separator`  
**Реализация через @rollout/ui-kit:** `Separator`, `cn`

---

### 3.9 SlotCard

**Описание:** Большая карточка-контейнер на Welcome-экране с фиолетовой dashed-рамкой и placeholder-текстом «Slot (swap it with your content)». В реальном приложении заменяется на маркетинговый баннер / анимацию.

**Props:**
```ts
interface SlotCardProps {
  children?: React.ReactNode;
  className?: string;
}
```

**Пример кода:**
```tsx
import { Card, cn } from "@rollout/ui-kit";

export function SlotCard({ children, className }: SlotCardProps) {
  return (
    <Card
      className={cn(
        "flex min-h-[320px] items-center justify-center rounded-2xl border-2 border-dashed border-purple-500/50 bg-[var(--bg-card)]",
        className,
      )}
    >
      {children ?? (
        <span className="text-[16px] text-[var(--text-secondary)]">
          Slot (swap it with your content)
        </span>
      )}
    </Card>
  );
}
```

**Shadcn-референс:** `Card`  
**Реализация через @rollout/ui-kit:** `Card`, `cn`

---

## 4. Структура каждого экрана

### 4.1 Authorization (Welcome)
**Скриншот:** `Authorization_layout_1248_3401.png`

**Layout:**
```
<Container max-w-[576px] mx-auto flex flex-col gap-7 pt-20 pb-8>
  <Logo className="self-start" />
  <SlotCard />
  <AuthButton variant="primary">Зарегистрироваться</AuthButton>
  <AuthButton variant="secondary">Войти</AuthButton>
  <DividerWithText text="или" />
  <div className="flex justify-center gap-4">
    <SocialAuthButton provider="gosuslugi" />
    <SocialAuthButton provider="sber" />
    <SocialAuthButton provider="vk" />
    <SocialAuthButton provider="yandex" />
    <SocialAuthButton provider="tinkoff" />
  </div>
  <p className="text-center text-[13px] text-[var(--text-muted)]">
    Используя приложение, вы принимаете соглашение и политику конфиденциальности
  </p>
  <Footer />
</Container>
```

**Состояния:**
- `loading` — кнопки в состоянии `disabled` с `Spinner` (если есть в ui-kit)
- `error` — текст ошибки под кнопками (например, сетевой сбой)

**Интерактивность:**
- `onClick` на «Зарегистрироваться» → навигация на `/register`
- `onClick` на «Войти» → навигация на `/login`
- `onClick` на соц. кнопки → открытие WebView / OAuth-редирект

---

### 4.2 Login
**Скриншоты:** `Login_layout_1248_3657.png` (телефон), `Login_layout_1670_2004.png` (умная камера)

**Layout:**
```
<Container max-w-[576px] mx-auto flex flex-col gap-7 pt-20 pb-8>
  <PageHeader title="Вход" onBack={() => navigate(-1)} />
  <SegmentedTabs
    tabs={[
      { id: "phone", label: "Телефон" },
      { id: "email", label: "Почта" },
      { id: "card", label: "Карта" },
    ]}
    activeId={activeTab}
    onChange={setActiveTab}
  />
  {activeTab === "phone" && (
    <AuthInputField
      type="tel"
      placeholder="+7 (999) 999-99-99"
      value={phone}
      onChange={setPhone}
    />
  )}
  {activeTab === "email" && (
    <AuthInputField
      type="email"
      placeholder="example@mail.ru"
      value={email}
      onChange={setEmail}
    />
  )}
  {activeTab === "card" && (
    <AuthButton variant="secondary" onClick={() => navigate("/smart-camera")}>
      Сканировать карту
    </AuthButton>
  )}
  <AuthButton variant="primary" onClick={handleLogin}>
    Войти
  </AuthButton>
  <Footer />
</Container>
```

**Состояния:**
- `loading` — кнопка «Войти» в `disabled` с заглушкой-спиннером
- `error` — отображается под инпутом (неверный формат телефона / email)
- `empty` — кнопка disabled, пока поле пустое

**Интерактивность:**
- `onSubmit` → валидация → mock-запрос → переход на `/select-profile`
- Таб «Карта» → кнопка перехода на экран умной камеры

---

### 4.3 Registration
**Скриншоты:** `Registration_layout_1549_2853.png` (пароль), `Registration_layout_1682_4928.png` (почта), `Registration_layout_1685_6450.png` (пустой ввод), `Registration_layout_1682_5731.png` (телефон заполнен), `Registration_layout_1682_6083.png` (телефон с клавиатурой), `Registration_layout_1683_4041.png` (почта с клавиатурой)

**Layout (шаг 1 — контакт):**
```
<PageHeader title="Регистрация" onBack={() => navigate(-1)} />
<SegmentedTabs tabs={[{id:"phone",label:"Телефон"},{id:"email",label:"Почта"}]} … />
<AuthInputField
  type={activeTab === "phone" ? "tel" : "email"}
  placeholder={activeTab === "phone" ? "+7 (999) 999-99-99" : "example@mail.ru"}
  value={contact}
  onChange={setContact}
/>
<AuthButton variant="primary" onClick={() => setStep(2)}>Продолжить</AuthButton>
```

**Layout (шаг 2 — пароль):**
```
<PageHeader title="Регистрация" onBack={() => setStep(1)} />
<p className="text-[17px] text-[var(--text-primary)]">Придумайте и подтвердите пароль</p>
<AuthInputField
  type="password"
  placeholder="Новый пароль"
  value={password}
  onChange={setPassword}
  showClear
  showTogglePassword
/>
<p className="text-[13px] text-[var(--text-muted)]">Минимум 8 символов</p>
<AuthInputField
  type="password"
  placeholder="Повторите пароль"
  value={confirmPassword}
  onChange={setConfirmPassword}
  showClear
  showTogglePassword
/>
<AuthButton variant="primary" onClick={handleRegister}>Зарегистрироваться</AuthButton>
```

**Состояния:**
- `loading` — кнопка disabled
- `error` — несовпадение паролей, менее 8 символов
- `success` — переход на `/select-profile`

---

### 4.4 Password Recovery
**Скриншоты:** `Rassword_Recovery_layout_1549_1706.png` (email), `Rassword_Recovery_layout_1248_4649.png` (новый пароль)

**Layout (шаг 1 — email):**
```
<PageHeader title="Восстановление пароля" onBack={() => navigate(-1)} />
<AuthInputField
  type="email"
  placeholder="example@mail.ru"
  value={email}
  onChange={setEmail}
/>
<AuthButton variant="primary" onClick={handleReset}>Сбросить текущий пароль</AuthButton>
<Footer />
```

**Layout (шаг 2 — новый пароль):**
```
<PageHeader title="Создайте новый пароль" onBack={() => navigate(-1)} />
<AuthInputField type="password" placeholder="Новый пароль" value={password} onChange={setPassword} showTogglePassword />
<AuthInputField type="password" placeholder="Повторите пароль" value={confirm} onChange={setConfirm} showTogglePassword />
<AuthButton variant="primary" onClick={handleSave}>Сохранить</AuthButton>
<Footer />
```

**Состояния:**
- `loading` — отправка запроса
- `error` — email не найден, пароли не совпадают
- `success` — редирект на `/login`

---

### 4.5 Select Profile
**Скриншоты:** `Select_Profile_layout_1549_1762.png` (список), `Select_Profile_layout_1549_2777.png` (ввод пароля)

**Layout (список профилей):**
```
<PageHeader title="Выберите профиль" onBack={() => navigate(-1)} />
<div className="flex flex-col gap-3">
  {profiles.map((p) => (
    <ProfileCard key={p.id} name={p.name} email={p.email} avatarUrl={p.avatarUrl} onClick={() => selectProfile(p)} />
  ))}
</div>
<AuthButton variant="secondary" onClick={() => navigate("/register")}>Создать новый</AuthButton>
<Footer />
```

**Layout (ввод пароля):**
```
<PageHeader title="Введите пароль" onBack={() => setMode("list")} showSearch />
<ProfileCard name={selected.name} email={selected.email} avatarUrl={selected.avatarUrl} onClick={() => {}} />
<AuthInputField type="password" placeholder="Пароль" value={password} onChange={setPassword} showTogglePassword />
<button className="self-start text-[15px] text-[var(--text-primary)]">Забыли пароль?</button>
<AuthButton variant="primary" onClick={handleLogin}>Войти</AuthButton>
<AuthButton variant="secondary" onClick={() => setMode("list")}>Вернуться к списку</AuthButton>
<Footer />
```

**Состояния:**
- `empty` — нет профилей → показываем только «Создать новый»
- `error` — неверный пароль (текст ошибки под инпутом)
- `loading` — кнопка «Войти» disabled

---

### 4.6 Authorization Social ID (референсы)
**Скриншоты:** 
- `Authorization_Social_ID_layout_1248_5321.png` — Госуслуги (светлая тема)
- `Authorization_Social_ID_layout_1248_5322.png` — VK ID (тёмная тема)
- `Authorization_Social_ID_layout_1248_5323.png` — Яндекс ID (тёмная тема)
- `Authorization_Social_ID_layout_1248_5324.png` — Т-Банк ID (светлая тема)
- `Authorization_Social_ID_layout_1248_5325.png` — Сбер ID (светлая тема)

**Примечание:** Это не самостоятельные экраны Rollout, а **внешние OAuth-страницы**, открываемые в WebView / системном браузере. В макетах они приведены для понимания UX-потока.

**Реализация в Rollout:** на экране `Authorization` (Welcome) кнопки соц. сетей вызывают:
```ts
const handleSocialLogin = (provider: string) => {
  const urls: Record<string, string> = {
    gosuslugi: "https://esia.gosuslugi.ru/...",
    vk: "https://id.vk.com/...",
    yandex: "https://passport.yandex.ru/...",
    sber: "https://online.sberbank.ru/...",
    tinkoff: "https://id.tinkoff.ru/...",
  };
  window.location.href = urls[provider]; // или open WebView
};
```

---

## 5. Пример полной страницы (LoginPage.tsx)

```tsx
import { useState } from "react";
import { useNavigate } from "react-router-dom";
import { cn } from "@rollout/ui-kit";
import { PageHeader } from "@/components/authorization/PageHeader";
import { SegmentedTabs } from "@/components/authorization/SegmentedTabs";
import { AuthInputField } from "@/components/authorization/AuthInputField";
import { AuthButton } from "@/components/authorization/AuthButton";
import { Footer } from "@/components/authorization/Footer";

export function LoginPage() {
  const navigate = useNavigate();
  const [activeTab, setActiveTab] = useState<"phone" | "email" | "card">("phone");
  const [phone, setPhone] = useState("");
  const [email, setEmail] = useState("");
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState("");

  const handleLogin = async () => {
    setError("");
    setLoading(true);
    // Mock-обработчик (без console.log)
    await new Promise((res) => setTimeout(res, 1000));
    const isValid =
      activeTab === "phone" ? phone.length >= 10 : email.includes("@");
    if (!isValid) {
      setError("Некорректные данные");
      setLoading(false);
      return;
    }
    navigate("/select-profile");
    setLoading(false);
  };

  return (
    <div className="min-h-screen bg-[var(--bg-primary)] text-[var(--text-primary)]">
      <div className="mx-auto flex min-h-screen max-w-[576px] flex-col gap-7 px-4 pt-20 pb-8">
        <PageHeader title="Вход" onBack={() => navigate(-1)} />

        <SegmentedTabs
          tabs={[
            { id: "phone", label: "Телефон" },
            { id: "email", label: "Почта" },
            { id: "card", label: "Карта" },
          ]}
          activeId={activeTab}
          onChange={(id) => setActiveTab(id as typeof activeTab)}
        />

        {activeTab === "phone" && (
          <AuthInputField
            type="tel"
            placeholder="+7 (999) 999-99-99"
            value={phone}
            onChange={setPhone}
          />
        )}

        {activeTab === "email" && (
          <AuthInputField
            type="email"
            placeholder="example@mail.ru"
            value={email}
            onChange={setEmail}
          />
        )}

        {activeTab === "card" && (
          <AuthButton
            variant="secondary"
            onClick={() => navigate("/smart-camera")}
          >
            Сканировать карту
          </AuthButton>
        )}

        {error && (
          <p className="text-center text-[14px] text-red-400">{error}</p>
        )}

        <AuthButton
          variant="primary"
          onClick={handleLogin}
          disabled={loading || (!phone && !email && activeTab !== "card")}
        >
          {loading ? "Вход…" : "Войти"}
        </AuthButton>

        <Footer />
      </div>
    </div>
  );
}
```

---

## 6. Чек-лист вёрстки

- [ ] **Все цвета через токены** — не хардкодить hex-значения в компонентах, использовать CSS-переменные (`var(--bg-primary)` и т.д.)
- [ ] **Типографика совпадает с макетом** — Geist Variable, размеры, веса, line-height по разделу 2.2
- [ ] **Отступы и радиусы по дизайн-системе** — container `max-w-[576px] mx-auto`, `pt-20`, `pb-8`, `gap-7`, `rounded-2xl` для карточек, `rounded-xl` для инпутов
- [ ] **Иконки из lucide-react** — `ArrowLeft`, `ChevronRight`, `Eye`, `EyeOff`, `X`, `Search` — размеры `size-5` / `size-6`
- [ ] **Контейнер `max-w-[576px] mx-auto`** — на всех экранах модуля
- [ ] **`pt-20` для отступа от шапки** — на всех страницах
- [ ] **`pb-tabbar md:pb-0`** — корректный отступ снизу для мобильных устройств с таб-баром
- [ ] **Все формы на локальном `useState`** — без глобальных сторов на этапе вёрстки
- [ ] **Mock-обработчики без `console.log`** — использовать `async/await` + `setTimeout` или пустые функции с `// TODO: API integration`
- [ ] **Route добавлен в `App.tsx`** — все 5+ роутов модуля зарегистрированы в `react-router-dom`
- [ ] **Скриншот соответствует макету** — визуальный diff пройден (табы, кнопки, инпуты, отступы, типографика, футер)
- [ ] **Импорты только из `@rollout/ui-kit`** — не использовать deep imports, не использовать `radix-ui`, не использовать `shadcn add`
- [ ] **Не использовать `radix-ui`** — только `@base-ui/react` через `@rollout/ui-kit`
- [ ] **Футер на всех экранах** — © 2025 Rollout + X, LinkedIn, Telegram
- [ ] **Социальные кнопки — круглые/rounded-2xl** с цветами провайдеров
- [ ] **Input с иконками** — глаз (EyeOff/Eye) и крестик (X) выровнены по центру справа, `absolute right-3 top-1/2 -translate-y-1/2`
- [ ] **Кнопка «Назад»** — стрелка `ArrowLeft`, size-6, отступ от заголовка 16 px

---

## Приложение: Список скриншотов

Все скриншоты сохранены в `/Users/mokoloskov/Documents/kimi/workspace/1-Authorization_screenshots/`:

| Файл | Экран | Описание |
|------|-------|----------|
| `Cover_1._Authorization_49_16.png` | Cover | Обложка Figma-модуля |
| `Authorization_layout_1248_3401.png` | Authorization | Welcome-экран |
| `Login_layout_1248_3657.png` | Login | Вход по телефону (табы + клавиатура) |
| `Login_layout_1670_2004.png` | Login | Умная камера (сканирование карты) |
| `Rassword_Recovery_layout_1248_4649.png` | Password Recovery | Новый пароль |
| `Rassword_Recovery_layout_1549_1706.png` | Password Recovery | Email для восстановления |
| `Select_Profile_layout_1549_1762.png` | Select Profile | Список профилей |
| `Select_Profile_layout_1549_2777.png` | Select Profile | Ввод пароля для профиля |
| `Authorization_Social_ID_layout_1248_5321.png` | Social ID | Госуслуги (референс) |
| `Authorization_Social_ID_layout_1248_5322.png` | Social ID | VK ID (референс) |
| `Authorization_Social_ID_layout_1248_5323.png` | Social ID | Яндекс ID (референс) |
| `Authorization_Social_ID_layout_1248_5324.png` | Social ID | Т-Банк ID (референс) |
| `Authorization_Social_ID_layout_1248_5325.png` | Social ID | Сбер ID (референс) |
| `Registration_layout_1549_2853.png` | Registration | Шаг пароля |
| `Registration_layout_1682_4928.png` | Registration | Email заполнен |
| `Registration_layout_1685_6450.png` | Registration | Пустой ввод |
| `Registration_layout_1682_5731.png` | Registration | Телефон заполнен |
| `Registration_layout_1682_6083.png` | Registration | Телефон + клавиатура |
| `Registration_layout_1683_4041.png` | Registration | Email + клавиатура |

---

*Инструкция сформирована на основе анализа Figma-макетов модуля 1-Authorization. Все размеры, цвета и поведение компонентов выведены из скриншотов и JSON-структуры файла.*
