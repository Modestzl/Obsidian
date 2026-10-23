# Лишние файлы, дубликаты и кандидаты на архивирование

Дата: 24.09.2026. Корень: `E:\GPT\amital\amital_ru`.

**Вывод:** в копии накопились служебные файлы, несколько почти одинаковых копий кода и тем. Но точный дубль по содержимому не обязательно лишний по назначению. Найдены реальные зависимости от файлов с «тестовыми» именами и ресурсов других тем. Удаление не выполнялось и этим отчётом не предлагается автоматически.

## Объём и метод

- Инвентаризация: **12 005 файлов**, **1 552 300 624 байта** (около 1,45 ГиБ). Внешний архив `E:\GPT\amital\amital-09-23-2026.tar.gz` не входит в подсчёт.
- На точные совпадения проверены **5 804 технических файла**. Найдены **582 группы**, **2 152 файла в группах**, то есть **1 570 дополнительных одинаковых копий** общим размером **8 609 807 байт** (8,21 МиБ). Сравнение — размер и SHA-256; нулевые файлы в группы не включены.
- Эти 1 570 копий **не являются списком разрешённых к удалению файлов**. В счёт входят однотипные шаблоны, языковые файлы, сторонние компоненты и сгенерированный код по разным путям.
- Для пяти пар каталогов выполнено отдельное сравнение одноимённых файлов, включая ресурсы тем. Изображения при этом не разбирались по содержанию; сравнивались только размеры и хеши.
- Поиск буквальных упоминаний кандидатов выполнен в **4 754 файлах** PHP/TPL/JS/CSS/XML/HTML/INI/JSON и `.htaccess`; архивные папки и кеш помечены отдельно. Исключались пользовательские файлы, скачиваемые материалы, большая часть внешних библиотек, изображения и SEO-кеш. Выдача поиска может содержать комментарии и совпадения слов, а не настоящие вызовы; важные зависимости проверены вручную.
- **18 файлов** уверенно классифицированы как служебные метаданные ОС/редактора, не логика магазина. Ещё есть архивные и диагностические кандидаты с разной степенью уверенности.
- Полный пофайловый реестр кандидатов содержит **1 481 уникальный путь**, **51 451 807 байт** (49,07 МиБ), включая содержимое предположительно архивных папок и тему `ami4_pre` с неизвестной активностью. Это объём для проверки, **не обещание освобождения диска**. Статистику совпадений нельзя прибавлять к этому объёму: списки пересекаются.

## 1. Вероятные архивные папки

Для перечисленных папок буквальных внешних ссылок в исследованном коде не найдено. Это дополнительный признак, а не доказательство отсутствия загрузки через настройки БД, внешние скрипты или сервер.

| Путь | Файлов / размер | Подтверждённое сравнение | Вывод |
|---|---:|---|---|
| [vqback/vqmod](E:/GPT/amital/amital_ru/vqback/vqmod) | 177 / 6,41 МиБ | С `vqmod` совпадают 166 файлов; 11 отличаются; уникальных относительных имён слева нет | Вероятная резервная копия vQmod. Содержит отличия, в том числе `vqmod.php`, `.htaccess` и кеш категории; сохранять как архив до проверки |
| [vqmod/1](E:/GPT/amital/amital_ru/vqmod/1) | 26 / 0,40 МиБ | Отдельные XML, установщик и кеш; основной загрузчик читает `vqmod/xml/*.xml` | Похоже на отложенный комплект/снимок. Штатными точками входа подключение этой папки не обнаружено; уникальность всей папки относительно остальных не утверждается |
| [vqmod/2](E:/GPT/amital/amital_ru/vqmod/2) | 101 / 1,24 МиБ | Внутри `dev` и `prod` по 50 файлов; 49 совпадают, отличается только `checked.cache`. Ещё один файл — `vqmod/2/vqmod.php` | Сильный кандидат на вынесение из рабочего дерева после проверки развёртывания |
| [catalog/view/theme/ami3_backup](E:/GPT/amital/amital_ru/catalog/view/theme/ami3_backup) | 381 / 13,10 МиБ | С `ami3` совпадают 347 файлов; 34 отличаются | Вероятная резервная тема; не точный полный дубль |
| [catalog/view/theme/ami3_12012017](E:/GPT/amital/amital_ru/catalog/view/theme/ami3_12012017) | 382 / 12,98 МиБ | С `ami3` совпадают 348 файлов; 34 отличаются | Вероятный датированный снимок; дата в имени не устанавливает фактическую дату сохранения |
| [catalog/view/theme/ami4_pre](E:/GPT/amital/amital_ru/catalog/view/theme/ami4_pre) | 381 / 12,96 МиБ | Внешних буквальных упоминаний имени темы не найдено; полного доказательства дублирования нет | Более слабый кандидат: назначение/активность надо проверить по `config_template` всех магазинов |

Подключение основного vQmod подтверждается [index.php:48](E:/GPT/amital/amital_ru/index.php:48) и [admin/index.php:17](E:/GPT/amital/amital_ru/admin/index.php:17); область XML — [VQMod::_getMods():222](E:/GPT/amital/amital_ru/vqmod/vqmod.php:222). Тема выбирается динамически, например [ControllerCommonHome::index():15](E:/GPT/amital/amital_ru/catalog/controller/common/home.php:15).

## 2. Отдельные резервные или отключённые файлы

Во всех строках ниже не найдено буквальных вызывающих ссылок в области поиска. Вывод «вероятная копия» дополнительно основан на сравнении или несовпадении имени с загрузчиком. Это не доказательство, что файл никогда не используется вручную.

| Путь | Основание | Что сохранить/проверить |
|---|---|---|
| [catalog/model/catalog/product01.php](E:/GPT/amital/amital_ru/catalog/model/catalog/product01.php) | Объявляет `ModelCatalogProduct`, как основной `product.php`; около 97,7% сходства последовательности строк, есть 11 блоков отличий | Не точный дубль. Загрузчик для `catalog/product01` ожидал бы `ModelCatalogProduct01`; см. `Loader::model()` ниже. Сохранить отличающуюся логику товара |
| [catalog/controller/checkout/00checkout.php](E:/GPT/amital/amital_ru/catalog/controller/checkout/00checkout.php) | Объявляет `ControllerCheckoutCheckout`; около 99,7% сходства с `checkout.php`, 5 блоков отличий | Вероятный промежуточный вариант оформления |
| [catalog/controller/checkout/01checkout.php](E:/GPT/amital/amital_ru/catalog/controller/checkout/01checkout.php) | Тот же класс; около 99,7% сходства с `checkout.php`, 6 блоков отличий | С `02checkout.php` совпадает размер, но отличается одна строка — это **не точный дубль** |
| [catalog/controller/checkout/02checkout.php](E:/GPT/amital/amital_ru/catalog/controller/checkout/02checkout.php) | Тот же класс; около 99,7% сходства с `checkout.php`, 6 блоков отличий | Сохранить уникальный вариант до анализа различий |
| [catalog/controller/checkout/03checkout.php](E:/GPT/amital/amital_ru/catalog/controller/checkout/03checkout.php) | Тот же класс; около 99,6% сходства с `checkout.php`, 5 блоков отличий | Вероятный промежуточный вариант |
| [catalog/controller/checkout/checkout05.php](E:/GPT/amital/amital_ru/catalog/controller/checkout/checkout05.php) | Тот же класс; около 99,7% сходства с `checkout.php`, 4 блока отличий | Имя не соответствует классу маршрута `checkout/checkout05` |
| [catalog/view/theme/ami6/template/checkout/!checkout.tpl](E:/GPT/amital/amital_ru/catalog/view/theme/ami6/template/checkout/!checkout.tpl) | Есть основной `checkout.tpl`; имена/размеры и хеши различаются; явного выбора `!checkout.tpl` не найдено | Вероятный снимок шаблона, сохранить отличия |
| [catalog/view/theme/ami6/template/product/promo_list01.tpl](E:/GPT/amital/amital_ru/catalog/view/theme/ami6/template/product/promo_list01.tpl) | Рядом `promo_list.tpl`; 9 685 против 9 694 байт, содержимое различается | Вероятный вариант списка акций; проверить динамический выбор шаблона |
| [1c/index.php_back](E:/GPT/amital/amital_ru/1c/index.php_back) | Резервное имя; заметно отличается от `1c/index.php` | Не точный дубль; сохранить историю доступа к внешней системе |
| [catalog/controller/pavblog/blog.php_](E:/GPT/amital/amital_ru/catalog/controller/pavblog/blog.php_) | Рядом рабочее имя `blog.php`; примерно 91,6% сходства строк, 9 блоков отличий | Не загружается обычным маршрутом, который строит имя с окончанием `.php` |
| [admin/controller/payment/google_checkout.php_](E:/GPT/amital/amital_ru/admin/controller/payment/google_checkout.php_) | Содержит класс платёжного контроллера, но окончание `.php_`; одноимённого `.php` в этой папке нет | Вероятно отключённый контроллер, **не дубликат существующего файла**; проверить необходимость сохранения модуля |
| [admin/controller/payment/authorizenet_sim.php_](E:/GPT/amital/amital_ru/admin/controller/payment/authorizenet_sim.php_) | Аналогично: `.php_`, соответствующий `.php` отсутствует | Сохранить как отключённый компонент до решения по модулю |
| [vqmod/xml/ocsql._xml](E:/GPT/amital/amital_ru/vqmod/xml/ocsql._xml) | Не подходит под `*.xml`; отличается от `ocsql.xml`, а не повторяет его | Кандидат на архивирование после сравнения с проблемным действующим правилом |
| [inde_x.html](E:/GPT/amital/amital_ru/inde_x.html) | Статическая приветственная страница `Welcome!`, примерно 97,3% сходства с корневым `index.html` | Вероятный старый вариант стартовой страницы; это не точный дубль |

Критерии имени подтверждены [Action::__construct():24](E:/GPT/amital/amital_ru/system/engine/action.php:24) и [Loader::model():39](E:/GPT/amital/amital_ru/system/engine/loader.php:39). Числовые копии checkout сохраняют старое имя класса, поэтому нельзя считать их обычными самостоятельными контроллерами только по наличию `.php`. Прямое включение внешним кодом всё ещё возможно.

Проценты — сходство последовательностей строк по `difflib`, не процент одинаковой бизнес-логики. Координаты всех блоков отличий сохранены без вывода секретов.

## 3. Диагностика и тестовые страницы

Для этих файлов вызывающие ссылки в исследованной области не найдены. Они могут открываться напрямую администратором или внешним мониторингом, поэтому их фактическое неиспользование не доказано.

| Путь | Подтверждено кодом | Оценка |
|---|---|---|
| [info.php:2](E:/GPT/amital/amital_ru/info.php:2) | `phpinfo()` | Диагностика PHP, кандидат на удаление из публичной поставки после проверки назначения |
| [utils/pinf.php:3](E:/GPT/amital/amital_ru/utils/pinf.php:3) | Ещё один `phpinfo()` | Дублирует диагностическую роль, но не байты файла |
| [api/testmail.php:3](E:/GPT/amital/amital_ru/api/testmail.php:3) | Непосредственный вызов `mail()` | Проверка отправки почты; не запускать ради анализа |
| [api/1123.php:2](E:/GPT/amital/amital_ru/api/1123.php:2) | Внешнее чтение JSON и `var_dump()` результатов | Диагностический запрос, требует выяснить владельца/назначение |
| [traceroute.php:12](E:/GPT/amital/amital_ru/traceroute.php:12) | Выполнение системной команды через `exec()` | Сетевая диагностика; не запускался |
| [ga_test.html:3](E:/GPT/amital/amital_ru/ga_test.html:3) | Заголовок «Тест Google Analitycs» | Тестовая страница аналитики; проверить, не нужна ли для текущих проверок |
| [pvz_yandex2.php:2](E:/GPT/amital/amital_ru/pvz_yandex2.php:2) | Выводит только `Какой-то текст` | Сильный кандидат: заглушка, а не реализация ПВЗ. Основной `pvz_yandex.php` другой |

## 4. Служебные файлы ОС и редактора

Это **не код магазина**. Для `dwsync.xml` остаётся вопрос старого рабочего процесса синхронизации разработчика. Всего 18 файлов; содержимое служебных метаданных с локальными именами/адресами не копировалось.

| Путь | Тип |
|---|---|
| [admin/view/javascript/shopunity/.DS_Store](E:/GPT/amital/amital_ru/admin/view/javascript/shopunity/.DS_Store) | Метаданные Finder/macOS |
| [admin/view/javascript/shopunity/uniform/_notes/dwsync.xml](E:/GPT/amital/amital_ru/admin/view/javascript/shopunity/uniform/_notes/dwsync.xml) | Метаданные синхронизации редактора; корневой элемент `dwsync` |
| [admin/view/javascript/shopunity/uniform/images/_notes/dwsync.xml](E:/GPT/amital/amital_ru/admin/view/javascript/shopunity/uniform/images/_notes/dwsync.xml) | То же |
| [admin/view/javascript/shopunity/uniform/css/_notes/dwsync.xml](E:/GPT/amital/amital_ru/admin/view/javascript/shopunity/uniform/css/_notes/dwsync.xml) | То же |
| [admin/view/javascript/shopunity/tooltip/_notes/dwsync.xml](E:/GPT/amital/amital_ru/admin/view/javascript/shopunity/tooltip/_notes/dwsync.xml) | То же |
| [admin/view/javascript/shopunity/codemirror/_notes/dwsync.xml](E:/GPT/amital/amital_ru/admin/view/javascript/shopunity/codemirror/_notes/dwsync.xml) | То же |
| [catalog/view/theme/ami2/image/icons/Thumbs.db](E:/GPT/amital/amital_ru/catalog/view/theme/ami2/image/icons/Thumbs.db) | Кеш миниатюр Windows |
| [catalog/view/theme/ami2/image/temp/Thumbs.db](E:/GPT/amital/amital_ru/catalog/view/theme/ami2/image/temp/Thumbs.db) | То же |
| [catalog/view/theme/ami2/stylesheet/images/icons/Thumbs.db](E:/GPT/amital/amital_ru/catalog/view/theme/ami2/stylesheet/images/icons/Thumbs.db) | То же |
| [catalog/view/theme/ami3/image/icons/Thumbs.db](E:/GPT/amital/amital_ru/catalog/view/theme/ami3/image/icons/Thumbs.db) | То же |
| [catalog/view/theme/ami3/image/temp/Thumbs.db](E:/GPT/amital/amital_ru/catalog/view/theme/ami3/image/temp/Thumbs.db) | То же |
| [catalog/view/theme/ami3/stylesheet/images/icons/Thumbs.db](E:/GPT/amital/amital_ru/catalog/view/theme/ami3/stylesheet/images/icons/Thumbs.db) | То же |
| [catalog/view/theme/ami3_backup/image/icons/Thumbs.db](E:/GPT/amital/amital_ru/catalog/view/theme/ami3_backup/image/icons/Thumbs.db) | То же |
| [catalog/view/theme/ami3_backup/image/temp/Thumbs.db](E:/GPT/amital/amital_ru/catalog/view/theme/ami3_backup/image/temp/Thumbs.db) | То же |
| [catalog/view/theme/ami3_backup/stylesheet/images/icons/Thumbs.db](E:/GPT/amital/amital_ru/catalog/view/theme/ami3_backup/stylesheet/images/icons/Thumbs.db) | То же |
| [catalog/view/theme/ami3_12012017/image/icons/Thumbs.db](E:/GPT/amital/amital_ru/catalog/view/theme/ami3_12012017/image/icons/Thumbs.db) | То же |
| [catalog/view/theme/ami3_12012017/image/temp/Thumbs.db](E:/GPT/amital/amital_ru/catalog/view/theme/ami3_12012017/image/temp/Thumbs.db) | То же |
| [catalog/view/theme/ami3_12012017/stylesheet/images/icons/Thumbs.db](E:/GPT/amital/amital_ru/catalog/view/theme/ami3_12012017/stylesheet/images/icons/Thumbs.db) | То же |

## 5. Файлы, которые нельзя объявлять лишними по имени

| Путь | Почему исключён из списка простого удаления |
|---|---|
| [api/test.php](E:/GPT/amital/amital_ru/api/test.php) | Ссылки есть в [ControllerProductSearch::index():255](E:/GPT/amital/amital_ru/catalog/controller/product/search.php:255) и ветке `gifts` со строки 279. Контроллер формирует адрес `api/test.php` и читает результат через `file_get_contents`. Удалённый адрес не доказывает, что обслуживается именно этой локальной копией, но явно показывает прикладную роль файла с таким именем |
| [vqmod/xml/a_vqmod_quickcheckout.xml_](E:/GPT/amital/amital_ru/vqmod/xml/a_vqmod_quickcheckout.xml_) | Не загружается текущим шаблоном `*.xml`, **но нужен механизму установки**: [ControllerModuleQuickcheckout::install():507](E:/GPT/amital/amital_ru/admin/controller/module/quickcheckout.php:507) переименовывает его в `.xml`; `uninstall():516` выполняет обратное действие |
| [catalog/controller/checkout/woc_checkout_cart.php](E:/GPT/amital/amital_ru/catalog/controller/checkout/woc_checkout_cart.php) | Вызывается из основного [checkout.php:1307](E:/GPT/amital/amital_ru/catalog/controller/checkout/checkout.php:1307); ссылки также есть в `ami6/js/cart.js:67` и `checkout.js`. Это часть связанной цепочки корзины |
| [catalog/view/theme/ami2](E:/GPT/amital/amital_ru/catalog/view/theme/ami2) | Даже при использовании `ami6` шаблон [ami6/template/checkout/login.tpl:80](E:/GPT/amital/amital_ru/catalog/view/theme/ami6/template/checkout/login.tpl:80) ссылается на изображения внутри `ami2`. Блок имеет класс `hidden`, что не делает путь автоматически ненужным |
| [catalog/view/theme/ami3](E:/GPT/amital/amital_ru/catalog/view/theme/ami3) | [ami6/template/account/register.tpl:55](E:/GPT/amital/amital_ru/catalog/view/theme/ami6/template/account/register.tpl:55) использует `ami3/image/icons/vk.png` и `ok.png`. XML Sphinx также содержит пути `ami3` |
| [catalog/view/theme/default](E:/GPT/amital/amital_ru/catalog/view/theme/default) | Контроллеры используют как запасной вариант при отсутствии шаблона выбранной темы; пример `common/home.php:15` |
| [catalog/view/theme/amit](E:/GPT/amital/amital_ru/catalog/view/theme/amit) | Неизвестно, выбрана ли эта тема в настройках. Совпадения слова `amit` в автоматическом поиске включают имена картинок, поэтому не считаются доказательством подключения темы |
| [index.html](E:/GPT/amital/amital_ru/index.html) | Приветственная страница, но выбор между `index.html` и `index.php` может определяться `DirectoryIndex` на сервере. Не удалять только по виду содержимого |
| [vqmod/vqcache](E:/GPT/amital/amital_ru/vqmod/vqcache) | Автоматически созданные, но используемые файлы. Ранее найдено несоответствие XML и модели OCSQL; перегенерация кеша без проверки может изменить поведение |
| [catalog/view/theme/ami6/js/checkout.js](E:/GPT/amital/amital_ru/catalog/view/theme/ami6/js/checkout.js) и [checkout.min.js](E:/GPT/amital/amital_ru/catalog/view/theme/ami6/js/checkout.min.js) | Исходная и сжатая версии; обе роли могут быть нужны. Общий смысл кода не делает одну из них ненужной |
| [admin/controller/tool/backup.php](E:/GPT/amital/amital_ru/admin/controller/tool/backup.php) | `backup` в имени обозначает функцию резервного копирования, а не архивный файл |

Картинки, документы, языковые пакеты и модули оплаты/доставки не объявлялись неиспользуемыми без БД. Файлы подтверждения домена, `.well-known`, карты сайта, `robots.txt`, внешний архив проекта и библиотеки также не записаны в мусор только из-за отсутствия обычных PHP-ссылок.

`system/cache1`, `system/cache2` и вложенная папка `amital.ru` в этой копии не содержат файлов. Их удаление почти не освободит места; необходимость каталогов проверяется отдельно.

## Реестры и воспроизводимость

- [Полный список кандидатов: 1 481 путь](E:/GPT/amital/docs/08-cleanup-candidates.csv): абсолютный путь, размер, категория, основание и проверка перед удалением. Папки развёрнуты до отдельных файлов; повторных строк нет.
- [Все найденные точные дубликаты](E:/GPT/amital/docs/08-exact-duplicates.csv): 2 152 строки с путями, группой, размером и SHA-256. Это справочник совпадений, а не список мусора.
- [Сравнение папок](E:/GPT/amital/docs/08-folder-comparison.json): количество совпадений и имена отличающихся файлов.
- [Сравнение отдельных файлов](E:/GPT/amital/docs/08-file-comparison.json): совпадение байтов, сходство строк и координаты отличий без текста исходников.
- [Ссылки из кода](E:/GPT/amital/docs/08-reference-evidence.csv) и [сводка](E:/GPT/amital/docs/08-reference-summary.json): путь вызывающего файла и номер строки без её содержимого.
- [Сводные размеры и количества](E:/GPT/amital/docs/08-inventory-summary.json).

Скрипты `audit_file_candidates.py`, `audit_file_references.py`, `build_cleanup_manifest.py` в этой папке — инструменты данного анализа. Они читают исходники и записывают отчёты только в `docs`; не выполняют PHP, не подключаются к БД и не обращаются в сеть.

Если будет отдельная задача на очистку, сначала подтвердить активные темы и внешние задания, сохранить уникальные отличия вне публичного каталога и проверить на тестовой копии главную, регистрацию, поиск, корзину и оформление. Сейчас никаких файлов проекта не перемещали и не удаляли.
