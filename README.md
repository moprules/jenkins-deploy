# jenkins-deploy
Простой способ развернуть Jenkins с SSL сертификатами на вашем сервере. Также сертификаты будут обновляться автоматически.

## 1. Первоначальная настройка
Перед использованием задайте в файле [__.env__](.env) доменное имя сервера, на котором разворачивается jenkins.

Например так:
```
DOMAIN=jenkins.mysite.ru 
```

Также не удаляйте переменную среды [__$DOLLAR__](.env). Она нужна для корректного считывания шаблонов настроек nginx.

## 2. Запуск jenkins
Всё что нужно сделать - это запустить комманду ниже:
```console
docker compose up -d --build
```

## 3. Узнать секретный ключ для администратора
В браузере перейдите по указанному вами доменному имени.

При первом входе в панель jenkins нужно ввести секретный пароль, который можно получить следующей коммандой:
```console
docker compose exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

# Дополнительные комманды

## Остановка jenkins
```console
docker compose down
```

## Если нужно удалить старые тома от jenkins
```console
docker volume rm -f cicd_jenkins_home
```

## Удаление всех контейнеров
```console
docker rm -f $(docker ps -aq)
```

## Удаление всех скаченных или собранных образов
```console
docker rmi -f $(docker images -aq)
```

## Удаление всех томов
```console
docker volume rm -f $(docker volume ls -q)
```

## Очистка кеша и удаление ненужных данных, образов и тд
```console
docker system prune -f --volumes
```

## При ручной настройке webhooks в url нужно ввести строку такого типа
```console
https://USERNAME:TOKEN_JENKINS_API@YOURDOMAIN.COM/project/PROJECTNAME
```

Например:

```console
https://user1:114bc4dfc81f3a9c26b1903111d7fda4be@example.org/project/example-project
```
