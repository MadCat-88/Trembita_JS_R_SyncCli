# Розгортання вебклієнта в Docker
## Вимоги

| ПЗ             |   Версія   | Примітка                     |
|:---------------|:----------:|------------------------------|
| Docker         | **20.10+** |                              |
| Docker Compose |   10.5+    | Якщо планується використання |
| Git            |            | Для клонування репозиторію   |

## Змінні оточення

Вебклієнт підтримує конфігурацію через змінні оточення. Нижче наведено основні параметри:

 	•	PROTOCOL – Протокол, який використовується для взаємодії з системою Трембіта. Можливі варіанти: http або https. Наприклад, http використовується для простого підключення, а https – для захищеного підключення з автентифікацією.
	•	PURPOSE_ID – Ідентифікатор мети обробки персональних даних. Цей параметр використовується для інтеграції з Підсистемою моніторингу доступу до персональних даних (ПМДПД).
	•	SECURITY_SERVER_IP – Хост або IP-адреса сервера Трембіта, до якого підключається клієнт. Це може бути FQDN або локальна IP-адреса, наприклад 10.0.20.235.
	•	INSTANCE_NAME – Ідентифікатор клієнтської підсистеми в системі Трембіта. Наприклад, SEVDEIR-TEST може бути тестовим інстансом для клієнта.
	•	CLIENT_MEMBER_CLASS – Клас учасника системи Трембіта, наприклад GOV, який зазвичай використовується для урядових організацій.
	•	CLIENT_MEMBER_CODE – Унікальний код клієнта в системі Трембіта. Наприклад, 10000004 – це код ЄДРПОУ організації-клієнта.
	•	CLIENT_SUBSYSTEM – Код підсистеми організації-клієнта в системі Трембіта. Використовується для визначення конкретної підсистеми, яка буде обробляти запити. Наприклад, SUB_CLIENT.
	•	SERVICE_MEMBER_CLASS – Клас учасника для сервісу, зазвичай такий самий, як і для клієнта (GOV).
	•	SERVICE_MEMBER_CODE – Унікальний код організації-постачальника сервісу, наприклад 10000004, код ЄДРПОУ організації, яка надає сервіс.
	•	SERVICE_CODE – Код сервісу в системі Трембіта. Наприклад, node_js_service – це код конкретного сервісу, який опублікований організацією-постачальником.
	•	SERVICE_SUBSYSTEM – Код підсистеми організації-постачальника сервісу в системі Трембіта. Наприклад, SUB_SERVICE.
    •	SERVICE_VERSION - Версія сервісу організації-постачальника сервісу в системі Трембіта.
	•	LOG_LEVEL – Рівень деталізації повідомлень у логах. Можливі значення: DEBUG (найдетальніший), INFO, WARNING, ERROR, CRITICAL. Значення DEBUG виводить максимальну кількість інформації для налагодження програми.
 	•	LOG_DIRECTORY - Вказує шлюх де слід зберігати лог-файл
 	•	ERROR_LOG_FILE_NAME -  Назва файлу для збереження логів помилок. Наприклад error.log
 	•	COMBINED_LOG_FILE_NAME - Назва файлу для збереження об'єднаних логів. Наприклад combined.log 
 	•	ASIC_DIRECTORY -  Директорія для збереження asic-файлів. Наприклад, ./asic .

Приклад заповнення змінних оточення:

```env
- `PORT`=3001;
- `PROTOCOL`=http;
- `PURPOSE_ID`=5cbc1e1b-be44-4679-9214-3d4995b35d3a;
- `SECURITY_SERVER_IP`=10.0.20.235;
- `INSTANCE_NAME`=SEVDEIR-TEST;
- `CLIENT_MEMBER_CLASS`=GOV;
- `CLIENT_MEMBER_CODE`=10000004;
- `CLIENT_SUBSYSTEM`=SUB_CLIENT;
- `SERVICE_MEMBER_CLASS`=GOV;
- `SERVICE_MEMBER_CODE`=10000004;
- `SERVICE_CODE`=py_sync;
- `SERVICE_SUBSYSTEM`=SUB_SERVICE; 
- `SERVICE_VERSION`=;
- `LOG_LEVEL`=DEBUG;
- `ASIC_DIRECTORY`=./asic
```

## Збір Docker-образу

Для того, щоб зібрати Docker-образ, необхідно:

1. Клонувати репозиторій

```bash
git clone https://github.com/kshypachov/rest-sync-client-next-js-page-router.git
```

2.	Перейти до директорії з вебклієнтом:

```bash
cd rest-sync-client-next-js-page-router
```

3. Виконати наступну команду в кореневій директорії проєкту:

```bash
sudo docker build -t node-app-rest .
```

Дана команда створить Docker-образ з іменем `node-app-rest`, використовуючи Dockerfile, який знаходиться в поточній директорії.

## Запуск та використання контейнера зі змінними оточення

Щоб запустити контейнер з додатком, необхідно виконати команду:

```bash
docker run -it --rm -p 3001:3001 \
    -e PORT=3001 \
    -e PROTOCOL=http \
    -e PURPOSE_ID=5cbc1e1b-be44-4679-9214-3d4995b35d3a \
    -e SECURITY_SERVER_IP=10.0.20.235 \
    -e INSTANCE_NAME=SEVDEIR-TEST \
    -e CLIENT_MEMBER_CLASS=GOV \
    -e CLIENT_MEMBER_CODE=10000004 \
    -e CLIENT_SUBSYSTEM=SUB_CLIENT \
    -e SERVICE_MEMBER_CLASS=GOV \
    -e SERVICE_MEMBER_CODE=10000004 \
    -e SERVICE_CODE=py_sync \
    -e SERVICE_SUBSYSTEM=SUB_SERVICE \
    -e SERVICE_VERSION= \
    -e LOG_LEVEL=DEBUG \
    -e ASIC_DIRECTORY=./asic \
    node-app-rest
```

де:
- параметр `-p 3001:3001` перенаправляє порт 3001 на локальній машині на порт 3001 всередині контейнера.
- інші змінні перелічені в пункті [Змінні оточення](#змінні-оточення)

## Перегляд журналів подій

Якщо виведення журналів подій налаштоване у консоль, переглядати їх можна за допомогою команди:

```bash
docker logs <container_id>
```
