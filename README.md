# Loop Tasks → Orchestration

Скилл переехал в [Orchestration](https://github.com/di-sukharev/orchestration-skill). Этот репозиторий больше не поддерживается.

Orchestration выполняет задачи по очереди. Каждую задачу ведёт новый саб-агент: изучает код, пишет план, реализует и запускает независимое ревью через Loop Code Review в рамках своей задачи.

## Переход

Отправьте агенту это сообщение.

```text
Установи скиллы глобально
https://github.com/di-sukharev/orchestration-skill
https://github.com/di-sukharev/loop-code-review-skill
```

Вместо `/loop-tasks <задачи>` используйте `/orchestration <задачи>`. Можно передать одну задачу, список или файл с задачами.

[Orchestration](https://github.com/di-sukharev/orchestration-skill) · [Лицензия MIT](LICENSE)
