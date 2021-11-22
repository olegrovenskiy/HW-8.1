# Самоконтроль выполненения задани

1. Где расположен файл с some_fact из второго пункта задания?

/etc/ansible/HW-8.1/group_vars/all

2. Какая команда нужна для запуска вашего playbook на окружении test.yml?

vagrant@vagrant:/etc/ansible/HW-8.1$ ansible-playbook site.yml -i inventory/test.yml


3. Какой командой можно зашифровать файл?

sudo ansible-vault encrypt <filename>

4. Какой командой можно расшифровать файл?

ansible-vault decrypt <filename>

5. Можно ли посмотреть содержимое зашифрованного файла без команды расшифровки файла? 
Если можно, то как?

да, можно, например через view или редактор

ansible-vault edit <filename> #Отредактировать зашифрованный
файл
# ansible-vault view <filename> #Просмотреть зашифрованный файл

6. Как выглядит команда запуска playbook, если переменные зашифрованы?

vagrant@vagrant:/etc/ansible/HW-8.1$ sudo ansible-playbook site.yml -i inventory/prod.yml --ask-vault-pass


ключ --ask-vault-pass

7. Как называется модуль подключения к host на windows?

winrm                          Run tasks over Microsoft's WinRM


8. Приведите полный текст команды для поиска информации в документации ansible для модуля подключений ssh

vagrant@vagrant:/etc/ansible/HW-8.1$ ansible-doc -t connection ssh


9. Какой параметр из модуля подключения ssh необходим для того, чтобы определить пользователя, 
под которым необходимо совершать подключение?

- remote_user
        User name with which to login to the remote server, normally set by the remote_user keyword.
        If no user is supplied, Ansible will let the ssh client binary choose the user as it
        normally
        [Default: (null)]
        set_via:
          env:
          - name: ANSIBLE_REMOTE_USER
          ini:
          - key: remote_user
            section: defaults
          vars:
          - name: ansible_user
          - name: ansible_ssh_user

        cli:
        - name: user





Пункты домашней работы

Подготовка

1. Установите ansible версии 2.10 или выше.

 Ubuntu 20.04.2 LTS

$ sudo apt update
$ sudo apt install software-properties-common
$ sudo add-apt-repository --yes --update ppa:ansible/ansible
$ sudo apt install ansible


vagrant@vagrant:~$ ansible --version
ansible [core 2.11.6]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  ansible collection location = /home/vagrant/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.8.10 (default, Sep 28 2021, 16:10:42) [GCC 9.3.0]
  jinja version = 2.10.1
  libyaml = True
vagrant@vagrant:~$





2. Создайте свой собственный публичный репозиторий на github с произвольным именем.

https://github.com/olegrovenskiy/HW-8.1

3. Скачайте playbook из репозитория с домашним заданием и перенесите его в свой репозиторий.

vagrant@vagrant:/etc/ansible/HW-8.1$ ls -l
total 16
drwxr-xr-x 5 root root 4096 Nov 21 10:09 group_vars
drwxr-xr-x 2 root root 4096 Nov 21 10:10 inventory
-rw-r--r-- 1 root root 1293 Nov 21 10:10 README.md
-rw-r--r-- 1 root root  209 Nov 21 10:10 site.yml
vagrant@vagrant:/etc/ansible/HW-8.1$

Основная часть

1. Попробуйте запустить playbook на окружении из test.yml, 
зафиксируйте какое значение имеет факт some_fact для указанного хоста при выполнении playbook'a.

12

TASK [Print fact] ******************************************************************************************************
ok: [localhost] => {
    "msg": 12


2. Найдите файл с переменными (group_vars) в котором задаётся найденное в 
первом пункте значение и поменяйте его на 'all default fact'.

/etc/ansible/HW-8.1/group_vars/all

3. Воспользуйтесь подготовленным (используется docker) или 
создайте собственное окружение для проведения дальнейших испытаний.

vagrant@vagrant:/etc/ansible/HW-8.1$ sudo docker ps
CONTAINER ID   IMAGE                      COMMAND           CREATED          STATUS          PORTS     NAMES
e9c037a50161   pycontribs/ubuntu:latest   "sleep 6000000"   18 seconds ago   Up 16 seconds             ubuntu
82e6d0121ce0   pycontribs/centos:7        "sleep 6000000"   3 minutes ago    Up 3 minutes              centos7
vagrant@vagrant:/etc/ansible/HW-8.1$


4. Проведите запуск playbook на окружении из prod.yml. Зафиксируйте полученные значения some_fact 
для каждого из managed host.

TASK [Print fact]****************ok: [ubuntu] => {
    "msg": "deb"
}
ok: [centos7] => {
    "msg": "el"
}


5. Добавьте факты в group_vars каждой из групп хостов так, 
чтобы для some_fact получились следующие значения: для deb - 'deb default fact', для el - 'el default fact'.


6. Повторите запуск playbook на окружении prod.yml. Убедитесь, что выдаются корректные 
значения для всех хостов.


TASK [Print fact] ****************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

7. При помощи ansible-vault зашифруйте факты в group_vars/deb и group_vars/el с паролем netology.

vagrant@vagrant:/etc/ansible/HW-8.1/group_vars/deb$ sudo cat examp.yml
$ANSIBLE_VAULT;1.1;AES256
61633065656665623731313836343639393865366130333065356332346239626331656561306662
3264356661366533343962306365656165393438313831370a336430356537633730313239353163
35333530346131396335643738646539623538396430643337626131666136363163636564663932
3230326239386333660a353263303161363037336566643235633439326238316465323933623932
35353533383535623665363736643932626632326233396532393331346662323330333161393938
3139623961613561383965626538313966333239623536343637
vagrant@vagrant:/etc/ansible/HW-8.1/group_vars/deb$


vagrant@vagrant:/etc/ansible/HW-8.1/group_vars/el$ sudo cat examp.yml
$ANSIBLE_VAULT;1.1;AES256
37353765353766643032343566316534393364303363376439343761393765366163626236303166
3765363338613036343363616130363835653535383634340a656361353134616533303837656438
64313131323662613630643261633935303132306563663861643632303136376532386434396230
6162626438323432650a363932633439356566626437323939363165646361643538303737646538
30316432666432323939376438316166633036613765333665363539616138666433303861386563
6230393561303437653566623465623537363262343265326466
vagrant@vagrant:/etc/ansible/HW-8.1/group_vars/el$

8. Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. 
Убедитесь в работоспособности.

vagrant@vagrant:/etc/ansible/HW-8.1$ sudo ansible-playbook site.yml -i inventory/prod.yml --ask-vault-pass
Vault password:

PLAY [Print os facts] *******************************************************************************************************

TASK [Gathering Facts] ******************************************************************************************************
[DEPRECATION WARNING]: Distribution Ubuntu 18.04 on host ubuntu should use /usr/bin/python3, but is using /usr/bin/python
for backward compatibility with prior Ansible releases. A future Ansible release will default to using the discovered
platform python for this host. See https://docs.ansible.com/ansible-
core/2.11/reference_appendices/interpreter_discovery.html for more information. This feature will be removed in version
2.12. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] *************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ***********************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP ******************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

vagrant@vagrant:/etc/ansible/HW-8.1$


9. Посмотрите при помощи ansible-doc список плагинов для подключения. 
Выберите подходящий для работы на control node.

 ansible-doc -t connection -l

  local

10. В prod.yml добавьте новую группу хостов с именем local, в ней разместите localhost с 
необходимым типом подключения.



11. Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. 
Убедитесь что факты some_fact для каждого из хостов определены из верных group_vars.

vagrant@vagrant:/etc/ansible/HW-8.1$ sudo ansible-playbook site.yml -i inventory/prod.yml --ask-vault-pass
Vault password:

PLAY [Print os facts] *******************************************************************************************************

TASK [Gathering Facts] ******************************************************************************************************
ok: [localhost]
[DEPRECATION WARNING]: Distribution Ubuntu 18.04 on host ubuntu should use /usr/bin/python3, but is using /usr/bin/python
for backward compatibility with prior Ansible releases. A future Ansible release will default to using the discovered
platform python for this host. See https://docs.ansible.com/ansible-
core/2.11/reference_appendices/interpreter_discovery.html for more information. This feature will be removed in version
2.12. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] *************************************************************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ***********************************************************************************************************
ok: [localhost] => {
    "msg": "all default fact"
}
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP ******************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    



