#Домашнее задание к занятию "08.01 Введение в Ansible"

## Задача 1
`ansible-playbook -i inventory/test.yml site.yml`
```
PLAY [Print os facts] ***************************************************************************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************************************************************************
[WARNING]: Platform darwin on host localhost is using the discovered Python interpreter at /Library/Frameworks/Python.framework/Versions/3.10/bin/python3.10, but future
installation of another Python interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.13/reference_appendices/interpreter_discovery.html for
more information.
ok: [localhost]

TASK [Print OS] *********************************************************************************************************************************************************************
ok: [localhost] => {
   "msg": "MacOSX"
}

TASK [Print fact] *******************************************************************************************************************************************************************
ok: [localhost] => {
   "msg": 12
}

PLAY RECAP **************************************************************************************************************************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
## Задача 2
`./group_vars/all/examp.yml`
```
---
  some_fact: "all default fact"
```
## Задача 3
Сделал через docker
`docker run -d --name=centos7 centos:7 sleep 6000`
`docker run -d --name=ubuntu pycontribs/ubuntu sleep 6000`

## Заздача 4
`ansible-playbook -i ./inventory/prod.yml site.yml`
```
PLAY [Print os facts] ***************************************************************************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************************************************************************
ok: [centos7]
ok: [ubuntu]

TASK [Print OS] *********************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] *******************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el"
}
ok: [ubuntu] => {
    "msg": "deb"
}

PLAY RECAP **************************************************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
## Задача 5 и 6

```
PLAY [Print os facts] ***************************************************************************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************************************************************************
ok: [centos7]
ok: [ubuntu]

TASK [Print OS] *********************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] *******************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP **************************************************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
## Задача 7
```
> ansible-vault encrypt ./group_vars/deb/examp.yml
New Vault password:
Confirm New Vault password:
Encryption successful
> ansible-vault encrypt ./group_vars/el/examp.yml
New Vault password:
Confirm New Vault password:
Encryption successful
```
## Задача 8
```
> ansible-playbook -i ./inventory/prod.yml site.yml --ask-vault-pass
Vault password:

PLAY [Print os facts] ***************************************************************************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************************************************************************
ok: [centos7]
ok: [ubuntu]

TASK [Print OS] *********************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] *******************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP **************************************************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
## Задача 9

`ansible-doc -l` Там их очень много, поиск по ключевым словам  "control node" ни к чему не привёл

## Задача 10

```
---
  el:
    hosts:
      centos7:
        ansible_connection: docker
  deb:
    hosts:
      ubuntu:
        ansible_connection: docker
  local:
    hosts:
      localhost:
        ansible_connection: local
```
## Задача 11

Запустил без создания отдельных переменных. Они прилетели из "all"

```
> ansible-playbook -i ./inventory/prod.yml site.yml --ask-vault-pass
Vault password:

PLAY [Print os facts] ***************************************************************************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************************************************************************
[WARNING]: Platform darwin on host localhost is using the discovered Python interpreter at /Library/Frameworks/Python.framework/Versions/3.10/bin/python3.10, but future
installation of another Python interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.13/reference_appendices/interpreter_discovery.html for
more information.
ok: [localhost]
ok: [centos7]
ok: [ubuntu]

TASK [Print OS] *********************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}
ok: [localhost] => {
    "msg": "MacOSX"
}

TASK [Print fact] *******************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}
ok: [localhost] => {
    "msg": "local default fact"
}

PLAY RECAP **************************************************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
# Необязательная часть

## Задача 1
```
> ansible-vault decrypt ./group_vars/deb/examp.yml
Vault password:
Decryption successful
> ansible-vault decrypt ./group_vars/el/examp.yml
Vault password:
Decryption successful
```
## Задача 2

`echo -n "PaSSw0rd" | ansible-vault encrypt_string`

```
---
 some_fact: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          38323131306262373362303738316437326663356237326534613133306262643230356532353863
          6531353763616430656431656633646166616462373864630a653135376236303261363937626664
          63343132663437663761356461316236306163656534346365313931353539306139316233356364
          3162393464633364340a303038346461333737613962376265353762303366386439303464633165
          3863
```
# Задача 3

```
> ansible-playbook -i inventory/test.yml site.yml --ask-vault-pass
Vault password:

PLAY [Print os facts] **********************************************************************************************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************************************************************************
[WARNING]: Platform darwin on host localhost is using the discovered Python interpreter at /Library/Frameworks/Python.framework/Versions/3.10/bin/python3.10, but future installation of another Python
interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.13/reference_appendices/interpreter_discovery.html for more information.
ok: [localhost]

TASK [Print OS] ****************************************************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "MacOSX"
}

TASK [Print fact] **************************************************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "PaSSw0rd"
}

PLAY RECAP *********************************************************************************************************************************************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## Задача 4

`> cat ./group_vars/rpm/examp.yml`

```
---
  some_fact: "rpm default fact"
```

```
> ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass
Vault password:

PLAY [Print os facts] **********************************************************************************************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************************************************************************
[WARNING]: Platform darwin on host localhost is using the discovered Python interpreter at /Library/Frameworks/Python.framework/Versions/3.10/bin/python3.10, but future installation of another Python
interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-core/2.13/reference_appendices/interpreter_discovery.html for more information.
ok: [localhost]
ok: [centos7]
ok: [fedora]
ok: [ubuntu]

TASK [Print OS] ****************************************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}
ok: [fedora] => {
    "msg": "Fedora"
}
ok: [localhost] => {
    "msg": "MacOSX"
}

TASK [Print fact] **************************************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}
ok: [fedora] => {
    "msg": "rpm default fact"
}
ok: [localhost] => {
    "msg": "PaSSw0rd"
}

PLAY RECAP *********************************************************************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
fedora                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
