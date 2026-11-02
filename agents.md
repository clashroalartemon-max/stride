# AGENTS.md

- Всё приложение — один файл `index.html` (CSS, разметка и JS внутри). Сборки нет; Netlify публикует корень репозитория.
- Состояние: глобальный `S`, `defaults()` задаёт поля, `normalizeState()` мигрирует старые сохранения, `save()` пишет в `localStorage` с дебаунсом (`save(true)` — сразу).
- Рендер: `renderAll()`, `renderSettings()`, `renderLive()` (кольцо на главной), `renderWeek()`.
- Норма шагов: `calcRecommendedSteps()` и `applyRecommendedGoal()` рядом с `renderSettings()`. `S.goalType` (переключатель `#goalSeg` в профиле) — отдельно от цели питания `S.foodGoal`. `S.goalManual` выставляется кнопками `goalMinus/goalPlus`; для старых сохранений без флага цель ≠ 10 000 считается ручной.
- Комментарии в коде — на русском; держите стиль коротких однострочных обработчиков.
