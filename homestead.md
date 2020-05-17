git 6f6a8518c00512753c41623e0029985e80579d1a

---

# Laravel Homestead

- [Введение](#introduction)
- [Установка и настройка](#installation-and-setup)
    - [Первые шаги](#first-steps)
    - [Настройка Homestead](#configuring-homestead)
    - [Запуск Vagrant Box](#launching-the-vagrant-box)
    - [Установка для проекта](#per-project-installation)
    - [Установка дополнительных расширений](#installing-optional-features)
    - [Псевдонимы](#aliases)
- [Повседневное использование](#daily-usage)
    - [Глобальный доступ к Homestead](#accessing-homestead-globally)
    - [Подключение через SSH](#connecting-via-ssh)
    - [Подключение к базам данных](#connecting-to-databases)
    - [Резервные копирование баз данных](#database-backups)
    - [Снимки баз данных](#database-snapshots)
    - [Добавление дополнительных сайтов](#adding-additional-sites)
    - [Переменные окружения](#environment-variables)
    - [Настройка расписания Cron](#configuring-cron-schedules)
    - [Настройка Mailhog](#configuring-mailhog)
    - [Настройка Minio](#configuring-minio)
    - [Порты](#ports)
    - [овместное использование вашей среды](#sharing-your-environment)
    - [Несколько версий PHP](#multiple-php-versions)
    - [Веб-серверы](#web-servers)
    - [Почта](#mail)
    - [Отладка и профилирование](#debugging-and-profiling)
    - [Отладка Веб-запросов при помощи Xdebug](#debugging-web-requests)
    - [Отладка консольных приложений](#debugging-cli-applications)
    - [Профилирование приложений при помощи Blackfire](#profiling-applications-with-blackfire)
- [Сетевые интерфейсы](#network-interfaces)
- [Расширения Homestead](#extending-homestead)
- [Обновление Homestead](#updating-homestead)
- [Специальные настройки провайдера](#provider-specific-settings)
    - [VirtualBox](#provider-specific-virtualbox)

<a name="introduction"></a>
## Введение

Laravel стремится преобразить процесс разработки на PHP, это относится и к локальной среде разработки. [Vagrant](https://www.vagrantup.com) обеспечивает простой, элегантный способ настройки и управления виртуальными машинами.

Laravel Homestead — официальный предустановленный Vagrant-бокс, который предоставляет вам замечательную среду разработки без необходимости установки PHP, веб-сервера и любого другого серверного программного обеспечения на ваш компьютер. Можно больше не беспокоиться о том, что ваша операционная система засоряется! Vagrant-боксы очень удобны. Если что-то пошло не так, вы можете уничтожить и пересоздать бокс в считанные минуты!

Homestead запускается на ОС Windows, Mac и Linux, и включает в себя Nginx, PHP, MySQL, Postgres, Redis, Memcached, Node и все другие полезные штуки, которые вам понадобятся для разработки удивительных Laravel-приложений.

> {note} Если вы используете Windows, возможно, вам необходимо включить аппаратную виртуализацию (VT-x). Она обычно включается через BIOS. Если вы используете Hyper-V на UEFI-системе, вам может понадобиться отключить Hyper-V, для доступа к VT-x.

<a name="included-software"></a>
### Включённое ПО

<style>
    #software-list > ul {
        column-count: 3; -moz-column-count: 3; -webkit-column-count: 3;
        column-gap: 5em; -moz-column-gap: 5em; -webkit-column-gap: 5em;
        line-height: 1.9;
    }
</style>

<div id="software-list" markdown="1">
- Ubuntu 18.04
- Git
- PHP 7.4
- PHP 7.3
- PHP 7.2
- PHP 7.1
- PHP 7.0
- PHP 5.6
- Nginx
- MySQL
- lmm для снимков MySQL и MariaDB
- Sqlite3
- PostgreSQL
- Composer
- Node (с Yarn, Bower, Grunt, and Gulp)
- Redis
- Memcached
- Beanstalkd
- Mailhog
- avahi
- ngrok
- Xdebug
- XHProf / Tideways / XHGui
- wp-cli
</div>

<a name="optional-software"></a>
### Дополнительное ПО (по желанию)

<style>
    #software-list > ul {
        column-count: 3; -moz-column-count: 3; -webkit-column-count: 3;
        column-gap: 5em; -moz-column-gap: 5em; -webkit-column-gap: 5em;
        line-height: 1.9;
    }
</style>

<div id="software-list" markdown="1">
- Apache
- Blackfire
- Cassandra
- Chronograf
- CouchDB
- Crystal & Lucky Framework
- Docker
- Elasticsearch
- Gearman
- Go
- Grafana
- InfluxDB
- MariaDB
- MinIO
- MongoDB
- MySQL 8
- Neo4j
- Oh My Zsh
- Open Resty
- PM2
- Python
- RabbitMQ
- Solr
- Webdriver & Laravel Dusk Utilities
</div>

<a name="installation-and-setup"></a>
## Установка и настройка

<a name="first-steps"></a>
### Первые шаги

Прежде чем запустить среду Homestead, вы должны установить [VirtualBox 6.x](https://www.virtualbox.org/wiki/Downloads), [VMWare](https://www.vmware.com), [Parallels](https://www.parallels.com/products/desktop/) или [Hyper-V](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v), а также [Vagrant](https://www.vagrantup.com/downloads.html). Все эти программные пакеты предоставляют простые в использовании визуальные инсталляторы для всех популярных операционных систем.

Для использования VMWare вам необходимо приобрести и VMware Fusion/Workstation, и [плагин VMware Vagrant](https://www.vagrantup.com/vmware). Хотя он и платный, зато VMware изначально обеспечивает большую скорость работы общих папок.

Для использования провайдера Parallels вам необходимо установить [плагин Parallels Vagrant](https://github.com/Parallels/vagrant-parallels). Он бесплатный.

Из-за [ограничения Vagrant](https://www.vagrantup.com/docs/hyperv/limitations.html), провайдер Hyper-V игнорирует все сетевые настройки.

#### Установка Homestead Vagrant Box

После установки VirtualBox / VMware и Vagrant вы должны добавить бокс `laravel/homestead` в ваш Vagrant, используя следующую команду в вашем терминале. Скачивание образа может занять несколько минут в зависимости от скорости вашего Интернет-подключения:

    vagrant box add laravel/homestead

Если выполнение команды завершится неудачно, проверьте, что у вас установлена свежая версия Vagrant.

> {note} Homestead периодически выпускает "alpha" / "beta" версии для тестирования, что может помешать команде `vagrant box add`. Если у вас возникли проблемы с командой `vagrant box add`, попробуйте выполнить команду `vagrant up`, тогда правильный бокс будет скачан когда Vargant попытается запустить виртуальную машину.

#### Установка Homestead

Вы можете установить Homestead клонировав репозиторий на вашу гостевую машину. Клонируйте репозиторий в папку `Homestead` в директорию "home", потому что коробка Homestead станет хостом всех ваших Laravel-проектов:

    git clone https://github.com/laravel/homestead.git ~/Homestead

Вы должны проверить меченную версию Homestead, так как ветка `master` не всегда может быть стабильна. Самую свежую стабильную версию можно найти на [странице релиза в GitHub](https://github.com/laravel/homestead/releases). Или же вы можете переключиться в ветку `release`, которая всегда содержит последний стабильный релиз:

    cd ~/Homestead

    git checkout release

После клонирования репозитория Homestead выполните в этой директории команду `bash init.sh`, чтобы создать конфиг `Homestead.yaml`. Файл `Homestead.yaml` будет помещен в директорию Homestead:

    // Mac / Linux...
    bash init.sh

    // Windows...
    init.bat

<a name="configuring-homestead"></a>
### Настройка Homestead

#### Настройка провайдера

Параметр `provider` в вашем файле `Homestead.yaml` указывает на то, какой Vagrant-провайдер следует использовать: `virtualbox`, `vmware_fusion`, `vmware_workstation`, `parallels` или `hyperv`. Вы можете выбрать тот, который предпочитаете:

    provider: virtualbox

#### Настройка общих папок

В свойстве `folders` файла `Homestead.yaml` перечислены все папки, которые вы хотите расшарить для вашей среды Homestead. Поскольку файлы в этих папках будут меняться, они будут синхронизироваться с вашей локальной машиной и средой Homestead. Вы можете настроить столько папок, сколько вам необходимо:

    folders:
        - map: ~/code/project1
          to: /home/vagrant/project1

> {note} Пользователи Windows не должны использовать `~/`, вместо этого нужно использовать полный путь к проекту, например `C:\Users\user\Code\project1`.

Вы всегда должны отдельно сопоставлять каждый проект к своей директории, вместо того, чтобы сопоставить ("слинковать") весь директорий `~/code`. Когда вы сопоставляете директории, виртуальная машина должна отслеживать все дисковые операции ввода-вывода для *каждого* файла в директории. Это может привести к проблемам производительности, если в директории содержится больше количество файлов.

    folders:
        - map: ~/code/project1
          to: /home/vagrant/project1

        - map: ~/code/project2
          to: /home/vagrant/project2

> {note} Вы никогда не должны монтировать `.` (текущая директория) при использовании Homestead. В этом случае Vagrant не сопоставит текущую директорию с `/vagrant` и не подключит дополнительные возможности, что вызовет неожиданные результаты во время подготовки.

Для включения [NFS](https://www.vagrantup.com/docs/synced-folders/nfs.html) просто добавьте простой ключ к вашей синхронизируемой папке:

    folders:
        - map: ~/code/project1
          to: /home/vagrant/project1
          type: "nfs"

> {note} При использовании NFS на Windows вам следует рассмотреть возможность установки плагина [vagrant-winnfsd](https://github.com/winnfsd/vagrant-winnfsd). Этот плагин будет поддерживать верные права пользователя / группы относительно файлов / директорий для файлов и директорий в боксе Homestead.

Также вы можете передавать параметры, поддерживаемые [синхронизируемыми папками](https://www.vagrantup.com/docs/synced-folders/basic_usage.html) Vagrant, указывая их под ключом `options`:

    folders:
        - map: ~/code/project1
          to: /home/vagrant/project1
          type: "rsync"
          options:
              rsync__args: ["--verbose", "--archive", "--delete", "-zz"]
              rsync__exclude: ["node_modules"]

#### Настройка сайтов Nginx 

Не знакомы с Nginx? Не проблема. Параметр `sites` позволяет легко связать "домен" с папкой в среде Homestead. Типовая конфигурация сайта включена в файл `Homestead.yaml`. И снова, вы можете добавить столько сайтов к своей среде Homestead, сколько необходимо. Homestead может служить удобной виртуальной средой для каждого проекта Laravel, над которым вы работаете:

    sites:
        - map: homestead.test
          to: /home/vagrant/project1/public

Если вы измените параметр `sites` после подключения Homestead-бокса, необходимо перезапустить `vagrant reload --provision`, чтобы обновить конфигурацию Nginx на виртуальной машине.

> {note} Скрипты Homestead пострены таким образом, чтобы быть идемпотентными, на сколько это возможно. Тем не менее, если у вас возникли проблемы во время настройки, вы должны разрушить и пересобрать виртуальную машину командой `vagrant destroy && vagrant up`.

<a name="hostname-resolution"></a>
#### Разрешение имён хостов

Homestead использует `mDNS` для автоматического разрешения хостов. Если вы укажете `hostname: homestead` в вашем файле `Homestead.yaml`, хост будет доступен по адресу `homestead.local`. Дистрибутивы MacOS, iOS и Linux desktop уже содержат поддержку `mDNS`. Для Windows требуется установить [Bonjour Print Services for Windows](https://support.apple.com/kb/DL999?viewlocale=en_US&locale=en_US).

Использование автоматических имен хостов лучше всех работает для установок Homestead "на каждый проект отдельно". Если вы размещаете несколько сайтов на одной копии Homestead, вам нужно добавить записи доменов в вайш файл `hosts` для каждого сайта. Файл `hosts` перенаправит запросы к вашим сайтам в вашу машину Homestead. На Mac и Linux этот файл расположен в `/etc/hosts`. На Windows он расположен в `C:\Windows\System32\drivers\etc\hosts`. Строки, которые вы добавляете в этот файл, будут выглядеть примерно так:

    192.168.10.10  homestead.test

Удостоверьтесь, что IP-адрес тот же, что вы установили в своём файле `Homestead.yaml`. Когда вы добавите домен в свой файл `hosts` и запустите Vagrant-бокс, вы можете получить доступ к сайту через свой веб-браузер:

    http://homestead.test

<a name="launching-the-vagrant-box"></a>
### Запуск Vagrant Box

Когда вы отредактировали `Homestead.yaml` по собственному усмотрению, выполните команду `vagrant up` в папке Homestead. Vagrant загрузит виртуальную машину и настроит ваши общие папки и сайты Nginx автоматически.

Чтобы уничтожить машину, вы можете использовать команду `vagrant destroy --force`.

<a name="per-project-installation"></a>
### Установка для проекта

Вместо глобальной установки Homestead и использования одного Homestead-бокса для всех ваших проектов, вы можете настроить отдельный экземпляр Homestead для каждого проекта. Установка Homestead для проекта может быть выгоднее, когда вы хотите поставлять файл `Vagrantfile` вместе с вашим проектом, позволяя тем, кто работает над проектом, выполнять `vagrant up`.

Чтобы установить Homestead непосредственно в ваш проект, затребуйте его с помощью Composer:

    composer require laravel/homestead --dev

Когда Homestead установлен, используйте команду `make` для создания `Vagrantfile` и файла `Homestead.yaml` в корне вашего проекта. Команда `make` автоматически настроит директивы `sites` м `folders` в файле `Homestead.yaml`.

Mac / Linux:

    php vendor/bin/homestead make

Windows:

    vendor\\bin\\homestead make

Затем выполните команду `vagrant up` в терминале и зайдите в свой проект по адресу `http://homestead.test` через браузер. Не забывайте, что вам по-прежнему необходимо добавить строку в `/etc/hosts` для `homestead.test` или любого другого выбранного вами домена, если вы не используете [разрешение имён хостов](#hostname-resolution).

<a name="installing-optional-features"></a>
### Установка дополнительного ПО

Опциональное ПО устанавливается использованием настроек "features" в вашем конфигурационном файле Homestead. Большинство дополнений можно включить или отключить установкой булевого значение, в то время как некоторые допускают несколько вариантов конфигурации:

    features:
        - blackfire:
            server_id: "server_id"
            server_token: "server_value"
            client_id: "client_id"
            client_token: "client_value"
        - cassandra: true
        - chronograf: true
        - couchdb: true
        - crystal: true
        - docker: true
        - elasticsearch:
            version: 7
        - gearman: true
        - golang: true
        - grafana: true
        - influxdb: true
        - mariadb: true
        - minio: true
        - mongodb: true
        - mysql8: true
        - neo4j: true
        - ohmyzsh: true
        - openresty: true
        - pm2: true
        - python: true
        - rabbitmq: true
        - solr: true
        - webdriver: true

#### MariaDB

Включение MariaDB удалит MySQL и установит MariaDB. MariaDB является полноценной заменой для MySQL, поэтому вы должны использовать тот же драйвер БД `mysql` в настройках базы данных приложения.

#### MongoDB

По умолчанию база данных MongoDB устанавливается с имененм пользователя `homestead` и паролем `secret`.

#### Elasticsearch

Вы можете указать желаемую версию Elasticsearch, которая может быть мажорной или точным номером версии (major.minor.patch). Установка по умолчанию создаст кластер 'homestead'. Вы никогда не должны предоставлять Elasticsearch больше половины оперативной памяти, поэтому удостоверьтесь, что ваша машина имеет по крайней мере в два раза больше памяти, чем выделено для Elasticsearch.

> {tip} Обратитесь к [документации Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current) чтобы узнать как настроить вашу конфигурацию.

#### Neo4j

По умолчанию Neo4j устанавливается с именем пользователя `homestead` и паролем `secret`. Для доступа кNeo4j через браузер перейдите по адресу `http://homestead.test:7474`. Порты `7687` (Bolt), `7474` (HTTP) и `7473` (HTTPS) готовы к обработке запросов от клиента Neo4j.

<a name="aliases"></a>
### Псевдонимы

Чтобы добавить псевдонимы Bash измените файл `aliases` в вашем директории Homestead:

    alias c='clear'
    alias ..='cd ..'

После изменения файла `aliases` необходимо перезапустить машину Homestead командой `vagrant reload --provision`. Это гарантирует, что ваши новые псеводонимы будут доступны в виртуальной машине.

<a name="daily-usage"></a>
## Повседневное использование

<a name="accessing-homestead-globally"></a>
### Глобальный доступ к Homestead

Иногда вам может понадобиться выполнить `vagrant up` вашей Homestead-машины из любого места вашей файловой системы. На системах Mac / Linux это можно сделать добавив Bash-функцию в ваш Bash-профиль. На Windows можно добавить "пакетный" файл к своему `PATH`. Эти скрипты позволят вам выполнять любые команды Vagrant из любого места вашей системы, и автоматически укажет команде на ваш установленный Homestead:

#### Mac / Linux

    function homestead() {
        ( cd ~/Homestead && vagrant $* )
    }

Не забудьте исправить путь `~/Homestead` в функции на реальное расположение вашего установленного Homestead. Когда функция установлена, вы можете выполнять такие команды, как `homestead up` или `homestead ssh` из любого места вашей системы.

#### Windows

Создайте `homestead.bat` пакетный файл из любого места на вашей машине, использовав следующее содержимое:

    @echo off

    set cwd=%cd%
    set homesteadVagrant=C:\Homestead

    cd /d %homesteadVagrant% && vagrant %*
    cd /d %cwd%

    set cwd=
    set homesteadVagrant=

Не забудьте исправить путь `C:\Homestead` в скрипте на реальное расположение вашего установленного Homestead.. После того как вы создали файл, добавьте местоположение своего файла в свой `PATH`. Вы можете выполнять такие команды, как `homestead up` или `homestead ssh` из любого места своей системы.

<a name="connecting-via-ssh"></a>
### Подключение через SSH

Для подключения к своей виртуальной машине по SSH вы можете использовать команду `vagrant ssh` из своей Homestead-директории.

Но поскольку вам, скорее всего, потребуется часто подключаться к вашей Homestead-машине по SSH, будет удобно создать "функцию" на вашей хост-машине для быстрого подключения, как описано выше.

<a name="connecting-to-databases"></a>
### Подключение к базам данных

База `homestead` изначально настроена на использование и MySQL, и PostgreSQL. Чтобы подключиться к вашей базе данных MySQL или Postgres через клиент БД с вашей хост-машины, вы должны подключиться к `127.0.0.1` через порт `33060` (MySQL) или `54320` (Postgres). Имя пользователя и пароль для обеих баз данных `homestead` / `secret`.

> {note} Вы должны использовать эти нестандартные порты только подключаясь к базам данных с вашей главной машины. Вы будете использовать порты 3306 и 5432 в вашем конфигурационном файле базы данных Laravel, так как Laravel запущен _на_ виртуальной машине.

<a name="database-backups"></a>
### Резервные копии баз данных

Homestead может автоматически создавать резервные копии баз данных когда Vagrant завершает работу. Эта возможность доступна в Vagrant начиная с версии 2.1.0. Или, если вы используете более старую версию Vagrant, вы можете установить плагин `vagrant-triggers`. Чтобы включить автоматические резервирования баз данных, добавьте следующую строку в ваш файл `Homestead.yaml`:

    backup: true

После этого Homestead будет экспортировать ваши базы данных в директории `mysql_backup` и `postgres_backup` при выполнении команды `vagrant destroy`. Эти директории располагаются в папке, куда вы склонировали Homestead или в корень вашего проекта при использовании метода [установки для проекта](#per-project-installation) .

<a name="database-snapshots"></a>
### Снимки базы данных

Homestead поддерживает сохранение состояния баз данных MySQL и MariaDB и переключение между ними использованием [Logical MySQL Manager](https://github.com/Lullabot/lmm). Для примера представьте работу над сайтом с базой данных в несколько гигабайт. Вы можете импортировать базу данных и сделать снимок. После выполненеия некоторой работы и создания какого-либо тестового контента, вы можете быстро вернуть базу данных к исходному состоянию.

Под капотом LMM использует возможности снимков LVM для поддержки копирования-при-записи. На практике это означает, что изменение одной записи в таблице приведёт только к тому, что внесённые изменения будут записаны на диск, что значительно экономит время во время восстановления.

Так как `lmm` взаимодействует с LVM, он должен быть запущен как `root`. Чтобы увидеть все доступные команды, выполните `sudo lmm` в вашем боксе Vagrant. Общий процесс выглядит следующим образом:

1. Импорт базы данных в ветку в lmm ветку по умолчанию `master`.
1. Создание снимка не изменённой базы данных командой `sudo lmm branch prod-YYYY-MM-DD`.
1. Изменение базы данных.
1. Выполнение `sudo lmm merge prod-YYYY-MM-DD` для отмены всех изменений.
1. Выполнение `sudo lmm delete <branch>` для удаления ненужных ветвей.

<a name="adding-additional-sites"></a>
### Добавление дополнительных сайтов

После настройки и запуска вашей среды Homestead вы можете захотеть добавить дополнительные Nginx-сайты для своих Laravel-приложений. Вы можете запустить в одной среде Homestead столько установок Laravel, сколько захотите. Вы можете добавить сайты в свой файл `Homestead.yaml`:

    sites:
        - map: homestead.test
          to: /home/vagrant/project1/public
        - map: another.test
          to: /home/vagrant/project2/public

Если Vagrant не управляет вашим файлом "hosts" автоматически, вам может потребоваться также добавить к тому файлу новый сайт:

    192.168.10.10  homestead.test
    192.168.10.10  another.test

После добавления сайта выполните команду `vagrant reload --provision` из своей директории Homestead.

<a name="site-types"></a>
#### Типы сайтов

Homestead поддерживает несколько типов сайтов, что позволяет вам запросто работать с проектами, которые не работаю на Laravel. Например, мы можем запросто добавить Symfony-приложение в Homestead, используя тип сайта `symfony2`:

    sites:
        - map: symfony2.test
          to: /home/vagrant/my-symfony-project/web
          type: "symfony2"

Доступные типы сайтов: `apache`, `apigility`, `expressive`, `laravel` (по умолчанию), `proxy`, `silverstripe`, `statamic`, `symfony2`, `symfony4` и `zf`.

<a name="site-parameters"></a>
#### Параметры сайтов

Вы можете добавить дополнительные Nginx-значения `fastcgi_param` вашему сайте через директиву `params`. Например, мы добавим параметр `FOO` со значением `BAR`:

    sites:
        - map: homestead.test
          to: /home/vagrant/project1/public
          params:
              - key: FOO
                value: BAR

<a name="environment-variables"></a>
### Переменные окружения

Вы можете назначить глобальные переменные окружения добавлением их в ваш файл `Homestead.yaml`:

    variables:
        - key: APP_ENV
          value: local
        - key: FOO
          value: bar

После изменения `Homestead.yaml`, не забудьте выполнить команду `vagrant reload --provision`. Она обновит конфигурацию PHP-FPM для всех установленных версий PHP, а также обновит окружение пользователя `vagrant`.

<a name="configuring-cron-schedules"></a>
### Настройка расписания Cron

Laravel предоставляет удобный способ для [планирования Cron-задач](/docs/{{version}}/scheduling) путём планирования единственной Artisan-команды `schedule:run` на ежеминутное выполнение. Команда `schedule:run` проверит запланированные задачи, определённые в классе `App\Console\Kernel`, и определит, какие задачи необходимо выполнить.

Если вы хотите выполнить команду `schedule:run` для сайта Homestead, вы можете задать значение `true` для параметра `schedule` при определении сайта:

    sites:
        - map: homestead.test
          to: /home/vagrant/project1/public
          schedule: true

Cron-задача для сайта будет определена в папке `/etc/cron.d` на виртуальной машине.

<a name="configuring-mailhog"></a>
### Настройка Mailhog

Mailhog предоставляет простой способ перехвата и просмотра исходящей электронной почты без реальной отправки получателям. Чтобы начать пользоваться Mailhog, добавьте в ваш `.env`-файл следующие настройки:

    MAIL_DRIVER=smtp
    MAIL_HOST=localhost
    MAIL_PORT=1025
    MAIL_USERNAME=null
    MAIL_PASSWORD=null
    MAIL_ENCRYPTION=null

После этого вы сможете получить доступ к панели Mailhog по адресу `http://localhost:8025`.

<a name="configuring-minio"></a>
### Настройка Minio

Minio это сервер хранения объектов с открытым исходным кодом, совместимый с API Amazon S3. ДЛя установки Minio, включите в вашем файле `Homestead.yaml` следующую опцию в секции [Установка дополнительных расширений](#installing-optional-features):

    minio: true

По умолчанию Minio запущен на порту 9600. Для доступа к панели урпавления Minio откройте старинцу `http://localhost:9600/`. Ключ доступа по умолчанию - `homestead`, секретный ключ - `secretkey`. При доступе к Minio всегда следует использовать регион `us-east-1`.

Для использования Minio необходимо настроить конфигурацию диска S3 в вашем файле `config/filesystems.php`. Нужно добавить опцию `use_path_style_endpoint`, а также `url` для ключа `endpoint`:

    's3' => [
        'driver' => 's3',
        'key' => env('AWS_ACCESS_KEY_ID'),
        'secret' => env('AWS_SECRET_ACCESS_KEY'),
        'region' => env('AWS_DEFAULT_REGION'),
        'bucket' => env('AWS_BUCKET'),
        'endpoint' => env('AWS_URL'),
        'use_path_style_endpoint' => true
    ]

Наконец, убедитесь что в файле `.env` имеются следующие опции:

    AWS_ACCESS_KEY_ID=homestead
    AWS_SECRET_ACCESS_KEY=secretkey
    AWS_DEFAULT_REGION=us-east-1
    AWS_URL=http://localhost:9600

Добавьте директивы `buckets` файл конфигурационный файл Homestead:

    buckets:
        - name: your-bucket
          policy: public
        - name: your-private-bucket
          policy: none

Атрибут `policy` может быть `none`, `download`, `upload` или `public`.

<a name="ports"></a>
### Порты

По умолчанию следующие порты переадресованы в вашу среду Homestead:

<div class="content-list" markdown="1">
- **SSH:** 2222 &rarr; переадресован в 22
- **ngrok UI:** 4040 &rarr; переадресован в 4040
- **HTTP:** 8000 &rarr; переадресован в 80
- **HTTPS:** 44300 &rarr; переадресован в 443
- **MySQL:** 33060 &rarr; переадресован в 3306
- **PostgreSQL:** 54320 &rarr; переадресован в 5432
- **MongoDB:** 27017 &rarr; переадресован в 27017
- **Mailhog:** 8025 &rarr; переадресован в 8025
- **Minio:** 9600 &rarr; переадресован в 9600
</div>

#### Перенаправление дополнительных портов

По желанию можно переадресовать дополнительные порты в Vagrant-бокс, а также указать их протокол:

    ports:
        - send: 50000
          to: 5000
        - send: 7777
          to: 777
          protocol: udp

<a name="sharing-your-environment"></a>
### Совместное использование вашей среды

Иногда вам может потребоваться поделиться тем, над чем вы работаете в данный момент, с другими разработчиками или клиентом. В Vagrant есть встроенный способ поддержки данного функционала через `vagrant share`; однако, это не будет работать, если в вашем файле `Homestead.yaml` настроено несколько сайтов.

Чтобы решить это проблему у Homestead есть собственная команда `share`. Для начала подключитесь к своей Homested-машине через `vagrant ssh` и выполните `share homestead.test`. Это позволит поделиться сайтом `homestead.test` из вашего конфига `Homestead.yaml`. Вы можете заменить любой из других своих настроенных сайтов на `homestead.test`:

    share homestead.test

После запуска команды вы увидите, как появится экран Ngrok, на котором содержится журнал активности и  общедоступные URL для сайта, которым вы поделились. Если вы хотите указать пользовательскую область, поддомен или другую настройку Ngrok, можно добавить их к своей команде `share`:

    share homestead.test -region=eu -subdomain=laravel

> {note} Помните, что Vagrant от природы небезопасен и вы выставляете свою виртуальную машину всему Интернет-пространству во время запуска команды `share`.

<a name="multiple-php-versions"></a>
### Несколько версий PHP

Homestead 6 представил поддержку нескольких версий PHP на одной и той же виртуальной машине. Вы можете указать какие версии PHP использовать для заданного сайта в своем файле `Homestead.yaml`. Доступные версии PHP: "5.6", "7.0", "7.1", "7.2", "7.3", and "7.4" (по умолчанию):

    sites:
        - map: homestead.test
          to: /home/vagrant/project1/public
          php: "7.1"

Дополнительно вы можете пользоваться любой из поддерживаемых версий PHP через CLI:

    php5.6 artisan list
    php7.0 artisan list
    php7.1 artisan list
    php7.2 artisan list
    php7.3 artisan list
    php7.4 artisan list

Также вы можете обновить консольную версию PHP по умолчанию, выполнив следующие команды на вашей виртуальной машине Homestead:

    php56
    php70
    php71
    php72
    php73
    php74

<a name="web-servers"></a>
### Веб-серверы

По умолчанию Homestead использует веб-сервер Nginx. Тем не менее можно использовать Apache, если в качестве типа сайта указан `apache`. Не смотря на то, что могут быть установлены оба веб-сервера, одновременно *запущен* может быть только один. Команда `flip` позволяет легко переключить используемый веб-сервер. Команда `flip` автоматически определяет какой сервер запущен, останавливает его и затем запускает другой сервер. Для использования этой команды подключитесь по SSH к вашей виртуальной машине Homestead и выполните команду в терминале:

    flip

<a name="mail"></a>
### Почта

Homestead включает в себя постфиксный агент передачи почты, который по умолчанию "слушает" порт `1025`. Таким образом вы можете настроить ваше приложение на использование почтового драйвера `smtp` на сервере `localhost` и порту `1025`. После этого все отправляемые почтовые сообщения будут отловлены Postfix-ом и переданы Mailhog. Чтобы увидеть отправляемые сообщения, откройте [http://localhost:8025](http://localhost:8025) в вашем браузере.

<a name="debugging-and-profiling"></a>
## Отладка и профилирование

<a name="debugging-web-requests"></a>
### Отладка веб-запросов при помощи Xdebug

Homestead поддерживает пошаговую отладку при помощи [Xdebug](https://xdebug.org). Например, вы можете загрузить веб-страницу в браузере, при этом PHP подключится к вашей IDE чтобы предоставить возможность исследвать и модифицировать исполняющийся код.

По умолчанию Xdebug запускается автоматически и готов к обработке запросв. Если вам нужно включить Xdebug из консоли, выполните команду `sudo phpenmod xdebug` на вашей виртуальной машине. Далее следуйте инструкциям вашей IDE по включению отладки. Наконец, настройте ваш бразуер, включив расширение Xdebug или [bookmarklet](https://www.jetbrains.com/phpstorm/marklets/).

> {note} При использовании Xdebug, PHP будет работать значительно медленнее. Чтобы отключить Xdebug, выполните `sudo phpdismod xdebug` в вашй виртуальной машине и перезагрузите FPM.

<a name="debugging-cli-applications"></a>
### Откладка консольных приложений

Для откладки консольных приложений PHP, используйте псевдоним `xphp` в вашей виртуальной машине:

    xphp path/to/script

#### Автозапуск Xdebug

При отладке функциональных тестов, выполняющих запросы к веб-серверу, проще запускать отладку автоматически, а не изменять тесты, добавляя какой-либо заголовок или устанавливая cookie для включения режима отладки. Чтобы заставить Xdebug запускаться автоматически, измените файл `/etc/php/7.x/fpm/conf.d/20-xdebug.ini` внутри вашего контейнера Vargant, добавив следующие инструкции:

    ; Если в Homestead.yaml указан другой IP-адрес, этот адрес может быть другим...
    xdebug.remote_host = 192.168.10.1
    xdebug.remote_autostart = 1

<a name="profiling-applications-with-blackfire"></a>
### Профилирование приложений при помощи Blackfire

[Blackfire](https://blackfire.io/docs/introduction) - это сервис SaaS для профилирования веб-запросов и консольных приложений написания утверждений производительности. Он предоставляет интерактивный интерфейс, отображая информацию в виде графиков вызовов и временных шкалах. Он может быть использован во время разработки, постановки, а также рабочем режиме без создания дополнительной нагрузки для конечных пользователей. Он обеспечивает производительнсоть, качество и проверку безопасности кода и настроек вашего `php.ini`.

[Blackfire Player](https://blackfire.io/docs/player/index) - это приложение с открытым исходным кодом для обхода веб-страниц для тестирования и очистки веб-приложения, который может работать совместно с Blackfire для формирования сценария профилирования.

Чтобы включить Blackfire, воспользуйтесь секцией "features" в вашем конфигурационном файле Homestead:

    features:
        - blackfire:
            server_id: "server_id"
            server_token: "server_value"
            client_id: "client_id"
            client_token: "client_value"

В этой конфигурации следует указать регистрационные данные вашего [аккаунта backfire.io](https://blackfire.io/signup). Blackfire предоставляет различные варианты профилирования приложения, включая консольные инструменты и расширение для браузера. Обратитесь к [документации Backfire за дополнительной информацией](https://blackfire.io/docs/cookbooks/index).

### Профилирование производительности PHP при помощи XHGui

[XHGui](https://www.github.com/perftools/xhgui) - это пользовательский интерфейс для изучения производительности PHP-приложений. Для включения XHGui, добавьте `xhgui: 'true'` к конфигурации вашего сайта:

    sites:
        -
            map: your-site.test
            to: /home/vagrant/your-site/public
            type: "apache"
            xhgui: 'true'

Если сайт уже существует, выполните команду `vagrant provision` после внесения изменений в конфигурацию.

Для профилирования веб-запросов, добавьте параметр `xhgui=on` к HTTP GET-запросу. XHGui автоматически присоединит к ответу необходимые cookie, поэтому последующие запросы не потребуют указания этого параметра в запросе. Увидеть результаты профилирования можно в браузер, открыв страницу `http://your-site.test/xhgui`.

Для профилирования консольных приложений, прежде выполните команду `XHGUI=on`:

    XHGUI=on path/to/script

Результаты профилирования можно получить таким же образом, как и для веб-приложений.

Заметьте, что профилирование замедляет выполнение скриптов и время выполнения может быть в два раза больше чем в реальности. Поэтому всегда обращайте внимание на относительные изменения времени, а не на абсолютные значения. Также, не забывайте, что это время включает любое время, в течени которого выполнение приостановленно во время отладки.

Профилирование потребует значительное пространство на диске, однако оно будет высвобождено автоматически через несколько дней.

<a name="network-interfaces"></a>
## Сетевые интерфейсы
Свойство `networks` файла `Homestead.yaml` настраивает сетевые интерфейсы вашей среды Homestead. Вы можете настроить столько интерфейсов, сколько потребуется:

    networks:
        - type: "private_network"
          ip: "192.168.10.20"

Чтобы включить интерфейс с [мостовым соединением](https://www.vagrantup.com/docs/networking/public_network.html), измените настройку `bridge` и измените тип сети на `public_network`:

    networks:
        - type: "public_network"
          ip: "192.168.10.20"
          bridge: "en1: Wi-Fi (AirPort)"

Чтобы включить [DHCP](https://www.vagrantup.com/docs/networking/public_network.html), просто уберите опцию `ip` из своей настройки:

    networks:
        - type: "public_network"
          bridge: "en1: Wi-Fi (AirPort)"

<a name="extending-homestead"></a>
## Extending Homestead

Вы можете как-либо расширять Homestead используя скрипт `after.sh`, находящийся в корневой директории. Добавьте в этот файл необходимые shell-команды, настраивающие виртуальную машину согласно вашим требованиям.

Во время настройки Homestead, Ubuntu может спросить хотите ли вы сохранить оригинальную конфигурацию пакета или перпезаписать её новым конфигурационным файлом. Чтобы этого избежать, следует использовать следующую команду при установке пакетов, которая не будет перезаписывать конфигурацию, написанную Homestead:

    sudo apt-get -y \
        -o Dpkg::Options::="--force-confdef" \
        -o Dpkg::Options::="--force-confold" \
        install your-package

### Пользовательские настройки

Когда Homestead используется в команде, вы можете захотеть использовать конфигурацию, лучше подходящую для вашего личного окружения. Для этого вы можете создать файл `user-customizations.sh` в корневой директории Homestead (директория, соддержащая ваш `Homestead.yaml`). В этом файле, вы можете выполнить нужные вам настройки; но, тем не менее, файл `user-customizations.sh` не должен находиться под упарвлением системы контроля версий.

<a name="updating-homestead"></a>
## Обновление Homestead

Прежде чем начать обновление Homestead убедитесь, что виртуальная машина остановлена, выполнив следующую команду в вашем директории Homestead:

    vagrant destroy

If you cloned the repository you can run the following commands at the location you originally cloned the repository:

Затем вам надо обновить исходный код Homestead. Если вы клонировали репозиторий, то можете выполнить следующие команды в директории с репозиторием:

    git fetch

    git pull origin release

Эти команды скачают последнюю версию кода Homestead из репозитория GitHub, извлекают последние теги и после этого проверит последнию версию, помеченную тегом "release". Вы можете найти последнюю стабильную версию на [странице релизов GitHub](https://github.com/laravel/homestead/releases).

Если вы установили Homestead через файл `composer.json`, то должны убедиться, что файл `composer.json` содержит строку `"laravel/homestead": "^10"` и обновить ваши зависимости:

    composer update

Затем, нужно обновить контейнер Vagrant командой `vagrant box update`:

    vagrant box update

Наконец, нужно пересоздать контейнер Homestead для использования последней установки Vagrant:

    vagrant up

<a name="provider-specific-settings"></a>
## Специальные настройки провайдера

<a name="provider-specific-virtualbox"></a>
### VirtualBox

#### `natdnshostresolver`

По умолчанию Homestead устанавливает опцию `natdnshostresolver` в `on`. Это позволяет Homestead использовать настройки DNS вашей операционной системы. Если вы хотите переопределить такое поведение, добавьте следующие строки к своему файлу `Homestead.yaml`:

    provider: virtualbox
    natdnshostresolver: off

#### Символьные ссылки в Windows

Если символьные ссылки не работают в Windows, вам понадобится добавить следующий блок в ваш `Vagrantfile`:

    config.vm.provider "virtualbox" do |v|
        v.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
    end
