# Расширения, интеграции и фоновые процессы

Дата: 24.09.2026. Ни один описанный процесс не запускался. Наличие точки запуска не доказывает наличие задания в расписании.

## Правила vQmod

Загрузчик [VQMod::_getMods():222](E:/GPT/amital/amital_ru/vqmod/vqmod.php:222) выбирает `vqmod/xml/*.xml`. В этой папке обнаружены восемь подходящих файлов:

| Правило | Что подтверждено содержимым | Что ещё проверить |
|---|---|---|
| [vqmod_opencart.xml](E:/GPT/amital/amital_ru/vqmod/xml/vqmod_opencart.xml) | Подключение `VQMod::modCheck` к загрузке PHP ядра и библиотек | Согласованность сформированного кеша |
| [sphinx_search.xml](E:/GPT/amital/amital_ru/vqmod/xml/sphinx_search.xml) | Заявлена версия расширения 1.2.2; изменения поиска, категорий, модели товара и админки | Какие операции совпадают с текущими исходниками; часть целей шаблонов указана явно для `ami3` |
| [ocsql.xml](E:/GPT/amital/amital_ru/vqmod/xml/ocsql.xml) | Предлагает заменять вызовы моделей в `catalog/controller/product/*.php` на `model_ocsql` | Обнаружено несоответствие объявленных замен имеющейся модели и сохранённому кешу; подробности в списке рисков |
| [Auto_SEO_URL.xml](E:/GPT/amital/amital_ru/vqmod/xml/Auto_SEO_URL.xml) | Цель — шаблон шапки админки | Основная маршрутизация уже подключена непосредственно из `index.php`; не приписывать всю SEO-логику этому XML |
| [sbacquiring.xml](E:/GPT/amital/amital_ru/vqmod/xml/sbacquiring.xml) | Изменения заказа в кабинете и админке для платёжного модуля | Настройки, используемый провайдер и применение замен |
| [otlog_opl.xml](E:/GPT/amital/amital_ru/vqmod/xml/otlog_opl.xml) | Модификация контроллера и модели заказов кабинета, названа «Отложенная оплата» | Условия доступности и связь с текущими оплатами |
| [pochtaros.xml](E:/GPT/amital/amital_ru/vqmod/xml/pochtaros.xml) | Правила для шаблонов доставки, языковых файлов и моделей админки; указана версия 3.10 | Активность доставки и применимость к выбранному checkout |
| [export.xml](E:/GPT/amital/amital_ru/vqmod/xml/export.xml) | Добавление экспортного инструмента в админку | Фактические операции импорта/экспорта и права |

Файлы `a_vqmod_quickcheckout.xml_` и `ocsql._xml` не подходят под этот шаблон загрузки. Это вывод о данном загрузчике, а не доказательство отсутствия любых других способов подключения. В `admin/mbooth/xml` есть отдельные XML Quick Checkout/Shopunity; их жизненный цикл ещё не исследован.

## Найденные интеграции и задания

| Процесс | Точка и подтверждённая операция | Источник / результат | Статус |
|---|---|---|---|
| Поиск Sphinx | [ModelCatalogSphinx::searchByQL():525](E:/GPT/amital/amital_ru/catalog/model/catalog/sphinx.php:525), поиск в `main` | Поисковый индекс; результаты затем связаны с карточками | Вызов в основном контроллере поиска есть. Версия сервиса и свежесть индекса неизвестны |
| Обновление индексов из админки | [admin/model/catalog/sphinxsearch.php:70](E:/GPT/amital/amital_ru/admin/model/catalog/sphinxsearch.php:70), `insertOrReplace()`, `delete()`, `sphinxProduct()`, `sphinxCategory()`; XML Sphinx добавляет вызовы | Товары/категории и поисковые индексы | Нужно проверить условия модуля, применимость XML и полный путь обновления |
| Построение словаря подсказок | [utils/create_suggestions.php:47](E:/GPT/amital/amital_ru/utils/create_suggestions.php:47), `BuildDictionarySQL()` | Читает `dict.txt`, удаляет и создаёт таблицу `oc_suggest`, заполняет её; пишет лог | Реально меняет БД. Команда `indexer --buildstops` в строке 110 — комментарий, не доказательство автоматического запуска |
| Захват/подтверждение платежей YooKassa | [capture_payments.php:3](E:/GPT/amital/amital_ru/capture_payments.php:3), ограничение CLI; вызовы `updatePaymentsStatuses()`, `capturePayment()`, `confirmOrderPayment()` со строки 142 | Платёжный сервис и заказы | Исполняемый служебный сценарий найден; расписание и действующая конфигурация неизвестны |
| Задание СДЭК | [cdek_integrator_cron.php:139](E:/GPT/amital/amital_ru/cdek_integrator_cron.php:139) проверяет контроллер и вызывает `module/cdek_integrator/cron` | Административная часть | Целевой `admin/controller/module/cdek_integrator.php` в копии отсутствует; есть явная ветка 404. Это не исключает других интеграций СДЭК |
| Другие доставки | [catalog/model/shipping](E:/GPT/amital/amital_ru/catalog/model/shipping) содержит `cdek.php`, `pochtaros.php`, `russianpost.php`, `shops.php` и др. | Адрес, настройки, тарифы | Наличие реализации подтверждено; подключённые способы нужно сверить по БД и checkout |
| YooMoney/YooKassa, Сбер и другие оплаты | [catalog/controller/payment](E:/GPT/amital/amital_ru/catalog/controller/payment) содержит `yoomoney.php`, `sbacquiring.php`, `sbrf_online.php`, `rbs.php`, `bank_transfer.php` и др. | Настройки, заказ, внешние ответы | Нельзя считать все найденные способы активными |
| Яндекс Маркет | [ControllerFeedYandexMarket::index():17](E:/GPT/amital/amital_ru/catalog/controller/feed/yandex_market.php:17) проверяет `yandex_market_status`, загружает `export/yandex_market` | Каталог → товарная выгрузка | Есть отдельное правило доступности в строке 148: положительный остаток **или** определённый статус. Расписание неизвестно |
| Sitemap | [ControllerFeedFastSitemap::index():14](E:/GPT/amital/amital_ru/catalog/controller/feed/fast_sitemap.php:14), `saveXML():308` | Данные товаров, категорий, производителей, серий и информации → XML | Файлы sitemap найдены. Способ и периодичность запуска неизвестны |
| Табличный экспорт/импорт | [admin/controller/tool/export.php:5](E:/GPT/amital/amital_ru/admin/controller/tool/export.php:5), `index()`, `download():108`; модель `tool/export`, PHPExcel | Каталог / таблицы | Инструмент и XML присутствуют; формат и правила перезаписи требуют разбора |
| SendPulse и брошенные корзины | [system/library/send_pulse.php:13](E:/GPT/amital/amital_ru/system/library/send_pulse.php:13), `__construct()`, `addEmails()`; [tool/forgotten_cart.php:31](E:/GPT/amital/amital_ru/catalog/controller/tool/forgotten_cart.php:31), `index()` | Подписки/корзины → внешние обращения | Код вызовов найден; активность, отбор получателей и расписание не установлены |
| Отправка почтовой очереди | [ControllerToolEmailAgent::index():4](E:/GPT/amital/amital_ru/catalog/controller/tool/email_agent.php:4) вызывает `model_tool_email_agent->sendMessages()` | Почтовая очередь | Запуск может отправлять письма. Есть отдельные контроллеры рассылок; автоматически не запускать |
| Вход к внешней системе из `1c` | [1c/index.php:3](E:/GPT/amital/amital_ru/1c/index.php:3), глобальный код по параметрам `acc_start`/`hrm_start`/`sklad_start` | Список узлов → проверка `nc` → перенаправление к авторизации | Это подтверждённый переход, а не обнаруженный механизм обмена каталогом |
| Очистка SEO-кеша | [index.php:6](E:/GPT/amital/amital_ru/index.php:6), [cache_cleaner.php:11](E:/GPT/amital/amital_ru/cache_cleaner.php:11) | Удаление файлов в разных папках | Ручные HTTP-точки; расписание неизвестно; обнаружены проблемы проверки и расхождение путей |

В `system/library/sphinx` обнаружены шаблоны `sphinx.conf.in`, `sphinx.rt.conf.in`. Это не действующая конфигурация серверного Sphinx. В просмотренном дереве файлов дамп `.sql` и файлы расписания не обнаружены; содержимое внешнего архива не исследовалось.

## Расширения витрины, которые заслуживают отдельного разбора

В [catalog/controller/module](E:/GPT/amital/amital_ru/catalog/controller/module) есть `personal_offers`, `preorder`, `product_sets`, `products_buy_with`, `products_of_seria`, `products_of_set`, `promo_products`, `retail_events`, `retail_promo`, `subscription`, `latest_viewed`, `quickcheckout`, `pavblog*`. На данном этапе это перечень имеющихся реализаций, не список включённых блоков. Их размещение зависит от расширений, макета, позиции и статуса — [common/content_top.php:43](E:/GPT/amital/amital_ru/catalog/controller/common/content_top.php:43).
