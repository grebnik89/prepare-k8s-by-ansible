Role Name
=========

swap_kernel

Requirements
------------

VM connection by ssh and ansible user

Role Variables
--------------

Based on group_vars/all.yml, contain version of containerd, runc and CNI plugins

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).

```bash
sudo swapoff -a # отключаем свап совсем (либо надо будет отдельно включать толлерантность к свап)
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab # сохранит состояние после перезагрузки
echo 'net.ipv4.ip_forward=1' | sudo tee -a /etc/sysctl.conf # включает IP-форвардинг. Без этого поды не смогут: Общаться между собой на разных нодах, Выходить во внешнюю сеть (через NAT), Принимать входящие соединения
echo 'net.bridge.bridge-nf-call-iptables=1' | sudo tee -a /etc/sysctl.conf # обеспечивает, чтобы трафик через bridge-интерфейсы проходил через iptables. Это нужно для: Работы Service mesh и Network Policies; Корректной маршрутизации трафика между подами; Безопасности кластера
sudo modprobe br_netfilter # подгрузить модуль netfilter, чтобы появилась настройка net.bridge.bridge-nf-call-iptables
```
