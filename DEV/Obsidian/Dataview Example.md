---
tags:
  - obsidian
  - dataview
type: reference
---

# Dataview Example

**Практическая шпаргалка** · заметки · проекты · задачи · связи

[[Obsidian]] · [[ShotKeys Obsidian]]

> [!abstract] Что делает Dataview
> Собирает списки и таблицы из заметок, свойств и задач вашего хранилища. Результаты обновляются при изменении исходных данных.

> [!tip] Как пользоваться примерами
> Код ниже показан как текст, чтобы его было удобно копировать. Создайте блок кода из трёх обратных кавычек, укажите после них **dataview**, вставьте запрос и закройте блок тремя кавычками. Посмотрите результат в режиме чтения — **Ctrl + E**. JavaScript для этих примеров не нужен.

**Навигация:** [[#🧭 Заметки и поиск|Заметки]] · [[#📋 Свойства и проекты|Проекты]] · [[#✅ Задачи и сроки|Задачи]] · [[#🔗 Связи и порядок|Связи]] · [[#⚡ Живой пример|Живой пример]]

## 🧩 Минимум синтаксиса

| Команда | Назначение |
| :-- | :-- |
| `LIST` | Список ссылок |
| `TABLE` | Таблица свойств |
| `TASK` | Задачи с чекбоксами |
| `CALENDAR` | Календарь по полю даты |
| `FROM` | Папка, тег или ссылки |
| `WHERE` | Условие отбора |
| `SORT … ASC / DESC` | Сортировка по возрастанию / убыванию |
| `LIMIT` | Ограничение количества результатов |
| `GROUP BY` | Группировка |

Пути считаются от корня хранилища: `"DEV/Obsidian"`, без диска и расширения `.md`. Папка в `FROM` включает вложенные папки. Без `FROM` запрос обращается ко всему хранилищу.

---

## 🧭 Заметки и поиск

### 01 · Содержание папки

Автоматическое оглавление ваших заметок об Obsidian; текущая заметка исключена.

```text
LIST
FROM "DEV/Obsidian"
WHERE file.path != this.file.path
SORT file.name ASC
```

### 02 · Последние изменения

Десять недавно изменённых заметок из папки разработки.

```text
TABLE file.mtime AS "Изменено", file.folder AS "Папка"
FROM "DEV"
WHERE file.path != this.file.path
SORT file.mtime DESC
LIMIT 10
```

### 03 · Отбор по тегу

Все заметки с тегом `#project`, кроме папки шаблонов. Замените тег на свой.

```text
LIST
FROM #project AND -"template"
SORT file.name ASC
```

### 04 · Создано за последние семь дней

Сегодня и шесть предыдущих календарных дней. `file.cday` — дата создания файла; после переноса файлов она может отличаться от исходной даты заметки.

```text
TABLE file.cday AS "Создано"
WHERE file.cday >= date(today) - dur(6 days)
  AND file.cday <= date(today)
SORT file.cday DESC
```

### 05 · Поиск по имени

Поиск названия без учёта регистра. Это не полнотекстовый поиск содержимого.

```text
LIST
WHERE contains(lower(file.name), "obsidian")
SORT file.name ASC
```

---

## 📋 Свойства и проекты

> [!example] Подготовка данных
> Для примеров 06–08 добавьте свойства в свои проектные заметки. YAML ниже должен находиться в самом начале исходной заметки, вне блока кода. Если свойства уже есть, дополните существующий блок.

```yaml
---
tags:
  - project
status: active
priority: 1
due: 2026-10-01
---
```

`status` — текст (`active` / `done`), `priority` — число (1 — важнее), `due` — дата `YYYY-MM-DD`. Дата выше служит примером. Альтернатива YAML для отдельного поля — строка `status:: active` в тексте заметки; дублировать одно поле двумя способами не нужно.

### 06 · Активные проекты

Сначала проекты с наивысшим приоритетом. `TABLE WITHOUT ID` позволяет самостоятельно назвать столбец со ссылкой.

```text
TABLE WITHOUT ID file.link AS "Проект",
  priority AS "Приоритет", due AS "Срок"
FROM #project
WHERE status = "active"
SORT priority ASC, due ASC
```

### 07 · Проекты по статусам

Сводка: статус, количество проектов и ссылки. После группировки исходные записи доступны через `rows`.

```text
TABLE length(rows) AS "Количество", rows.file.link AS "Проекты"
FROM #project
GROUP BY status
SORT key ASC
```

### 08 · Календарь проектных сроков

Показывает проекты с заполненным `due` на календаре. Это срок заметки-проекта, а не отдельной задачи.

```text
CALENDAR due
FROM #project
WHERE due
```

---

## ✅ Задачи и сроки

В исходной заметке задачи записываются обычными Markdown-чекбоксами. Для срока добавьте поле прямо в строку задачи:

```markdown
- [ ] Проверить настройки [due:: 2026-10-01]
- [ ] Дополнить инструкцию
- [x] Установить Dataview
```

> [!info] Интерактивные результаты
> Отметка чекбокса в результате `TASK` меняет задачу в исходном файле. `!completed` выбирает задачи, не отмеченные как выполненные; нестандартные статусы тоже могут попасть в выборку.

### 09 · Открытые задачи по файлам

Собирает незавершённые задачи из `DEV` и группирует по исходной заметке.

```text
TASK
FROM "DEV"
WHERE !completed
GROUP BY file.link
```

### 10 · Просроченные задачи

Все незавершённые задачи со сроком раньше сегодняшнего дня.

```text
TASK
WHERE !completed AND due AND due < date(today)
SORT due ASC
```

### 11 · Сегодня и следующие шесть дней

Ближайшие задачи без просроченных. Записи без срока не показываются.

```text
TASK
WHERE !completed AND due
  AND due >= date(today)
  AND due < date(today) + dur(7 days)
SORT due ASC
```

### 12 · Задачи без срока

Помогает разобрать список дел, которым ещё не назначена дата.

```text
TASK
FROM "DEV"
WHERE !completed AND !due
GROUP BY file.link
```

---

## 🔗 Связи и порядок

### 13 · Кто ссылается на текущую заметку

Удобно для страниц тем и проектов. `this.file.link` означает заметку, в которой выполняется запрос.

```text
LIST
WHERE contains(file.outlinks, this.file.link)
  AND file.path != this.file.path
SORT file.name ASC
```

### 14 · Заметки без входящих ссылок

Кандидаты для включения в оглавления. Отсутствие входящих ссылок не означает, что заметка не нужна.

```text
LIST
FROM "DEV"
WHERE length(file.inlinks) = 0
SORT file.name ASC
```

### 15 · Количество заметок по папкам

Быстрая сводка по разделу разработки; каждая вложенная папка считается отдельно.

```text
TABLE length(rows) AS "Заметок"
FROM "DEV"
GROUP BY file.folder
SORT length(rows) DESC
```

### 16 · Проекты без статуса

Находит записи, в которых забыли заполнить свойство `status`.

```text
LIST
FROM #project
WHERE !status
SORT file.name ASC
```

---

## ⚡ Живой пример

Этот блок уже выполняется: пять последних изменённых заметок из вашей папки Obsidian. Его исходный код виден в режиме редактирования.

```dataview
TABLE WITHOUT ID file.link AS "Заметка", file.mtime AS "Изменено"
FROM "DEV/Obsidian"
WHERE file.path != this.file.path
SORT file.mtime DESC
LIMIT 5
```

## 🛠 Если результат пустой

- Проверьте имя папки, тег и наличие нужных свойств в исходных заметках.
- `#project`, `status`, `priority` и `due` в примерах — предлагаемая схема; добавьте её или замените на свою.
- Укажите язык блока `dataview`, а не `text`. Учебные блоки выше намеренно показывают код.
- Используйте прямые кавычки `"`, даты `YYYY-MM-DD` и одинаковый регистр значений: `active` и `Active` различаются.
- Пустая выборка без сообщения об ошибке обычно означает, что подходящих данных пока нет.

> [!quote] Официальная документация
> [Типы результатов](https://blacksmithgu.github.io/obsidian-dataview/queries/query-types/) · [Команды запросов](https://blacksmithgu.github.io/obsidian-dataview/queries/data-commands/) · [Поля заметок](https://blacksmithgu.github.io/obsidian-dataview/annotation/metadata-pages/) · [Добавление свойств](https://blacksmithgu.github.io/obsidian-dataview/annotation/add-metadata/) · [Поля задач](https://blacksmithgu.github.io/obsidian-dataview/annotation/metadata-tasks/)
