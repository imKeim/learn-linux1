## Цели

Проверка базовых навыков управления пользователями, группами, правами и процессами

## Задачи

В этой работе вы будете управлять учетными записями пользователей и групп, устанавливать разрешения для файлов и каталогов и управлять процессами.


## Инструкции

Войдите на **workstation** как пользователь *student* с паролем *student*.

На workstation запустите сценарий `lab rhcsa-rh124-review2 start`, чтобы начать обзорную работу. Этот сценарий запускает процесс, потребляющий максимум ресурсов процессора, и создает необходимые файлы для настройки среды.

```
[student@workstation ~]$ lab rhcsa-rh124-review2 start
```

Для выполнения этого упражнения необходимо выполнить следующие задачи на **serverb**.

1.	Завершите процесс, который использует больше всего процессорного времени.
2.	Создайте новую группу с именем *database* и GID 50000.
3.	Создайте нового пользователя с именем *dbuser1*, входящего в дополнительную группу *database*. Начальный пароль пользователя *dbuser1* ― *redhat*. Настройте принудительное изменение пароля пользователя *dbuser1* при первом входе в систему. Пользователь *dbuser1* должен иметь возможность сменить пароль через 10 дней после первого изменения пароля. Срок действия пароля *dbuser1* должен заканчиваться через 30 дней после изменения пароля.
4.	Предоставьте *dbuser1* возможность использовать команду `sudo` для выполнения команд от имени привилегированного пользователя.
5.	Настройте для пользователя *dbuser1* маску по умолчанию 007.
6.	Создайте каталог **/home/student/grading/review2** с пользователем-владельцем *student* и группой-владельцем *database*. Настройте разрешения для этого каталога, чтобы любой новый файл в нем наследовал группу-владельца *database*, независимо от того, какой пользователь создал файл. Разрешения для **/home/student/grading/review2** должны предоставлять участникам группы *database* и пользователю *student* доступ к каталогу и возможность создания содержимого в нем. Все остальные пользователи должны иметь разрешения на чтение и выполнение для этого каталога. Кроме того, пользователям должно быть разрешено удалять из каталога **/home/student/grading/review2** только те файлы, которые им принадлежат, но не файлы, принадлежащие другим пользователям.

## Оценка

На **workstation** выполните команду `lab rhcsa-rh124-review2 grade`, чтобы проверить, правильно ли было выполнено упражнение.

```
[student@workstation ~]$ lab rhcsa-rh124-review2 grade
```

## Конец

На **workstation** запустите сценарий `lab rhcsa-rh124-review2 finish`, чтобы завершить обзорную работу. Этот сценарий завершает процесс, удаляет файлы и каталоги, созданные в начале обзорной работы, и обеспечивает очистку среды **serverb**.

```
[student@workstation ~]$ lab rhcsa-rh124-review2 finish
```

Обзорная работа завершена.