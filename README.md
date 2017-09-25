## Laboratory work III

Данная лабораторная работа посвещена изучению систем контроля версий на примере **Git**.

```bash
$ open https://git-scm.com
```

## Tasks

- [X] 1. Создать публичный репозиторий с названием **lab03** и с лиценцией **MIT**
- [X] 2. Ознакомиться со ссылками учебного материала
- [X] 3. Выполнить инструкцию учебного материала
- [X] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=<babyrage666> # Устанавливаем значение переменной окружения GITHUB_USERNAME
$ export GITHUB_EMAIL=<kapotyuk0901@yandex.ru> # Устанавливаем значение переменной окружения
$ alias edit=subl # Выбираем текстовый редактор, в котором будем работать
```

```ShellSession
$ mkdir lab03 && cd lab03 # sozdnie i pererhod v direktoriyu
$ git init # initializaciya hranilishya
$ git config --global user.name ${GITHUB_USERNAME} # declaraciya username
$ git config --global user.email ${GITHUB_EMAIL} # declaraciya e-mail
$ git config -e --global # edit
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03 
$ git pull origin master # obyedinenie repozitoriya
$ touch README.md # sozdanie file
$ git status # status
$ git add README.md # add file
$ git commit -m"added README.md" # commitim
$ git push origin master # psuhim
```

Добавить на сервисе **GitHub** в репозитории **lab03** файл **.gitignore**
со следующем содержимом:

```ShellSession
*build*/
*install*/
*.swp
```

```ShellSession
$ git pull origin master # obyedinenie repozitoriev
$ git log # check loga
```

```ShellSession
$ mkdir sources # sozdanie directorii
$ mkdir include
$ mkdir examples
$ cat > sources/print.cpp <<EOF
#include <print.hpp>

void print(const std::string& text, std::ostream& out) {
  out << text;
}

void print(const std::string& text, std::ofstream& out) {
  out << text;
}
EOF
```

```ShellSession
$ cat > include/print.hpp <<EOF
#include <string>
#include <fstream>
#include <iostream>

void print(const std::string& text, std::ostream& out = std::cout);
void print(const std::string& text, std::ofstream& out);
EOF
```

```ShellSession
$ cat > examples/example1.cpp <<EOF
#include <print.hpp>

int main(int argc, char** argv) {
  print("hello");
}
EOF
```

```ShellSession
$ cat > examples/example2.cpp <<EOF
#include <fstream>
#include <print.hpp>

int main(int argc, char** argv) {
  std::ofstream file("log.txt");
  print(std::string("hello"), file);
}
EOF
```

```ShellSession
$ edit README.md # redactirovat'
```

```ShellSession
$ git status # pokaz sostoyaniya dereva
$ git add . # dobavit' vse
$ git commit -m"added sources" # commit
$ git push origin master # push
```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=03
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [GitHub](https://github.com)
- [Bitbucket](https://bitbucket.org)
- [Gitlab](https://about.gitlab.com)
- [LearnGitBranching](http://learngitbranching.js.org/)

```
Copyright (c) 2017 Братья Вершинины
```
