- Project SSRS VS2022 (Reports with MS SQL 2019, OracleXE 21c, PostgreSQL 14, SQLite, MySQL, XML).
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
   - Устанавливаем ReportBuilder (если хотим менять в веб-версии)
     https://www.microsoft.com/ru-ru/download/details.aspx?id=53613устанавливыаем - ReportBuilder.msi
   - взять готовые отчеты из проекта - Project SSRS
     - в проекте VS прописать Свойства проекта
       -> TargetServerUrl = http://localhost:80/ReportServer2019
       -> OverwriteDatasets = true
       -> OverwriteDatasources = true
     - Пуск проекта Project SSRS. Все автоматически уйдет на сервер.

  4) Базу SQLite перенести - C:\\DB_SQLite\\CurrencyChartFXMaven.db
  5) Для работы сервера необходимы ODBC Drivers x64
     - sqlite - http://www.ch-werner.de/sqliteodbc/ - файл sqliteodbc_w64.exe
     - postgeeSQL - https://www.postgresql.org/ftp/odbc/versions/msi/ - файл psqlodbc_13_02_0000-x64.zip
     - oracle - - https://www.oracle.com/database/technologies/oracle21c-windows-downloads.html - файл NT_213000_client_x64.zip
        - после установки меняем в глоб. реестре:
        - Компьютер\HKEY_LOCAL_MACHINE\SOFTWARE\ORACLE\KEY_OraClient21Home1 c AMERICAN_AMERICA.WE8MSWIN1252
          на NLS_LANG = AMERICAN_AMERICA.AL32UTF8 (либо AMERICAN_AMERICA.CL8MSWIN1251)
        - настраиваем tnsnames.ora для настройки x64 Oracle Client:
          XE = (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = localhost)(PORT = 1521)))(CONNECT_DATA =(SERVICE_NAME = XE)))
  6) Перезагружаем сервер (ПК)

----------------------------------------------------------------------------
Настройка HTTPS в Microsoft SQL Server 2019 Reporting Services
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
   - URL адрес веб-службы - выбираем HTTPS-сертификат (https://desktop-kq2l9b1/ReportServer2019)
     Применить.
   - URL адрес веб-портала - Долполнительно - Насколько HTTPS удостоверений для выбранного сейчас компонента Reporting Services
     Добавить, выбрать сертификат - ОК - ОК (https://desktop-kq2l9b1/Reports2019)

----------------------------------------------------------------------------
Visual Studio 2022 - настройки ODBC при разработке
----------------------------------------------------------------------------
Если вы работаете в Visual Studio и не видите служб Reporting Services в проектах необходимо
 - добавить компонент <Хранение и обработка данных> -> включить ВСЕ или SQL Server Data Tools
Если не появился +
 - из меню Visual Studio <Расширения> выберите <Управление расширениями> установить Microsoft Reporting Services Projects

-> XML read
------------------------------------------------------
- https://bank.gov.ua/depo_securities
- Пример построения https://www.mssqltips.com/sqlservertip/3148/sql-server-reporting-services-xml-data-source-and-data-set/

-> SQLite ODBC Driver
------------------------------------------------------
- http://www.ch-werner.de/sqliteodbc/
- скачиваем и ставим sqliteodbc.exe, sqliteodbc_w64.exe
- ODBC прописать при подключении в SSRS
- Driver={SQLite3 ODBC Driver};database=C:\\DB_SQLite\\CurrencyChartFXMaven.db;longnames=0;timeout=1000;notxn=0;syncpragma=NORMAL;stepapi=0

-> MySQL ODBC Driver
------------------------------------------------------
- https://dev.mysql.com/downloads/connector/odbc/
- скачиваем и ставим mysql-connector-odbc-8.0.28-winx64.msi или более новую (если не установнено вместе с базой данных)
- ODBC прописать при подключении в SSRS
- Driver={MySQL ODBC 8.0 ANSI Driver};Server=localhost;User=test_user;Password=12345678;Option=3;

-> PostgreSQL ODBC Driver
------------------------------------------------------
- https://www.postgresql.org/ftp/odbc/versions/msi/
- скачиваем и ставим psqlodbc_13_02_0000-x64 или более новую
- ODBC прописать при подключении в SSRS
- Driver={PostgreSQL ANSI};Server=localhost;Port=5432;Database=test_database;Uid=test_user;Pwd=12345678;

-> Oracle OLE DB Driver
------------------------------------------------------
- https://www.oracle.com/database/technologies/oracle21c-windows-downloads.html
- скачиваем и ставим Oracle клиента - NT_213000_client_x64.zip или более новый
- после установки меняем в глоб. реестре:
  - Компьютер\HKEY_LOCAL_MACHINE\SOFTWARE\ORACLE\KEY_OraClient21Home1 c AMERICAN_AMERICA.WE8MSWIN1252
    на NLS_LANG = AMERICAN_AMERICA.AL32UTF8 (либо AMERICAN_AMERICA.CL8MSWIN1251)
  - настраиваем tnsnames.ora для настройки x64 Oracle Client:
    XE = (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = localhost)(PORT = 1521)))(CONNECT_DATA =(SERVICE_NAME = XE)))
- настраиваем OLE DB подключение в SSRS
  - поставщик OLE DB: Oracle provider for OLE DB
  - имя базы = XE, логин = TEST_USER и пароль = TEST_USER.