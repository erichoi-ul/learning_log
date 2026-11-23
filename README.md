# Learning Log

Веб-приложение на Django для ведения журнала обучения: пользователи могут регистрироваться, создавать собственные темы и добавлять или редактировать заметки по ним.

## Структура проекта
- `learning_log/` — основные настройки проекта и корневые маршруты.
- `learning_logs/` — модели `Topic` и `Entry`, формы, представления, шаблоны и автоматические тесты.
- `users/` — регистрация, вход и выход пользователей.
- `Dockerfile`, `compose.yaml` — конфигурация для развёртывания в Docker (Gunicorn + WhiteNoise).

## Локальный запуск (Windows)
1. Перейдите в каталог проекта и активируйте виртуальное окружение:
   ```powershell
   .\(\ll_env\Scripts\Activate.\)ps1
   ```
2. Установите зависимости:
   ```powershell
   python -m pip install -r requirements.txt
   ```
3. Примените миграции и запустите сервер разработки:
   ```powershell
   python manage.py migrate
   python manage.py runserver
   ```
4. Для запуска автоматических тестов выполните:
   ```powershell
   python manage.py test
   ```

## Развёртывание через Docker Compose
1. Клонируйте репозиторий на сервер и создайте в корне файл `.env` с переменными окружения.
2. Запустите сборку и контейнер в фоновом режиме:
   ```bash
   docker compose up -d --build
   ```
3. При необходимости создайте суперпользователя внутри контейнера:
   ```bash
   docker compose exec web python manage.py createsuperuser
   ```

## Переменные окружения (`.env`)

| Переменная | Описание | Пример значения |
| :--- | :--- | :--- |
| `DJANGO_SECRET_KEY` | Секретный ключ криптографической подписи Django | `replace-with-a-long-random-secret` |
| `DJANGO_DEBUG` | Режим отладки (`False` для продакшн-сервера) | `False` |
| `DJANGO_ALLOWED_HOSTS` | Разрешённые IP-адреса и домены через запятую | `192.168.1.11,localhost,127.0.0.1` |
| `DJANGO_CSRF_TRUSTED_ORIGINS` | Доверенные адреса для защиты форм CSRF | `http://192.168.1.11:20010` |
| `DJANGO_USE_HTTPS` | Включение защищённых cookie для HTTPS (`True`/`False`) | `False` |
| `SQLITE_PATH` | Путь к файлу базы данных SQLite внутри контейнера | `/app/data/db.sqlite3` |
| `APP_BIND_IP` | IP-адрес сетевого интерфейса для публикации порта | `0.0.0.0` |
| `APP_PORT` | Внешний порт приложения, выделенный на сервере | `20010` |
