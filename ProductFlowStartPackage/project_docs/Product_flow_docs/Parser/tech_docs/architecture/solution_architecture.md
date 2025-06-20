# Архитектура решения

## Высокоуровневая диаграмма

```ascii
       [ User/CI ]
            |
        (executes)
            |
            v
[ ./parse_master_plan (Go Executable) ]
|           |           |
|           |           +----(reads)----> [ ai_executable_plan.json (input) ]
|           |
|           +----(writes)---> [ File System (output directory) ]
|                                       |
|                                       +-- docs/
|                                       |   |-- tech_docs/
|                                       |   |   |-- architecture/
|                                       |   |   |-- api/
|                                       |   |   +-- deployment/
|                                       |   +-- user_docs/
|                                       |
|                                       +-- tasks/
|                                           |-- dev/
|                                           |   +-- task_1.1.json
|                                           |-- deploy/
|                                               +-- task_2.1.json
|
+-----(logs to)-----> [ Console (stdout/stderr) ]
```

## Логическая архитектура

Приложение построено как единый CLI-инструмент на Go. Логическая структура разделена на пакеты для соблюдения принципа разделения ответственности (SRP):

- `main` package: Точка входа, парсинг CLI-флагов, оркестрация вызовов других пакетов
- `parser` package: Ответственен за чтение, десериализацию и валидацию входного JSON в Go-структуры
- `generator` package: Ответственен за создание файловой структуры, включая документационные артефакты и файлы задач

```ascii
[ main (CLI) ] -> [ parser (Validation) ] -> [ generator (File Ops) ]
```

## Физическая архитектура

Физическая архитектура представляет собой один скомпилированный бинарный файл без внешних зависимостей. Он может быть запущен на любой целевой ОС/архитектуре (Linux, macOS, Windows), для которой он был скомпилирован. Не требует установки рантайма (как Node.js).

```ascii
[ Target Machine (Linux/macOS/Windows) ]
     |
     +-- [ ./parse_master_plan (Compiled Binary) ]
         |
         +-- (interacts with) --> [ Local File System ]
```

## Стек технологий

- **Go (версия 1.18+)**
  - Выбран для решения проблемы производительности
  - Компилируемый язык со статической типизацией
  - Высокая скорость выполнения и надежность
  - Минимальное потребление ресурсов
  - Встроенные возможности для работы с JSON, файловой системой и конкурентного выполнения
  - Результат - один бинарный файл без зависимостей

## Принципы проектирования

1. **KISS (Keep It Simple, Stupid)**
   - Реализация с использованием только стандартной библиотеки Go

2. **SRP (Single Responsibility Principle)**
   - Логика разделена на пакеты (`parser`, `generator`)

3. **DRY (Don't Repeat Yourself)**
   - Функции для записи файлов и обработки ошибок являются переиспользуемыми

## Модели данных

### Структуры для ai_executable_plan.json

```go
type Plan struct {
  SolutionArchitecture any    `json:"solutionArchitecture"`
  DevelopmentPlan      any    `json:"developmentPlan"`
  DeploymentPlan       any    `json:"deploymentPlan"`
  Tasks                []Task `json:"tasks"`
}

type Task struct {
  TaskNumber  string   `json:"task_number"`
  Responsible string   `json:"responsible"`
  Subtasks    []Subtask `json:"subtasks"`
  // ... other fields
}
```

## Атрибуты качества

### Безопасность
- Угроза Path Traversal митигируется с помощью `filepath.Clean` и `filepath.Abs`
- Перед записью производится проверка, что результирующий путь является дочерним по отношению к --output директории

### Масштабируемость
- Вертикальная масштабируемость обеспечивается высокой производительностью Go
- Эффективная работа с памятью
- Возможность обработки JSON-файлов значительного размера (сотни МБ) без заметной деградации

### Отказоустойчивость
- Надежность обеспечивается статической типизацией
- Явная обработка ошибок в Go (`if err != nil`)
- Каждая I/O операция или операция парсинга возвращает ошибку
- Предсказуемое поведение при ошибках 