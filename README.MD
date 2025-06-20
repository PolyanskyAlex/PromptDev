# PromptDev - AI Agent Rules Framework

Фреймворк для создания промптов и правил для автоматизированной разработки с помощью AI-агента в Cursor IDE.

## Описание

PromptDev - это мета-система, которая позволяет создавать структурированные наборы правил и пайплайнов для AI-агентов, автоматизирующих полный цикл разработки программного обеспечения.

## Структура проекта

```
PromptDev/
├── .cursor/rules/          # Правила для текущего агента
├── rules/                  # Шаблоны правил для создаваемых проектов
├── pipelines/             # Шаблоны пайплайнов разработки и деплоя
├── docs/                  # Документация
├── README.MD              # Описание проекта
└── PROJECR_OPERATE_README.md  # Руководство по использованию
```

## Ключевые компоненты

### Система правил
- **mainRules.mdc** - Главный маршрутизатор поведения агента
- **_common_principles.mdc** - Базовые принципы разработки корпоративного уровня
- **dev.mdc** - Правила высококачественной разработки (SOLID, TDD, Security)
- **testing.mdc** - Правила тестирования и контроля качества
- **deploy.mdc** - Правила развертывания и CI/CD

### Пайплайны
- **dev_pipeline.mdc** - Иерархический пайплайн разработки с управлением задачами
- **deploy_pipeline.mdc** - Пайплайн развертывания с валидацией и откатом

## Принципы работы

1. **JSON-манифест как единый источник правды** - вся информация о проекте в `ai_executable_plan.json`
2. **Автономная инициализация** - проект настраивается одной командой
3. **Иерархические задачи** - поддержка родительских задач (X.Y) и подзадач (X.Y.Z)
4. **Context7 интеграция** - использование актуальной документации библиотек
5. **Атомарные коммиты** - каждая подзадача в отдельном коммите по Conventional Commits

## Использование

### Для разработчиков промптов
Этот репозиторий содержит готовые шаблоны правил и пайплайнов, которые можно адаптировать под конкретные проекты.

### Process workflow
1. Создать JSON-манифест проекта
2. Запустить команду: "подготовь проект по документу ai_executable_plan.json"
3. Система автоматически создаст структуру проекта и документацию
4. Выполнять задачи командами типа: "выполни задачу 1.1.1"

## Технические требования

- Cursor IDE с поддержкой MCP серверов
- Context7 API доступ (для работы с документацией библиотек)
- Git для контроля версий
- Node.js (для системных скриптов)

## Статус

🚧 **В разработке** - Проект находится в стадии активной разработки. Базовая архитектура готова, ведется работа над системными скриптами.

## Лицензия

MIT License

## Контрибьюция

Приветствуются предложения по улучшению архитектуры правил и пайплайнов. Пожалуйста, создавайте Issues для обсуждения изменений.