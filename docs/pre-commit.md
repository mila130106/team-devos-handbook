# Налаштування Git Hooks

Для підтримки чистоти нашого репозиторію та уникнення випадкових помилок, кожен учасник команди повинен налаштувати локальний `pre-commit` хук. Він забороняє робити прямі коміти у гілку `main`.

## Як встановити хук

1. Відкрийте термінал у корені проєкту.
2. Виконайте наступну команду, щоб створити скрипт:

```bash
cat << 'EOF' > .git/hooks/pre-commit
#!/bin/bash
current_branch=$(git rev-parse --abbrev-ref HEAD)
if [ "$current_branch" = "main" ]; then
  echo "❌ Помилка: Прямі коміти в гілку main заборонені!"
  echo "👉 Створіть окрему гілку (git checkout -b feature/назва) та використовуйте Pull Request."
  exit 1
fi
exit 0
EOF