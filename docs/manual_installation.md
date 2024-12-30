# Посібник з встановлення клієнту вручну

## Опис
Цей посібник допоможе пройти процесс встановлення клієнту вручну. Для почтаку потрібно мати чисту систему Ubuntu,
всі необхідні пакети та репозиторії будуть підключені пізніше.

## Загальні вимоги

- Node.js v20.15.1+
- npm v10.7.0+
- Git (для клонування репозиторію)
- Ubuntu Server 24.04

## Встановлення клієнту
### 1. Встановлення залежностей
Для початку потрібно встановити всі необхідні пакети. Виконайте наступні команди для встановлення NodeJs і супутніх інструментів:

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo bash - 
sudo apt-get update
sudo apt-get install -y curl gcc nodejs git
```

### 2. Клонування репозиторію
1. Клонуйте репозиторій проєкту:
```bash
git clone https://github.com/kshypachov/rest-sync-client-next-js-page-router.git
```

2.	Перейдіть до директорії проєкту:
```bash
cd rest-sync-client-next-js-page-router
```

3. Встановіть залежності Node.js
```bash
npm install
```

4. Перейменуйте файл .env.example в .env.local та заповніть відповідні параметри.

```bash
# Налаштування Next.js
PORT='3001'

# Налаштування вихідних запитів
# Протокол, який використовується для взаємодії з ШБО (https або http)
# Протокол https вимагає взаємної автентифікації клієнта з ШБО з використанням сертифікатів
PROTOCOL=https
# Хостнейм, FQDN або локальна IP-адреса ШБО
# можливі варіанти: 192.168.1.1, trembita.example.gov.ua
SECURITY_SERVER_IP=your_trembita_host # TREMBITA URL

PURPOSE_ID='5cbc1e1b-be44-4679-9214-3d4995b35d3a' # Ідентифікатор мети обробки персональних даних для взаємодії з сервісами, що використовують Підсистему моніторингу доступу до персональних даних системи Трембіта (ПМДПД). Може бути не заданим (закоментувати параметр), якщо обмін відбувається без використання цього ПМДПД.
INSTANCE_NAME='SEVDEIR-TEST' # Повний ідентифікатор клієнтської підсистеми Трембіти, що використовується для надсилання повідомлень-запитів
# xRoadInstance (SEVDEIR чи SEVDEIR-TEST)

# Налаштування Uxp-Client 
CLIENT_MEMBER_CLASS='GOV'
CLIENT_MEMBER_CODE='your_client_member_code'   # код ЄДРПОУ організації-клієнта
CLIENT_SUBSYSTEM=' your_client_subsystem_code'   # код підсистеми ШБО організації-клієнта, що буде використовуватись для запитів
IF_CLIENT_SAVE_ASIC='true'

# Налаштування Uxp-Service 
SERVICE_MEMBER_CLASS='GOV'
SERVICE_MEMBER_CODE='your_service_member_code'
SERVICE_SUBSYSTEM='your_service_subsystem_code'
SERVICE_CODE='your_service_code'
SERVICE_VERSION='your_service_version'

# Налаштування логування
LOG_LEVEL='debug' # Рівень логування (info, debug, error і т.д.)
LOG_DIRECTORY='./logs' # Директорія для збереження лог-файлів
ERROR_LOG_FILE_NAME='error.log' # Назва файлу для збереження логів помилок
COMBINED_LOG_FILE_NAME='combined.log' # Назва файлу для збереження об'єднаних логів
```

5. У разі налаштування взаємодіЇ з ШБО через протокол HTTPS перед першим запуском необхідно згенерувати самопідписний сертифікат за допомогою команди

  ```bash
  npm run gen-srt
  ```

6. Запустіть проєкт за допомогою команд

  ```bash
  npm run build
  ```

  ```bash
  npm start
  ```

7.Відкрийте в браузері посилання

  ```bash
  http://<your_server_ip>:3001
  ```

8.В разі налаштування взаємодії з ШБО через протокол HTTPS необхідно на головній сторінці веб додатка завантажити згенерований клієнтом сертифікат безпеки та передати його адміністратору ШБО для реєстрації клієнту. Водночас адміністратор ШБО повинен передати згенерований ШБО сертифікат безпеки, який необхідно помістити в директорію клієнту certs/service-cert.pem
