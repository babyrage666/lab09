[![Build Status](https://travis-ci.org/babyrage666/lab08.svg?branch=master)](https://travis-ci.org/babyrage666/lab08)
## Laboratory work VII

Данная лабораторная работа посвещена изучению систем документирования исходного кода на примере **Doxygen**

```ShellSession
$ open https://www.stack.nl/~dimitri/doxygen/manual/index.html
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab08** на сервисе **GitHub**
- [x] 2. Выполнить инструкцию учебного материала
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

Определяем глобальную переменную.
```ShellSession
$ export GITHUB_USERNAME=babyrage666
$ alias edit=subl
```

Подготовка к выполнению **Лабораторной работы №7**.
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab06 lab08
$ cd lab08
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab08
```

Cоздаем директорию **docs** и загуржаем в нее файл конфигураций **doxygen.conf**.
```ShellSession
$ mkdir docs
$ doxygen -g docs/doxygen.conf
$ cat docs/doxygen.conf #просмотр файла
```

Редактирование файла **doxygen.conf**.
```ShellSession
#Задаем название проекта print.
$ sed --in-place 's/\(PROJECT_NAME.*=\).*$/\1 print/g' docs/doxygen.conf
#Указываем путь к examples.
$ sed --in-place 's/\(EXAMPLE_PATH.*=\).*$/\1 examples/g' docs/doxygen.conf
# Указываем путь к include.
$ sed --in-place 's/\(INCLUDE_PATH.*=\).*$/\1 examples/g' docs/doxygen.conf
# Вводим файл README.md.
$ sed --in-place 's/\(INPUT *=\).*$/\1 README.md include/g' docs/doxygen.conf
# Назначаем файл README.md основным.
$ sed --in-place 's/\(USE_MDFILE_AS_MAINPAGE.*=\).*$/\1 README.md/g' docs/doxygen.conf
# Указываем путь каталога docs.
$ sed --in-place 's/\(OUTPUT_DIRECTORY.*=\).*$/\1 docs/g' docs/doxygen.conf
```

Редактируем файл **README.md**
```ShellSession
$ sed --in-place 's/lab06/lab08/g' README.md
```

Документируем **print**.
```ShellSession
$ # Описываем входные и выходные параметры в декларации функций.
$ edit include/print.hpp
```

Отправка данных на удаленный сервер **GitHub**
```ShellSession
$ git add .
$ git commit -m"added doxygen.conf"
$ git push origin master
```

Подключаем **Travis CI**
```ShellSession
$ travis login --auto
$ travis enable
```

Собираем HTML-документ **doxygen**.
```
# Собираем проект doxygen.
$ doxygen docs/doxygen.conf
$ ls | grep "[^docs]" | xargs rm -rf
$ mv docs/html/* . && rm -rf docs
# Создаем ветку gh-pages и заносим в нее файлы doxygen.
$ git checkout -b gh-pages
$ git add .
$ git commit -m"added documentation"
$ git push origin gh-pages
$ git checkout master # Переключаемся на ветку master
```

Добавляем скриншот HTML-страницы **doxygen** в GoogleDrive и разрешаем доступ для rusdevops@gmail.com.
```ShellSession
$ mkdir artifacts && cd artifacts
$ open https://${GITHUB_USERNAME}.github.io/lab08/print_8hpp.html 
$ sleep 5s && gnome-screenshot --file artifacts/screenshot.png
$ gdrive upload screenshot.png
$ SCREENSHOT_ID=`gdrive list | grep screenshot | awk '{ print $1; }'`
$ gdrive share ${SCREENSHOT_ID} --role reader --type user --email rusdevops@gmail.com
$ echo https://drive.google.com/open?id=${SCREENSHOT_ID}

```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=07
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [HTML](https://ru.wikipedia.org/wiki/HTML)
- [LAΤΕΧ](https://ru.wikipedia.org/wiki/LaTeX)
- [man](https://ru.wikipedia.org/wiki/Man_(%D0%BA%D0%BE%D0%BC%D0%B0%D0%BD%D0%B4%D0%B0_Unix))
- [CHM](https://ru.wikipedia.org/wiki/HTMLHelp)
- [PostScript](https://ru.wikipedia.org/wiki/PostScript)

```
Copyright (c) 2017 Братья Вершинины
```
