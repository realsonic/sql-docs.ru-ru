---
title: Соединитель Apache Spark для SQL Server
description: Узнайте, как использовать соединитель Apache Spark для SQL Server.
ms.custom: ''
ms.date: 08/31/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: rajmera3
ms.author: raajmera
ms.reviewer: mikeray
ms.openlocfilehash: b4c7027fd4695e1c1110fb19ae8cefc9be34ac10
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438857"
---
# <a name="apache-spark-connector-sql-server--azure-sql"></a>Соединитель Apache Spark для SQL Server и Azure SQL

Соединитель Apache Spark для SQL Server и Azure SQL — это высокопроизводительный соединитель, который позволяет использовать данные о транзакциях в аналитике больших данных и сохранять результаты для нерегламентированных запросов или отчетов. Соединитель позволяет использовать любую базу данных SQL, локальную или облачную, в качестве источника входных данных или приемника выходных данных для заданий Spark.

Эта библиотека содержит исходный код соединителя Apache Spark для SQL Server и Azure SQL.

[Apache Spark](https://spark.apache.org/) — это единый аналитический механизм для крупномасштабной обработки данных.

Вы можете получить соединитель от [Maven](https://search.maven.org/search?q=spark-mssql-connector). Чтобы импортировать соединитель, используйте координату `com.microsoft.azure:spark-mssql-connector:<latest version>`. Вы также можете создать соединитель на основе источника данных или скачать JAR-файл из раздела выпуска на GitHub. Последние сведения о соединителе см. в [репозитории GitHub для соединителя Spark SQL](https://github.com/microsoft/sql-spark-connector).

## <a name="supported-features"></a>Поддерживаемые функции

* Поддержка всех привязок Spark (Scala, Python, R).
* Поддержка обычной проверки подлинности и вкладки ключа Active Directory (AD).
* Поддержка для операции записи переупорядоченного `dataframe`.
* Поддержка операции записи в единственном экземпляре SQL Server и пуле данных в Кластерах больших данных SQL Server.
* Поддержка надежного соединителя для единственного экземпляра SQL Server.

| Компонент                            | Поддерживаемые версии              |
|--------------------------------------|---------------------------------|
| Apache Spark                         | 2.4.5 (Spark 3.0 не поддерживается) |
| Scala                                | 2,11                            |
| Microsoft JDBC Driver для SQL Server | 8.2                             |
| Microsoft SQL Server                 | SQL Server 2008 или более поздняя версия        |
| Базы данных SQL Azure                  | Поддерживается                       |

> [!NOTE]
> Использование Azure Synapse Analytics не проверяется с помощью этого соединителя. Вы можете это сделать, однако могут возникнуть непредвиденные проблемы.

### <a name="supported-options"></a>Поддерживаемые варианты
Соединитель Apache Spark для SQL Server и Azure SQL поддерживает параметры, определенные здесь: [SQL DataSource JDBC](https://spark.apache.org/docs/latest/sql-data-sources-jdbc.html)

Кроме того, поддерживаются указанные ниже параметры

| Параметр | Значение по умолчанию | Описание |
| --------- | ------------------ | ------------------------------------------ |
| `reliabilityLevel` | `BEST_EFFORT` | `BEST_EFFORT` или `NO_DUPLICATES`. `NO_DUPLICATES` реализует надежную операцию вставки в сценариях перезапуска исполнителя |
| `dataPoolDataSource` | `none` | `none` означает, что значение не задано и соединитель должен записывать данные в один экземпляр SQL Server. Присвойте этому параметру имя источника данных для записи в таблицу пула данных в кластере больших данных|
| `isolationLevel` | `READ_COMMITTED` | Указание уровня изоляции |
| `tableLock` | `false` | Реализует операцию вставки с параметром `TABLOCK` для повышения производительности записи. |
| `schemaCheckEnabled` | `true` | Отключает строгие проверки кадра данных и схемы таблицы SQL, если установлено значение false |

Другие [параметры массового копирования](../jdbc/using-bulk-copy-with-the-jdbc-driver.md#sqlserverbulkcopyoptions) могут быть заданы в качестве параметров для `dataframe` и передаваться в API `bulkcopy` при записи.

## <a name="performance-comparison"></a>Сравнение производительности

Соединитель Apache Spark для SQL Server и Azure SQL выполняет операции в 15 раз быстрее, чем универсальный соединитель JDBC для записи в SQL Server. Характеристики производительности зависят от типа и объема данных, используемых параметров и могут варьироваться при запусках. Следующие результаты производительности отображают время, затрачиваемое на перезапись таблицы SQL с 143,9 млн строк в `dataframe` Spark. `dataframe` Spark создается путем считывания таблицы HDFS `store_sales`, созданной с помощью [теста производительности TPCDS Spark](https://github.com/databricks/spark-sql-perf). Время на считывание `store_sales` в `dataframe` исключено. Результаты представляют собой усредненное значение по трем запускам.

| Тип соединителя | Параметры | Описание |  Время записи |
| --------- | ------------------ | -------------------------------------| ---------- |
| `JDBCConnector` | По умолчанию | Универсальный соединитель JDBC с параметрами по умолчанию |  1385 секунд |
| `sql-spark-connector` | `BEST_EFFORT` | Наилучший вариант `sql-spark-connector` с параметрами по умолчанию |580 секунд |
| `sql-spark-connector` | `NO_DUPLICATES ` | Надежный `sql-spark-connector` | 709 секунд |
| `sql-spark-connector` | `BEST_EFFORT` + tabLock=true | Наилучший вариант `sql-spark-connector` с включенной блокировкой таблицы | 72 секунды |
| `sql-spark-connector` | `NO_DUPLICATES ` + tabLock=true| Надежный `sql-spark-connector` с включенной блокировкой таблицы| 198 секунд |

Config

- Конфигурация Spark: num_executors = 20, executor_memory = 1664 m, executor_cores = 2.
- Конфигурация создания данных: scale_factor=50, partitioned_tables=true.
- Файл данных `store_sales` с 143 997 590 строками.

Среда

- CU5 [кластера больших данных SQL Server](../../big-data-cluster/release-notes-big-data-cluster.md).
- `master` + 6 узлов.
- Сервер 5-го поколения (для каждого узла), 512 ГБ ОЗУ, 4 ТБ NVM на узел, сетевая карта 10 ГБ.

## <a name="commonly-faced-issues"></a>Часто встречающиеся проблемы

### `java.lang.NoClassDefFoundError: com/microsoft/aad/adal4j/AuthenticationException`

Эта проблема возникает из-за использования старой версии драйвера MSSQL (который теперь включен в этот соединитель) в среде Hadoop. Если вы использовали предыдущий соединитель Azure SQL и вручную устанавливали драйверы в этом кластере для обеспечения совместимости Azure Active Directory, их необходимо удалить.

Действия по устранению проблемы:

1. Если вы используете универсальную среду Hadoop, проверьте и удалите JAR-файл MSSQL: `rm $HADOOP_HOME/share/hadoop/yarn/lib/mssql-jdbc-6.2.1.jre7.jar`. Если вы используете Databricks, добавьте глобальный скрипт или скрипт инициализации кластера, чтобы удалить старые версии драйвера MSSQL из папки `/databricks/jars`, или вставьте следующую строку в существующий сценарий: `rm /databricks/jars/*mssql*`.
2. Добавьте пакеты `adal4j` и `mssql`. Например, можно использовать Maven, но должен подойти любой вариант. 

   > [!CAUTION]
   > Не устанавливайте соединитель SQL Spark таким образом.

1. Добавьте класс драйвера в конфигурацию подключения. Пример:

   ```csharp
   connectionProperties = {
     `Driver`: `com.microsoft.sqlserver.jdbc.SQLServerDriver`
   }`
   ```

Дополнительные сведения и описание способов устранения см. здесь: [https://github.com/microsoft/sql-spark-connector/issues/26](https://github.com/microsoft/sql-spark-connector/issues/26#issuecomment-672006339).

## <a name="get-started"></a>Приступая к работе

Соединитель Apache Spark для SQL Server и Azure SQL основан на API Spark DataSourceV1 и API массовых операций SQL Server. Он использует тот же интерфейс, что и встроенный соединитель Spark-SQL JDBC. Такая интеграция позволяет легко интегрировать соединитель и перенести существующие задания Spark, просто обновив параметр формата с помощью `com.microsoft.sqlserver.jdbc.spark`.

Чтобы включить соединитель в проекты, скачайте этот репозиторий и создайте JAR-файл с помощью SBT.

## <a name="write-to-a-new-sql-table"></a>Запись в новую таблицу SQL

> [!WARNING]
> Режим `overwrite` сначала удаляет таблицу, если она уже существует в базе данных по умолчанию. Используйте этот параметр с осторожностью, чтобы избежать непредвиденных потерь данных.

При использовании режима `overwrite`, если для воссоздания таблицы не используется параметр `truncate`, индексы будут потеряны. Таблица columnstore будет кучей. Если вы хотите сохранить существующее индексирование, задайте для параметра `truncate` значение true. Например, `.option("truncate","true")`.

```python
server_name = "jdbc:sqlserver://{SERVER_ADDR}"
database_name = "database_name"
url = server_name + ";" + "databaseName=" + database_name + ";"

table_name = "table_name"
username = "username"
password = "password123!#" # Please specify password here

try:
  df.write \
    .format("com.microsoft.sqlserver.jdbc.spark") \
    .mode("overwrite") \
    .option("url", url) \
    .option("dbtable", table_name) \
    .option("user", username) \
    .option("password", password) \
    .save()
except ValueError as error :
    print("Connector write failed", error)
```

### <a name="append-to-sql-table"></a>Добавление в таблицу SQL

```python
try:
  df.write \
    .format("com.microsoft.sqlserver.jdbc.spark") \
    .mode("append") \
    .option("url", url) \
    .option("dbtable", table_name) \
    .option("user", username) \
    .option("password", password) \
    .save()
except ValueError as error :
    print("Connector write failed", error)
```

## <a name="specify-the-isolation-level"></a>Указание уровня изоляции

По умолчанию этот соединитель использует уровень изоляции `READ_COMMITTED` при выполнении массовой операции вставки в базу данных. Если вы хотите переопределить уровень изоляции, используйте параметр `mssqlIsolationLevel`, как показано ниже.

```python
    .option("mssqlIsolationLevel", "READ_UNCOMMITTED") \
```

## <a name="read-from-sql-table"></a>Чтение из таблицы SQL

```python
jdbcDF = spark.read \
        .format("com.microsoft.sqlserver.jdbc.spark") \
        .option("url", url) \
        .option("dbtable", table_name) \
        .option("user", username) \
        .option("password", password).load()
```

## <a name="azure-active-directory-authentication"></a>Аутентификация Azure Active Directory

### <a name="python-example-with-service-principal"></a>Пример Python с субъектом-службой

```python
context = adal.AuthenticationContext(authority)
token = context.acquire_token_with_client_credentials(resource_app_id_url, service_principal_id, service_principal_secret)
access_token = token["accessToken"]

jdbc_db = spark.read \
        .format("com.microsoft.sqlserver.jdbc.spark") \
        .option("url", url) \
        .option("dbtable", table_name) \
        .option("accessToken", access_token) \
        .option("encrypt", "true") \
        .option("hostNameInCertificate", "*.database.windows.net") \
        .load()
```

### <a name="python-example-with-active-directory-password"></a>Пример Python с паролем Active Directory

```python
jdbc_df = spark.read \
        .format("com.microsoft.sqlserver.jdbc.spark") \
        .option("url", url) \
        .option("dbtable", table_name) \
        .option("authentication", "ActiveDirectoryPassword") \
        .option("user", user_name) \
        .option("password", password) \
        .option("encrypt", "true") \
        .option("hostNameInCertificate", "*.database.windows.net") \
        .load()
```

Для проверки подлинности с помощью Active Directory необходимо установить требуемую зависимость.

Для **Scala** необходимо установить артефакт `_com.microsoft.aad.adal4j_`.

Для **Python** необходимо установить библиотеку `_adal_`.  Это можно сделать с помощью PIP.

[Примеры записных книжек](https://github.com/microsoft/sql-spark-connector/tree/master/samples).

## <a name="support"></a>Поддержка

Соединитель Apache Spark для Azure SQL и SQL Server является проектом с открытым кодом. Корпорация Майкрософт не предоставляет поддержку для этого проекта. Чтобы устранить проблемы с соединителем или если у вас возникли вопросы, создайте запрос в этом репозитории проекта. Сообщество по вопросам соединителей активно отслеживает запросы.

## <a name="next-steps"></a>Дальнейшие шаги

Посетите [репозиторий GitHub для соединителя SQL Spark](https://github.com/microsoft/sql-spark-connector).

Сведения об уровнях изоляции см. в разделе [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).
