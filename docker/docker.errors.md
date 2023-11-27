###

failed to solve: failed to register layer: write /usr/local/include/node/openssl/evp.h: no space left on device

Пробовал удалить вообще все старые image и container + прогнать
() > docker prune
() > docker image prune
() > docker volume prune
() > docker network prune

Всё равно, сильно место не почистилось.

В итоге, нагуглил вот такую команду которая выручила
() > docker system prune -f
