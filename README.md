# Frontend Code Quality Starter

Учебный проект для настройки инструментов контроля качества frontend-кода.

- **pnpm** — менеджер пакетов
- **ESLint** — проверка JavaScript-кода
- **Prettier** — форматирование
- **eslint-config-prettier** — устранение конфликтов ESLint и Prettier
- **EditorConfig** — базовые настройки файлов
- **Husky** — Git hooks
- **lint-staged** — запуск проверок только для staged-файлов

## Структура

```text
frontend-code-quality-starter/
├── .husky/
│   └── pre-commit
├── src/
│   └── index.js
├── .editorconfig
├── .gitignore
├── .prettierignore
├── .prettierrc
├── eslint.config.js
├── package.json
└── pnpm-lock.yaml
```

---

## 1. Создание проекта

```bash
pnpm init
git init
```

### `.gitignore`

```text
node_modules
dist
coverage
.DS_Store
```

---

## 2. EditorConfig

Создаём `.editorconfig`:

```ini
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
indent_style = space
indent_size = 2
trim_trailing_whitespace = true
```

---

## 3. ESLint

```bash
pnpm create @eslint/config@latest
```

Добавляем в `package.json`:

```json
{
  "scripts": {
    "lint": "eslint .",
    "lint:fix": "eslint . --fix"
  }
}
```

Проверка:

```bash
pnpm lint
```

---

## 4. Prettier

```bash
pnpm add --save-dev --save-exact prettier@3.9.9
```

### `.prettierrc`

```json
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 120,
  "tabWidth": 2,
  "endOfLine": "lf"
}
```

### `.prettierignore`

```text
node_modules
dist
coverage
```

Добавляем в `package.json`:

```json
{
  "scripts": {
    "format": "prettier . --write",
    "format:check": "prettier . --check"
  }
}
```

Проверка:

```bash
pnpm format
pnpm format:check
```

---

## 5. ESLint + Prettier

Устанавливаем:

```bash
pnpm add --save-dev --save-exact eslint-config-prettier
```

Добавляем `prettierConfig` последним в `eslint.config.js`:

```js
import prettierConfig from 'eslint-config-prettier';

export default defineConfig([
  // ESLint configuration

  prettierConfig,
]);
```

```text
ESLint   → проблемы в коде
Prettier → форматирование
```

---

## 6. Устанавливаем Husky и lint-staged

Устанавливаем **оба пакета**:

```bash
pnpm add --save-dev husky lint-staged
```

- `husky` — запускает Git hooks
- `lint-staged` — запускает команды только для staged-файлов

---

## 7. Настраиваем Husky

```bash
pnpm exec husky init
```

Создаётся:

```text
.husky/
└── pre-commit
```

В `package.json` появится:

```json
{
  "scripts": {
    "prepare": "husky"
  }
}
```

---

## 8. Настраиваем lint-staged

Добавляем в `package.json`:

```json
{
  "lint-staged": {
    "*.{js,mjs,cjs}": ["eslint --fix", "prettier --write"],
    "*.{json,md,yml,yaml}": ["prettier --write"]
  }
}
```

---

## 9. Настраиваем pre-commit

`.husky/pre-commit`:

```bash
pnpm exec lint-staged
```

Теперь:

```text
git commit
    ↓
  Husky
    ↓
lint-staged
    ↓
┌───────────┬───────────┐
│  ESLint   │  Prettier │
└───────────┴───────────┘
    ↓
 commit ✓
```

---

## 10. Проверяем

```bash
git add .
git commit -m "test: code quality"
```

Если ESLint обнаружит ошибку, commit будет остановлен.

---

## Команды

### ESLint

```bash
pnpm lint
pnpm lint:fix
```

### Prettier

```bash
pnpm format
pnpm format:check
```

### Git

```bash
git add .
git commit -m "message"
```

## Итог

```text
EditorConfig → настройки файлов

ESLint       → качество кода
Prettier     → форматирование

lint-staged  → только staged-файлы
Husky        → запуск перед commit
```
