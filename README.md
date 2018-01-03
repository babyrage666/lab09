[![Build Status](https://travis-ci.org/babyrage666/lab09.svg?branch=v0.1.0.0)](https://travis-ci.org/babyrage666/lab09)
## Laboratory work IX

Данная лабораторная работа посвещена изучению процесса создания пакета на примере **Github Release**

```ShellSession
$ open https://help.github.com/articles/creating-releases/
```

## Tasks

- [X] 1. Создать публичный репозиторий с названием **lab09** на сервисе **GitHub**
- [X] 2. Ознакомиться со ссылками учебного материала
- [X] 3. Получить токен для доступа к репозиториям сервиса **GitHub**
- [X] 4. Сгенерировать GPG ключ и добавить его к аккаунту сервиса **GitHub**
- [X] 5. Выполнить инструкцию учебного материала
- [X] 6. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial
Определяем глобальные переменные.
```ShellSession
$ export GITHUB_TOKEN=ef6edc3a2c7716814bb0765962ee7cb3abfa570e
$ export GITHUB_USERNAME=babyrage666
$ alias gsed=sed
```
Подготовка к выполнению **Лабораторной работы №9**.
```ShellSession
$ # Скачиваем и устанавливаем дополнительные пакеты.
$ go get github.com/aktau/github-release
$ git clone https://github.com/${GITHUB_USERNAME}/lab08 projects/lab09
$ cd projects/lab09
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab09
```
Переименовываем **lab08** на **lab09** в файле **README.md**.
```ShellSession
$ gsed -i 's/lab08/lab09/g' README.md
```
Собираем **cmake** проект и генерируем **TGZ** пакет.
```ShellSession
$ cmake -H. -B_build -DCPACK_GENERATOR="TGZ"
$ cmake --build _build --target package
```
Подключаем **Travis CI**.
```ShellSession
$ travis login --auto
$ travis enable
```
Добавляем **tag** для GitHub-репозитория.
```ShellSession
$ # Добавляем tag связанный с gpg ключом.
$ git tag -s v0.1.0.0
$ # Подтверждаем tag.
$ git tag -v v0.1.0.0
$ # Пушим на удаленный репозиторий.
$ git push origin master --tags
```
Настраиваем релиз проекта.
```ShellSession
$ # Проверяем версию github-release.
$ github-release --version
$ # Информация о релизе lab09.
$ github-release info -u ${GITHUB_USERNAME} -r lab09
$ # Настраивем релиз.
$ github-release release \
    --user ${GITHUB_USERNAME} \
    --repo lab09 \
    --tag v0.1.0.0 \
    --name "libprint" \
    --description "my first release"
```
Корректируем релиз и отправляем на **GitHub**.
```ShellSession
$ # Определяем тип операционной системы uname -s и ядра uname -m.
$ export PACKAGE_OS=`uname -s` PACKAGE_ARCH=`uname -m`
$ # Компонуем название релиза.
$ export PACKAGE_FILENAME=print-${PACKAGE_OS}-${PACKAGE_ARCH}.tar.gz
$ # Выгружаем релиз.
$ github-release upload \
    --user ${GITHUB_USERNAME} \
    --repo lab09 \
    --tag v0.1.0.0 \
    --name "${PACKAGE_FILENAME}" \
    --file _build/*.tar.gz
```
Скачиваем и проверяем готовый релиз.
```ShellSession
$ github-release info -u ${GITHUB_USERNAME} -r lab09
$ # Скачиваем созданный релиз.
$ wget https://github.com/${GITHUB_USERNAME}/lab09/releases/download/v0.1.0.0/${PACKAGE_FILENAME}
$ # Проверяем содержимое релиза.
$ tar -ztf ${PACKAGE_FILENAME}
```

## Report

```ShellSession
$ popd
$ export LAB_NUMBER=09
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [Create Release](https://help.github.com/articles/creating-releases/)
- [Get GitHub Token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)
- [Signing Commits](https://help.github.com/articles/signing-commits-with-gpg/)
- [Go Setup](http://www.golangbootcamp.com/book/get_setup)
- [github-release](https://github.com/aktau/github-release)

```
Copyright (c) 2017 Братья Вершинины
```
