# Налаштування Git Hooks

Для підтримки чистоти нашого репозиторію та уникнення помилок кожен учасник команди повинен налаштувати локальний `pre-commit` хук. Він виконує дві перевірки:
1. Забороняє прямі коміти у гілку `main`.
2. Перевіряє відсутність зайвих пробілів у кінці рядків (trailing whitespace).

## Як встановити хук

1. Відкрийте термінал у корені проєкту.
2. Виконайте наступну команду, щоб створити скрипт:

```bash
cat << 'EOF' > .git/hooks/pre-commit
#!/bin/bash

# 1. Перевірка на заборону прямих комітів у main
current_branch=$(git rev-parse --abbrev-ref HEAD)
if [ "$current_branch" = "main" ]; then
  echo "❌ Помилка: Прямі коміти в гілку main заборонені!"
  echo "👉 Створіть окрему гілку (git checkout -b feature/назва) та використовуйте Pull Request."
  exit 1
fi

# 2. Перевірка на trailing whitespace
git diff --cached --check --
if [ $? -ne 0 ]; then
  echo "❌ Помилка: Знайдено зайві пробіли в кінці рядків (trailing whitespace)!"
  echo "👉 Видаліть зайві пробіли перед комітом."
  exit 1
fi

exit 0
EOF