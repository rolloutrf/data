# Инструкция по вёрстке модуля 7-2FA

## 1. Обзор модуля

### Назначение
Модуль **7-2FA** реализует поток подключения двухфакторной аутентификации (2FA) через SMS. Пользователь последовательно проходит три экрана: ввод номера телефона, ввод SMS-кода подтверждения и установка/подтверждение пароля (PIN-кода) для входа.

### Экраны модуля (из Figma)

| Страница Figma | Назначение | Компоненты |
|----------------|-----------|------------|
| **PhoneNumber** | Ввод номера телефона с выбором кода страны | `.2FAAddPhone` (COMPONENT_SET), `layout` (INSTANCE) |
| **ValidationCode** | Ввод 5-значного SMS-кода | `.2FASmsCode` (COMPONENT_SET), `.2FAHead` (COMPONENT_SET), `.2FACodeInputSubtitle` (COMPONENT_SET) |
| **Password** | Ввод/подтверждение PIN-кода (4 цифры) | `,2FACodeInput` (COMPONENT_SET), `.2FAInputCodeContent` (COMPONENT_SET), `layout` (INSTANCE) |
| **Cover** | Обложка модуля (не экран) | `7. 2FA` (COMPONENT) |

### Роуты (URL-пути)

```tsx
// App.tsx
<Route path="/2fa/phone" element={<PhoneNumberPage />} />
<Route path="/2fa/verify" element={<ValidationCodePage />} />
<Route path="/2fa/password" element={<PasswordPage />} />
```

Переходы между экранами:
- `PhoneNumber` → `ValidationCode` (после нажатия "Дальше")
- `ValidationCode` → `Password` (после успешного ввода 5-значного кода)
- `Password` → `Success` или `Dashboard` (после ввода 4-значного PIN)

---

## 2. Дизайн-система (из макетов)

### Цвета

| Токен | Значение | Использование |
|-------|----------|---------------|
| `--bg-page` | `#0A0A0A` | Фон всей страницы |
| `--bg-card` | `#1C1C1E` | Фон карточек, инпутов, клавиатуры |
| `--bg-primary` | `#E5E5E5` | Primary кнопка (светло-серый) |
| `--text-primary` | `#FAFAFA` | Основной текст (заголовки, body) |
| `--text-secondary` | `#8E8E93` | Вторичный текст (подзаголовки, placeholder) |
| `--text-muted` | `#636366` | Placeholder в инпутах, disabled текст |
| `--border-default` | `#3A3A3C` | Бордеры карточек, инпутов, клавиатуры |
| `--border-active` | `#8E8E93` | Активный/фокусный бордер |
| `--error` | `#FF453A` | Ошибка (красный текст, бордер, заливка) |
| `--error-bg` | `rgba(255, 69, 58, 0.15)` | Фон ошибки (полупрозрачный красный) |
| `--keypad-bg` | `#1C1C1E` | Фон клавиатуры |
| `--keypad-key-bg` | `#2C2C2E` | Фон кнопки клавиатуры |
| `--success` | `#34C759` | Успешное состояние (зелёный) |

### Типографика (Geist Variable)

| Элемент | Размер | Вес | line-height | Letter-spacing |
|---------|--------|-----|-------------|----------------|
| **H1 (экран заголовок)** | 32px | 600 | 1.2 | -0.02em |
| **H2 (раздел)** | 24px | 600 | 1.3 | -0.01em |
| **H3 (компонент)** | 20px | 600 | 1.3 | -0.01em |
| **Body** | 16px | 400 | 1.5 | 0 |
| **Body-small** | 14px | 400 | 1.4 | 0 |
| **Caption** | 12px | 400 | 1.4 | 0.01em |
| **Button** | 16px | 500 | 1 | 0 |
| **Keypad-key** | 24px | 400 | 1 | 0 |
| **Keypad-sub** | 10px | 400 | 1 | 0.05em |

### Спейсинг

| Параметр | Значение |
|----------|----------|
| **Контейнер max-width** | `576px` |
| **Контейнер padding-x** | `16px` (мобайл), `24px` (desktop) |
| **Контейнер padding-top** | `80px` (pt-20) — отступ от шапки |
| **Контейнер padding-bottom** | `32px` (pb-8) |
| **Мобильный отступ от таббара** | `pb-tabbar` (≈ 80px) |
| **Section gap** | `28px` (gap-7) |
| **Card padding** | `16px` — `24px` |
| **Card border-radius** | `16px` |
| **Input border-radius** | `12px` |
| **Button border-radius** | `12px` |
| **Keypad key border-radius** | `8px` |
| **SMS-box border-radius** | `8px` |
| **SMS-box size** | `56px × 56px` |
| **Gap между SMS-box** | `12px` |
| **Gap между PIN-dots** | `16px` |

### Иконки

- **Источник:** `lucide-react`
- **Размеры:** `size-5` (20px) для внутренних иконок, `size-6` (24px) для кнопок/навигации
- **Цвет:** наследует от текста (`currentColor`)
- **Ключевые иконки:** `ChevronDown` (селектор страны), `Delete` (backspace на клавиатуре), `X` (соцсеть), `Linkedin` (соцсеть), `Send` (Telegram)

---

## 3. Компоненты UI

### 3.1. Logo

**Описание:** Логотип Rollout в верхнем левом углу экрана. Белый SVG, размер ~32px.

**Props:**
```tsx
interface LogoProps {
  className?: string;
}
```

**Пример кода:**
```tsx
import { cn } from "@rollout/ui-kit";

export function Logo({ className }: LogoProps) {
  return (
    <div className={cn("h-8 w-8 text-[#FAFAFA]", className)}>
      <svg viewBox="0 0 32 32" fill="currentColor">
        {/* Rollout logo SVG */}
      </svg>
    </div>
  );
}
```

**Реализация через @rollout/ui-kit:** Используй `cn` для объединения классов. Логотип — кастомный SVG-компонент.

---

### 3.2. PhoneInput

**Описание:** Комбинированный инпут с селектором кода страны (+7) и полем для ввода телефона. Состояния: default, focused, error, filled.

**Props:**
```tsx
interface PhoneInputProps {
  value: string;
  countryCode: string;
  onChange: (value: string) => void;
  onCountryChange?: (code: string) => void;
  placeholder?: string;
  error?: string;
  disabled?: boolean;
}
```

**Пример кода:**
```tsx
import { useState } from "react";
import { cn, Field } from "@rollout/ui-kit";
import { ChevronDown } from "lucide-react";

export function PhoneInput({
  value,
  countryCode,
  onChange,
  onCountryChange,
  placeholder = "900 000 00 00",
  error,
  disabled,
}: PhoneInputProps) {
  const [focused, setFocused] = useState(false);

  return (
    <div className="w-full">
      <div
        className={cn(
          "flex items-center gap-3 rounded-xl border bg-[#1C1C1E] px-4 py-3.5 transition-colors",
          focused && !error ? "border-[#8E8E93]" : "border-[#3A3A3C]",
          error && "border-[#FF453A] bg-[rgba(255,69,58,0.15)]",
          disabled && "opacity-50"
        )}
      >
        {/* Country selector */}
        <button
          type="button"
          className="flex items-center gap-1 text-[#FAFAFA]"
          onClick={() => onCountryChange?.("+7")}
        >
          <span className="text-base font-medium">{countryCode}</span>
          <ChevronDown className="size-5 text-[#8E8E93]" />
        </button>

        <div className="h-6 w-px bg-[#3A3A3C]" />

        {/* Phone input */}
        <input
          type="tel"
          value={value}
          onChange={(e) => onChange(e.target.value.replace(/\D/g, ""))}
          onFocus={() => setFocused(true)}
          onBlur={() => setFocused(false)}
          placeholder={placeholder}
          disabled={disabled}
          className="flex-1 bg-transparent text-base text-[#FAFAFA] placeholder:text-[#636366] outline-none"
          maxLength={10}
        />
      </div>
      {error && (
        <p className="mt-2 text-sm text-[#FF453A]">{error}</p>
      )}
    </div>
  );
}
```

**Shadcn-референс:** `Input` — но реализация кастомная через `Field` + нативный `input` из `@rollout/ui-kit`.

---

### 3.3. SmsCodeInput

**Описание:** 5 отдельных квадратных полей для ввода 5-значного SMS-кода. Автофокус на следующее поле, поддержка вставки. Состояния: default, focused, filled, error.

**Props:**
```tsx
interface SmsCodeInputProps {
  value: string[];
  onChange: (value: string[]) => void;
  error?: boolean;
  disabled?: boolean;
  onComplete?: (code: string) => void;
}
```

**Пример кода:**
```tsx
import { useRef, useCallback } from "react";
import { cn } from "@rollout/ui-kit";

export function SmsCodeInput({
  value,
  onChange,
  error,
  disabled,
  onComplete,
}: SmsCodeInputProps) {
  const inputsRef = useRef<(HTMLInputElement | null)[]>([]);
  const length = 5;

  const handleChange = useCallback(
    (index: number, char: string) => {
      const digit = char.replace(/\D/g, "").slice(0, 1);
      if (!digit) return;

      const newValue = [...value];
      newValue[index] = digit;
      onChange(newValue);

      if (index < length - 1) {
        inputsRef.current[index + 1]?.focus();
      } else if (newValue.every((v) => v) && onComplete) {
        onComplete(newValue.join(""));
      }
    },
    [value, onChange, onComplete]
  );

  const handleKeyDown = useCallback(
    (index: number, e: React.KeyboardEvent) => {
      if (e.key === "Backspace" && !value[index] && index > 0) {
        const newValue = [...value];
        newValue[index - 1] = "";
        onChange(newValue);
        inputsRef.current[index - 1]?.focus();
      }
    },
    [value, onChange]
  );

  const handlePaste = useCallback(
    (e: React.ClipboardEvent) => {
      e.preventDefault();
      const pasted = e.clipboardData.getData("text").replace(/\D/g, "").slice(0, length);
      const newValue = pasted.split("").concat(Array(length).fill("")).slice(0, length);
      onChange(newValue);
      if (newValue.every((v) => v) && onComplete) {
        onComplete(newValue.join(""));
      }
    },
    [onChange, onComplete]
  );

  return (
    <div className="flex justify-center gap-3">
      {Array.from({ length }).map((_, i) => (
        <input
          key={i}
          ref={(el) => { inputsRef.current[i] = el; }}
          type="text"
          inputMode="numeric"
          maxLength={1}
          value={value[i] || ""}
          disabled={disabled}
          onChange={(e) => handleChange(i, e.target.value)}
          onKeyDown={(e) => handleKeyDown(i, e)}
          onPaste={handlePaste}
          className={cn(
            "size-14 rounded-lg border-2 bg-[#1C1C1E] text-center text-2xl font-semibold text-[#FAFAFA] outline-none transition-colors",
            "focus:border-[#8E8E93]",
            error ? "border-[#FF453A]" : "border-[#3A3A3C]",
            value[i] && !error && "border-[#FAFAFA]"
          )}
        />
      ))}
    </div>
  );
}
```

**Shadcn-референс:** `Input` — но реализация полностью кастомная через 5 нативных `input` с управлением фокусом.

---

### 3.4. NumericKeypad

**Описание:** Кастомная цифровая клавиатура для ввода PIN-кода. Кнопки 1-9 с буквами (ABC, DEF и т.д.), 0 внизу по центру, backspace справа. Стиль как в iOS PIN-коде.

**Props:**
```tsx
interface NumericKeypadProps {
  onKeyPress: (key: string) => void;
  onBackspace: () => void;
  disabled?: boolean;
}
```

**Пример кода:**
```tsx
import { cn } from "@rollout/ui-kit";
import { Delete } from "lucide-react";

const KEYS = [
  { digit: "1", letters: "" },
  { digit: "2", letters: "ABC" },
  { digit: "3", letters: "DEF" },
  { digit: "4", letters: "GHI" },
  { digit: "5", letters: "JKL" },
  { digit: "6", letters: "MNO" },
  { digit: "7", letters: "PQRS" },
  { digit: "8", letters: "TUV" },
  { digit: "9", letters: "WXYZ" },
  { digit: "", letters: "" },
  { digit: "0", letters: "" },
  { digit: "backspace", letters: "" },
];

export function NumericKeypad({ onKeyPress, onBackspace, disabled }: NumericKeypadProps) {
  return (
    <div className="grid grid-cols-3 gap-2.5 p-4">
      {KEYS.map((key, index) => {
        if (key.digit === "" && key.letters === "") {
          return <div key={index} />;
        }

        if (key.digit === "backspace") {
          return (
            <button
              key={index}
              type="button"
              disabled={disabled}
              onClick={onBackspace}
              className={cn(
                "flex h-14 items-center justify-center rounded-lg bg-[#2C2C2E]",
                "active:bg-[#3A3A3C] transition-colors",
                disabled && "opacity-50"
              )}
            >
              <Delete className="size-6 text-[#FAFAFA]" />
            </button>
          );
        }

        return (
          <button
            key={index}
            type="button"
            disabled={disabled}
            onClick={() => onKeyPress(key.digit)}
            className={cn(
              "flex h-14 flex-col items-center justify-center rounded-lg bg-[#2C2C2E]",
              "active:bg-[#3A3A3C] transition-colors",
              disabled && "opacity-50"
            )}
          >
            <span className="text-2xl font-normal text-[#FAFAFA]">{key.digit}</span>
            {key.letters && (
              <span className="text-[10px] font-normal tracking-wider text-[#8E8E93]">
                {key.letters}
              </span>
            )}
          </button>
        );
      })}
    </div>
  );
}
```

**Shadcn-референс:** `Button` — но полностью кастомная клавиатура, не используется стандартный Button из-за специфичной вёрстки (буквы под цифрами).

---

### 3.5. PinDots

**Описание:** 4 круглых индикатора, показывающих количество введённых цифр PIN. Заполненные точки — белые, пустые — тёмно-серые.

**Props:**
```tsx
interface PinDotsProps {
  count: number;        // количество введённых цифр (0-4)
  max?: number;         // максимум точек (default: 4)
  error?: boolean;
}
```

**Пример кода:**
```tsx
import { cn } from "@rollout/ui-kit";

export function PinDots({ count, max = 4, error }: PinDotsProps) {
  return (
    <div className="flex justify-center gap-4">
      {Array.from({ length: max }).map((_, i) => (
        <div
          key={i}
          className={cn(
            "size-3.5 rounded-full transition-colors",
            i < count
              ? error
                ? "bg-[#FF453A]"
                : "bg-[#FAFAFA]"
              : "bg-[#3A3A3C]"
          )}
        />
      ))}
    </div>
  );
}
```

**Shadcn-референс:** Нет прямого аналога — кастомный компонент индикатора.

---

### 3.6. PrimaryButton

**Описание:** Основная кнопка. Светло-серый фон (`#E5E5E5`), тёмный текст (`#0A0A0A`), полная ширина. Используется для основного действия ("Дальше").

**Props:**
```tsx
interface PrimaryButtonProps {
  children: React.ReactNode;
  onClick?: () => void;
  disabled?: boolean;
  loading?: boolean;
  type?: "button" | "submit";
}
```

**Пример кода:**
```tsx
import { cn, Button } from "@rollout/ui-kit";

export function PrimaryButton({
  children,
  onClick,
  disabled,
  loading,
  type = "button",
}: PrimaryButtonProps) {
  return (
    <Button
      type={type}
      onClick={onClick}
      disabled={disabled || loading}
      className={cn(
        "w-full rounded-xl bg-[#E5E5E5] py-4 text-base font-medium text-[#0A0A0A]",
        "hover:bg-[#D4D4D4] active:bg-[#C4C4C4]",
        "disabled:opacity-50 disabled:cursor-not-allowed",
        "transition-colors"
      )}
    >
      {loading ? "Загрузка..." : children}
    </Button>
  );
}
```

**Shadcn-референс:** `Button` из `@rollout/ui-kit` — использовать `Button` с кастомными классами через `cn`.

---

### 3.7. Footer

**Описание:** Футер с copyright и иконками социальных сетей. Прижат к низу экрана.

**Props:**
```tsx
interface FooterProps {
  className?: string;
}
```

**Пример кода:**
```tsx
import { cn } from "@rollout/ui-kit";
import { X, Linkedin, Send } from "lucide-react";

export function Footer({ className }: FooterProps) {
  return (
    <footer
      className={cn(
        "flex items-center justify-between py-6 text-sm text-[#8E8E93]",
        className
      )}
    >
      <span>© 2025 Rollout</span>
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

**Shadcn-референс:** Кастомный компонент, не требует shadcn-референса.

---

### 3.8. ErrorMessage

**Описание:** Блок ошибки с красным текстом. Может включать ссылку/кнопку для повторного действия ("Запросить новый").

**Props:**
```tsx
interface ErrorMessageProps {
  message: string;
  actionLabel?: string;
  onAction?: () => void;
}
```

**Пример кода:**
```tsx
import { cn } from "@rollout/ui-kit";

export function ErrorMessage({ message, actionLabel, onAction }: ErrorMessageProps) {
  return (
    <div className="space-y-2">
      <p className="text-sm font-medium text-[#FF453A]">{message}</p>
      {actionLabel && onAction && (
        <button
          type="button"
          onClick={onAction}
          className="text-sm font-medium text-[#FAFAFA] underline underline-offset-2"
        >
          {actionLabel}
        </button>
      )}
    </div>
  );
}
```

---

## 4. Структура каждого экрана

### 4.1. PhoneNumber (Укажите номер телефона)

**Скриншот:** `PhoneNumber_layout_1395_966.png`

**Layout:**
```
┌─────────────────────────────────────┐
│  [Logo]                              │  ← pt-20 отступ от шапки
│                                      │
│  Укажите номер телефона              │  ← H1, 32px, semibold
│  Мы привяжем его к кошельку...      │  ← Body, 16px, secondary
│                                      │
│  ┌─────────────────────────────────┐ │
│  │ +7 ▼  │  900 000 00 00          │ │  ← PhoneInput, gap-3
│  └─────────────────────────────────┘ │
│                                      │
│  ┌─────────────────────────────────┐ │
│  │         Дальше                  │ │  ← PrimaryButton
│  └─────────────────────────────────┘ │
│                                      │
│  Нажимая кнопку, вы соглашаетесь... │  ← Caption, 12px, muted
│  ...с политикой...                   │
│                                      │
│  ─────────────────────────────────── │
│  © 2025 Rollout      [X] [in] [✈]  │  ← Footer
└─────────────────────────────────────┘
```

**Контейнер:** `mx-auto flex max-w-[576px] flex-col gap-7 pt-20 pb-8 px-4 md:px-6`

**Список компонентов сверху вниз:**
1. `Logo` — логотип Rollout
2. `Typography` (H1) — "Укажите номер телефона"
3. `Typography` (Body) — подзаголовок с описанием
4. `PhoneInput` — поле ввода телефона с селектором страны
5. `PrimaryButton` — "Дальше"
6. `Typography` (Caption) — дисклеймер с ссылкой на политику
7. `Footer` — copyright + социальные иконки

**Состояния:**

| Состояние | Описание |
|-----------|----------|
| **Default** | Пустой инпут, кнопка активна |
| **Loading** | Кнопка показывает спиннер, инпут disabled |
| **Error** | Под инпутом красный текст ошибки (например, "Неверный формат номера") |
| **Filled** | Номер введён, кнопка остаётся активной |
| **Success** | Переход на ValidationCode |

**Интерактивность:**
- `onChange` в PhoneInput — форматирование ввода (только цифры, маска)
- `onClick` на PrimaryButton — валидация номера, API-запрос на отправку SMS, navigate('/2fa/verify')
- `onCountryChange` — открытие селектора страны (в MVP можно захардкодить +7)
- Ссылка "политикой" — открытие внешней ссылки в новой вкладке

---

### 4.2. ValidationCode (Код подтверждения)

**Скриншоты:**
- Заголовок: `ValidationCode__2FAHead_1024_3049.png` (первый вариант)
- Субтитл: `ValidationCode__2FACodeInputSubtitle_1024_3078.png` (первый вариант)
- Поле ввода: `ValidationCode__2FASmsCode_1024_3006.png` (первый вариант — пустые квадраты)

**Layout:**
```
┌─────────────────────────────────────┐
│  [Logo]                              │
│                                      │
│  Код подтверждения                   │  ← H1, 32px
│  Введите 5-значный код из SMS        │  ← Body, 16px, secondary
│  сообщения: +7 900 000-00-00        │
│                                      │
│  [Неправильный код...]               │  ← ErrorMessage (условно)
│                                      │
│  ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐ │  ← SmsCodeInput (5 полей)
│  │    │ │    │ │    │ │    │ │    │ │
│  └────┘ └────┘ └────┘ └────┘ └────┘ │
│                                      │
│  Запросить новый                     │  ← Button/Link (через 60 сек)
│                                      │
│  ─────────────────────────────────── │
│  © 2025 Rollout      [X] [in] [✈]  │
└─────────────────────────────────────┘
```

**Контейнер:** `mx-auto flex max-w-[576px] flex-col gap-7 pt-20 pb-8 px-4 md:px-6`

**Список компонентов сверху вниз:**
1. `Logo` — логотип Rollout
2. `Typography` (H1) — "Код подтверждения"
3. `Typography` (Body) — "Введите 5-значный код из SMS сообщения: +7 900 000-00-00"
4. `ErrorMessage` (условно) — ошибка валидации кода
5. `SmsCodeInput` — 5 полей для ввода цифр
6. `Typography` (Button/Link) — "Запросить новый" с таймером обратного отсчёта
7. `Footer`

**Состояния:**

| Состояние | Описание |
|-----------|----------|
| **Default** | 5 пустых квадратов, подсказка с номером телефона |
| **Focused** | Первый (или текущий) квадрат с border `#8E8E93` |
| **Filled** | Квадраты заполнены цифрами, border белый |
| **Error** | Красный текст "Неправильный код — попробуйте ещё раз или запросите новый", квадраты с красным border |
| **Loading** | Отправка кода на сервер, квадраты disabled |
| **Timer** | "Запросить новый (59)" — серый текст с таймером |
| **Success** | Переход на Password |

**Интерактивность:**
- `onChange` в SmsCodeInput — автофокус на следующее поле, поддержка paste
- `onComplete` — API-запрос на проверку кода
- Ошибка: отображается ErrorMessage, квадраты получают красный border
- "Запросить новый" — запускает таймер 60 секунд, затем активирует кнопку
- Если код верный — navigate('/2fa/password')

---

### 4.3. Password (Повторите пароль)

**Скриншот:** `Password_layout_1394_493.png`

**Layout:**
```
┌─────────────────────────────────────┐
│  [Logo]                              │
│                                      │
│  Повторите пароль                    │  ← H1, 32px, center
│  Тот же, который используете...      │  ← Body, 16px, secondary, center
│                                      │
│  ●    ●    ●    ●                    │  ← PinDots (4 точки)
│                                      │
│                                      │
│  ┌────┐ ┌────┐ ┌────┐               │
│  │ 1  │ │ 2  │ │ 3  │               │
│  │    │ │ABC │ │DEF │               │
│  └────┘ └────┘ └────┘               │
│  ┌────┐ ┌────┐ ┌────┐               │
│  │ 4  │ │ 5  │ │ 6  │               │
│  │GHI │ │JKL │ │MNO │               │  ← NumericKeypad
│  └────┘ └────┘ └────┘               │
│  ┌────┐ ┌────┐ ┌────┐               │
│  │ 7  │ │ 8  │ │ 9  │               │
│  │PQRS│ │TUV │ │WXYZ│               │
│  └────┘ └────┘ └────┘               │
│  ┌────┐ ┌────┐ ┌────┐               │
│  │    │ │ 0  │ │ ⌫  │               │
│  └────┘ └────┘ └────┘               │
│                                      │
│  ─────────────────────────────────── │
│  © 2025 Rollout      [X] [in] [✈]  │
└─────────────────────────────────────┘
```

**Контейнер:** `mx-auto flex max-w-[576px] flex-col gap-7 pt-20 pb-8 px-4 md:px-6 pb-tabbar md:pb-8`

**Список компонентов сверху вниз:**
1. `Logo` — логотип Rollout
2. `Typography` (H1, centered) — "Повторите пароль"
3. `Typography` (Body, centered) — "Тот же, который используете при входе в баланс для покупок"
4. `PinDots` — 4 индикатора ввода
5. `NumericKeypad` — цифровая клавиатура
6. `Footer`

**Состояния:**

| Состояние | Описание |
|-----------|----------|
| **Default** | 4 пустые точки, клавиатура активна |
| **Filled (1-3)** | Часть точек заполнена белым |
| **Filled (4)** | Все 4 точки заполнены, API-запрос на проверку |
| **Error** | Точки красные, вибрация/анимация тряски |
| **Success** | Точки зелёные, переход на следующий экран |
| **Loading** | Клавиатура disabled |

**Интерактивность:**
- `onKeyPress` на NumericKeypad — добавляет цифру к PIN, обновляет PinDots
- `onBackspace` — удаляет последнюю цифру
- При 4-й цифре — автоматический `onComplete` с PIN-кодом
- API-ответ `success` — зелёные точки, переход
- API-ответ `error` — красные точки, анимация тряски, сброс через 1 сек

---

## 5. Пример полной страницы

### PhoneNumberPage.tsx

```tsx
import { useState } from "react";
import { useNavigate } from "react-router-dom";
import { cn } from "@rollout/ui-kit";
import { PhoneInput } from "./PhoneInput";
import { PrimaryButton } from "./PrimaryButton";
import { Footer } from "./Footer";
import { Logo } from "./Logo";

export function PhoneNumberPage() {
  const navigate = useNavigate();
  const [phone, setPhone] = useState("");
  const [countryCode, setCountryCode] = useState("+7");
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState("");

  const handleSubmit = async () => {
    if (phone.length < 10) {
      setError("Введите корректный номер телефона");
      return;
    }

    setError("");
    setLoading(true);

    try {
      // Mock API call
      await new Promise((resolve) => setTimeout(resolve, 1500));
      // В реальном проекте: await api.sendSms({ phone: countryCode + phone });
      navigate("/2fa/verify");
    } catch {
      setError("Не удалось отправить SMS. Попробуйте позже.");
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="min-h-screen bg-[#0A0A0A]">
      <div className="mx-auto flex max-w-[576px] flex-col gap-7 pt-20 pb-8 px-4 md:px-6">
        {/* Logo */}
        <Logo />

        {/* Header */}
        <div className="space-y-3">
          <h1 className="text-[32px] font-semibold leading-tight tracking-tight text-[#FAFAFA]">
            Укажите номер телефона
          </h1>
          <p className="text-base leading-relaxed text-[#8E8E93]">
            Мы привяжем его к кошельку и в случае чего отправим смс с кодом. Например, если решите закрыть баланс для покупок.
          </p>
        </div>

        {/* Phone Input */}
        <PhoneInput
          value={phone}
          countryCode={countryCode}
          onChange={setPhone}
          onCountryChange={setCountryCode}
          error={error}
          disabled={loading}
        />

        {/* Submit Button */}
        <PrimaryButton
          onClick={handleSubmit}
          loading={loading}
          disabled={phone.length < 10}
        >
          Дальше
        </PrimaryButton>

        {/* Disclaimer */}
        <p className="text-xs leading-relaxed text-[#636366]">
          Нажимая кнопку, вы соглашаетесь на обработку и передачу данных в соответствии с{" "}
          <a
            href="/privacy"
            className="text-[#8E8E93] underline underline-offset-2 hover:text-[#FAFAFA] transition-colors"
          >
            политикой
          </a>{" "}
          оператору услуг информационного обмена РОЛЛАУТ в целях запуска идентификации банком
        </p>

        {/* Footer */}
        <Footer className="mt-auto" />
      </div>
    </div>
  );
}
```

---

## 6. Чек-лист вёрстки

- [ ] **Все цвета через токены** — не хардкод `#0A0A0A`, `#FAFAFA` и т.д. в компонентах, использовать CSS-переменные или Tailwind-классы с централизованной конфигурацией
- [ ] **Типографика совпадает с макетом** — Geist Variable, размеры, веса, line-height по таблице из раздела 2
- [ ] **Отступы и радиусы по дизайн-системе** — Card 16px, Input 12px, Button 12px, SMS-box 8px
- [ ] **Иконки из lucide-react** — ChevronDown, Delete, X, Linkedin, Send; размеры size-5 / size-6
- [ ] **Контейнер max-w-[576px] mx-auto** — на всех экранах модуля
- [ ] **pt-20 для отступа от шапки** — 80px сверху
- [ ] **pb-tabbar md:pb-0 для мобильного отступа** — на экране Password (с клавиатурой)
- [ ] **Все формы на локальном useState** — без Redux/Zustand для ввода данных
- [ ] **Mock-обработчики без console.log** — использовать Promise + setTimeout для имитации API
- [ ] **Route добавлен в App.tsx** — `/2fa/phone`, `/2fa/verify`, `/2fa/password`
- [ ] **Скриншот соответствует макету** — визуальный diff пройден
- [ ] **SmsCodeInput** — поддержка paste, автофокус, backspace
- [ ] **NumericKeypad** — буквы под цифрами как в макете, backspace-иконка
- [ ] **PinDots** — 4 точки, плавная смена цвета при заполнении
- [ ] **PhoneInput** — только цифры, маска форматирования, селектор страны
- [ ] **Footer** — прижат к низу, иконки соцсетей с hover-эффектом
- [ ] **ErrorMessage** — красный текст, кнопка повторной отправки
- [ ] **Не использовать radix-ui** — только @base-ui/react через @rollout/ui-kit
- [ ] **Не shadcn add** — не добавлять новые компоненты shadcn, использовать @rollout/ui-kit
- [ ] **Импортировать всё из @rollout/ui-kit** — без deep imports (`import { Button, Input, Field, cn } from "@rollout/ui-kit"`)
- [ ] **Тёмная тема по умолчанию** — фон `#0A0A0A`, текст `#FAFAFA`

---

## Приложение: Список скриншотов

| Файл | Описание | Размер |
|------|----------|--------|
| `Cover_7__2FA_14_16.png` | Обложка модуля | 2.9 MB |
| `PhoneNumber_layout_1395_966.png` | Экран ввода телефона (layout) | 76 KB |
| `PhoneNumber__2FAAddPhone_1024_2141.png` | Компонент .2FAAddPhone (варианты) | 163 KB |
| `ValidationCode__2FASmsCode_1024_3006.png` | Компонент .2FASmsCode (5 вариантов) | 29 KB |
| `ValidationCode__2FAHead_1024_3049.png` | Компонент .2FAHead (заголовки) | 141 KB |
| `ValidationCode__2FACodeInputSubtitle_1024_3078.png` | Компонент .2FACodeInputSubtitle (субтитлы) | 69 KB |
| `Password_layout_1394_493.png` | Экран ввода пароля (layout) | 41 KB |
| `Password__2FACodeInput_1010_9875.png` | Компонент ,2FACodeInput (точки) | 11 KB |
| `Password__2FAInputCodeContent_1010_9896.png` | Компонент .2FAInputCodeContent (экран без клавиатуры) | 30 KB |
| `ValidationCode_Typography___H3_1024_3102.png` | Типографика H3 (не используется) | 4 KB |

---

*Инструкция сгенерирована на основе анализа Figma-макетов модуля 7-2FA. Все цвета, размеры и компоненты выведены из скриншотов и структуры Figma-файла.*
