# AGENTS.md

## Архитектура
Всё приложение — один файл `index.html`: `<style>` с CSS, разметка вкладок (`.tab`), нижнее меню `#nav`/`.nav`, и `<script>` с логикой (шагомер, Firebase, камера/Gemini, магазин, кейсы). Сборки нет, публикуется корень репозитория (`netlify.toml`).

## Правила рендеринга (Chrome Android)
- `.nav` — `position: fixed`, сплошной фон `#0e0e18 !important`, БЕЗ `transform: translateZ`, `backface-visibility`, `will-change`, `contain`, `isolation`, `backdrop-filter`. Отдельный GPU-слой у меню давал цветные полосы и рябь.
- Переключение вкладок (`switchTab`) мгновенное: `staggerIn()` там не вызывается, у `.nav`, `.pill`, `.tab` и потомков в конце `<style>` стоят `transition/animation: none !important`.
- `.glass`, `.case-card`, `.case-ico`, `.rcard`, `.item` — без `will-change` и `backdrop-filter`.
- Фон (`.backdrop`, `.blob`, `.grain`) не упрощать — это требование владельца.

## Соглашения
- Правки точечные, файл целиком не переписывать.
- Переопределения добавляются в конец `<style>`.
