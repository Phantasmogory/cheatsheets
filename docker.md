```sh
docker ps -a # список контейнеров, в том числе и отработавших
docker rm $(docker ps -a | grep strava_django_web_1 | awk '{print $1}') # удалить все контейнеры по имени
docker ps -a | grep Exit | cut -d ' ' -f 1 | xargs sudo docker rm #удалить отработавшие контейнеры

#удалить ненужные образы и слои
docker image prune


-p 8080:80 # Map TCP port 80 in the container to port 8080 on the Docker host

# быстропроксик
docker run --name squid -d --restart=always --publish 3128:3128 --volume ./squid.conf:/etc/squid/squid.conf sameersbn/squid:3.5.27-2 
#сначала без волума стянуть конфиг если нужно что то править 
docker cp squid:/etc/squid/squid.conf squid.conf
docker exec -it squid tail -f /var/log/squid/access.log

# быстровпн
docker run --name openvpn --cap-add=NET_ADMIN -it -p 1194:1194/udp -p 8081:8080/tcp -e HOST_ADDR=$(curl -s https://api.ipify.org) alekslitvinenk/openvpn 

#зайти внутрь контейнера
docker exec -it <containerid> /bin/bash #
docker exec -it strava_django_web_1 /bin/bash #
docker exec -it strava_django_web_1 /bin/sh # if alpine
docker exec -it strava_django_web_1 --user root /bin/bash

docker-compose build

# запустить и отключиться
docker-compose up -d 

# подключиться к потоку логов
docker-compose logs --follow 
# другой вариант
tail -100f $(docker inspect --format='{{.LogPath}}' <container_name_or_id>) 

#очистка логов
truncate -s 0 $(docker inspect --format='{{.LogPath}}' <container_name_or_id>)
truncate -s 0 /var/lib/docker/containers/*/*-json.log

docker-compose down

#adhoc команды
docker-compose exec strava_django_web_1 python manage.py makemigrations
docker-compose exec strava_django_web_1 python manage.py migrate
docker-compose exec strava_django_web_1 python manage.py collectstatic
```
