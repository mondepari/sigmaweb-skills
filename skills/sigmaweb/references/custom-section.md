# «Свой блок» (CUSTOM_SECTION): HTML + CSS

Только когда в библиотеке нет подходящего блока. Актуальные правила всегда в `get_design_rules`.

## Разметка

- Корень — `<div class="cs-root">`, классы с префиксом `cs-`.
- Разрешены: `div section article header footer aside main nav span p h1–h6 ul ol li dl dt dd strong em b i u s small mark sup sub code pre time br hr img figure figcaption blockquote a table thead tbody tfoot tr td th caption` и inline `<svg>`.
- Запрещено (вырезается): `script style iframe form input button select textarea video object embed`, обработчики `on*`, `javascript:`.
- Кнопки — ссылки: `<a class="cs-btn" href="#lead">Записаться</a>` (`#lead` открывает форму заявки) или якорь/URL.
- Каждый текстовый элемент — `data-sw-text="имя"` (латиница, уникально): владелец правит его прямо на холсте.
- Картинки — только `https://` из медиатеки; `data-sw-img="имя"` делает фото заменяемым.

## Стили

- Цвета — только переменные темы: `var(--sw-primary)`, `--sw-primary-foreground`, `--sw-foreground`,
  `--sw-background`, `--sw-surface`, `--sw-muted`, `--sw-muted-foreground`, `--sw-border`, `--sw-accent`
  и смеси `color-mix(in srgb, var(--sw-primary) 12%, transparent)`. Тогда блок перекрашивается вместе с темой.
- Шрифт заголовков — `font-family: var(--sw-font-heading, inherit)`; скругления — `var(--sw-radius, 16px)`.
- CSS изолирован внутри блока; `position: fixed` → `absolute`; `@import`, `@font-face`, внешние `url()` вырезаются.
- Адаптив: `@media (max-width: 720px)`, размеры шрифта через `clamp()`.
- Анимации — CSS (`@keyframes`, `transition`), спокойные; при «уменьшить движение» гаснут сами.
- Лимиты: HTML ≤ 64 КБ, CSS ≤ 32 КБ, фигурные скобки должны сходиться.

## Пример

```json
{
  "page": "home",
  "after": "start",
  "html": "<div class=\"cs-root\"><h2 class=\"cs-t\" data-sw-text=\"title\">Запись за 30 секунд</h2><p class=\"cs-l\" data-sw-text=\"lead\">Выберите мастера и время онлайн</p><a class=\"cs-btn\" href=\"#lead\" data-sw-text=\"cta\">Записаться</a></div>",
  "css": ".cs-root{padding:48px;text-align:center;border-radius:var(--sw-radius,16px);background:color-mix(in srgb,var(--sw-primary) 10%,transparent)} .cs-t{font-family:var(--sw-font-heading,inherit);font-size:clamp(28px,4vw,44px);color:var(--sw-foreground)} .cs-btn{display:inline-block;margin-top:16px;padding:12px 22px;border-radius:999px;background:var(--sw-primary);color:var(--sw-primary-foreground);text-decoration:none}"
}
```
