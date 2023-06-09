# Container (Контейнер)

## Что такое контейнер?

Это изолированный процесс на вашей машине, который изолирован от всех других процессов на хост-машине.

- Это запускаемый экземпляр образа. Вы можете создавать, запускать, останавливать, перемещать или удалять контейнер с помощью DockerApi или CLI
- Может быть запущен на local (or virtual) machine или в облаке
- Может быть запущен на любой ОС
- Изолирован от других контейнеров и запускает собственное ПО, бинарники и конфиги

## Запуск контейнера

*Note: перед запуском контейнера. нужно [создать образ](Image%20(Образ).md)*

1. Запустите свой контейнер, используя команду `docker run` и укажите имя образа

```powershell
docker run -dp 3000:3000 <Имя_Образа>
```

- `-d` - это флаг для запуска нового контейнера в режиме “*detached”* (in the background)
- `-p` - это флаг для создания сопоставления между портом хоста (3000) и портом контейнера (3000). Без сопоставления портов, вы не сможете получить доступ к приложению.
1. Откройте веб-браузер по адресу [http://localhost:3000](http://localhost:3000/)

Чтобы посмотреть на запущенные контейнеры, используйте команду `docker ps` или графический интерфейс Docker Desktop.

## Удаление контейнера

Вы не можете запустить новый контейнер, пока ваш старый контейнер все еще работает.

Причина в том, что старый контейнер уже использует порт хоста, и только один процесс на машине (включая контейнеры) может прослушивать определенный порт.

1. Получите ID контейнера, используя команду `docker ps`.
2. Используйте команду `docker stop` для остановки контейнера
    
    ```powershell
    docker stop <ID-container>
    ```
    
3. Используйте команду `docker rm` для удаления контейнера
    
    ```powershell
    docker rm <ID-container>
    ```
    

## Запуск контейнера с использованием тома

Запустите контейнер используя команду `docker run` с флагом `--mount`, чтобы указать монтирование тома

```powershell
docker run -dp 3000:3000 --mount type=<Тип_Тома>,src=<Имя_Тома>,target=<Путь_Монтирования> <Имя_Образа>
```

## Запуск контейнера с использованием Bind Mount

```powershell
docker run -dp 3000:3000 `
    -w /app --mount type=bind,src="$(pwd)",target=/app `
    node:18-alpine `
    sh -c "yarn install && yarn run dev"
```

- `w /app` - Устанавливает «рабочий каталог» или текущий каталог, из которого будет запускаться команда
- `-mount type=bind,src="$(pwd)",target=/app` - привязывает монтирование текущего каталога с хоста в каталог */app*
- Команды для запуска:
    - `node:18-alpine` - Образ для использования
    - `sh` - Запускает оболочку
    - `yarn install` - Устанавливает все пакеты из `package.json`
    - `yarn run dev` - Запускает приложение.