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
Создан ansible [playbook](https://github.com/uNkindy/Astra_HW_1/blob/main/playbook.yml) для разворачивания конфигурации на 3 хостах последовательно. #Плейбук исполняется автоматически при создании ВМ. Плейбук можно разделить на 3 части, каждая часть настраивает конфигурацию на отдельной ВМ:
1. __host1:__
- Производится обновление пакетов nss для последующей установки FreeIpa server;
- Устанавливается FreeIpa server с доменом test.local, устанавливается FreeIpa DNS server. Установка производится при помощи роли [ctl-fed-security.ansible_ipaserver](https://galaxy.ansible.com/ctl-fed-security/ansible_ipaserver), доступной в ansible-galaxy.
- Cоздаются и добавляются 2 новых пользователя user1 и user2 при помощи пакета расширения community.ipa_user для ansible.

2. __host2:__