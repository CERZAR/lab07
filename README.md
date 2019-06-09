[![Build Status](https://travis-ci.org/CERZAR/lab04.svg?branch=master)](https://travis-ci.org/CERZAR/lab04)

## Laboratory work IV

Данная лабораторная работа посвещена изучению систем непрерывной интеграции на примере сервиса **Travis CI**

```ShellSession
$ open https://travis-ci.org
```

## Tasks

- [x] 1. Авторизоваться на сервисе **Travis CI** с использованием **GitHub** аккаунта
- [x] 2. Создать публичный репозиторий с названием **lab04** на сервисе **GitHub**
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Включить интеграцию сервиса **Travis CI** с созданным репозиторием
- [x] 5. Получить токен для **Travis CLI** с правами **repo** и **user**
- [x] 6. Получить фрагмент вставки значка сервиса **Travis CI** в формате **Markdown**
- [x] 7. Выполнить инструкцию учебного материала
- [x] 8. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=CERZAR                                        # Установка переменной GITHUB_USERNAME
$ export GITHUB_TOKEN=f90eac0be4dcf7e1f1a5130944e262a2dbaaa5b7         # Установка переменной GITHUB_TOKEN
```

```ShellSession
$ cd ${GITHUB_USERNAME}/workspace                                      # Переход в рабочую директорию
$ pushd .                                                              # Сохранение текущей директории
~/CERZAR/workspace ~/CERZAR/workspace
$ source scripts/activate                                              # Выполнение скрипта настройки пространства
```

```ShellSession
$ \curl -sSL https://get.rvm.io | bash -s -- --ignore-dotfiles         # Получение и исполнение установочного bash-файла
Turning on ignore dotfiles mode. 
Downloading https://github.com/rvm/rvm/archive/master.tar.gz 
Installing RVM to /home/cezar/.rvm/ 
Installation of RVM in /home/cezar/.rvm/ is almost complete: 

 * To start using RVM you need to run `source /home/cezar/.rvm/scripts/rvm` 
   in all your open shell windows, in rare cases you need to reopen all shell windows. 
 * WARNING: Found --user-install in /etc/gemrc, please remove it, as it will break rubygems in RVM. 
   If it is intended or a mistake export rvm_ignore_gemrc_issues=1 to avoid this warning. 

Thanks for installing RVM 
$ echo "source $HOME/.rvm/scripts/rvm" >> scripts/activate            # Дописывание в scripts/activate команду для выполнения скрипта запуска rvm
$ . scripts/activate                                                  # Выполнение активационного скрипта
$ rvm autolibs disable                                                # Отключение установки зависимостей
$ rvm install ruby-2.4.2                                              # Установление ruby версии 2.4.2
Warning, new version of rvm available '1.29.8', you are using older version '1.29.8-next'.
You can disable this warning with:   echo rvm_autoupdate_flag=0 >> ~/.rvmrc
You can enable auto-update with:     echo rvm_autoupdate_flag=2 >> ~/.rvmrc
You can update manually with:        rvm get VERSION                         (e.g. 'rvm get stable')

Searching for binary rubies, this might take some time.
No binary rubies available for: manjaro/18.0.4/x86_64/ruby-2.4.2.
Continuing with compilation. Please read 'rvm help mount' to get more information on binary rubies.
Installing Ruby from source to: /home/cezar/.rvm/rubies/ruby-2.4.2, this may take a while depending on your cpu(s)...
ruby-2.4.2 - #downloading ruby-2.4.2, this may take a while depending on your connection...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 12.0M  100 12.0M    0     0  7141k      0  0:00:01  0:00:01 --:--:-- 7137k
No checksum for downloaded archive, recording checksum in user configuration.
ruby-2.4.2 - #extracting ruby-2.4.2 to /home/cezar/.rvm/src/ruby-2.4.2.....
ruby-2.4.2 - #configuring..................................................................
ruby-2.4.2 - #post-configuration..
ruby-2.4.2 - #compiling............................................................................................................................................................................-
ruby-2.4.2 - #installing............
ruby-2.4.2 - #making binaries executable..
ruby-2.4.2 - #downloading rubygems-3.0.3
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  882k  100  882k    0     0  1970k      0 --:--:-- --:--:-- --:--:-- 1970k
No checksum for downloaded archive, recording checksum in user configuration.
ruby-2.4.2 - #extracting rubygems-3.0.3.....
ruby-2.4.2 - #removing old rubygems........
ruby-2.4.2 - #installing rubygems-3.0.3...................................
ruby-2.4.2 - #gemset created /home/cezar/.rvm/gems/ruby-2.4.2@global
ruby-2.4.2 - #importing gemset /home/cezar/.rvm/gemsets/global.gems................................................................
ruby-2.4.2 - #generating global wrappers.......
ruby-2.4.2 - #gemset created /home/cezar/.rvm/gems/ruby-2.4.2
ruby-2.4.2 - #importing gemsetfile /home/cezar/.rvm/gemsets/default.gems evaluated to empty gem list
ruby-2.4.2 - #generating default wrappers.......
ruby-2.4.2 - #adjusting #shebangs for (gem irb erb ri rdoc testrb rake).
Install of ruby-2.4.2 - #complete 
Ruby was built without documentation, to build it run: rvm docs generate-ri
$ rvm use 2.4.2 –default                                                # Установка установленной версии как основной
Using /home/cezar/.rvm/gems/ruby-2.4.2
$ gem install travis                                                    # Установка travis (из пакетов для ruby)
Fetching backports-3.15.0.gem
Fetching multipart-post-2.1.1.gem
Fetching faraday-0.15.4.gem
Fetching faraday_middleware-0.13.1.gem
Fetching highline-1.7.10.gem
Fetching multi_json-1.13.1.gem
Fetching addressable-2.4.0.gem
Fetching net-http-persistent-2.9.4.gem
Fetching gh-0.15.1.gem
Fetching ffi-1.11.1.gem
Fetching net-http-pipeline-1.0.1.gem
Fetching launchy-2.4.3.gem
Fetching ethon-0.12.0.gem
Fetching websocket-1.2.8.gem
Fetching pusher-client-0.6.2.gem
Fetching travis-1.8.10.gem
Fetching typhoeus-0.8.0.gem
Successfully installed multipart-post-2.1.1
Successfully installed faraday-0.15.4
Successfully installed faraday_middleware-0.13.1
Successfully installed highline-1.7.10
Successfully installed backports-3.15.0
Successfully installed multi_json-1.13.1
Successfully installed addressable-2.4.0
Successfully installed net-http-persistent-2.9.4
Successfully installed net-http-pipeline-1.0.1
Successfully installed gh-0.15.1
Successfully installed launchy-2.4.3
Building native extensions. This could take a while...
Successfully installed ffi-1.11.1
Successfully installed ethon-0.12.0
Successfully installed typhoeus-0.8.0
Successfully installed websocket-1.2.8
Successfully installed pusher-client-0.6.2
Successfully installed travis-1.8.10
Parsing documentation for multipart-post-2.1.1
Installing ri documentation for multipart-post-2.1.1
Parsing documentation for faraday-0.15.4
Installing ri documentation for faraday-0.15.4
Parsing documentation for faraday_middleware-0.13.1
Installing ri documentation for faraday_middleware-0.13.1
Parsing documentation for highline-1.7.10
Installing ri documentation for highline-1.7.10
Parsing documentation for backports-3.15.0
Installing ri documentation for backports-3.15.0
Parsing documentation for multi_json-1.13.1
Installing ri documentation for multi_json-1.13.1
Parsing documentation for addressable-2.4.0
Installing ri documentation for addressable-2.4.0
Parsing documentation for net-http-persistent-2.9.4
Installing ri documentation for net-http-persistent-2.9.4
Parsing documentation for net-http-pipeline-1.0.1
Installing ri documentation for net-http-pipeline-1.0.1
Parsing documentation for gh-0.15.1
Installing ri documentation for gh-0.15.1
Parsing documentation for launchy-2.4.3
Installing ri documentation for launchy-2.4.3
Parsing documentation for ffi-1.11.1
Installing ri documentation for ffi-1.11.1
Parsing documentation for ethon-0.12.0
Installing ri documentation for ethon-0.12.0
Parsing documentation for typhoeus-0.8.0
Installing ri documentation for typhoeus-0.8.0
Parsing documentation for websocket-1.2.8
Installing ri documentation for websocket-1.2.8
Parsing documentation for pusher-client-0.6.2
Installing ri documentation for pusher-client-0.6.2
Parsing documentation for travis-1.8.10
Installing ri documentation for travis-1.8.10
Done installing documentation for multipart-post, faraday, faraday_middleware, highline, backports, multi_json, addressable, net-http-persistent, net-http-pipeline, gh, launchy, ffi, ethon, typhoeus, websocket, pusher-client, travis after 36 seconds
17 gems installed
```

```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab03 projects/lab04            # Скачивание репозитория
Cloning into 'projects/lab04'...
remote: Enumerating objects: 27, done.
remote: Counting objects: 100% (27/27), done.
remote: Compressing objects: 100% (19/19), done.
remote: Total 27 (delta 4), reused 23 (delta 3), pack-reused 0
Unpacking objects: 100% (27/27), done.
$ cd projects/lab04                                                               # Переход в папку с репозиторием
$ git remote remove origin                                                        # Удаление связки с удаленным репозиторием
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab04               # Добавление связки с новым удаленным репозиторием
```

```ShellSession
$ cat > .travis.yml <<EOF                                                         # Записывание
language: cpp
EOF
```

```ShellSession
$ cat >> .travis.yml <<EOF                                                        # Дописывание

script:
- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
- cmake --build _build
- cmake --build _build --target install
EOF
```

```ShellSession
$ cat >> .travis.yml <<EOF                                                        # Дописывание

addons:
  apt:
    sources:
      - george-edison55-precise-backports
    packages:
      - cmake
      - cmake-data
EOF
```

```ShellSession
$ travis login --github-token ${GITHUB_TOKEN}                                     # Авторизация
Shell completion not installed. Would you like to install it now? |y| y
Successfully logged in as CERZAR!
```

```ShellSession
$ travis lint                                                                     # Исполнение команды проверки
Warnings for .travis.yml:
[x] value for addons section is empty, dropping
[x] in addons section: unexpected key apt, dropping
```

```ShellSession
$ sed -i '1i [![Build Status](https://travis-ci.org/CERZAR/lab04.svg?branch=master)](https://travis-ci.org/CERZAR/lab04)' README.md   # Применение преобразований
```

```ShellSession
$ git add .travis.yml                            # Фиксация .travis.yml
$ git add README.md                              # Фиксация README.md
$ git commit -m"added CI"                        # Коммит изменений (зафиксированных)
[master 0cf1e74] added CI
 2 files changed, 15 insertions(+)
 create mode 100644 .travis.yml
$ git push origin master                         # Отправка изменений ветки master в удаленный репозиторий
Username for 'https://github.com': CERZAR
Password for 'https://CERZAR@github.com': 
Enumerating objects: 31, done.
Counting objects: 100% (31/31), done.
Delta compression using up to 4 threads
Compressing objects: 100% (26/26), done.
Writing objects: 100% (31/31), 14.05 KiB | 3.51 MiB/s, done.
Total 31 (delta 6), reused 0 (delta 0)
remote: Resolving deltas: 100% (6/6), done.
To https://github.com/CERZAR/lab04
 * [new branch]      master -> master
```

```ShellSession
$ travis lint                     # Проверка конфига
Warnings for .travis.yml:
[x] value for addons section is empty, dropping
[x] in addons section: unexpected key apt, dropping
$ travis accounts                 # Получение информации об аккаунтах
CERZAR (Cerzar): subscribed, 5 repositories
$ travis sync                     # Синхронизация
synchronizing: . done
$ travis repos                    # Получение списка репозиториев
CERZAR/lab00 (active: no, admin: yes, push: yes, pull: yes)
Description: Изучение систем обмена данными

CERZAR/lab01 (active: no, admin: yes, push: yes, pull: yes)
Description: Изучение утилит для разработки проектов

CERZAR/lab02 (active: no, admin: yes, push: yes, pull: yes)
Description: ???

CERZAR/lab03 (active: no, admin: yes, push: yes, pull: yes)
Description: ???

CERZAR/lab04 (active: yes, admin: yes, push: yes, pull: yes)
Description: ???
$ travis enable                   # Активация проекта
Detected repository as CERZAR/lab04, is this correct? |yes| yes
CERZAR/lab04: enabled :)
$ travis whatsup                  # Список последних сборок
CERZAR/lab04 passed: #1
$ travis branches                 # Список последних сборок по веткам проекта
master:  #1    passed     added CI
$ travis history                  # История cборок для проекта
#1 passed:       master added CI
$ travis show                     # Отображение основной информации о последней сборке
Job #1.1:  added CI
State:         passed
Type:          push
Branch:        master
Compare URL:   https://github.com/CERZAR/lab04/compare/d3f614c3c40f^...0cf1e742adec
Duration:      27 sec
Started:       2019-06-09 22:51:01
Finished:      2019-06-09 22:51:28
Allow Failure: false
Config:        os: linux
```

## Report

```ShellSession
$ popd
$ export LAB_NUMBER=04
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Homework

Вы продолжаете проходить стажировку в "Formatter Inc." (см [подробности](https://github.com/tp-labs/lab03#Homework)).

В прошлый раз ваше задание заключалось в настройке автоматизированной системы **CMake**.

Сейчас вам требуется настроить систему непрерывной интеграции для библиотек и приложений, с которыми вы работали в [прошлый раз](https://github.com/tp-labs/lab03#Homework). Настройте сборочные процедуры на различных платформах:
* используйте [TravisCI](https://travis-ci.com/) для сборки на операционной системе **Linux** с использованием компиляторов **gcc** и **clang**;
* используйте [AppVeyor](https://www.appveyor.com/) для сборки на операционной системе **Windows**.

## Links

- [Travis Client](https://github.com/travis-ci/travis.rb)
- [AppVeyour](https://www.appveyor.com/)
- [GitLab CI](https://about.gitlab.com/gitlab-ci/)

```
Copyright (c) 2015-2019 The ISC Authors
```
