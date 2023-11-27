# Delete <none> images/containers

() > docker image prune (y)
() > docker container prune (y)
() > docker system prune
(?) > docker rmi $(docker images -f "dangling=true" | grep "<none>.\*<none>" | awk '{ print $3; }')
