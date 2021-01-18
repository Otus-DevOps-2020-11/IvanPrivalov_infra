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
