# Урок 5. Docker Compose - ДЗ

## Создать сервис, состоящий из 2 различных контейнеров: 1 - веб, 2 - БД (compose)

__Выполнение__

1. Для запуска этого проекта с помощью __докера__ необходимо выполнить две команды:

```
/$ docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:8.0.31

/$ docker run --name myphp -d --link some-mysql:db -p 8081:80 phpmyadmin/phpmyadmin
```
`Аргумент --link связывает наши 2 сущности`

2. Теперь выполним тот же проект с помощью __Docker Compose__:

* Создаем файл с расширением yaml

```
$ sudo nano docker-compose.yml
```

и прописываем команды внутри него:

```
# Указываем версию синтаксиса файла docker-compose
version: '3.1'

# Описываем сервисы, которые будут запущены
services:

  # Первый сервис: MySQL сервер
  some-mysql:
    # Используемый Docker образ
    image: mysql:8.0.31
    # Переменные окружения для MySQL
    environment:
      MYSQL_ROOT_PASSWORD: my-secret-pw
    # Монтируем том для сохранения данных MySQL
    volumes:
      - ./mysql_data:/var/lib/mysql

  # Второй сервис: phpMyAdmin
  myphp:
    # Используемый Docker образ
        image: phpmyadmin/phpmyadmin
    # Проброс портов: хост:контейнер
    ports:
      - 8081:80
 # Указываем зависимость от MySQL сервиса
    depends_on:
      - some-mysql
    # Переменные окружения для phpMyAdmin
    environment:
      PMA_HOST: some-mysql
```
* Сохраняем и запускаем его с помощью команды:
```
$ docker-compose up -d
```
* Теперь легко сможем зайти в myadmin по адресу 8081 в браузуре виртуальной машины с логином root и паролем, указанным при создании файла yml

Прилагаю скриншоты:



