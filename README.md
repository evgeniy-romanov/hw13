# **Введение**
# **Docker**
## *Домашнее задание* :
1. Создайте свой кастомный образ nginx на базе alpine. После запуска nginx должен отдавать кастомную страницу (достаточно изменить дефолтную страницу nginx)
2. Определите разницу между контейнером и образом. Вывод опишите в домашнем задании.
3. Ответьте на вопрос: Можно ли в контейнере собрать ядро?
4. Собранный образ необходимо запушить в docker hub и дать ссылку на ваш репозиторий.
5. Задание со * (звездочкой) Создайте кастомные образы nginx и php, объедините их в docker-compose. После запуска nginx должен показывать php info. Все собранные образы должны быть в docker hub.

---

### 1. Создание кастомного образа nginx и публикация его в docker hub 

 Устнавливаем Docker на хост машину (Ubuntu) по следующей инструкции - https://docs.docker.com/engine/install/ubuntu/. Создаём Dockerfile следующего содержания.
```
FROM nginx:1.21.6-alpine
LABEL maintainer="evgeniy.romanov@mail.ru"
# Задаём текущую рабочую директорию
WORKDIR /usr/share/nginx/html/
# Копируем код из локального контекста в рабочую директорию образа
COPY index.html /usr/share/nginx/html/index.html
# Указываем порт
EXPOSE 80
# Указываем Nginx запускаться на переднем плане (daemon off)
ENTRYPOINT ["nginx", "-g", "daemon off;"]
```
Собираем образ nginx на базе alpine.
```
evgeniy@home:~/hw13$ docker build -t evgeniy1986romanov/nginx-alpine-otus:v1 .

Sending build context to Docker daemon  231.4kB
Step 1/6 : FROM nginx:1.21.6-alpine
1.21.6-alpine: Pulling from library/nginx
df9b9388f04a: Pull complete 
a285f0f83eed: Pull complete 
e00351ea626c: Pull complete 
06f5cb628050: Pull complete 
32261d4e220f: Pull complete 
9da77f8e409e: Pull complete 
Digest: sha256:a74534e76ee1121d418fa7394ca930eb67440deda413848bc67c68138535b989
Status: Downloaded newer image for nginx:1.21.6-alpine
 ---> b1c3acb28882
Step 2/6 : LABEL maintainer="evgeniy.romanov@mail.ru"
 ---> Running in 6898e7bf7c27
Removing intermediate container 6898e7bf7c27
 ---> b5fe6f43d579
Step 3/6 : WORKDIR /usr/share/nginx/html/
 ---> Running in 98863637ed99
Removing intermediate container 98863637ed99
 ---> 70aaf5f1d560
Step 4/6 : COPY index.html /usr/share/nginx/html/index.html
 ---> 3ff36a1c4d14
Step 5/6 : EXPOSE 80
 ---> Running in 9875397840ce
Removing intermediate container 9875397840ce
 ---> e1f9a1462a60
Step 6/6 : ENTRYPOINT ["nginx", "-g", "daemon off;"]
 ---> Running in a615bd44b624
Removing intermediate container a615bd44b624
 ---> 4ab5e3e26a01
Successfully built 4ab5e3e26a01
Successfully tagged evgeniy1986romanov/nginx-alpine-otus:v1

```
Логинюсь в doker hub и отправляю туда полученный образ
```
evgeniy@home:~/hw13$ docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: evgeniy1986romanov
Password: 
WARNING! Your password will be stored unencrypted in /home/evgeniy/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```

```
evgeniy@home:~/hw13$ docker push evgeniy1986romanov/nginx-alpine-otus:v1
The push refers to repository [docker.io/evgeniy1986romanov/nginx-alpine-otus]
353c71d5bb8a: Pushed 
c0e7c94aefd8: Pushed 
d6dd885da0bb: Pushed 
a43749efe4ec: Pushed 
45b275e8a06d: Pushed 
4721bfafc708: Pushed 
4fc242d58285: Pushed 
v1: digest: sha256:aea1791926f9dc06867a2af07bc4b22973cb3090e32ced6b6203aaaeff02cc0b size: 1775
```
Запускаем наш image и проверяем работоспособность нашего контейнера путём проверки нашей изменённой тестовой странички.
```
evgeniy@home:~/hw13$ docker run -d -p 8080:80 evgeniy1986romanov/nginx-alpine-otus:v1
8ce7f3ed9ed8e60fa275377e50b39f16c5322c87414bf86854724ada134d4694

evgeniy@home:~/hw13$ curl http://localhost:8080
<!DOCTYPE html>

<html lang="ru">
<head>

<meta charset="UTF-8">

<title> Test page nginx</title>
</head>

<body>

<h1> Test page for Docker - Nginx <h1>
</body>

</html>

```
Проверим в браузере
#
![alt text](https://github.com/evgeniy-romanov/hw13/raw/main/1.png)
#
### 2. Определите разницу между контейнером и образом Docker-образ - это шаблон, на основе которого запускается контейнер. Шаблон по своей сути неизменяем, на его основе может быть запущен один или несколько одинаковых инстансов, в процессе работы которых в них могут произойти изменения, но существовать эти изменения будут только до рестарта контейнера. В свою очередь, произведя изменения в запущенном контейнере, можно сохранить его в новый образ.
#
### 3. Можно ли в контейнере собрать ядро? Собрать ядро можно, но использовать его для загрузки в контейнере не получится - контейнер буде запущен на основе ядра хостовой системы.
#
## 4. **Ссылка на репозиторий - https://hub.docker.com/r/evgeniy1986romanov/nginx-alpine-otus**















