# Loop Tasks

Когда одна сессия выполняет задачи подряд, старые задачи засоряют контекст. Loop Tasks запускает для каждой задачи нового агента с чистым контекстом. Каждый агент пишет код, проверяет код через [Loop Code Review](https://github.com/di-sukharev/loop-code-review-skill), делает коммит и пуш.

## Установка

Отправьте агенту это сообщение.

```text
Установи скиллы глобально
https://github.com/di-sukharev/loop-tasks-skill
https://github.com/di-sukharev/loop-code-review-skill
```

## Запуск

Напишите `/loop-tasks <задачи>`. В Codex напишите `$loop-tasks <задачи>`. Вместо списка задач можно указать файл с задачами. Если пуш не нужен, допишите «без пуша».

## Другие скиллы

- [Code Scout](https://github.com/di-sukharev/code-scout-skill) поручает поиск кода дешёвой модели.
- [Orchestration](https://github.com/di-sukharev/orchestration-skill) поручает чтение и написание кода дешёвой модели.
- [Loop Code Review](https://github.com/di-sukharev/loop-code-review-skill) отдаёт код новому ревьюеру без истории чата.

[Инструкция для агента](loop-tasks/SKILL.md) · [Лицензия MIT](LICENSE)
