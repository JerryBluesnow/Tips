mkdir immich-app
cd immich-app
wget -O .env https://github.com/immich-app/immich/releases/latest/download/example.env
wget https://github.com/immich-app/immich/releases/latest/download/docker-compose.yml
mkdir /usr/src/app/upload
docker compose up
http:://host-ip:2283

another one is pichome