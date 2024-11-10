
## 1. Узнать IP-адрес интерфейса, подключенного к сети Интернет
### Скрипт:
```
#!/bin/bash

ip_address=$(echo $(ip addr) | grep 'inet' | grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}/[0-9]{1,2}' | tail -n 1)
echo "IP address: $ip_address"
```
###  Результат:
```[root@oracle-server /]# ./task-1.sh
IP address: 78.140.240.10/24
```
## 2. Создать именованный пайп ( named pipe ). Вывести в файл через созданный pipe вывод команды ss -plnt
### Скрипт:
```
#!/bin/bash

pipe_name='pipe-1'
output='output.txt'
mkfifo $pipe_name
ss -plnt > $pipe_name &
cat $pipe_name  | tee -a $output
rm $pipe_name
```
###  Результат:
```[root@oracle-server /]# ./task-2.sh
State  Recv-Q Send-Q Local Address:Port Peer Address:PortProcess
LISTEN 0      128          0.0.0.0:22        0.0.0.0:*    users:(("sshd",pid=1153,fd=5))
```
## 3. При помощи именованного пайпа заархивировать всё, что в него отправляем. Например, содержимое файла `/var/log/messages` На выходе получить архив `tar` или любой другой
### Скрипт:
```
#!/bin/bash

pipe_name='pipe-2'
mkfifo $pipe_name

(cat $pipe_name | zip  archive.zip  -) &
cat /var/log/messages > $pipe_name


rm $pipe_name
echo 'Архив успешно создан'

```
###  Результат:

```[root@oracle-server /]# ./task-3.sh
updating: - (deflated 77%)
Архив успешно создан
```
## 4. Вывести дату в unixtime На вход команды date через пайп подать свой формат выводимой даты
### Скрипт:

```
#!/bin/bash

pipe_name='pipe-3'
date_format='+%s'
mkfifo "$pipe_name"

echo $date_format > "$pipe_name" &

date -d "2024-11-10 16:00:00" "$(cat "$pipe_name")"

rm "$pipe_name"
```
###  Результат:

```[root@oracle-server /]# ./task-4.sh
1731254400
```
## 5. При помощи `HEREDOC` записать в файл многострочное сообщение
###  Результат:
```
[root@oracle-server /]#  cat << EOF > output.txt
> $(ls -la)
> EOF
[root@oracle-server /]# cat output.txt
total 168
-rw-------.   1 root root 115734 Nov  9 18:05 -
dr-xr-xr-x.  17 root root   4096 Nov 10 12:45 .
dr-xr-xr-x.  17 root root   4096 Nov 10 12:45 ..
-rw-r--r--.   1 root root   1180 Nov 10 12:24 archive.zip
lrwxrwxrwx.   1 root root      7 Oct  9  2021 bin -> usr/bin
dr-xr-xr-x.   5 root root   4096 Sep 21  2022 boot
drwxr-xr-x.  17 root root   3060 Nov  8 06:57 dev
drwxr-xr-x. 102 root root   8192 Nov  9 16:54 etc
drwxr-xr-x.   2 root root      6 Oct  9  2021 home
lrwxrwxrwx.   1 root root      7 Oct  9  2021 lib -> usr/lib
lrwxrwxrwx.   1 root root      9 Oct  9  2021 lib64 -> usr/lib64
drwxr-xr-x.   2 root root      6 Oct  9  2021 media
drwxr-xr-x.   2 root root      6 Oct  9  2021 mnt
drwxr-xr-x.   2 root root      6 Oct  9  2021 opt
-rw-r--r--.   1 root root    308 Nov 10 12:20 output.txt
dr-xr-xr-x. 156 root root      0 Nov  8 06:57 proc
dr-xr-x---.   3 root root    140 Nov 10 12:45 root
drwxr-xr-x.  32 root root    920 Nov  9 16:48 run
lrwxrwxrwx.   1 root root      8 Oct  9  2021 sbin -> usr/sbin
drwxr-xr-x.   2 root root      6 Oct  9  2021 srv
dr-xr-xr-x.  13 root root      0 Nov  8 06:57 sys
-rwxr-xr-x.   1 root root    172 Nov  9 16:32 task-1.sh
-rwxr-xr-x.   1 root root    141 Nov 10 12:20 task-2.sh
-rwxr-xr-x.   1 root root    190 Nov  9 18:04 task-3.sh
-rwxr-xr-x.   1 root root    178 Nov 10 12:43 task-4.sh
-rwxr-xr-x.   1 root root      0 Nov  9 16:21 task-5.sh
drwxrwxrwt.   3 root root    106 Nov 10 14:14 tmp
drwxr-xr-x.  13 root root    158 Sep 21  2022 usr
drwxr-xr-x.  21 root root   4096 Sep 21  2022 var
 ```
