
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
[docker-compose.yml](docker-compose.yaml)
![DZ1-5-1](https://github.com/martishinid/DZ1/assets/121010186/8cae6bd9-78d2-4152-bc8f-2a4b7137564a)

3. 
![DZ1-5-2](https://github.com/martishinid/DZ1/assets/121010186/502ec34a-2696-471c-a429-bef262d2e000)



![DZ1-5-3](https://github.com/martishinid/DZ1/assets/121010186/fb5ece6a-e2e1-4d5e-ac76-282f153c36cf)

7. 

![DZ1-5-4](https://github.com/martishinid/DZ1/assets/121010186/40f41c6a-bb76-4db4-84ff-a560f0efdf5a)
![DZ1-5-5](https://github.com/martishinid/DZ1/assets/121010186/a454c336-abff-4697-ab8c-5003b260dd47)



