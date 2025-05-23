# Альтернативы, источники вдохновения и сравнения

Что вдохновило на создание **FastAPI**, сравнение его с альтернативами и чему он научился у них.

## Введение

**FastAPI** не существовал бы, если б не было более ранних работ других людей.

Они создали большое количество инструментов, которые вдохновили меня на создание **FastAPI**.

Я всячески избегал создания нового фреймворка в течение нескольких лет.
Сначала я пытался собрать все нужные функции, которые ныне есть в **FastAPI**, используя множество различных фреймворков, плагинов и инструментов.

Но в какой-то момент не осталось другого выбора, кроме как создать что-то, что предоставляло бы все эти функции сразу.
Взять самые лучшие идеи из предыдущих инструментов и, используя новые возможности Python (которых не было до версии 3.6, то есть подсказки типов), объединить их.

## Предшествующие инструменты

### <a href="https://www.djangoproject.com/" class="external-link" target="_blank">Django</a>

Это самый популярный Python-фреймворк, и он пользуется доверием.
Он используется для создания проектов типа Instagram.

Django довольно тесно связан с реляционными базами данных (такими как MySQL или PostgreSQL), потому использовать NoSQL базы данных (например, Couchbase, MongoDB, Cassandra и т.п.) в качестве основного хранилища данных - непросто.

Он был создан для генерации HTML-страниц на сервере, а не для создания API, используемых современными веб-интерфейсами (React, Vue.js, Angular и т.п.) или другими системами (например, <abbr title="Интернет вещей">IoT</abbr>) взаимодействующими с сервером.

### <a href="https://www.django-rest-framework.org/" class="external-link" target="_blank">Django REST Framework</a>

Фреймворк Django REST был создан, как гибкий инструментарий для создания веб-API на основе Django.

DRF использовался многими компаниями, включая Mozilla, Red Hat и Eventbrite.

Это был один из первых примеров **автоматического документирования API** и это была одна из первых идей, вдохновивших на создание **FastAPI**.

/// note | Заметка

Django REST Framework был создан Tom Christie.
Он же создал Starlette и Uvicorn, на которых основан **FastAPI**.

///

/// check | Идея для **FastAPI**

Должно быть автоматическое создание документации API с пользовательским веб-интерфейсом.

///

### <a href="https://flask.palletsprojects.com" class="external-link" target="_blank">Flask</a>

Flask - это "микрофреймворк", в нём нет интеграции с базами данных и многих других вещей, которые предустановлены в Django.

Его простота и гибкость дают широкие возможности, такие как использование баз данных NoSQL в качестве основной системы хранения данных.

Он очень прост, его изучение интуитивно понятно, хотя в некоторых местах документация довольно техническая.

Flask часто используется и для приложений, которым не нужна база данных, настройки прав доступа для пользователей и прочие из множества функций, предварительно встроенных в Django.
Хотя многие из этих функций могут быть добавлены с помощью плагинов.

Такое разделение на части и то, что это "микрофреймворк", который можно расширить, добавляя необходимые возможности, было ключевой особенностью, которую я хотел сохранить.

Простота Flask, показалась мне подходящей для создания API.
Но ещё нужно было найти "Django REST Framework" для Flask.

/// check | Идеи для **FastAPI**

Это будет микрофреймворк. К нему легко будет добавить необходимые инструменты и части.

Должна быть простая и лёгкая в использовании система маршрутизации запросов.

///

### <a href="https://requests.readthedocs.io" class="external-link" target="_blank">Requests</a>

На самом деле **FastAPI** не является альтернативой **Requests**.
Их область применения очень разная.

В принципе, можно использовать Requests *внутри* приложения FastAPI.

Но всё же я использовал в FastAPI некоторые идеи из Requests.

**Requests** - это библиотека для взаимодействия с API в качестве клиента,
в то время как **FastAPI** - это библиотека для *создания* API (то есть сервера).

Они, так или иначе, диаметрально противоположны и дополняют друг друга.

Requests имеет очень простой и понятный дизайн, очень прост в использовании и имеет разумные значения по умолчанию.
И в то же время он очень мощный и настраиваемый.

Вот почему на официальном сайте написано:

> Requests - один из самых загружаемых пакетов Python всех времен


Использовать его очень просто. Например, чтобы выполнить запрос `GET`, Вы бы написали:

```Python
response = requests.get("http://example.com/some/url")
```

Противоположная *операция пути* в FastAPI может выглядеть следующим образом:

```Python hl_lines="1"
@app.get("/some/url")
def read_url():
    return {"message": "Hello World"}
```

Глядите, как похоже `requests.get(...)` и `@app.get(...)`.

/// check | Идеи для **FastAPI**

* Должен быть простой и понятный API.
* Нужно использовать названия HTTP-методов (операций) для упрощения понимания происходящего.
* Должны быть разумные настройки по умолчанию и широкие возможности их кастомизации.

///

### <a href="https://swagger.io/" class="external-link" target="_blank">Swagger</a> / <a href="https://github.com/OAI/OpenAPI-Specification/" class="external-link" target="_blank">OpenAPI</a>

Главной функцией, которую я хотел унаследовать от Django REST Framework, была автоматическая документация API.

Но потом я обнаружил, что существует стандарт документирования API, использующий JSON (или YAML, расширение JSON) под названием Swagger.

И к нему уже был создан пользовательский веб-интерфейс.
Таким образом, возможность генерировать документацию Swagger для API позволила бы использовать этот интерфейс.

В какой-то момент Swagger был передан Linux Foundation и переименован в OpenAPI.

Вот почему, когда говорят о версии 2.0, обычно говорят "Swagger", а для версии 3+ "OpenAPI".

/// check | Идеи для **FastAPI**

Использовать открытые стандарты для спецификаций API вместо самодельных схем.

Совместимость с основанными на стандартах пользовательскими интерфейсами:

* <a href="https://github.com/swagger-api/swagger-ui" class="external-link" target="_blank">Swagger UI</a>
* <a href="https://github.com/Rebilly/ReDoc" class="external-link" target="_blank">ReDoc</a>

Они были выбраны за популярность и стабильность.
Но сделав беглый поиск, Вы можете найти десятки альтернативных пользовательских интерфейсов для OpenAPI, которые Вы можете использовать с **FastAPI**.

///

### REST фреймворки для Flask

Существует несколько REST фреймворков для Flask, но потратив время и усилия на их изучение, я обнаружил, что многие из них не обновляются или заброшены и имеют нерешённые проблемы из-за которых они непригодны к использованию.

### <a href="https://marshmallow.readthedocs.io/en/stable/" class="external-link" target="_blank">Marshmallow</a>

Одной из основных функций, необходимых системам API, является "<abbr title="также называют маршаллингом или преобразованием">сериализация</abbr>" данных, то есть преобразование данных из кода (Python) во что-то, что может быть отправлено по сети.
Например, превращение объекта содержащего данные из базы данных в объект JSON, конвертация объекта `datetime` в строку и т.п.

Еще одна важная функция, необходимая API — проверка данных, позволяющая убедиться, что данные действительны и соответствуют заданным параметрам.
Как пример, можно указать, что ожидаются данные типа `int`, а не какая-то произвольная строка.
Это особенно полезно для входящих данных.

Без системы проверки данных Вам пришлось бы прописывать все проверки вручную.

Именно для обеспечения этих функций и была создана Marshmallow.
Это отличная библиотека и я много раз пользовался ею раньше.

Но она была создана до того, как появились подсказки типов Python.
Итак, чтобы определить каждую <abbr title="Формат данных">схему</abbr>,
Вам нужно использовать определенные утилиты и классы, предоставляемые Marshmallow.

/// check | Идея для **FastAPI**

Использовать код программы для автоматического создания "схем", определяющих типы данных и их проверку.

///

### <a href="https://webargs.readthedocs.io/en/latest/" class="external-link" target="_blank">Webargs</a>

Другая немаловажная функция API - <abbr title="чтение и преобразование данных в объекты Python">парсинг</abbr> данных из входящих запросов.

Webargs - это инструмент, который был создан для этого и поддерживает несколько фреймворков, включая Flask.

Для проверки данных он использует Marshmallow и создан теми же авторами.

Это превосходный инструмент и я тоже часто пользовался им до **FastAPI**.

/// info | Информация

Webargs бы создан разработчиками Marshmallow.

///

/// check | Идея для **FastAPI**

Должна быть автоматическая проверка входных данных.

///

### <a href="https://apispec.readthedocs.io/en/stable/" class="external-link" target="_blank">APISpec</a>

Marshmallow и Webargs осуществляют проверку, анализ и сериализацию данных как плагины.

Но документации API всё ещё не было. Тогда был создан APISpec.

Это плагин для множества фреймворков, в том числе и для Starlette.

Он работает так - Вы записываете определение схем, используя формат YAML, внутри докстринга каждой функции, обрабатывающей маршрут.

Используя эти докстринги, он генерирует схему OpenAPI.

Так это работает для Flask, Starlette, Responder и т.п.

Но теперь у нас возникает новая проблема - наличие постороннего микро-синтаксиса внутри кода Python (большие YAML).

Редактор кода не особо может помочь в такой парадигме.
А изменив какие-то параметры или схемы для Marshmallow можно забыть отредактировать докстринг с YAML и сгенерированная схема становится недействительной.

/// info | Информация

APISpec тоже был создан авторами Marshmallow.

///

/// check | Идея для **FastAPI**

Необходима поддержка открытого стандарта для API - OpenAPI.

///

### <a href="https://flask-apispec.readthedocs.io/en/latest/" class="external-link" target="_blank">Flask-apispec</a>

Это плагин для Flask, который связан с Webargs, Marshmallow и APISpec.

Он получает информацию от Webargs и Marshmallow, а затем использует APISpec для автоматического создания схемы OpenAPI.

Это отличный, но крайне недооценённый инструмент.
Он должен быть более популярен, чем многие плагины для Flask.
Возможно, это связано с тем, что его документация слишком скудна и абстрактна.

Он избавил от необходимости писать чужеродный синтаксис YAML внутри докстрингов.

Такое сочетание Flask, Flask-apispec, Marshmallow и Webargs было моим любимым стеком при построении бэкенда до появления **FastAPI**.

Использование этого стека привело к созданию нескольких генераторов проектов. Я и некоторые другие команды до сих пор используем их:

* <a href="https://github.com/tiangolo/full-stack" class="external-link" target="_blank">https://github.com/tiangolo/full-stack</a>
* <a href="https://github.com/tiangolo/full-stack-flask-couchbase" class="external-link" target="_blank">https://github.com/tiangolo/full-stack-flask-couchbase</a>
* <a href="https://github.com/tiangolo/full-stack-flask-couchdb" class="external-link" target="_blank">https://github.com/tiangolo/full-stack-flask-couchdb</a>

Эти генераторы проектов также стали основой для [Генераторов проектов с **FastAPI**](project-generation.md){.internal-link target=_blank}.

/// info | Информация

Как ни странно, но Flask-apispec тоже создан авторами Marshmallow.

///

/// check | Идея для **FastAPI**

Схема OpenAPI должна создаваться автоматически и использовать тот же код, который осуществляет сериализацию и проверку данных.

///

### <a href="https://nestjs.com/" class="external-link" target="_blank">NestJS</a> (и <a href="https://angular.io/" class="external-link" target="_blank">Angular</a>)

Здесь даже не используется Python. NestJS - этот фреймворк написанный на JavaScript (TypeScript), основанный на NodeJS и вдохновлённый Angular.

Он позволяет получить нечто похожее на то, что можно сделать с помощью Flask-apispec.

В него встроена система внедрения зависимостей, ещё одна идея взятая от Angular.
Однако требуется предварительная регистрация "внедрений" (как и во всех других известных мне системах внедрения зависимостей), что увеличивает количество и повторяемость кода.

Так как параметры в нём описываются с помощью типов TypeScript (аналогично подсказкам типов в Python), поддержка редактора работает довольно хорошо.

Но поскольку данные из TypeScript не сохраняются после компиляции в JavaScript, он не может полагаться на подсказки типов для определения проверки данных, сериализации и документации.
Из-за этого и некоторых дизайнерских решений, для валидации, сериализации и автоматической генерации схем, приходится во многих местах добавлять декораторы.
Таким образом, это становится довольно многословным.

Кроме того, он не очень хорошо справляется с вложенными моделями.
Если в запросе имеется объект JSON, внутренние поля которого, в свою очередь, являются вложенными объектами JSON, это не может быть должным образом задокументировано и проверено.

/// check | Идеи для **FastAPI**

Нужно использовать подсказки типов, чтоб воспользоваться поддержкой редактора кода.

Нужна мощная система внедрения зависимостей. Необходим способ для уменьшения повторов кода.

///

### <a href="https://sanic.readthedocs.io/en/latest/" class="external-link" target="_blank">Sanic</a>

Sanic был одним из первых чрезвычайно быстрых Python-фреймворков основанных на `asyncio`.
Он был сделан очень похожим на Flask.

/// note | Технические детали

В нём использован <a href="https://github.com/MagicStack/uvloop" class="external-link" target="_blank">`uvloop`</a> вместо стандартного цикла событий `asyncio`, что и сделало его таким быстрым.

Он явно вдохновил создателей Uvicorn и Starlette, которые в настоящее время быстрее Sanic в открытых бенчмарках.

///

/// check | Идеи для **FastAPI**

Должна быть сумасшедшая производительность.

Для этого **FastAPI** основан на Starlette, самом быстром из доступных фреймворков (по замерам незаинтересованных лиц).

///

### <a href="https://falconframework.org/" class="external-link" target="_blank">Falcon</a>

Falcon - ещё один высокопроизводительный Python-фреймворк.
В нём минимум функций и он создан, чтоб быть основой для других фреймворков, например, Hug.

Функции в нём получают два параметра - "запрос к серверу" и "ответ сервера".
Затем Вы "читаете" часть запроса и "пишите" часть ответа.
Из-за такой конструкции невозможно объявить параметры запроса и тела сообщения со стандартными подсказками типов Python в качестве параметров функции.

Таким образом, и валидацию данных, и их сериализацию, и документацию нужно прописывать вручную.
Либо эти функции должны быть встроены во фреймворк, сконструированный поверх Falcon, как в Hug.
Такая же особенность присутствует и в других фреймворках, вдохновлённых идеей Falcon, использовать только один объект запроса и один объект ответа.

/// check | Идея для **FastAPI**

Найдите способы добиться отличной производительности.

Объявлять параметры `ответа сервера` в функциях, как в Hug.

Хотя в FastAPI это необязательно и используется в основном для установки заголовков, куки и альтернативных кодов состояния.

///

### <a href="https://moltenframework.com/" class="external-link" target="_blank">Molten</a>

Molten мне попался на начальной стадии написания **FastAPI**. В нём были похожие идеи:

* Использование подсказок типов.
* Валидация и документация исходя из этих подсказок.
* Система внедрения зависимостей.

В нём не используются сторонние библиотеки (такие, как Pydantic) для валидации, сериализации и документации.
Поэтому переиспользовать эти определения типов непросто.

Также требуется более подробная конфигурация и используется стандарт WSGI, который не предназначен для использования с высокопроизводительными инструментами, такими как Uvicorn, Starlette и Sanic, в отличие от ASGI.

Его система внедрения зависимостей требует предварительной регистрации, и зависимости определяются, как объявления типов.
Из-за этого невозможно объявить более одного "компонента" (зависимости), который предоставляет определенный тип.

Маршруты объявляются в единственном месте с использованием функций, объявленных в других местах (вместо использования декораторов, в которые могут быть обёрнуты функции, обрабатывающие конкретные ресурсы).
Это больше похоже на Django, чем на Flask и Starlette.
Он разделяет в коде вещи, которые довольно тесно связаны.

/// check | Идея для **FastAPI**

Определить дополнительные проверки типов данных, используя значения атрибутов модели "по умолчанию".
Это улучшает помощь редактора и раньше это не было доступно в Pydantic.

Фактически это подтолкнуло на обновление Pydantic для поддержки одинакового стиля проверок (теперь этот функционал уже доступен в Pydantic).

///

### <a href="https://github.com/hugapi/hug" class="external-link" target="_blank">Hug</a>

Hug был одним из первых фреймворков, реализовавших объявление параметров API с использованием подсказок типов Python.
Эта отличная идея была использована и другими инструментами.

При объявлении параметров вместо стандартных типов Python использовались собственные типы, но всё же это был огромный шаг вперед.

Это также был один из первых фреймворков, генерировавших полную API-схему в формате JSON.

Данная схема не придерживалась стандартов вроде OpenAPI и JSON Schema.
Поэтому было бы непросто совместить её с другими инструментами, такими как Swagger UI.
Но опять же, это была очень инновационная идея.

Ещё у него есть интересная и необычная функция: используя один и тот же фреймворк можно создавать и API, и <abbr title="Интерфейс командной строки">CLI</abbr>.

Поскольку он основан на WSGI, старом стандарте для синхронных веб-фреймворков, он не может работать с веб-сокетами и другими модными штуками, но всё равно обладает высокой производительностью.

/// info | Информация

Hug создан Timothy Crosley, автором <a href="https://github.com/timothycrosley/isort" class="external-link" target="_blank">`isort`</a>, отличного инструмента для автоматической сортировки импортов в Python-файлах.

///

/// check | Идеи для **FastAPI**

Hug повлиял на создание некоторых частей APIStar и был одним из инструментов, которые я счел наиболее многообещающими, наряду с APIStar.

Hug натолкнул на мысли использовать в **FastAPI** подсказки типов Python для автоматического создания схемы, определяющей API и его параметры.

Hug вдохновил **FastAPI** объявить параметр `ответа` в функциях для установки заголовков и куки.

///

### <a href="https://github.com/encode/apistar" class="external-link" target="_blank">APIStar</a> (<= 0.5)

Непосредственно перед тем, как принять решение о создании **FastAPI**, я обнаружил **APIStar**.
В нем было почти все, что я искал и у него был отличный дизайн.

Это была одна из первых реализаций фреймворка, использующего подсказки типов для объявления параметров и запросов, которые я когда-либо видел (до NestJS и Molten).
Я нашёл его примерно в то же время, что и Hug, но APIStar использовал стандарт OpenAPI.

В нём были автоматические проверка и сериализация данных и генерация схемы OpenAPI основанные на подсказках типов в нескольких местах.

При определении схемы тела сообщения не использовались подсказки типов, как в Pydantic, это больше похоже на Marshmallow, поэтому помощь редактора была недостаточно хорошей, но всё же APIStar был лучшим доступным вариантом.

На тот момент у него были лучшие показатели производительности (проигрывающие только Starlette).

Изначально у него не было автоматической документации API для веб-интерфейса, но я знал, что могу добавить к нему Swagger UI.

В APIStar была система внедрения зависимостей, которая тоже требовала предварительную регистрацию компонентов, как и ранее описанные инструменты.
Но, тем не менее, это была отличная штука.

Я не смог использовать его в полноценном проекте, так как были проблемы со встраиванием <abbr title="Авторизация и соответствующие допуски к операциям пути (эндпоинтам)">функций безопасности</abbr> в схему OpenAPI, из-за которых невозможно было встроить все функции, применяемые в генераторах проектов на основе Flask-apispec.
Я добавил в свой список задач создание пул-реквеста, добавляющего эту функциональность.

В дальнейшем фокус проекта сместился.

Это больше не был API-фреймворк, так как автор сосредоточился на Starlette.

Ныне APIStar - это набор инструментов для проверки спецификаций OpenAPI.

/// info | Информация

APIStar был создан Tom Christie. Тот самый парень, который создал:

* Django REST Framework
* Starlette (на котором основан **FastAPI**)
* Uvicorn (используемый в Starlette и **FastAPI**)

///

/// check | Идеи для **FastAPI**

Воплощение.

Мне казалось блестящей идеей объявлять множество функций (проверка данных, сериализация, документация) с помощью одних и тех же типов Python, которые при этом обеспечивают ещё и помощь редактора кода.

После долгих поисков среди похожих друг на друга фреймворков и сравнения их различий, APIStar стал самым лучшим выбором.

Но APIStar перестал быть фреймворком для создания веб-сервера, зато появился Starlette, новая и лучшая основа для построения подобных систем.
Это была последняя капля, сподвигнувшая на создание **FastAPI**.

Я считаю **FastAPI** "духовным преемником" APIStar, улучившим его возможности благодаря урокам, извлечённым из всех упомянутых выше инструментов.

///

## Что используется в **FastAPI**

### <a href="https://docs.pydantic.dev/" class="external-link" target="_blank">Pydantic</a>

Pydantic - это библиотека для валидации данных, сериализации и документирования (используя JSON Schema), основываясь на подсказках типов Python, что делает его чрезвычайно интуитивным.

Его можно сравнить с Marshmallow, хотя в бенчмарках Pydantic быстрее, чем Marshmallow.
И он основан на тех же подсказках типов, которые отлично поддерживаются редакторами кода.

/// check | **FastAPI** использует Pydantic

Для проверки данных, сериализации данных и автоматической документации моделей (на основе JSON Schema).

Затем **FastAPI** берёт эти схемы JSON и помещает их в схему OpenAPI, не касаясь других вещей, которые он делает.

///

### <a href="https://www.starlette.io/" class="external-link" target="_blank">Starlette</a>

Starlette - это легковесный <abbr title="Новый стандарт построения асинхронных веб-сервисов Python">ASGI</abbr> фреймворк/набор инструментов, который идеален для построения высокопроизводительных асинхронных сервисов.

Starlette очень простой и  интуитивный.
Он разработан таким образом, чтобы быть легко расширяемым и иметь модульные компоненты.

В нём есть:

* Впечатляющая производительность.
* Поддержка веб-сокетов.
* Фоновые задачи.
* Обработка событий при старте и финише приложения.
* Тестовый клиент на основе HTTPX.
* Поддержка CORS, сжатие GZip, статические файлы, потоковая передача данных.
* Поддержка сессий и куки.
* 100% покрытие тестами.
* 100% аннотированный код.
* Несколько жёстких зависимостей.

В настоящее время Starlette показывает самую высокую скорость среди Python-фреймворков в тестовых замерах.
Быстрее только Uvicorn, который является сервером, а не фреймворком.

Starlette обеспечивает весь функционал микрофреймворка, но не предоставляет автоматическую валидацию данных, сериализацию и документацию.

**FastAPI** добавляет эти функции используя подсказки типов Python и Pydantic.
Ещё **FastAPI** добавляет систему внедрения зависимостей, утилиты безопасности, генерацию схемы OpenAPI и т.д.

/// note | Технические детали

ASGI - это новый "стандарт" разработанный участниками команды Django.
Он пока что не является "стандартом в Python" (то есть принятым PEP), но процесс принятия запущен.

Тем не менее он уже используется в качестве "стандарта" несколькими инструментами.
Это значительно улучшает совместимость, поскольку Вы можете переключиться с Uvicorn на любой другой ASGI-сервер (например, Daphne или Hypercorn) или Вы можете добавить ASGI-совместимые инструменты, такие как `python-socketio`.

///

/// check | **FastAPI** использует Starlette

В качестве ядра веб-сервиса для обработки запросов, добавив некоторые функции сверху.

Класс `FastAPI` наследуется напрямую от класса `Starlette`.

Таким образом, всё что Вы могли делать со Starlette, Вы можете делать с **FastAPI**, по сути это прокачанный Starlette.

///

### <a href="https://www.uvicorn.org/" class="external-link" target="_blank">Uvicorn</a>

Uvicorn - это молниеносный ASGI-сервер, построенный на uvloop и httptools.

Uvicorn является сервером, а не фреймворком.
Например, он не предоставляет инструментов для маршрутизации запросов по ресурсам.
Для этого нужна надстройка, такая как Starlette (или **FastAPI**).

Он рекомендуется в качестве сервера для Starlette и **FastAPI**.

/// check | **FastAPI** рекомендует его

Как основной сервер для запуска приложения **FastAPI**.

Вы можете объединить его с Gunicorn, чтобы иметь асинхронный многопроцессный сервер.

Узнать больше деталей можно в разделе [Развёртывание](deployment/index.md){.internal-link target=_blank}.

///

## Тестовые замеры и скорость

Чтобы понять, сравнить и увидеть разницу между Uvicorn, Starlette и FastAPI, ознакомьтесь с разделом [Тестовые замеры](benchmarks.md){.internal-link target=_blank}.
