# Этапы создания репозитория в системе Ubuntu и сборка nginx с модулем brotli
## Среда для работы:
Host:

- Ubuntu 24.04
- ansible ver. 2.16.3
- vagrant 2.4.1
  
Guest:
- Ubuntu 22.04
## Необходиые дополнительные пакеты и библиотеки для сборки пакета их исходников
Весь список находится в yml файле  [Repozitory/ansible/roles/repo/vars/main.yml](Repozitory/ansible/roles/repo/vars/main.yml)
"Repozitory/ansible/roles/repo/vars/main.yml"
Закомментируемы пакеты уже входят в состав дистрибутива Ubuntu 22.04, для другой системы они могут понадобиться.

