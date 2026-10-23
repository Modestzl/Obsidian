# Архитектура: первичная инвентаризация

## Версия и происхождение

**Код:** витрина объявляет `VERSION = '1.5.6.4'` в [index.php:40](E:/GPT/amital/amital_ru/index.php:40), административная часть — в [admin/index.php:3](E:/GPT/amital/amital_ru/admin/index.php:3). Это объявленная версия основы OpenCart. Полное совпадение с официальным дистрибутивом не проверено; сравнение с эталоном не выполнялось.

Подтверждены отдельные расширения и проектная логика: vQmod, автоматические SEO-адреса, поиск Sphinx, розничные остатки, предзаказы и дополнительные правила оформления. Конкретную стороннюю сборку вроде ocStore пока нельзя назвать. Маркировка **OCU Memcached** относится к [system/library/cache.php:16](E:/GPT/amital/amital_ru/system/library/cache.php:16), а не доказывает происхождение всего сайта.

**Внешние данные:** версия PHP на сервере неизвестна. Проверка `PHP5.1+` в [system/startup.php:6](E:/GPT/amital/amital_ru/system/startup.php:6) — старый порог начальной загрузки, не установленная версия и не гарантия совместимости всего проекта.

## Из каких частей состоит сайт

1. Веб-сервер принимает адрес. Локальный [`.htaccess`:3](E:/GPT/amital/amital_ru/.htaccess) задаёт перенаправление с `www` и передачу несуществующих файлов/каталогов в `index.php` через `_route_`.
2. [index.php:80](E:/GPT/amital/amital_ru/index.php:80) собирает общие объекты: загрузчик, настройки, соединение с БД, запрос и ответ. Настройки читает из таблицы `oc_setting` с учётом магазина, строки 95–117.
3. Контроллер выбирает действия для страницы. Например, `ControllerProductProduct::index()` в [catalog/controller/product/product.php:5](E:/GPT/amital/amital_ru/catalog/controller/product/product.php:5).
4. Модель читает и обрабатывает данные. Например, `ModelCatalogProduct::getProduct()` в [catalog/model/catalog/product.php:415](E:/GPT/amital/amital_ru/catalog/model/catalog/product.php:415) получает товар, название, производителя, акционную цену и статус остатка.
5. Шаблон `.tpl` собирает HTML. `Controller::render()` в [system/engine/controller.php:72](E:/GPT/amital/amital_ru/system/engine/controller.php:72) превращает массив данных контроллера в переменные шаблона и выполняет его как PHP.
6. JavaScript выполняется в браузере и может отправлять дополнительные запросы; CSS задаёт внешний вид. Пример подключения JS и CSS: [ami6/template/common/header.tpl:25](E:/GPT/amital/amital_ru/catalog/view/theme/ami6/template/common/header.tpl:25).

Таким образом, название и цена товара приходят из данных, PHP выбирает и рассчитывает результат, `.tpl` размещает его на странице, CSS оформляет, JavaScript обслуживает интерактивные действия.

## Конфигурация и хранилища

**Код:** [config.php:24](E:/GPT/amital/amital_ru/config.php:24) выбирает драйвер `mysqli`, префикс таблиц `oc_`; строки 34–46 содержат выбор кеша и флаги поведения. Значения реквизитов подключения не документируются. [admin/config.php](E:/GPT/amital/amital_ru/admin/config.php) — отдельная конфигурация админки.

Пути в конфигурации рассчитаны на Linux-каталог `/usr/app/amital.ru/`, а не на текущую Windows-папку. Просто запускать эту копию как готовый локальный сайт нельзя: потребуется отдельно подготовленная тестовая среда и согласованный набор данных.

Основная БД хранит каталог, заказы, клиентов и настройки. Sphinx — отдельный поисковый сервис: код запрашивает его индекс `main`, см. [ModelCatalogSphinx::searchByQL():525](E:/GPT/amital/amital_ru/catalog/model/catalog/sphinx.php:525). Индекс можно представить как отдельный подготовленный список для быстрого поиска; его актуальность относительно БД пока неизвестна.

В конфигурации указан `CACHE_DRIVER = 'memcached'`. [Cache::__construct():28](E:/GPT/amital/amital_ru/system/library/cache.php:28) использует PHP-класс `Memcache`, подключается к сервису и предусматривает файловую ветку, если соединение не установлено. Наличие PHP-расширения и работа сервиса не проверены.

## Тема

В `catalog/view/theme` обнаружены `ami2`, `ami3`, `ami3_12012017`, `ami3_backup`, `ami4_pre`, `ami6`, `amit`, `default`.

**Код:** выбор темы идёт через `config_template`, загруженный из БД. Если нужного шаблона нет, многие контроллеры используют `default`; пример — [ControllerCommonHome::index():15](E:/GPT/amital/amital_ru/catalog/controller/common/home.php:15).

**Гипотеза:** `ami6` использовалась при формировании части сохранённого кеша: есть [vq2-catalog_view_theme_ami6_template_common_header.tpl](E:/GPT/amital/amital_ru/vqmod/vqcache/vq2-catalog_view_theme_ami6_template_common_header.tpl) и кеш карточки товара. Это не подтверждает текущий `config_template`, конкретный магазин или актуальность кеша.

## Загрузка изменённого кода

Обе точки входа вызывают `VQMod::bootup()` и `VQMod::modCheck()`. [vqmod/vqmod.php:7](E:/GPT/amital/amital_ru/vqmod/vqmod.php:7) объявляет версию **2.6.1**. `_getMods()` читает именно `vqmod/xml/*.xml`, строка 222.

XML содержит инструкции поиска и замены участков PHP. Результат сохраняется в `vqmod/vqcache`; [vqmod_opencart.xml:8](E:/GPT/amital/amital_ru/vqmod/xml/vqmod_opencart.xml:8) охватывает загрузку ядра, движка и библиотек. Поэтому исходный файл, правило XML и ранее созданный кеш нужно рассматривать совместно. Кеш не редактируют как источник исправления.

Папки `vqmod/1`, `vqmod/2/dev`, `vqmod/2/prod`, `vqback` обнаружены, но их загрузка основными точками входа не установлена. Их нельзя считать активными только по наличию и нельзя удалять на основании этого обзора.
