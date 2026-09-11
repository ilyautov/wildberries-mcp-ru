# Установка wildberries-mcp-ru агентом

Документ для ИИ-агента, который ставит сервер за человека. Человеку удобнее
[README](README.md).

## 1. Проверить uv

```bash
uvx --version || curl -LsSf https://astral.sh/uv/install.sh | sh
```

## 2. Прописать сервер

Claude Desktop: `~/Library/Application Support/Claude/claude_desktop_config.json`
(macOS) или `%APPDATA%\Claude\claude_desktop_config.json` (Windows).
Cline: `cline_mcp_settings.json`.

```json
{
  "mcpServers": {
    "wb": {
      "command": "uvx",
      "args": ["wildberries-mcp-ru"],
      "env": {
        "WB_API_TOKEN": "<значение>"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

## 3. Ключи

| переменная | тип | где взять |
|---|---|---|
| `WB_API_TOKEN` | секрет | Токен из кабинета seller.wildberries.ru, Настройки → Доступ к API. Уходит в Authorization без Bearer. |

Значения спрашиваются у человека и в репозиторий не пишутся. Второй путь, без
переменных окружения: запустить сервер и вызвать `wb_add_cabinet`,
ключи лягут в `~/.marketplace-mcp/cabinets.json` с правами 600.

## 4. Проверить

Перезапустить клиент и вызвать `wb_check_auth`. Ответ «ключей нет»
означает, что сервер поднялся, а ключи не дошли: смотреть шаг 3. Каталог
отвечает `wb_list_sections`, в нём 307 методов.

## Если не поднимается

- `uvx` не найден: шаг 1, потом перезапустить клиент, он читает PATH при старте.
- Пусто в списке инструментов: клиент не перечитал конфигурацию, нужен рестарт.
- Ошибка авторизации при верных ключах: активный кабинет в
  `~/.marketplace-mcp/cabinets.json` имеет приоритет над переменными окружения.
