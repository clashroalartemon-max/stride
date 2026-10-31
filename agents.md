# AGENTS.md

## Архитектура
- Всё приложение — один файл `index.html` (стили в `<style>`, логика в одном классическом `<script>` в конце `<body>`). Сборки нет, Netlify публикует корень (`netlify.toml`).
- Состояние — глобальный объект `S` (`defaults()` / `normalizeState()`), хранится в localStorage (`stride.v2`). `save()` пишет с дебаунсом, `save(true)` — сразу.
- UI разбит на вкладки `<section class="tab" id="tab-*">`, переключение — `switchTab()`; рендер-функции `render*()` собраны в `renderAll()`.

## Firebase (друзья)
- `<script type="module">` в `<head>` импортирует Firebase SDK v9 с gstatic CDN, выполняет `signInAnonymously` и кладёт API в `window.FB`, затем шлёт событие `fb-ready` (или `fb-error`). Модуль исполняется ПОСЛЕ основного скрипта, поэтому основной код слушает событие, а не обращается к `FB` при загрузке.
- Блок «ДРУЗЬЯ» в основном скрипте: `cloudSyncSoon()` вызывается из `save()` и с дебаунсом делает `updateDoc` своих шагов. Переменные блока объявлены через `var`, т.к. `save()` может вызваться раньше, чем выполнится блок (TDZ).
- Облачный профиль (`users/{myUid}`, слушается через `onSnapshot`) — источник правды для `S.nickname` и пополнения `S.friends` (`applyCloudMe()`), чтобы сброс/импорт копии не ломали связь с аккаунтом.
- Уникальность ника проверяется запросом по `nicknameLower` (регистронезависимо). Это проверка без транзакции — для гарантии понадобится коллекция-реестр `nicknames/{lower}`.
- Правила безопасности — `firestore.rules` (применяются в Firebase Console, Netlify их не деплоит).

## Соглашения
- Интерфейс и комментарии на русском. Переиспользуйте существующие классы (`glass card`, `card-title`, `row`, `chip-btn`, `modal/sheet`, `f-field`, `empty`, `toast()`).
- Пользовательские строки из Firestore вставлять в HTML только через `escapeHtml()`.
- Файлы `manifest.webmanifest`, `sw.js`, `icons/` упоминаются в `index.html`, но в репозитории пока отсутствуют.
