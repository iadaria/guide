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

### Dockerfile

On server: > git clone https://our.project.git
() > cd our-project
() > nano ./apps/api/Dockerfile
() ~/docker-demo > ls
(. - current directory) ~/docker-demo > docker build -f ./apps/api/Dockerfile -t test:latest .
() ~/docker-demo > docker run --name api -d test:latest
() ~/docker-demo > docker ps
() ~/docker-demo > docker logs api
(test = 1.55GB -> 755MB) ~/docker-demo > docker images

### Оптимизация images

Утилита github.com/wagoodman/dive

() > dive <image-name>
(Changed dockerfile) >
() > docker build -t test:latest -f ./apps/api/Dockerfile .
() > docker run --name api -d test:latest || > docker run go2

### Пример сборки на golang

() >
(current dir) > docker build -t go .

### Сети Docker

Libnetwork:

- Network Namespace
- Linux Bridge
- Virtual Ethernet Devices
- IP Table

Bridge - default.
host - напрямую без слоев
overlay - Docker swarm - объединить несколько контейнеров
macvlan - добавляем типа новое физическое устройство. Уникальный MAC адрес для контейнера.
null - Без сети

### Docker network commands

connect - Подключить контейнер к сети
create - Создать сеть
disconnect - Отключение контейнера от сети
inspect - Параметры сети
ls -
prune -
rm -

() > docker network ls
() > docker network bridge
() >
() >

### Bridge

Когда хотим объединить несколько контейнеров в рамках одного хоста.
Используется для локальной разработки.
На продакш для небольшого сервиса и одного хоста.
Конт1 может пингануть Конт2 (рис eth-bridge)
Конт1 может пингануть Конт2, Конт2 может пингануть Конт3, но Конт1 не может пингануть Конт3. Для изолирования по разным сетям.(eth-bridge-2)

- Простые в конфигурации
- Обеспечивает локальный service discovery
- Работает только на 1й хост машине

(create am image) > docker build -t demo3:latest -f ./docker-demo-3/Dockerfile .
(docker-demo-3) > docker build -t demo3:latest .
(star a few containers from this image)
() > docker run --name=node-1 -d demo3:latest
() > docker run --name node-2 -d demo3:latest
(Онм подключились к сети bridge)
() > docker network inspect bridge
(enter into the first container) > docker exec -it node-1 sh
(ping container #2) > curl 172.17.0.3:3000
(could not resolve host) > curl node-2:3000
() > exit
(resolve the problem - create the network) > docker network create my-network
() > docker network ls
(connecting the network with the first network) > docker network connect my-network node-1
(connecting the network with the second network) > docker network connect my-network node-2
() > docker network inspect my-network
(enter to the second container) > docker exec -it node-2 sh
(pinging by name of container) > curl node-1:3000
() > docker run --name=node-3 --network my-network -d demo3:latest

- В bridge сразу не добавился тк сразу указали network в которой должен быть этот контейнер
  (inside the container - works) > curl localhost:3000
  (3000:3000 host port пробрасывать контейнер на порт 3000)
  (get error outside the container) > curl localhost:3000
- открыли порт контейнера node-4 наружу, в нашу хостовую машину, чтобы мы могли достучаться. открываем порт 3000.
  () > docker run --name=node-4 -p 3000:3000 --network my-network -d demo3:latest
  (now works) > curl localhost:3000

### Drivers host and null

- Будет работать на хостовой сети
  () > docker run --name node-5 -d --network host demo3
  () > docker network ls
  () > docker network inspect host
  () > curl 127.0.0.1:3000
  () > docker run --name node-6 -d --network none demo3
  () > docker exec -it <hash> shd
  () > curl localhost:3000
  result: {"eth0":["192.168.65.3"],"eth1":["192.168.64.2"],"services1":["192.168.65.6"]}/opt/app #

  (remove an image) > docker rm <image-name>
  (remove container) > docker rm <name-container>

### DNS

DNS - распределенная система для получения информации о доменах. Чаще всего используется для получения IP-адреса по имени хоста.

() > docker run node-5
() > cat /etc/resolv.conf -> 192.168.1.1
() > docker exec -it node-5 sh
() > cat /etc/resolv.conf -> 192.168.65.7
(change dns behavior ) > docker run --name node-8 -d --dns 8.8.8.8 demo3
() > cat /etc/resolv.conf

### Volumes

Зачем нужны:

- Персистентное хранение данных (например после удаления контейнера)
- Экспортирование логов
- Передача конфигов в контейнер
- Share данных между контейнерами

(listening on port 3000) > node src/index.js
() > curl "127.0.0.1:3000/set?id=123"
(result: 123) > curl "127.0.0.1:3000/get"
() > docker build -t demo4:latest .
() > docker volume ls
() > docker volume inspect <hash>
() > docker volume create demo
() > docker volume ls
(localization from 'Mountpoint')

- На хостовой машине взять volume demo и примонтировать его внутрь(чтобы папочка data отображалась внутри нашего volume)
  $ docker run --name volume-1 -d -v demo:/opt/app/data -p 3000:3000 demo4:latest
  $ curl "127.0.0.1:3000/set?id=1234"
- Проверяем что-то появилось
  $ sudo ls /var/lib/docker/volumes/demo/\_data
  (result: 1234): $ sudo cat /var/lib/docker/volumes/demo/\_data/req
