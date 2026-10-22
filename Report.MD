
#  Report on Lab_4


## Завдання на лабороторну

---

### 1. У якому стані може бути процес в linux? Використовуючи додатковий матеріал, намалюйте граф переходів

У лінукс процеси можуть перебувати у наступних станах:

R - процес виконується на процесорі в даний момент або перебуває у черзі на виконання.

S - процес перебуває у стані «сну» та очікує на подію . Процес можна легко перервати сигналом.

D - Процес чекає на завершення прямих операцій введення-виведення , зазвичай під час роботи з диском або мережевою файловою системою. Його не можна перервати або закрити через kill -9 до завершення I/O.

Z - процес закінчив свою роботу, але запис про нього все ще перебуває у таблиці процесів, оскільки батьківський процес ще не зчитав код його завершення . Всі ресурси  процес вже вивільнив.

T / t - процес зупинено користувачем (наприклад, сполученням клавіш Ctrl + Z) або підсистемою налагодження (відлагоджувачем gdb чи системним викликом ptrace).

I - фоновий потік ядра, який не виконує завдань і очікує на роботу .

X - процес повністю знищено та вилучено з системи.

Для перегляду процесів можна використати команду ps aux або.

```
student@CsnKhai:~$ ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.2  0.8   4180  2216 ?        Ss   07:06   0:00 /sbin/init
root         2  0.0  0.0      0     0 ?        S    07:06   0:00 [kthreadd]
root         3  0.0  0.0      0     0 ?        S    07:06   0:00 [ksoftirqd/0]
root         4  0.0  0.0      0     0 ?        S    07:06   0:00 [kworker/0:0]
root         5  0.0  0.0      0     0 ?        S<   07:06   0:00 [kworker/0:0H]
root         7  0.1  0.0      0     0 ?        S    07:06   0:00 [rcu_sched]
root         8  0.0  0.0      0     0 ?        S    07:06   0:00 [rcu_bh]
root         9  0.0  0.0      0     0 ?        S    07:06   0:00 [migration/0]
root        10  0.0  0.0      0     0 ?        S    07:06   0:00 [watchdog/0]
root        11  0.0  0.0      0     0 ?        S<   07:06   0:00 [khelper]
root        12  0.0  0.0      0     0 ?        S    07:06   0:00 [kdevtmpfs]
root        13  0.0  0.0      0     0 ?        S<   07:06   0:00 [netns]
root        14  0.0  0.0      0     0 ?        S<   07:06   0:00 [writeback]
root        15  0.0  0.0      0     0 ?        S<   07:06   0:00 [kintegrityd]
root        16  0.0  0.0      0     0 ?        S<   07:06   0:00 [bioset]
root        17  0.0  0.0      0     0 ?        S<   07:06   0:00 [kworker/u3:0]
root        18  0.0  0.0      0     0 ?        S<   07:06   0:00 [kblockd]
root        19  0.0  0.0      0     0 ?        S<   07:06   0:00 [ata_sff]
root        20  0.0  0.0      0     0 ?        S    07:06   0:00 [khubd]
root        21  0.0  0.0      0     0 ?        S<   07:06   0:00 [md]
root        22  0.0  0.0      0     0 ?        S<   07:06   0:00 [devfreq_wq]
root        23  0.1  0.0      0     0 ?        R    07:06   0:00 [kworker/0:1]
root        25  0.0  0.0      0     0 ?        S    07:06   0:00 [khungtaskd]
root        26  0.0  0.0      0     0 ?        S    07:06   0:00 [kswapd0]
root        27  0.0  0.0      0     0 ?        SN   07:06   0:00 [ksmd]
.
.
.
.
student    912  0.0  0.3   2460   824 ?        Ss   07:11   0:00 /usr/lib/openssh/sftp-server
student    915  0.0  1.2   6668  3020 pts/0    Ss   07:11   0:00 -bash
student    931  0.0  0.5   5280  1364 ?        Ss   07:11   0:00 bash -c while true; do sleep 1;head -v -n 8 /proc/meminfo; he
student   1214  0.0  0.1   4220   276 ?        S    07:12   0:00 sleep 1
student   1215  0.0  0.4   5216  1168 pts/0    R+   07:12   0:00 ps aux
```

---

### 2. Вивчіть команду pstree. Виведіть (виділіть) ланцюжок (предків) поточного процесу.

Команда pstree відображає запущені процеси у вигляді наочного ієрархічного дерева, де чітко видно перехід з одного процесу до іншого.

для отримання тільки поточного процесу та увесь ланцюжок його предків , використаемо команду pstree -s $$.

```
student@CsnKhai:~$ pstree -s $$
init───sshd───sshd───sshd───bash───pstree
```

---

### 3. Що таке файлова система proc?

proc - це віртуальна файлова система, яка надає інтерфейс до внутрішніх даних ядра  у режимі реального часу.

Вона існує тільки в оперативній пам'яті . Файли та каталоги всередині proc генеруються у момент звернення до них.

---

### 4. Виведіть інформацію про процесор (його тип, підтримувані технології тощо).

для отримування даних про процесор можна використати команду cat /proc/cpuinfo у результаті виконання якої отримаємо наступний результат:

```
student@CsnKhai:~$ cat /proc/cpuinfo
processor       : 0
vendor_id       : AuthenticAMD
cpu family      : 25
model           : 80
model name      : AMD Ryzen 5 5600G with Radeon Graphics
stepping        : 0
microcode       : 0xffffffff
cpu MHz         : 3893.869
cache size      : 512 KB
physical id     : 0
siblings        : 1
core id         : 0
cpu cores       : 1
apicid          : 0
initial apicid  : 0
fdiv_bug        : no
f00f_bug        : no
coma_bug        : no
fpu             : yes
fpu_exception   : yes
cpuid level     : 13
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr_opt rdtscp constant_tsc nonstop_tsc extd_apicid pni pclmulqdq ssse3 fma cx16 sse4_1 sse4_2 movbe popcnt aes xsave avx f16c rdrand lahf_lm cr8_legacy abm sse4a misalignsse 3dnowprefetch arat vmmcall fsgsbase bmi1 avx2 bmi2 rdseed adx
bogomips        : 7787.73
clflush size    : 64
cache_alignment : 64
address sizes   : 48 bits physical, 48 bits virtual
power management:
```

Тут можна побачити модель процесорі кількість виділених ядер частоту та інщі дані процесора.

---

### 5. Візьміть id будь-якого процесу та перегляньте про нього інформацію за допомогою файлової системи proc. Яку інформацію можна отримати?

Для аналізу було використано процес з ID 931
```
student    931  0.0  0.5   5328  1412 ?        Ss   07:11   0:02 bash -c while true; do sleep 1;head -v -n 8 /proc/meminfo; he
```

Далі було виконано наступні команди підряд cd /proc =>  cd 931 =>  ls -la  після чого було отримано натупний результат

```
student@CsnKhai:~$ cd /proc
student@CsnKhai:/proc$ cd 931
student@CsnKhai:/proc/931$ ls -l
total 0
dr-xr-xr-x 2 student student 0 Oct 22 08:41 attr
-rw-r--r-- 1 student student 0 Oct 22 08:41 autogroup
-r-------- 1 student student 0 Oct 22 08:41 auxv
-r--r--r-- 1 student student 0 Oct 22 08:41 cgroup
--w------- 1 student student 0 Oct 22 08:41 clear_refs
-r--r--r-- 1 student student 0 Oct 22 07:12 cmdline
-rw-r--r-- 1 student student 0 Oct 22 08:41 comm
-rw-r--r-- 1 student student 0 Oct 22 08:41 coredump_filter
-r--r--r-- 1 student student 0 Oct 22 08:41 cpuset
lrwxrwxrwx 1 student student 0 Oct 22 08:41 cwd -> /home/student
-r-------- 1 student student 0 Oct 22 08:41 environ
lrwxrwxrwx 1 student student 0 Oct 22 08:41 exe -> /bin/bash
dr-x------ 2 student student 0 Oct 22 07:11 fd
dr-x------ 2 student student 0 Oct 22 08:41 fdinfo
-rw-r--r-- 1 student student 0 Oct 22 08:41 gid_map
-r-------- 1 student student 0 Oct 22 08:41 io
-r--r--r-- 1 student student 0 Oct 22 08:41 latency
-r--r--r-- 1 student student 0 Oct 22 08:41 limits
-rw-r--r-- 1 student student 0 Oct 22 08:41 loginuid
dr-x------ 2 student student 0 Oct 22 08:41 map_files
-r--r--r-- 1 student student 0 Oct 22 08:41 maps
-rw------- 1 student student 0 Oct 22 08:41 mem
-r--r--r-- 1 student student 0 Oct 22 08:41 mountinfo
-r--r--r-- 1 student student 0 Oct 22 08:41 mounts
-r-------- 1 student student 0 Oct 22 08:41 mountstats
dr-xr-xr-x 5 student student 0 Oct 22 08:41 net
dr-x--x--x 2 student student 0 Oct 22 08:41 ns
-rw-r--r-- 1 student student 0 Oct 22 08:41 oom_adj
-r--r--r-- 1 student student 0 Oct 22 08:41 oom_score
-rw-r--r-- 1 student student 0 Oct 22 08:41 oom_score_adj
-r--r--r-- 1 student student 0 Oct 22 08:41 pagemap
-r--r--r-- 1 student student 0 Oct 22 08:41 personality
-rw-r--r-- 1 student student 0 Oct 22 08:41 projid_map
lrwxrwxrwx 1 student student 0 Oct 22 08:41 root -> /
-rw-r--r-- 1 student student 0 Oct 22 08:41 sched
-r--r--r-- 1 student student 0 Oct 22 08:41 schedstat
-r--r--r-- 1 student student 0 Oct 22 08:41 sessionid
-rw-r--r-- 1 student student 0 Oct 22 08:41 setgroups
-r--r--r-- 1 student student 0 Oct 22 08:41 smaps
-r--r--r-- 1 student student 0 Oct 22 08:41 stack
-r--r--r-- 1 student student 0 Oct 22 07:12 stat
-r--r--r-- 1 student student 0 Oct 22 08:41 statm
-r--r--r-- 1 student student 0 Oct 22 07:12 status
-r--r--r-- 1 student student 0 Oct 22 08:41 syscall
dr-xr-xr-x 3 student student 0 Oct 22 07:38 task
-r--r--r-- 1 student student 0 Oct 22 08:41 timers
-rw-r--r-- 1 student student 0 Oct 22 08:41 uid_map
-r--r--r-- 1 student student 0 Oct 22 08:41 wchan
```

після використаємо наступну команду cat /proc/931/stat для детального перегляду цього файлу

```
student@CsnKhai:/proc/931$ cat /proc/931/stat
931 (bash) S 893 931 931 0 -1 4218880 786828 8260588 0 4 3 348 0 0 20 0 1 0 32843 5455872 353 4294967295 134512640 135476856 3213330320 3213326696 3078173736 0 65536 4 65538 3238365411 0 0 17 0 0 0 0 0 0 135483128 135502024 137121792 3213331828 3213332134 3213332134 3213332466 0
```

У результаті буви ведено системні дані про стан процесу.

---

### 6. Використовуючи команду ps, отримайте інформацію про розглянутий раніше процес (у питанні 5). Інформація має бути наступною: власник процесу, аргументи, з якими було запущено процес виконання, група-власник цього процесу тощо.

Для отримання інформації про розглянутий процес необхідно використати команду /proc/931$ ps -p 931 -o user,group,pid,ppid,stat,cmd яка виведе назначені стовбчики:

```
student@CsnKhai:/proc/931$ ps -p 931 -o user,group,pid,ppid,stat,cmd
USER     GROUP      PID  PPID STAT CMD
student  student    931   893 Ss   bash -c while true; do sleep 1;head -v -n 8 /proc/meminfo; head -v -n 2 /proc/stat /proc/ve
```
---

### 7. Як визначити процеси ядра та користувацькі процеси?

При виконанні команди ps у виведених даних процеси ядра зображені у квсадратних дужках на відміну від користуваціких.

```
root        27  0.0  0.0      0     0 ?        SN   07:06   0:00 [ksmd]
student   1215  0.0  0.4   5216  1168 pts/0    R+   07:12   0:00 ps aux
```

---

### 8. Виведіть на термінал перелік процесів. Коротко опишіть статуси процесів. У якому стані вони перебувають чи можуть перебувати?

```
root         1  0.0  0.8   4180  2216 ?        Ss   07:06   0:00 /sbin/init
root         2  0.0  0.0      0     0 ?        S    07:06   0:00 [kthreadd]
root         3  0.0  0.0      0     0 ?        S    07:06   0:00 [ksoftirqd/0]
root         4  0.0  0.0      0     0 ?        S    07:06   0:00 [kworker/0:0]
root         5  0.0  0.0      0     0 ?        S<   07:06   0:00 [kworker/0:0H]
root         7  0.0  0.0      0     0 ?        R    07:06   0:00 [rcu_sched]
root         8  0.0  0.0      0     0 ?        S    07:06   0:00 [rcu_bh]
root         9  0.0  0.0      0     0 ?        S    07:06   0:00 [migration/0]
root        10  0.0  0.0      0     0 ?        S    07:06   0:00 [watchdog/0]
root        11  0.0  0.0      0     0 ?        S<   07:06   0:00 [khelper]
root        12  0.0  0.0      0     0 ?        S    07:06   0:00 [kdevtmpfs]
root        13  0.0  0.0      0     0 ?        S<   07:06   0:00 [netns]
root        14  0.0  0.0      0     0 ?        S<   07:06   0:00 [writeback]
root        15  0.0  0.0      0     0 ?        S<   07:06   0:00 [kintegrityd]
root        16  0.0  0.0      0     0 ?        S<   07:06   0:00 [bioset]
root        17  0.0  0.0      0     0 ?        S<   07:06   0:00 [kworker/u3:0]
root        18  0.0  0.0      0     0 ?        S<   07:06   0:00 [kblockd]
root        19  0.0  0.0      0     0 ?        S<   07:06   0:00 [ata_sff]
root        20  0.0  0.0      0     0 ?        S    07:06   0:00 [khubd]
root        21  0.0  0.0      0     0 ?        S<   07:06   0:00 [md]
root        22  0.0  0.0      0     0 ?        S<   07:06   0:00 [devfreq_wq]
root        23  0.0  0.0      0     0 ?        R    07:06   0:06 [kworker/0:1]
root        25  0.0  0.0      0     0 ?        S    07:06   0:00 [khungtaskd]
root        26  0.0  0.0      0     0 ?        S    07:06   0:00 [kswapd0]
root        27  0.0  0.0      0     0 ?        SN   07:06   0:00 [ksmd]
root        28  0.0  0.0      0     0 ?        S    07:06   0:00 [fsnotify_mark]
root        29  0.0  0.0      0     0 ?        S    07:06   0:00 [ecryptfs-kthrea]
root        30  0.0  0.0      0     0 ?        S<   07:06   0:00 [crypto]
root        42  0.0  0.0      0     0 ?        S<   07:06   0:00 [kthrotld]
root        43  0.0  0.0      0     0 ?        S    07:06   0:00 [kworker/u2:1]
root        44  0.0  0.0      0     0 ?        S    07:06   0:00 [scsi_eh_0]
root        45  0.0  0.0      0     0 ?        S    07:06   0:00 [scsi_eh_1]
root        67  0.0  0.0      0     0 ?        S<   07:06   0:00 [deferwq]
root        68  0.0  0.0      0     0 ?        S<   07:06   0:00 [charger_manager]
root       114  0.0  0.0      0     0 ?        S<   07:06   0:00 [kpsmoused]
root       115  0.0  0.0      0     0 ?        S    07:06   0:00 [scsi_eh_2]
root       124  0.0  0.0      0     0 ?        S<   07:06   0:00 [kworker/u3:1]
root       125  0.0  0.0      0     0 ?        S    07:06   0:00 [jbd2/sda1-8]
root       126  0.0  0.0      0     0 ?        S<   07:06   0:00 [ext4-rsv-conver]
root       257  0.0  0.2   3008   620 ?        S    07:06   0:00 upstart-udev-bridge --daemon
root       270  0.0  0.5  12040  1408 ?        Ss   07:06   0:00 /lib/systemd/systemd-udevd --daemon
message+   343  0.0  0.3   4236   984 ?        Ss   07:06   0:00 dbus-daemon --system --fork
root       360  0.0  0.7   4212  1760 ?        Ss   07:06   0:00 /lib/systemd/systemd-logind
syslog     362  0.0  0.4  30476  1064 ?        Ssl  07:06   0:00 rsyslogd
root       370  0.0  0.2   2880   600 ?        S    07:06   0:00 upstart-file-bridge --daemon
root       471  0.0  0.2   2868   596 ?        S    07:06   0:00 upstart-socket-bridge --daemon
root       597  0.0  0.9   5512  2344 ?        Ss   07:06   0:00 dhclient -1 -v -pf /run/dhclient.eth0.pid -lf /var/lib/dhcp/d
root       721  0.0  0.3   4644   836 tty4     Ss+  07:06   0:00 /sbin/getty -8 38400 tty4
root       723  0.0  0.3   4644   828 tty5     Ss+  07:06   0:00 /sbin/getty -8 38400 tty5
root       726  0.0  0.3   4644   840 tty2     Ss+  07:06   0:00 /sbin/getty -8 38400 tty2
root       727  0.0  0.3   4644   832 tty3     Ss+  07:06   0:00 /sbin/getty -8 38400 tty3
root       729  0.0  0.3   4644   832 tty6     Ss+  07:06   0:00 /sbin/getty -8 38400 tty6
root       752  0.0  1.0   7796  2488 ?        Ss   07:06   0:00 /usr/sbin/sshd -D
root       760  0.0  0.3   3052   788 ?        Ss   07:06   0:00 cron
root       827  0.0  0.8   4400  2028 tty1     Ss   07:06   0:00 /bin/login --
root       841  0.0  0.0      0     0 ?        S    07:10   0:00 [kauditd]
student    858  0.0  1.2   6668  3032 tty1     S+   07:10   0:00 -bash
root       873  0.0  1.5  11192  3772 ?        Ss   07:11   0:00 sshd: student [priv]
root       876  0.0  1.5  11192  3752 ?        Ss   07:11   0:00 sshd: student [priv]
student    893  0.3  0.7  11192  1900 ?        S    07:11   0:26 sshd: student@pts/0
student    911  0.0  0.7  11192  1828 ?        S    07:11   0:00 sshd: student@notty
student    912  0.0  0.3   2460   824 ?        Ss   07:11   0:00 /usr/lib/openssh/sftp-server
student    915  0.0  1.2   6668  3032 pts/0    Ss   07:11   0:00 -bash
student    931  0.0  0.5   5328  1412 ?        Ss   07:11   0:05 bash -c while true; do sleep 1;head -v -n 8 /proc/meminfo; he
root      3511  0.0  0.0      0     0 ?        S    07:18   0:01 [kworker/u2:2]
student  20623  0.0  0.1   4220   276 ?        S    09:37   0:00 sleep 1
```

У наведених процесах є наступні статуси:
S - процес перебуває в стані очікування певної події і його можна легко перервати.

Ss  - процес перебуває в стані сну та є головним  у своїй сесії

S+  - процес перебуває в стані сну та працює на передньому плані термінала.

S+ - процес перебуває в стані сну та працює на передньому плані термінала.

SN - процес перебуває в стані сну та має знижений пріоритет виконання.

Ssl - процес перебуває в стані сну, є лідером сесії та містить декілька потоків виконання.

R - процес виконується на процесорі в даний момент або чекає на свою чергу в процесорній черзі.

---

### 9. Виведіть процеси лише конкретного користувача.

Для отриманні інформації про процеси користувача використаємо команду ps -u student:

```
student@CsnKhai:/proc/931$ ps -u student
  PID TTY          TIME CMD
  858 tty1     00:00:00 bash
  893 ?        00:00:31 sshd
  911 ?        00:00:00 sshd
  912 ?        00:00:00 sftp-server
  915 pts/0    00:00:00 bash
  931 ?        00:00:06 bash
30732 ?        00:00:00 sleep
30733 pts/0    00:00:00 ps
```

---

### 10. Які можна використовувати утиліти для аналізу задач, що виконуються (аналізуючи довідку по команді ps)?

Для аналізу разом з ps можнавикористати еаприклад grep для пошуку процесів за назвою за назвою як наприклад ps aux | grep bash

```
student@CsnKhai:/proc/931$ ps aux | grep bash
student    858  0.0  1.2   6668  3032 tty1     S+   07:10   0:00 -bash
student    915  0.0  1.2   6668  3032 pts/0    Ss   07:11   0:00 -bash
student    931  0.0  0.5   5328  1412 ?        Ss   07:11   0:06 bash -c while true; do sleep 1;head -v -n 8 /proc/meminfo; head -v -n 2 /proc/stat /proc/version /proc/uptime /proc/loadavg /proc/sys/fs/file-nr /proc/sys/kernel/hostname; tail -v -n 32 /proc/net/dev;echo '==> /proc/df <==';df -l;echo '==> /proc/who <==';who;echo '==> /proc/end <==';echo '##Moba##'; done
student   2117  0.0  0.3   4672   820 pts/0    S+   10:16   0:00 grep --color=auto bash
```

---

### 11. Вивчіть команду top. Яку інформацію вона виводить?

Команда top вкиводить інформацію про потоки як ps але оновлює інформацію кожні 3 секунди через що спроба його скопіювати є складною.

```
  893 student   20   0   11192   1900   1112 S  0.3  0.8   0:34.70 sshd
  931 student   20   0    5328   1412   1200 S  0.3  0.6   0:06.86 bash
    1 root      20   0    4180   2216   1416 S  0.0  0.9   0:00.86 init
    2 root      20   0       0      0      0 S  0.0  0.0   0:00.00 kthreadd
    3 root      20   0       0      0      0 R  0.0  0.0   0:01.10 ksoftirqd/0
    4 root      20   0       0      0      0 S  0.0  0.0   0:00.00 kworker/0:0
    5 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 kworker/0:0H
    7 root      20   0       0      0      0 R  0.0  0.0   0:00.97 rcu_sched
    8 root      20   0       0      0      0 S  0.0  0.0   0:00.00 rcu_bh
    9 root      rt   0       0      0      0 S  0.0  0.0   0:00.00 migration/0
   10 root      rt   0       0      0      0 S  0.0  0.0   0:00.34 watchdog/0
   11 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 khelper
   12 root      20   0       0      0      0 S  0.0  0.0   0:00.00 kdevtmpfs
   13 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 netns
   14 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 writeback
   15 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 kintegrityd
   16 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 bioset
   17 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 kworker/u3:0
   18 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 kblockd
   19 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 ata_sff
   20 root      20   0       0      0      0 S  0.0  0.0   0:00.00 khubd
   21 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 md
   22 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 devfreq_wq
```

---

### 12. Відобразіть процеси лише для конкретного користувача за допомогою команди top.

Для виведеня процесів лише конкренного користквача використаємо команду top -u student

```
  893 student   20   0   11192   1900   1112 S  0.3  0.8   0:37.72 sshd
  858 student   20   0    6668   3032   1672 S  0.0  1.2   0:00.02 bash
  911 student   20   0   11192   1828   1056 S  0.0  0.7   0:00.00 sshd
  912 student   20   0    2460    824    692 S  0.0  0.3   0:00.00 sftp-server
  915 student   20   0    6668   3032   1668 S  0.0  1.2   0:00.02 bash
  931 student   20   0    5328   1412   1200 S  0.0  0.6   0:07.37 bash
 9371 student   20   0    5452   1440   1008 R  0.0  0.6   0:00.00 top
 9443 student   20   0    4220    276    224 S  0.0  0.1   0:00.00 sleep
```

---

### 13. Які інтерактивні команди можна подавати керувати командою top? Наведіть кілька прикладів

Для керування командою top можна використати інтерактивні команди такі як:

M - відсортувати процеси за використанням оперативної пам'яті.

P - відсортувати процеси за завантаженням процесора

k - завершити процес

u -відфільтрувати процеси за конкретним користувачем.

1 - показати завантаження по кожному ядру процесора окремо.

q - вийти з програми.

---

### 14. Виконайте сортування вмісту вікна процесів, використовуючи різні параметри (наприклад, обсяг займаного процесорного часу тощо).

Для прикладу було відсортовано потоки за обсягом використаної оперативної пам'яті

Не відсотовані потоки

```
  893 student   20   0   11192   1900   1112 S  0.3  0.8   0:42.04 sshd
  931 student   20   0    5328   1412   1200 S  0.3  0.6   0:08.12 bash
  858 student   20   0    6668   3032   1672 S  0.0  1.2   0:00.02 bash
  911 student   20   0   11192   1828   1056 S  0.0  0.7   0:00.00 sshd
  912 student   20   0    2460    824    692 S  0.0  0.3   0:00.00 sftp-server
  915 student   20   0    6668   3032   1668 S  0.0  1.2   0:00.02 bash
16678 student   20   0    5452   1440   1008 R  0.0  0.6   0:00.00 top
16696 student   20   0    4220    280    224 S  0.0  0.1   0:00.00 sleep

```

Відсортовані потоки

```
  858 student   20   0    6668   3032   1672 S  0.0  1.2   0:00.02 bash
  915 student   20   0    6668   3032   1668 S  0.0  1.2   0:00.02 bash
  893 student   20   0   11192   1900   1112 S  0.3  0.8   0:41.34 sshd
  911 student   20   0   11192   1828   1056 S  0.0  0.7   0:00.00 sshd
 9371 student   20   0    5452   1440   1008 R  0.0  0.6   0:00.54 top
  931 student   20   0    5328   1412   1200 S  0.0  0.6   0:08.00 bash
  912 student   20   0    2460    824    692 S  0.0  0.3   0:00.00 sftp-server
15519 student   20   0    4220    272    224 S  0.0  0.1   0:00.00 sleep
```

---

### 15. Поняття пріоритету, які команди використовуються для встановлення пріоритету?

Пріоритет процесу це числове значення, яке визначає, скільки процесорного часу планувальник ядра надаватиме конкретному процесу порівняно з іншими.

Для встановлення пріорітету використовуєтся команда nice -n 0 де число 0 є звичайним пріорітемом -20 найвищім а 19 найнижчим наприклад:

```
student@CsnKhai:~$ md5sum /dev/urandom &
[1] 24115
student@CsnKhai:~$ nice -n 19 md5sum /dev/urandom &
[2] 24302
student@CsnKhai:~$ top
  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
24115 student   20   0    4236    536    476 R 97.4  0.2   0:52.49 md5sum
24302 student   39  19    4236    536    476 R  1.3  0.2   0:00.30 md5sum
```

---

### 16. Чи можна змінити пріоритет процесу за допомогою команди top? Якщо так, то як?

Для зміни пріорітету у команді top використавши інтерактивну команду за допомогою r аоясля чого зміпюємо пріорітет неопхідного процесу процесe, проте мені не вистачало прав тому для зміни я використав команду sudo renice -n 5 -p 24115

```
student@CsnKhai:~$ sudo renice -n -5 -p 24115
[sudo] password for student:
24115 (process ID) old priority 19, new priority -5
```

Через це скоріш за все через топ можна лише зміньшувати пріорітет.

---

### 17. Вивчіть команду kill. Як за допомогою команди kill надіслати керуючий сигнал процесу? Наведіть приклад сигналів, що часто використовуються.

Зо допомогою команди kill можна kill -15 24115 де 15 сигнал м'якого завершення процесу а 24115 номер сигналу
```

24302 student   39  19    4236    536    476 R 99.1  0.2   3:49.40 md5sum
  893 student   20   0   11192   1900   1112 S  0.7  0.8   0:51.86 sshd
 3728 student   20   0    5452   1436   1000 R  0.3  0.6   0:00.01 top
    1 root      20   0    4180   2216   1416 S  0.0  0.9   0:00.86 init
    2 root      20   0       0      0      0 S  0.0  0.0   0:00.00 kthreadd

```
Основними кодами для команди є 15 для м'якого завершення процесу, 9 для примусового знищення 20 для зипинки процесу та 18 для вмикання зупиненого процесу

---

### 18. Команди jobs, fg, bg, nohup. Навіщо вони призначені? Використовуючи команду sleep, yes продемонструйте механізм керування процесами за допомогою fg, bg.

jobs — виводить список усіх завдань, запущених у поточному сеансі командної оболонки.

fg  — переводить фонове або зупинене завдання на передній план.

bg — відновлює виконання зупиненого завдання у фоновому режимі.

nohup — запускає команду так, щоб вона ігнорувала сигнал SIGHUP. Завдання продовжить працювати навіть після того, як ви закриєте термінал або вийдете з системи.

Далі буде продемонситровано роботу цих команд

Виконаємо sleep, yes та продемонстурємо bg

```
student@CsnKhai:~$ sleep 300
^Z
[4]+  Stopped                 sleep 300
student@CsnKhai:~$ jobs
[2]   Running                 nice -n 19 md5sum /dev/urandom &
[3]-  Running                 nohup md5sum /dev/urandom &
[4]+  Stopped                 sleep 300
student@CsnKhai:~$ bg %
[4]+ sleep 300 &
```

Тепер виконаємо команду yes

```
student@CsnKhai:~$ yes "Hello" > /dev/null &
[5] 22186
student@CsnKhai:~$ jobs
[2]   Running                 nice -n 19 md5sum /dev/urandom &
[3]   Running                 nohup md5sum /dev/urandom &
[4]-  Running                 sleep 300 &
[5]+  Running                 yes "Hello" > /dev/null &
student@CsnKhai:~$ fg %5
yes "Hello" > /dev/null
^C
```

fg, jobs
```
student@CsnKhai:~$ nano test1
Use "fg" to return to nano.

[3]+  Stopped                 nano test1
student@CsnKhai:~$
student@CsnKhai:~$ nano test2
Use "fg" to return to nano.
[4]+  Stopped                 nano test2
student@CsnKhai:~$ jobs
[2]   Running                 nice -n 19 md5sum /dev/urandom &
[3]-  Stopped                 nano test1
[4]+  Stopped                 nano test2
student@CsnKhai:~$ fg 3
nano test1
student@CsnKhai:~$ fg 4
nano test2
student@CsnKhai:~$ jobs
[2]+  Running                 nice -n 19 md5sum /dev/urandom &

```

nohup
```
student@CsnKhai:~$ nohup md5sum /dev/urandom &
[3] 17032
student@CsnKhai:~$ nohup: ignoring input and appending output to ‘nohup.out’
nohup md5sum /dev/u
student@CsnKhai:~$ jobs
[2]-  Running                 nice -n 19 md5sum /dev/urandom &
[3]+  Running                 nohup md5sum /dev/urandom &
```
---

## Висновки

У результаті виконання лабораторної роботи №4 було комплексно досліджено підсистему управління процесами та віртуальну файлову систему /proc в ОС Linux. На практиці опрацьовано життєвий цикл і стани процесів (R, S, Z тощо), побудовано дерева їхніх взаємозв'язків за допомогою утиліти pstree, а також вивчено методи зчитування системних метрик ЦП і конкретних процесів через файли /proc/cpuinfo та /proc/<PID>/status. Окрім цього, закріплено навички моніторингу за допомогою ps і top, продемонстровано реальний вплив пріоритетів (nice/renice) на розподіл ресурсів ЦП між обчислювальними задачами, та засвоєно механізми керування керуючими сигналами (kill) і фоновими завданнями (jobs, fg, bg, nohup).