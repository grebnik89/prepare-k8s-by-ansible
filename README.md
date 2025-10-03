# k8s-cluster

### Проект для подготовки и развертывания кластера k8s c помощью kubeadm.

Для работы необходимо: 
1. Развернуть от 1 вм для мастер ноды и от 2х вм для воркер нод, получить IP адреса
2. На каждой ноде создать пользователя ansible и настроить ssh подключение
3. Создать inventory, куда прописать IP машин, например:

```yaml
---
all:
  children:
    k8s_cluster:
      children:
        master:
          hosts: 
            master_node:
              ansible_host: <master_IP>
        workers:
          hosts:
            worker1_node:
              ansible_host: <worker1_IP>
            worker2_node:
              ansible_host: <worker2_IP>

```

4. Проверить доступность с помощью ping

ps: проверить работу DNS, в случае некорректного резолвинга - исправить. В моем случае, для KVM/libvirt нужно выполнить задачу с тэгом debug_fix 

### Что происходит после запуска playbook:

1. Отключение swap (roles/swap_kernel_DNS), настройка параметров ядра я корректной работы сети, настройка DNS
2. Container Runtime установка (roles/container_runtime), необходимо передать переменную "fix_dns=true"
3. Kubeadm, kubectl, kubelet установка (roles/kube_components), необходимо передать переменную "fix_dns=true"