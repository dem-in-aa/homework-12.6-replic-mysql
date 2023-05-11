# Домашнее задание к занятию «Репликация и масштабирование. Часть 1» Андрей Дёмин

### Задание 1

На лекции рассматривались режимы репликации master-slave, master-master, опишите их различия.

<ins>*Ответ*</ins>:

1) в режиме Master-Slave новые данные пишутся только на Master, а со Slave они только читаются; в режиме Master-Master данные активно пишутся на оба сервера;
2) Отказоустойчивость. Master-Slave: при недоступности Master его роль исполняет один из Slave, возможна потеря части данных. Master-Master: при отказе одного сервера данные можно получить без потерь с другого сервера;
3) Целостность БД. Master-Slave: после потери и восстановления связи данные с Master будут дописаны в Slave до актуального состояния. Master-Master: после потери и восстановления связи между серверами возникает конфликт и нарушение целостности данных.

---

### Задание 2

Выполните конфигурацию master-slave репликации, примером можно пользоваться из лекции.

*Приложите скриншоты конфигурации, выполнения работы: состояния и режимы работы серверов.*

<ins>1.Установка репозитория MySQL 8</ins>

Предварительно нужно установить пакет gnupg.
```
apt-get install gnupg
```
Установка MySQL APT репозитория:
```
https://dev.mysql.com/downloads/
```
Загрузка пакета:
```cd /tmp
wget -c https://dev.mysql.com/get/mysql-apt-config_0.8.25-1_all.deb
ls -fla | grep mysql
```
Установим пакет:
```
dpkg -i mysql-apt-config_0.8.25-1_all.deb
```
После установки пакета в /etc/apt/source.list.d/ добавится mysql.list.
Обновляем репозиторий:
```
apt-get update
```
<ins>2.Установка MySQL 8</ins>
```
apt-get install mysql-server
```
<ins>3.Настройка безопасности</ins>

Для настройки безопасного доступа к MySQL существует специальный скрипт:
```
mysql_secure_installation
```
Он позволяет настроить следующие параметры:

-VALIDATE PASSWORD COMPONENT — использовать компонент VALIDATE PASSWORD? (No)

-Изменить пароль от root? (No)

-Удалить анонимных пользователей? (y)

-Запретить удалённый логин под root? (y)

-Удалить тестовую базу и доступ к ней? (y)

-Обновить таблицу привилегий? (y)

Проверка статуса:
```
systemctl status mysql
```
Проверка на автозагрузку:
```
systemctl is-enabled mysql
```
Если MySQL не запускается автоматически:
```
systemctl enable mysql
```

<ins>4.Перемещение базы данных MySQL 8</ins>

База данных MySQL 8 при установке по умолчанию пишется в папку /var/lib/mysql. Со временем база растёт и поднимается вопрос переноса её на другой раздел диска. Перенесём базу данных в /opt/mysql/mysql.

Создадим директорию для MySQL:
```
cd /opt
mkdir mysql
ls -Fla
```




---

## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

---

### Задание 3* 

Выполните конфигурацию master-master репликации. Произведите проверку.

*Приложите скриншоты конфигурации, выполнения работы: состояния и режимы работы серверов.*
