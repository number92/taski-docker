### Taski ###
Простой планнер, обладающий функциями создания/удаления задач.
Установка
Клонируйте репозиторий на свой компьютер:

git clone https://github.com/number92/taski-docker.git
mkdir taski
cd taski
Создайте файл .env и заполните его своими данными. Перечень данных указан в корневой директории проекта в файле .env.example.
Установка docker compose на сервер:
sudo apt update
sudo apt install curl
curl -fSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo apt-get install docker-compose-plugin
В директорию taski/ скопируйте файлы docker-compose.production.yml и .env

Последовательно выполните все команды:
sudo docker compose -f docker-compose.production.yml pull
sudo docker compose -f docker-compose.production.yml down
sudo docker compose -f docker-compose.production.yml up -d
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collect_static/. /static_backend/static/

На сервере в редакторе nano откройте конфиг Nginx:

sudo nano /etc/nginx/sites-enabled/default
Измените настройки location в секции server:

location / {
    proxy_set_header Host $http_host;
    proxy_pass http://127.0.0.1:9000;
}
Проверьте работоспособность конфига Nginx:

sudo nginx -t
Если ответ в терминале такой, значит, ошибок нет:

nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
Перезапускаем Nginx

sudo service nginx reload
