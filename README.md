Система запускается как *образы docker* и является клеем между *cube.js* и СУБД *mongodb*.

Для корректного запуска *необходимо создать SSL* ключи для подклчения к mongodb:

	openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out certificate.pem 

	cat key.pem certificate.pem > mongo.pem

После этого можно приступить к созданию образов docker и их запуску:

	sudo docker-compose build

	sudo docker-compose up -d

Можно добавить в базу mongodb *тестовые данные*. 
Для этого нужно остановить сервисы:
	
	sudo docker-compose down

Раскомментировать *14 строчку в файле docker-compose.yaml* "./dump:/dump" и выполнить следующие команды:

	sudo docker-compose up -d	

	curl https://cube.dev/downloads/events-dump.zip > events-dump.zip

	unzip events-dump.zip

	sudo docker exec mongo mongorestore dump/stats/events.bson -u root -p admin123

	sudo docker-compose down	

После чего необходимо снова *закомментироват указанную строку* и запустить систему:

	sudo docker-compose up -d

Изменения *имени базы данных* и *прочих параметров* возможно посредством редактирования файлов *mongosqld.conf* и/или *.env* 
В этом случае, *при уже запущеной системе*, необходимо *перезапустить* сервисы *mongobi* и *cubejs* командами:

	sudo docker restart mongo-bi

	sudo docker restart cubejs

Веб интерфейс системы по умолчанию расположен на [локальном хосте, порт 4000](http://localhost:4000/)
