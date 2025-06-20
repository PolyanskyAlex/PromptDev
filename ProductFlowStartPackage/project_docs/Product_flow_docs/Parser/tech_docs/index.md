# Техническая документация TaskParserForCursorIDE

## Содержание

### Архитектура
- [Архитектура решения](architecture/solution_architecture.md)
  - Высокоуровневая диаграмма
  - Логическая архитектура
  - Физическая архитектура
  - Стек технологий
  - Принципы проектирования
  - Модели данных
  - Атрибуты качества

### API
- [CLI API](api/cli.md)
  - Обзор
  - Использование
  - Обработка ошибок
  - Примеры использования
  - Коды возврата
  - Безопасность

- [Работа с файловой системой](api/filesystem.md)
  - Безопасная запись файлов
  - Безопасность
  - Структура директорий
  - Обработка ошибок

- [Работа с JSON](api/json.md)
  - Структура манифеста
  - Парсинг JSON
  - Обработка ошибок
  - Валидация данных

## Версионирование

Текущая версия: 1.0.0

### История изменений

#### 1.0.0 (2024-06-19)
- Первоначальный релиз
- Базовая функциональность парсинга JSON
- Безопасная работа с файловой системой
- CLI интерфейс 