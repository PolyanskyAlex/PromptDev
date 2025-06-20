# Работа с JSON

## Структура манифеста

Входной JSON файл парсится в следующие Go структуры:

```go
type Plan struct {
    SolutionArchitecture SolutionArchitecture `json:"solutionArchitecture"`
    DevelopmentPlan     DevelopmentPlan      `json:"developmentPlan"`
    DeploymentPlan      DeploymentPlan       `json:"deploymentPlan"`
    Tasks               []Task               `json:"tasks"`
}
```

Полная схема всех структур доступна в файле `types.go`.

## Парсинг JSON

Для чтения и парсинга JSON используется функция `readAndParsePlan`:

```go
func readAndParsePlan(filePath string) (*Plan, error)
```

### Этапы работы функции

1. Чтение файла с помощью `os.ReadFile`
2. Десериализация JSON в структуры с помощью `json.Unmarshal`
3. Возврат указателя на заполненную структуру `Plan` или ошибки

### Пример использования

```go
plan, err := readAndParsePlan("path/to/plan.json")
if err != nil {
    log.Fatal(err)
}

// Доступ к данным
fmt.Printf("Number of tasks: %d\n", len(plan.Tasks))
```

## Обработка ошибок

### Типы ошибок

1. **Ошибки чтения файла**
   - Файл не существует
   - Нет прав на чтение
   - Проблемы с кодировкой

2. **Ошибки парсинга**
   - Некорректный JSON синтаксис
   - Несоответствие типов данных
   - Отсутствие обязательных полей

### Примеры обработки ошибок

```go
plan, err := readAndParsePlan("plan.json")
switch {
case os.IsNotExist(err):
    log.Fatal("File does not exist")
case os.IsPermission(err):
    log.Fatal("Permission denied")
case err != nil:
    if _, ok := err.(*json.SyntaxError); ok {
        log.Fatal("Invalid JSON syntax")
    }
    log.Fatal("Unknown error:", err)
}
```

## Валидация данных

### Проверки при парсинге

1. **Обязательные поля**
   - Проверка наличия всех required полей
   - Проверка корректности типов данных

2. **Зависимости между полями**
   - Проверка корректности связей между задачами
   - Валидация ссылок на другие части плана

### Пример валидации

```go
func validatePlan(plan *Plan) error {
    if plan.Tasks == nil {
        return errors.New("tasks section is required")
    }
    
    for _, task := range plan.Tasks {
        if task.TaskNumber == "" {
            return errors.New("task number is required")
        }
        // Другие проверки...
    }
    
    return nil
}
```

## Генерация документов

Функция `generateDocuments` отвечает за создание документационных артефактов из структуры `Plan`. Она создает три файла в директории `project_docs`:

- `SAD.md` - документ с архитектурой решения
- `dev_plan.md` - план разработки
- `deploy_plan.md` - план развертывания

### Использование

```go
func generateDocuments(plan *Plan, outputDir string, force bool) error
```

#### Параметры

- `plan` - указатель на структуру `Plan`, содержащую данные для генерации
- `outputDir` - путь к директории, где будут созданы документы
- `force` - флаг, разрешающий перезапись существующих файлов

#### Возвращаемые значения

- `error` - ошибка, если что-то пошло не так

### Пример использования

```go
plan, err := readAndParsePlan("input.json")
if err != nil {
    log.Fatal(err)
}

err = generateDocuments(plan, "output", true)
if err != nil {
    log.Fatal(err)
}
```

### Обработка ошибок

Функция возвращает ошибку в следующих случаях:
- Не удалось создать директорию `project_docs`
- Не удалось сериализовать структуры в JSON
- Не удалось записать файлы (и `force=false` при попытке перезаписи) 