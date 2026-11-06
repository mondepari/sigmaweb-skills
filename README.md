# SigmaWeb для ИИ-агентов

Подключите свой сайт на [SigmaWeb](https://sigmweb.ru) к ИИ-агенту — Claude Code, Codex,
Gemini CLI, Cursor — и собирайте или переделывайте его словами: «добавь отзывы и FAQ на главную»,
«перенеси мой старый сайт example.ru с контактами и фото товаров», «сделай SEO всех страниц».

Агент работает через MCP-сервер SigmaWeb и собирает страницы **из библиотеки блоков конструктора**
(первый экран, услуги, цены, галерея, отзывы, FAQ, формы заявок, каталог…). Свой HTML-блок — только
когда подходящего блока нет. Все правки идут в черновик: вы видите их в редакторе, можете отменить
одной кнопкой и сами публикуете сайт.

В этом репозитории:

| Файл | Для чего |
|---|---|
| [`skills/sigmaweb/SKILL.md`](skills/sigmaweb/SKILL.md) | skill: как агенту работать с SigmaWeb (Claude Code и другие агенты с поддержкой Agent Skills) |
| [`skills/sigmaweb/references/`](skills/sigmaweb/references) | инструменты, перенос сайта клиента, правила «Своего блока» |
| [`AGENTS.md`](AGENTS.md) | те же правила одним файлом — для Codex, Cursor, Gemini CLI, Copilot |
| `.claude-plugin/`, `.mcp.json` | плагин Claude Code: skill + подключение MCP-сервера |

## 1. Ключ сайта

В студии [sigmaweb-studio.ru](https://sigmaweb-studio.ru): **Сайт → Интеграции → «ИИ-агенты (MCP)»**
→ «Создать ключ» (`swk_…`). Права: «Чтение и правка черновика» или «Только чтение». Ключ показывается
один раз; отозвать можно там же. Правка сайта агентом доступна при активной подписке.

**Агентствам (тариф agency):** ключ аккаунта — в «Интеграциях» кабинета. Он открывает все сайты
аккаунта и создание новых из шаблонов: «сделай сайты для этих 5 клиентов по их старым сайтам».

```bash
export SIGMAWEB_KEY=swk_ваш_ключ
```

## 2. Подключение

### Claude Code — плагином (MCP + skill)

```
/plugin marketplace add mondepari/sigmaweb-skills
/plugin install sigmaweb@sigmaweb
```

Плагин берёт ключ из переменной `SIGMAWEB_KEY`.

### Claude Code — вручную

```bash
claude mcp add --transport http sigmaweb https://sigmaweb-studio.ru/api/mcp \
  --header "Authorization: Bearer $SIGMAWEB_KEY"
git clone https://github.com/mondepari/sigmaweb-skills.git
cp -r sigmaweb-skills/skills/sigmaweb ~/.claude/skills/
```

### Codex CLI

`~/.codex/config.toml`:

```toml
[mcp_servers.sigmaweb]
url = "https://sigmaweb-studio.ru/api/mcp"
bearer_token_env_var = "SIGMAWEB_KEY"
```

Правила — скопируйте [`AGENTS.md`](AGENTS.md) в корень рабочей папки.

### Gemini CLI

`~/.gemini/settings.json`:

```json
{ "mcpServers": { "sigmaweb": { "httpUrl": "https://sigmaweb-studio.ru/api/mcp", "headers": { "Authorization": "Bearer swk_ваш_ключ" } } } }
```

Правила — [`AGENTS.md`](AGENTS.md) как `GEMINI.md` в рабочей папке.

### Cursor

`~/.cursor/mcp.json`:

```json
{ "mcpServers": { "sigmaweb": { "url": "https://sigmaweb-studio.ru/api/mcp", "headers": { "Authorization": "Bearer swk_ваш_ключ" } } } }
```

Правила — [`AGENTS.md`](AGENTS.md) в корень проекта или в Rules.

## 3. Что можно попросить

- «Посмотри мой сайт и скажи, каких разделов не хватает главной» — и «добавь их».
- «Перенеси сайт https://old-site.ru: контакты, услуги с ценами, фото работ — сделай современную версию».
- «Загрузи фото из папки ./photos в медиатеку и поставь их в галерею».
- «Сгенерируй фото для первого экрана» (ИИ-фото платные — по норме тарифа).
- «Заполни SEO всех страниц».
- «Перенеси 3 последние новости со старого сайта в блог, с фото и датами».
- «Посмотри, как главная выглядит на телефоне, и поправь, что режет глаз» (скриншоты `render_preview`).
- Ключ аккаунта: «Создай сайт для стоматологии "Улыбка" по шаблону ниши и перенеси контакты с ulybka.ru».

Подробно об инструментах — [`references/tools.md`](skills/sigmaweb/references/tools.md).

## Безопасность

- Ключ сайта даёт доступ только к одному сайту; правки — только в черновик, публикации нет.
- Ключ аккаунта (agency) может публиковать сайты и статьи (`publish_site`, `publish_blog_post`) —
  поручайте это агенту явно; откат версии — в студии.
- Каждая правка агента записывается; в редакторе — плашка «ИИ-агент изменил черновик · Отменить».
- Храните ключ как пароль; при утечке — «Отозвать» в студии.
