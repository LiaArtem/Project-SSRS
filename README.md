- Project SSRS VS2019 (Reports with MS SQL 2019, OracleXE 21c, PostgreSQL 14, SQLite, MySQL, XML)

SSRS работает с x86 ODBC Drivers и x86 Oracle Client

------------------------------------------------------
XML read
------------------------------------------------------
- https://bank.gov.ua/depo_securities
- Пример построения https://www.mssqltips.com/sqlservertip/3148/sql-server-reporting-services-xml-data-source-and-data-set/

------------------------------------------------------
SQLite ODBC Driver
------------------------------------------------------
- http://www.ch-werner.de/sqliteodbc/
- скачиваем и ставим sqliteodbc.exe
- ODBC прописать при подключении в SSRS
- Driver={SQLite3 ODBC Driver};database=d:\Прочие\Project\Project SSRS\CurrencyChartFXMaven.db;longnames=0;timeout=1000;notxn=0;syncpragma=NORMAL;stepapi=0

------------------------------------------------------
MySQL ODBC Driver
------------------------------------------------------
- https://dev.mysql.com/downloads/connector/odbc/
- скачиваем и ставим mysql-connector-odbc-8.0.27-win32.msi или более новую
- ODBC прописать при подключении в SSRS
- Driver={MySQL ODBC 8.0 ANSI Driver};Server=localhost;User=test_user;Password=12345678;Option=3;

------------------------------------------------------
PostgreSQL ODBC Driver
------------------------------------------------------
- https://www.postgresql.org/ftp/odbc/versions/msi/
- скачиваем и ставим psqlodbc_13_02_0000-x86.zip или более новую
- ODBC прописать при подключении в SSRS
- Driver={PostgreSQL ANSI};Server=localhost;Port=5432;Database=test_database;Uid=test_user;Pwd=12345678;

------------------------------------------------------
Oracle OLE DB Driver
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

