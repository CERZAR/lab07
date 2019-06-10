## Laboratory work VII

Данная лабораторная работа посвещена изучению систем управления пакетами на примере **Hunter**

```ShellSession
$ open https://github.com/ruslo/hunter
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab07** на сервисе **GitHub**
- [x] 2. Выполнить инструкцию учебного материала
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=CERZAR         # Установка переменной GITHUB_USERNAME
# Добавлен синоним из прошлой лабораторной работы
$ alias gsed=sed                        # Синоним gsed
```

```ShellSession
$ cd ${GITHUB_USERNAME}/workspace       # Переход в указанную директорию
$ pushd .                               # Сохранение директории
~/CERZAR/workspace ~/CERZAR/workspace
$ source scripts/activate               # Выполнение скрипта настройки пространства
```

```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab06 projects/lab07      # Клонирование репозитория в указанную директорию
Cloning into 'projects/lab07'...
remote: Enumerating objects: 60, done.
remote: Counting objects: 100% (60/60), done.
remote: Compressing objects: 100% (36/36), done.
remote: Total 60 (delta 19), reused 55 (delta 17), pack-reused 0
Unpacking objects: 100% (60/60), done.
$ cd projects/lab07                                                         # Переход в директорию
$ git remote remove origin                                                  # Удаление ссылки на удаленный репозиторий из локального
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab07         # Указание новой ссылки на удаленный репозиторий
```

```ShellSession
$ wget https://github.com/hunter-packages/gate/archive/v0.9.0.tar.gz -O /tmp/gate.tar.gz          # Загрузка файла в указанный путь
--2019-06-10 23:28:48--  https://github.com/hunter-packages/gate/archive/v0.9.0.tar.gz
Loaded CA certificate '/etc/ssl/certs/ca-certificates.crt'
Resolving github.com (github.com)... 140.82.118.4
Connecting to github.com (github.com)|140.82.118.4|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://codeload.github.com/hunter-packages/gate/tar.gz/v0.9.0 [following]
--2019-06-10 23:28:50--  https://codeload.github.com/hunter-packages/gate/tar.gz/v0.9.0
Resolving codeload.github.com (codeload.github.com)... 192.30.253.120
Connecting to codeload.github.com (codeload.github.com)|192.30.253.120|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 336289 (328K) [application/x-gzip]
Saving to: ‘/tmp/gate.tar.gz’

/tmp/gate.tar.gz                                 100%[==========================================================================================================>] 328,41K   597KB/s    in 0,6s    

2019-06-10 23:28:51 (597 KB/s) - ‘/tmp/gate.tar.gz’ saved [336289/336289]
$ tar -xf /tmp/gate.tar.gz                                                                      # Разархивирование
$ mkdir -p cmake                                                                                # Создание директории
$ mv gate-0.9.0/cmake/HunterGate.cmake cmake                                                    # Перемещение указанного файла в директорию
$ rm -rf gate-0.9.0                                                                             # Рекурсивное удаление указанной директории
# Оригинальная команда изменена: минимальная версия CMake должна быть 3.0; добавлено экранирование переносов строки
$ gsed -i '/cmake_minimum_required(VERSION 3.0)/a\                                              # Добавление текста после указанной строки
\
include("cmake/HunterGate.cmake")\
huntergate(\
  URL "https://github.com/ruslo/hunter/archive/v0.23.83.tar.gz"\
  SHA1 "12dec078717539eb7b03e6d2a17797cba9be9ba9"\
)
' CMakeLists.txt
```

```ShellSession
$ git rm -rf third-party/gtest                                            # Рекурсивное удаление директории через git
rm 'third-party/gtest'
# Оригинальная команда изменена: добавлено экранирование переносов строки
$ gsed -i '/set(PRINT_VERSION_STRING "v\${PRINT_VERSION}")/a\             # Добавление текста после указанной строки
\
hunter_add_package(GTest)
find_package(GTest CONFIG REQUIRED)
' CMakeLists.txt
$ gsed -i 's/add_subdirectory(third-party\/gtest)//' CMakeLists.txt       # Удаление указанной строки
$ gsed -i 's/gtest_main/GTest::main/' CMakeLists.txt                      # Замена левой строки на правую
```

```ShellSession
$ cmake -H. -B_builds -DBUILD_TESTS=ON                                    # Конфигурирование
-- [hunter] Initializing Hunter workspace (12dec078717539eb7b03e6d2a17797cba9be9ba9)
-- [hunter]   https://github.com/ruslo/hunter/archive/v0.23.83.tar.gz
-- [hunter]   -> /home/cezar/.hunter/_Base/Download/Hunter/0.23.83/12dec07
* Closing connection 0
* Closing connection 1
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
-- [hunter] Calculating Toolchain-SHA1
-- [hunter] Calculating Config-SHA1
-- [hunter] HUNTER_ROOT: /home/cezar/.hunter
-- [hunter] [ Hunter-ID: 12dec07 | Toolchain-ID: 70e2fd0 | Config-ID: 510d5e8 ]
-- [hunter] GTEST_ROOT: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Install (ver.: 1.8.0-hunter-p11)
-- [hunter] Building GTest
loading initial cache file /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/cache.cmake
loading initial cache file /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/args.cmake
-- The C compiler identification is GNU 8.3.0
-- The CXX compiler identification is GNU 8.3.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Build
Scanning dependencies of target GTest-Release
[  6%] Creating directories for 'GTest-Release'
[ 12%] Performing download step (download, verify and extract) for 'GTest-Release'
-- Downloading...
   dst='/home/cezar/.hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
   timeout='none'
-- Using src='https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p11.tar.gz'
-- [download 0% complete]
-- [download 1% complete]
-- [download 2% complete]
-- [download 3% complete]
-- [download 4% complete]
-- [download 5% complete]
-- [download 6% complete]
-- [download 7% complete]
-- [download 8% complete]
-- [download 9% complete]
-- [download 10% complete]
-- [download 11% complete]
-- [download 12% complete]
-- [download 13% complete]
-- [download 14% complete]
-- [download 15% complete]
-- [download 16% complete]
-- [download 17% complete]
-- [download 18% complete]
-- [download 19% complete]
-- [download 20% complete]
-- [download 21% complete]
-- [download 22% complete]
-- [download 23% complete]
-- [download 24% complete]
-- [download 25% complete]
-- [download 26% complete]
-- [download 27% complete]
-- [download 28% complete]
-- [download 29% complete]
-- [download 30% complete]
-- [download 31% complete]
-- [download 32% complete]
-- [download 33% complete]
-- [download 34% complete]
-- [download 35% complete]
-- [download 36% complete]
-- [download 37% complete]
-- [download 38% complete]
-- [download 39% complete]
-- [download 40% complete]
-- [download 41% complete]
-- [download 42% complete]
-- [download 43% complete]
-- [download 44% complete]
-- [download 45% complete]
-- [download 46% complete]
-- [download 47% complete]
-- [download 48% complete]
-- [download 49% complete]
-- [download 50% complete]
-- [download 51% complete]
-- [download 52% complete]
-- [download 53% complete]
-- [download 54% complete]
-- [download 55% complete]
-- [download 56% complete]
-- [download 57% complete]
-- [download 59% complete]
-- [download 61% complete]
-- [download 62% complete]
-- [download 64% complete]
-- [download 66% complete]
-- [download 68% complete]
-- [download 69% complete]
-- [download 71% complete]
-- [download 73% complete]
-- [download 74% complete]
-- [download 76% complete]
-- [download 78% complete]
-- [download 80% complete]
-- [download 81% complete]
-- [download 83% complete]
-- [download 85% complete]
-- [download 86% complete]
-- [download 88% complete]
-- [download 90% complete]
-- [download 92% complete]
-- [download 93% complete]
-- [download 95% complete]
-- [download 97% complete]
-- [download 98% complete]
-- [download 100% complete]
* Closing connection 0
* Closing connection 1
-- verifying file...
       file='/home/cezar/.hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
-- Downloading... done
-- extracting...
     src='/home/cezar/.hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
     dst='/home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 18%] No patch step for 'GTest-Release'
[ 25%] No update step for 'GTest-Release'
[ 31%] Performing configure step for 'GTest-Release'
loading initial cache file /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/cache.cmake
loading initial cache file /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/args.cmake
-- The C compiler identification is GNU 8.3.0
-- The CXX compiler identification is GNU 8.3.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Found PythonInterp: /usr/bin/python (found version "3.7.3") 
-- Looking for pthread.h
-- Looking for pthread.h - found
-- Looking for pthread_create
-- Looking for pthread_create - not found
-- Check if compiler accepts -pthread
-- Check if compiler accepts -pthread - yes
-- Found Threads: TRUE  
-- Configuring done
-- Generating done
-- Build files have been written to: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Build/GTest-Release-prefix/src/GTest-Release-build
[ 37%] Performing build step for 'GTest-Release'
Scanning dependencies of target gtest
Scanning dependencies of target gmock
Scanning dependencies of target gmock_main
[  9%] Building CXX object googlemock/gtest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
[ 18%] Building CXX object googlemock/CMakeFiles/gmock.dir/__/googletest/src/gtest-all.cc.o
[ 27%] Building CXX object googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 36%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/__/googletest/src/gtest-all.cc.o
[ 45%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock-all.cc.o
[ 54%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[ 63%] Linking CXX static library libgtest.a
[ 63%] Built target gtest
Scanning dependencies of target gtest_main
[ 72%] Building CXX object googlemock/gtest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 81%] Linking CXX static library libgmock.a
[ 81%] Built target gmock
[ 90%] Linking CXX static library libgmock_main.a
[ 90%] Built target gmock_main
[100%] Linking CXX static library libgtest_main.a
[100%] Built target gtest_main
[ 43%] Performing install step for 'GTest-Release'
[ 36%] Built target gmock_main
[ 63%] Built target gmock
[ 81%] Built target gtest
[100%] Built target gtest_main
Install the project...
-- Install configuration: "Release"
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/libgmock.a
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/libgmock_main.a
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock-actions.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock-generated-matchers.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock-spec-builders.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/internal
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/internal/gmock-internal-utils.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/internal/gmock-generated-internal-utils.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/internal/gmock-port.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/internal/custom
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/internal/custom/gmock-matchers.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/internal/custom/gmock-port.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/internal/custom/gmock-generated-actions.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock-matchers.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock-cardinalities.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock-more-matchers.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock-generated-actions.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock-generated-nice-strict.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock-generated-function-mockers.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock-more-actions.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/cmake/GMock/GMockConfig.cmake
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/cmake/GMock/GMockConfigVersion.cmake
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/cmake/GMock/GMockTargets.cmake
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/cmake/GMock/GMockTargets-release.cmake
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/libgtest.a
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/libgtest_main.a
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/gtest_prod.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/gtest-param-test.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/gtest-death-test.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/gtest.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/gtest_pred_impl.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/gtest-test-part.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-param-util-generated.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-internal.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-port.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-death-test-internal.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-string.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-linked_ptr.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-tuple.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-type-util.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-param-util.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-port-arch.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-filepath.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/custom
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/custom/gtest-port.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/custom/gtest.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/custom/gtest-printers.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/gtest-printers.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/gtest-spi.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/gtest-typed-test.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/gtest-message.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/cmake/GTest/GTestConfig.cmake
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/cmake/GTest/GTestConfigVersion.cmake
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/cmake/GTest/GTestTargets.cmake
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/cmake/GTest/GTestTargets-release.cmake
loading initial cache file /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/args.cmake
[ 50%] Completed 'GTest-Release'
[ 50%] Built target GTest-Release
Scanning dependencies of target GTest-Debug
[ 56%] Creating directories for 'GTest-Debug'
[ 62%] Performing download step (download, verify and extract) for 'GTest-Debug'
-- verifying file...
       file='/home/cezar/.hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
-- File already exists and hash match (skip download):
  file='/home/cezar/.hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
  SHA1='76c6aec038f7d7258bf5c4f45c4817b34039d285'
-- extracting...
     src='/home/cezar/.hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
     dst='/home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 68%] No patch step for 'GTest-Debug'
[ 75%] No update step for 'GTest-Debug'
[ 81%] Performing configure step for 'GTest-Debug'
loading initial cache file /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/cache.cmake
loading initial cache file /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/args.cmake
-- The C compiler identification is GNU 8.3.0
-- The CXX compiler identification is GNU 8.3.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Found PythonInterp: /usr/bin/python (found version "3.7.3") 
-- Looking for pthread.h
-- Looking for pthread.h - found
-- Looking for pthread_create
-- Looking for pthread_create - not found
-- Check if compiler accepts -pthread
-- Check if compiler accepts -pthread - yes
-- Found Threads: TRUE  
-- Configuring done
-- Generating done
-- Build files have been written to: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Build/GTest-Debug-prefix/src/GTest-Debug-build
[ 87%] Performing build step for 'GTest-Debug'
Scanning dependencies of target gtest
Scanning dependencies of target gmock
Scanning dependencies of target gmock_main
[  9%] Building CXX object googlemock/gtest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
[ 18%] Building CXX object googlemock/CMakeFiles/gmock.dir/__/googletest/src/gtest-all.cc.o
[ 27%] Building CXX object googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 36%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/__/googletest/src/gtest-all.cc.o
[ 45%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock-all.cc.o
[ 54%] Linking CXX static library libgmockd.a
[ 63%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[ 72%] Linking CXX static library libgtestd.a
[ 72%] Built target gmock
[ 72%] Built target gtest
Scanning dependencies of target gtest_main
[ 81%] Building CXX object googlemock/gtest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 90%] Linking CXX static library libgtest_maind.a
[ 90%] Built target gtest_main
[100%] Linking CXX static library libgmock_maind.a
[100%] Built target gmock_main
[ 93%] Performing install step for 'GTest-Debug'
[ 36%] Built target gmock_main
[ 63%] Built target gmock
[ 81%] Built target gtest
[100%] Built target gtest_main
Install the project...
-- Install configuration: "Debug"
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/libgmockd.a
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/libgmock_maind.a
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock-actions.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock-generated-matchers.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock-spec-builders.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/internal
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/internal/gmock-internal-utils.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/internal/gmock-generated-internal-utils.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/internal/gmock-port.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/internal/custom
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/internal/custom/gmock-matchers.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/internal/custom/gmock-port.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/internal/custom/gmock-generated-actions.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock-matchers.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock-cardinalities.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock-more-matchers.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock-generated-actions.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock-generated-nice-strict.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock-generated-function-mockers.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gmock/gmock-more-actions.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/cmake/GMock/GMockConfig.cmake
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/cmake/GMock/GMockConfigVersion.cmake
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/cmake/GMock/GMockTargets.cmake
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/cmake/GMock/GMockTargets-debug.cmake
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/libgtestd.a
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/libgtest_maind.a
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/gtest_prod.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/gtest-param-test.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/gtest-death-test.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/gtest.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/gtest_pred_impl.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/gtest-test-part.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-param-util-generated.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-internal.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-port.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-death-test-internal.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-string.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-linked_ptr.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-tuple.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-type-util.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-param-util.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-port-arch.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/gtest-filepath.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/custom
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/custom/gtest-port.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/custom/gtest.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/internal/custom/gtest-printers.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/gtest-printers.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/gtest-spi.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/gtest-typed-test.h
-- Up-to-date: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/include/gtest/gtest-message.h
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/cmake/GTest/GTestConfig.cmake
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/cmake/GTest/GTestConfigVersion.cmake
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/cmake/GTest/GTestTargets.cmake
-- Installing: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/Install/lib64/cmake/GTest/GTestTargets-debug.cmake
loading initial cache file /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest/args.cmake
[100%] Completed 'GTest-Debug'
[100%] Built target GTest-Debug
-- [hunter] Build step successful (dir: /home/cezar/.hunter/_Base/12dec07/70e2fd0/510d5e8/Build/GTest)
-- [hunter] Cache saved: /home/cezar/.hunter/_Base/Cache/raw/e5d9e498c1511b3f97a4553cb48724a59e71c7ea.tar.bz2
-- Configuring done
-- Generating done
-- Build files have been written to: /home/cezar/CERZAR/workspace/projects/lab07/_builds
$ cmake --build _builds                                                   # Сборка
Scanning dependencies of target print
[ 25%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 50%] Linking CXX static library libprint.a
[ 50%] Built target print
Scanning dependencies of target check
[ 75%] Building CXX object CMakeFiles/check.dir/tests/test1.cpp.o
[100%] Linking CXX executable check
[100%] Built target check
$ cmake --build _builds --target test                                     # Сборка указанной цели (выполнение тестирования)
Running tests...
Test project /home/cezar/CERZAR/workspace/projects/lab07/_builds
    Start 1: check
1/1 Test #1: check ............................   Passed    0.00 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.01 sec
$ ls -la $HOME/.hunter                                                    # Отображение всех файлов в указанной директории с подробностями
total 12
drwxr-xr-x  3 cezar cezar 4096 июн 10 23:40 .
drwx------ 22 cezar cezar 4096 июн 10 23:40 ..
drwxr-xr-x  6 cezar cezar 4096 июн 10 23:41 _Base
```

```ShellSession
$ git clone https://github.com/ruslo/hunter $HOME/projects/hunter                       # Клонирование указанного репозитория в указанную директорию
Cloning into '/home/cezar/projects/hunter'...
remote: Enumerating objects: 41702, done.
remote: Total 41702 (delta 0), reused 0 (delta 0), pack-reused 41702
Receiving objects: 100% (41702/41702), 10.62 MiB | 3.58 MiB/s, done.
Resolving deltas: 100% (26234/26234), done.
$ export HUNTER_ROOT=$HOME/projects/hunter                                              # Установка указанной переменной
$ rm -rf _builds                                                                        # Рекурсивное удаление директории
$ cmake -H. -B_builds -DBUILD_TESTS=ON                                                  # Конфигурирование
-- [hunter] Initializing Hunter workspace (12dec078717539eb7b03e6d2a17797cba9be9ba9)
-- [hunter]   https://github.com/ruslo/hunter/archive/v0.23.83.tar.gz
-- [hunter]   -> /home/cezar/projects/hunter/_Base/Download/Hunter/0.23.83/12dec07
* Closing connection 0
* Closing connection 1
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
-- [hunter] Calculating Toolchain-SHA1
-- [hunter] Calculating Config-SHA1
-- [hunter] HUNTER_ROOT: /home/cezar/projects/hunter
-- [hunter] [ Hunter-ID: 12dec07 | Toolchain-ID: 70e2fd0 | Config-ID: cedd094 ]
-- [hunter] GTEST_ROOT: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Install (ver.: 1.8.0-hunter-p11)
-- [hunter] Building GTest
loading initial cache file /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/cache.cmake
loading initial cache file /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/args.cmake
-- The C compiler identification is GNU 8.3.0
-- The CXX compiler identification is GNU 8.3.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Build
Scanning dependencies of target GTest-Release
[  6%] Creating directories for 'GTest-Release'
[ 12%] Performing download step (download, verify and extract) for 'GTest-Release'
-- Downloading...
   dst='/home/cezar/projects/hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
   timeout='none'
-- Using src='https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p11.tar.gz'
-- [download 0% complete]
-- [download 1% complete]
-- [download 2% complete]
-- [download 3% complete]
-- [download 4% complete]
-- [download 5% complete]
-- [download 6% complete]
-- [download 7% complete]
-- [download 8% complete]
-- [download 9% complete]
-- [download 10% complete]
-- [download 11% complete]
-- [download 12% complete]
-- [download 13% complete]
-- [download 14% complete]
-- [download 15% complete]
-- [download 16% complete]
-- [download 17% complete]
-- [download 18% complete]
-- [download 19% complete]
-- [download 20% complete]
-- [download 21% complete]
-- [download 22% complete]
-- [download 23% complete]
-- [download 24% complete]
-- [download 25% complete]
-- [download 26% complete]
-- [download 27% complete]
-- [download 28% complete]
-- [download 29% complete]
-- [download 30% complete]
-- [download 31% complete]
-- [download 32% complete]
-- [download 33% complete]
-- [download 34% complete]
-- [download 35% complete]
-- [download 36% complete]
-- [download 37% complete]
-- [download 38% complete]
-- [download 39% complete]
-- [download 40% complete]
-- [download 42% complete]
-- [download 44% complete]
-- [download 45% complete]
-- [download 47% complete]
-- [download 49% complete]
-- [download 50% complete]
-- [download 52% complete]
-- [download 54% complete]
-- [download 56% complete]
-- [download 57% complete]
-- [download 59% complete]
-- [download 61% complete]
-- [download 62% complete]
-- [download 64% complete]
-- [download 66% complete]
-- [download 68% complete]
-- [download 69% complete]
-- [download 71% complete]
-- [download 73% complete]
-- [download 74% complete]
-- [download 76% complete]
-- [download 78% complete]
-- [download 80% complete]
-- [download 81% complete]
-- [download 83% complete]
-- [download 85% complete]
-- [download 86% complete]
-- [download 88% complete]
-- [download 90% complete]
-- [download 92% complete]
-- [download 93% complete]
-- [download 95% complete]
-- [download 97% complete]
-- [download 98% complete]
-- [download 100% complete]
* Closing connection 0
* Closing connection 1
-- verifying file...
       file='/home/cezar/projects/hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
-- Downloading... done
-- extracting...
     src='/home/cezar/projects/hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
     dst='/home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 18%] No patch step for 'GTest-Release'
[ 25%] No update step for 'GTest-Release'
[ 31%] Performing configure step for 'GTest-Release'
loading initial cache file /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/cache.cmake
loading initial cache file /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/args.cmake
-- The C compiler identification is GNU 8.3.0
-- The CXX compiler identification is GNU 8.3.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Found PythonInterp: /usr/bin/python (found version "3.7.3") 
-- Looking for pthread.h
-- Looking for pthread.h - found
-- Looking for pthread_create
-- Looking for pthread_create - not found
-- Check if compiler accepts -pthread
-- Check if compiler accepts -pthread - yes
-- Found Threads: TRUE  
-- Configuring done
-- Generating done
-- Build files have been written to: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Build/GTest-Release-prefix/src/GTest-Release-build
[ 37%] Performing build step for 'GTest-Release'
Scanning dependencies of target gtest
Scanning dependencies of target gmock_main
Scanning dependencies of target gmock
[  9%] Building CXX object googlemock/gtest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
[ 18%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/__/googletest/src/gtest-all.cc.o
[ 36%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock-all.cc.o
[ 36%] Building CXX object googlemock/CMakeFiles/gmock.dir/__/googletest/src/gtest-all.cc.o
[ 45%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[ 54%] Building CXX object googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 63%] Linking CXX static library libgtest.a
[ 63%] Built target gtest
[ 72%] Linking CXX static library libgmock_main.a
Scanning dependencies of target gtest_main
[ 81%] Building CXX object googlemock/gtest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 81%] Built target gmock_main
[ 90%] Linking CXX static library libgmock.a
[ 90%] Built target gmock
[100%] Linking CXX static library libgtest_main.a
[100%] Built target gtest_main
[ 43%] Performing install step for 'GTest-Release'
[ 36%] Built target gmock_main
[ 63%] Built target gmock
[ 81%] Built target gtest
[100%] Built target gtest_main
Install the project...
-- Install configuration: "Release"
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/libgmock.a
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/libgmock_main.a
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock-actions.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock-generated-matchers.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock-spec-builders.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/internal
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/internal/gmock-internal-utils.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/internal/gmock-generated-internal-utils.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/internal/gmock-port.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/internal/custom
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/internal/custom/gmock-matchers.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/internal/custom/gmock-port.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/internal/custom/gmock-generated-actions.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock-matchers.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock-cardinalities.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock-more-matchers.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock-generated-actions.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock-generated-nice-strict.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock-generated-function-mockers.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock-more-actions.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/cmake/GMock/GMockConfig.cmake
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/cmake/GMock/GMockConfigVersion.cmake
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/cmake/GMock/GMockTargets.cmake
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/cmake/GMock/GMockTargets-release.cmake
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/libgtest.a
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/libgtest_main.a
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/gtest_prod.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/gtest-param-test.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/gtest-death-test.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/gtest.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/gtest_pred_impl.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/gtest-test-part.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-param-util-generated.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-internal.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-port.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-death-test-internal.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-string.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-linked_ptr.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-tuple.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-type-util.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-param-util.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-port-arch.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-filepath.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/custom
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/custom/gtest-port.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/custom/gtest.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/custom/gtest-printers.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/gtest-printers.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/gtest-spi.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/gtest-typed-test.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/gtest-message.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/cmake/GTest/GTestConfig.cmake
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/cmake/GTest/GTestConfigVersion.cmake
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/cmake/GTest/GTestTargets.cmake
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/cmake/GTest/GTestTargets-release.cmake
loading initial cache file /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/args.cmake
[ 50%] Completed 'GTest-Release'
[ 50%] Built target GTest-Release
Scanning dependencies of target GTest-Debug
[ 56%] Creating directories for 'GTest-Debug'
[ 62%] Performing download step (download, verify and extract) for 'GTest-Debug'
-- verifying file...
       file='/home/cezar/projects/hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
-- File already exists and hash match (skip download):
  file='/home/cezar/projects/hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
  SHA1='76c6aec038f7d7258bf5c4f45c4817b34039d285'
-- extracting...
     src='/home/cezar/projects/hunter/_Base/Download/GTest/1.8.0-hunter-p11/76c6aec/1.8.0-hunter-p11.tar.gz'
     dst='/home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 68%] No patch step for 'GTest-Debug'
[ 75%] No update step for 'GTest-Debug'
[ 81%] Performing configure step for 'GTest-Debug'
loading initial cache file /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/cache.cmake
loading initial cache file /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/args.cmake
-- The C compiler identification is GNU 8.3.0
-- The CXX compiler identification is GNU 8.3.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Found PythonInterp: /usr/bin/python (found version "3.7.3") 
-- Looking for pthread.h
-- Looking for pthread.h - found
-- Looking for pthread_create
-- Looking for pthread_create - not found
-- Check if compiler accepts -pthread
-- Check if compiler accepts -pthread - yes
-- Found Threads: TRUE  
-- Configuring done
-- Generating done
-- Build files have been written to: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Build/GTest-Debug-prefix/src/GTest-Debug-build
[ 87%] Performing build step for 'GTest-Debug'
Scanning dependencies of target gtest
Scanning dependencies of target gmock
Scanning dependencies of target gmock_main
[  9%] Building CXX object googlemock/gtest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
[ 18%] Building CXX object googlemock/CMakeFiles/gmock.dir/__/googletest/src/gtest-all.cc.o
[ 27%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/__/googletest/src/gtest-all.cc.o
[ 36%] Building CXX object googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 45%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock-all.cc.o
[ 54%] Linking CXX static library libgtestd.a
[ 63%] Linking CXX static library libgmockd.a
[ 72%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[ 72%] Built target gmock
[ 72%] Built target gtest
Scanning dependencies of target gtest_main
[ 81%] Building CXX object googlemock/gtest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 90%] Linking CXX static library libgtest_maind.a
[ 90%] Built target gtest_main
[100%] Linking CXX static library libgmock_maind.a
[100%] Built target gmock_main
[ 93%] Performing install step for 'GTest-Debug'
[ 36%] Built target gmock_main
[ 63%] Built target gmock
[ 81%] Built target gtest
[100%] Built target gtest_main
Install the project...
-- Install configuration: "Debug"
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/libgmockd.a
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/libgmock_maind.a
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock-actions.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock-generated-matchers.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock-spec-builders.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/internal
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/internal/gmock-internal-utils.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/internal/gmock-generated-internal-utils.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/internal/gmock-port.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/internal/custom
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/internal/custom/gmock-matchers.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/internal/custom/gmock-port.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/internal/custom/gmock-generated-actions.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock-matchers.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock-cardinalities.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock-more-matchers.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock-generated-actions.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock-generated-nice-strict.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock-generated-function-mockers.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gmock/gmock-more-actions.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/cmake/GMock/GMockConfig.cmake
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/cmake/GMock/GMockConfigVersion.cmake
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/cmake/GMock/GMockTargets.cmake
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/cmake/GMock/GMockTargets-debug.cmake
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/libgtestd.a
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/libgtest_maind.a
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/gtest_prod.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/gtest-param-test.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/gtest-death-test.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/gtest.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/gtest_pred_impl.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/gtest-test-part.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-param-util-generated.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-internal.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-port.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-death-test-internal.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-string.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-linked_ptr.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-tuple.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-type-util.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-param-util.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-port-arch.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/gtest-filepath.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/custom
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/custom/gtest-port.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/custom/gtest.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/internal/custom/gtest-printers.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/gtest-printers.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/gtest-spi.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/gtest-typed-test.h
-- Up-to-date: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/include/gtest/gtest-message.h
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/cmake/GTest/GTestConfig.cmake
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/cmake/GTest/GTestConfigVersion.cmake
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/cmake/GTest/GTestTargets.cmake
-- Installing: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/Install/lib64/cmake/GTest/GTestTargets-debug.cmake
loading initial cache file /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest/args.cmake
[100%] Completed 'GTest-Debug'
[100%] Built target GTest-Debug
-- [hunter] Build step successful (dir: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Build/GTest)
-- [hunter] Cache saved: /home/cezar/projects/hunter/_Base/Cache/raw/cf9469a84a14691e75b59c78e0c94688b65956e7.tar.bz2
-- Configuring done
-- Generating done
-- Build files have been written to: /home/cezar/CERZAR/workspace/projects/lab07/_builds
$ cmake --build _builds                                                     # Сборка
Scanning dependencies of target print
[ 25%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 50%] Linking CXX static library libprint.a
[ 50%] Built target print
Scanning dependencies of target check
[ 75%] Building CXX object CMakeFiles/check.dir/tests/test1.cpp.o
[100%] Linking CXX executable check
[100%] Built target check
$ cmake --build _builds --target test                                       # Сборка указанной цели
Running tests...
Test project /home/cezar/CERZAR/workspace/projects/lab07/_builds
    Start 1: check
1/1 Test #1: check ............................   Passed    0.00 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.01 sec
```

```ShellSession
$ cat $HUNTER_ROOT/cmake/configs/default.cmake | grep GTest               # Вывод в консоль указанного файла. Вывод только строк, содержащих подстроку GTest
  hunter_default_version(GTest VERSION 1.7.0-hunter-6)
  hunter_default_version(GTest VERSION 1.8.0-hunter-p11)
$ cat $HUNTER_ROOT/cmake/projects/GTest/hunter.cmake                      # Вывод в консоль указанного файла
# Copyright (c) 2013, Ruslan Baratov
# All rights reserved.

# !!! DO NOT PLACE HEADER GUARDS HERE !!!

include(hunter_add_version)
include(hunter_cacheable)
include(hunter_download)
include(hunter_pick_scheme)
include(hunter_cmake_args)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter.tar.gz"
    SHA1
    1ed1c26d11fb592056c1cb912bd3c784afa96eaa
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-1"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-1.tar.gz"
    SHA1
    0cb1dcf75e144ad052d3f1e4923a7773bf9b494f
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-2"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-2.tar.gz"
    SHA1
    e62b2ef70308f63c32c560f7b6e252442eed4d57
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-3"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-3.tar.gz"
    SHA1
    fea7d3020e20f059255484c69755753ccadf6362
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-4"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-4.tar.gz"
    SHA1
    9b439c0c25437a083957b197ac6905662a5d901b
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-5"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-5.tar.gz"
    SHA1
    796804df3facb074087a4d8ba6f652e5ac69ad7f
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-6"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-6.tar.gz"
    SHA1
    64b93147abe287da8fe4e18cfd54ba9297dafb82
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-7"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-7.tar.gz"
    SHA1
    19b5c98747768bcd0622714f2ed40f17aee406b2
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-8"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-8.tar.gz"
    SHA1
    ac4d2215aa1b1d745a096e5e3b2dbe0c0f229ea5
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-9"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-9.tar.gz"
    SHA1
    8a47fe9c4e550f4ed0e2c05388dd291a059223d9
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-10"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-10.tar.gz"
    SHA1
    374e6dbe8619ab467c6b1a0b470a598407b172e9
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.7.0-hunter-11"
    URL
    "https://github.com/hunter-packages/gtest/archive/v1.7.0-hunter-11.tar.gz"
    SHA1
    c6ae948ca2bea1d734af01b1069491b00933ed31
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    1.8.0-hunter-p2
    URL
    "https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p2.tar.gz"
    SHA1
    93148cb8850abe78b76ed87158fdb6b9c48e38c4
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    1.8.0-hunter-p5
    URL https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p5.tar.gz
    SHA1 3325aa4fc8b30e665c9f73a60f19387b7db36f85
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    1.8.0-hunter-p6
    URL
    "https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p6.tar.gz"
    SHA1
    f57096bd01c6f8cbef043b312d4d1e82f29648b6
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    1.8.0-hunter-p7
    URL
    "https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p7.tar.gz"
    SHA1
    4fe083a96d7597f7dce6f453dca01e1d94a1e45b
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    1.8.0-hunter-p8
    URL
    "https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p8.tar.gz"
    SHA1
    1cdd396b20c8d29f7ea08baaa49673b1c261f545
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    1.8.0-hunter-p9
    URL
    "https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p9.tar.gz"
    SHA1
    a345f16cb610e0b5dfa7778dc2852b784cfede5b
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    1.8.0-hunter-p10
    URL
    "https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p10.tar.gz"
    SHA1
    1d92c9f51af756410843b13f8c4e4df09e235394
)

hunter_add_version(
    PACKAGE_NAME
    GTest
    VERSION
    "1.8.0-hunter-p11"
    URL
    "https://github.com/hunter-packages/googletest/archive/1.8.0-hunter-p11.tar.gz"
    SHA1
    76c6aec038f7d7258bf5c4f45c4817b34039d285
)

if(HUNTER_GTest_VERSION VERSION_LESS 1.8.0)
  set(_gtest_license "LICENSE")
else()
  set(_gtest_license "googletest/LICENSE")
endif()

hunter_cmake_args(
    GTest
    CMAKE_ARGS
    HUNTER_INSTALL_LICENSE_FILES=${_gtest_license}
)

hunter_pick_scheme(DEFAULT url_sha1_cmake)
hunter_cacheable(GTest)
hunter_download(PACKAGE_NAME GTest PACKAGE_INTERNAL_DEPS_ID 1)
$ mkdir cmake/Hunter                                                  # Создание директории
$ cat > cmake/Hunter/config.cmake <<EOF                               # Запись в файл
hunter_config(GTest VERSION 1.7.0-hunter-9)
EOF
```

```ShellSession
$ git submodule add github.com/ruslo/polly tools/polly                # Добавление подмодуля git
Cloning into '/home/cezar/CERZAR/workspace/projects/lab07/tools/polly'...
remote: Enumerating objects: 42, done.
remote: Counting objects: 100% (42/42), done.
remote: Compressing objects: 100% (26/26), done.
remote: Total 6147 (delta 23), reused 30 (delta 16), pack-reused 6105
Receiving objects: 100% (6147/6147), 1.57 MiB | 1.77 MiB/s, done.
Resolving deltas: 100% (4204/4204), done.
# Добавлен к команде проброс флага -DBUILD_TESTS=ON
$ tools/polly/bin/polly.py --test --fwd BUILD_TESTS=ON                   # Сборка и тестирование через утилиту polly
Python version: 3.7
Build dir: /home/cezar/CERZAR/workspace/projects/lab07/_builds/default
Execute command: [
  `which`
  `cmake`
]

[/home/cezar/CERZAR/workspace/projects/lab07]> "which" "cmake"

/usr/bin/cmake
Execute command: [
  `cmake`
  `--version`
]

[/home/cezar/CERZAR/workspace/projects/lab07]> "cmake" "--version"

cmake version 3.14.4

CMake suite maintained and supported by Kitware (kitware.com/cmake).
Execute command: [
  `cmake`
  `-H.`
  `-B/home/cezar/CERZAR/workspace/projects/lab07/_builds/default`
  `-DCMAKE_TOOLCHAIN_FILE=/home/cezar/CERZAR/workspace/projects/lab07/tools/polly/default.cmake`
  `-DBUILD_TESTS=ON`
]

[/home/cezar/CERZAR/workspace/projects/lab07]> "cmake" "-H." "-B/home/cezar/CERZAR/workspace/projects/lab07/_builds/default" "-DCMAKE_TOOLCHAIN_FILE=/home/cezar/CERZAR/workspace/projects/lab07/tools/polly/default.cmake" "-DBUILD_TESTS=ON"

-- [polly] Used toolchain: Default
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
-- [hunter] Calculating Toolchain-SHA1
-- [hunter] Calculating Config-SHA1
-- [hunter] HUNTER_ROOT: /home/cezar/projects/hunter
-- [hunter] [ Hunter-ID: 12dec07 | Toolchain-ID: 70e2fd0 | Config-ID: cedd094 ]
-- [hunter] GTEST_ROOT: /home/cezar/projects/hunter/_Base/12dec07/70e2fd0/cedd094/Install (ver.: 1.8.0-hunter-p11)
-- Configuring done
-- Generating done
-- Build files have been written to: /home/cezar/CERZAR/workspace/projects/lab07/_builds/default
Execute command: [
  `cmake`
  `--build`
  `/home/cezar/CERZAR/workspace/projects/lab07/_builds/default`
  `--`
]

[/home/cezar/CERZAR/workspace/projects/lab07]> "cmake" "--build" "/home/cezar/CERZAR/workspace/projects/lab07/_builds/default" "--"

Scanning dependencies of target print
[ 25%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 50%] Linking CXX static library libprint.a
[ 50%] Built target print
Scanning dependencies of target check
[ 75%] Building CXX object CMakeFiles/check.dir/tests/test1.cpp.o
[100%] Linking CXX executable check
[100%] Built target check
Run tests
Execute command: [
  `ctest`
]

[/home/cezar/CERZAR/workspace/projects/lab07/_builds/default]> "ctest"

Test project /home/cezar/CERZAR/workspace/projects/lab07/_builds/default
    Start 1: check
1/1 Test #1: check ............................   Passed    0.00 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.00 sec
-
Log saved: /home/cezar/CERZAR/workspace/projects/lab07/_logs/polly/default/log.txt
-
Generate: 0:00:05.950403s
Build: 0:00:02.711895s
Test: 0:00:00.014052s
-
Total: 0:00:08.676575s
-
SUCCESS
```

```ShellSession
$ git status                                                      # Статус локального репозитория
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   .gitmodules
        deleted:    third-party/gtest
        new file:   tools/polly

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   CMakeLists.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        _logs/
        cmake/
$ git commit -m "Updated modules"                                 # Коммит зафиксированных изменений
[master bef1f3c] Updated modules
 3 files changed, 4 insertions(+), 4 deletions(-)
 delete mode 160000 third-party/gtest
 create mode 160000 tools/polly
$ git add cmake                                                   # Фиксация директории
$ git commit -m "Added cmake config files"                        # Коммит
[master c288bdc] Added cmake config files
 2 files changed, 529 insertions(+)
 create mode 100644 cmake/Hunter/config.cmake
 create mode 100644 cmake/HunterGate.cmake
$ git add CMakeLists.txt                                          # Фиксация
$ git commit -m "Added hunter gate and GTest to CMakeLists.txt"   # Коммит
[master a3a465f] Added hunter gate and GTest to CMakeLists.txt
 1 file changed, 13 insertions(+), 3 deletions(-)
$ git push origin master                                          # Отправка изменений в удаленный репозиторий
Username for 'https://github.com': CERZAR
Password for 'https://CERZAR@github.com': 
Enumerating objects: 73, done.
Counting objects: 100% (73/73), done.
Delta compression using up to 4 threads
Compressing objects: 100% (63/63), done.
Writing objects: 100% (73/73), 38.86 KiB | 5.55 MiB/s, done.
Total 73 (delta 22), reused 0 (delta 0)
remote: Resolving deltas: 100% (22/22), done.
To https://github.com/CERZAR/lab07
 * [new branch]      master -> master
```

## Report

```ShellSession
$ popd
$ export LAB_NUMBER=07
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Homework

### Задание
1. Создайте cвой hunter-пакет.

## Links

- [Create Hunter package](https://docs.hunter.sh/en/latest/creating-new/create.html)
- [Custom Hunter config](https://github.com/ruslo/hunter/wiki/example.custom.config.id)
- [Polly](https://github.com/ruslo/polly)

```
Copyright (c) 2015-2019 The ISC Authors
```
