# Курсы
- https://stepik.org/course/123300

# Песочницы
- https://labs.play-with-docker.com/
- https://codesandbox.io/p/devbox

# Команды linux
- cat /etc/\*rel\* - версия ОС 
- sudo apt-get remove docker docker-engine docker.io containerd runc - удаление docker
- sudo apt install curl
- ipaddr - сетевые адреса хоста
- export DOCKER\_HOST=ip\_addr - указать хост докера

# Установка docker
- https://docs.docker.com/engine/install/ubuntu/
- curl -fsSL https://get.docker.com -o get-docker.sh
- sudo sh get-docker.sh
- apt install docker.io - установка через apt
- sudo usermod -aG docker $USER

# Команды docker
- docker - список команд
- docker version - версия docker
- sudo docker run docker/whalesay cowsay Hi! 
- docker search whalesay - поиск образа
- sudo docker pull carolynvs/whalesayd - скачать образ
- docker run имя\_контейнера - запуск контейнера или скачивание образа и запуск
- docker run имя\_контейнера:версия - запуск контейнера или скачивание образа и запуск
- docker run ubuntu sleep 5 - запуск контейнера и процесса
- docker run -d имя\_контейнера - запуск контейнера в фоне
- docker run --name webapp nginx - запуск контейнера nginx под новым именем webapp 
- docker attach id\_контейнера - возврат исполнения контейнера из фона
- docker run -it alpine sh - запуск sh в контейнере alpine в интерактивном режиме
- docker run -p 80:5000 robotocloud/webapp - связывание портов
- docker run -v /opt/datadir/:/var/lib/mysql mysql - связывание ресурсов
- docker run -e VAR=value image_\name - установить переменную окружения внутри контейнера
- docker ps - спиcок запущенных контейнеров 
- docker ps -a = список всех контейнеров
- docker inspect containerID - информация о контейнере
- docker logs containerID - логи контейнере
- docker stop имя/id контейнера
- docker rm имя/id контейнера - удаление контейнера
- docker rm $(docker ps -aq) - удалить все контейнеры
- docker container prune - удалить все контейнеры
- docker images - список всех доступных на хосте образов
- docker images rmi имя образа - удаление образа (если нет его контейнеров)
- docker rmi -f $(docker images) - удалить все образы
- docker exec имя\_контейнера cat etc/hosts = выполнение команды в запущенном контейнере 
- docker build . -t image\_name - создать образ на основе Dockerfile 
- docker pusj image\_name - залить образ в docker\_hub
- docker history имя - информация о слоях образа
- docker tag hash\_name image\_name:image\_tag - присвоить имя и тег образу
- docker login - подключится к docker hub

# Dockerfile
- FROM - образ на базе которого создается новый образ контейнера
- RUN - запустить команду на указанном образе
- COPY app.py /opt/app.py - скопировать файл app.py в образ в каталог /opt/app.py
- ENTRYPOINT - процесс запускаемый в контейнере

## Flask
```
FROM ubuntu

RUN apt-get update
RUN apt-get install -y python3 python3-pip

RUN pip3 install flask

COPY app.py /opt/app.py

ENTRYPOINT FLASK_APP=/opt/app.py flask run --host=0.0.0.0 --port=5000
```

## Ubuntu sleeper
```
FROM Ubuntu

CMD sleep 5
```

```
FROM Ubuntu

CMD ["sleep", "5"]
```
- docker build . -t ubuntu-sleeper
- docker run ubuntu-sleeper

```
FROM Ubuntu

ENTRYPOINT ["sleep"]

CMD ["5"]
```
- docker run ubuntu-sleeper
- sleep 5
- docker run ubuntu-sleeper 10
- sleep 10
- docker run --entrypoint super-sleep ubuntu-sleeper 10
- super-sleep 10

# Docker Compose

```
# docker-compose.yml

services:
  web:
    image: "rotorocloud/rotorobot"
  database:
    image: "mongodb"
  messaging:
    image: "redis:alpine"
  orchestration:
    image: "ansible"
```
