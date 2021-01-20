# Домашнее задание №3

## Решение

Адреса хостов:

```shell
bastion_IP = 178.154.246.116
someinternalhost_IP = 10.130.0.35
```

1.Создаем на локальном хосте файл config в каталоге c:\users\diamond\.ssh\id_rsa

2.Добавляем в него следующую конфигурацию ssh:

```shell
# bastion
Host bastion
   HostName 178.154.246.116
   User appuser
   IdentityFile c:\users\diamond\.ssh\id_rsa

# someinternalhost
Host someinternalhost
   HostName 10.130.0.35
   User appuser
   IdentityFile c:\users\diamond\.ssh\id_rsa
   ProxyJump appuser@178.154.246.116
```

3.Подключаемся к someinternalhost по алиасу, используя ProxyJump через bastion:

```shell
ssh someinternalhost
```

<details><summary>Пример вывода</summary>
<p>

```log
$ ssh someinternalhost
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-142-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

appuser@someinternalhost:~$

```
</p>
</details>

# Домашнее задание №4

## В ДЗ сделано:

1) Установлен и настроен yc CLI для работы с аккаунтом Yandex Cloud;
2) Создан инстанс с помощью CLI;
3) Установклен на хост ruby, mongodb для работы приложения, деплой тестового приложения;
4) Созданы bash-скрипты для установки на хост необходимых пакетов и деплоя приложения;
5) Создан startup-сценарий init-cloud для автоматического деплоя приложения после создания хоста.
Данные для проверки деплоя приложения:

```shell
testapp_IP=178.154.247.238
testapp_port=9292
```

# Основное задание

Созданы bash-скрипты для деплоя приложения:

1) Скрипт install_ruby.sh содержит команды по установке Ruby;
2) Скрипт install_mongodb.sh содержит команды по установке MongoDB;
3) Скрипт deploy.sh содержит команды скачивания кода, установки зависимостей через bundler и запуск приложения.

Для создания инстанса используется команда:

```shell
yc compute instance create --name reddit-app --hostname reddit-app --memory=4 --create-boot-disk image-folder-id=standard-images,image-family=ubuntu-1604-lts,size=10GB --network-interface subnet-name=default-ru-central1-a,nat-ip-version=ipv4 --metadata serial-port-enable=1 --ssh-key c:/users/diamond/.ssh/id_rsa.pub
```

# Дополнительное задание

Создан файл metadata.yaml (startup-сценарий init-cloud), используемый для provision хоста после его создания.
Для создания инстанса и деплоя приложения используется команда (запускаем из директории где лежит metadata.yaml):

```shell
yc compute instance create --name reddit-app --hostname reddit-app --memory=4 --create-boot-disk image-folder-id=standard-images,image-family=ubuntu-1604-lts,size=10GB --network-interface subnet-name=default-ru-central1-a,nat-ip-version=ipv4 --metadata serial-port-enable=1 --metadata-from-file user-data=./metadata.yaml
```

Подключение к хосту выполняем командой:

ssh -i c:/users/diamond/ssh/id_rsa -A yc-user@178.154.247.238
