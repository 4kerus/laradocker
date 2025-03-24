# Laradocker

Этот проект предоставляет готовую среду для развертывания Laravel с использованием Docker.
- Nginx (`stable-alpine`) 
- PHP (`8.4-fpm`)  
- Node.js (`20-alpine`)
- MySQL (`8.0`)

## Требования
Docker + Docker Compose

## Установка

### 1. Клонирование репозитория
```sh
git clone https://github.com/4kerus/laradocker.git && cd laradocker
```

### 2. Сборка контейнеров
```sh
docker compose pull && docker compose build
```

### 3. Создание нового Laravel-проекта
```sh
docker compose run --rm php composer create-project laravel/laravel laravel --prefer-dist \
  && mv README.md README-docker.md \
  && sudo mv -f ./laravel/* ./laravel/.* ./ \
  && sudo rm -rf ./laravel
```

### 4. Настройка переменных окружения
Создайте файл `.env` и укажите настройки базы данных:
```sh
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=secret
```

### 5. Запуск контейнеров
```sh
docker compose up -d
```

### 6. Запуск миграций
```sh
docker compose exec php php artisan migrate
```

### 7. Установка прав на папки
```sh
docker compose exec php chmod -R 775 storage bootstrap/cache \
  && docker compose exec php chown -R www-data:www-data storage bootstrap/cache
```

## Остановка контейнеров
```sh
docker compose down
```

## Дополнительная информация
- После установки доступ к приложению можно получить по адресу `http://localhost`.
- Для работы с Artisan используйте `docker compose exec php php artisan <command>`.
- Для работы с Npm используйте `docker compose run --rm node npm <command>`.
