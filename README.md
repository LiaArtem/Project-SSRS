- Project SSRS VS2019 (Reports with MS SQL 2019, OracleXE 21c, PostgreSQL 14, SQLite, MySQL, XML).
Deployment MS SQL Server 2019 Reporting Services.

----------------------------------------------------------------------------
Настройка Microsoft SQL Server 2019 Reporting Services
----------------------------------------------------------------------------
1) Установили Microsoft SQL Server 2019 Reporting Services
   https://www.microsoft.com/ru-RU/download/confirmation.aspx?id=100122 - SQLServerReportingServices.exe
2) Запустили Report Server Configuration Manager
   - Соединились
   - Учетная запись службы -> выбираем Сетевая служба и жмем применить
   - URL-адрес веб-службы -> Виртуальный каталог - ReportServer2019 и жмем применить. (URL - http://localhost:80/ReportServer2019)
   - База данных -> Изменить базу данных -> Создать новую базу данных отчетов
   - URL-адрес веб-портала -> Виртуальный каталог - Reports2019 и жмем применить. (URL - http://localhost:80/Reports2019)
   - Настройки электронной почты - настраиваем если нужно.
   - Выходим

 3) Добавляем уже созданные отчеты (Google Chrome - http://localhost:80/Reports2019)
   - ReportBuilder
     https://www.microsoft.com/ru-ru/download/details.aspx?id=53613устанавливыаем - ReportBuilder.msi
   - взять готовые отчеты из проекта - Project SSRS
     - в проекте VS прописать Свойства проекта
       -> TargetServerUrl = http://localhost:80/ReportServer2019
       -> OverwriteDatasets = true
       -> OverwriteDatasources = true
     - Пуск проекта Project SSRS. Все автоматически уйдет на сервер.

  4) Базу SQLite перенести - C:\\DB_SQLite\\CurrencyChartFXMaven.db
  5) Для работы сервера необходимы ODBC Drivers x64
     - sqlite - http://www.ch-werner.de/sqliteodbc/  - файл sqliteodbc_w64.exe
     - postgeeSQL - https://www.postgresql.org/ftp/odbc/versions/msi/ - файл psqlodbc_13_02_0000-x64.zip
     - oracle - - https://www.oracle.com/database/technologies/oracle21c-windows-downloads.html - файл NT_213000_client_x64.zip
        - после установки меняем в глоб. реестре:
        - Компьютер\HKEY_LOCAL_MACHINE\SOFTWARE\ORACLE\KEY_OraClient21Home1 c AMERICAN_AMERICA.WE8MSWIN1252
          на NLS_LANG = AMERICAN_AMERICA.AL32UTF8 (либо AMERICAN_AMERICA.CL8MSWIN1251)
        - настраиваем tnsnames.ora для настройки x86 Oracle Client:
          XE = (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = localhost)(PORT = 1521)))(CONNECT_DATA =(SERVICE_NAME = XE)))

----------------------------------------------------------------------------
Visual Studio 2019 - SSRS работает с x86 ODBC Drivers и x86 Oracle Client
----------------------------------------------------------------------------

-> XML read
------------------------------------------------------
- https://bank.gov.ua/depo_securities
- Пример построения https://www.mssqltips.com/sqlservertip/3148/sql-server-reporting-services-xml-data-source-and-data-set/

-> SQLite ODBC Driver
------------------------------------------------------
- http://www.ch-werner.de/sqliteodbc/
- скачиваем и ставим sqliteodbc.exe
- ODBC прописать при подключении в SSRS
- Driver={SQLite3 ODBC Driver};database=C:\\DB_SQLite\\CurrencyChartFXMaven.db;longnames=0;timeout=1000;notxn=0;syncpragma=NORMAL;stepapi=0

-> MySQL ODBC Driver
------------------------------------------------------
- https://dev.mysql.com/downloads/connector/odbc/
- скачиваем и ставим mysql-connector-odbc-8.0.27-win32.msi или более новую
- ODBC прописать при подключении в SSRS
- Driver={MySQL ODBC 8.0 ANSI Driver};Server=localhost;User=test_user;Password=12345678;Option=3;

-> PostgreSQL ODBC Driver
------------------------------------------------------
- https://www.postgresql.org/ftp/odbc/versions/msi/
- скачиваем и ставим psqlodbc_13_02_0000-x86.zip или более новую
- ODBC прописать при подключении в SSRS
- Driver={PostgreSQL ANSI};Server=localhost;Port=5432;Database=test_database;Uid=test_user;Pwd=12345678;

-> Oracle OLE DB Driver
------------------------------------------------------
- https://www.oracle.com/database/technologies/oracle21c-windows-downloads.html
- скачиваем и ставим Oracle клиента x86 - NT_213000_client.zip или более новый
- после установки меняем в глоб. реестре:
  - Компьютер\HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\ORACLE\KEY_OraClient19Home1_32bit c AMERICAN_AMERICA.WE8MSWIN1252
    на NLS_LANG = AMERICAN_AMERICA.AL32UTF8 (либо AMERICAN_AMERICA.CL8MSWIN1251)
- настраиваем tnsnames.ora для настройки x86 Oracle Client:
    XE = (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = localhost)(PORT = 1521)))(CONNECT_DATA =(SERVICE_NAME = XE)))
- настраиваем OLE DB подключение в SSRS
  - поставщик OLE DB: Oracle provider for OLE DB
  - имя базы = XE, логин = TEST_USER и пароль = TEST_USER.