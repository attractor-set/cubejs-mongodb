Система запускается как образы docker и является клеем между cube.js и СУБД mongodb.

Для корректного запуска необходимо создать SSL ключи для подклчения к mongodb:

openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out certificate.pem 

cat key.pem certificate.pem > mongo.pem

После этого можно приступить к созданию образов docker и их запуску:

sudo docker-compose build

docker-compose up -d

Можно добавить в базу mongodb тестовые данные. 
Для этого нужно раскомментировать 14 строчку в файле docker-compose.yaml "./dump:/dump" и выполнить следующие команды:

docker-compose down

curl https://cube.dev/downloads/events-dump.zip > events-dump.zip

unzip events-dump.zip

docker exec mongo mongorestore dump/stats/events.bson -u root -p admin123

docker-compose up -d

Изменения имени базы данных и прочих параметров возможно посредством редактирования файлов mongosqld.conf и/или .env 
В этом случае, при уже запущеной системе, необходимо перезапустить сервисы mongobi и cubejs командами:

docker restart mongo-bi

docker restart cubejs
