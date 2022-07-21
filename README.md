![workflow](https://github.com/DD477/yamdb_final/actions/workflows/yamdb_workflow.yaml/badge.svg)


<div align="center">
  
  ## Проект YaMDb
   ### Сервис сбора отзывов о произведениях: книгах, фильмах, музыке.
    
  Авторы:
  <p>
    
  [Vladislav Nosikov](https://github.com/Creepy-Panda)
    · 
  [Dmitry Dobrodeev](https://github.com/DD477)
    · 
  [Ivan Skvortsov](https://github.com/Ivan-Skvortsov)
  </p>
  
</div>


## Оглавление

* [О проекте](#о-проекте)
* [Использованные технологии](#использованные-технологии)
* [Необходимый софт](#необходимый-софт)
* [Как запустить проект](#как-запустить-проект)
* [Запуск docker-контейнера](#запуск-docker-контейнера)
* [Тестовые базы данных](#тестовые-базы-данных)
* [Пользовательские роли](#пользовательские-роли)
* [Регистрация пользователей](#регистрация-пользователей)
* [Создание пользователя администратором](#создание-пользователя-администратором)

## О проекте
Проект YaMDb собирает отзывы пользователей на произведения. Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список категорий может быть расширен администратором (например, можно добавить категорию «Изобразительное искусство» или «Ювелирка»).
Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.
В каждой категории есть произведения: книги, фильмы или музыка. Например, в категории «Книги» могут быть произведения «Винни-Пух и все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» группы «Насекомые» и вторая сюита Баха.
Произведению может быть присвоен жанр из списка предустановленных. Новые жанры может создавать только администратор.
Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — рейтинг (целое число). На одно произведение пользователь может оставить только один отзыв.

[К оглавлению](#оглавление) ↑

## Использованные технологии
[![Python](https://img.shields.io/badge/-Python-464646?style=flat-square&logo=Python)](https://www.python.org/)
[![Django](https://img.shields.io/badge/-Django-464646?style=flat-square&logo=Django)](https://www.djangoproject.com/)
[![Django REST Framework](https://img.shields.io/badge/-Django%20REST%20Framework-464646?style=flat-square&logo=Django%20REST%20Framework)](https://www.django-rest-framework.org/)
[![Simple JWT](https://img.shields.io/badge/-Simple_JWT-464646?style=flat-square)](https://django-rest-framework-simplejwt.readthedocs.io/en/latest/)
[![Djoser](https://img.shields.io/badge/-Djoser-464646?style=flat-square)](https://djoser.readthedocs.io/en/latest/getting_started.html)
[![Docker](https://img.shields.io/badge/-Docker-464646?style=flat-square&logo=docker)](https://www.docker.com/)

[К оглавлению](#оглавление) ↑

## Необходимый софт
- [Python](https://www.python.org/) 3.8.10 или выше
- [PostgreSQL](https://www.postgresql.org/) 13
- [Docker](https://www.docker.com/) 4.10.1
- [Docker Compose](https://docs.docker.com/compose/) 3.8

[К оглавлению](#оглавление) ↑

## Как запустить проект:
- Клонировать репозиторий 
   ```sh
   git clone https://github.com/DD477/infra_sp2.git
   ```
- Перейти в папку с проектом
   ```sh
   cd api_yamdb
   ```
- Cоздать и активировать виртуальное окружение
   ```sh
   python3 -m venv venv
   ```
   ```sh
   source venv/bin/activate
   ```
- Обновить менеджер пакетов (pip)
   ```sh
   pip install --upgrade pip
   ```
- Установить зависимости из файла requirements.txt
   ```sh
   pip install -r requirements.txt
   ```
   
### Запуск docker-контейнера:
- В папке `infra` создайть файл `.env` - в нем будут храниться переменные окружения для проекта. Пример его содержимого ниже:
```sh
SECRET_KEY=SECRET_KEY
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
```
- Запуск docker-compose
```sh
cd infra/
docker-compose up -d
```
- Выполнить миграции
```sh
sudo docker-compose exec web python3 manage.py makemigrations 
sudo docker-compose exec web python3 manage.py migrate 
```
- Создать суперпользователя
```sh
sudo docker-compose exec web python3 manage.py createsuperuser 
```
- Собрать статику
```sh
docker-compose exec web python manage.py collectstatic --no-input 
```
### Тестовые базы данных
В репозитории, в директории /api_yamdb/static/data, подготовлены несколько файлов в формате .csv с контентом для ресурсов Users, Titles, Categories, Genres, Review и Comments. Вы можете заполнить базу данных контентом из приложенных csv-файлов. Для этого необходимо выполнить команду:
   ```sh
   docker-compose exec web python3 manage.py import_csv
   ```
- После запуска контейнеров, сайт будет доступен по адресу http://localhost/
- У проекта нет интерфейса, так что можно пользоватья любом удобный инструментом для работы с API
- Спецификация API будет доступна http://localhost/redoc/
   
[К оглавлению](#оглавление) ↑

### Пользовательские роли

- **Аноним** — может просматривать описания произведений, читать отзывы и комментарии.
- **Аутентифицированный пользователь (user)** — может читать всё, как и Аноним, может публиковать отзывы и ставить оценки произведениям (фильмам/книгам/песенкам), может комментировать отзывы; может редактировать и удалять свои отзывы и комментарии, редактировать свои оценки произведений. Эта роль присваивается по умолчанию каждому новому пользователю.
- **Модератор (moderator)** — те же права, что и у Аутентифицированного пользователя, плюс право удалять и редактировать любые отзывы и комментарии.
- **Администратор (admin)** — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
- **Суперюзер Django** обладает правами администратора, пользователя с правами admin. Даже если изменить пользовательскую роль суперюзера — это не лишит его прав администратора.

[К оглавлению](#оглавление) ↑

### Регистрация пользователей

- Пользователь отправляет POST-запрос с параметрами email и username на эндпоинт /api/v1/auth/signup/.
- Сервис YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на указанный адрес email.
- Пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен).
В результате пользователь получает токен и может работать с API проекта, отправляя этот токен с каждым запросом.

После регистрации и получения токена пользователь может отправить PATCH-запрос на эндпоинт /api/v1/users/me/ и заполнить поля в своём профайле (описание полей — в документации redoc).

[К оглавлению](#оглавление) ↑

### Создание пользователя администратором

Пользователя может создать администратор — через админ-зону сайта или через POST-запрос на специальный эндпоинт api/v1/users/ (описание полей запроса для этого случая — в документации). В этом случае письмо пользователю не отправляется.

После этого пользователь должен самостоятельно отправить свой email и username на эндпоинт /api/v1/auth/signup/, в ответ ему придет письмо с кодом подтверждения.

Далее пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен), как и при самостоятельной регистрации.

[К оглавлению](#оглавление) ↑

### Лицензия:
- MIT
