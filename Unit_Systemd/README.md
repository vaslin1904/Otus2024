# **Systemd**
___________________________________________
## **Создание сервиса мониторинга**
___________________________________________
1. Размещаем конфиг **watchlog** в директорию /etc/default со значением переменных:
   - $WORD - устанавливается слова для поиска. В данной работе "Alert"
   - $LOG - полное имя лога, в котором будет произодиться поиск (/var/log/watchlog.log).
2. Размещаем **watchlog.log** в директорию /var/log.
3. Размещаем скрипт [watchlog.sh](ansible/roles/my_service/files/opt/watchlog.sh) в директорию /opt, который при нахождении эталонного слова в заданном логе с помощью команды **logger** помещает сообщение в системный лог.
4. Создаем Unit-service systemd [watchlog.service](ansible/roles/my_service/files/etc/systemd/system/watchlog.service),
который передает параметры из [/etc/default/watchlog](ansible/roles/my_service/files/etc/default/watchlog)
в скрипт [watchlog.sh](ansible/roles/my_service/files/opt/watchlog.sh) и запускает его.
5. Создаем Unit-timer systemd [watchlog.timer](ansible/roles/my_service/files/etc/systemd/system/watchlog.timer),
который каждый 30 секунд после первого запуска будет запускать watchlog.service.
При совпадении имен таймер сам будет искать необходимый сервис, если он был запущен хотя бы один раз.
6. После размещения новых Unit в директорию /etc/systemd/system Необходимо перезапустить systemd <br>
                  **sudo systemctl daemon-reload**
7. Разово запустим сервис watchlog, а затем запускаем одноименный таймер.
8. Проверим работу системы мониторинга:<br>
              **tail -n /var/log/syslog | grep word**<br>
_____________________________________________________________________
## **Установить spawn-fcgi и создать unit-файл (spawn-fcgi.sevice) с помощью переделки init-скрипта**
_____________________________________________________________________
1. Устанавливаем spawn-fcgi и необходимые для него пакеты, заодно и nginx для следующего этапа<br>
([список пакетов - install_pkgs](ansible/roles/my_service/vars/main.yml)
2. Инит файл [[https://gist.github.com/cea2k/1318020](https://gist.github.com/cea2k/1318020 )
3. Размещаем конфиг [fcgi.conf](ansible/roles/my_service/files/etc/spawn-fcgi/fcgi.conf) в директорию /etc/spawn-fcgi
3. Создаем Unit-service systemd [spawn-fcgi.service](ansible/roles/my_service/files/etc/systemd/system/spawn-fcgi.service).
 в директории /etc/systemd/system
4. После размещения новых Unit в директорию /etc/systemd/system Необходимо перезапустить systemd <br>
                  **sudo systemctl daemon-reload**
5. Запускаем созданный сервис и проверяем его статус <br>
    **sudo systemctl start spawn-fcgi.service**
    **sudo systemctl status spawn-fcgi.service**
______________________________________________
## **Доработать unit-файл Nginx (nginx.service) для запуска нескольких инстансов сервера с разными конфигурационными файлами одновременно**
______________________________________________
1. Cоздадим новый Unit-service systemd [nginx@.service](ansible/roles/my_service/files/etc/systemd/system/nginx@.service) для работы с шаблонами
в директории /etc/systemd/system
Имя экземпляра  в шаблоне имеет вид - **%I** (ExecStart=/usr/sbin/nginx -c /etc/nginx/nginx-**%I**.conf -g)
2. После размещения новых Unit в директорию /etc/systemd/system Необходимо перезапустить systemd <br>
                  **sudo systemctl daemon-reload**
4.  Создаем два файла конфигурации из стандартного конфига /etc/nginx/nginx.conf с модификацией путей до PID-файлов и разделением по портам:
   - [/etc/nginx/nginx-first.conf](ansible/roles/my_service/files/etc/nginx/nginx-first.conf)
   - [/etc/nginx/nginx-second.conf](ansible/roles/my_service/files/etc/nginx/nginx-second.conf)
5. Проверяем работу:
* Запускаем сервисы: **sudo systemctl start nginx@first**
                     **sudo systemctl start nginx@second**
*Проверяем их статус: **sudo systemctl status nginx@first**
                      **sudo systemctl status nginx@second**
*Или Посмотрим какие порты слушаются: **ss -tnulp | grep nginx**
*Или Посмотрим список процессов: **ps afx | grep nginx**
____________________________________________
## **Вывод**
В данной работе рассмотрели создание юнитов systemd, их запуска. Рассмотрели способ одновременного запуска сервиса с разными конфигурационными файлами.
_____________________________________________
Выполнение работы автоматизировано с помощью playbook ansible

