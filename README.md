[![Build Status](https://travis-ci.org/CERZAR/lab06.svg?branch=master)](https://travis-ci.org/CERZAR/lab06)

## Laboratory work VI

Данная лабораторная работа посвещена изучению средств пакетирования на примере **CPack**

```ShellSession
$ open https://cmake.org/Wiki/CMake:CPackPackageGenerators
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab06** на сервисе **GitHub**
- [x] 2. Выполнить инструкцию учебного материала
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=CERZAR                           # Инициализация переменной GITHUB_USERNAME
$ export GITHUB_EMAIL=stavropoltsev.t@gmail.com           # Инициализация переменной GITHUB_EMAIL
$ alias edit=nano                                         # Синоним команды edit
$ alias gsed=sed                                          # Синоним команды gsed
```

```ShellSession
$ cd ${GITHUB_USERNAME}/workspace                         # Перемещение в указанную папку
$ pushd .                                                 # Сохранение указанной папки
~/CERZAR/workspace ~/CERZAR/workspace
$ source scripts/activate                                 # Выполнение скрипта активации
```

```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab05 projects/lab06      # Копирование файлов из удаленного репозитория
Cloning into 'projects/lab06'...
remote: Enumerating objects: 51, done.
remote: Counting objects: 100% (51/51), done.
remote: Compressing objects: 100% (30/30), done.
remote: Total 51 (delta 15), reused 47 (delta 14), pack-reused 0
Unpacking objects: 100% (51/51), done.
$ cd projects/lab06                                                         # Переход в папку с проектом
$ git remote remove origin                                                  # Удаление ссылки на старый удаленный репозиторий
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab06         # Добавление ссылки на новый удаленный репозиторий
```

```ShellSession
$ gsed -i '/project(print)/a\                         # Дописывание в указанный файл указанной строки
set(PRINT_VERSION_STRING "v\${PRINT_VERSION}")
' CMakeLists.txt
$ gsed -i '/project(print)/a\                         # Дописывание в указанный файл указанной строки
set(PRINT_VERSION\
  \${PRINT_VERSION_MAJOR}.\${PRINT_VERSION_MINOR}.\${PRINT_VERSION_PATCH}.\${PRINT_VERSION_TWEAK})
' CMakeLists.txt
$ gsed -i '/project(print)/a\                         # Дописывание в указанный файл указанной строки
set(PRINT_VERSION_TWEAK 0)
' CMakeLists.txt
$ gsed -i '/project(print)/a\                         # Дописывание в указанный файл указанной строки
set(PRINT_VERSION_PATCH 0)
' CMakeLists.txt
$ gsed -i '/project(print)/a\                         # Дописывание в указанный файл указанной строки
set(PRINT_VERSION_MINOR 1)
' CMakeLists.txt
$ gsed -i '/project(print)/a\                         # Дописывание в указанный файл указанной строки
set(PRINT_VERSION_MAJOR 0)
' CMakeLists.txt
$ git diff                                            # Просмотр отличий локальной версии от последнего коммита
diff --git a/CMakeLists.txt b/CMakeLists.txt
index aa7a323..71b64e3 100644
--- a/CMakeLists.txt
+++ b/CMakeLists.txt
@@ -7,6 +7,13 @@ option(BUILD_EXAMPLES "Build examples" OFF)
 option(BUILD_TESTS "Build tests" OFF)
 
 project(print)
+set(PRINT_VERSION_MAJOR 0)
+set(PRINT_VERSION_MINOR 1)
+set(PRINT_VERSION_PATCH 0)
+set(PRINT_VERSION_TWEAK 0)
+set(PRINT_VERSION
+  ${PRINT_VERSION_MAJOR}.${PRINT_VERSION_MINOR}.${PRINT_VERSION_PATCH}.${PRINT_VERSION_TWEAK})
+set(PRINT_VERSION_STRING "v${PRINT_VERSION}")
 
 add_library(print STATIC ${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
```

```ShellSession
$ touch DESCRIPTION && edit DESCRIPTION                   # Создание и редактирование DESCRIPTION
$ touch ChangeLog.md                                      # Создание ChangeLog.md
$ export DATE="`LANG=en_US date +'%a %b %d %Y'`"          # Создание переменной DATE
$ cat > ChangeLog.md <<EOF                                # Запись в файл ChangeLog.md
* ${DATE} ${GITHUB_USERNAME} <${GITHUB_EMAIL}> 0.1.0.0
- Initial RPM release
EOF
```

```ShellSession
$ cat > CPackConfig.cmake <<EOF                           # Запись в указанный файл указанной строки
include(InstallRequiredSystemLibraries)
EOF
```

```ShellSession
$ cat >> CPackConfig.cmake <<EOF                          # Дописывание в указанный файл указанных строк
set(CPACK_PACKAGE_CONTACT ${GITHUB_EMAIL})
set(CPACK_PACKAGE_VERSION_MAJOR \${PRINT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR \${PRINT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH \${PRINT_VERSION_PATCH})
set(CPACK_PACKAGE_VERSION_TWEAK \${PRINT_VERSION_TWEAK})
set(CPACK_PACKAGE_VERSION \${PRINT_VERSION})
set(CPACK_PACKAGE_DESCRIPTION_FILE \${CMAKE_CURRENT_SOURCE_DIR}/DESCRIPTION)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "static C++ library for printing")
EOF
```

```ShellSession
$ cat >> CPackConfig.cmake <<EOF                         # Дописывание в указанный файл указанных строк

set(CPACK_RESOURCE_FILE_LICENSE \${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
set(CPACK_RESOURCE_FILE_README \${CMAKE_CURRENT_SOURCE_DIR}/README.md)
EOF
```

```ShellSession
$ cat >> CPackConfig.cmake <<EOF                         # Дописывание в указанный файл указанных строк

set(CPACK_RPM_PACKAGE_NAME "print-devel")
set(CPACK_RPM_PACKAGE_LICENSE "MIT")
set(CPACK_RPM_PACKAGE_GROUP "print")
set(CPACK_RPM_CHANGELOG_FILE \${CMAKE_CURRENT_SOURCE_DIR}/ChangeLog.md)
set(CPACK_RPM_PACKAGE_RELEASE 1)
EOF
```

```ShellSession
$ cat >> CPackConfig.cmake <<EOF                         # Дописывание в указанный файл указанных строк

set(CPACK_DEBIAN_PACKAGE_NAME "libprint-dev")
set(CPACK_DEBIAN_PACKAGE_PREDEPENDS "cmake >= 3.0")
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)
EOF
```

```ShellSession
$ cat >> CPackConfig.cmake <<EOF                         # Дописывание в указанный файл указанных строк

include(CPack)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF                         # Дописывание в указанный файл указанных строк

include(CPackConfig.cmake)
EOF
```

```ShellSession
$ gsed -i 's/lab05/lab06/g' README.md                 # Замена левого набора символов на правый
```

```ShellSession
$ git add .                                           # Фиксация изменений
$ git commit -m"added cpack config"                   # Коммит зафиксированных изменений
[master 3fe39fc] added cpack config
 5 files changed, 51 insertions(+), 21 deletions(-)
 create mode 100644 CPackConfig.cmake
 create mode 100644 ChangeLog.md
 create mode 100644 DESCRIPTION
$ git tag v0.1.0.0                                    # Установка тэга
$ git push origin master –tags                        # Отправка изменений
Username for 'https://github.com': CERZAR
Password for 'https://CERZAR@github.com': 
Enumerating objects: 57, done.
Counting objects: 100% (57/57), done.
Delta compression using up to 4 threads
Compressing objects: 100% (50/50), done.
Writing objects: 100% (57/57), 28.10 KiB | 3.51 MiB/s, done.
Total 57 (delta 17), reused 0 (delta 0)
remote: Resolving deltas: 100% (17/17), done.
To https://github.com/CERZAR/lab06
 * [new branch]      master -> master
 * [new tag]         v0.1.0.0 -> v0.1.0.0
```

```ShellSession
$ travis login –auto                                      # Авторизация в Travis CI
Successfully logged in as CERZAR!
$ travis enable                                           # Включение обработки в Travis CI
Detected repository as CERZAR/lab06, is this correct? |yes| yes
CERZAR/lab06: enabled :)
```

```ShellSession
$ cmake -H. -B_build                                      # Сборка (CMakeLists.txt берется из текущей директории, сборка в директорию _build)
-- The C compiler identification is GNU 8.3.0
-- The CXX compiler identification is GNU 8.3.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/cezar/CERZAR/workspace/projects/lab06/_build
$ cmake --build _build                                   # Компиляция в директории _build
Scanning dependencies of target print
[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print
$ cd _build                                               # Переход в указанную директорию
$ cpack -G "TGZ"                                          # Упаковка с использованием CPack
CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print
CPack: Create package
CPack: - package: /home/cezar/CERZAR/workspace/projects/lab06/_build/print-0.1.0.0-Linux.tar.gz generated.
$ cd ..                                                   # Переход на уровень выше
```

```ShellSession
$ cmake -H. -B_build -DCPACK_GENERATOR="TGZ"              # Сборка (CMakeLists.txt берется из текущей директории, сборка в директорию _build). Установка
-- Configuring done
-- Generating done
-- Build files have been written to: /home/cezar/CERZAR/workspace/projects/lab06/_build
$ cmake --build _build --target package                   # Компиляция цели package
[100%] Built target print
Run CPack packaging tool...
CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print
CPack: Create package
CPack: - package: /home/cezar/CERZAR/workspace/projects/lab06/_build/print-0.1.0.0-Linux.tar.gz generated.
```

```ShellSession
$ mkdir artifacts                                         # Создание указанной папки
$ mv _build/*.tar.gz artifacts                            # Перемещение собранного пакета
$ tree artifacts                                          # Отображения дерева указанной папки
artifacts
└── print-0.1.0.0-Linux.tar.gz

0 directories, 1 file
```

## Report

```ShellSession
$ popd
$ export LAB_NUMBER=06
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Homework

После того, как вы настроили взаимодействие с системой непрерывной интеграции,</br>
обеспечив автоматическую сборку и тестирование ваших изменений, стоит задуматься</br>
о создание пакетов для измениний, которые помечаются тэгами (см. вкладку [releases](https://github.com/tp-labs/lab06/releases)).</br>
Пакет должен содержать приложение _solver_ из [предыдущего задания](https://github.com/tp-labs/lab03#задание-1)
Таким образом, каждый новый релиз будет состоять из следующих компонентов:
- архивы с файлами исходного кода (`.tar.gz`, `.zip`)
- пакеты с бинарным файлом _solver_ (`.deb`, `.rpm`, `.msi`, `.dmg`)

В качестве подсказки:
```bash
$ cat .travis.yml
os: osx
script:
...
- cpack -G DragNDrop # dmg

$ cat .travis.yml
os: linux
script:
...
- cpack -G DEB # deb

$ cat .travis.yml
os: linux
addons:
  apt:
    packages:
    - rpm
script:
...
- cpack -G RPM # rpm

$ cat appveyor.yml
platform:
- x86
- x64
build_script:
...
- cpack -G WIX # msi
```

Для этого нужно добавить ветвление в конфигурационные файлы для **CI** со следующей логикой:</br>
если **commit** помечен тэгом, то необходимо собрать пакеты (`DEB, RPM, WIX, DragNDrop, ...`) </br>
и разместить их на сервисе **GitHub**. (см. пример для [Travi CI](https://docs.travis-ci.com/user/deployment/releases))</br>

## Links

- [DMG](https://cmake.org/cmake/help/latest/module/CPackDMG.html)
- [DEB](https://cmake.org/cmake/help/latest/module/CPackDeb.html)
- [RPM](https://cmake.org/cmake/help/latest/module/CPackRPM.html)
- [NSIS](https://cmake.org/cmake/help/latest/module/CPackNSIS.html)

```
Copyright (c) 2015-2019 The ISC Authors
```
