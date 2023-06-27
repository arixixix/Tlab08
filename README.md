## Laboratory work VIII

Данная лабораторная работа посвещена изучению систем автоматизации развёртывания и управления приложениями на примере **Docker**

```sh
$ open https://docs.docker.com/get-started/
```

## Homework

Для начала, я ознакомилась с базовыми принципами работы Docker-файлов. Ниже я привела основные для наглядности.

### Установка Docker

```sh
sudo apt-get install docker
```

### Основные инструкции Dockerfile

```sh
FROM
LABEL
RUN
COPY
CMD
WORKDIR
ENV
```

### Основные команды для управления контейнерами

```sh
run
stop
rm
logs
```

### Основные команды для управления образами

```sh
build
push
ls
rmi
```

### Доп команды

```sh
docker login
docker version
docker system prune
docker container ls -a -s
```

### Флаги

```sh
-t
ps -a 
-s
```

Далее я сделала 2 докер-файла, чтобы научиться работать с Docker.

Первый, это результат работы следующего python-файла:

### DockerExp/DockerPic.py
```sh
print("            ***********         **********       ********     **  ***       ************     **********  ")
print("           ***         **      **        **     **           **  ***       **               ***     **   ")
print("          ***          **     **        **     **           ** ***        **********       ***     **    ")
print("         ***          **     **       **      **           ****          **********      **********      ")
print("        ***          **     **       **      **           *****         **              ******           ")
print("       ***          **     **       **      **           **   ***      ***********     ***  ***          ")
print("      *************       ***********      **********   **      ***   ************    ***     ***        ")
```

### Dockerfile для этой программы 

```sh
FROM python

WORKDIR /app_dir

COPY . .

RUN apt-get update

CMD ["python", "DockerPic.py"]

```

Данный Docker-файл описывает процесс сборки образа Docker на основе базового образа python.

Вот пошаговое объяснение того, что делает каждая инструкция в Docker-файле:


   1. `FROM python` Определяет базовый образ, на основе которого будет создаваться новый образ. В данном случае используется официальный образ Python.

   2. `WORKDIR /app_dir` Устанавливает рабочую директорию внутри контейнера. Все последующие инструкции будут выполняться в этой директории. В данном случае устанавливается рабочая директория /app_dir.

   3. `COPY . .` Копирует содержимое текущего каталога (где находится Docker-файл) внутрь контейнера в указанную рабочую директорию (/app_dir). То есть все файлы и каталоги из текущего контекста сборки будут скопированы внутрь контейнера.

   4. `RUN apt-get update` Обновляет список пакетов внутри контейнера. Эта команда выполняется во время сборки образа и используется для установки и обновления пакетов внутри контейнера. В данном случае обновляется список пакетов с помощью apt-get update.

   5. `CMD ["python", "DockerPic.py"]` Устанавливает команду по умолчанию, которая будет выполняться при запуске контейнера. В данном случае контейнер будет запускать скрипт DockerPic.py с помощью интерпретатора Python. Если в командной строке при запуске контейнера указывается другая команда, она заменит эту команду по умолчанию.


Таким образом, этот Docker-файл создает образ, внутри которого будет установлен интерпретатор Python, скопированы файлы из текущего каталога внутрь контейнера, обновлены пакеты и установлены зависимости, а затем при запуске контейнера будет выполняться скрипт DockerPic.py.



Следующие команды (создание docker image и его запуск) были выполнены для проверки работы:

```sh
% sudo docker build -t picture ./DockerExp
% sudo docker run picture
```

Вот объяснение каждой команды:

  1. `sudo docker build -t picture ./DockerExp` Эта команда создает Docker-образ с помощью файла Dockerfile, находящегося в директории ./DockerExp. Опция -t указывает тег (или имя) для образа, в данном случае picture. Команда выполняется с правами суперпользователя (sudo), чтобы обеспечить доступ к Docker-демону.

  2. `sudo docker run picture` Эта команда запускает контейнер на основе образа с тегом picture. Команда выполняется также с правами суперпользователя (sudo).

В результате выполнения этих команд будет создан Docker-образ с тегом picture и запущен соответствующий контейнер.


Далее аналогичные действия были проведены для следущего python-файла:

### DockerThis/THIS.py

```sh
import this
```

### Dockerfile

```sh
FROM python

LABEL maintainer="arixixix"

WORKDIR .

COPY . .

CMD ["python", "THIS.py"]
```

Тут команды аналогичны предыдущем командам Docker-файла, за исключением `LABEL maintainer="arixixix"`:

Команда `LABEL maintainer="arixixix"` в Dockerfile используется для добавления метаданных (метки) к Docker-образу. В данном случае, метаданные определяют автора "arixixix", то есть меня. Метки не влияют на работу контейнера, но предоставляют полезную информацию и могут быть использованы для поиска, организации и документирования образов.


### Результат работы команд терминала приведенных выше:

```sh
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```

Также был сделан yml файл для GitHub Action, чтобы продемонстрировать работу двух докер файлов.

### .github/workflows/main.yml

```sh
on:
  push:
    branches:
        master
          
jobs:
  Docker:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v3
        - name: PyFilePic
          run: |
            sudo docker build -t picture ./DockerExp
            sudo docker run picture
            
        - name: PyFileThis
          run: |
            sudo docker build -t this ./DockerThis
            sudo docker run this
          
          
```


## Links

- [Book](https://www.dockerbook.com)
- [Instructions](https://docs.docker.com/engine/reference/builder/)

```
Copyright (c) 2015-2021 The ISC Authors
```
