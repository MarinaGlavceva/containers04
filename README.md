# Лабораторная работа: Запуск Apache в Docker

## Цель работы
В данной лабораторной работе целью является ознакомиться с основными командами Docker и процессом развертывания веб-сервера Apache в контейнере Ubuntu.

---

## Задание
1. Запустить контейнер Ubuntu в Docker.
2. Установить веб-сервер Apache.
3. Настроить отображение страницы с текстом "Hello, World!".
4. Провести тестирование.
5. Описать процесс выполнения с пояснениями команд и их выводов.

---

## Подготовка
Перед началом работы я убедилась, что Docker установлен на моем компьютере.Проверила  работоспособность Docker:
```sh
docker --version
```

Затем я создала репозиторий `containers04` на GitHub и склонировала его себе:
```sh
git clone https://github.com/MarinaGlavceva/containers04
cd containers04
```

Создала файл `README.md` и добавила в него описание проекта.

---

## Выполнение
### 1. Запуск контейнера Ubuntu
Я запустила контейнер Ubuntu с пробросом порта 8000 на 80 порт контейнера и назвала его `containers04`:

```sh
docker run -ti -p 8000:80 --name containers04 ubuntu bash
```
#### Объяснение команды:
- **`docker run`** – Запуск нового контейнера.
- **`-ti`** – Режим интерактивной работы (терминал + ввод команд).
- **`-p 8000:80`** – Проброс порта 8000 хоста в порт 80 контейнера.
- **`--name containers04`** – Присвоение имени контейнеру.
- **`ubuntu`** – Использование образа Ubuntu.
- **`bash`** – Запуск командной оболочки Bash внутри контейнера.

**Вывод в консоли:**
![Image](https://github.com/user-attachments/assets/18153567-68b2-4fae-9d24-65708b35267f)

### 2. Установка Apache в контейнере
Я обновила список пакетов и установила веб-сервер Apache:
```sh
apt update
apt install apache2 -y
```
#### Объяснение команд:
1. **`apt update`**  
   - **Назначение:** Обновляет список пакетов и их версий из репозиториев.  
   - **Что делает?**  
     - Загружает самую свежую информацию о доступных пакетах из официальных источников.  
     - Обновляет кэш списка пакетов в системе.  
   - **Вывод в консоли:** Показывает процесс получения списка пакетов с серверов (`Get:1 http://archive.ubuntu.com/...`).

2. **`apt install apache2 -y`**  
   - **Назначение:** Устанавливает пакет Apache (веб-сервер).  
   - **Что делает?**  
     - Загружает Apache и его зависимости.  
     - Автоматически устанавливает его в систему.  
     - Флаг `-y` позволяет установить пакет без запроса подтверждения.  
   - **Вывод в консоли:** Показывает процесс установки (`Reading package lists... Done`, `Setting up apache2 ...`).


Запустила Apache:
```sh
service apache2 start
```
#### Объяснение команды:
- **`service apache2 start`** – Запускает веб-сервер Apache.

**Вывод в консоли:**
```
 * Starting Apache httpd web server apache2
```

### 3. Проверка работы Apache
Я открыла браузер и перешла по адресу `http://localhost:8000`. В браузере отобразилась стандартная стартовая страница Apache.

### 4. Добавление страницы "Hello, World!"
Проверила содержимое веб-директории:
```sh
ls -l /var/www/html/
```

Создала файл `index.html` с текстом:
```sh
echo '<h1>Hello, World!</h1>' > /var/www/html/index.html
```
#### Объяснение команд:
1. **`ls -l /var/www/html/`** – Проверяет файлы в веб-директории.
   **Вывод в консоли:**
   ```
   total 0
   ```
2. **`echo '<h1>Hello, World!</h1>' > /var/www/html/index.html`** – Создает страницу с текстом.

Обновила страницу в браузере и увидела `Hello, World!`.

### 5. Просмотр конфигурации Apache
Перешла в каталог конфигурации:
```sh
cd /etc/apache2/sites-enabled/
```

Вывела содержимое конфигурационного файла:
```sh
cat 000-default.conf
```
#### Объяснение команд:
1. **`cd /etc/apache2/sites-enabled/`** – Переход в каталог с файлами виртуальных хостов.
2. **`cat 000-default.conf`** – Выводит конфигурацию виртуального хоста.

**Вывод в консоли:**

#### Разбор содержимого `000-default.conf`:
Этот файл содержит настройки виртуального хоста Apache. В нем я увидел:
- **`<VirtualHost *:80>`** – Определяет виртуальный хост, который слушает HTTP-запросы на порту 80.
- **`ServerAdmin webmaster@localhost`** – Указывает email администратора сервера.
- **`DocumentRoot /var/www/html`** – Определяет корневую директорию веб-сайта, куда загружаются файлы.
- **`ErrorLog ${APACHE_LOG_DIR}/error.log`** – Путь к файлу логов ошибок.
- **`CustomLog ${APACHE_LOG_DIR}/access.log combined`** – Путь к файлу логов обращений.
- Некоторые закомментированные строки, указывающие, что можно задавать настройки SSL, логи и другие параметры.
- 
### 6. Остановка и удаление контейнера
Вышла из контейнера:
```sh
exit
```

Проверила список контейнеров:
```sh
docker ps -a
```

Удалила контейнер:
```sh
docker rm containers04
```

**Вывод в консоли:**
```
CONTAINER ID   IMAGE      COMMAND   CREATED   STATUS   NAMES
b5bb22a525ca   ubuntu     "bash"    2 mins ago   Exited   containers04
```

## Выводы
В ходе выполнения лабораторной работы я:
- Освоила базовые команды Docker.
- Запустила контейнер Ubuntu.
- Установила и запустила Apache в контейнере.
- Настроила веб-страницу и отобразила ее в браузере.
- Изучила структуру конфигурации Apache.
- Научилась управлять контейнерами (создавать, запускать, останавливать, удалять).

Этот опыт поможет мне в дальнейшей работе с контейнерами и развертыванием веб-приложений.



