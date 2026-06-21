Vue 3 + TypeScript + Vite

This template should help get you started developing with Vue 3 and TypeScript in Vite. The template uses Vue 3 <script setup> SFCs, check out the script setup docs to learn more.
Recommended IDE Setup

    VS Code + Volar (and disable Vetur) + TypeScript Vue Plugin (Volar).

Type Support For .vue Imports in TS

TypeScript cannot handle type information for .vue imports by default, so we replace the tsc CLI with vue-tsc for type checking. In editors, we need TypeScript Vue Plugin (Volar) to make the TypeScript language service aware of .vue types.

If the standalone TypeScript plugin doesn't feel fast enough to you, Volar has also implemented a Take Over Mode that is more performant. You can enable it by the following steps:

    Disable the built-in TypeScript Extension
        Run Extensions: Show Built-in Extensions from VSCode's command palette
        Find TypeScript and JavaScript Language Features, right click and select Disable (Workspace)
    Reload the VSCode window by running Developer: Reload Window from the command palette.

Что бы внедрить в CRM
📦 Widget loader

// src/plugins/widgetLoader.js
export default function loadWidget(jsHref) {
  // Если уже зарегистрирован — ничего не делаем
  if (customElements.get('ai-chat-widget')) return Promise.resolve(false);

  return new Promise((resolve) => {
    const script = document.createElement('script');
    script.src = jsHref;
    script.async = true;

    script.onload = () => resolve(true);
    script.onerror = () => {
      resolve(false);
    };

    document.head.appendChild(script);
  });
}

// src/main.js

import loadWidget from '@/plugins/widjetLoader.js';

async function initApp() {
try {
    // запускаем Vue-приложение
    new Vue({
      router,
      store,
      async created() {
        const error = new URLSearchParams(window.location.search)?.get('error');
        const token = localStorage.getItem('token')
          || sessionStorage.getItem('token')
          || new URLSearchParams(window.location.search)?.get('token');
        const role = localStorage.getItem('role') || sessionStorage.getItem('role');
        const login = localStorage.getItem('login') || sessionStorage.getItem('login');

        if (token && role && login) {
          this.$store.commit(this.$types.TOKEN_SET, token);
          this.$store.commit(this.$types.ROLE_SET, role);
          this.$store.commit(this.$types.LOGIN_SET, login);

          createApi(token);
          try {
            await this.$store.dispatch(this.$types.USER_FETCH);
          } catch (err) {
            this.$store.dispatch(this.$types.LOGOUT);
            console.warn(err);
          }
          return;
        }

        if (error || (token && !role && !login)) return;
        this.$store.dispatch(this.$types.LOGOUT);
      },
      render: (h) => h(App),
    }).$mount('#app');
    // загружаем виджет
    await loadWidget('/widget/ai-chat-widget.umd.js');

} catch (err) {
  console.error('Ошибка загрузки виджета:', err);
  }
}

// запускаем
initApp();

## app.vue

<ai-chat-widget></ai-chat-widget>


## прокидываение пропса

## entery.vue

defineProps({
  apiToken: String,
})

## туда откуда вызывается виджет

// вариаент если например index.html
<ai-chat-widget id="chat-widget"></ai-chat-widget>

const widget = document.getElementById('chat-widget')
rus = 'rrrrrr11111111111111111111'
widget.apiToken = rus

<ai-chat-widget v-if="isLogged && widgetLoaded" id="ai-chat-widget"></ai-chat-widget>

## crm
data() {
  return {
    widgetLoaded: false,
  };
},
mounted() {
  this.setFavicon();
  customElements.whenDefined('ai-chat-widget')
    .then(() => {
      this.widgetLoaded = true;

      if (this.isLogged) {
        this.getAiToken(this.isLogged);
      }
    })
    .catch(() => console.warn('ai-chat-widget не был загружен'));
},
methods: {
  getAiToken(token) {
    // ID нашего виджета в DOM
    const widgetId = 'ai-chat-widget';

    // Вспомогательная функция, которая пытается установить токен
    const setToken = () => {
      const el = document.getElementById(widgetId);
      if (!el) return false; // если элемент ещё не в DOM — выходим
      el.apiToken = token || ''; // присваиваем токен (или очищаем, если пустой)
      return true; // сигнализируем, что установка прошла успешно
    };

    // Если виджет уже есть — токен установится сразу
    // Если виджет ещё не определён (например, рендерится по v-if),
    // ждём, пока custom element будет зарегистрирован, и повторяем установку
    if (!setToken()) {
      customElements.whenDefined(widgetId).then(() => setToken());
    }
  },
},

watch: {
  isLogged: {
    immediate: true,
    handler(newToken) {
      this.getAiToken(newToken);
    },
  },
},

Что бы внедрить в Doctor
📦 Widget loader
plungins ai-chat-widget.js

export default async () => {
  const widgetUrl = '/widget/ai-chat-widget.umd.js';

  try {
    // если уже зарегистрирован — выходим
    if (customElements.get('ai-chat-widget')) return;

    await new Promise((resolve, reject) => {
      const script = document.createElement('script');
      script.src = widgetUrl;
      script.async = true;

      script.onload = () => {
        window.dispatchEvent(new CustomEvent('ai-widget-loaded'));
        resolve();
      };

      script.onerror = () => {
        console.warn(`[AI Widget] Не удалось загрузить скрипт: ${widgetUrl}`);
        // удаляем неудачный тег, чтобы не оставлять мусор
        script.remove();
        resolve(); // не reject — чтобы не ломать всё приложение
      };

      document.head.appendChild(script);
    });
  } catch (error) {
    console.error('[AI Widget] Ошибка при инициализации:', error);
    // даже при ошибке не падаем — просто выходим
  }
};


### nuxt config
{ src: '~/plugins/ai-chat-widget.js', ssr: false }

### сам плагин если файлом закидываем в static/widget

### вывод в default layout
<ai-chat-widget
  id="ai-chat-widget"
  :api-token="apiToken"
  :api-url="apiUrl"
></ai-chat-widget>

apiToken() {
  const token = this.$auth.strategy.token.get();
  const removeBearerToken = token.replace('Bearer ', '');
  return removeBearerToken;
},
apiUrl() {
  const url = `${this.$coreApi.baseURL}/`;
  return url;
}