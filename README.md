### Тестовое домашнее задание для пресейла __Ansible + Vagrant__:
В целях выполнения требований на знание IT-инфраструктуры для инженера presale, а также умение использования открытых источников информации для решения прикладных задач было выбрано тестовое домашнее задание №1 (Ansible + Vagrant). Тестовое домашнее задание состоит из 3 заданий (Vagrant, Ansible, Docker).
#### Для выполнения ДЗ на сервере поднято следующее окружение:
- Host OS Linux Centos 7;
- Vagrant 2.3.1;
- Ansible 2.9.27, Python version 2.7.5;

#### Задание 1 (Vagrant).
Написан [Vagrantfile](https://github.com/uNkindy/Astra_HW_1/blob/main/Vagrantfile), поднимающий стенд из 3 виртуальных машин (host1, host2, host3) со следующими характеристиками:
1. __host1:__
- OS Centos 7;
- 4Gb RAM;
- CPUs 3;
- port forwarding 8080.
- hostname host1.test.local
2. __host2:__
- OS Centos 7;
- 3Gb RAM;
- CPUs 2;
- port forwarding 8081.
- hostname host2.test.local
3. __host3:__
- OS Almalinux 8;
- 2Gb RAM;
- CPUs 2;
- port forwarding 8082.
- hostname host3.test.local

Настроен vagrant provision для автоматизации распространения конфигурации на все ВМ c помощью ansible.
___
#### Задание 2 (Ansible).
Создан ansible [playbook](https://github.com/uNkindy/Astra_HW_1/blob/main/playbook.yml) для разворачивания конфигурации на 3 хостах последовательно. Плейбук исполняется автоматически при создании ВМ. Плейбук можно разделить на 3 части, каждая часть настраивает конфигурацию на отдельной ВМ:
1. __host1:__
- Производится обновление пакетов nss для последующей установки FreeIpa server;
- Устанавливается FreeIpa server с доменом test.local, устанавливается FreeIpa DNS server. Установка производится при помощи роли [ctl-fed-security.ansible_ipaserver](https://galaxy.ansible.com/ctl-fed-security/ansible_ipaserver), доступной в ansible-galaxy;
- Cоздаются и добавляются 2 новых пользователя user1 и user2 при помощи пакета расширения community.ipa_user для ansible.

2. __host2:__
- Обновляются все пакеты linux для последующей установки Foreman;
- Тимплейтом прокидывается на host2 настройки [hosts](https://github.com/uNkindy/Astra_HW_1/blob/main/templates/hosts.j2);
- Производится установка Foreman из коллекции официальных ролей [Foreman Operation Collection](https://galaxy.ansible.com/theforeman/operations);
- Устанавливается FreeIpa client, регистрирующий ВМ host2 в домене test.local;

3. __host3:__
- Устанавливаются yum utils;
- Добавляется репозиторий docker;
- Устанавливаются пакеты docker c последующим запуском процесса;
- Устанввливаются и запускаются контейнеры Zabbix. Контенеры качаются с официального docker hub zabbix;

В плейбуке также прописаны дефолтные маршруты для работы с веб-страницами устанавливаемых программных решений. Это необходимо, поскольку хост с запускаемыми ВМ не локальный.
___
#### Требования к развертыванию:
- Данный стенд разврачивается командой:
```console
[root@devops Astra_HW_1]# vagrant up host1 host2 host3
```
- После окончания установки стенда командой curl можно проверить доступность 3 ВМ по пробрасываемым портам. Также можно зайти на веб-интерфейсы установленных программных продуктов;
```console
[root@devops Astra_HW_1]# curl localhost:8080
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="https://host1.test.local/ipa/ui">here</a>.</p>
</body></html>

[root@devops Astra_HW_1]# curl localhost:8081
<html><body>You are being <a href="https://localhost/">redirected</a>.</body></html>

[root@devops Astra_HW_1]# curl localhost:8082
<!DOCTYPE html>
<html>
        <head>
                <meta http-equiv="X-UA-Compatible" content="IE=Edge"/>
                <meta charset="utf-8" />
                <meta name="viewport" content="width=device-width, initial-scale=1">
                <meta name="Author" content="Zabbix SIA" />
```