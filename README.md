# yamdb_final
![Django workflow](https://github.com/krapiwin/yamdb_final/actions/workflows/main.yml/badge.svg)
# YaMDb api

Протестировать работу сервиса можете по [ссылке](http://yamdtest.ddns.net/api/v1/)

## Самая актуальная версия API всемирно известного YaMDb.

YaTube API это RESTful API, позволяющий создавать и редактировать отзывы на различные произведения, оценивать их и
оставлять комментарии к отзывам. В основе проекта лежат Django и Django REST Framework.

## Особенности

- Пишите свои ревью и ставьте оценки произведениям.
- Авторизация происходит по токену. 
- Токен можно получить введя код подтверждения из электронного письма.
- При просмотре информации о каком-либо произведении будет выведена его средняя оценка.
- Для авторизованных пользователей присутствует возможность комментировать отзывы.

## Технологии

- [Django]
- [Django REST Framework]

## Начало работы
```
git clone
cd yamdb_final/infra
```
```
docker compose up -d
docker compose exec web python manage.py migrate
docker compose exec web python manage.py collectstatic
docker compose exec web python manage.py loaddata fixtures.json
```
ИЛИ одной строчкой!
```
docker compose up -d && sleep 2 && docker compose exec web python manage.py migrate && docker compose exec web python manage.py collectstatic && docker compose exec web python manage.py loaddata fixtures.json
```

### Как запустить проект:

```
http://127.0.0.1/api/v1/
```

## Пользовательские роли
```Аноним — может просматривать описания произведений, читать отзывы и комментарии.```

```Аутентифицированный пользователь (user) — может, как и Аноним, читать всё, дополнительно он может публиковать отзывы и ставить оценку произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы;может редактировать и удалять свои отзывы и комментарии. Эта роль присваивается по умолчанию каждому новому пользователю.```

```Модератор (moderator) — те же права, что и у Аутентифицированного пользователя плюс право удалять любые отзывы и комментарии.```

```Администратор (admin) — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.```

```Суперюзер Django — обладет правами администратора (admin)```

## Примеры работы с API:

Получение списка всех категорий и создание новой(доступно только для роли 'Administrator') происходит на эндпоинте:

```sh
http://127.0.0.1/api/v1/categories/
```

Пример ответа при GET запросе с параметрами offset и count:
```
[
  {
    "count": 0,
    "next": "string",
    "previous": "string",
    "results": [
      {
        "name": "string",
        "slug": "string"
      }
    ]
  }
]

```

Редактирование категорий запрещено. Удаление категории происходит на эндпоинте:

```sh
http://127.0.0.1/api/v1/categories/{slug}/
```


Получение списка жанров и создание нового(доступно только для роли 'Administrator') происходит на эндпоинте:

```sh
http://127.0.0.1/api/v1/genres/
```
Пример ответа:
```
[
  {
    "count": 0,
    "next": "string",
    "previous": "string",
    "results": [
      {
        "name": "string",
        "slug": "string"
      }
    ]
  }
]
```

Получение конкретного жанра, а так же его редактирование запрещено. А удаление происходит на эндпоинте:

```sh
http://127.0.0.1/api/v1/genres/{slug}/
```

Получение списка произведений и создание нового происходит на эндпоинте:

```sh
http://127.0.0.1/api/v1/titles/
```

Пример ответа:
```
[
  {
    "id": 0,
    "title": "string",
    "slug": "string",
    "description": "string"
  }
]
```

Для конкретной группы доступен только метод GET(id - первичный ключ таблицы Groups) на эндпоинте:

```sh
http://127.0.0.1/api/v1/groups/{id}/
```

Пример ответа:
```
{
  "id": 0,
  "title": "string",
  "slug": "string",
  "description": "string"
}
```

Следующий эндпоинт возвращает все подписки пользователя, сделавшего запрос. Анонимные запросы запрещены (метод GET):

```sh
http://127.0.0.1/api/v1/follow/
```

Пример ответа:
```
[
  {
    "user": "string",
    "following": "string"
  }
]
```

Подписка пользователя от имени которого сделан запрос на пользователя переданного в теле запроса. Анонимные запросы
запрещены (метод POST):

```sh
http://127.0.0.1/api/v1/follow/
```

Создание, обновление и верификация токена происходит на следущих эндпоинтах:

```sh
http://127.0.0.1/api/v1/titles/{title_id}/reviews/{review_id}/comments/
```

Пример ответа:
```
[
  {
    "count": 0,
    "next": "string",
    "previous": "string",
    "results": [
      {
        "id": 0,
        "text": "string",
        "author": "string",
        "pub_date": "2019-08-24T14:15:22Z"
      }
    ]
  }
]
```

Получение конкретного комментария, а так же его редактирование и удаление
(доступно только для автора комментария, модератора или администратора) происходит на эндпоинте:

```sh
http://127.0.0.1/api/v1/titles/{title_id}/reviews/{review_id}/comments/{comment_id}/
```
Пример ответа: 
```
{
  "id": 0,
  "text": "string",
  "author": "string",
  "pub_date": "2019-08-24T14:15:22Z"
}
```

Регистрация пользователя и получение jwt токена происходит на следующих эндпоинтах:

```sh
http://127.0.0.1/api/v1/auth/signup/
http://127.0.0.1/api/v1/auth/token/
```

About Author:

Daniil Krapivin is upcoming python god. Ex-professional sailor and amateur cyclist, he found his passion in writing best code possible. His stack includes: Django, DRF, Telegram Bot, Docker and more.


MIT License

Copyright (c) 2022 Daniil Krapivin

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
