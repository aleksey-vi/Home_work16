# OTUS ДЗ 14 PAM

## Домашнее задание

PAM
1. Запретить всем пользователям, кроме группы admin логин в выходные (суббота и воскресенье), без учета праздников

### Установка Ansible

Для начала создадим группу admin и пользователей, входящих и не входящих в группу admin:

    groupadd admin
    useradd admin1 && echo "pass"|passwd --stdin admin1 && usermod -a -G admin admin1; \\
    useradd notadmin1 && echo "pass"|passwd --stdin notadmin1

Далее воспользуемся комбинацией из двух модулей pam_succeed_if и pam_time

Добавим правило в time.conf

разрешить всем любые действия в системе в любой день кроме выходных

    echo `*;*;*;!Wd0000-2400` >> /etc/security/time.conf

Добавим правила в файл /etc/pam.d/system-auth:

```
account [success=1 default=ignore] pam_succeed_if.so user ingroup admin
account required pam_time.so
```
Логика работы правил: если пользователь входит в группу admin, следующее правило (account required pam_time.so) пропускается. Иначе производится проверка на время модулем pam_time.so.

### Проверка
Не забываем, что пароль - pass

Выполним команды:

    sudo date +%Y%m%d -s "20200807"
    date

Вывод:

Mon Aug 07 00:00:07 UTC 2020 (не выходной)  

su admin1 - доступ разрешён
su notadmin1 - доступ разрешён

Выполним команды:

      sudo date +%Y%m%d -s "20200808"
      date
Вывод:

Sun Aug 08 00:00:02 UTC 2020 (выходной)  

su admin1 - доступ разрешён
su notadmin1 - доступ запрещён
