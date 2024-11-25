



## 1. Отразить 7 последних строк из: wtmp  last

### Скрипт:
```
#!/bin/bash

WTMP_PATH='/var/log/wtmp'
last -f $WTMP_PATH | head -n 7
```
###  Результат:
```
root     pts/0        185.119.1.185    Mon Nov 25 12:01   still logged in
root     pts/1        185.119.1.185    Sat Nov 23 12:08 - 15:39  (03:31)
root     pts/0        185.119.1.185    Sat Nov 23 11:37 - 18:25  (06:48)
root     pts/2        5.142.46.101     Fri Nov 22 13:04 - 15:14  (02:10)
root     pts/0        5.142.46.101     Fri Nov 22 13:01 - 15:04  (02:02)
root     pts/1        5.142.46.101     Fri Nov 22 11:36 - 13:40  (02:04)
root     pts/0        5.142.46.101     Fri Nov 22 10:23 - 12:56  (02:32)
```
## 2. При помощи logger записать в общий журнал послание добра и мира посланникам из Альфа-Центавра с facility kernel
### Скрипт:
```
#!/bin/bash

message="добра и мира посланникам из Альфа-Центавра"
logger -p kern.info "$message"
```
###  Результат:
```
Nov 25 12:46:21 oracle-server.local root[162929]: добра и мира посланникам из Альфа-Центавра
```
## 3.Посмотреть последние 25 строчек лога сервиса systemd-logind

### Скрипт:
```
#!/bin/bash

journalctl -u systemd-logind -n 25
```
###  Результат:

```
-- Logs begin at Thu 2024-11-14 16:08:13 UTC, end at Mon 2024-11-25 12:53:20 UTC. --
Nov 22 06:07:16 oracle-server.local systemd-logind[669]: New session 19 of user root.
Nov 22 07:05:07 oracle-server.local systemd-logind[669]: Session 19 logged out. Waitin>
Nov 22 07:05:07 oracle-server.local systemd-logind[669]: Removed session 19.
Nov 22 07:05:17 oracle-server.local systemd-logind[669]: New session 21 of user root.
Nov 22 09:22:38 oracle-server.local systemd-logind[669]: Session 21 logged out. Waitin>
Nov 22 09:22:38 oracle-server.local systemd-logind[669]: Removed session 21.
Nov 22 10:23:45 oracle-server.local systemd-logind[669]: New session 22 of user root.
Nov 22 11:35:59 oracle-server.local systemd-logind[669]: New session 24 of user root.
Nov 22 12:56:43 oracle-server.local systemd-logind[669]: Session 22 logged out. Waitin>
Nov 22 12:56:43 oracle-server.local systemd-logind[669]: Removed session 22.
Nov 22 13:01:56 oracle-server.local systemd-logind[669]: New session 25 of user root.
Nov 22 13:04:06 oracle-server.local systemd-logind[669]: New session 26 of user root.
Nov 22 13:40:41 oracle-server.local systemd-logind[669]: Session 24 logged out. Waitin>
Nov 22 13:40:41 oracle-server.local systemd-logind[669]: Removed session 24.
Nov 22 15:04:31 oracle-server.local systemd-logind[669]: Session 25 logged out. Waitin>
Nov 22 15:04:31 oracle-server.local systemd-logind[669]: Removed session 25.
Nov 22 15:14:21 oracle-server.local systemd-logind[669]: Session 26 logged out. Waitin>
Nov 22 15:14:21 oracle-server.local systemd-logind[669]: Removed session 26.
Nov 23 11:37:13 oracle-server.local systemd-logind[669]: New session 27 of user root.
Nov 23 12:08:34 oracle-server.local systemd-logind[669]: New session 29 of user root.
Nov 23 15:39:58 oracle-server.local systemd-logind[669]: Session 29 logged out. Waitin>
Nov 23 15:39:58 oracle-server.local systemd-logind[669]: Removed session 29.
Nov 23 18:25:51 oracle-server.local systemd-logind[669]: Session 27 logged out. Waitin>
Nov 23 18:25:51 oracle-server.local systemd-logind[669]: Removed session 27.
Nov 25 12:01:02 oracle-server.local systemd-logind[669]: New session 30 of user root.
```
## 4. Создать конфигурацию syslog для отправки сообщений уровня info в файл /var/log/my.log
### Скрипт:

```
#!/bin/bash

CONFIG_PATH='/etc/rsyslog.conf'
LOG_PATH='/var/log/my.log'
rule='*.info '$LOG_PATH''
echo $rule >> $CONFIG_PATH
```
###  Результат:

```
[root@oracle-server etc]# cat rsyslog.conf | tail -n 1
*.info /var/log/my.log
```
## 5. Все события ssh записывать параллельно в отдельный файл, производить ротацию каждые сутки или по размеру (не более 1 мегабайта), всего 10 файлов в ротации.
###  Скрипт:
```
#!/bin/bash

CONFIG_PATH='/etc/logrotate.d/ssh'
LOG_PATH='/var/log/ssh/ssh.log'
rule="$LOG_PATH{
daily
rotate 10
size 1M
missingok
}"

echo "$rule" >> "$CONFIG_PATH"
 ```
###  Результат:

```
[root@oracle-server hw-2]# cat /etc/logrotate.d/ssh
/var/log/ssh/ssh.log{
daily
rotate 10
size 1M
missingok
}
```
## 6. Вывести сообщения с последнего запуска системы. Вывести сообщения не старше одного часа
### Скрипт:

```
#!/bin/bash

echo '1. сообщения с последнего запуска системы'
journalctl --boot
echo '2. сообщения не старше одного часа'
journalctl --since "1 hour ago"
```
###  Результат:

```
[root@oracle-server hw-2]# ./task-6.sh
1. сообщения с последнего запуска системы
-- Logs begin at Thu 2024-11-14 16:08:13 UTC, end at Mon 2024-11-25 13:27:52 UTC. --
Nov 14 16:08:13 localhost.localdomain kernel: Linux version 5.4.17-2136.310.7.1.el8uek>
Nov 14 16:08:13 localhost.localdomain kernel: Command line: BOOT_IMAGE=(hd0,gpt2)/boot>
Nov 14 16:08:13 localhost.localdomain kernel: x86/fpu: Supporting XSAVE feature 0x001:>
Nov 14 16:08:13 localhost.localdomain kernel: x86/fpu: Supporting XSAVE feature 0x002:>
Nov 14 16:08:13 localhost.localdomain kernel: x86/fpu: Supporting XSAVE feature 0x004:>
Nov 14 16:08:13 localhost.localdomain kernel: x86/fpu: Supporting XSAVE feature 0x020:>
Nov 14 16:08:13 localhost.localdomain kernel: x86/fpu: Supporting XSAVE feature 0x040:>
Nov 14 16:08:13 localhost.localdomain kernel: x86/fpu: Supporting XSAVE feature 0x080:>
Nov 14 16:08:13 localhost.localdomain kernel: x86/fpu: xstate_offset[2]:  576, xstate_>
Nov 14 16:08:13 localhost.localdomain kernel: x86/fpu: xstate_offset[5]: 1088, xstate_>
Nov 14 16:08:13 localhost.localdomain kernel: x86/fpu: xstate_offset[6]: 1152, xstate_>
Nov 14 16:08:13 localhost.localdomain kernel: x86/fpu: xstate_offset[7]: 1664, xstate_>
Nov 14 16:08:13 localhost.localdomain kernel: x86/fpu: Enabled xstate features 0xe7, c>
Nov 14 16:08:13 localhost.localdomain kernel: BIOS-provided physical RAM map:
Nov 14 16:08:13 localhost.localdomain kernel: BIOS-e820: [mem 0x0000000000000000-0x000>
Nov 14 16:08:13 localhost.localdomain kernel: BIOS-e820: [mem 0x0000000000100000-0x000>
Nov 14 16:08:13 localhost.localdomain kernel: BIOS-e820: [mem 0x0000000058f50000-0x000>
Nov 14 16:08:13 localhost.localdomain kernel: BIOS-e820: [mem 0x000000005c8bc000-0x000>
Nov 14 16:08:13 localhost.localdomain kernel: BIOS-e820: [mem 0x000000007c73a000-0x000>
Nov 14 16:08:13 localhost.localdomain kernel: BIOS-e820: [mem 0x000000007e522000-0x000>
Nov 14 16:08:13 localhost.localdomain kernel: BIOS-e820: [mem 0x000000007e9a1000-0x000>
Nov 14 16:08:13 localhost.localdomain kernel: BIOS-e820: [mem 0x000000007e9a3000-0x000>
Nov 14 16:08:13 localhost.localdomain kernel: BIOS-e820: [mem 0x000000007ea74000-0x000>
Nov 14 16:08:13 localhost.localdomain kernel: BIOS-e820: [mem 0x000000007ea76000-0x000>
Nov 14 16:08:13 localhost.localdomain kernel: BIOS-e820: [mem 0x000000007ea8f000-0x000>
Nov 14 16:08:13 localhost.localdomain kernel: BIOS-e820: [mem 0x000000007ea91000-0x000>
Nov 14 16:08:13 localhost.localdomain kernel: BIOS-e820: [mem 0x000000007eb1b000-0x000>
Nov 14 16:08:13 localhost.localdomain kernel: BIOS-e820: [mem 0x000000007fb9b000-0x000>
Nov 14 16:08:13 localhost.localdomain kernel: BIOS-e820: [mem 0x000000007fbf3000-0x000>
Nov 14 16:08:13 localhost.localdomain kernel: BIOS-e820: [mem 0x000000007fbfb000-0x000>
Nov 14 16:08:13 localhost.localdomain kernel: BIOS-e820: [mem 0x000000007fbff000-0x000>
Nov 14 16:08:13 localhost.localdomain kernel: BIOS-e820: [mem 0x000000007ff7c000-0x000>
Nov 14 16:08:13 localhost.localdomain kernel: NX (Execute Disable) protection: active
Nov 14 16:08:13 localhost.localdomain kernel: efi: EFI v2.70 by BHYVE
Nov 14 16:08:13 localhost.localdomain kernel: efi:  SMBIOS=0x7fbcc000  ACPI=0x7fbfa000>
Nov 14 16:08:13 localhost.localdomain kernel: SMBIOS 2.8 present.
Nov 14 16:08:13 localhost.localdomain kernel: DMI: vStack SmartDC vStack bhyve/BHYVE, >
Nov 14 16:08:13 localhost.localdomain kernel: tsc: Detected 3000.000 MHz processor
Nov 14 16:08:13 localhost.localdomain kernel: tsc: Detected 2992.853 MHz TSC
Nov 14 16:08:13 localhost.localdomain kernel: e820: update [mem 0x00000000-0x00000fff]>
Nov 14 16:08:13 localhost.localdomain kernel: e820: remove [mem 0x000a0000-0x000fffff]>
2. сообщения не старше одного часа
-- Logs begin at Thu 2024-11-14 16:08:13 UTC, end at Mon 2024-11-25 13:27:52 UTC. --
Nov 25 12:28:13 oracle-server.local sshd[162565]: Invalid user apache from 83.222.191.>
Nov 25 12:28:15 oracle-server.local sshd[162565]: Connection closed by invalid user ap>
Nov 25 12:28:22 oracle-server.local sshd[162568]: Invalid user appltest from 83.222.19>
Nov 25 12:28:24 oracle-server.local sshd[162568]: Connection closed by invalid user ap>
Nov 25 12:28:31 oracle-server.local sshd[162571]: Invalid user ansadmin from 83.222.19>
Nov 25 12:28:33 oracle-server.local sshd[162571]: Connection closed by invalid user an>
Nov 25 12:28:40 oracle-server.local sshd[162574]: Invalid user apache from 83.222.191.>
Nov 25 12:28:42 oracle-server.local sshd[162574]: Connection closed by invalid user ap>
Nov 25 12:28:49 oracle-server.local sshd[162577]: Invalid user ar from 83.222.191.62 p>
Nov 25 12:28:51 oracle-server.local sshd[162577]: Connection closed by invalid user ar>
Nov 25 12:28:58 oracle-server.local sshd[162580]: Invalid user apache from 83.222.191.>
Nov 25 12:29:00 oracle-server.local sshd[162580]: Connection closed by invalid user ap>
Nov 25 12:29:07 oracle-server.local sshd[162583]: Invalid user applprod from 83.222.19>
Nov 25 12:29:09 oracle-server.local sshd[162583]: Connection closed by invalid user ap>
Nov 25 12:29:17 oracle-server.local sshd[162586]: Invalid user applprod from 83.222.19>
Nov 25 12:29:18 oracle-server.local sshd[162586]: Connection closed by invalid user ap>
Nov 25 12:29:26 oracle-server.local sshd[162589]: Invalid user appluat from 83.222.191>
Nov 25 12:29:27 oracle-server.local sshd[162589]: Connection closed by invalid user ap>
Nov 25 12:29:35 oracle-server.local sshd[162592]: Invalid user apimobile from 83.222.1>
Nov 25 12:29:37 oracle-server.local sshd[162592]: Connection closed by invalid user ap>
Nov 25 12:29:44 oracle-server.local sshd[162595]: Invalid user ansible from 83.222.191>
Nov 25 12:29:46 oracle-server.local sshd[162595]: Connection closed by invalid user an>
Nov 25 12:29:53 oracle-server.local sshd[162598]: Invalid user api from 83.222.191.62 >
Nov 25 12:29:55 oracle-server.local sshd[162598]: Connection closed by invalid user ap>
Nov 25 12:30:02 oracle-server.local sshd[162601]: Invalid user app from 83.222.191.62 >
Nov 25 12:30:04 oracle-server.local sshd[162601]: Connection closed by invalid user ap>
Nov 25 12:30:11 oracle-server.local sshd[162604]: Invalid user ansible from 83.222.191>
Nov 25 12:30:13 oracle-server.local sshd[162604]: Connection closed by invalid user an>
Nov 25 12:30:20 oracle-server.local sshd[162607]: Invalid user apps from 83.222.191.62>
Nov 25 12:30:22 oracle-server.local sshd[162607]: Connection closed by invalid user ap>
Nov 25 12:30:29 oracle-server.local sshd[162610]: Invalid user apulis from 83.222.191.>
Nov 25 12:30:31 oracle-server.local sshd[162610]: Connection closed by invalid user ap>
Nov 25 12:30:31 oracle-server.local sshd[162613]: Connection closed by authenticating >
Nov 25 12:30:36 oracle-server.local sshd[162619]: Invalid user tianyu from 210.114.22.>
Nov 25 12:30:36 oracle-server.local sshd[162619]: Received disconnect from 210.114.22.>
Nov 25 12:30:36 oracle-server.local sshd[162619]: Disconnected from invalid user tiany>
Nov 25 12:30:38 oracle-server.local sshd[162616]: Invalid user appltest from 83.222.19>
Nov 25 12:30:40 oracle-server.local sshd[162616]: Connection closed by invalid user ap>
Nov 25 12:30:47 oracle-server.local sshd[162622]: Invalid user ansible from 83.222.191>
Nov 25 12:30:50 oracle-server.local sshd[162622]: Connection closed by invalid user an>
Nov 25 12:30:57 oracle-server.local sshd[162625]: Invalid user ansible from 83.222.191>
```
