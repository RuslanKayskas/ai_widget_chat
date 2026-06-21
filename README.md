# 🤖 AI Chat Widget

Независимый AI Chat Widget, разработанный на Vue 3.

Реализован как Web Component и может быть встроен в любые веб-приложения без зависимости от их архитектуры.

---

## ✨ Возможности

* 📦 Web Component (`<ai-chat-widget>`)
* ⚡ UMD сборка через Vite
* 🔐 Передача токена авторизации
* 🔄 Ленивая загрузка
* 🛡️ Защита от повторной регистрации
* 🧩 Встраивание в любые веб-системы
* 🧠 Обработка ошибок загрузки
* 🔌 Полная изоляция от host-приложения

---

## 🧪 UI (Mock)

### 💬 Виджет чата

```text id="mock1"
┌──────────────────────────────┐
│ 🤖 AI Assistant              │
├──────────────────────────────┤
│ 👤 Привет                    │
│ 🤖 Привет! Чем могу помочь? │
│                              │
│ 👤 Напиши код Vue компонента │
│ 🤖 Вот пример:              │
│   <template>                │
│     <div>Hello</div>        │
│   </template>               │
│                              │
├──────────────────────────────┤
│ [ Написать сообщение... ] ➤  │
└──────────────────────────────┘
```

---

## 📦 Интеграция

### 🌐 Любое HTML-приложение

```html id="html1"
<script src="/widget/ai-chat-widget.umd.js"></script>

<ai-chat-widget
  id="ai-chat-widget"
  api-token="YOUR_TOKEN"
></ai-chat-widget>
```

---

### ⚡ Vue / SPA приложения

```javascript id="vue1"
import loadWidget from '@/plugins/widgetLoader';

await loadWidget('/widget/ai-chat-widget.umd.js');

customElements.whenDefined('ai-chat-widget')
  .then(() => {
    console.log('Widget loaded');
  });
```

---

### 🧩 Nuxt (client-side)

```javascript id="nuxt1"
export default {
  src: '~/plugins/ai-chat-widget.js',
  ssr: false
}
```

---

### 🔐 Передача токена

```javascript id="token1"
const widget = document.getElementById('ai-chat-widget');

widget.apiToken = 'JWT_TOKEN_HERE';
```

---

## 🏗️ Архитектура

```text id="arch1"
Any Web Application
        │
        ▼
loadWidget()
        │
        ▼
ai-chat-widget.umd.js
        │
        ▼
<ai-chat-widget>
        │
        ▼
Vue 3 Widget
        │
        ▼
API Layer
```

---

## 🎯 Сценарии использования

* корпоративные системы
* CRM и админ-панели
* SaaS платформы
* личные кабинеты пользователей
* внутренние сервисы компаний
* любые Vue / Nuxt / JS приложения

---

## ⚙️ Технологии

| Tech            | Purpose      |
| --------------- | ------------ |
| Vue 3           | UI           |
| TypeScript      | Typing       |
| Vite            | Build        |
| Web Components  | Isolation    |
| Custom Elements | Registration |
| UMD             | Distribution |

---

## 📸 Превью

### Widget UI

![Widget](./screenshots/widget.png)

### Integration example

![Integration](./screenshots/integration.png)

---

## 🚀 Итог

Проект представляет собой независимый AI-виджет, который может быть встроен в любую веб-экосистему без изменения её архитектуры.
