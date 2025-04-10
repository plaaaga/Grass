
# Руководство по установке Grass 1.25X

## 📦 Требования

* Windows или Ubuntu (Linux)
* Доступ к последней версии Grass Bot
* (Для Ubuntu) установленный пакет `screen`

---

## 🪟 Установка на Windows

1. Скачайте последнюю версию  **Grass Bot** .
2. Распакуйте все файлы в выбранную директорию.
3. Убедитесь, что папка `config` находится в одной директории с файлом `run.exe`. И ЗАПОЛНИТЕ ЕЕ НОРМАЛЬНО!!!! так же заполните settings.yaml !!!
4. Запустите бот двойным щелчком по файлу:

```bash
run.exe
```

---

## 🐧 Установка на Ubuntu

### 1. Создание рабочей директории

```bash
mkdir /home/grass-bot
```

### 2. Загрузка файлов

* Перенесите исполняемый файл Linux-версии и папку `config` в `/home/grass-bot` с помощью FTP/SFTP.

### 3. Запуск через `screen`

```bash
cd /home/grass-bot
screen -S grass-bot-session
chmod +x run_linux
./run_linux
# Для выхода из screen: Ctrl+A, затем D
```

---

## 💻 Управление screen-сессиями

### 📄 Просмотр всех сессий

```bash
screen -ls
```

### 🔁 Повторное подключение

Если статус сессии:

* `(Detached)` — можно просто подключиться:

```bash
screen -r grass-bot-session
```

* `(Attached)` — кто-то уже подключён, используйте принудительное подключение:

```bash
screen -d -r grass-bot-session
```

### ❌ Завершение сессии

```bash
screen -X -S grass-bot-session quit
```

---

## 🚨 Устранение ошибок

Если возникает ошибка:

```
Permission denied
```

То сделайте файл исполняемым:

```bash
chmod +x /home/grass-bot/run_linux
```

---

## 🧠 Примечание

Убедитесь, что файл запуска (`run_linux`) и папка `config` находятся в одной директории.

---

## 📁 Создание копии скрипта с новым screen

Если вы хотите запустить **ещё одну копию Grass Bot** с другим конфигом и логикой — например, для параллельного фарма — выполните следующие действия:

### 1. Создайте новую папку

```bash
mkdir /home/grass-bot-2
```

### 2. Скопируйте содержимое оригинальной директории

```bash
cp -r /home/grass-bot/* /home/grass-bot-2
```

> Убедитесь, что в новой папке вы изменили `config`, `settings.yaml` и другие нужные параметры, чтобы избежать конфликтов.

### 3. Сделайте исполняемый файл исполняемым (обязательно!)

```bash
chmod +x /home/grass-bot-2/run_linux
```

> Без этого бот просто не запустится. Не пропускайте.

### 4. Запустите копию с новым screen

```bash
cd /home/grass-bot-2
screen -S grass-bot-2
./run_linux
# Для выхода из screen: Ctrl+A, затем D
```

### 5. Управление новой screen-сессией

* 📄 Подключиться:

```bash
screen -r grass-bot-2
```

* 💥 Принудительно подключиться:

```bash
screen -d -r grass-bot-2
```

* ❌ Завершить:

```bash
screen -X -S grass-bot-2 quit
```

---

## 😠 Разочарование

Если вы дошли до этого пункта — значит, вам  **пришлось создавать ещё одну копию скрипта** , править конфиги, настраивать новый `screen`, и не забыть про `chmod +x`.

Это больно, это муторно, и, скорее всего, вы сделали это ночью, залипая в терминал.

**Но теперь у вас есть два (или больше) бота. А значит, вы победили.**

---

## 📊 Создание базы данных PostgreSQL на Linux

Выполните следующие команды:

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib -y
sudo -u postgres psql
```

Затем внутри консоли psql:

```sql
CREATE DATABASE grass;
CREATE USER grassuser WITH PASSWORD 'grasspass';
GRANT ALL PRIVILEGES ON DATABASE grass TO grassuser;
\c grass
GRANT ALL ON SCHEMA public TO grassuser;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO grassuser;
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO grassuser;
ALTER SCHEMA public OWNER TO grassuser;
```

После этого в `settings.yaml` на строке №5 укажите:

```yaml
database_url: "postgres://grassuser:grasspass@127.0.0.1:5432/grass"
```

---
