```sh
docker ps -a # список контейнеров, в том числе и отработавших

docker exec -it <containerid> /bin/bash #
docker exec -it strava_django_web_1 /bin/bash #
docker exec -it strava_django_web_1 /bin/sh # if alpine

docker-compose build
docker-compose up -d
docker-compose down

```
