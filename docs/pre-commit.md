# Налаштування Git Hooks

Для дотримання стандартів коду та уникнення помилок кожен розкручуник налаштовує локальний `pre-commit` хук. Він виконує дві перевірки:
1. Забороняє прямі коміти у гілку `main`.
2. Перевіряє відсутність зайвих пробілів у кінці рядків (trailing whitespace).

## Як встановити хук

Виконайте в терміналі у корені репозиторію:

```bash
cat << 'EOF' > .git/hooks/pre-commit
#!/bin/bash

# 1. Перевірка на заборону прямих комітів у main
current_branch=$(git rev-parse --abbrev-ref HEAD)
if [ "$current_branch" = "main" ]; then
  echo "❌ Помилка: Прямі коміти в гілку main заборонені!"
  exit 1
fi

# 2. Перевірка на trailing whitespace
git diff --cached --check --
if [ $? -ne 0 ]; then
  echo "❌ Помилка: Знайдено зайві пробіли в кінці рядків!"
  exit 1
fi

exit 0
EOF

chmod +x .git/hooks/pre-commit