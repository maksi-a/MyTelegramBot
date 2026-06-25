# MyTelegramBot

Оптимизированный Telegram-бот на Python и aiogram 3 для Windows 11 и VS Code.

## Что нужно установить

1. Python 3.11 или новее.
2. VS Code.
3. Расширение Python для VS Code.
4. Библиотеки из `requirements.txt`.

## Важно: сначала откройте папку проекта

Команды нужно выполнять **не** из `C:\WINDOWS\system32`, а из папки, где лежат файлы проекта `bot.py` и `requirements.txt`.

Пример для PowerShell:

```powershell
cd C:\MyTelegramBot
```

Если проект лежит в другой папке, укажите свой путь:

```powershell
cd "C:\Users\Ваше_имя\Desktop\MyTelegramBot"
```

Проверить, что вы в правильной папке, можно так:

```powershell
dir
```

В списке файлов должны быть `bot.py`, `requirements.txt`, `.env.example` и `README.md`.

## Установка зависимостей в PowerShell

```powershell
cd C:\MyTelegramBot
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Если PowerShell пишет, что запуск скриптов отключён, выполните один раз:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

После этого закройте PowerShell, откройте его снова, перейдите в папку проекта и повторите активацию:

```powershell
cd C:\MyTelegramBot
.\.venv\Scripts\Activate.ps1
```

Если не хотите менять политику PowerShell, можно активировать окружение через обычный CMD:

```cmd
cd C:\MyTelegramBot
.venv\Scripts\activate.bat
python -m pip install --upgrade pip
pip install -r requirements.txt
```

## Настройка переменных окружения в PowerShell

Перед запуском укажите токен бота и Telegram ID администратора:

```powershell
$env:BOT_TOKEN="ваш_новый_токен_от_BotFather"
$env:ADMIN_ID="ваш_telegram_id"
python bot.py
```

> Не храните реальный токен бота в коде или публичном репозитории.

## Что означают частые ошибки при установке

### `UnauthorizedAccess` или `PSSecurityException` при `Activate.ps1`

PowerShell заблокировал запуск скрипта активации виртуального окружения. Это стандартная защита Windows, а не ошибка Python и не ошибка бота.

Исправление:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### Как понять, что виртуальное окружение активировалось

После успешной активации слева в PowerShell должно появиться `(.venv)`, например:

```powershell
(.venv) PS C:\MyTelegramBot>
```

Если `(.venv)` нет, окружение не активировано. В этом случае `pip install -r requirements.txt` может поставить библиотеки в общий Python, а не в папку проекта.

Если активация через PowerShell заблокирована, выполните:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

Затем закройте PowerShell, откройте его снова и выполните:

```powershell
cd C:\MyTelegramBot
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### `No such file or directory: 'requirements.txt'`

Команда `pip install -r requirements.txt` запущена не из папки проекта. На скриншоте видно, что терминал находится в `C:\WINDOWS\system32`, поэтому файл `requirements.txt` там не найден.

Исправление:

```powershell
cd C:\MyTelegramBot
pip install -r requirements.txt
```

Если проект лежит не в `C:\MyTelegramBot`, замените путь на свой.

### Не вставляйте токен внутрь `os.getenv()`

Неправильно:

```python
BOT_TOKEN = os.getenv("123456:ABCDEF")
ADMIN_ID = int(os.getenv("8449031023", "0"))
```

`os.getenv()` принимает не сам токен, а имя переменной окружения. Поэтому такая запись ищет переменную с именем `123456:ABCDEF` и возвращает `None`.

Правильно оставлять в `bot.py` так:

```python
BOT_TOKEN = os.getenv("BOT_TOKEN")
ADMIN_ID = int(os.getenv("ADMIN_ID", "0"))
```

А реальные значения указывать в файле `C:\MyTelegramBot\.env`:

```text
BOT_TOKEN=ваш_новый_токен_от_BotFather
ADMIN_ID=8449031023
```

### `RuntimeError: Не задан BOT_TOKEN` после ввода `$env:BOT_TOKEN`

Если бот пишет, что `BOT_TOKEN` не задан, значит Python не видит переменную окружения с точным именем `BOT_TOKEN`. Частая причина — опечатка или случайная русская буква в имени переменной, например `ВОТ_TOKEN` вместо `BOT_TOKEN`.

Проверьте переменные перед запуском:

```powershell
cd C:\MyTelegramBot
.\.venv\Scripts\Activate.ps1
$env:BOT_TOKEN="ваш_новый_токен_от_BotFather"
$env:ADMIN_ID="ваш_telegram_id"
Write-Host $env:BOT_TOKEN
Write-Host $env:ADMIN_ID
python -c "import os; print(os.getenv('BOT_TOKEN')); print(os.getenv('ADMIN_ID'))"
python bot.py
```

Если `Write-Host $env:BOT_TOKEN` или команда `python -c ...` выводит пустую строку, удалите строку и наберите имя переменной заново латинскими буквами: `BOT_TOKEN`.

Не используйте старый токен, который был отправлен в чат или опубликован в коде. Перевыпустите токен в `@BotFather` и используйте новый.

## Если Windows создал файлы с лишним `.txt`

На Windows часто включено скрытие расширений файлов. Из-за этого Блокнот может создать файл не `.env.example`, а `.env.example.txt`. В вашем случае на скриншоте также видны `requirements.txt.txt`, `README.md.txt` и `run_bot.bat.txt`.

Переименуйте их в PowerShell из папки проекта:

```powershell
cd C:\MyTelegramBot
Rename-Item .env.example.txt .env.example
Rename-Item requirements.txt.txt requirements.txt
Rename-Item README.md.txt README.md
Rename-Item run_bot.bat.txt run_bot.bat
```

После переименования проверьте файлы:

```powershell
dir
```

Должно получиться примерно так:

```text
.env.example
requirements.txt
README.md
run_bot.bat
bot.py
```

Чтобы Windows больше не добавлял `.txt` незаметно, включите показ расширений: Проводник → Вид → Показать → Расширения имён файлов.

## Полный запуск с нуля через `.env`

Если PowerShell снова не передаёт `BOT_TOKEN` в Python, используйте локальный файл `.env`. Это самый простой вариант для Windows.

1. Остановите старые процессы Python:

```powershell
taskkill /F /IM python.exe
taskkill /F /IM pythonw.exe
```

Если PowerShell напишет, что процесс не найден, это нормально.

2. Перейдите в папку проекта:

```powershell
cd C:\MyTelegramBot
```

3. Создайте файл `.env` в `C:\MyTelegramBot`. Внутри должны быть две строки:

```text
BOT_TOKEN=ваш_новый_токен_от_BotFather
ADMIN_ID=8449031023
```

4. Проверьте, что файл называется именно `.env`, а не `.env.txt`:

```powershell
dir -Force
```

5. Активируйте окружение и запустите бота:

```powershell
.\.venv\Scripts\Activate.ps1
python bot.py
```

Файл `.env` содержит секреты, поэтому он добавлен в `.gitignore` и не должен попадать в GitHub или публичные чаты.

## Как изменить `.env` без открытия Блокнота

Можно полностью перезаписать файл `.env` из PowerShell. Находясь в папке проекта, выполните:

```powershell
cd C:\MyTelegramBot
@"
BOT_TOKEN=ваш_новый_токен_от_BotFather
ADMIN_ID=8449031023
"@ | Set-Content -Encoding UTF8 .env
```

После этого проверьте конфигурацию:

```powershell
python check_config.py
```

Также можно запустить `set_env.bat`: он спросит `BOT_TOKEN` и `ADMIN_ID`, сам перезапишет `.env` и запустит проверку.

```powershell
cd C:\MyTelegramBot
.\set_env.bat
```

## Диагностика, если ошибка `Не задан BOT_TOKEN` осталась

Если после создания `.env` ошибка осталась, запустите диагностический скрипт:

```powershell
cd C:\MyTelegramBot
.\.venv\Scripts\Activate.ps1
python check_config.py
```

Он покажет:

- найден ли файл `C:\MyTelegramBot\.env`;
- есть ли в нём `BOT_TOKEN` и `ADMIN_ID`;
- видит ли Python переменные окружения;
- обновлён ли `bot.py` до версии с поддержкой `.env`.

Если в выводе будет `Поддержка .env в bot.py: НЕТ`, значит у вас старая версия `bot.py`. Замените `bot.py` на актуальный файл из проекта.

## Если `check_config.py` видит токен, а `bot.py` всё равно пишет `Не задан BOT_TOKEN`

Причина может быть в BOM-метке UTF-8, которую иногда добавляет Windows PowerShell при создании `.env` через `Set-Content`. В актуальной версии `bot.py` файл `.env` читается через `utf-8-sig`, поэтому BOM автоматически убирается.

Проверьте, что в `bot.py` строка чтения `.env` выглядит так:

```python
for raw_line in env_path.read_text(encoding="utf-8-sig").splitlines():
```

После этого запустите:

```powershell
cd C:\MyTelegramBot
python check_config.py
python bot.py
```

## Финальный чек-лист запуска

Если `python check_config.py` показывает `.env найден: ДА`, `BOT_TOKEN` найден, `ADMIN_ID` найден и `Поддержка .env в bot.py: ДА`, значит настройки уже читаются правильно. Строки `BOT_TOKEN: НЕ НАЙДЕНО` и `ADMIN_ID: НЕ НАЙДЕНО` в разделе переменных окружения PowerShell/Windows не являются ошибкой, если значения найдены в `.env`.

Проверьте только эти места:

1. `C:\MyTelegramBot\bot.py` должен содержать `BOT_TOKEN = os.getenv("BOT_TOKEN")` и `ADMIN_ID = int(os.getenv("ADMIN_ID", "0"))`. Не вставляйте токен напрямую в `bot.py`.
2. `C:\MyTelegramBot\.env` должен содержать реальные значения:

```text
BOT_TOKEN=ваш_новый_токен_от_BotFather
ADMIN_ID=8449031023
```

3. В токене не должно быть лишних символов в конце, например `?`, пробелов или кавычек.
4. Запускать нужно из папки `C:\MyTelegramBot`:

```powershell
cd C:\MyTelegramBot
.\.venv\Scripts\Activate.ps1
python check_config.py
python bot.py
```

Если после этого появляется уже не `Не задан BOT_TOKEN`, а другая ошибка, значит проблема больше не в `.env`; нужно читать новый текст ошибки.

## Что делать после установки

После установки зависимостей нужно запустить бота. Есть два варианта.

### Вариант 1: запуск через PowerShell

```powershell
cd C:\MyTelegramBot
.\.venv\Scripts\Activate.ps1
$env:BOT_TOKEN="ваш_новый_токен_от_BotFather"
$env:ADMIN_ID="ваш_telegram_id"
python bot.py
```

Если запуск успешный, в терминале появится сообщение:

```text
✅ Бот запущен и готов к работе!
```

После этого откройте своего бота в Telegram и отправьте команду `/start`.

### Вариант 2: запуск через `run_bot.bat`

Можно дважды нажать на файл `run_bot.bat` в папке проекта или запустить его из терминала:

```powershell
cd C:\MyTelegramBot
.\run_bot.bat
```

Скрипт сам перейдёт в папку проекта, активирует `.venv`, попросит `BOT_TOKEN` и `ADMIN_ID`, если они не заданы, и запустит `bot.py`.

## Файлы проекта

- `bot.py` — основной код бота.
- `requirements.txt` — зависимости Python.
- `.env.example` — пример нужных переменных окружения.
- `cat.jpg` — необязательная картинка для команды `/cat`.
- `bot.db` — SQLite-база, создаётся автоматически после запуска.
- `bot.log` — лог-файл, создаётся автоматически после запуска.
- `run_bot.bat` — удобный запуск бота на Windows.
- `check_config.py` — диагностика `.env`, переменных окружения и версии `bot.py`.
- `set_env.bat` — удобная перезапись `.env` без открытия Блокнота.

## Запуск как Windows-служба через NSSM

Для постоянной работы бота на Windows можно установить его как службу через NSSM. Рекомендуется запускать бота через Python из виртуального окружения проекта, а не через общий Python.

Рекомендуемые поля в NSSM:

```text
Path: C:\MyTelegramBot\.venv\Scripts\python.exe
Startup directory: C:\MyTelegramBot
Arguments: bot.py
```

Если вы специально хотите использовать общий Python, можно указать:

```text
Path: C:\Users\maks\AppData\Local\Programs\Python\Python313\python.exe
Startup directory: C:\MyTelegramBot
Arguments: bot.py
```

Но в этом случае `aiogram` должен быть установлен именно в общий Python 3.13. Надёжнее использовать `.venv`, потому что зависимости проекта установлены там.

### Установка службы через командную строку администратора

Откройте PowerShell или CMD от имени администратора и выполните, заменив путь к `nssm.exe` на свой:

```powershell
C:\nssm\win64\nssm.exe install MyTelegramBot
```

В открывшемся окне NSSM укажите:

- `Path` — `C:\MyTelegramBot\.venv\Scripts\python.exe`
- `Startup directory` — `C:\MyTelegramBot`
- `Arguments` — `bot.py`

На вкладке `I/O` можно указать лог-файлы:

```text
Output: C:\MyTelegramBot\service-output.log
Error: C:\MyTelegramBot\service-error.log
```

Затем нажмите `Install service`.

### Запуск, остановка и перезапуск службы

```powershell
nssm start MyTelegramBot
nssm stop MyTelegramBot
nssm restart MyTelegramBot
```

Или через стандартные команды Windows:

```powershell
net start MyTelegramBot
net stop MyTelegramBot
```

### Если служба NSSM уже установлена

Ошибка `Couldn't create service! Perhaps it is already installed...` означает, что служба с именем `MyTelegramBot` уже существует. Повторно нажимать `Install service` с тем же именем нельзя.

Если нужно только добавить или изменить логи, удалять службу не обязательно. Откройте редактирование:

```powershell
nssm edit MyTelegramBot
```

Затем на вкладке `I/O` укажите:

```text
Output: C:\MyTelegramBot\service-output.log
Error: C:\MyTelegramBot\service-error.log
```

Сохраните изменения и перезапустите службу:

```powershell
nssm restart MyTelegramBot
```

Если хотите полностью удалить и установить заново, выполните PowerShell или CMD от имени администратора:

```powershell
nssm stop MyTelegramBot
nssm remove MyTelegramBot confirm
```

Если после удаления служба всё ещё видна в Windows, выполните:

```powershell
sc delete MyTelegramBot
```

После удаления снова установите службу:

```powershell
nssm install MyTelegramBot
```

### Проверка после запуска службы

1. Проверьте, что файл `C:\MyTelegramBot\.env` существует и содержит `BOT_TOKEN` и `ADMIN_ID`.
2. Проверьте логи:

```text
C:\MyTelegramBot\bot.log
C:\MyTelegramBot\service-output.log
C:\MyTelegramBot\service-error.log
```

3. Откройте Telegram и отправьте боту `/start`.

Если меняете `bot.py` или `.env`, перезапустите службу:

```powershell
nssm restart MyTelegramBot
```

## Команды бота

- `/start` — запустить или перезапустить бота.
- `/help` — показать справку.
- `/id` — показать Telegram ID пользователя.
- `/links` — показать inline-кнопки.
- `/status` — показать статус бота.
- `/users` — список пользователей, только для администратора.
- `/broadcast текст` — рассылка, только для администратора.
- `/cat` — отправить `cat.jpg`, если файл есть в папке проекта.
