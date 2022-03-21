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
* [Установка](#установка)
* [Тестовые базы данных](#тестовые-базы-данных)
* [Использование](#использование)
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
* [Django](https://www.djangoproject.com/)
* [Django REST framework](https://www.django-rest-framework.org/)
* [Djoser](https://djoser.readthedocs.io/en/latest/getting_started.html)
* [Simple JWT](https://django-rest-framework-simplejwt.readthedocs.io/en/latest/)
* [Docker](https://www.docker.com/products/docker-desktop)

[К оглавлению](#оглавление) ↑

## Необходимый софт
Для развертывания проекта локально, на Вашем комьютере требуется Python версии 3.8.10 и выше. <br>
Скачать дистрибутив для Вашей ОС можно на официальном сайте: https://www.python.org/downloads/

## Установка
Склонируйте проект на Ваш компьютер
   ```sh
   git clone https://github.com/DD477/infra_sp2.git
   ```
Перейдите в папку с проектом
   ```sh
   cd api_yamdb
   ```
Активируйте виртуальное окружение
   ```sh
   python3 -m venv venv
   ```
   ```sh
   source venv/bin/activate
   ```
Обновите менеджер пакетов (pip)
   ```sh
   pip3 install --upgrade pip
   ```
Установите необходимые зависимости
   ```sh
   pip3 install -r requirements.txt
   ```
   
Запустите docker-compose
  ```sh
  #все команды docker-compose выполнять в папке с docker-compose.yaml
  docker-compose up
  ```
Выполните миграции
   ```sh
   docker-compose exec web python3 manage.py makemigrations
   ```
   ```sh
   docker-compose exec web python3 manage.py migrate
   ```
Создайте пользователя с правами администратора
   ```sh
   docker-compose exec web python3 manage.py createsuperuser
   ```
   
[К оглавлению](#оглавление) ↑

## Тестовые базы данных
В репозитории, в директории /api_yamdb/static/data, подготовлены несколько файлов в формате csv с контентом для ресурсов Users, Titles, Categories, Genres, Review и Comments. Вы можете заполнить базу данных контентом из приложенных csv-файлов. Для этого необходимо выполнить команду:
   ```sh
   docker-compose exec web python3 manage.py import_csv
   ```
   
[К оглавлению](#оглавление) ↑

## Использование
* [Документация API доступна по адресу /redoc](http://127.0.0.1:8000/redoc/)
* [Для удобства работы с приложением, можно воспользоваться панелью администрирования по адресу /admin](http://127.0.0.1:8000/admin/)
* Для работы с API используйте любой удобный для Вас инструмент

[К оглавлению](#оглавление) ↑


## Пользовательские роли

- **Аноним** — может просматривать описания произведений, читать отзывы и комментарии.
- **Аутентифицированный пользователь (user)** — может читать всё, как и Аноним, может публиковать отзывы и ставить оценки произведениям (фильмам/книгам/песенкам), может комментировать отзывы; может редактировать и удалять свои отзывы и комментарии, редактировать свои оценки произведений. Эта роль присваивается по умолчанию каждому новому пользователю.
- **Модератор (moderator)** — те же права, что и у Аутентифицированного пользователя, плюс право удалять и редактировать любые отзывы и комментарии.
- **Администратор (admin)** — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
- **Суперюзер Django** обладает правами администратора, пользователя с правами admin. Даже если изменить пользовательскую роль суперюзера — это не лишит его прав администратора.

[К оглавлению](#оглавление) ↑

## Регистрация пользователей

- Пользователь отправляет POST-запрос с параметрами email и username на эндпоинт /api/v1/auth/signup/.
- Сервис YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на указанный адрес email.
- Пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен).
В результате пользователь получает токен и может работать с API проекта, отправляя этот токен с каждым запросом.

После регистрации и получения токена пользователь может отправить PATCH-запрос на эндпоинт /api/v1/users/me/ и заполнить поля в своём профайле (описание полей — в документации redoc).

[К оглавлению](#оглавление) ↑

## Создание пользователя администратором

Пользователя может создать администратор — через админ-зону сайта или через POST-запрос на специальный эндпоинт api/v1/users/ (описание полей запроса для этого случая — в документации). В этом случае письмо пользователю не отправляется.

После этого пользователь должен самостоятельно отправить свой email и username на эндпоинт /api/v1/auth/signup/, в ответ ему придет письмо с кодом подтверждения.

Далее пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token (JWT-токен), как и при самостоятельной регистрации.

[К оглавлению](#оглавление) ↑
