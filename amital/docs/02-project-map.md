# Карта проекта

Корень исходников: `E:\GPT\amital\amital_ru`. Это карта ролей, а не перечень каждого файла.

| Путь | Назначение | С чем связан | Для каких задач изменяют |
|---|---|---|---|
| [.htaccess](E:/GPT/amital/amital_ru/.htaccess) | Правила обработки URL веб-сервером | `index.php`, `_route_`, настройки Apache | Перенаправления и адреса; только после проверки серверной конфигурации |
| [index.php](E:/GPT/amital/amital_ru/index.php) | Вход витрины, загрузка настроек, запуск маршрута | `config.php`, vQmod, `system`, `oc_setting` | Общая инициализация; отдельная служебная ветка очистки требует внимания |
| [config.php](E:/GPT/amital/amital_ru/config.php), [admin/config.php](E:/GPT/amital/amital_ru/admin/config.php) | Пути и параметры подключения | БД, Memcache, Sphinx | Подготовка среды; содержат секреты, не публиковать |
| [system/engine](E:/GPT/amital/amital_ru/system/engine) | Механизм маршрутизации, загрузки моделей и шаблонов | Все контроллеры и модели | Общие механизмы; обычные изменения магазина обычно находятся выше этого уровня |
| [catalog/controller/common](E:/GPT/amital/amital_ru/catalog/controller/common) | Главная, шапка, подвал, блоки, SEO-адреса | Тема, модули, макеты из БД | Структура страниц и маршрутизация |
| [catalog/controller/product](E:/GPT/amital/amital_ru/catalog/controller/product) | Категории, товар, поиск и другие списки | Модели каталога, Sphinx, шаблоны `product` | Правила выдачи, данные карточки, сортировка, пагинация |
| [catalog/model/catalog/product.php](E:/GPT/amital/amital_ru/catalog/model/catalog/product.php) | Чтение товаров, цены, остатки и количество результатов | `oc_product` и связанные таблицы | Условия выборки и вычисляемые признаки товара |
| [catalog/model/catalog/sphinx.php](E:/GPT/amital/amital_ru/catalog/model/catalog/sphinx.php), [sphinxql/list.php](E:/GPT/amital/amital_ru/catalog/model/catalog/sphinxql/list.php) | Поиск и выборки через поисковый индекс | Sphinx, контроллеры каталога, автодополнение | Поиск, фильтры, подсказки; нужны сведения об индексах |
| [catalog/controller/checkout](E:/GPT/amital/amital_ru/catalog/controller/checkout) | Корзина и оформление заказа | `Cart`, доставка, оплата, модель заказа, JS | Добавление товара, проверки, выбор способа получения и оплаты |
| [system/library/cart.php](E:/GPT/amital/amital_ru/system/library/cart.php) | Состав и суммы корзины, остатки, доступные магазины | Сессия, БД, покупатель | Пересчёт корзины и общая логика доступности |
| [catalog/model/total](E:/GPT/amital/amital_ru/catalog/model/total) | Составляющие итоговой суммы | Корзина и включённые расширения | Скидки, купоны, доставка, надбавки |
| [catalog/model/shipping](E:/GPT/amital/amital_ru/catalog/model/shipping), [catalog/controller/payment](E:/GPT/amital/amital_ru/catalog/controller/payment) | Реализации доставки и оплаты | Настройки БД и внешние сервисы | Доступность способов, расчёт, проведение платежа |
| [catalog/controller/account](E:/GPT/amital/amital_ru/catalog/controller/account) | Регистрация, вход, история и действия с заказами | Модели `account`, клиент, сессия | Личный кабинет; подробный разбор впереди |
| [catalog/controller/module](E:/GPT/amital/amital_ru/catalog/controller/module) | Блоки главной и других страниц | Настройки расширений, макеты, тема | Подборки, баннеры, акции, подписки, предзаказ |
| [catalog/view/theme](E:/GPT/amital/amital_ru/catalog/view/theme) | Шаблоны, стили и JS нескольких тем | `config_template`, контроллеры | Вёрстка; сначала подтвердить активную тему |
| [catalog/view/javascript](E:/GPT/amital/amital_ru/catalog/view/javascript), [js](E:/GPT/amital/amital_ru/js), [css](E:/GPT/amital/amital_ru/css) | Общие ресурсы браузера | Шаблоны подключают конкретные файлы | Интерактивность и оформление; проверять обычные и `.min` версии |
| [catalog/language](E:/GPT/amital/amital_ru/catalog/language) | Тексты интерфейса | Контроллеры и выбранный язык | Названия кнопок, предупреждения, подписи |
| [admin/index.php](E:/GPT/amital/amital_ru/admin/index.php), [admin/controller](E:/GPT/amital/amital_ru/admin/controller), [admin/model](E:/GPT/amital/amital_ru/admin/model), [admin/view](E:/GPT/amital/amital_ru/admin/view) | Отдельное приложение управления магазином | Та же БД, права пользователя, vQmod | Каталог, заказы, настройки, расширения и экспорт |
| [vqmod/xml](E:/GPT/amital/amital_ru/vqmod/xml) | Правила изменения кода при загрузке | Оригинальные PHP/TPL, кеш vQmod | Исправления, связанные с существующими модификаторами |
| [vqmod/vqcache](E:/GPT/amital/amital_ru/vqmod/vqcache) | Сгенерированные варианты файлов | XML и исходники | Диагностика результата применения; не источник патча |
| [api](E:/GPT/amital/amital_ru/api) | Набор самостоятельных PHP-точек и вспомогательных классов | Поиск, геоданные, доставка — по именам файлов; связи ещё исследуются | Разбирать по конкретному вызывающему сценарию; не считать единой документированной API |
| [1c/index.php](E:/GPT/amital/amital_ru/1c/index.php) | Проверка доступности внешнего узла и перенаправление к авторизации | `ip_addr.txt`, системная команда `nc` | Доступ к внешней системе; импорт каталога здесь не обнаружен |
| [catalog/controller/feed](E:/GPT/amital/amital_ru/catalog/controller/feed), [generator](E:/GPT/amital/amital_ru/generator), [utils](E:/GPT/amital/amital_ru/utils) | Выгрузки, sitemap, служебные утилиты | Каталог, поисковый индекс, файлы | Экспорт и обслуживание; запуск может менять данные |
| [image](E:/GPT/amital/amital_ru/image), [img](E:/GPT/amital/amital_ru/img), [download](E:/GPT/amital/amital_ru/download), [files](E:/GPT/amital/amital_ru/files) | Изображения, загрузки, документы и прикладные файлы | БД, шаблоны и интеграции | Контент; массово не исследовались, могут содержать закрытые данные |
| [vendor](E:/GPT/amital/amital_ru/vendor), [system/PHPExcel](E:/GPT/amital/amital_ru/system/PHPExcel) | Сторонние библиотеки | Например, SendPulse и экспорт таблиц | Обычно меняется вызывающий код; внутренности библиотек не разбирались |

## Кеши, логи и создаваемые файлы

- В конфигурации предусмотрены `system/cache` и `system/logs`; при проверке этих каталогов в данной копии не обнаружено. Это не означает их отсутствия на сервере.
- `system/cache1/autourl`, `system/cache2/autourl` присутствуют. [AutomaticSeoUrl::index():45](E:/GPT/amital/amital_ru/catalog/controller/common/automatic_seo_url.php:45) работает с `DIR_CACHE/autourl`, а отдельные очистители используют другие пути. Не смешивать их назначения без проверки.
- `vqmod/mods.cache`, `vqmod/checked.cache`, `vqmod/vqcache` — служебные результаты vQmod; логирование предусмотрено в `vqmod/logs`, [vqmod.php:19](E:/GPT/amital/amital_ru/vqmod/vqmod.php:19).
- `sitemap.xml`, `sm_*.xml` лежат в корне. Генератор [ControllerFeedFastSitemap::index():14](E:/GPT/amital/amital_ru/catalog/controller/feed/fast_sitemap.php:14), `saveXML():308` пишет XML. Сам факт наличия XML не доказывает свежесть.
- Обнаружены файлы логов в `api`, `utils`, `system/database`; содержимое не выгружалось из-за возможных персональных и служебных данных.
- `index.html` и `index.php` одновременно существуют. Какой будет выбран для `/`, зависит также от `DirectoryIndex`/настроек веб-сервера; локальный `.htaccess` этого не определяет.
- `~f`, `~i`, `~o`, `~t`, `~v`, `amital.ru`, `awstats`, `tracker`, `tmp`, `override` требуют отдельной классификации. Имя папки не доказывает её роль или неиспользование.
