# [Blade шаблоны](https://laravel.com/docs/9.x/blade#main-content)

Blade является мощнейшим инструментом, для фронтэнд части клиент-серверного приложения. Он позволяет пользоваться и изменять динамический контент непосредственно в HTML разметке. Все файлы отображений обязательно должны быть с расширением ```.blade.php```.

Внутри файлов шаблонов можно получить доступ к любому динамическому полю в ```Request```, просто обратясь к нему в двойных фигурных скобках: ```{{ $variable }}```

## Шаблоны шаблонов

При обычном проектировании веб-ресурсов, зачастую приходится повторять один и тот же код, в нескольких местах, или файлах. Например навигационную панель, футеры, боковые панели и т.д.

Blade предлагает универсальное решение. Создаём blade файл в папке ```layouts```.

В файле [app.blade.php](https://github.com/DavidaaWoW/GlobusDelievery/blob/master/resources/views/layouts/app.blade.php) объявляем стандартную HTML разметку, подключаем все завиимости, а внутри главного элемента контейнера прописываем директиву ```@section('navbar')```, которая означает, что далее будет происходить вставка контента, который при необходимости можно будет дополнить при наследовании. Данная секция отвечает за наличие навигационной панели.

Интернет ресурс был написан с помощью библиотеки [Bootstrap](https://getbootstrap.com/docs/5.2/getting-started/introduction/).

В navbar-е мы как раз впервые и воспользуемся роутингами, в виде вызова прослушек, через их относительный путь, не забывая указать основную группу. Роутинги, также вызываются в двойных фигурных скобках: ```{{ route('user.cart') }}```

Следом объявляем две директивы ```@yield()```, которые позволяют вставлять контент в определённое место при наследовании шаблонов. Так, были подготовлены yield-ы для основного контента, а также для компонента поиска.

Шаблоны шаблонов готовы! Теперь, мы можем просто прописывать весь необходимый контент сразу же в файлах, просто вставляя шаблон приложения, получая на выходе скелет разметки и саму навигационную панель.

## Страницы аутентификации

В страницах аутентификации не используются шаблоны шаблонов, так как у них уникальная структура, без навигационной панели.

Интересным же в них является взаимодействие с формами и автоматическая обработка ошибок.

- Формы

В самом теге формы нет ничего необычного, только в действии вызывается не ссылка, или JS функция, а роутинг ```action="{{ route('user.registration') }}"```.

Внутри формы, обязательно прописывается директива [CSRF](https://laravel.com/docs/9.x/csrf#main-content), она отвечает, за валидацию запросов, по сути, создавая внутри формы дополнительный ```input``` с типом ```hidden```, в который помещается уникальный токен, далее считывающийся на стороне сервера. Если эту директиву не поставить, то при попытке передачи данных, laravel выдаст ошибку 419.

- Обработка ошибок

В Blade существует директива ```@error```, которая позволяет отображать ошибки, если страница была вызвана из контроллера с методом ```withErrors```, внутри директивы прописывается ошибка, доступ к содержанию которой появляется по полю ```{{ $message }}```. Директиву необходимо закрыть: ```@enderror```

## [Рестораны](https://github.com/DavidaaWoW/GlobusDelievery/blob/master/resources/views/user/restaurants.blade.php)

Список ресторанов - первая страница с использованием шаблонных скелетов. Как можно заметить, в файле отсутствует каркас стандартной ```HTML``` разметки, он заменяется на простое наследование от шаблонной ```layouts.app```, с помощью ключевой аннотации ```extends```.

Далее идёт встраиваение в секции, описанные в шаблоне приложения. Причём контент встраивания может быть передан как вторым параметров в вызове аннотации, так и развёрнуто - до соответствующей аннотации закрытия. В секцию ```docname``` передаётся название страницы, так как название вмещается в одну строку, пользуемся свёрнутым методом передачи.

Следом передаётся отображение списка ресторанов. Список описан в отдельном шаблоне. Для встраивания одних шаблонов в другие используется аннотация ```@include```. 

### [Список ресторанов](https://github.com/DavidaaWoW/GlobusDelievery/blob/master/resources/views/layouts/rest_list.blade.php)

В данном шаблоне происходит последовательный вывод всех ресторанов, согласно критериям поиска и сортировки. При этом, на фронт уже приходят отсортированные и отфильтрованные данные. Список ресторанов приходит в коллекции ```$restaurants```. Внутри blade шаблонов возможна итерация по коллекциям с помощью циклов. В данном случае пользуемся циклом ```@foreach```, обявляемым также с помощью аннотации.

Далее, внутри цикла последовательно обращаемся к нужным полям объекта, выводя соответствующую информацию:
```
                <img src="{{ $restaurant->image }}" class="card-img-top mx-auto" style="width:300px;" alt="">
```

В ```Blade``` также возможно использование ветвления, реализуемое с помощью аннотации ```@if...@else```. В данном шаблоне проверяется, если в результате поиска пришла пустая коллекция, то выводится соответствующее сообщение.

Все вышеперечисленные аннотации необходимо закрывать: ```@endforeach ... @endif```

***

После встраивания шаблона со списком ресторанов идёт проверка на необходимость отображения значка корзины. Так, если стоимость текущего заказа равна нулю, это значит, что корзина пуста, а соответственно, данный значок нет нужды отображать.

### Секция поиска

Данная секция отвечает по сути за встраивание на страницу строки поиска и сортировки. Состоит по сути из единственной формы, в которой отражён ```input``` для поиска и два ```dropdown-а``` для фильтрации.

Как поиск так и сортировка выполняется с помощью технологии ```AJAX```. Данная технология позволяет **НЕ** перезагружать страницу после поиска и фильтрации. При этом, саму форму запрещено отправлять с помощью ```ENTER``` внутри ```input-а```, в то же время, поиск производится автоматически при **ЛЮБОМ** изменении input-а, внутри ```event-listener-а```, вызывается функция ```makeRequest```. 

В функции ```makeRequest()``` описывается ajax запрос, где на контроллер передаётся обязательный ```csrf токен```, значение ```input-а```, а также фильтры. При успешном запросе, происходит перерисовка всего списка ресторанов.

При изменении вида, или типа фильтрации вызывается соответствующая функция ```actionSortType```, или ```actionSort```, получающие на вход результат выбора, которым заменяется значение скрытых ```input-ов``` в форме, после чего происходит вызов основной функции ```makeRequest```.

***

## [Категории и блюда](https://github.com/DavidaaWoW/GlobusDelievery/blob/master/resources/views/user/rest_content.blade.php)

Данный шаблон получает на вход коллекцию категорий, а также двухмерный массив блюд. Изначально происходит итерация по категориям с помощью цикла ```@for```, от самой сущености категорий отображается по сути только название. А далее, по индексу уже происходит итерация по коллекции блюд в пределах категории. Помимо обычных отображений, происходит вставка php кода, с помощью аннотации ```@php```. Внутри проверяется состояние текущего блюда в пределах заказа - в зависимости от того, находится ли блюдо в корзине, или нет, флагу присваивается определённое значение.

С помощью аннотаций ветвления происходит отображение либо значка добавления в корзину, либо счётчика количества блюда.

Взаимодействие с корзиной также происходит с помощью AJAX-а. 

+ Функция ```getOrder``` отвечает за получение обновлённого объекта текущего заказа, она вызывается при каждом обновлении корзины. Возвращает по сути ```promise```.
+ Функции ```addToCart``` и ```makeRequest``` являются парными, и отвечают за добавление блюда в корзину. При этом, происходит обновление значка на счётчик, а также изменение суммы заказа в корзине. При этом происходит обработка 401 статуса, который выбрасывается сервером в том случае, если пользователь неавторизован.
+ Функции ```increase``` и ```decrease``` отвечают за увеличение и уменьшение количества товара в корзине, при отсутствии ошибок, вызывают промис ```getOrder```, и на основе его обновляют информацию. Функция ```decrease``` также может заменять счётчик на значок корзины.

## [Корзина](https://github.com/DavidaaWoW/GlobusDelievery/blob/master/resources/views/user/basket.blade.php)

Корзина отображает информацию лишь в том случае, если пользователь авторизован и в корзину добавлено хоть одно блюдо, иначе выводит информацию о том, что в корзине ничего нет.

На вход корзина получает коллекцию элементов заказа - ```order_items```. Внутри изначально вычисляется итоговая сумма заказа, после чего происходит итерация по элементам заказа с последовательным их отображением.

В целом, корзина работает по тому же принципу, что и взаимодействие с товарами на странице контента ресторанов. Из новых AJAX функций появляется лишь ```updateInfo``` и ```updateTotalPrice```, обновляющие итоговую сумму заказа и стоимость всех блюд в элементе заказа.

## [Профиль](https://github.com/DavidaaWoW/GlobusDelievery/blob/master/resources/views/user/profile.blade.php)

Профиль является по сути своей архивом заказов. В нём происходит итерация по заказам пользователя, отсортированным по дате обновления, в порядке убывания.

О том, откуда в шаблоне появляется доступ к объекту пользователя, описано в [провайдерах](https://github.com/DavidaaWoW/GlobusDelievery/tree/master/app/Providers).

## [Информация о заказе](https://github.com/DavidaaWoW/GlobusDelievery/blob/master/resources/views/user/order_info.blade.php)

Страница информации о заказе, по сути своей является той же самой страницей корзины, лишь с обрезанной возможностью редактировать заказ.
