### Ansible

Главное чтобы был python
Зачем?:

- Автоматизация повторяющихся задач(если несколько серверов)
- Автоматизация сложных задач
- Концепция IAC (инфраструктура как код)

  Какие задачи решаем?:

- Человеческий фактор
- Недостаток прозрачности настроек и документации
- Сложность повторения
- Экономия времени
- Переносимость решений

Плюсы Ansible:

- Нет дополнительного ПО - используется python
- Можно легко дописывать свои модули
- Низкий порог вхождения
- Возможность интеграции API через AWX

Подули и плагины - подключаемые блоки внутри нашей Ansible.
Модули выполняются на клиентах и выполняются при подключении.
Плагины выполняются на хост машине и до подключения

- Installing
  () > brew install ansible
  () > ansible --version

- Inventory
  Описание ваших хостов
- (name group) [demo]
  () > ansible -i hosts.ini -m ping demo

- Модули - отдельные блоки кода, которые можно использовать для выполнения команд на хостах и сборка возвращаемых значений.
  Часть встроено, часть можно установить
  () > ansible webservers -m service -a "name=httpd state=started"
  google - ansible module list: https://docs.ansible.com/ansible/2.9/modules/list_of_all_modules.html
  Не нужно писать свои на python

- Ad-hoc
  () > ansible + (inventory) -i hosts + (module) -m user + (arguments) -a "name=daria state=present" + (hosts) all
  (see ansible.ad-hoc.command)
  Вместо playbook
  Это команда для быстрого выполнения скрипта, которую ты не хочешь сохранять для дальнейшего использования.
- Для быстрых фиксов (упал сервис, быстро переподнять)
- Для получения информации с сервера (например, получить ip серверов)
- Для тестирования отдельных команда
  () > ansible -i hosts -m user -a "name=iadaria state=present" all
  (check existed user and create user) > ansible -i hosts.ini -m user -a "name=root state=present" demo
  (del user)> ansible -i hosts.ini -m user -a "name=root2 state=absent" -b -K demo
  (del user)> ansible -i hosts.ini -m user -a "name=root2 state=absent" -e "ansible_become=true ansible_become_password=123" demo
  (3th approach - bad idea ) >
  ```ini
  [demo]
  45.144.28.204 ansible_user=root ansible_port=22 ansible_become=true ansible_become_password=123
  ```

### Ansible playbooks

Все скрипты пишем в playbooks.
Это описание конфигурации, оркестрации или выкладки. Оно описываем состояние удаленной системы или шаги отдельного процесса.
(see ansible.playbook.structure)
(-K ask password) > ansible-playbook -i hosts.ini user.yml -K
(with folder demo-server) > ansible-playbook -i demo-server user.yml -K

### Переменные:

- Переиспользование в разных местах playbook
- Использование различных значений для разных окружений и серверов
- Агрегация конфигов в одном месте

1. Нельзя использовать:

- ключевые слова python (async)
- \*my_var
- my var
- my.var
- my-var
- 5my_var
- Зарезервированные слова playbooks(environment)

2. Где задавать:

- playbook
- block
- tasks
- group_vars
- host_vars
- inventory
- extra_vars
  (mountain) > ansible-playbook -i demo-server user.yml -K --extra-vars "user=daria"

  3. Порядок переменных которые повторяются
     (see ansible.vars)

  4. Debug
     (debugger: always) > p task > p task.name > p task.args
     (show arg user) > p task.vars
     (show all variables) > p task_vars
     () > p task_vars['inventory_hostname']
     () > p task.vars['user]
     (change arguments) > task_args['name']='daria2'
     () > task.args['state']='absent'
     (step forward) > u
     (step forward) > c
     (step backward) >
  5. Blocks
     (ansible.blocks)
  6. Async tasks
     (see ansible.async)
  7. Ansible lint
     () > brew install ansible-lint
     () > ansible-lint config.yml
     () > brew install pre-commit
     () > pre-commit install
     () > mkdir .pre-commit.config.yaml

### Vagrant

Чтобы работать с docker-swarm нам нужен кластер машин, для того чтобы сделать кластер, нам либо нужно поднять их руками, либо автоматизировать.
Целые виртуальные машины.
https://www.vagrantup.com/download
(installing) > brew tap hashicorp/tap
() > brew install hashicorp/tap/hashicorp-vagrant
() > vagrant --version

### Docker swarm

### Ansible advance

### Deploy приложения на кластер
