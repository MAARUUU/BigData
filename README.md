# Установка Spark на Ubuntu
*Spark* - это фреймворк (ПО, объединяющее готовые компоненты большого программного проекта), который используют для параллельной обработки неструктурированных или слабоструктурированных данных.

![](https://bigdataschool.ru/wp-content/uploads/2019/10/spark0.png)

## Подключение к удалённому серверу
 
```shell
# базовая команда подключения
# remote_host - IP-адрес или доменное имя узла, к которому вы пытаетесь подключиться.
ssh remote_host 
# если сервер привязан к домену и есть какой-то пользователь
ssh user@domen
```

## Основные команды терминала Ubuntu
- `cp file.js /home/test/` — копирует файл по указанному пути
- `lsof -i -n -P` — посмотреть работающие порты
- `kill PID` — убить порт по его PID
- `curl url` — посылает get запрос по указанному `url`
- `wget url` — скачивает файл
- `unzip file` — распаковка zip-архивов
- `tar -zxvf file.tgz` — распаковка tgz-архивов
- `echo "Hello World!"` — вывести сообщение в консоль
- `echo "Hello" >> hello.js` — запишет строку `“Hello”` в файл `hello.js`
- `htop` — покажет запущенные процессы и загруженность системы
- `ls` — посмотреть список файлов и папок в текущей директории
- `ls -lah` — посмотреть список всех файлов и папок в текущей директории с указанием прав на запись/чтение и размера файлов
- `ll` — сокращённая запись `ls -lah`
- `cd /folder`  — перейти в нужную папку
- `cd ..` — перейти в предыдущую папку
- `mkdir name` — создать папку
- `touch name.js` — создать файл
- `cat index.html` — вывести код файла в терминал
- `rm -rf file.js` — удаление папки или файла
- `pwd` — вернёт путь к папке, в который мы в данный момент находимся
- `clear` — очистить консоль
- `mv main.js index.js` — команда для перемещения файла из одной папки в другую, но часто используется для переименования файла
- `tree` - показать дерево файлов и директорий, начиная от корня
- `clock -w` - сохранить системное время в BIOS
- `shutdown -h now` - Остановить систему
- `shutdown -r now` - перегрузить систему
- `logout` - выйти из системы

## Терминальный мультиплексор tmux
Tmux — это менеджер терминалов, который позволяет работать с несколькими сессиями в одном окне. То есть вместо нескольких открытых окон терминала — вы используете одно, которое можно делить на несколько окон.
![](https://static.1cloud.ru/img/support/377.png)
Киллер фича ти-макса — сохранение состояний подключений и процессов. После разрыва соединения с сервером вы подключаетесь, и все запущенные программы и процессы продолжают работать. Дополнительно можно работать совместно с другими в терминале, если все подключены к одной сессии.

### Установка и настройка Tmux
Устанавливается Tmux из стандартных репозиториев Linux:
```shell
apt-get install tmux
```
После установки рекомендуется сразу отредактировать конфигурационный файл tmux (/etc/tmux.conf) и внести следующие изменения:
```shell
set -g mouse on
```
Эта строчка кода позволит свободно перемещать границы разделения окон с помощью курсора мышки.
### *Работа с сессиями в Tmux*
Для создания рабочей сессии без идентификатора — достаточно ввести tmux в терминале. Будет создана сессия 0:
![](https://static.1cloud.ru/img/support/378.png)
Идентификатор сессии отображается внизу слева в квадратных скобках. Для создания именной сессии достаточно ввести следующую команду:
```shell
tmux new -s название сессии
```
Поскольку tmux завершает соединение с сохранением состояния сессии, правильным способом возобновить работу ти-макса будет его запуск командой:
```shell
tmux attach || tmux new
```
Просмотреть список созданных сессий можно командой:
```shell
tmux ls
```
Закрыть все сессии можно командой:
```shell
tmux kill-server
```
## Список часто используемых команд и хоткейсов Tmux
*Команды для управления сессиями:*
- `tmux new [имя_сеанса]` — начать новый сеанс. Имя_сеанса опционально;
- `tmux attach -t [имя_сеанса]` - подключиться к уже существующей сессии. Если имя заранее не было задано, тогда команда будет выглядить так: tmux attach -t 0;
- `tmux ls` — список открытых сессий Tmux;
- `kill-server` — остановить все запущенные сессии;
- `kill-session -t [имя_сеанса]` — завершить сессию;
- `list-clients -t [имя_сеанса]` — посмотреть клиентов, подключенных к сессии;
- `list-sessions` — вывести список всех запущенных сессий.

*Хоткейсы для управления окнами:*
- `Ctrl + b, c` — создать новое окно;
- `Ctrl + b, w` — просмотреть список окон;
- `Ctrl + b, n` — следующее окно;
- `Ctrl + b, p` — предыдущее окно;
- `Ctrl + b, w` — следующее окно;
- `Ctrl + b, номер окна (цифрой)` — переключиться на нужное окно;
- `Ctrl + b, “` — горизонтальное разделение окна;
- `Ctrl + b, %` — вертикальное разделение окна.

## Начальная настройка сервера Ubuntu

Когда вы впервые создаете новый сервер Ubuntu, необходимо выполнить ряд важных шагов по конфигурации в рамках базовой настройки. Эти шаги помогут повысить уровень безопасности и удобства работы с сервером и послужат прочной основой для последующих действий.

### *Шаг 1 — Вход с привилегиями root*
Чтобы войти на сервер, вам нужно знать публичный IP-адрес вашего сервера. Также вам потребуется пароль или, если вы установили ключ SSH для аутентификации, приватный ключ для учетной записи root user. 
Если вы еще не подключились к серверу, выполните вход в систему как root user, используя следующую команду (замените выделенную часть команды на публичный IP-адрес вашего сервера):
```shell
ssh root@your_server_ip
```
### *Шаг 2 — Создание нового пользователя*
После входа в систему с правами root мы готовы добавить новую учетную запись пользователя. 
Этот пример создает нового пользователя с именем sammy:
```shell
adduser sammy
```
