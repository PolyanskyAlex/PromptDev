# Быстрый старт

## Введение

TaskParserForCursorIDE - это утилита командной строки для парсинга мастер-плана и генерации файловой структуры проекта. Она принимает JSON файл с описанием проекта и создает соответствующую структуру директорий и файлов.

## Установка

1. Скачайте последнюю версию утилиты для вашей операционной системы из папки `bin`:
   - Windows: `parse-plan_windows_amd64.exe`
   - Linux: `parse-plan_linux_amd64`
   - macOS: `parse-plan_darwin_amd64`

2. Поместите файл в удобную директорию и убедитесь, что у вас есть права на его выполнение.

## Базовое использование

### Способ 1: Через исполняемый файл

1. Если вы находитесь в корневой папке проекта:
   ```bash
   # Windows
   .\bin\parse-plan_windows_amd64.exe -input="project_docs\ai_executable_plan.json" -output="project" -force=true

   # Linux
   ./bin/parse-plan_linux_amd64 -input="project_docs/ai_executable_plan.json" -output="project" -force=true

   # macOS
   ./bin/parse-plan_darwin_amd64 -input="project_docs/ai_executable_plan.json" -output="project" -force=true
   ```

2. Если вы находитесь в папке `bin`:
   ```bash
   # Windows
   .\parse-plan_windows_amd64.exe -input="..\project_docs\ai_executable_plan.json" -output="..\project" -force=true

   # Linux/macOS
   ./parse-plan_linux_amd64 -input="../project_docs/ai_executable_plan.json" -output="../project" -force=true
   ```

### Способ 2: Через Go (для разработчиков)

Если у вас установлен Go, вы можете запустить программу напрямую из исходного кода:

```bash
go run . -input="project_docs\ai_executable_plan.json" -output="project" -force=true
```

## Параметры командной строки

- `-input` (обязательный): путь к JSON файлу с описанием проекта
- `-output` (обязательный): путь к директории, где будет создана структура проекта
- `-force` (опциональный): флаг для перезаписи существующих файлов (по умолчанию false)

## Решение проблем

### Типичные ошибки

1. **"File not found"**
   - Проверьте, что путь к входному файлу указан правильно относительно текущей директории
   - Убедитесь, что у вас есть права на чтение файла

2. **"Permission denied"**
   - Проверьте права доступа к выходной директории
   - Убедитесь, что у вас есть права на запись
   - В Linux/macOS проверьте, что у исполняемого файла есть права на выполнение (`chmod +x`)

3. **"File already exists"**
   - Используйте флаг `-force=true` для перезаписи существующих файлов
   - Или выберите другую выходную директорию

4. **"Command not found" или "not recognized"**
   - Проверьте, что вы указываете правильный путь к исполняемому файлу
   - Убедитесь, что вы используете правильный разделитель пути для вашей ОС (`\` для Windows, `/` для Linux/macOS)

### Получение помощи

Если вы столкнулись с проблемой:

1. Проверьте сообщение об ошибке в консоли
2. Убедитесь, что все пути указаны правильно относительно текущей директории
3. Проверьте права доступа к файлам и директориям
4. Обратитесь к технической документации в `docs/tech_docs/` 