# Домашнее задание к занятию «Как работает сеть в K8s»-***Вуколов Евгений***
 
### Цель задания
 
Настроить сетевую политику доступа к подам.
 
### Чеклист готовности к домашнему заданию
 
1. Кластер K8s с установленным сетевым плагином Calico.
 
### Инструменты и дополнительные материалы, которые пригодятся для выполнения задания
 
1. [Документация Calico](https://www.tigera.io/project-calico/).
2. [Network Policy](https://kubernetes.io/docs/concepts/services-networking/network-policies/).
3. [About Network Policy](https://docs.projectcalico.org/about/about-network-policy).
 
-----
 
### Задание 1. Создать сетевую политику или несколько политик для обеспечения доступа
 
1. Создать deployment'ы приложений frontend, backend и cache и соответсвующие сервисы.
2. В качестве образа использовать network-multitool.
3. Разместить поды в namespace App.
4. Создать политики, чтобы обеспечить доступ frontend -> backend -> cache. Другие виды подключений должны быть запрещены.
5. Продемонстрировать, что трафик разрешён и запрещён.
 
### Правила приёма работы
 
1. Домашняя работа оформляется в своём Git-репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд, а также скриншоты результатов.
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.


# **Решение**

### Задание 1.

- Вначале задания разворачиваю K8s на VM ubuntu 22.04 в Yandex cloud с помощью bash скрипта [k8s_setup.sh](https://github.com/Evgenii-379/3.3-3.3.md-k8s/blob/main/k8s_setup.sh)

- Создаю namespace App:
```
kubectl create namespace app
```
- ![scrin](https://github.com/Evgenii-379/3.3-3.3.md-k8s/blob/main/Снимок%20экрана%202025-04-09%20173753.png)

```
# Иициализация:

sudo kubeadm init --pod-network-cidr=10.244.0.0/16

mkdir -p $HOME/.kube
sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

# Установка Calico как сетевой плагин:

kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.26.1/manifests/calico.yaml

```
- ![scrin](https://github.com/Evgenii-379/3.3-3.3.md-k8s/blob/main/Снимок%20экрана%202025-04-09%20173653.png)

- Создание deployment'ов и сервисов, а так же развёртывание: 

- ![scrin](https://github.com/Evgenii-379/3.3-3.3.md-k8s/blob/main/Снимок%20экрана%202025-04-09%20174224.png)

- Создание политик доступа:

- ![scrin](https://github.com/Evgenii-379/3.3-3.3.md-k8s/blob/main/Снимок%20экрана%202025-04-09%20174436.png)

- Проверяю работу политик подключения:

Проверка разрешенных подключений:

Frontend -> Backend

- ![scrin](https://github.com/Evgenii-379/3.3-3.3.md-k8s/blob/main/Снимок%20экрана%202025-04-09%20220349.png)

Backend -> Cache

- ![scrin](https://github.com/Evgenii-379/3.3-3.3.md-k8s/blob/main/Снимок%20экрана%202025-04-09%20220416.png)

- Запрещённые подключения:

Прямой доступ из Frontend в Cache и обратное подключение Backend -> Frontend

- ![scrin](https://github.com/Evgenii-379/3.3-3.3.md-k8s/blob/main/Снимок%20экрана%202025-04-09%20220552.png)


- Манифесты настроек :

[deployments.yaml](https://github.com/Evgenii-379/3.3-3.3.md-k8s/blob/main/config.yaml/deployments.yaml)

[network-policies.yaml](https://github.com/Evgenii-379/3.3-3.3.md-k8s/blob/main/config.yaml/network-policies.yaml)

















