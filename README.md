### Документация
##### Установка
 1) mongoDB `brew update; brew install mongodb; mkdir -p /data/db`
 
 2) установка зависимостей `npm install`
#### Запуск
###### запуск монги и сервера должен осуществляться в разных окнах терминала
###### запуск через npm
 1) server - `npm run start` доступен по `localhost:3001`
 
 2) mongo - `mongo --port 27017`
 
###### запуск через docker
 1) `docker-machine create --driver virtualbox dev`
 
 2) `docker-machine ip` - ip адресс виртуальной машины
 
 3) `docker-compose build`
 
 4) `docker-compose up`
 
### Компоненты
Сервис написан на node.js с использованием фреймворка express
#### Список компонентов
1) `./api/server/index.js` - инициализация приложения и роутинг.

2) `./api/models` - методы для взаимодействия с базой данных, ничего кроме запросов к бд, обработчик данных передается коллбэком.

3) `./api/controllers` - тут обрабатываем запросы, возвращаем статус и response, передаем нужные параметры запроса к бд, получаем данные из модели и обрабатываем.

4) `./api/bin` - точки входа для dev и production версии.

5) `./api/utl.js` - вспомогательные функции.

6) `./api/db.js` - обертка для коннекта с бд.

### Методы API
1) `GET /coordinates` - получаем список устройтв, с массивами из 1000 последних координат, доступны фильтры по дате `lt - меньше, gt - больше`, `page, limit (максимальное значение == 100)` - пагинация, в параметр запроса можно добавить `encode`, чтобы получить ответ сжатый в gzip.<br>

2) `GET /coordinates/:id` -  получаем координаты по id девайса, доступны фильтры по дате `lt - меньше, gt - больше` `page` - номер пакета (без фильтра будет возвращаться последний), параметр `encode` крайне желателен, даже обязателен, так как в ответе 100 000 объктов<br>

3) `POST /coordinates` - отправка пакета с координатами сжатого в gzip, обязательные заголовки  device-id=id устройства`.<br>

4) `GET /activity` - получаем список всех устройств, которые содержат id устройства и массив c  значениями активности за конкретное время, доступны фильтры по дате `lt - меньше, gt - больше`, `page, limit (максимальное значение == 100)` - пагинация.<br>

5) `GET /activity/:id` -  получаем массив активностей по id, доступны фильтры по дате `lt - меньше, gt - больше`, `page, limit` - пагинация,.<br>

6) `POST /activity` - Отправка значения активности, пример `{time: int, value: int}`, обязательные заголовки `device-id=id устройства`.<br>

7) `GET /clear/:collection` - очистка коллекции (для разработки), потом уберу.



