
# Домашнее задание к занятию 4 «Оркестрация группой Docker контейнеров на примере Docker Compose»

### Инструкция к выполению

## Задача 1
Создан образ на основе Nginx версии 1.21.1 - [martishinid/custom-nginx](https://hub.docker.com/repository/docker/martishinid/custom-nginx/general).
В нем изменена главная страница. https://hub.docker.com/repository/docker/martishinid/custom-nginx/general

## Задача 2
1. Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
- имя контейнера "ФИО-custom-nginx-t2"
- контейнер работает в фоне
- контейнер опубликован на порту хост системы 127.0.0.1:8080
  ![DZ1-2-1](https://github.com/martishinid/DZ1/assets/121010186/4921187a-4b3e-481d-8948-83919a2f7f58)

2. Переименуйте контейнер в "custom-nginx-t2"
![DZ1-2-2](https://github.com/martishinid/DZ1/assets/121010186/863acd44-df2f-440c-9a73-7fdc94b25c2b)

3. Выполните команду ```date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080  ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html```
4. Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.
![DZ1-2-2-34](https://github.com/martishinid/DZ1/assets/121010186/7b472ab6-43e9-4c22-a74f-f090dcdaa9fe)



## Задача 3
1. Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2".
2. Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.
3. Выполните ```docker ps -a``` и объясните своими словами почему контейнер остановился.
# Ответ:
```docker exec -it custom-nginx-t2 bash```
Подключившись с контейнеру и нажав Ctrl-C мы завершим работу контейнера, так как контейнер это процесс в системе.
4. Перезапустите контейнер
5. Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.
6. Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.
7. Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".
8. Запомните(!) и выполните команду ```nginx -s reload```, а затем внутри контейнера ```curl http://127.0.0.1:80 ; curl http://127.0.0.1:81```.
![DZ1-3-8](https://github.com/martishinid/DZ1/assets/121010186/190bbfbb-1cd8-4dc1-9ad4-0ad462c0150c)

9. Выйдите из контейнера, набрав в консоли  ```exit``` или Ctrl-D.
10. Проверьте вывод команд: ```ss -tlpn | grep 127.0.0.1:8080``` , ```docker port custom-nginx-t2```, ```curl http://127.0.0.1:8080```. Кратко объясните суть возникшей проблемы.

![DZ1-3-10](https://github.com/martishinid/DZ1/assets/121010186/62f92b00-a7cc-43b6-b30c-6c8da8b187ef)
Проблема в том, что  изначально контейнер пробрасывает порт на 80 во внуторь, для того чтобы изменить проброс необходимо либо пересобирать контейнер, либо править конфигурационный файл, представленный ниже.
![DZ1-3-11](https://github.com/martishinid/DZ1/assets/121010186/0ba021f5-7249-4846-b762-6565bb0c4561)

11. * Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер. Останавливать контейнер можно. [пример источника](https://www.baeldung.com/linux/assign-port-docker-container)
12. Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)
![DZ1-3-12](https://github.com/martishinid/DZ1/assets/121010186/f537ac89-3b9d-45ce-adf8-3139c1fb016d)

## Задача 4

1.
```
docker run -v '/$(pwd):/data' --name centos -it centos
docker run -v '/$(pwd):/data' --name debian -it debian
```
2. Проверка содержимого папки в обоих контейнерах
   ```
   ls /data
   ```
   
![DZ1-4-1](https://github.com/martishinid/DZ1/assets/121010186/526ce93b-1d9c-429f-976f-6e12e9e34f8a)




## Задача 5

1. Создайте отдельную директорию(например /tmp/netology/docker/task5) и 2 файла внутри него.

[compose.yml](compose.yaml)
2. Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла. (подсказка: https://docs.docker.com/compose/compose-file/14-include/)

3. Выполните в консоли вашей хостовой ОС необходимые команды чтобы залить образ custom-nginx как custom-nginx:latest в запущенное вами, локальное registry. Дополнительная документация: https://distribution.github.io/distribution/about/deploying/
4. Откройте страницу "https://127.0.0.1:9000" и произведите начальную настройку portainer.(логин и пароль адмнистратора)
5. Откройте страницу "http://127.0.0.1:9000/#!/home", выберите ваше local  окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:

```
version: '3'

services:
  nginx:
    image: 127.0.0.1:5000/custom-nginx
    ports:
      - "9090:80"
```
6. Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".

7. Удалите любой из манифестов компоуза(например compose.yaml).  Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод, файл compose.yaml , скриншот portainer c задеплоенным компоузом.

---

### Правила приема

Домашнее задание выполните в файле readme.md в GitHub-репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.
