# YaMDB

![yamdb_workflow](https://github.com/bitcoineazy/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

[![docker](https://img.shields.io/badge/-Docker-464646??style=flat-square&logo=docker)](https://www.docker.com/)
[![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-464646??style=flat-square&logo=PostgreSQL)](https://www.postgresql.org/)
[![NGINX](https://img.shields.io/badge/-NGINX-464646??style=flat-square&logo=NGINX)](https://nginx.org/ru/)
[![Python](https://img.shields.io/badge/-Python-464646??style=flat-square&logo=Python)](https://www.python.org/)
[![Django](https://img.shields.io/badge/-Django-464646??style=flat-square&logo=Django)](https://www.djangoproject.com/)
[![GitHub%20Actions](https://img.shields.io/badge/-GitHub%20Actions-464646??style=flat-square&logo=GitHub%20actions)](https://github.com/features/actions)
[![GitHub](https://img.shields.io/badge/-GitHub-464646??style=flat-square&logo=GitHub)](https://github.com/)


***REST API*** для сбора отзывов пользователей на произведения.<br>
* Ресурс доступен по адресу [62.84.112.39/admin/](http://62.84.112.39/admin/), [62.84.112.39/redoc/](http://62.84.112.39/redoc/)
* Данные для входа в интерфейс администратора ```username:pass = admin:admin```

Обращение к API сервиса:
* http://62.84.112.39/api/v1/auth/token/
* http://62.84.112.39/api/v1/users/
* http://62.84.112.39/api/v1/titles/
* http://62.84.112.39/api/v1/genres/
* http://62.84.112.39/api/v1/categories/
* http://62.84.112.39/api/v1/titles/{title_id}/reviews/
* http://62.84.112.39/api/v1/titles/{title_id}/reviews/{review_id}/
* http://62.84.112.39/api/v1/titles/{title_id}/reviews/{review_id}/comments/


# Установка и запуск сервиса
 
1. Установить: [docker](https://www.docker.com/get-started), [docker-compose](https://docs.docker.com/compose/install/)
2. Собрать базу данных на основе ресурсов: ```sudo docker-compose exec web python manage.py makemigrations && sudo docker-compose exec web python manage.py migrate```
3. Создать профиль администратора: ```sudo docker-compose exec web python manage.py createsuperuser```
4. Собрать статику: ```sudo docker-compose exec web python manage.py collectstatic```
5. Через интерфейс администратора [0.0.0.0/admin](https://0.0.0.0/admin) можно создавать новые записи в бд
6. Собрать проект и запустить: ```docker-compose up --build```

# Документация


- Находится по адресу [0.0.0.0/redoc](https://0.0.0.0/redoc/) или на сервере [62.84.112.39/redoc/](http://62.84.112.39/redoc/)
- Каждый ресурс описан в документации: указаны эндпоинты (адреса, по которым можно сделать запрос), разрешённые типы запросов, права доступа и дополнительные параметры, если это необходимо.

# Ресурсы API

- auth: аутентификация.
- users: пользователи.
- titles: произведения, к которым пишут отзывы (определённый фильм, книга или песенка).
- categories: категории (типы) произведений («Фильмы», «Книги», «Музыка»).
- genres: жанры произведений. Одно произведение может быть привязано к нескольким жанрам.
- reviews: отзывы на произведения. Отзыв привязан к определённому произведению.
- comments: комментарии к отзывам. Комментарий привязан к определённому отзыву.

Отзывы:
+ получить список всех отзывов;
+ создать новый отзыв;
+ получить отзыв по id;
+ частично обновить отзыв по id;
+ удалить отзыв по id.

Комментарии к отзывам:

+ Получить список всех комментариев к отзыву по id;
+ создать новый комментарий для отзыва, получить комментарий для отзыва по id;
+ частично обновить комментарий к отзыву по id;
+ удалить комментарий к отзыву по id.

JWT-токен:

+ Отправление confirmation_code на переданный email;
+ получение JWT-токена в обмен на email и confirmation_code.

Пользователи:

+ получить список всех пользователей;
+ создание пользователя получить пользователя по username;
+ изменить данные пользователя по username;
+ удалить пользователя по username;
+ получить данные своей учетной записи;
+ изменить данные своей учетной записи.

Категории (типы) произведений:

+ получить список всех категорий;
+ создать категорию;
+ удалить категорию.

Категории жанров:

+ получить список всех жанров
+ создать жанр;
+ удалить жанр.

Произведения, к которым пишут отзывы:

+ получить список всех объектов;
+ создать произведение для отзывов;
+ информация об объекте;
+ обновить информацию об объекте;
+ удалить произведение.

Пользовательские роли
- Аноним — может просматривать описания произведений, читать отзывы и комментарии.
Аутентифицированный пользователь (user) — может читать всё, как и Аноним, может публиковать отзывы и ставить оценки произведениям (фильмам/книгам/песенкам), может комментировать отзывы; может редактировать и удалять свои отзывы и комментарии, редактировать свои оценки произведений. Эта роль присваивается по умолчанию каждому новому пользователю.
- Модератор (moderator) — те же права, что и у Аутентифицированного пользователя, плюс право удалять и редактировать любые отзывы и комментарии.
- Администратор (admin) — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
- Суперюзер Django должен всегда обладать правами администратора, пользователя с правами admin. Даже если изменить пользовательскую роль суперюзера — это не лишит его прав администратора. Суперюзер — всегда администратор, но администратор — не обязательно суперюзер.

Самостоятельная регистрация новых пользователей
1. Пользователь отправляет POST-запрос с параметрами email и username на эндпоинт /api/v1/auth/signup/.
2. Сервис YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на указанный адрес email.
Пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен).
- Пользователя может создать администратор — через админ-зону сайта или через POST-запрос на специальный эндпоинт api/v1/users/ (описание полей запроса для этого случая — в документации). В этот момент письмо с кодом подтверждения пользователю отправлять не нужно.
После этого пользователь должен самостоятельно отправить свой email и username на эндпоинт /api/v1/auth/signup/ , в ответ ему должно прийти письмо с кодом подтверждения.
Далее пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен), как и при самостоятельной регистрации.

- После регистрации и получения токена пользователь может отправить PATCH-запрос на эндпоинт /api/v1/users/me/ и заполнить поля в своём профайле (описание полей — в документации).

Создание пользователя администратором

- Пользователя может создать администратор — через админ-зону сайта или через POST-запрос на специальный эндпоинт api/v1/users/ (описание полей запроса для этого случая — в документации). В этот момент письмо с кодом подтверждения пользователю отправлять не нужно.
После этого пользователь должен самостоятельно отправить свой email и username на эндпоинт /api/v1/auth/signup/ , в ответ ему должно прийти письмо с кодом подтверждения.
Далее пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен), как и при самостоятельной регистрации.


