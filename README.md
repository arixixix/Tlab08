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

Следующие команды (создание docker image и его запуск) были выполнены для проверки работы:

```sh
% sudo docker build -t picture ./DockerExp
% sudo docker run picture
```

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
