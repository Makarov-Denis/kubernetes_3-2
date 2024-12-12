# «Установка Kubernetes»- Макаров Денис

### Цель задания

Установить кластер K8s.

### Чеклист готовности к домашнему заданию

1. Развёрнутые ВМ с ОС Ubuntu 20.04-lts.


### Инструменты и дополнительные материалы, которые пригодятся для выполнения задания

1. [Инструкция по установке kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/).
2. [Документация kubespray](https://kubespray.io/).

-----

### Задание 1. Установить кластер k8s с 1 master node

1. Подготовка работы кластера из 5 нод: 1 мастер и 4 рабочие ноды.
2. В качестве CRI — containerd.
3. Запуск etcd производить на мастере.
4. Способ установки выбрать самостоятельно.

### Решение

Команды используемые для поднятия кластера kubespray.

```
sudo apt update  -y
sudo apt install git -y
sudo apt install python3 -y
sudo apt install python3-pip -y
sudo apt install python3.10-venv -y
git clone https://github.com/kubernetes-sigs/kubespray
cd kubespray
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install -r requirements.txt
pip install -r requirements.txt

cp -rfp inventory/sample inventory/mycluster

declare -a IPS=()
pip3 install ruamel.yaml
CONFIG_FILE=inventory/mycluster/hosts.yaml python3 contrib/inventory_builder/inventory.py ${IPS[@]}

ansible-playbook -i inventory/mycluster/hosts.yaml cluster.yml -b -v &

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chmod $(id -u):$(id -g) $HOME/.kube/config

```
Терраформ

[scr](https://github.com/Makarov-Denis/kubernetes_3-2/tree/main/src)

![nods](https://github.com/user-attachments/assets/b6dcb6b4-152e-463d-a70a-73ec0c8baad5)

Сконфигурировал файл инвентаря hosts.yaml и запустил playbook

![ansible](https://github.com/user-attachments/assets/4989265e-767f-44bf-a180-1fe0600b7a72)

![cluster](https://github.com/user-attachments/assets/a0d5d26a-f588-49ae-bf99-2f347eeb96d8)

## Дополнительные задания (со звёздочкой)

**Настоятельно рекомендуем выполнять все задания под звёздочкой.** Их выполнение поможет глубже разобраться в материале.   
Задания под звёздочкой необязательные к выполнению и не повлияют на получение зачёта по этому домашнему заданию. 

------
### Задание 2*. Установить HA кластер

1. Установить кластер в режиме HA.
2. Использовать нечётное количество Master-node.
3. Для cluster ip использовать keepalived или другой способ.

### Правила приёма работы

1. Домашняя работа оформляется в своем Git-репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl get nodes`, а также скриншоты результатов.
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.
