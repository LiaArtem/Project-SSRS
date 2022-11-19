- Project SSRS VS2022 (Reports with Oracle, MS SQL, Azure SQL, PostgreSQL, MySQL, MariaDB, IBM DB2, IBM Informix, Firebird, SQLite, XML).
Deployment MS SQL Server 2022 Reporting Services.

----------------------------------------------------------------------------
Настройка Microsoft SQL Server 2022 Reporting Services
----------------------------------------------------------------------------
1) Установили Microsoft SQL Server 2022 Reporting Services
   https://www.microsoft.com/ru-RU/download/details.aspx?id=104502 - SQLServerReportingServices.exe
2) Запустили Report Server Configuration Manager
   - Соединились
   - Учетная запись службы -> выбираем Сетевая служба и жмем применить
   - URL-адрес веб-службы -> Виртуальный каталог - ReportServer2022 и жмем применить. (URL - http://localhost:80/ReportServer2022)
   - База данных -> Изменить базу данных -> Создать новую базу данных отчетов
     - Тип проверки подлинности: - Учетная запись SQL Server
     - Имя пользователя: - sa
     - Пароль: - !Aa112233
   - База данных -> Изменить учетные данные
     - Сервер базы данных:
       - Тип проверки подлинности: - Учетная запись SQL Server
       - Имя пользователя: - sa
       - Пароль: - !Aa112233
     - Учетные данные:
       - Тип проверки подлинности: - Учетная запись SQL Server
       - Имя пользователя: - sa
       - Пароль: - !Aa112233
   - URL-адрес веб-портала -> Виртуальный каталог - Reports2022 и жмем применить. (URL - http://localhost:80/Reports2022)
   - Настройки электронной почты - настраиваем если нужно.
   - Выходим

 3) Добавляем уже созданные отчеты (Google Chrome - http://localhost:80/Reports2022)
   - Устанавливаем ReportBuilder (если хотим менять в отчетах в веб-версии)
     https://www.microsoft.com/ru-ru/download/details.aspx?id=53613 - устанавливаем - ReportBuilder.msi
   - взять готовые отчеты из проекта - Project SSRS
     - в проекте VS прописать Свойства проекта
       -> TargetServerUrl = http://localhost:80/ReportServer2022
       -> OverwriteDatasets = true
       -> OverwriteDatasources = true
     - Пуск проекта Project SSRS. Все автоматически уйдет на сервер.

  4) Базу SQLite перенести - C:\\DB_SQLite\\CurrencyChartFXMaven.db
  5) Для работы сервера необходимы ODBC Drivers x64
  6) Перезагружаем сервер (ПК)

----------------------------------------------------------------------------
Настройка HTTPS в Microsoft SQL Server 2022 Reporting Services
----------------------------------------------------------------------------
1) Добавляем IIS Management Console (Диспетчер служб IIS) если ее нет.
   Клавиша Windows -> пишем "Включение и отключение компонентов Windows" -> Службы IIS -> Средства управления веб-сайтом (все включить)
   + -> Службы IIS -> Службы Интернета -> Общие функции HTTP -> Документ по умолчанию, Ошибки HTTP, Просмотр каталога, Статическое содержимое (все включить)
   ОК.
2) Запускаем Диспетчер служб IIS
   2.1 Если нет Локального центра сертификации
   Панель управления -> Администирование -> Диспетчер служб IIS
   - Сертификаты сервера -> правой клавишей мышки -> Создание самозаверенного сертификата
     - Понятное имя сертификата - desktop-kq2l9b1 (имя ПК)
     - Хранилище - Личный
   2.2 Если есть Локальный центр сертификации
   Панель управления -> Администирование -> Диспетчер служб IIS
   - Сертификаты сервера -> правой клавишей мышки -> Создать сертификат домена
     - Полное имя - desktop-kq2l9b1 (имя ПК)
     - Организация, подраздение (например - test)
     - Город, область - (например - Kyiv)
     - Страна - (например - UA)
   - Далее - Локальный центр сертификации и вводим понятное имя.
3) Запускаем Report Server Configuration Manager
   - URL адрес веб-службы - выбираем HTTPS-сертификат (https://desktop-kq2l9b1/ReportServer2022)
     Применить.
   - URL адрес веб-портала - Долполнительно - Насколько HTTPS удостоверений для выбранного сейчас компонента Reporting Services
     Добавить, выбрать сертификат - ОК - ОК (https://desktop-kq2l9b1/Reports2022)

----------------------------------------------------------------------------
Visual Studio 2022 - настройки ODBC при разработке
----------------------------------------------------------------------------
Если вы работаете в Visual Studio и не видите служб Reporting Services в проектах необходимо
 - добавить компонент <Хранение и обработка данных> -> включить ВСЕ или SQL Server Data Tools
Если не появился +
 - из меню Visual Studio <Расширения> выберите <Управление расширениями> установить Microsoft Reporting Services Projects

!!!! Если такая ошибка в DataSource_*** вкладка Учетные данные -> Использовать имя пользователя и пароль -> Прописать их
The current action cannot be completed. The user data source credentials do not meet the requirements to run this report or shared dataset.
Either the user data source credentials are not stored in the report server database, or the user data source is configured not to require credentials
but the unattended execution account is not specified. (rsInvalidDataSourceCredentialSetting)

-> XML read
------------------------------------------------------
- https://bank.gov.ua/depo_securities
- Пример построения https://www.mssqltips.com/sqlservertip/3148/sql-server-reporting-services-xml-data-source-and-data-set/

-> SQLite ODBC Driver
------------------------------------------------------
- http://www.ch-werner.de/sqliteodbc/
- скачиваем и ставим sqliteodbc.exe, sqliteodbc_w64.exe
- Запускаем -> Источники данных ODBC x64 -> Системный DSN ->  SQLite Datasource x64 -> Прописать Database Name = C:\DB_SQLite\CurrencyChartFXMaven.db
- ODBC прописать при подключении в SSRS
- Driver={SQLite3 ODBC Driver};database=C:\\DB_SQLite\\CurrencyChartFXMaven.db;longnames=0;timeout=1000;notxn=0;syncpragma=NORMAL;stepapi=0

-> PostgreSQL ODBC Driver
------------------------------------------------------
- https://www.postgresql.org/ftp/odbc/versions/msi/
- скачиваем и ставим psqlodbc_13_02_0000-x64 или более новую
- ODBC прописать при подключении в SSRS
- Driver={PostgreSQL ANSI};Server=localhost;Port=5432;Database=testdb;Uid=testdb;Pwd=!Aa112233;

-> MySQL ODBC Driver
------------------------------------------------------
- https://dev.mysql.com/downloads/connector/odbc/
- скачиваем и ставим mysql-connector-odbc-8.0.28-winx64.msi или более новую (если не установнено вместе с базой данных)
- ODBC прописать при подключении в SSRS
- Driver={MySQL ODBC 8.0 ANSI Driver};Server=localhost;User=test_user;Password=!Aa112233;Option=3;

-> MariaDB ODBC Driver
------------------------------------------------------
- https://mariadb.com/downloads/connectors/connectors-data-access/odbc-connector
- скачиваем и ставим mariadb-connector-odbc-3.1.17-win64.msi или более новую (если не установнено вместе с базой данных)
- ODBC прописать при подключении в SSRS
- DRIVER={MariaDB ODBC 3.1 Driver}; SERVER=localhost; PORT=3307; UID=root; PASSWORD=!Aa112233;OPTION=3;

-> Oracle Driver
------------------------------------------------------
  *** ODBC
  - инструкция - https://www.oracle.com/cis/database/technologies/releasenote-odbc-ic.html
  - https://www.oracle.com/database/technologies/instant-client/winx64-64-downloads.html

  - скачиваем Oracle Instant Client Basic Package - instantclient-basic-windows.x64-21.7.0.0.0dbru.zip
  - распаковываем с папку c:\oracle\product, если их нет создаем
  - добавляем в Переменные среды -> Cистемные переменные -> Path = c:\oracle\product\instantclient_21_7\
  - скачиваем SQL*Plus Package - instantclient-sqlplus-windows.x64-21.7.0.0.0dbru.zip
  - распаковываем с папку c:\oracle\product\
  - копируем файлы tnsnames.ora и sqlnet.ora в папку c:\oracle\product\instantclient_21_7\network\admin\
  - проверяем cmd:
    - sqlplus /nolog
    - connect TEST_USER/!Aa112233@XE
    - exit
  - скачиваем ODBC Package - instantclient-odbc-windows.x64-21.7.0.0.0dbru.zip
  - распаковываем с папку c:\oracle\product\
  - запускаем cmd под администратором
    - cd c:\oracle\product\instantclient_21_7\
    - odbc_install
  - запускаем -> Источники данных ODBC (64-разрядная версия)
  - проверяем -> Драйверы, должен появится -> Oracle in instantclient_21_7
  - выбираем -> Системный DSN -> Добавить:
    - Data Source Name: Oracle_x64
    - Description: Oracle_x64
    - TNS Service Name: XE
    - User ID: TEST_USER
    - OK
  - настраиваем ODBC подключение в SSRS
  - поставщик ODBC: System data source name = Oracle_x64
  - логин = TEST_USER и пароль = !Aa112233.

  *** OLE DB (альтернатива)
  - https://www.oracle.com/database/technologies/oracle21c-windows-downloads.html
  - скачиваем и ставим Oracle клиента - NT_213000_client_x64.zip или более новый
  - перегружаем ПК
  - копируем tnsnames.ora и sqlnet.ora в папку c:\app\client\Admin\product\21.0.0\client_1\network\admin\
  - после установки меняем в глоб. реестре:
    - Компьютер\HKEY_LOCAL_MACHINE\SOFTWARE\ORACLE\KEY_OraClient21Home1 c AMERICAN_AMERICA.WE8MSWIN1252
      на NLS_LANG = AMERICAN_AMERICA.AL32UTF8 (либо AMERICAN_AMERICA.CL8MSWIN1251)
  - настраиваем OLE DB подключение в SSRS
  - поставщик OLE DB: Oracle provider for OLE DB
  - имя базы = XE, логин = TEST_USER и пароль = !Aa112233.

-> IBM DB2 Driver
------------------------------------------------------
- *** OLE DB устанавливается вместе с базой данных (если локальная база данных)
- настраиваем OLE DB подключение в SSRS
  - поставщик OLE DB: Provider=IBMOLEDB.DB2COPY1;Data Source=SAMPLE;Location=localhost:25000
  - логин = db2admin и пароль = !Aa112233.

- *** ODBC (если база данных находится на отдельном сервере)
- настраиваем ODBC подключение в SSRS
  - https://www.ibm.com/support/pages/node/6830623 поиск IBM Data Server Driver for ODBC and CLI (64-bit) -> Download
    - IBM Data Server Driver for ODBC and CLI (Windows/x86-64 64 bit) V11.5.8 Fix Pack 0
  - скачать IBM Data Server Driver for ODBC and CLI (64-bit) -> v11.5.8_ntx64_odbc_cli.zip
  - распаковать содержимое в c:\Program Files\IBM\ предварително создав папку IBM
  - добавляем в Переменные среды -> Cистемные переменные -> Path = c:\Program Files\IBM\clidriver\bin
  - запускаем под администатором: cmd db2oreg1 –i
  - запускаем под администатором: cmd db2oreg1 –setup
  - запускаем -> Источники данных ODBC (64-разрядная версия)
  - проверяем -> Драйверы, должен появится -> IBM DATA SERVER DRIVER for ODBC.....
  - выбираем -> Системный DSN -> Добавить:
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
  - настраиваем ODBC подключение в SSRS
  - поставщик ODBC: System data source name = ibmdb2_odbc
  - логин = DB2INST1 и пароль = !Aa112233.

-> IBM Informix ODBC Driver
------------------------------------------------------
- Скачиваем и устанавливаем IBM Informix Client SDKV 4.50 (ibm.csdk.4.50.FC8.WIN.zip)
  - Если база данных не локальная необходимо настроить SSL подключение:
    - включить при установке - Use OPENSSL instead of GSKit?
    - Windows -> Источники данных ODBC (64 разрадная версия) -> Системный DSN
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
- Скачиваем и устанавливаем Firebird_ODBC_2.0.5.156_x64.exe
- Скачиваем и устанавливаем Firebird-4.0.2.2816-0-x64.exe (!!! Выборочная установка -> Компоненты сервера (выключить))
- настраиваем ODBC подключение в SSRS
  - поставщик ODBC: DRIVER=Firebird/InterBase(r) driver;UID=SYSDBA;PWD=!Aa112233;DBNAME=localhost/3050://firebird/data/testdb.fdb


