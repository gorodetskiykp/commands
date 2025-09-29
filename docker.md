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
- ps aux - список запущенных процессов

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
- docker search --filter=stars=15 --limit 3 nginx - поиск образа
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
- docker -H=remote-docker-engine:2375 - переключение на удаленный docker-хост
- docker -H=10.10.10.1:2375 run nginx - запуск контейнера на удаленном docker-хосте
- docker run --cpus=.5 nginx - ограничение на использование не более 50% CPU хоста
- docker run --memory=100m nginx - ограничение на RAM=100Mb
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
- docker volume create data\_volume - создание тома для постоянного хранения данных контейнера
- docker run -v data\_volume:/var/lib/mysql mysql - монтирование тома (если тома нет - он будет создан)
- docker run -v /data/mysql:/var/lib/mysql mysql - монтирование произвольного каталога
- docker run --mount type=bind,source=/data/mysql,target=/var/lib/mysql mysql - новый стиль монтирования
- docker run -d --name webserver --volume secure-vol:/opt:ro nginx - монтирование в режиме Только для чтения
- docker run -d --name webserver --mount source=secure-vol,target=/opt,readonly nginx
- docker run -d --name webserver --mount source=secure-vol,target=/opt,ro nginx
- docker volume rm some-vol - удалить том some-vol
- docker volume prune - удалить неиспользуемые тома

## Network

- docker run webapp - запуск во внутренней сети (BRIDGE)
- docker run webapp --network=none
- docker run webapp --network=host - запуск в сети хоста
- docker network create --driver bridge --subnet 182.18.0.0/16 my-custom-network
- docker network ls - список доступных сетей
- docker network connect NewtworkName ContainerNmae - подключить контейнер к сети
- docker network disconnect NewtworkName ContainerNmae - отключить контейнер от сети
- docker network prune - удалить все неиспользуемые сети
- docker network rm NetworkName - удалить сеть

## Оркестрация

- docker service create --replicas=100 node

### Docker Swarm

- docker swarm init
- docker swarm join --token <token>

### Kubernetes

- kubectl create deployment node --image=node:v1
- kubectl scale --replicas=9 node
- kubectl set image deployment node --image=node:v2
- kubectl rollout undo deployment node
- kubectl run hello-minikube - запуск контейнера в кластере
- kubectl cluster-info
- kubectl get nodes

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
```
docker run -d --name=redis redis
docker run -d --name=db postgres:9.4
docker run -d --name=vote -p 5000:80 --link redis:redis voting-app
docker run -d --name=result -p 5001:80 --link db:db result-app
docker run -d --name=worker --link db:db --link redis:redis worker
```
```
redis:
  image: redis

db:
  image: postgres:9.4

vote:
  image: voting-app
  ports:
  -  5000:80
  links:
  -  redis

result:
  image: result-app
  ports:
  -  5001:80
  links:
  -  db

worker:
  image: worker
  links:
  -  db
  -  redis
```
- db:db = db
- redis:redis = redis
```
docker-compose up
```
```
# если часть образов есть только в локальных Dockerfile
# отсутствуют в Docker Hub

redis:
  image: redis

db:
  image: postgres:9.4

vote:
  build ./vote
  ports:
  -  5000:80
  links:
  -  redis

result:
  build: ./result
  ports:
  -  5001:80
  links:
  -  db

worker:
  build ./worker 
  links:
  -  db
  -  redis
```
## Версии docker-compose
```
# ver.1

redis:
  image: redis
db: 
  image: postgres:9.4
vote:
  image: voting-app
  ports:
  -  5000:80
  links:
  -  redis
```
```
# ver.2

version: 2
services:
  redis:
    image: redis
  db: 
    image: postgres:9.4
  vote:
    image: voting-app
    ports:
    -  5000:80
    depends_on:
    -  redis
```
```
# ver.3

version: 3
services:
  redis:
    image: redis
  db: 
    image: postgres:9.4
  vote:
    image: voting-app
    ports:
    -  5000:80
```
## Networks
```
version: 2
services:
  redis:
    image: redis
    networks:
    -  backend
  db: 
    image: postgres:9.4
    networks:
    -  backend
  vote:
    image: voting-app
    ports:
    -  5000:80
    depends_on:
    -  redis
    networks:
    -  frontend
    -  backend
  result:
    image: result-app
    networks:
    -  frontend
    -  backend
networks:
  frontend:
  backend:
```
## Основные команды
- docker-compose up -d
- docker-compose down
- docker-compose ps
- docker-compose logs
- docker-compose up -d --scale vote=3



