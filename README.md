# TaskVault — архитектурный паспорт

Локально-первый планировщик задач для двух платформ (Android + desktop). Сервер хранит каноническую копию и принимает **идемпотентные команды**; клиент работает без сети и синхронизирует дельту по `sync_token`.

Проект продолжает лабораторную работу 1 (аудит Todoist / Notion Mobile / Obsidian): берём модель Todoist Sync API и отвергаем гидратацию Notion и «только файлы» Obsidian.

Курс: *Создание клиент-серверных приложений*, ИТМО, группа К3420.  
Авторы: Джибрил Олойеде, Абдуллах Олойеде.

## Почему не микросервисы

Команда из двух человек, один предметный контур (задача / проект / метка / синхронизация). Модульный монолит с явными границами модулей дешевле, чем сеть между сервисами. События нужны внутри процесса (outbox), не как отдельная шина на старте.

## Состав `docs/architecture`

| Файл | Что это |
|------|---------|
| [ADR-001.md](docs/architecture/ADR-001.md) | Решение: local-first клиент + модульный монолит API + delta sync |
| [c4-diagram.drawio](docs/architecture/c4-diagram.drawio) | C4: System Context и Container (открыть в [diagrams.net](https://app.diagrams.net/)) |
| [openapi.yaml](docs/architecture/openapi.yaml) | Контракт HTTP: 9 операций, схемы, Bearer JWT |
| [er-diagram.png](docs/architecture/er-diagram.png) | ER: 8 сущностей |

## Как открыть диаграммы

1. C4: https://app.diagrams.net/ → Open → выбрать `c4-diagram.drawio`.
2. OpenAPI: вставить YAML в https://editor.swagger.io/.

## Стек (целевой, не реализация этой лабораторной)

- Клиент: Flutter или Kotlin Multiplatform, локальная БД SQLite (WAL), очередь команд.
- API: один деплой (модульный монолит), PostgreSQL.
- Auth: JWT access + refresh, устройство регистрируется при первом логине.

Репозиторий: https://github.com/jibrilpython/taskvault-passport

На этом этапе репозиторий содержит **паспорт**, не работающий сервер.
