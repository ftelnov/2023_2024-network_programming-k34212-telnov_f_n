# Lab #3

## Информация

University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming)
Year: 2023/2024
Group: K34212
Author: Telnov Fedor Nikolaevich
Lab: Lab3
Date of create: 15.02.2024
Date of finished: 15.02.2024

## Прогресс

### Установка софта на доп. VM

Устанавливаю необходимый софт - PG, Redis:

![soft_setup](../assets/soft_setup.png)

Проверяю, что Redis-сервер доступен:

![redis_ping](../assets/redis_ping.png)

Клонирую репозиторий netbox:

![clone_netbox](../assets/clon_netbox.png)

Завожу пользователя, даю права на директорию, генерю секретный ключ:

![add_user](../assets/add_user.png)

Используя предоставляемый docker-compose файл, поднимаю кластер:

![cluster_up](../assets/cluster_up.png)

В результате удалось зайти на развернутый netbox:

![netbox_online](../assets/netbox_online.png)

### Добавление CHR

Для добавления устройства в Netbox делаю следующее:

- создаю производителя устройства
- создаю модель
- создаю сайт
- для каждого устройства заполняю интерфейсы и IP-адреса

В результате получаются следующие конфигурации:

CHR-1:

![CHR-1](../assets/2025-03-03-16-46-02.png)

CHR-2:

![CHR-2](../assets/CHR-2.png)

### Экстракт данных из Netbox

Перед экстрактом я выпускаю токен, а дальше использую плагин Ansible для построения инвентаря:

![inv_extract](../assets/inv_extract.png)

После вызова команды "ansible-inventory -v --list -y -i extract_inv.yml > netbox_inventory.yml", получился файл "netbox_inventory.yml", прикрепленный в папке с ЛР-3.

### Настройка CHR с помощью ansible

Перед использованием, нужно докинуть параметр "ansible_host" в определения хостов CHR:

![alter_params](../assets/alter_params.png)

Далее я создал плейбук для редактирования параметров CHR:

![change_params](../assets/change_params.png)

После отработки плейбука параметры CHR корректно изменились:

![chr_params_change](../assets/chr_params_change.png)

Теперь более сложный плейбук, который сначала собирает серийный номер, сохраняет его в переменную, а потом выставляет его в Netbox. В инвентаре serial изначально пустые у хостов. Код плейбука:

![alter_serial](../assets/alter_serial.png)

## Вывод

В данной работе проведено знакомство с Netbox - системой учета сетевых ресурсов, а также его связкой с Ansible. Интеграция достаточно удобная, позволяет как получить инвентарь в Ansible, так и использовать его для менеджмента машин.
