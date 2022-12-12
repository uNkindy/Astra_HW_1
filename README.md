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
2. __host2:__
- OS Centos 7;
- 3Gb RAM;
- CPUs 2;
- port forwarding 8081.
3. __host3:__
- OS Almalinux 8;
- 2Gb RAM;
- CPUs 2;
- port forwarding 8082.

Настроен vagrant provision для автоматизации распространения конфигурации на все ВМ.
___
#### Задание 2 (Ansible).
Создано 3 плейбука для разворачивания конфигурации на 