# Что менять для какой задачи

Это памятка для поиска места изменения, не готовый план патча. На этапе анализа ничего из перечисленного не меняется.

| Задача | Начать с | Что обязательно проследить дальше |
|---|---|---|
| Убрать товары без остатка из категории | [product.php:525](E:/GPT/amital/amital_ru/catalog/model/catalog/product.php:525), `getProducts()`; [getTotalProducts():1121](E:/GPT/amital/amital_ru/catalog/model/catalog/product.php:1121) | Список и количество результатов, дерево категорий, фильтр магазинов, XML, кеш vQmod, AJAX-выдача |
| Убрать их из поиска и подсказок | [sphinx.php:525](E:/GPT/amital/amital_ru/catalog/model/catalog/sphinx.php:525), `searchByQL()`; [sphinxautocomplete.php](E:/GPT/amital/amital_ru/catalog/controller/module/sphinxautocomplete.php) | Схема и обновление индекса, категории/бренды/счётчики, отдельный код подсказок, JS |
| Изменить «В наличии» или доступность кнопки | [product/product.php:456](E:/GPT/amital/amital_ru/catalog/controller/product/product.php:456); [Cart::getCartVars():629](E:/GPT/amital/amital_ru/system/library/cart.php:629) | Складской и розничный остаток, `stock_status_id`, `retail_only`, предзаказ; серверные проверки добавления и оформления |
| Изменить минимальную сумму | [getProduct():440](E:/GPT/amital/amital_ru/catalog/model/catalog/product.php:440); [checkout.php:1841](E:/GPT/amital/amital_ru/catalog/controller/checkout/checkout.php:1841), `notAvailableShipAndPay()`; [low_order_fee.php:3](E:/GPT/amital/amital_ru/catalog/model/total/low_order_fee.php:3) | Сначала уточнить смысл: цена позиции, сумма корзины, ограничение доставки/оплаты или надбавка. Единого места пока не установлено |
| Изменить скидку или цену | [product.php:278](E:/GPT/amital/amital_ru/catalog/model/catalog/product.php:278), расчёт скидок; [cart.php:29](E:/GPT/amital/amital_ru/system/library/cart.php:29) | Покупатель и группа, сроки, акции, купоны, модули итогов, сумма сохранённого заказа |
| Изменить оформление заказа | [checkout/checkout.php](E:/GPT/amital/amital_ru/catalog/controller/checkout/checkout.php) | AJAX-методы, серверная валидация, модель заказа, выбранная тема и её `checkout.js`/`.min.js`; отдельный Quick Checkout нельзя считать активным по имени |
| Добавить/ограничить доставку или оплату | [catalog/model/shipping](E:/GPT/amital/amital_ru/catalog/model/shipping), [catalog/model/payment](E:/GPT/amital/amital_ru/catalog/model/payment) | Настройки расширения, фильтры checkout, адрес/геозона, вес/сумма, внешние ответы |
| Изменить главную | [common/home.php](E:/GPT/amital/amital_ru/catalog/controller/common/home.php), контроллеры блоков `common/content_*`, `common/column_*` | `config_template`, макеты и настройки модулей, реальные шаблоны |
| Изменить внешний вид | [catalog/view/theme](E:/GPT/amital/amital_ru/catalog/view/theme) | Подтвердить тему; проследить подключённый CSS/JS, fallback `default`, исходные и минифицированные файлы |
| Изменить URL товара/категории | [automatic_seo_url.php](E:/GPT/amital/amital_ru/catalog/controller/common/automatic_seo_url.php), `index()`, `rewrite()` | `.htaccess`, старые редиректы в `index.php`, SEO-кеш, sitemap, канонические адреса |
| Исправить экспорт | [feed/yandex_market.php](E:/GPT/amital/amital_ru/catalog/controller/feed/yandex_market.php), [tool/export.php](E:/GPT/amital/amital_ru/admin/controller/tool/export.php) | Соответствующая модель, правила наличия/цен, настройки, вызывающее задание |

Для будущей доработки: найти все влияющие места → установить версии среды и активные настройки → подготовить минимальный патч → описать изменение для покупателя → проверить на тестовой копии → сохранить исходные файлы для отката. Любой запуск, который может менять данные или обращаться во внешние сервисы, требует отдельного согласования пользователя.
