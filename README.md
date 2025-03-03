# k8s-monitoring-logging-practicum
Практикум по курсу Kubernetes: мониторинг и логирование

# Ansible роли для разворачивания Prometheus и стека логирования EFK

## Описание
Данный репозиторий содержит Ansible роли для автоматизированного развертывания мониторинга и логирования вне Kubernetes. Включает:
- Prometheus (и его экспортеры)
- Node Exporter
- ElasticSearch + Kibana
- FluentBit для сбора логов

## Требования
- Ubuntu 22.04 LTS / Debian 12
- Ansible 2.12+
- Доступ к управляемым серверам по SSH

## Установка
1. Клонируйте репозиторий:
   ```sh
   git clone https://github.com/your-repo/ansible-monitoring.git
   cd ansible-monitoring

2. Заполните инвентарь inventory.yml:
```yaml
all:
  hosts:
    monitoring:
      ansible_host: 192.168.1.100
      ansible_user: ubuntu
      ansible_ssh_private_key_file: ~/.ssh/id_rsa
```
4. Запустите плейбук:
```sh
ansible-playbook -i inventory.yml site.yml
```
## Структура репозитория
```sh
k8s-monitoring-logging-practicum/
│── roles/
│   ├── prometheus/
│   ├── node_exporter/
│   ├── elasticsearch/
│   ├── fluentbit/
│── inventory.yml
│── site.yml
│── README.md
```
## Роли:
- prometheus: установка Prometheus, настройка мониторинга самого себя  
- node_exporter: установка и настройка Node Exporter, добавление в конфиг - Prometheus  
- elasticsearch: установка ElasticSearch + Kibana  
- fluentbit: настройка FluentBit для сбора логов в ElasticSearch  




### Настройки авторизации
> С версии 8.x Elastic усилил безопасность, добавив: 
Enrollment Token – для привязки Kibana к защищённому Elasticsearch.
Verification Code – чтобы гарантировать, что человек, имеющий токен, действительно управляет узлом Kibana.
```
docker exec -it elasticsearch bin/elasticsearch-create-enrollment-token --scope kibana # получить токен
docker exec -it kibana bin/kibana-verification-code # код авторизации
```


### Использование ssh-agent (если стоит пароль на ssh_key)
Чтобы избежать постоянного ввода пароля, можно добавить ключ в ssh-agent:

```sh
eval $(ssh-agent -s)  # Запускаем ssh-agent
ssh-add ~/.ssh/id_rsa  # Добавляем ключ (введи пароль один раз)
```
Теперь Ansible сможет использовать этот ключ без ввода пароля при каждом подключении.
Проверить, что ключ добавлен:

```sh
ssh-add -l
```
Если видишь список добавленных ключей — всё работает.

