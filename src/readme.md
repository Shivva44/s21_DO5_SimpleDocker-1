# Simple Docker

Введение в докер. Разработка простого докер-образа для собственного сервера.


## Содержание 

1. [Part 1. Готовый докер](#part-1-готовый-докер)
2. [Part 2. Операции с контейнером](#part-2-операции-с-контейнером)
3. [Part 3. Мини веб-сервер](#part-3-мини-веб-сервер)
4. [Part 4. Свой докер](#part-4-свой-докер)
5. [Part 5. Dockle](#part-5-dockle)
6. [Part 6. Базовый Docker Compose](#part-6-базовый-docker-compose)


## Part 1. Готовый докер

- Возьмем официальный докер-образ с **nginx** и выкачаем его при помощи `docker pull`. 

<img  src="Screens/Part1/1.png"  width="50%" />

- Проверим наличие докер-образа через `docker images`.

<img  src="Screens/Part1/2.png"  width="50%" />

- Запустим докер-образ через `docker run -d [image_id|repository]`.

<img  src="Screens/Part1/3.png"  width="50%" />

- Проверим, что образ запустился через `docker ps`


<img  src="Screens/Part1/4.png"  width="50%" />

- Посмотрим информацию о контейнере через `docker inspect [container_id|container_name]`.

<img  src="Screens/Part1/5.png"  width="50%" />

- По выводу команды определим и поместим в отчёт:
  - Размер контейнера
  <img  src="Screens/Part1/8.png"  width="20%" />
  - Список замапленных портов
  <img  src="Screens/Part1/7.png"  width="20%" />
  - Ip контейнера
  <img  src="Screens/Part1/6.png"  width="20%" />

- Остановим докер образ через `docker stop [container_id|container_name]`.

<img  src="Screens/Part1/9.png"  width="50%" />

- Проверим, что образ остановился через `docker ps`.

<img  src="Screens/Part1/10.png"  width="50%" />

- Запустим докер с портами 80 и 443 в контейнере, замапленными на такие же порты на локальной машине, через команду *run*.

<img  src="Screens/Part1/11.png"  width="50%" />

- Проверим, что в браузере по адресу *localhost:80* доступна стартовая страница **nginx**.

<img  src="Screens/Part1/12.png"  width="50%" />

- Перезапустим докер контейнер через `docker restart [container_id|container_name]`.

<img  src="Screens/Part1/13.png"  width="50%" />

- Проверим через `docker ps` , что контейнер перезапустился.

<img  src="Screens/Part1/14.png"  width="50%" />

## Part 2. Операции с контейнером

- Прочитаем конфигурационный файл *nginx.conf* внутри докер контейнера через команду *exec*.

<img  src="Screens/Part2/1.png"  width="50%" />

- Создаем на локальной машине файл *nginx.conf* используя команду `touch nginx.conf`

- Настраиваем в нем по пути */status* отдачу страницы статуса сервера **nginx**.

<img  src="Screens/Part2/2.png"  width="50%" />

- Скопируем созданный файл *nginx.conf* внутрь докер-образа через команду `docker cp`.

<img  src="Screens/Part2/3.png"  width="50%" />

- Перезапустим **nginx** внутри докер-образа через команду *exec*.

<img  src="Screens/Part2/4.png"  width="50%" />

- Проверим, что по адресу `localhost:80/status` отдается страничка со статусом сервера **nginx**.

<img  src="Screens/Part2/5.png"  width="50%" />

- Экспортируем контейнер в файл *container.tar* через команду *export*.

<img  src="Screens/Part2/6.png"  width="50%" />

- Остановим контейнер.

<img  src="Screens/Part2/7.png"  width="50%" />

- Удалим образ через `docker rmi [image_id|repository]`, не удаляя перед этим контейнеры.

<img  src="Screens/Part2/8.png"  width="50%" />

- Удалим остановленный контейнер.

<img  src="Screens/Part2/9.png"  width="50%" />

- Импортируем контейнер обратно через команду *import*.

<img  src="Screens/Part2/11.png"  width="50%" />

- Запустим импортированный контейнер.

<img  src="Screens/Part2/12.png"  width="50%" />

- Проверь, что по адресу *localhost:80/status* отдается страничка со статусом сервера **nginx**.

<img  src="Screens/Part2/13.png"  width="50%" />


## Part 3. Мини веб-сервер

- Напишем мини-сервер на **C** и **FastCgi**, который будет возвращать простейшую страничку с надписью `Hello World!`.

<img  src="Screens/Part3/1.png"  width="50%" />

- Запустим написанный мини-сервер через *spawn-fcgi* на порту 8080. Для этого:
    - Создадим контейнер с замапленным портом `81`.

    <img  src="Screens/Part3/2.png"  width="50%" />

    - Откроем интерактивный терминал внутри контейнера команджой `docker exec -it part3 /bin/bash`

    <img  src="Screens/Part3/3.png"  width="50%" />

    - Установим необходимые бибилотеки используя команды:
        - `apt-get update`  
        - `apt-get install gcc` 
        - `apt-get install spawn-fcgi` 
        - `apt-get install libfcgi-dev`
    
    - Скопируем наш минисервер в контейнер.

    <img  src="Screens/Part3/4.png"  width="50%" />

    - Скомпилируем мини-сервер.

    <img  src="Screens/Part3/5.png"  width="50%" />

    - Запустим наш мини-сервер.

    <img  src="Screens/Part3/6.png"  width="50%" />

- Напишем свой *nginx.conf*, который будет проксировать все запросы с 81 порта на *127.0.0.1:8080*.

<img  src="Screens/Part3/7.png"  width="50%" />

- Скопируем *nginx.conf* в контейнер.

<img  src="Screens/Part3/8.png"  width="50%" />

- Проверим, что в браузере по *localhost:81* отдается написанная тобой страничка.

<img  src="Screens/Part3/9.png"  width="50%" />


- На локальной машине положим созданный файл `nginx.conf` по пути `./nginx/nginx.conf`.


## Part 4. Свой докер

- Напишем свой докер-образ, который:
    - собирает исходники мини сервера на FastCgi из [Части 3](#part-3-мини-веб-сервер);
    - запускает его на 8080 порту;
    - копирует внутрь образа написанный *./nginx/nginx.conf*;
    - запускает **nginx**.

<img  src="Screens/Part4/1.png"  width="50%" />

- Соберем написанный докер-образ через `docker build` при этом указав имя и тег.

<img  src="Screens/Part4/2.png"  width="50%" />

- Проверим что образ появился 

<img  src="Screens/Part4/8.png"  width="50%" />

- Запустим собранный докер-образ с маппингом 81 порта на 80 на локальной машине и маппингом папки *./nginx* внутрь контейнера по адресу, где лежат конфигурационные файлы **nginx**'а

<img  src="Screens/Part4/3.png"  width="50%" />

- Проверим, что по localhost:80 доступна страничка написанного мини сервера.

<img  src="Screens/Part4/4.png"  width="50%" />

- Допишем в *./nginx/nginx.conf* проксирование странички */status*, по которой надо отдавать статус сервера **nginx**.

<img  src="Screens/Part4/5.png"  width="50%" />

- Перезапустим докер-образ.

<img  src="Screens/Part4/6.png"  width="50%" />

- Проверим, что теперь по *localhost:80/status* отдается страничка со статусом **nginx**

<img  src="Screens/Part4/7.png"  width="50%" />


## Part 5. Dockle

- Просканируем образ из предыдущего задания через `dockle [image_id|repository]`.

<img  src="Screens/Part5/1.png"  width="50%" />

- Исправим образ так, чтобы при проверке через **dockle** не было ошибок и предупреждений.

<img  src="Screens/Part5/2.png"  width="50%" />

- Сканируем образ снова.

<img  src="Screens/Part5/3.png"  width="50%" />

## Part 6. Базовый Docker Compose

- Напишем файл *docker-compose.yml*, с помощью которого:
    - Подними докер-контейнер из [Части 5](#part-5-инструмент-dockle) _(он должен работать в локальной сети, т.е. не нужно использовать инструкцию **EXPOSE** и мапить порты на локальную машину)_.
    - Подними докер-контейнер с **nginx**, который будет проксировать все запросы с 8080 порта на 81 порт первого контейнера.
    Замапь 8080 порт второго контейнера на 80 порт локальной машины.

<img  src="Screens/Part6/1.png"  width="50%" />

- Соберем и запустим проект с помощью команд `docker-compose build` и `docker-compose up`.

<img  src="Screens/Part6/2.png"  width="50%" />
<img  src="Screens/Part6/3.png"  width="50%" />

- Проверим, что в браузере по *localhost:80* отдается написанная страничка, как и ранее.

<img  src="Screens/Part6/4.png"  width="50%" />

[**К содержанию**](#Содержание) 













