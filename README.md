SSRS работает с x86 ODBC Drivers

SQLite ODBC Driver
- http://www.ch-werner.de/sqliteodbc/
- скачиваем и ставим sqliteodbc.exe
- ODBC прописать при подключении в SSRS
- Driver={SQLite3 ODBC Driver};database=d:\Прочие\Project\Project SSRS\CurrencyChartFXMaven.db;longnames=0;timeout=1000;notxn=0;syncpragma=NORMAL;stepapi=0

MySQL ODBC Driver
- https://dev.mysql.com/downloads/connector/odbc/
- скачиваем и ставим mysql-connector-odbc-8.0.27-win32.msi
- ODBC прописать при подключении в SSRS
- Driver={MySQL ODBC 8.0 ANSI Driver};Server=localhost;User=test_user;Password=12345678;Option=3;

XML read
- https://bank.gov.ua/depo_securities
- Пример построения https://www.mssqltips.com/sqlservertip/3148/sql-server-reporting-services-xml-data-source-and-data-set/