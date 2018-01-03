[![Build Status](https://travis-ci.org/babyrage666/lab08.svg?branch=master)](https://travis-ci.org/babyrage666/lab08)
## Laboratory work VIII

Данная лабораторная работа посвещена изучению средств пакетирования на примере **CPack**

```ShellSession
$ open https://cmake.org/Wiki/CMake:CPackPackageGenerators
```

## Tasks

- [X] 1. Создать публичный репозиторий с названием **lab08** на сервисе **GitHub**
- [X] 2. Выполнить инструкцию учебного материала
- [X] 3. Ознакомиться со ссылками учебного материала
- [X] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial
Определяем глобальные переменные.
```ShellSession
$ export GITHUB_USERNAME=babyrage666
$ export GITHUB_EMAIL=kapotyuk0901@yandex.ru
$ alias edit=subl
$ alias gsed=sed
```
Подготовка к выполнению **Лабораторной работы №8**
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab07 lab08
$ cd lab08
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab08
```
Вносим изменения в **CMakeLists.txt** *(переменные для файла **CPackConfig.cmake**)*
```ShellSession
$ sed -i '' '/project(print)/a\
set(PRINT_VERSION_STRING "v${PRINT_VERSION}")
' CMakeLists.txt
$ sed -i '' '/project(print)/a\
set(PRINT_VERSION \
\${PRINT_VERSION_MAJOR}.\${PRINT_VERSION_MINOR}.\${PRINT_VERSION_PATCH}.\${PRINT_VERSION_TWEAK})
' CMakeLists.txt
$ sed -i '' '/project(print)/a\
$ # Надстройка программы (без изменения кода). Для локальной работы.
set(PRINT_VERSION_TWEAK 0)
' CMakeLists.txt
$ sed -i '' '/project(print)/a\
$ # Исправление ошибки пакета (без изменения кода).
set(PRINT_VERSION_PATCH 0)
' CMakeLists.txt
$ sed -i '' '/project(print)/a\
$ # Изменение интерфейса (изменение кода: добавляется class или func)
set(PRINT_VERSION_MINOR 1)
' CMakeLists.txt
$ sed -i '' '/project(print)/a\
$ # 0 - alpha версия, 1 - стабильная версия
set(PRINT_VERSION_MAJOR 0)
' CMakeLists.txt
```
Создаем файлы: **DESCRIPTION** и **ChangeLog.md**.
```ShellSession
$ touch DESCRIPTION && edit DESCRIPTION
$ touch ChangeLog.md
$ # Присваиваем переменной дату и время и записываем в файл ChangeLog.md.
$ # Формат для русской локализации ОС [ date "+%Y-%m-%d %H:%M:%S" ].
$ DATE=`date "+%Y-%m-%d %H:%M:%S"` cat > ChangeLog.md <<EOF
* ${DATE} ${GITHUB_USERNAME} <${GITHUB_EMAIL}> 0.1.0.0
- Initial RPM release
EOF
```
Подключаем библиотеку для **CPack**.
```ShellSession
$ cat > CPackConfig.cmake <<EOF
include(InstallRequiredSystemLibraries)
EOF
```
Переносим значения переменных из файла **CMakeLists.txt**.
```ShellSession
$ cat >> CPackConfig.cmake <<EOF
set(CPACK_PACKAGE_CONTACT ${GITHUB_EMAIL})
set(CPACK_PACKAGE_VERSION_MAJOR \${PRINT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR \${PRINT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH \${PRINT_VERSION_PATCH})
set(CPACK_PACKAGE_VERSION_TWEAK \${PRINT_VERSION_TWEAK})
set(CPACK_PACKAGE_VERSION \${PRINT_VERSION})
set(CPACK_PACKAGE_DESCRIPTION_FILE \${CMAKE_CURRENT_SOURCE_DIR}/DESCRIPTION)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "static c++ library for printing")
EOF
```
Определяем путь к файлам **LICENSE** и **README.md**.
```ShellSession
$ cat >> CPackConfig.cmake <<EOF

set(CPACK_RESOURCE_FILE_LICENSE \${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
set(CPACK_RESOURCE_FILE_README \${CMAKE_CURRENT_SOURCE_DIR}/README.md)
EOF
```
Устанавливаем параметры для сборки **RPM** пакета.
```ShellSession
$ cat >> CPackConfig.cmake <<EOF

set(CPACK_RPM_PACKAGE_NAME "print-devel")
set(CPACK_RPM_PACKAGE_LICENSE "MIT")
set(CPACK_RPM_PACKAGE_GROUP "print")
set(CPACK_RPM_PACKAGE_URL "https://github.com/${GITHUB_USERNAME}/lab07")
set(CPACK_RPM_CHANGELOG_FILE \${CMAKE_CURRENT_SOURCE_DIR}/ChangeLog.md)
set(CPACK_RPM_PACKAGE_RELEASE 1)
EOF
```
Устанавливаем параметры для сборки **DEB** пакета.
```ShellSession
$ cat >> CPackConfig.cmake <<EOF

set(CPACK_DEBIAN_PACKAGE_NAME "libprint-dev")
set(CPACK_DEBIAN_PACKAGE_HOMEPAGE "https://${GITHUB_USERNAME}.github.io/lab07")
set(CPACK_DEBIAN_PACKAGE_PREDEPENDS "cmake >= 3.0")
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)
EOF
```
Подключаем **CPack** к файлу **CPackConfig.cmake** для обработки введенных переменных.
```ShellSession
$ cat >> CPackConfig.cmake <<EOF

include(CPack)
EOF
```
Подключаем **CPackConfig.cmake** к файлу **CMakeLists.txt**.
```ShellSession
$ cat >> CMakeLists.txt <<EOF

include(CPackConfig.cmake)
EOF
```
Переименовываем **lab07** на **lab08** в файле **README.md**.
```ShellSession
$ sed -i 's/lab07/lab08/g' README.md
```
Отправляем данный на удаленный репозиторий на **GitHub**.
```ShellSession
$ git add .
$ git commit -m"added cpack config"
$ git push origin master
```
Подключаем **Travis CI**.
```ShellSession
$ travis login --auto
$ travis enable
```
Собираем **cmake** проект и при помощи **cpack** генерируем пакеты различного формата.
```ShellSession
$ cmake -H. -B_build
$ cmake --build _build
$ cd _build
$ cpack -G "TGZ"
$ cpack -G "RPM"
$ cpack -G "DEB"
$ cpack -G "NSIS"
$ cpack -G "DragNDrop"
$ cd ..
```
Собираем cmake проект через **TGZ** пакет.
```ShellSession
$ cmake -H. -B_build -DCPACK_GENERATOR="TGZ"
# После подключения CPack - автоматически создается цель package.
$ cmake --build _build --target package
```
Переносим архив **print-0.1.0.0-Darwin.tar.gz** в каталог **artifacts**.
```ShellSession
$ mkdir artifacts
$ mv _build/*.tar.gz artifacts
$ tree artifacts
```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=08
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [DMG](https://cmake.org/cmake/help/latest/module/CPackDMG.html)
- [DEB](https://cmake.org/cmake/help/latest/module/CPackDeb.html)
- [RPM](https://cmake.org/cmake/help/latest/module/CPackRPM.html)
- [NSIS](https://cmake.org/cmake/help/latest/module/CPackNSIS.html)

```
Copyright (c) 2017 Братья Вершинины
```
