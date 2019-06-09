## Laboratory work II

Данная лабораторная работа посвещена изучению систем контроля версий на примере **Git**.

```bash
$ open https://git-scm.com
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab02** и с лиценцией **MIT**
- [x] 2. Сгенирировать токен для доступа к сервису **GitHub** с правами **repo**
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Выполнить инструкцию учебного материала
- [x] 5. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=CERZAR                                   # Установка переменной GITHUB_USERNAME
$ export GITHUB_EMAIL=stavropoltsev.t@gmail.com                   # Установка переменной GITHUB_EMAIL
$ export GITHUB_TOKEN=d1e117aebb87f774bb42e508a66d677318908e9d    # Установка переменной GITHUB_TOKEN
$ alias edit=nano                                                 # Установка синонима команды edit
```

```ShellSession
$ cd ${GITHUB_USERNAME}/workspace                                 # Переход в папку workspace
$ source scripts/activate                                         # Выполнение скрипта подготовки
```

```ShellSession
$ mkdir ~/.config                                                 # Создание папки с конфигами
# mkdir: cannot create directory ‘/home/cezar/.config’: File exists
$ cat > ~/.config/hub <<EOF                                       # Создание файла конфига hub и запись в него
github.com:
- user: ${GITHUB_USERNAME}
  oauth_token: ${GITHUB_TOKEN}
  protocol: https
EOF
$ git config --global hub.protocol https                          # Установка переменной конфига git
```

```ShellSession
$ mkdir projects/lab02 && cd projects/lab02                       # Создание папки и переход в нее
$ git init                                                        # Создание пустого репозитория
Initialized empty Git repository in /home/cezar/CERZAR/workspace/projects/lab02/.git/
$ git config --global user.name ${GITHUB_USERNAME}                # Установка переменной конфига user.name для git
$ git config --global user.email ${GITHUB_EMAIL}                  # Установка переменной конфига user.email для git
# check your git global settings
$ git config -e --global                                          # Вывод всего конфига из git
[hub]
        protocol = https
[user]
        name = CERZAR
        email = stavropoltsev.t@gmail.com
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab02.git   # Добавление ссылки на репозиторий на гитхабе
$ git pull origin master                                                  # Перенос изменения с гитхаба
remote: Enumerating objects: 6, done.
remote: Counting objects: 100% (6/6), done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (6/6), done.
From https://github.com/CERZAR/lab02
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> origin/master
$ touch README.md                                                         # Создать README.md
$ git status                                                              # Посмотреть статус репозитория
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

        README.md

nothing added to commit but untracked files present (use "git add" to track) 
$ git add README.md                                                       # Добавить README.md в список фиксированных
$ git commit -m"added README.md"                                          # Закоммитить фиксированные изменения
[master 5a9364f] added README.md
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md
$ git push origin master                                                  # Отправить изменения на гитхаб
Username for 'https://github.com': CERZAR
Password for 'https://CERZAR@github.com': 
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 4 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 284 bytes | 284.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/CERZAR/lab02.git
   d3f614c..5a9364f  master -> master
```

Добавить на сервисе **GitHub** в репозитории **lab02** файл **.gitignore**
со следующем содержимом:

```ShellSession
*build*/
*install*/
*.swp
.idea/
```

```ShellSession
$ git pull origin master                                                # Перенос изменений с гитхаба
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/CERZAR/lab02
 * branch            master     -> FETCH_HEAD
   5a9364f..d7be91b  master     -> origin/master
Updating 5a9364f..d7be91b
Fast-forward
 .gitignore | 4 ++++
 1 file changed, 4 insertions(+)
 create mode 100644 .gitignore
$ git log                                                               # Просмотр коммитов
commit d7be91b470f9f1d8e71f45df0a8f29e1b49f9d79 (HEAD -> master, origin/master)
Author: CERZAR <47750144+CERZAR@users.noreply.github.com>
Date:   Sun Jun 9 20:50:36 2019 +0300

    Create .gitignore

commit 5a9364f0c56d00af1039e16e52fd6b7ef37257b0
Author: CERZAR <stavropoltsev.t@gmail.com>
Date:   Sun Jun 9 19:50:05 2019 +0300

    added README.md

commit d3f614c3c40f39ef07254abcc8918f9815598488
Author: CERZAR <47750144+CERZAR@users.noreply.github.com>
Date:   Sun Jun 9 20:30:59 2019 +0300

    Initial commit
```

```ShellSession
$ mkdir sources                                                       # Создание папки sources
$ mkdir include                                                       # Создание папки include
$ mkdir examples                                                      # Создание папки examples
$ cat > sources/print.cpp <<EOF                                       # Запись кода в файл
#include <print.hpp>

void print(const std::string& text, std::ostream& out)
{
  out << text;
}

void print(const std::string& text, std::ofstream& out)
{
  out << text;
}
EOF
```

```ShellSession
$ cat > include/print.hpp <<EOF                                       # Запись кода в файл
#include <fstream>
#include <iostream>
#include <string>

void print(const std::string& text, std::ofstream& out);
void print(const std::string& text, std::ostream& out = std::cout);
EOF
```

```ShellSession
$ cat > examples/example1.cpp <<EOF                                   # Запись кода в файл
#include <print.hpp>

int main(int argc, char** argv)
{
  print("hello");
}
EOF
```

```ShellSession
$ cat > examples/example2.cpp <<EOF                                    # Запись кода в файл
#include <print.hpp>

#include <fstream>

int main(int argc, char** argv)
{
  std::ofstream file("log.txt");
  print(std::string("hello"), file);
}
EOF
```

```ShellSession
$ edit README.md                                                       # Редактировать README.md
```

```ShellSession
$ git status                                                           # Просмотр статуса репозитория
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

        examples/
        include/
        sources/

nothing added to commit but untracked files present (use "git add" to track)
$ git add .                                                           # Фиксация всех изменений
$ git commit -m"added sources"                                        # Коммит зафиксированных изменений
[master 37b32af] added sources
 4 files changed, 32 insertions(+)
 create mode 100644 examples/example1.cpp
 create mode 100644 examples/example2.cpp
 create mode 100644 include/print.hpp
 create mode 100644 sources/print.cpp
$ git push origin master                                              # Отправка изменений на гитхаб
Username for 'https://github.com': CERZAR
Password for 'https://CERZAR@github.com': 
Enumerating objects: 10, done.
Counting objects: 100% (10/10), done.
Delta compression using up to 4 threads
Compressing objects: 100% (7/7), done.
Writing objects: 100% (9/9), 970 bytes | 970.00 KiB/s, done.
Total 9 (delta 0), reused 0 (delta 0)
To https://github.com/CERZAR/lab02.git
   d7be91b..37b32af  master -> master
```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=02
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER}.git tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Homework

### Part I

1. Создайте пустой репозиторий на сервисе github.com (или gitlab.com, или bitbucket.com).
2. Выполните инструкцию по созданию первого коммита на странице репозитория, созданного на предыдещем шаге.
3. Создайте файл `hello_world.cpp` в локальной копии репозитория (который должен был появиться на шаге 2). Реализуйте программу **Hello world** на языке C++ используя плохой стиль кода. Например, после заголовочных файлов вставьте строку `using namespace std;`.
4. Добавьте этот файл в локальную копию репозитория.
5. Закоммитьте изменения с *осмысленным* сообщением.
6. Изменитьте исходный код так, чтобы программа через стандартный поток ввода запрашивалось имя пользователя. А в стандартный поток вывода печаталось сообщение `Hello world from @name`, где `@name` имя пользователя.
7. Закоммитьте новую версию программы. Почему не надо добавлять файл повторно `git add`?
8. Запуште изменения в удалёный репозиторий.
9. Проверьте, что история коммитов доступна в удалёный репозитории.

### Part II

**Note:** *Работать продолжайте с теми же репоззиториями, что и в первой части задания.*
1. В локальной копии репозитория создайте локальную ветку `patch1`.
2. Внесите изменения в ветке `patch1` по исправлению кода и избавления от `using namespace std;`.
3. **commit**, **push** локальную ветку в удалённый репозиторий.
4. Проверьте, что ветка `patch1` доступна в удалёный репозитории.
5. Создайте pull-request `patch1 -> master`.
6. В локальной копии в ветке `patch1` добавьте в исходный код комментарии.
7. **commit**, **push**.
8. Проверьте, что новые изменения есть в созданном на **шаге 5** pull-request
9. В удалённый репозитории выполните  слияние PR `patch1 -> master` и удалите ветку `patch1` в удаленном репозитории.
10. Локально выполните **pull**.
11. С помощью команды **git log** просмотрите историю в локальной версии ветки `master`.
12. Удалите локальную ветку `patch1`.

### Part III

**Note:** *Работать продолжайте с теми же репоззиториями, что и в первой части задания.*
1. Создайте новую локальную ветку `patch2`.
2. Измените *code style* с помощью утилиты [**clang-format**](http://clang.llvm.org/docs/ClangFormat.html). Например, используя опцию `-style=Mozilla`.
3. **commit**, **push**, создайте pull-request `patch2 -> master`.
4. В ветке **master** в удаленном репозитории измените комментарии, например, расставьте знаки препинания, переведите комментарии на другой язык.
5. Убедитесь, что в pull-request появились *конфликтны*.
6. Для этого локально выполните **pull** + **rebase** (точную последовательность команд, следует узнать самостоятельно). **Исправьте конфликты**.
7. Сделайте *force push* в ветку `patch2`
8. Убедитель, что в pull-request пропали конфликтны. 
9. Вмержите pull-request `patch2 -> master`.

## Links

- [hub](https://hub.github.com/)
- [GitHub](https://github.com)
- [Bitbucket](https://bitbucket.org)
- [Gitlab](https://about.gitlab.com)
- [LearnGitBranching](http://learngitbranching.js.org/)

```
Copyright (c) 2015-2019 The ISC Authors
```
