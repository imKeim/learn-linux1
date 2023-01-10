## Цели

После завершения этого раздела вы сможете войти в удаленную систему и выполнять команды с помощью `ssh`.

## Что такое OpenSSH?

*OpenSSH* реализует протокол SSH (Secure Shell) в системах Linux. Протокол SSH позволяет системам взаимодействовать друг с другом в зашифрованном и безопасном режиме в небезопасной сети.

Команда `ssh` позволяет установить безопасное подключение к удаленной системе, пройти аутентификацию от имени определенного пользователя и получить интерактивный сеанс командной оболочки в удаленной системе для этого пользователя. Кроме того, с помощью команды `ssh` можно выполнять отдельные команды в удаленной системе без запуска интерактивной оболочки.

## Примеры использования SSH

Следующая команда `ssh` позволяет войти на удаленный сервер **remotehost** от имени текущего локального пользователя. В этом примере удаленная система предлагает пройти аутентификацию, указав пароль этого пользователя.

```bash
[user01@host ~]$ ssh remotehost
user01@remotehost's password: redhat
...output omitted...
[user01@remotehost ~]$ 
```

С помощью команды `exit` можно выйти из удаленной системы.

```bash
[user01@remotehost ~]$ exit
logout
Connection to remotehost closed.
[user01@host ~]$ 
```

Следующая команда `ssh` позволяет войти на удаленный сервер **remotehost** от имени пользователя *user02*. Удаленная система опять же предлагает пройти аутентификацию, указав пароль этого пользователя.

```bash
[user01@host ~]$ ssh user02@remotehost
user02@remotehost's password: shadowman
...output omitted...
[user02@remotehost ~]$ 
```

Эта команда `ssh` выполняет команду `hostname` в удаленной системе **remotehost** от имени пользователя *user02* без доступа к удаленной интерактивной оболочке.

```bash
[user01@host ~]$ ssh user02@remotehost hostname
user02@remotehost's password: shadowman
remotehost.lab.example.com
[user01@host ~]$ 
```

Обратите внимание, что предыдущая команда отображала вывод на терминале локальной системы.

## Идентификация удаленных пользователей

Команда `w` отображает список пользователей, вошедших в систему. С ее помощью можно узнать, какие пользователи вошли в систему через **ssh**, из каких удаленных расположений и что они делают.

```bash
[user01@host ~]$ ssh user01@remotehost
user01@remotehost's password: redhat
[user01@remotehost ~]$ w
 16:13:38 up 36 min,  1 user,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
user02   pts/0    172.25.250.10    16:13    7:30   0.01s  0.01s -bash
user01   pts/1    172.25.250.10    16:24    3.00s  0.01s  0.00s w
[user02@remotehost ~]$ 
```

Приведенный выше вывод показывает, что пользователь *user02* вошел в систему на псевдотерминале 0 сегодня в 16:13 с хоста с IP-адресом 172.25.250.10 и бездействовал после приглашения командной оболочки в течение семи минут и тридцати секунд. Вывод также показывает, что пользователь *user01* вошел в систему на псевдотерминале 1 и бездействовал в течение последних трех секунд после выполнения команды `w`.

## Ключи хостов SSH

SSH защищает связь, используя шифрование с открытым ключом. Когда клиент SSH подключается к SSH-серверу, сервер отправляет копию открытого ключа клиенту до входа клиента в систему. Это необходимо для настройки безопасного шифрования канала связи и выполнения аутентификации между клиентом и сервером.

Когда пользователь выполняет команду `ssh` для подключения к серверу SSH, команда проверяет наличие копии открытого ключа для этого сервера в локальных файлах известных хостов. Системный администратор может заранее настроить его в файле **/etc/ssh/ssh_known_hosts**, или у пользователя может быть файл **~/.ssh/known_hosts** с этим ключом в домашнем каталоге.

Если у клиента есть копия ключа, команда `ssh` будет сравнивать ключ из файлов известных хостов для этого сервера с полученным ключом. Если ключи не совпадают, клиент предполагает, что сетевой трафик к серверу перехватывается или сервер был скомпрометирован, и запрашивает у пользователя подтверждение, продолжать подключение или нет.

<details>
<summary>Примечание</summary>

Установите значение *yes* для параметра **StrictHostKeyChecking** в файле **~/.ssh/config** пользователя или системном файле **/etc/ssh/ssh_config**, чтобы команда `ssh` всегда прерывала подключение SSH, если открытые ключи не совпадают.
</details>

Если у клиента нет копии открытого ключа в файлах известных хостов, команда `ssh` попросит подтвердить выполнение входа. Если вы сделаете это, копия открытого ключа будет сохранена в вашем файле **~/.ssh/known_hosts**, чтобы подлинность сервера в будущем подтверждалась автоматически.

```bash
[user01@host ~]$ ssh newhost
The authenticity of host 'remotehost (172.25.250.12)' can't be established.
ECDSA key fingerprint is SHA256:qaS0PToLrqlCO2XGklA0iY7CaP7aPKimerDoaUkv720.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'newhost,172.25.250.12' (ECDSA) to the list of known hosts.
user01@newhost's password: redhat
...output omitted...
[user01@newhost ~]$ 
```

### Управление ключами с помощью файлов известных хостов SSH

Если открытый ключ сервера изменился из-за того, что ключ был потерян при сбое жесткого диска или заменен по какой-либо обоснованной причине, вам потребуется отредактировать файлы известных хостов, заменив запись старого открытого ключа записью нового ключа, чтобы можно было входить в систему без ошибок.

Открытые ключи хранятся в файле **/etc/ssh/ssh_known_hosts**, а также в файле** ~/.ssh/known_hosts** каждого пользователя в клиенте SSH. Каждый ключ указан в одной строке. Первое поле — это список имен хостов и IP-адресов, которые используют этот открытый ключ. Второе поле — это алгоритм шифрования ключа. Последнее поле — это сам ключ.

```bash
[user01@host ~]$ cat ~/.ssh/known_hosts
remotehost,172.25.250.11 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBOsEi0e+FlaNT6jul8Ag5Nj+RViZl0yE2w6iYUr+1fPtOIF0EaOgFZ1LXM37VFTxdgFxHS3D5WhnIfb+68zf8+w=
```

Каждый удаленный SSH-сервер, к которому вы подключаетесь, сохраняет свой открытый ключ в каталоге **/etc/ssh** в файлах с расширением *.pub*.

```bash
[user01@remotehost ~]$ ls /etc/ssh/*key.pub
/etc/ssh/ssh_host_ecdsa_key.pub  /etc/ssh/ssh_host_ed25519_key.pub  /etc/ssh/ssh_host_rsa_key.pub
```

<details>
<summary>Примечание</summary>

Рекомендуется добавлять записи, соответствующие файлам ssh_host_*key.pub сервера, в свой файл ~/.ssh/known_hosts или системный файл /etc/ssh/ssh_known_hosts.
</details>