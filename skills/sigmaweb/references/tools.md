# Инструменты MCP-сервера sigmaweb

Сервер: `https://sigmaweb-studio.ru/api/mcp` (Streamable HTTP), заголовок `Authorization: Bearer swk_…`.
Точные схемы аргументов отдаёт сам сервер (`tools/list`).

## Чтение (право read)

| Инструмент | Что делает |
|---|---|
| `get_site` | обзор: название, адрес, тип/режим, профиль бизнеса, страницы (id, slug, число блоков), лимиты |
| `get_page { page, include_data? }` | блоки страницы по порядку с данными; `page` — id или slug (`home`) |
| `list_block_types { query? }` | каталог блоков библиотеки, доступных этому сайту |
| `get_block_schema { type }` | JSON Schema данных блока + пример |
| `page_gaps { page }` | каких разделов не хватает странице, куда вставить, сколько ещё можно |
| `get_design_rules` | правила «Своего блока» и тема сайта |
| `get_site_info` | контакты, шапка, подвал |
| `list_media { query?, kind?, limit? }` | медиатека владельца |
| `search_help { query }` | справка SigmaWeb |
| `render_preview { page, device?, from? }` | скриншот черновика (ПК / телефон), фрагментами-картинками |
| `list_design_systems { mood?, niche? }` | готовые системы дизайна (палитра, шрифты, стиль) |

## Правка черновика (право write, активная подписка)

| Инструмент | Что делает |
|---|---|
| `add_block { page, type, data?, after? }` | новый блок библиотеки (data — поверх примера) |
| `update_block { block_id, patch? \| data? }` | изменить данные блока |
| `move_block { block_id, after }` | переставить (`after` — id блока или `"start"`) |
| `delete_block { block_id }` | удалить (только по просьбе владельца) |
| `set_custom_section { page?, block_id?, after?, html, css }` | «Свой блок» HTML + CSS — когда нет блока в библиотеке |
| `set_page_seo { page, title?, description?, keywords? }` | SEO страницы |
| `create_page { title, slug? }` | новая пустая страница |
| `update_site_info { part: contacts\|header\|footer, patch }` | контакты, шапка, подвал |
| `set_business_profile { about }` | описание бизнеса (опора встроенного ИИ) |
| `import_media { items: [{ url, alt?, name?, tags? }], folder? }` | фото по ссылкам → медиатека |
| `generate_photos { block_id, photos? }` | ИИ-фото в места блока (платно); без `photos` — список мест и цена |

| `update_theme { design_system?, primary?, accent?, font_heading?, font_body? }` | оформление всего сайта |

## Ключ аккаунта (тариф agency)

| Инструмент | Что делает |
|---|---|
| `list_sites { query? }` | сайты аккаунта (id → `site_id`) |
| `list_templates { query?, type? }` | шаблоны по нише + пустые старты (`blank`) |
| `create_site { name, template, subdomain?, design_system? }` | новый черновик из шаблона → `site_id` |

Остальные инструменты с ключом аккаунта требуют `site_id`.

## HTTP-загрузка файлов (право write)

`POST https://sigmaweb-studio.ru/api/agent/upload` — multipart: `file` (до 20), `alt`, `folder`, `tags`
(через запятую), `site_id` (для ключа аккаунта). Ответ: `{ results: [{ ok, url, id, width, height } | { ok: false, error }] }`.

## Лимиты

120 вызовов в минуту на ключ, 600 правок в сутки, ответ ≤ 200 КБ, фото ≤ 8 МБ,
блоков: ~30 рекомендовано / 60 максимум на странице, 400 на сайт.
