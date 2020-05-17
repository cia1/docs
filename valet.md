git fede701240df2213ba6956d3dd8090da6dc8bbe9

---

# Laravel Valet

- [Введение](#introduction)
    - [Valet или Homestead](#valet-or-homestead)
- [Установка](#installation)
    - [Обновление](#upgrading)
- [Обслуживание сайтов](#serving-sites)
    - [Команда "Park"](#the-park-command)
    - [Команда "Link"](#the-link-command)
    - [Защита сайтов при помощи TLS](#securing-sites)
- [Общий доступ к сайтам](#sharing-sites)
- [Переменные окружения для каждого сайта](#site-specific-environment-variables)
- [Пользовательские драйверы Valet](#custom-valet-drivers)
    - [Локальные драйверы](#local-drivers)
- [Другие команды Valet](#other-valet-commands)
- [Файлы и директории Valet](#valet-directories-and-files)

<a name="introduction"></a>
## Введение

Valet — среда для разработки в Laravel для минималистов, работающих на Mac. Без Vagrant, без файла `/etc/hosts`. Можно даже расшаривать сайты в общий доступ через локальные туннели. _Да, нам тоже это нравится._

Laravel Valet включает на вашем Mac фоновую автозагрузку [nginx](https://www.nginx.com/). Затем с помощью [DnsMasq](https://en.wikipedia.org/wiki/Dnsmasq), Valet настраивает прокси для всех запросов к домену `*.test` для переадресации их на сайты на вашей локальной машине.

Другими словами, это молниеносное окружение для разработки в Laravel, которое использует около 7 Мб RAM. Valet не является полной заменой для Vagrant или Homestead, но предоставляет отличную альтернативу, когда вам нужна база для гибкой настройки, максимальная скорость, или вы работаете на машине с ограниченным объёмом RAM.

Изначально Valet поддерживает, но не ограничивается только ими:

<style>
    #valet-support > ul {
        column-count: 3; -moz-column-count: 3; -webkit-column-count: 3;
        line-height: 1.9;
    }
</style>

<div class="content-list" markdown="1">
- [Laravel](https://laravel.com)
- [Lumen](https://lumen.laravel.com)
- [Bedrock](https://roots.io/bedrock/)
- [CakePHP 3](https://cakephp.org)
- [Concrete5](https://www.concrete5.org/)
- [Contao](https://contao.org/en/)
- [Craft](https://craftcms.com)
- [Drupal](https://www.drupal.org/)
- [ExpressionEngine](https://www.expressionengine.com/)
- [Jigsaw](https://jigsaw.tighten.co)
- [Joomla](https://www.joomla.org/)
- [Katana](https://github.com/themsaid/katana)
- [Kirby](https://getkirby.com/)
- [Magento](https://magento.com/)
- [OctoberCMS](https://octobercms.com/)
- [Sculpin](https://sculpin.io/)
- [Slim](https://www.slimframework.com)
- [Statamic](https://statamic.com)
- Статический HTML
- [Symfony](https://symfony.com)
- [WordPress](https://wordpress.org)
- [Zend](https://framework.zend.com)
</div>

Также вы можете дополнить Valet своими [собственными драйверами](#custom-valet-drivers).

<a name="valet-or-homestead"></a>
### Valet или Homestead

Как вы знаете, Laravel предлагает [Homestead](/docs/{{version}}/homestead) — другую локальную среду разработки. Valet и Homestead отличаются целевой аудиторией и подходом к локальной разработке. Homestead включает в себя целую виртуальную машину с Ubuntu и автоматической настройкой Nginx. Homestead — отличный выбор, если вам нужна полностью виртуальная среда разработки на Linux или на Windows / Linux.

Valet поддерживает только Mac и требует установки PHP и сервера базы данных непосредственно на вашу локальную машину. Это легко делается при помощи [Homebrew](https://brew.sh/) такими командами, как `brew install php` и `brew install mysql`. Valet обеспечивает молниеносную среду разработки с минимальным потреблением ресурсов, поэтому идеально подходит для разработчиков, использующих только PHP / MySQL и не нуждающихся в полностью виртуальной среде разработки.

И Valet, и Homestead являются отличным выбором для настройки среды разработки в Laravel. Выбор зависит от ваших личных предпочтений и потребностей вашей команды.

<a name="installation"></a>
## Установка

**Valet требует macOS и [Homebrew](https://brew.sh/). Перед установкой необходимо убедиться, что другие программы, такие как Apache и Nginx, не используют 80 порт вашей локальной машины.**

<div class="content-list" markdown="1">
- Установите или обновите [Homebrew](https://brew.sh/) до последней версии с помощью `brew update`.
- Установите PHP 7.4 с помощью Homebrew командой `brew install php`.
- Установите Valet через Composer при помощи команды `composer global require laravel/valet`. Убедитесь, что в вашей системной переменной "PATH" есть директория `~/.composer/vendor/bin`.
- Выполните команду `valet install`. Она установит и настроит Valet и DnsMasq, и пропишет демон Valet в автозагрузку.
</div>

Послу установки Valet, попробуйте запинговать любой домен `*.test` на своем терминале при помощи команды `ping foobar.test`. Если Valet установлен корректно, то вы увидите, что этот домен соответствует `127.0.0.1`.

Valet автоматически запустит своего демона при запуске ОС. После начальной установки Valet больше не потребуется выполнять `valet start` или `valet install`.

#### Использование другого домена

По умолчанию Valet использует для ваших проектов домен верхнего уровня `.test`. Если вы хотите использовать другой домен, используйте команду `valet domain tld-name`.

Например, чтобы использовать `.app` вместо `.test`, выполните `valet domain app`, и Valet начнет атоматически использовать ваши проекты на `*.app`.

#### База данных

Если вам нужна база данных, попробуйте MySQL с помощью команды `brew install mysql@5.7`. После установки MySQL, вы можете запустить её командой `brew services start mysql@5.7`. Затем вы можете подключиться к БД на `127.0.0.1`, используя имя пользователя `root` и пароль - пустая строка.

#### Версии PHP

Valet позволяет переключать версии PHP командой `valet use php@version`. Valet установить указанную версию PHP при помощи Brew, если она ещё не установлена:

    valet use php@7.2

    valet use php

> {note} Valet не предоставляет одновременный доступ к нескольким версиям PHP, даже если у вас установлено несколько версий.

#### Сброс установки

Если у вас возникли проблемы при запуске установленного Valet, выполните команду `composer global update` сразу после `valet install` чтобы сбросить установку - это может помочь решить многие проблемы. В редких случаях может потребоваться "жёсткий сброс", для этого выполните `valet uninstall --force` сразу после `valet install`.

<a name="upgrading"></a>
### Обновление

Вы можете обновить установленный Valet терминальной командой `composer global update`. После обновления рекомендуется выполнить команду `valet install`, чтобы Valet сделал дополнительные обновления ваших конфигурационных файлов при необходимости.

<a name="serving-sites"></a>
## Обслуживание сайтов

После установки Valet можно начать обслуживать сайты. Valet предоставляет две команды для помощи в обслуживании Laravel-сайтов: `park` и `link`.

<a name="the-park-command"></a>
#### Команда `park`

<div class="content-list" markdown="1">
- Создайте новый каталог на своём Mac с помощью команды `mkdir ~/Sites`. Далее, перейдите к ней  `cd ~/Sites` и выполните `valet park`. Эта команда зарегистрирует текущий каталог как путь, по которому Valet будет искать сайты.
- Теперь создайте новый Laravel-сайт в этом каталоге: `laravel new blog`.
- Откройте `http://blog.test` в своём браузере.
</div>

**Вот и всё.** Теперь все проекты, которые вы разместите в каталоге "парковки", будут обслуживаться автоматически с адресами в соответствии с названиями их папок в конвенции `http://folder-name.test`.

<a name="the-link-command"></a>
#### Команда `link`

Для обслуживания Laravel-сайтов также можно использовать команду `link`. Эта команда полезна, когда вы хотите обслуживать один сайт в каталоге, а не весь каталог.

<div class="content-list" markdown="1">
- Для использования команды перейдите к одному из своих проектов и выполните `valet link app-name` в терминале. Valet создаст в `~/.config/valet/Sites` символьную ссылку на ваш текущий каталог.
- После запуска команды `link` вы можете перейти на сайт `http://app-name.test` в своём браузере.
</div>

Для просмотра списка всех привязанных каталогов выполните команд `valet links`. Для удаления символьной ссылки используйте `valet unlink app-name`.

> {tip} Вы можете использовать команду `valet link` для обслуживания одного проекта из нескольких (под)доменов. Чтобы добавить поддомен или другой домен в свой проект, выполните `valet link subdomain.app-name` в папке проекта.

<a name="securing-sites"></a>
#### Защита сайтов при помощи TLS

По умолчанию Valet обслуживает сайты через чистый HTTP. Но при желании вы можете включить шифрование TLS, используя HTTP/2, с помощью команды `secure`. Например, если Valet обслуживает ваш сайт на домене `laravel.test`, то для его защиты вам надо выполнить команду:

    valet secure laravel

Чтобы отключить защиту и перевести трафик обратно на чистый HTTP, используйте команду `unsecure`. Как и команда `secure`, эта команда принимает имя хоста, для которого необходимо отключить защиту:

    valet unsecure laravel

<a name="sharing-sites"></a>
## Общий доступ к сайтам

Valet включает команду для открытия доступа к вашим локальным сайтам для всех, предоставляя простой путь для тестирования вашего сайта на мобильном устройстве или демонстрации вашей команде или клиентам. Кроме Valet не нужно никакое ПО.

### Общий доступ к сайтам при помощи Ngrok

Чтобы предоставить доступ к сайту, перейдите в директорий этого сайта в терминале и выполните команду `valet share`. Общедоступный URL будет скопирован в ваш буфер обмена и вы можете вставить его в адресную строку вашего браузера или браузера вашей команды.

Чтобы отменить общий доступ, нажмите `Control + C`, чтобы остановить процесс.

> {tip} Вы можете передать дополнительные параметры в команде, например `valet share --region=eu`. Для получения дополнительной информации, обратитесь к [документации ngrok](https://ngrok.com/docs).

### Общий доступ к сайтам в локальной сети

Valet по умолчанию ограничивает входящий трафик внутренним интерфейсом `127.0.0.1`. Таким образом ваша машина разработки не подвергается угрозам безопасности из интернета.

Если вы хотите предоставить другим устройствам вашей локальной сети доступ к сайтам Valet через IP-адрес вашего компьютера (например `192.168.1.10/app-name.test`), вам необходимо вручную отредактировать файлы конфигурации Nginx для соответствующих сайтов, сняв ограничение директивы `listen`, удалив префикс `127.0.0.1:` для портов 80 и 443.

Если вы не выполняли команду `valet secure` для проекта, вы можете открыть сетевой доступ ко всем не-HTTPS сайтам, отредактировав файл `/usr/local/etc/nginx/valet/valet.conf`. Если же требуется предоставить доступ к сайту через HTTPS (вы выполняли команду `valet secure` для этого сайта), вы должны отредактировать файл `~/.config/valet/Nginx/app-name.test`.

После обновления конфигурации Nginx, выполните команду `valet restart` чтобы изменения вступили в силу.

<a name="site-specific-environment-variables"></a>
## Индивидуальные переменные окружения

Некоторые приложения, использующие другие фреймворки, могут зависеть от переменных окружения сервера, но не предоставлять возможности их настройки в рамках приложения. Valet позволяет настраивать переменные окружения индивидуально для каждого сайта, добавляя их в файл `.valet-env.php`, расположенный в корневой директории вашего проекта. Эти переменные будут добавлены к глобальному массиву `$_SERVER`:

    <?php

    // Установить $_SERVER['key'] в значение "value" для сайта foo.test...
    return [
        'foo' => [
            'key' => 'value',
        ],
    ];

    // Установить $_SERVER['key'] в значение "value" для всех сайтов...
    return [
        '*' => [
            'key' => 'value',
        ],
    ];

<a name="custom-valet-drivers"></a>
## Пользовательские драйверы Valet

Вы можете написать свой собственный «драйвер» Valet для обслуживания PHP-приложений, работающих на другом фреймворке или CMS, изначально не поддерживаемых Valet. При установке Valet создаётся папка `~/.config/valet/Drivers`, содержащая файл `SampleValetDriver.php`. В этом файле находится пример реализации драйвера для демонстрации. Для написания драйвера необходимо реализовать всего три метода: `serves`, `isStaticFile` и `frontControllerPath`.

Все три метода принимают в качестве аргументов `$sitePath`, `$siteName` и `$uri`. Параметр `$sitePath` — полный путь к сайту на вашей машине, например, `/Users/Lisa/Sites/my-project`. Параметр `$siteName` — часть домена "хост" / "имя сайта" (`my-project`). Параметр `$uri` — URI входящих запросов (`/foo/bar`).

Когда вы завершили написание своего драйвера Valet, поместите его в директорию `~/.config/valet/Drivers`, используя принцип именования `FrameworkValetDriver.php`. Например, если вы написали драйвер для WordPress, то имя файла должно быть `WordPressValetDriver.php`.

Давайте рассмотрим примеры реализации каждого из этих методов.

#### Метод `serves`

Метод `serves` должен возвращать `true`, если ваш драйвер должен обрабатывать входящие запросы. Иначе метод должен возвращать `false`. В этом методе вы должны попытаться определить, содержит ли данный `$sitePath` проект того типа, который вы хотите обслуживать.

Например, давайте предположим, что мы пишем `WordPressValetDriver`. Наш метод `serves` должен выглядеть примерно так:

    /**
     * Определить, обслуживает ли драйвер запрос.
     *
     * @param  string  $sitePath
     * @param  string  $siteName
     * @param  string  $uri
     * @return bool
     */
    public function serves($sitePath, $siteName, $uri)
    {
        return is_dir($sitePath.'/wp-admin');
    }

#### Метод `isStaticFile`

Метод `isStaticFile` должен определять, является ли входящий запрос запросом к статическому файлу, такому как изображение или таблица стилей. Если файл статический, метод должен вернуть полный путь к этому файлу на диске. Если входящий запрос не к статическому файлу, метод должен вернуть `false`:

    /**
     * Определить, является ли входящий запрос запросом к статическому файлу.
     *
     * @param  string  $sitePath
     * @param  string  $siteName
     * @param  string  $uri
     * @return string|false
     */
    public function isStaticFile($sitePath, $siteName, $uri)
    {
        if (file_exists($staticFilePath = $sitePath.'/public/'.$uri)) {
            return $staticFilePath;
        }

        return false;
    }

> {note} Метод `isStaticFile` будет вызван, только если метод `serves` возвращает `true` для входящего запроса, и значение URI запроса не равно `/`.

#### Метод `frontControllerPath`

Метод `frontControllerPath` должен вернуть полный путь к "первичному контроллеру" вашего приложения, которым обычно является ваш файл "index.php" или его эквивалент:

    /**
     * Получение полного пути к первичному контроллеру приложения.
     *
     * @param  string  $sitePath
     * @param  string  $siteName
     * @param  string  $uri
     * @return string
     */
    public function frontControllerPath($sitePath, $siteName, $uri)
    {
        return $sitePath.'/public/index.php';
    }

<a name="local-drivers"></a>
### Локальные драйверы

Если вы хотите определить пользовательский драйвер Valet для одного приложения, создайте `LocalValetDriver.php` в корневой директории приложения. Ваш драйвер может расширить базовый класс `ValetDriver` или расширить существующий определенный драйвер приложения, такой как `LaravelValetDriver`:

    class LocalValetDriver extends LaravelValetDriver
    {
        /**
         * Определить, обслуживает ли драйвер запрос.
         *
         * @param  string  $sitePath
         * @param  string  $siteName
         * @param  string  $uri
         * @return bool
         */
        public function serves($sitePath, $siteName, $uri)
        {
            return true;
        }

        /**
         * Get the fully resolved path to the application's front controller.
         *
         * @param  string  $sitePath
         * @param  string  $siteName
         * @param  string  $uri
         * @return string
         */
        public function frontControllerPath($sitePath, $siteName, $uri)
        {
            return $sitePath.'/public_html/index.php';
        }
    }

<a name="other-valet-commands"></a>
## Другие команды Valet

Команда  | Описание
------------- | -------------
`valet forget` | Выполните эту команду в каталоге "парковки" для удаления его из списка каталогов парковки.
`valet log` | Отображение списка логов, созданных службами Valet.
`valet paths` | Просмотр всех путей "парковки".
`valet restart` | Перезапуск демона Valet.
`valet start` | Запуск демона Valet.
`valet stop` | Остановка демона Valet.
`valet trust` | Добавляет файлы sudoers для Brew и Valet, позволяя Valet исполнять команды без запроса пароля.
`valet uninstall` | Полное удаление демона Valet.

<a name="valet-directories-and-files"></a>
## Директории и файлы Valet

Следующая информация о директориях и файлах Valet может быть полезна при решении проблем вашего окружения:

Файл / Путь | Описание
--------- | -----------
`~/.config/valet/` | Содержит всю конфигурацию Valet. Вы можете захотеть иметь резервные копии этой директории.
`~/.config/valet/dnsmasq.d/` | Содержит конфигурацию DNSMasq.
`~/.config/valet/Drivers/` | Содержит драйверы Valet.
`~/.config/valet/Extensions/` | Содержит расширения и подолнительные команды Valet.
`~/.config/valet/Nginx/` | Содержит все генерируемые Valet-ом конфигурации сайтов Nginx. Эти файлы пересоздаются при выполнении команд `install`, `secure` и `tld`.
`~/.config/valet/Sites/` | Содержит все символьные ссылки на связанные проекты.
`~/.config/valet/config.json` | Основной конфигурационный файл Valet
`~/.config/valet/valet.sock` | Сокет PHP-FPM, используемый в конфигурации Nginx. Этот файл существует только если PHP корректно запущен.
`~/.config/valet/Log/fpm-php.www.log` | Лог пользовательских ошибок PHP.
`~/.config/valet/Log/nginx-error.log` | Лог пользовательских ошибок Nginx.
`/usr/local/var/log/php-fpm.log` | Системный лог ошибок PHP-FPM.
`/usr/local/var/log/nginx` | Содержит лог доступа и ошибок Nginx.
`/usr/local/etc/php/X.X/conf.d` | Содержит файлы `*.ini` для разных версий PHP.
`/usr/local/etc/php/X.X/php-fpm.d/valet-fpm.conf` | ФАйл конфигурации пула PHP-FPM.
`~/.composer/vendor/laravel/valet/cli/stubs/secure.valet.conf` | Конфигурация Nginx по умолчанию, используемая для генерации сертификатов сайтов.
