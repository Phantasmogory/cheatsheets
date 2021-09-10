```sh
docker ps -a # список контейнеров, в том числе и отработавших
docker rm $(docker ps -a | grep strava_django_web_1 | awk '{print $1}') # удалить все контейнеры по имени

docker run --name squid -d --restart=always --publish 3128:3128 --volume ./squid.conf:/etc/squid/squid.conf sameersbn/squid:3.5.27-2 # быстропроксик
#сначала без волума стянуть конфиг если нужно что то править 
docker cp squid:/etc/squid/squid.conf squid.conf

docker exec -it squid tail -f /var/log/squid/access.log


docker run --name openvpn --cap-add=NET_ADMIN -it -p 1194:1194/udp -p 80:8080/tcp -e HOST_ADDR=$(curl -s https://api.ipify.org) alekslitvinenk/openvpn # быстровпн


docker exec -it <containerid> /bin/bash #
docker exec -it strava_django_web_1 /bin/bash #
docker exec -it strava_django_web_1 /bin/sh # if alpine

docker-compose build
docker-compose up -d
docker-compose down

docker-compose logs --follow # подключиться к потоку логов
tail -100f $(docker inspect --format='{{.LogPath}}' <container_name_or_id>) # другой вариант

#очистка логов
truncate -s 0 $(docker inspect --format='{{.LogPath}}' <container_name_or_id>)
truncate -s 0 /var/lib/docker/containers/*/*-json.log



docker-compose exec strava_django_web_1 python manage.py makemigrations
docker-compose exec strava_django_web_1 python manage.py migrate
docker-compose exec strava_django_web_1 python manage.py collectstatic
```
