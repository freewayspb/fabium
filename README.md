Front-end automated boilerplate
====================
Автоматизированный шаблон для быстрого старта front-end разработки

## Навигация
* [Для чего все это?](#Для-чего-все-это)
* [Что в мешке?](#Что-в-мешке)
    * [Сборщик проекта](#Сборщик-проекта)
    * [Автоматизированные задачи](#Автоматизированные-задачи)
    * [Файловая структура](#Файловая-структура)
* [Как все работает?](#Как-все-работает)
    - [Задачи](#Задачи)
    - [Группы задач](#Группы-задач)
* [Системные требования](#Системные-требования)
* [Установка](#Установка)
* [Проблемы](#Проблемы)
* [TODO](#TODO)

## Для чего все это?
Каждый раз, начиная новый проект, я ставил `gulp`, необходимые модули, настраивал, организовывал файловую структуру проекта, создавал необходимые первоначальные файлы. Я часто обращался к законченным проектам, чтобы взять те или иные файлы и настройки.
На это уходило много времени и я решил, что пора собрать один шаблон, который позволит быстро начать работу над проектом.

Я знаю про существование таких инструментов как [Yo](http://yeoman.io) и я знаю точно, что мне нужно, поэтому проще и быстрее было собрать свой шаблон.

## Что в мешке?

Шаблон я собирал с учетом своих потребностей и опыта, который накапливался на протяжении многих проектов. Поэтому, я не обещаю, что он может быть полезен вам в первоначальном виде, но его можно использовать как наглядный пример и дорабатывать под свои нужды.

### Сборщик проекта
Я считаю, что любая рутина должна быть автоматизирована. Раньше приходилось собирать вручную спрайты, минифицировать файлы, но сейчас появилось большое количество инструментов, которые помогают автоматизировать все это и сэкономить время и нервы разработчика.

В качестве сборщика проекта я выбрал `gulp`.
До этого я имел опыт работы с `grunt`, но мне не нравилось то, как медленно он компилировал файлы. Мне важно чтобы изменения в файлах происходили моментально.
А `Gulp` после `grunt-a` поражает своей скоростью.

### Автоматизированные задачи
- Компиляция [stylus](http://learnboost.github.io/stylus/)
- Компиляция [CoffeeScript](http://coffeescript.org/)
- Компиляция [Jade](http://jade-lang.com/)-шаблонов
- Сборка спрайтов и генерация стиля для них. Я писал [статью](http://habrahabr.ru/post/227945/) о том, как это все работает.
- Добавление вендорных префиксов к свойствам
- Минификация css и js
- Оптимизация картинок

### Файловая структура
Первый уровень проекта — папки `builder`, `src` и `built`.

`builder` — папка в которой лежит `gulpfile` с тасками для `gulp`, где будут лежать `node-модули` и откуда буду запускаться все команды в консоли.

В папке `src` лежат все исходные файлы проекта, а папка `built` создается и изменяется автоматически при выполнении команд `gulp-а`.

**Содержимое папки `src`**
- `assets`
    + `styles` — стили проекта
    + `images` — картинки проекта, включая `content` папку для картинок в контенте
    + `scripts` — скрипты делятся на 2 типа: сторонние библиотеки (`vendor`) и то, что было написано для этого проекта (`local`)
- `templates`
    + `pages` — шаблоны страниц
    + `blocks` — блоки из которых будут собираться страницы.


## Как все работает?
В `gulpfile.coffee` описаны таски, которые выполняют те или иные действия. Таски можно вызывать по отдельности и группами. Вызов группами — самый частый юзкейс.
Все `gulp`-плагины загружаются автоматически из `package.json` с помощью плагина `gulp-load-plugins`. Это позволяет уменьшить объем `gulpfile.coffee`.
`Gulp` в процессе работы берет файл из 1 папки, выполняет с ним необходимые операции и сохраняет в другой папке. Для удобства, все пути к файлам я вынес в переменные и храню их в файле `config.yml`.

### Задачи
* `sprite` — генерация спрайта на основе картинок, который лежат в папке `config.paths.src.sprites.images.all`
* `coffee` — компиляция `.сoffee` из папки `local`
* `vendor` — перенос скриптов в `built` папку
* `stylus` — компиляция `.styl`
* `images` — перенос картинок в `built` папку
* `jade` — компиляция `.jade`-шаблонов
* `autoprefixer` — добавление вендорных префиксов (настройки по-умолчанию)
* `scripts:min` — минификация скриптов
* `styles:min` — минификация стилей
* `images:min` — оптимизация картинок
* `watch` — отслеживание изменений в файлах и запуск необходимого таска

### Группы задач
* `default` - компилирует шаблоны, скрипты, стили, собирает спрайты. Но делает это всего 1 раз;
* `dev` — Включает в себя сам `default` и задачу `watch`, которая начинает отслеживать изменения в файлах и запускает необходимые таски по отдельности, в зависимости от файла, который изменился;
* `minify` — сжимает файлы и оптимизирует картинки, которые были созданы после работы `default`;
* `prod` — таск который запускается на сервере или перед заливкой на сервер. Он выполняет группу `default` и после этого запускает `minify`;


## Системные требования
* node.js
* gulp
* coffee-script — для запуска gulpfile.coffee. При желании можно перевести `.coffee` в `.js`

## Установка
Если у вас не установлен `node.js`, [скачайте](http://nodejs.org) и установите.
<br>
Вместе с `node.js` у вас установится пакетный менеджер `npm`.
<br>
Теперь установите `coffee-script` и `gulp`, введя в консоли команду `npm i -g coffee-script gulp` (понадобятся права администратора).
<br>
После этих шагов, можно приступать к установке зависимостей в самом проекте.
<br>
Скачайте и распакуйте архив с шаблоном, в консоли откройте папку `builder` и введите `npm i`. Пойдет процесс скачивания и установки всех перечисленных в `package.json` модулей. После завершения установки, можно смело напечатать `gulp` в консоли и отработает группа задач `default`

## Проблемы
При запуске `gulp` система может выдат ошибку о том, что не найден модуль `coffee-script/register`. Решение — установить переменную NODE_PATH

**Ссылки на решения**
* http://stackoverflow.com/questions/9587665/nodejs-cannot-find-installed-module-on-windows
* http://stackoverflow.com/questions/12594541/npm-global-install-cannot-find-module

## TODO
* Подключить миксины для stylus (http://kouto-swiss.io/, nib)
* конкатенация js
* base64
* JSHint
* bower
* https://github.com/jackfranklin/gulp-load-plugins
* https://github.com/sindresorhus/gulp-changed
* https://www.npmjs.org/package/gulp-accord
* https://www.npmjs.org/package/gulp-task-listing / https://www.npmjs.org/package/gulp-help
* Подготовить grunt-версию
* livereload?
