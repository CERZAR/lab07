[![Build Status](https://travis-ci.org/CERZAR/lab05.svg?branch=master)](https://travis-ci.org/CERZAR/lab05)

## Laboratory work V

Данная лабораторная работа посвещена изучению фреймворков для тестирования на примере **GTest**

```ShellSession
$ open https://github.com/google/googletest
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab05** на сервисе **GitHub**
- [x] 2. Выполнить инструкцию учебного материала
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=CERZAR        # Переменная окружения
$ alias gsed=sed # for *-nix system    # Синоним команды gsed
```

```ShellSession
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
~/CERZAR/workspace ~/CERZAR/workspace
$ source scripts/activate
```

```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab04 projects/lab05           # Скачивание из удаленного репозитория в указанную папку
Cloning into 'projects/lab05'...
remote: Enumerating objects: 37, done.
remote: Counting objects: 100% (37/37), done.
remote: Compressing objects: 100% (26/26), done.
remote: Total 37 (delta 9), reused 30 (delta 6), pack-reused 0
Unpacking objects: 100% (37/37), done.
$ cd projects/lab05                                                              # Переход в созданную папку
$ git remote remove origin                                                       # Удаление ссылки на удаленный репозиторий из локального
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab05              # Указание новой ссылки на удаленный репозиторий
```

```ShellSession
$ mkdir third-party                                                              # Создание папки
$ git submodule add https://github.com/google/googletest third-party/gtest       # Скачивание удаленного репозитория в указанную папку
Cloning into '/home/cezar/CERZAR/workspace/projects/lab05/third-party/gtest'...
remote: Enumerating objects: 16892, done.
remote: Total 16892 (delta 0), reused 0 (delta 0), pack-reused 16892
Receiving objects: 100% (16892/16892), 5.96 MiB | 1.38 MiB/s, done.
Resolving deltas: 100% (12445/12445), done.
$ cd third-party/gtest && git checkout release-1.8.1 && cd ../..                 # Переход в указанную папку, переход в указанную ветку, возврат
Note: checking out 'release-1.8.1'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD is now at 2fe3bd99 Merge pull request #1433 from dsacre/fix-clang-warnings
$ git add third-party/gtest                                                    # Фиксация изменений
$ git commit -m"added gtest framework"                                         # Коммит зафиксированных изменений
[master 1466b76] added gtest framework
 2 files changed, 4 insertions(+)
 create mode 100644 .gitmodules
 create mode 160000 third-party/gtest
```

```ShellSession
$ gsed -i '/option(BUILD_EXAMPLES "Build examples" OFF)/a\                    # Вставить вторую строку после указанной первой строки
option(BUILD_TESTS "Build tests" OFF)
' CMakeLists.txt
$ cat >> CMakeLists.txt <<EOF                                                 # Дописывание в CMakeLists.txt указанного кода

if(BUILD_TESTS)
  enable_testing()
  add_subdirectory(third-party/gtest)
  file(GLOB \${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
  add_executable(check \${\${PROJECT_NAME}_TEST_SOURCES})
  target_link_libraries(check \${PROJECT_NAME} gtest_main)
  add_test(NAME check COMMAND check)
endif()
EOF
```

```ShellSession
$ mkdir tests                                                                 # Создание указанной папки
$ cat > tests/test1.cpp <<EOF                                                 # Создание указанного файла с указанным кодом
#include <print.hpp>

#include <gtest/gtest.h>

TEST(Print, InFileStream)
{
  std::string filepath = "file.txt";
  std::string text = "hello";
  std::ofstream out{filepath};

  print(text, out);
  out.close();

  std::string result;
  std::ifstream in{filepath};
  in >> result;

  EXPECT_EQ(result, text);
}
EOF
```

```ShellSession
$ cmake -H. -B_build -DBUILD_TESTS=ON                                      # Этап конфигурирование
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
-- Build files have been written to: /home/cezar/CERZAR/workspace/projects/lab05/_build
$ cmake --build _build                                                  # Компиляция
Scanning dependencies of target gtest
[  8%] Building CXX object third-party/gtest/googlemock/gtest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
[ 16%] Linking CXX static library libgtest.a
[ 16%] Built target gtest
Scanning dependencies of target print
[ 25%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 33%] Linking CXX static library libprint.a
[ 33%] Built target print
Scanning dependencies of target gtest_main
[ 41%] Building CXX object third-party/gtest/googlemock/gtest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 50%] Linking CXX static library libgtest_main.a
[ 50%] Built target gtest_main
Scanning dependencies of target check
[ 58%] Building CXX object CMakeFiles/check.dir/tests/test1.cpp.o
[ 66%] Linking CXX executable check
[ 66%] Built target check
Scanning dependencies of target gmock
[ 75%] Building CXX object third-party/gtest/googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 83%] Linking CXX static library libgmock.a
[ 83%] Built target gmock
Scanning dependencies of target gmock_main
[ 91%] Building CXX object third-party/gtest/googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[100%] Linking CXX static library libgmock_main.a
[100%] Built target gmock_main
$ cmake --build _build --target test                                 # Компиляция указанной цели
Running tests...
Test project /home/cezar/CERZAR/workspace/projects/lab05/_build
    Start 1: check
1/1 Test #1: check ............................   Passed    0.00 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.01 sec
```

```ShellSession
$ _build/check                                                       # Выполнение исполняемого файла с тестами
Running main() from /home/cezar/CERZAR/workspace/projects/lab05/third-party/gtest/googletest/src/gtest_main.cc
[==========] Running 1 test from 1 test case.
[----------] Global test environment set-up.
[----------] 1 test from Print
[ RUN      ] Print.InFileStream
[       OK ] Print.InFileStream (0 ms)
[----------] 1 test from Print (0 ms total)

[----------] Global test environment tear-down
[==========] 1 test from 1 test case ran. (0 ms total)
[  PASSED  ] 1 test.
$ cmake --build _build --target test -- ARGS=--verbose              # Компиляция с выводом всей информации
Running tests...
UpdateCTestConfiguration  from :/home/cezar/CERZAR/workspace/projects/lab05/_build/DartConfiguration.tcl
UpdateCTestConfiguration  from :/home/cezar/CERZAR/workspace/projects/lab05/_build/DartConfiguration.tcl
Test project /home/cezar/CERZAR/workspace/projects/lab05/_build
Constructing a list of tests
Done constructing a list of tests
Updating test list for fixtures
Added 0 tests to meet fixture requirements
Checking test dependency graph...
Checking test dependency graph end
test 1
    Start 1: check

1: Test command: /home/cezar/CERZAR/workspace/projects/lab05/_build/check
1: Test timeout computed to be: 10000000
1: Running main() from /home/cezar/CERZAR/workspace/projects/lab05/third-party/gtest/googletest/src/gtest_main.cc
1: [==========] Running 1 test from 1 test case.
1: [----------] Global test environment set-up.
1: [----------] 1 test from Print
1: [ RUN      ] Print.InFileStream
1: [       OK ] Print.InFileStream (0 ms)
1: [----------] 1 test from Print (0 ms total)
1: 
1: [----------] Global test environment tear-down
1: [==========] 1 test from 1 test case ran. (0 ms total)
1: [  PASSED  ] 1 test.
1/1 Test #1: check ............................   Passed    0.00 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.00 sec
```

```ShellSession
$ gsed -i 's/lab04/lab05/g' README.md                                                        # Замена левой строки на правую
$ gsed -i 's/\(DCMAKE_INSTALL_PREFIX=_install\)/\1 -DBUILD_TESTS=ON/' .travis.yml            # Дописывание к найденной по вхождению левой строки строке правой строки
$ gsed -i '/cmake --build _build --target install/a\                                         # Дописывание правой строки после найденной левой строки
- cmake --build _build --target test -- ARGS=--verbose
' .travis.yml
```

```ShellSession
$ travis lint                                            # Проверка
[x] value for addons section is empty, dropping
[x] in addons section: unexpected key apt, dropping
```

```ShellSession
$ git add .travis.yml                                    # Фиксация указанного файла
$ git add tests                                          # Фиксация указанного файла
$ git add –p                                             # Фиксация указанного файла
diff --git a/README.md b/README.md
index 85b7a74..fb4c6cd 100644
--- a/README.md
+++ b/README.md
@@ -11,7 +11,7 @@ $ open https://travis-ci.org
 ## Tasks
 
 - [x] 1. Авторизоваться на сервисе **Travis CI** с использованием **GitHub** аккаунта
-- [x] 2. Создать публичный репозиторий с названием **lab04** на сервисе **GitHub**
+- [x] 2. Создать публичный репозиторий с названием **lab05** на сервисе **GitHub**
 - [x] 3. Ознакомиться со ссылками учебного материала
 - [x] 4. Включить интеграцию сервиса **Travis CI** с созданным репозиторием
 - [x] 5. Получить токен для **Travis CLI** с правами **repo** и **user**
Stage this hunk [y,n,q,a,d,j,J,g,/,e,?]? a
$ git commit -m"added tests"                             # Коммит зафиксированных изменений
[master 14a6f08] added tests
 4 files changed, 44 insertions(+), 14 deletions(-)
 create mode 100644 tests/test1.cpp
$ git push origin master                                 # Отправка изменений в удаленный репозиторий
Username for 'https://github.com': CERZAR
Password for 'https://CERZAR@github.com': 
Enumerating objects: 48, done.
Counting objects: 100% (48/48), done.
Delta compression using up to 4 threads
Compressing objects: 100% (41/41), done.
Writing objects: 100% (48/48), 21.39 KiB | 3.06 MiB/s, done.
Total 48 (delta 14), reused 0 (delta 0)
remote: Resolving deltas: 100% (14/14), done.
To https://github.com/CERZAR/lab05
 * [new branch]      master -> master
```

```ShellSession
$ travis login –auto                                     # Авторизация
We need your GitHub login to identify you.
This information will not be sent to Travis CI, only to api.github.com.
The password will not be displayed.

Try running with --github-token or --auto if you don't want to enter your password anyway.

Username: CERZAR
Password for CERZAR: ********
Successfully logged in as CERZAR!
$ travis enable                                          # Включение непрерывной интеграции для репозитория
Detected repository as CERZAR/lab05, is this correct? |yes| yes
CERZAR/lab05: enabled :)
```

```ShellSession
$ mkdir artifacts                                        # Создание директории
$ sleep 20s && gnome-screenshot --file artifacts/screenshot.png      
# for macOS: $ screencapture -T 20 artifacts/screenshot.png
# open https://github.com/${GITHUB_USERNAME}/lab05
```

## Report

```ShellSession
$ popd
$ export LAB_NUMBER=05
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Homework

### Задание
1. Создайте `CMakeList.txt` для библиотеки *banking*.
2. Создайте модульные тесты на классы `Transaction` и `Account`.
    * Используйте mock-объекты.
    * Покрытие кода должно составлять 100%.
3. Настройте сборочную процедуру на **TravisCI**.
4. Настройте [Coveralls.io](https://coveralls.io/).

## Links

- [C++ CI: Travis, CMake, GTest, Coveralls & Appveyor](http://david-grs.github.io/cpp-clang-travis-cmake-gtest-coveralls-appveyor/)
- [Boost.Tests](http://www.boost.org/doc/libs/1_63_0/libs/test/doc/html/)
- [Catch](https://github.com/catchorg/Catch2)

```
Copyright (c) 2015-2019 The ISC Authors
```
