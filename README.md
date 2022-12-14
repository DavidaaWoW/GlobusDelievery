
## О проекте

Данный проект является реализованным шаблоном сервиса по доставке еды.

## Описание

Веб-приложение отражает в себе сервис по доставке еды. В качестве основы была взята часть реальной базы данных сервиса [DeliveryClub](https://www.delivery-club.ru/moscow), пользуясь парсером HTML страниц, написанным на языке python. В качестве базы данных был использован MySQL, внутри базы данных были написаны триггерные функции, помогающие с импортом данных, а также работой самого приложения.

У неаутентифицированного пользователя есть возможность просматривать список ресторанов и их меню. Для возможности добавлять блюда в корзину, доступа к самой корзине и профилю, пользователю необходимо аутентифицироваться. Во время регистрации, или входа пользователь имеет возможность сохранить данные в браузерной сессии, для последующего автоматического входа. После аутентификации, пользователь имеет возможность добавлять блюда из меню в корзину. Далее, либо на странице ресторана, либо на странице корзины, пользователь имеет возможность изменить количество товара, либо удалить его из корзины. Вся информация о стоимости и количестве текущего блюда, а также общей стоимости заказа динамически обновляется и показывается пользователю. На странице корзины у пользователя есть возможность оформить заказ, после которого корзина обнуляется, а статус заказа изменяется на "обработку". На странице профиля у пользователя есть возможнось просмотреть список всех заказов, а также посмотреть о них подробную информацию.

В проекте были использованы основные возможности экосистемы Laravel. В частности:

- [Шаблонизатор Blade](https://laravel.com/docs/9.x/blade)
- [Eloquent ORM](https://laravel.com/docs/9.x/eloquent)
- [Artisan console](https://laravel.com/docs/9.x/artisan#main-content)

Клиентская часть интернет-ресурса была написана преимущественно с использованием технологии [AJAX](https://api.jquery.com/jquery.ajax/)

## Документация

Все основные манипуляции были тщательно задокументированы, их описания можно найти непосредственно в каждом разделе.

### Содержание

+ [Docker](https://github.com/DavidaaWoW/GlobusDelievery/tree/master/docker)
+ [Подготовка БД](https://github.com/DavidaaWoW/GlobusDelievery/tree/master/database/source)
+ [Работа с БД](https://github.com/DavidaaWoW/GlobusDelievery/tree/master/database)
+ [Модели](https://github.com/DavidaaWoW/GlobusDelievery/tree/master/app/Models)
+ [Контроллеры](https://github.com/DavidaaWoW/GlobusDelievery/tree/master/app/Http/Controllers)
+ [Пути(Routing)](https://github.com/DavidaaWoW/GlobusDelievery/tree/master/routes)
+ [View. Шаблоны Blade](https://github.com/DavidaaWoW/GlobusDelievery/tree/master/resources/views)
+ [Middleware](https://github.com/DavidaaWoW/GlobusDelievery/tree/master/app/Http/Middleware)
+ [Providers+Composers](https://github.com/DavidaaWoW/GlobusDelievery/tree/master/app/Providers)

## Авторские права

Данный проект является открытым и свободно распространяемым. Создателем всего кода является Гегия Давит, или же [@DavidaaWoW](https://github.com/DavidaaWoW)
