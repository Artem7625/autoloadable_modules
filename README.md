# DynamicModuleLoader

DynamicModuleLoader - это пример веб-сервиса и клиента на Python для сортировки словаря с использованием динамически подгружаемых модулей. Клиент считывает данные из файла JSON и отправляет их на сервер с использованием запроса POST по маршруту /json/. В ответ клиетну приходит отсортированный словарь.

# Начало работы
1. Клонируйте репозиторий, выполнив команду:
   ```
   git clone https://github.com/Artem7625/DynamicModuleLoader.git
   ```
2. Если вы используете GIT, в корневой директории создайте файл .gitignore и пропишите в нем имена файлов и папок, которые не нужно отслеживать, например:
   ```
   venv
   logs
   .env
   .gitignore
   __pycache__
   ```

3. В корневой директории создайте и активируйте виртуальное окружение, выполнив команду:
   ```
   python3 -m venv venv
   ```

   Активация:
   Для Linux (в терминале):
   ```
   source venv/bin/activate
   ```
   Для Windows (в командной строке или PowerShell):
   ```
   venv\Scripts\activate
   ```

   Установите зависимости, выполнив команду:
   ```
   pip install -r requirements.txt
   ```

4. В корневой директории создайте текстовый файл .env, выполнив команду:

   Для Linux (в терминале):
   ```
   touch .env
   ```
   Для Windows (в командной строке или PowerShell):
   ```
   type nul > .env
   ```

   В файле .env пропишите конфигурационные данные, как показано в файле .env.example:


5. Перейдите в директорию app/ и запустите сервер, выполнив команду:
   ```
   python3 main.py
   ```
   Для доступа к веб-сервису откройте веб-браузер и перейдите по адресу:
   ```
   http://localhost:8000.
   ```
   Используйте порт, который прописан в файле .env

# Использование
### Отправка JSON-данных
Используйте предоставленный клиент или любой другой инструмент на ваш выбор для отправки JSON-данных на сервер по маршруту /json/ с помощью запроса POST. Убедитесь, что JSON-данные соответствуют указанному формату:
```
{
	"module": "myname",
	"function": "mytest",
	"data": {
		"ERROR number 1": {
			"ident": "2.1.11",
			"value": " test   test   "
		},
		"ERROR number 2": {
			"ident": "2.1.2",
			"value": " bla bla   "
		},
		"ERROR number 3": {
			"ident": "2.5",
			"value": "Boo Boo     Boo"
		}
	}
}
```
Поместите JSON данные в файл data.json в корневой директории проекта.

Для просмотра списка доступных для автозагрузки функций посетите http://localhost:8000/html/ в веб-браузере. Используйте порт, который прописан в файле .env


# Описание скрипта
### Как работает скрипт
Этот скрипт предназначен для сортировки словаря по заданным правилам и вывода доступных для динамического импорта функций в виде таблицы. В целях безопасности в переменных окружения явно приписываются названия модулей и входящих в них функций, доступных для динамического импорта.

Пользователь отправляет POST-запрос с данными, в которых указаны:
- название модуля
- название функции
- словарь, который нужно отсортировать


Названия модуля и функции извлекаются из данных запроса. На основе этих данных динамически подгружается функция для сортировки словаля по ключу "data".
Если модуль или функция отсутствуют или запрещены для импорта, сервер вернет ошибку с кодом 500.

### API эндпоинты
Скрипт использует следующие API эндпоинты:

1. #### /json

- Метод: POST
- Описание: Этот эндпоинт используется для отправки JSON-данных на сервер для обработки. Данные должны соответствовать определенной структуре JSON.

##### Ответ:
HTTP-статус: 200 (OK)
Тип содержимого: JSON

2. #### /html/

- Метод: GET
- Описание: Этот эндпоинт предоставляет веб-страницу с таблицей, на которой отображаются доступные функции для автоматической загрузки, их исходный код и докстринги.

##### Ответ
- HTTP-статус: 200 (OK)
- Тип содержимого: HTML

### Зависимости и установка
Для работы скрипта необходимы следующие зависимости:

- Python 3.x
- FastAPI (для обработки запросов)
- requests (для отправки запросов)
- environs (для работы с переменными окружения)

# Возможность масштабируемости и расширяемости приложения
Архитектура приложения позволяет легко его масштабировать.

1. Добавление новых модулей и функций.
Приложение уже поддерживает динамическое импортирование модулей и функций. Это позволяет добавлять новые функциональные возможности без изменения исходного кода приложения. Просто создайте новый модуль и функцию, добавьте их в вашу конфигурацию, и они станут доступными для вызова через API вашего приложения.

2. Управление доступом и разрешениями.
Вы можете легко управлять доступом к новым модулям и функциям, используя класс PermissionChecker. Передавайте этот класс как зависимость в ваши маршруты API, и вы сможете контролировать, какие пользователи имеют доступ к различным функциям. Можно создать более сложные системы управления разрешениями, если это необходимо.

3. Масштабирование горизонтально.
Если ваше приложение стало очень нагруженным, вы можете масштабировать его горизонтально, добавляя дополнительные инстансы приложения и балансировку нагрузки между ними. FastAPI хорошо масштабируется и может работать с большим количеством одновременных запросов.

# Лицензия
Этот проект распространяется под лицензией MIT. Вы можете свободно использовать, изменять и распространять его в соответствии с условиями лицензии.