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

<img width="1435" alt="Снимок экрана 2023-10-06 в 11 42 33" src="https://github.com/LubaLuckshmi/docker/assets/120129430/8ee3d703-151d-4776-889d-07f48eafb3f4">
<img width="1439" alt="Снимок экрана 2023-10-06 в 11 44 45" src="https://github.com/LubaLuckshmi/docker/assets/120129430/e3254933-532f-4bda-bfad-c27ad3096522">
<img width="1440" alt="Снимок экрана 2023-10-06 в 11 47 24" src="https://github.com/LubaLuckshmi/docker/assets/120129430/6caa0ad1-f9d3-4aad-b5dc-3bc8bbab9e0c">

![VirtualBox_Ubundu-linux_06_10_2023_11_39_50](https://github.com/LubaLuckshmi/docker/assets/120129430/80955d3e-61f3-475a-a0df-e71fb2cd42d2)

