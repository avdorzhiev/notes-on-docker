# Docker Compose

## Что такое [Docker Compose](https://docs.docker.com/compose/)

Это инструмент, разработанный для помощи в определении и совместном использовании мульти-контейнерных приложений. С помощью Compose мы можем создать файл YAML для определения сервисов и с помощью одной команды вы можете запустить все или снести.

## Создание

1. В корне проекта создайте `docker-compose.yml` файл 
    
    ```yaml
    services:
      app:
        image: node:18-alpine
        command: sh -c "yarn install && yarn run dev"
        ports:
          - 3000:3000
        working_dir: /app
        volumes:
          - ./:/app
        environment:
          MYSQL_HOST: mysql
          MYSQL_USER: root
          MYSQL_PASSWORD: secret
          MYSQL_DB: todos
      mysql:
        image: mysql:8.0
        volumes:
          - todo-mysql-data:/var/lib/mysql
        environment:
          MYSQL_ROOT_PASSWORD: secret
          MYSQL_DATABASE: todos
    ```
    
    - `app, mysql`  - имя сервиса
    - `image` - имя образа
    - `command` - команда, которая близко к определению образа `image`
    - `ports` - порты *<Host_Port>*:*<Container_Port>*
    - `working_dir` - Рабочая директория
    - `voluems` - Отображение томов
    - `environment` - Переменные среды
2. Запустите свой стек приложений с помощью команды `docker compose up`. *(Добавьте флаг -d для запуска в фоновом режиме)*