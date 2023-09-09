# Домашнее задание к занятию 3 «Резервное копирование» - Наталья Мартынова (Пономарева)

### Задание 1

Выполняем резервное копирование каталога пользователя в /tmp/backup/

```
 rsync -avc --delete --exclude '.*' /home/user1/ /tmp/backup/

```

```
-a, --archive – архивный режим, включает рекурсивное копирование и сохранение прав и владельца
-v, --verbose – вывести подробную информацию о процессе
-c, --checksum – использование сверки по контрольным суммам, а не по времени изменения и размеру файлов.
--delete – удалять файлы, которых нет в источнике
--exclude – исключить файлы
```

![Снимок1](https://github.com/NatoshFehn/hw-sflt-03/blob/main/Снимок1.jpg)


![Снимок2](https://github.com/NatoshFehn/hw-sflt-03/blob/main/Снимок2.jpg)


----

### Задание 2

1). Резервное копирование домашней директории 

```
rsync -avc --delete --exclude '.*' /home/user1/ /tmp/backup/

```

2). Создаем скрипт /home/sync.sh и выполняем по расписанию каждый день 

3). Настройка crontab

```
crontab -e

0 * 1-31 * * /home/sync.sh

```

4). Скрипт проверяет вывод rsynс и выводит информацию в системный log

```
#!/bin/bash

source=$HOME 
dest='/tmp/backup/'

rsync_sending=$(rsync -avc --delete --exclude '.*' $source $dest  2>&1 | sed  '/^#\|^$\| *#/d' | awk 'NR==1 {print $1}')

if [ -d "$dest" ]; then
        if [ $rsync_sending == "sending" ] ; then
                logger "backup created Successfully";
        else
                logger "backup no create";
        fi
else
        logger "backup dir not found";
fi

```

![Снимок3](https://github.com/NatoshFehn/hw-sflt-03/blob/main/Снимок3.jpg)

Результат выполения

![Снимок4](https://github.com/NatoshFehn/hw-sflt-03/blob/main/Снимок4.jpg)
