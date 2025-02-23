# Этапы создания репозитория в системе Ubuntu и сборка nginx с модулем brotli
## Среда для работы:
Host:

- Ubuntu 24.04
- ansible ver. 2.16.3
- vagrant 2.4.1
  
Guest:
- Ubuntu 22.04
## Необходимые дополнительные пакеты и библиотеки для сборки пакета их исходников
Весь список находится в yml файле  [ansible/roles/repo/vars/main.yml](ansible/roles/repo/vars/main.yml)
Закомментируемые пакеты уже входят в состав дистрибутива Ubuntu 22.04, для другой системы они могут понадобиться.
## Этапы сборки Nginx с модулем Brotli
1. Скачивание паке nginx с исходниками в созданную папку repo
2. Скачивание Brotli в папку с модулями nginx (repo/nginx-1.18.0/debian/modules) из ресурса [https://github.com/google/ngx_brotli](https://github.com/google/ngx_brotli)
   

