### Проблемы классической развертки приложения

- Конфликт версий зависимостей
- Приложение работа на сервере А но не работает на В.
- Не безопасный запуск
- Невозможность масштабирования

### Что даем Ansible?

- Простую автоматизацию сложных и повторяющихся задач
- Хранение вашей конфигурации как код
- Экономия времени и переносимость решений
- Декларативная система описаний через YML
- Нет необходимости дополнительного ПО на серверах

### Что дает Docker?

- Консистентное изолированное окружение для запуска приложений
- Простое управление и обновление / откат приложений
- Легкое масштабирование приложений под нагрузкой
- Удобный способ доставки приложений
- Простая работа с сетью и Service Discovery

Namespace, использует ядро linux,
pws - api

> docker run -it node

Просто процесс ОС.
Запуск контейнера - создание нового namespace.
Получим node-овские процессы. Namespace со своим юзером и там наше приложение.

### commands

> docker ps
> (every new container)> docker run
> (stopped containers0)>docker ps -a
> (remove container)>docker rm <name>
> (remove container)>docker rm <id>
> (remove started container f-force)>docker rm <name> -f
> (delete all stopped containers)> docker container prune
> (change default name)> docker rename <old-name> <newname>
> ()>

# example - mongo db

d = detach

> docker run --name my-mongo -d mongo
> docker ps
> docker ps -a
> docker kill --signal=1 my-mongo

# logs

Любой контейнер генерирует логи
(Mem usage/limits/CPU)> docker stats
(get info of containers) > docker inspect <name>
(exactly item - part of JSON)> docker inspect -f "{{.State.Status}}" <name>
() > docker logs <name>
() > docker logs <name> | grep "id"
() > docker logs <name> | grep "22262"
(before 10 rows) > docker logs <name> | grep "id" -B 10
(after 10 rows) > docker logs <name> | grep "id" -A 10
(2 first rows) > docker logs <name> | grep "id" -m 2
(2 first rows) > docker logs <name> | grep "id.\*" -m 2
() > docker logs <name> -f
() > docker logs <name> | grep "13" > test-new.txt

# docker exec

() > docker exec -w /root <name> pwd
() > docker exec <name> pwd
(show all envs from container) > docker exec -e MYVAR=1 <name> printenv
() > docker exec -it <name> bash
() > docker exec <name> mongod --version (worked)
(outside the container) > docker exec <name> mongod --version > mongo.txt
(inside the container) > docker exec <name> bash -c 'mongo --version >mongo2.txt'
(1 check mongo2.txt) > docker exec -it <name> bash
(2 check mongo2.txt) > ls
() >

# task

- create a new container mongodb by using 'create' command
- start the container
- put a file itsalive.txt inside the container
- stop the container
- start again and be sure that the file exists

() > docker create --name mongodb mongo
() > docker ps -a
() > docker start mongodb
() > docker ps
() > docker exec mongodb -c 'touch itsalive.txt'
() > docker exec mongodb -c 'cat insalive.txt'
() > docker stop mongodb
() > docker start mongodb
() > docker exec mongodb bash -c 'ls'
() > docker exec mongodb bash -it mongodb bash
() > docker ls

### Docker image

# What is it?

Когда делала docker run - скачивали с registry и запускали внутри
Слоеная архитектура. img каждый этап какой-то сборки.

> docker pull nginx

Выкачиваем nginx с Registry. скачивает несколько слоев(docker-img-1)
Когда после img создадим контейнер добавим еще один img(docker-img-2)
Для разных контейнеров используется один и тот же слой, для оптимизации.
Переиспользуется слои в контейнерах которые не связанных.

() > docker images
(save nginx to archive) > docker save --output nginx.tar nginx
() > mkdir nginx
() > tar xvf nginx.tar -C nginx
() > cd nginx & ls
Есть папки 6 штук - наши слои которые отображались при скачивании. Есть json файлы.
(History how was created) > docker history nginx
(How much?) > sudo du -sh /var/lib/docker/overlay2
() > ls ./.docker -a
() > sudo ls ?/containers
(from archive) > docker image import

() > docker pull ubuntu
() > docker push <name>

() > docker image ls
() > docker image ls --format {{.Tag}}
() > docker image ls --filter "before=node"

() > docker image rm <name>
(Чтобы удалить image нужно остановить все контейнеры которые на него ссылаются)
() > docker image rm <name>:latest -f
(docker image:latest) > docker images
(создали такой же image с новым именем и тегом) > docker tag nginx:latest node:latest
(удаляем image у которых tag равен <none>) > docker image prune

## Свой image

Каждая новая строка - новый слой.

Контекст выполнения - это путь выполнения команды docker build со всеми вложенными директориями. Как удалить из контекста? - .dockerignore

# dockerfile команды

(Аргументы сборки, только на момент билда для переиспользования)
() > ARG CODE*VERSION=latest
(для передачи из вне)
()> ARG MY_ARG
() >
(Базовый образ)
() > FROM node:14-alpine as build
(Если кто-то базируется на образе)
() > ONBUILD ADD . /app/src
() >
(Мета информация, останется в финальном image)
() > LABEL version="1.0"
() >
(Настройка пользователя)
() > USER root
(Рабочая директория)
() > WORKDIR /opt/app
() >
(Добавления файлов в образ. ADD - внутрь контейнера, умеет разархивировать и работает с url.)
() > ADD *.json ./
() > ADD --chown=root files\_ /somedir/
() > myarc.tar.gz /somedir/
() > ADD http://url.ru/mydata.tar.gaz /somedir/
() > COPY .json ./
() >
(Установка shell)
() > SHELL ["/bin/sh", "-c"]
(Выполнение команды в shell, RUN выполнить команду shell)
() > RUN echo hello
(Переменные окружения. Будет и в финальном image)
() > ENV FOO=1
() > ENV BAR=$FOO
() > RUN DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -y
(Создание volume на host машине)
() > VOLUME ["/data"]
(Команды запуска, или ENTRYPOINT или CMD)
() > ENTRYPOINT ["top", "-c"]
() > CMD ["node", "./dist/apps/api/main.js"]
(Сигнал при остановке контейнера)
() > STOPSIGNAL 9
(Указание порта на котором будет слушать запросы, документация теоретически - НЕ ПРОКИДЫВАЕТ)
() > EXPOSE 80/tcp

() > # escape=` (backtick)` - Инструкция для парсера
() >
