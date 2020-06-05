## Laboratory work III

<a href="https://yandex.ru/efir/?stream_id=vjKAlxJ0UQrs"><img src="https://raw.githubusercontent.com/tp-labs/lab03/master/preview.png" width="640"/></a>

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**

```sh
$ open https://cmake.org/
```

## Tasks

- [ ] 1. Создать публичный репозиторий с названием **lab03** на сервисе **GitHub**
- [ ] 2. Ознакомиться со ссылками учебного материала
- [ ] 3. Выполнить инструкцию учебного материала
- [ ] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```sh
$ export GITHUB_USERNAME=<имя_пользователя>
#создаётся переменная GITHUB_USERNAME с введенным значением
```

```sh
$ cd ${GITHUB_USERNAME}/workspace
#переходим в папку workspace
$ pushd .
#текущий путь сохраняется в стек путей
$ source scripts/activate
активируется написанное в файле scripts/activate
```

```sh
$ git clone https://github.com/${GITHUB_USERNAME}/lab02.git projects/lab03
#клонируется репозиторий второй лабораторной работы в папку projects/lab03
$ cd projects/lab03
#переходим в папку projects/lab03
$ git remote remove origin
#удаляем ветку origin удаленного репозитория
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03.git
#создаем новую ветку origin, указывающую на новый удаленный репозиторий
```

```sh
$ g++ -std=c++11 -I./include -c sources/print.cpp
#компиллируем файл print.cpp при помощи g++ в объектный файл 
$ ls print.o
#показывает скомпиллированный на предыдущем шаге файл
$ nm print.o | grep print
#выводится записанное в файл print.o 
$ ar rvs print.a print.o
#создается файл print.a на основе print.o 
$ file print.a
#выводится тип файла print.a (статическая библиотека)
$ g++ -std=c++11 -I./include -c examples/example1.cpp
#компиллируем файл example1.cpp при помощи g++ в объектный файл 
$ ls example1.o
#показывает скомпиллированный на предыдущем шаге файл 
$ g++ example1.o print.a -o example1
#собираем из файлов example1.o и print.a файл example1
$ ./example1 && echo
#выводится результат работы файла example1 в стандартный поток вывода
```

```sh
$ g++ -std=c++11 -I./include -c examples/example2.cpp
#компиллируем файл example2.cpp при помощи g++ в объектный файл
$ nm example2.o
#выводится содержимое файла example2.o в стандартный поток вывода
$ g++ example2.o print.a -o example2
#собираем из файлов example2.o и print.a файл example2 
$ ./example2
#запускаем файл example2
$ cat log.txt && echo
#результат работы example2 записывается в log.txt, выводим его содержимое в стандартный поток вывода 
```

```sh
$ rm -rf example1.o example2.o print.o
$ rm -rf print.a
$ rm -rf example1 example2
$ rm -rf log.txt
#удаляем файлы example1.o, example2.o, print.o, print.a, example1, example2, log.txt
```

```sh
$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.4)
project(print)
EOF
#перенаправляем стандартный поток ввода в файл CMakeLists.txt и записываем туда строки, введенные до EOF
```

```sh
$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
#перенаправляем стандартный поток ввода в файл CMakeLists.txt и записываем туда строки, введенные до EOF
```

```sh
$ cat >> CMakeLists.txt <<EOF
add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
EOF
#перенаправляем стандартный поток ввода в файл CMakeLists.txt и записываем туда строки, введенные до EOF
```

```sh
$ cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include)
EOF
#перенаправляем стандартный поток ввода в файл CMakeLists.txt и записываем туда строки, введенные до EOF
```

```sh
$ cmake -H. -B_build
#создание файлов проекта в текущей директории в папке _build 
$ cmake --build _build
#сборка проекта из _build (компилляция и линковка) 
```

```sh
$ cat >> CMakeLists.txt <<EOF

add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)
EOF
#перенаправляем стандартный поток ввода в файл CMakeLists.txt и записываем туда строки, введенные до EOF
```

```sh
$ cat >> CMakeLists.txt <<EOF

target_link_libraries(example1 print)
target_link_libraries(example2 print)
EOF
#перенаправляем стандартный поток ввода в файл CMakeLists.txt и записываем туда строки, введенные до EOF
```

```sh
$ cmake --build _build
#собираем весь проект (print уже собрана, она повторно не собирается)
$ cmake --build _build --target print
#собираем только print (уже собрана, повторно не собирается) 
$ cmake --build _build --target example1
#собираем example1, для него нужна print, ее тоже собираем (они уже собраны, повторно не собираются) 
$ cmake --build _build --target example2
#собираем example2, для него нужна print, ее тоже собираем (они уже собраны, повторно не собираются) 
```

```sh
$ ls -la _build/libprint.a
#просматриваем полную информацию про статическую библиотеку libprint.a
$ _build/example1 && echo
hello
#запускаем файл example1 и выводим в стандартный поток вывода результат работы (выводится hello) 
$ _build/example2
#запускаем файл example2 
$ cat log.txt && echo
hello
#выводим в стандартный поток вывода содержимое файла log.txt, созданного example2 
$ rm -rf log.txt
#удаляем файл log.txt 
```

```sh
$ git clone https://github.com/tp-labs/lab03 tmp
#клонируем репозиторий третьей лабораторной работы в папку tmp 
$ mv -f tmp/CMakeLists.txt .
#обновляем файл CMakeLists.txt, переместив такой файл из лабораторной работы в рабочий каталог проекта 
$ rm -rf tmp
#удаляем папку tmp
```

```sh
$ cat CMakeLists.txt
#выводим содержимое файла CMakeLists.txt в стандартный поток вывода
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
#создание файлов проекта в текущей директории в папке _build со значением переменной CMAKE_INSTALL_PREFIX=_install 
$ cmake --build _build --target install
#сборка проекта с установленной целью install
$ tree _install
#выводим содержимое папки _install в виде дерева
```

```sh
$ git add CMakeLists.txt
#добавляем в область подготовленный файлов файл CMakeLists.txt 
$ git commit -m"added CMakeLists.txt"
#создаем коммит с сообщением "added CMakeLists.txt"
$ git push origin master
#пушим на ветку origin master в удаленный репозиторий все изменения 
```

## Report

```sh
$ popd
$ export LAB_NUMBER=03
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
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
Copyright (c) 2015-2020 The ISC Authors
```
