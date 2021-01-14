---
title: Соединение SQL с замыканием на себя в Python и R
description: Узнайте, как использовать подключение с замыканием на себя для обратного соединения с SQL Server через ODBC с целью чтения или записи данных из скрипта Python или R, выполняемого с помощью процедуры sp_execute_external_script.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/20/2020
ms.topic: how-to
author: Aniruddh25
ms.author: anmunde
ms.reviewer: dphansen
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 49d62ab967f972d777a02659b5a87b9e11740604
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094574"
---
# <a name="loopback-connection-to-sql-server-from-a-python-or-r-script"></a>Подключение к SQL Server из скрипта Python или R с замыканием на себя
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Узнайте, как использовать подключение с замыканием на себя со [Службами машинного обучения](../sql-server-machine-learning-services.md) для обратного соединения с SQL Server через [ODBC](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md) с целью чтения или записи данных из скрипта Python или R, выполняемого с помощью процедуры [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Его можно применять, если нельзя использовать аргументы **InputDataSet** и **OutputDataSet** процедуры `sp_execute_external_script`.

## <a name="connection-string"></a>Строка подключения

Чтобы установить подключение с замыканием на себя, необходимо использовать правильную строку подключения. Обычно обязательными аргументами являются имя [драйвера ODBC](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md), адрес сервера и имя базы данных.

### <a name="connection-string-on-windows"></a>Строка подключения в Windows

Для проверки подлинности SQL Server в Windows скрипт Python или R может использовать атрибут строки подключения **Trusted_Connection** для прохождения проверки подлинности от имени того же пользователя, который запустил процедуру sp_execute_external_script.

Вот пример строки подключения с замыканием на себя в Windows:

``` 
"Driver=SQL Server;Server=.;Database=nameOfDatabase;Trusted_Connection=Yes;"
```

### <a name="connection-string-on-linux"></a>Строка подключения в Linux

Для проверки подлинности SQL Server в Linux скрипт Python или R должен использовать атрибуты **ClientCertificate** и **ClientKey** драйвера ODBC, чтобы пройти проверку подлинности от имени пользователя, который выполнил процедуру `sp_execute_external_script`. Для этого необходимо использовать [драйвер ODBC последней версии](../../connect/odbc/download-odbc-driver-for-sql-server.md) 17.4.1.1.

Вот пример строки подключения с замыканием на себя в Linux:

```
"Driver=ODBC Driver 17 for SQL Server;Server=fe80::8012:3df5:0:5db1%eth0;Database=nameOfDatabase;ClientCertificate=file:/var/opt/mssql-extensibility/data/baeaac72-60b3-4fae-acfd-c50eff5d34a2/sqlsatellitecert.pem;ClientKey=file:/var/opt/mssql-extensibility/data/baeaac72-60b3-4fae-acfd-c50eff5d34a2/sqlsatellitekey.pem;TrustServerCertificate=Yes;Trusted_Connection=no;Encrypt=Yes"
```

Адрес сервера, расположение файла с сертификатом клиента и расположение файла с ключом клиента уникальны для каждой процедуры `sp_execute_external_script`. Их можно получить с помощью API **rx_get_sql_loopback_connection_string()** для Python или **rxGetSqlLoopbackConnectionString()** для R.

Дополнительные сведения об атрибутах строки подключения см. в статье [Ключевые слова и атрибуты строки подключения и имени DSN](../../connect/odbc/dsn-connection-string-attribute.md#new-connection-string-keywords-and-connection-attributes) для Microsoft ODBC Driver for SQL Server.

## <a name="generate-connection-string-with-revoscalepy-for-python"></a>Создание строки подключения с помощью пакета revoscalepy для Python

Вы можете использовать интерфейс API **rx_get_sql_loopback_connection_string()** из пакета [revoscalepy](../python/ref-py-revoscalepy.md), чтобы создать правильную строку подключения с замыканием на себя в скрипте Python.

Он принимает следующие аргументы:

| Аргумент | Описание |
|-|-|
| name_of_database | Имя базы данных, к которой устанавливается подключение |
| odbc_driver | Имя драйвера ODBC |

### <a name="examples"></a>Примеры

Пример для SQL Server в Windows:

```sql
EXECUTE sp_execute_external_script
@language = N'Python',
@script = N'
from revoscalepy import rx_get_sql_loopback_connection_string, RxSqlServerData, rx_data_step
loopback_connection_string = rx_get_sql_loopback_connection_string(odbc_driver="SQL Server", name_of_database="DBName")
print("Connection String:{0}".format(loopback_connection_string))
data_set = RxSqlServerData(sql_query = "select col1, col2 from tableName",
                           connection_string = loopback_connection_string)
OutputDataSet = rx_data_step(data_set)
'
WITH RESULT SETS ((col1 int, col2 int))
GO
```

Пример для SQL Server на Linux:

```sql
EXECUTE sp_execute_external_script
@language = N'Python',
@script = N'
from revoscalepy import rx_get_sql_loopback_connection_string, RxSqlServerData, rx_data_step
loopback_connection_string = rx_get_sql_loopback_connection_string(odbc_driver="ODBC Driver 17 for SQL Server",
                                                                   name_of_database="DBName")
print("Loopback Connection String:{0}".format(loopback_connection_string))
data_set = RxSqlServerData(sql_query = "select col1, col2 from tableName",
                           connection_string = loopback_connection_string)
OutputDataSet = rx_data_step(data_set)
'
WITH RESULT SETS ((col1 int, col2 int))
GO
```

## <a name="generate-connection-string-with-revoscaler-for-r"></a>Создание строки подключения с помощью пакета RevoScaleR для R

Вы можете использовать интерфейс API **rxGetSqlLoopbackConnectionString()** из пакета [RevoScaleR](../r/ref-r-revoscaler.md), чтобы создать правильную строку подключения с замыканием на себя в скрипте R.

Он принимает следующие аргументы:

| Аргумент | Описание |
|-|-|
| nameOfDatabase | Имя базы данных, к которой устанавливается подключение |
| odbcDriver | Имя драйвера ODBC |

### <a name="examples"></a>Примеры

Пример для SQL Server в Windows:

```sql
EXECUTE sp_execute_external_script
@language = N'R',
@script = N'
    loopbackConnectionString <- rxGetSqlLoopbackConnectionString(nameOfDatabase="DBName", odbcDriver ="SQL Server")
    print(paste("Connection String:", loopbackConnectionString))
    dataSet <- RxSqlServerData(sqlQuery = "select col1, col2 from tableName",
                               connectionString = loopbackConnectionString)
    OutputDataSet <- rxDataStep(dataSet)
'
WITH RESULT SETS ((col1 int, col2 int))
GO
```

Пример для SQL Server на Linux:

```sql
EXECUTE sp_execute_external_script
@language = N'R',
@script = N'
    loopbackConnectionString <-  rxGetSqlLoopbackConnectionString(nameOfDatabase="DBName", 
                                                                  odbcDriver ="ODBC Driver 17 for SQL Server")
    print(paste("Connection String:", loopbackConnectionString))
    dataSet <- RxSqlServerData(sqlQuery = "select col1, col2 from tableName", 
                               connectionString = loopbackConnectionString)
    OutputDataSet <- rxDataStep(dataSet)
'
WITH RESULT SETS ((col1 int, col2 int))
GO
```

## <a name="next-steps"></a>Дальнейшие действия

+ [Microsoft ODBC Driver for SQL Server](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md)
+ [revoscalepy](../python/ref-py-revoscalepy.md)
+ [RevoScaleR](../r/ref-r-revoscaler.md)
