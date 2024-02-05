- Project SSRS VS2022 (Reports with Oracle, MS SQL, Azure SQL, PostgreSQL, MySQL, MariaDB, IBM DB2, IBM Informix, Firebird, SQLite, XML).
Deployment MS SQL Server 2022 Reporting Services.

1) Встановлюємо SQL Server та інші бази даних з проекту - Docker-Win11
   https://github.com/LiaArtem/Docker-Win11/
   https://github.com/LiaArtem/Oracle_23c_Free/

2) Заповнюємо бази данних даними запускаючи проект - CurrencyChartFX-Java-21-Maven
   https://github.com/LiaArtem/CurrencyChartFX-Java-21-Maven
   +
   https://github.com/LiaArtem/ASP_RESTful_Web_API (для Report_Fair_mssql)
   - POST /api/FairValue за необхідну дату
   - POST /api/IsinSecur

----------------------------------------------------------------------------
Налаштування Microsoft SQL Server 2022 Reporting Services
----------------------------------------------------------------------------
1) Встановлюємо Microsoft SQL Server 2022 Reporting Services
   https://www.microsoft.com/ua-RU/download/details.aspx?id=104502 - SQLServerReportingServices.exe
2) Запускаємо Report Server Configuration Manager
   - З'єдналися
   - Обліковий запис служби -> вибираємо Мережева служба і тиснемо застосувати
   - URL-адреса веб-служби -> Віртуальний каталог - ReportServer2022 і тиснемо застосувати. (URL - http://localhost:80/ReportServer2022)
   - База даних -> Змінити базу даних -> Створити нову базу даних звітів
     - Тип автентифікації: - Обліковий запис SQL Server
     - Ім'я користувача: - sa
     - Пароль: - !Aa112233
   - База даних -> Змінити облікові дані
   - сервер бази даних:
       - Тип автентифікації: - Обліковий запис SQL Server
       - Ім'я користувача: - sa
       - Пароль: - !Aa112233
     - Облікові дані:
       - Тип автентифікації: - Обліковий запис SQL Server
       - Ім'я користувача: - sa
       - Пароль: - !Aa112233
   - URL-адреса веб-порталу -> Віртуальний каталог - Reports2022 і тиснемо застосувати. (URL - http://localhost:80/Reports2022)
   - Налаштування електронної пошти – налаштовуємо якщо потрібно.
   - Вихід

3) Додаємо вже створені звіти (Google Chrome - http://localhost:80/Reports2022)
   - Встановлюємо ReportBuilder (якщо хочемо змінювати звіти у веб-версії)
     https://www.microsoft.com/ua-ru/download/details.aspx?id=53613 - встановлюємо - ReportBuilder.msi
   - взяти готові звіти з проекту - Project SSRS
     - у проекті VS прописати Властивості проекту
       -> TargetServerUrl = http://localhost:80/ReportServer2022
       -> OverwriteDatasets = true
       -> OverwriteDatasources = true
     - Пуск проекту Project SSRS. Все автоматично піде на сервер.

4) Для роботи сервера необхідні ODBC Drivers x64
5) Перезавантажуємо сервер (ПК)

----------------------------------------------------------------------------
Налаштування HTTPS у Microsoft SQL Server 2022 Reporting Services
----------------------------------------------------------------------------
1) Додаємо IIS Management Console (Диспетчер служб IIS) якщо її немає.
   Клавіша Windows -> пишемо "Увімкнення та вимкнення компонентів Windows" -> Служби IIS -> Засоби керування веб-сайтом (все включити)
   + -> Служби IIS -> Служби Інтернету -> Загальні функції HTTP -> Документ за замовчуванням, Помилки HTTP, Перегляд каталогу, Статичний вміст (увімкнути)
   ОК.
2) Запускаємо Диспетчер служб IIS
   2.1 Якщо немає Локального центру сертифікації
   Панель керування -> Адміністрування -> Диспетчер служб IIS
   - Сертифікати сервера -> правою клавішею мишки -> Створення самозавіреного сертифіката
     - Зрозуміле ім'я сертифіката – desktop-kq2l9b1 (ім'я ПК)
     - Сховище - Особистий
   2.2 Якщо є Локальний центр сертифікації
   Панель керування -> Адміністрування -> Диспетчер служб IIS
   - Сертифікати сервера -> правою клавішею мишки -> Створити сертифікат домену
     - Повне ім'я – desktop-kq2l9b1 (ім'я ПК)
     - Організація, підрозділ (наприклад - test)
     - Місто, область - (наприклад - Kyiv)
     - Країна - (наприклад - UA)
   - Далі – Локальний центр сертифікації та вводимо зрозуміле ім'я.
3) Запускаємо Report Server Configuration Manager
   - URL-адреса веб-служби - вибираємо HTTPS-сертифікат (https://desktop-kq2l9b1/ReportServer2022)
     Застосувати.
   - URL-адреса веб-порталу - Додатково - Наскільки HTTPS посвідчень для обраного зараз компонента Reporting Services
     Додати, вибрати сертифікат - ОК - ОК (https://desktop-kq2l9b1/Reports2022)

----------------------------------------------------------------------------
Visual Studio 2022 - налаштування ODBC
----------------------------------------------------------------------------

-> XML read
------------------------------------------------------
- https://bank.gov.ua/depo_securities
- Приклад побудови https://www.mssqltips.com/sqlservertip/3148/sql-server-reporting-services-xml-data-source-and-data-set/

-> SQLite ODBC Driver
------------------------------------------------------
- Базу SQLite перенести - C:\DB_SQLite\CurrencyChartFXMaven.db
- http://www.ch-werner.de/sqliteodbc/
- завантажуємо та ставимо sqliteodbc.exe, sqliteodbc_w64.exe
- Запускаємо -> Джерела даних ODBC x64 -> Системний DSN -> SQLite Datasource x64 -> Прописати Database Name = C:\DB_SQLite\CurrencyChartFXMaven.db
- ODBC прописати при підключенні до SSRS
- Driver={SQLite3 ODBC Driver};database=C:\\DB_SQLite\\CurrencyChartFXMaven.db;longnames=0;timeout=1000;notxn=0;syncpragma=NORMAL;stepapi=0

-> PostgreSQL ODBC Driver
------------------------------------------------------
- https://www.postgresql.org/ftp/odbc/versions/msi/
- завантажуємо та ставимо psqlodbc_16_00_0000-x64.zip або новішу
- ODBC прописати при підключенні до SSRS
- Driver={PostgreSQL ANSI};Server=localhost;Port=5432;Database=testdb;Uid=testdb;Pwd=!Aa112233;

-> MySQL ODBC Driver
------------------------------------------------------
- https://dev.mysql.com/downloads/connector/odbc/
- завантажуємо та ставимо mysql-connector-odbc-8.3.0-winx64.msi або нову (якщо не встановлено разом із базою даних)
- ODBC прописати при підключенні до SSRS
- Driver={MySQL ODBC 8.3 ANSI Driver};Server=localhost;User=test_user;Password=!Aa112233;Option=3;

-> MariaDB ODBC Driver
------------------------------------------------------
- https://mariadb.com/downloads/connectors/connectors-data-access/odbc-connector/
- завантажуємо та ставимо mariadb-connector-odbc-3.1.20-win64.msi або новішу (якщо не встановлено разом з базою даних)
- ODBC прописати при підключенні до SSRS
- DRIVER={MariaDB ODBC 3.1 Driver}; SERVER=localhost; PORT=3307; UID=root; PASSWORD=!Aa112233;OPTION=3;

-> Oracle Driver
------------------------------------------------------
  *** ODBC
  - інструкція - https://www.oracle.com/cis/database/technologies/releasenote-odbc-ic.html
  - https://www.oracle.com/database/technologies/instant-client/winx64-64-downloads.html

  - завантажуємо Oracle Instant Client Basic Package - instantclient-basic-windows.x64-21.13.0.0.0dbru.zip
  - Розпаковуємо з папку c:\oracle\product, якщо їх немає
    - !!!!! запускаємо cmd під адміністратором
    - cmd -> sysdm.cpl -> Додатково -> Змінні оточення
    - додаємо в Змінні середовища -> Системні змінні -> Path = c:\oracle\product\instantclient_21_13\
  - завантажуємо SQL*Plus Package - instantclient-sqlplus-windows.x64-21.13.0.0.0dbru.zip
  - Розпаковуємо з папку c:\oracle\product\
  - Копіюємо файли tnsnames.ora і sqlnet.ora в папку c:\oracle\product\instantclient_21_13\network\admin\
    - Перевіряємо cmd:
      - sqlplus /nolog
      - connect TEST_USER/!Aa112233@FREE
      - exit
  - завантажуємо ODBC Package - instantclient-odbc-windows.x64-21.13.0.0.0dbru.zip
  - Розпаковуємо з папку c:\oracle\product\
    - !!!!! запускаємо cmd під адміністратором
      - cd c:\oracle\product\instantclient_21_13\
      - odbc_install
  - налаштовуємо ODBC підключення до SSRS
  - ODBC прописати при підключенні до SSRS
  - DRIVER={Oracle in instantclient_21_13};DBQ=localhost:1521/FREE;UID=TEST_USER;PWD=!Aa112233;TLO=O;FBS=60000;FWC=F;CSR=F;MDI=T;MTS=F;DPM=F;NUM=NLS;BAM=IfAllSuccessful;BTD=F;RST=T;LOB=T;FDL=0;FRC=0;QTO=T;FEN=F;XSM=Default;EXC=F;APA=T;DBA=W;

  *** OLE DB (альтернатива)
  - https://www.oracle.com/database/technologies/oracle21c-windows-downloads.html
  - завантажуємо та ставимо Oracle клієнта - NT_213000_client_x64.zip або новіший
  - перевантажуємо ПК
  - Копіюємо tnsnames.ora і sqlnet.ora в папку c:\app\client\Admin\product\21.0.0\client_1\network\admin\
  - після встановлення міняємо у глоб. реєстрі:
    - Комп'ютер\HKEY_LOCAL_MACHINE\SOFTWARE\ORACLE\KEY_OraClient21Home1 з AMERICAN_AMERICA.WE8MSWIN1252
      на NLS_LANG = AMERICAN_AMERICA.AL32UTF8 (або AMERICAN_AMERICA.CL8MSWIN1251)
  - налаштовуємо OLE DB підключення до SSRS
  - Постачальник OLE DB: Oracle provider for OLE DB
  - ім'я бази = XE, логін = TEST_USER та пароль = !Aa112233.

-> IBM DB2 Driver
------------------------------------------------------
- *** OLE DB встановлюється разом із базою даних (якщо локальна база даних)
- налаштовуємо OLE DB підключення до SSRS
  - постачальник OLE DB: Provider=IBMOLEDB.DB2COPY1;Data Source=SAMPLE;Location=localhost:25000
  - логін = db2admin та пароль = !Aa112233.

- *** ODBC (якщо база даних знаходиться на окремому сервері)
- налаштовуємо ODBC підключення до SSRS
  - https://www.ibm.com/support/pages/node/6830623 пошук IBM Data Server Driver for ODBC and CLI (64-bit) -> Download
    - IBM Data Server Driver for ODBC and CLI (Windows/x86-64 64-bit) V11.5.9 Fix Pack 0
  - скачати IBM Data Server Driver for ODBC and CLI (64-bit) -> v11.5.9_ntx64_odbc_cli.zip
  - розпакувати вміст у c:\Program Files\IBM\ попередньо створивши папку IBM
    - !!!!! запускаємо cmd під адміністратором
    - cmd -> sysdm.cpl -> Додатково -> Змінні оточення
    - додаємо в Змінні середовища -> Системні змінні -> Path = c:\Program Files\IBM\clidriver\bin
    - cmd -> cd c:\Program Files\IBM\clidriver\bin
    - запускаємо під адміністатором: cmd -> db2oreg1 -i
    - запускаємо під адміністатором: cmd -> db2oreg1 -setup
  - запускаємо -> Джерела даних ODBC (64-розрядна версія)
  - перевіряємо -> Драйвери, повинен з'явиться -> IBM DATA SERVER DRIVER for ODBC.
  - вибираємо -> Системний DSN -> Додати:
    - Data Source Name: ibmdb2_odbc
    - Description: ibmdb2_odbc
    - Database alias: -> Add
      - Data Source:
        - User ID: DB2INST1
        - Password: !Aa112233
        - check Save password
      - TCP/IP:
        - Database name: sample
        - Host name: localhost
        - Port: 50000
      - OK
  - налаштовуємо ODBC підключення до SSRS
  - постачальник ODBC: System data source name = ibmdb2_odbc
  - логін = DB2INST1 та пароль = !Aa112233.

-> IBM Informix ODBC Driver (працює тільки з локальною базою даних)
------------------------------------------------------
- Завантажуємо та встановлюємо IBM Informix Client SDKV 4.50 (ibm.csdk.4.50.FC8.WIN.zip)
  - Якщо база даних не локальна, необхідно налаштувати SSL підключення:
    - увімкнути під час встановлення - Use OPENSSL instead of GSKit?
    - Windows -> Джерела даних ODBC (64 розрядна версія) -> Системний DSN
      - General:
        - Data Source Name - informix_odbc
        - Description - informix_odbc
      - Connection:
        - Server Name - informix_test
        - Host Name - localhost
        - Service - turbo_test
        - Database Name - sample
        - User Id - informix
        - Password - !Aa112233

-> Firebird ODBC Driver
------------------------------------------------------
- Завантажуємо та встановлюємо Firebird_ODBC_2.0.5.156_x64.exe
- Завантажуємо та встановлюємо Firebird-5.0.0.1306-0-windows-x64.exe (!!! Вибіркова установка -> Компоненти сервера (вимкнути))
- налаштовуємо ODBC підключення до SSRS
  - постачальник ODBC: DRIVER=Firebird/InterBase(r) driver;UID=SYSDBA;PWD=!Aa112233;DBNAME=localhost/3050://firebird/data/testdb.fdb


