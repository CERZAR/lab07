[![Build Status](https://travis-ci.org/CERZAR/lab04.svg?branch=master)](https://travis-ci.org/CERZAR/lab04)
## Laboratory work III

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**

```ShellSession
$ open https://cmake.org/
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab03** на сервисе **GitHub**
- [x] 2. Ознакомиться со ссылками учебного материала
- [x] 3. Выполнить инструкцию учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=CERZAR                   # Установка переменной GITHUB_USERNAME
```

```ShellSession
$ cd ${GITHUB_USERNAME}/workspace                 # Переход в рабочую директорию
$ pushd .                                         # Сохранение текущей директории
~/CERZAR/workspace ~/CERZAR/workspace
$ source scripts/activate                         # Выполнение скрипта настройки пространства
```

```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab02.git projects/lab03
Cloning into 'projects/lab03'...
remote: Enumerating objects: 21, done.
remote: Counting objects: 100% (21/21), done.
remote: Compressing objects: 100% (16/16), done.
remote: Total 21 (delta 2), reused 11 (delta 0), pack-reused 0
Unpacking objects: 100% (21/21), done.
$ cd projects/lab03
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03.git
```

```ShellSession
$ g++ -std=c++11 -I./include -c sources/print.cpp           # Компиляция source/print.cpp, используя заголовки из ./include и стандарт C++11
$ ls print.o                                                # Проверить наличие файла
print.o
$ nm print.o | grep print                                   # Вывести символы, содержащиеся в объектном файле
0000000000000095 t _GLOBAL__sub_I__Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSo
0000000000000000 T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSo
0000000000000026 T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E
$ ar rvs print.a print.o                                    # Создать архив (статическую библиотеку)
ar: creating print.a
a - print.o
$ file print.a                                              # Информация о файле print.a
print.a: current ar archive
$ g++ -std=c++11 -I./include -c examples/example1.cpp       # Компиляция source/example1.cpp, используя заголовки из ./include и стандарт C++11
$ ls example1.o                                             # Проверить наличие файла
example1.o
$ g++ example1.o print.a -o example1                        # Линковка объектного файла и статической библиотеки. Вывод в example1
$ ./example1 && echo                                        # Запустить полученный файл. И напечатать перевод строки
hello
```

```ShellSession
$ g++ -std=c++11 -I./include -c examples/example2.cpp       # Компиляция source/example2.cpp, используя заголовки из ./include и стандарт C++11
$ nm example2.o                                             # Вывести символы, содержащиеся в полученном файле
                 U __cxa_atexit
                 U __dso_handle
0000000000000000 V DW.ref.__gxx_personality_v0
                 U _GLOBAL_OFFSET_TABLE_
0000000000000163 t _GLOBAL__sub_I_main
                 U __gxx_personality_v0
0000000000000000 T main
                 U __stack_chk_fail
                 U _Unwind_Resume
000000000000011a t _Z41__static_initialization_and_destruction_0ii
                 U _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E
                 U _ZNSaIcEC1Ev
                 U _ZNSaIcED1Ev
                 U _ZNSt14basic_ofstreamIcSt11char_traitsIcEEC1EPKcSt13_Ios_Openmode
                 U _ZNSt14basic_ofstreamIcSt11char_traitsIcEED1Ev
                 U _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEC1EPKcRKS3_
                 U _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEED1Ev
                 U _ZNSt8ios_base4InitC1Ev
                 U _ZNSt8ios_base4InitD1Ev
0000000000000000 r _ZStL19piecewise_construct
0000000000000000 b _ZStL8__ioinit
$ g++ example2.o print.a -o example2                          # Линковка объектного файла со статической библиотекой. Результат в example2
$ ./example2                                                  # Исполнение example2
$ cat log.txt && echo                                         # Вывод в консоль и печать переноса строки
hello
```

```ShellSession
# Удаление файлов
$ rm -rf example1.o example2.o print.o
$ rm -rf print.a
$ rm -rf example1 example2
$ rm -rf log.txt
```

```ShellSession
$ cat > CMakeLists.txt <<EOF      # Запись в указаный файл до встречи EOF
cmake_minimum_required(VERSION 3.4)
project(print)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF     # Дописывание в указаный файл до встречи EOF
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF     # Дописывание в указаный файл до встречи EOF
add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF     # Дописывание в указаный файл до встречи EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include)
EOF
```

```ShellSession
$ cmake -H. -B_build              # Конфигурирование (CMakeLists.txt берется из текущей директории, результат кладется в директорию _build)
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
-- Build files have been written to: /home/cezar/CERZAR/workspace/projects/lab03/_build
$ cmake --build _build            # Выполнение поставленных в CMakeLists.txt задач. Производится в директории _build
Scanning dependencies of target print
[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF      # Дописывание в указаный файл до встречи EOF

add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF     # Дописывание в указаный файл до встречи EOF

target_link_libraries(example1 print)
target_link_libraries(example2 print)
EOF
```

```ShellSession
$ cmake --build _build            # Выполнение поставленных в CMakeLists.txt задач. Производится в директории _build
-- Configuring done
-- Generating done
-- Build files have been written to: /home/cezar/CERZAR/workspace/projects/lab03/_build
[ 33%] Built target print
Scanning dependencies of target example2
[ 50%] Building CXX object CMakeFiles/example2.dir/examples/example2.cpp.o
[ 66%] Linking CXX executable example2
[ 66%] Built target example2
Scanning dependencies of target example1
[ 83%] Building CXX object CMakeFiles/example1.dir/examples/example1.cpp.o
[100%] Linking CXX executable example1
[100%] Built target example1
$ cmake --build _build --target print         # Выполнение поставленных в CMakeLists.txt задач. Производится в директории _build для цели print
[100%] Built target print
$ cmake --build _build --target example1      # Выполнение поставленных в CMakeLists.txt задач. Производится в директории _build для цели example1
[ 50%] Built target print
[100%] Built target example1
$ cmake --build _build --target example2      # Выполнение поставленных в CMakeLists.txt задач. Производится в директории _build для цели example2
[ 50%] Built target print
[ 50%] Built target print
[100%] Built target example2
```

```ShellSession
$ ls -la _build/libprint.a                    # Проверить наличие файла и вывести информацию о нем
-rw-r--r-- 1 cezar cezar 3110 июн  9 21:03 _build/libprint.a 
$ _build/example1 && echo                     # Выполнить example1 и вывести перенос строки
hello
$ _build/example2                             # Выполнить example2
$ cat log.txt && echo                         # Напечатать log.txt в консоль и вывести перенос строки
hello
$ rm -rf log.txt                              # Рекурсивно и без подтверждения удалить log.txt
```

```ShellSession
$ git clone https://github.com/tp-labs/lab03 tmp          # Скачивание репозитория в директорию tmp
Cloning into 'tmp'...
remote: Enumerating objects: 73, done.
remote: Total 73 (delta 0), reused 0 (delta 0), pack-reused 73
Unpacking objects: 100% (73/73), done.
$ mv -f tmp/CMakeLists.txt .                              # Принудительно переместить CMakeLists.txt в текущую директорию
$ rm -rf tmp                                              # Рекурсивно и без подтверждения удалить директорию tmp
```

```ShellSession
$ cat CMakeLists.txt                                      # Вывод CMakeLists.txt в консоль
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_EXAMPLES "Build examples" OFF)

project(print)

add_library(print STATIC ${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)

target_include_directories(print PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

if(BUILD_EXAMPLES)
  file(GLOB EXAMPLE_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/examples/*.cpp")
  foreach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
    get_filename_component(EXAMPLE_NAME ${EXAMPLE_SOURCE} NAME_WE)
    add_executable(${EXAMPLE_NAME} ${EXAMPLE_SOURCE})
    target_link_libraries(${EXAMPLE_NAME} print)
    install(TARGETS ${EXAMPLE_NAME}
      RUNTIME DESTINATION bin
    )
  endforeach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
endif()

install(TARGETS print
    EXPORT print-config
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)
install(EXPORT print-config DESTINATION cmake)
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install          # Конфигурирование (CMakeLists.txt берется из текущей директории, результат кладется в директорию _build, установка поизведется в _install)
-- Configuring done
-- Generating done
-- Build files have been written to: /home/cezar/CERZAR/workspace/projects/lab03/_build
$ cmake --build _build --target install                       # Выполнение поставленных в CMakeLists.txt задач. Производится в директории _build для цели install
-- Install configuration: ""
[100%] Built target print
Install the project...
-- Install configuration: ""
-- Installing: /home/cezar/CERZAR/workspace/projects/lab03/_install/lib/libprint.a
-- Installing: /home/cezar/CERZAR/workspace/projects/lab03/_install/include
-- Installing: /home/cezar/CERZAR/workspace/projects/lab03/_install/include/print.hpp
-- Installing: /home/cezar/CERZAR/workspace/projects/lab03/_install/cmake/print-config.cmake
-- Installing: /home/cezar/CERZAR/workspace/projects/lab03/_install/cmake/print-config-noconfig.cmake
$ tree _install                                               # Вывод дерева файлов
_install
├── cmake
│   ├── print-config.cmake
│   └── print-config-noconfig.cmake
├── include
│   └── print.hpp
└── lib
    └── libprint.a

3 directories, 4 files
```

```ShellSession
$ git add CMakeLists.txt                                      # Фиксация указанного файла
$ git commit -m"added CMakeLists.txt"                         # Коммит
[master 9824186] added CMakeLists.txt
 1 file changed, 36 insertions(+)
 create mode 100644 CMakeLists.txt
$ git push origin master                                      # Отправка изменений
Username for 'https://github.com': CERZAR
Password for 'https://CERZAR@github.com': 
Enumerating objects: 24, done.
Counting objects: 100% (24/24), done.
Delta compression using up to 4 threads
Compressing objects: 100% (19/19), done.
Writing objects: 100% (24/24), 8.42 KiB | 2.81 MiB/s, done.
Total 24 (delta 3), reused 0 (delta 0)
remote: Resolving deltas: 100% (3/3), done.
To https://github.com/CERZAR/lab03.git
 * [new branch]      master -> master
```

## Report

```ShellSession
$ popd
$ export LAB_NUMBER=03
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Homework

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.

### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.

**Удачной стажировки!**

## Links
- [Основы сборки проектов на С/C++ при помощи CMake](https://eax.me/cmake/)
- [CMake Tutorial](http://neerc.ifmo.ru/wiki/index.php?title=CMake_Tutorial)
- [C++ Tutorial - make & CMake](https://www.bogotobogo.com/cplusplus/make.php)
- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2015-2019 The ISC Authors
```
