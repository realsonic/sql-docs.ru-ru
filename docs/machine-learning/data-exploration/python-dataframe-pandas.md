---
title: Вставка данных из таблицы SQL в кадр данных Python pandas
titleSuffix: SQL machine learning
description: Узнайте, как считывать данные из таблицы SQL и вставлять их в таблицу данных pandas с помощью Python.
author: dphansen
ms.author: davidph
ms.date: 07/23/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current||=azuresqldb-current'
ms.openlocfilehash: 3acd5153599c6237117e52b483247033ebf983ec
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091634"
---
# <a name="insert-data-from-a-sql-table-into-a-python-pandas-dataframe"></a>Вставка данных из таблицы SQL в кадр данных Python pandas
[!INCLUDE[SQL Server SQL DB SQL MI](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

В этой статье описывается, как вставить данные SQL в кадр данных [pandas](https://pandas.pydata.org/) с помощью пакета [pyodbc](../../connect/python/pyodbc/python-sql-driver-pyodbc.md) в Python. Строки и столбцы, содержащиеся в кадре данных, можно использовать для дальнейшего изучения данных.

## <a name="prerequisites"></a>Предварительные требования

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
* [SQL Server для Windows](../../database-engine/install-windows/install-sql-server.md) или [для Linux](../../linux/sql-server-linux-overview.md)
::: moniker-end

::: moniker range="=azuresqldb-current"
* [База данных SQL Azure](/azure/sql-database/sql-database-get-started-portal)
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
* [Управляемый экземпляр SQL Azure](/azure/azure-sql/managed-instance/instance-create-quickstart)

* [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) для восстановления образца базы данных в Управляемый экземпляр SQL Azure.
::: moniker-end

* Azure Data Studio. Сведения об установке см. в разделе [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

* [Восстановление образца базы данных](../../samples/adventureworks-install-configure.md) для получения демонстрационных данных, используемых в этой статье.

## <a name="verify-restored-database"></a>Проверка восстановленной базы данных

Чтобы убедиться, что восстановленная база данных существует, выполните запрос к таблице **Person.CountryRegion**.

```sql
USE AdventureWorks;
SELECT * FROM Person.CountryRegion;
```

## <a name="install-python-packages"></a>Установка пакетов Python

[Скачивание и установка Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md).

Установите следующие пакеты Python.
  * pyodbc
  * pandas

  Чтобы установить эти пакеты, выполните приведенные ниже действия.

  1. В записной книжке Azure Data Studio выберите **Управление пакетами**.
  2. В области **Управление пакетами** выберите вкладку **Добавить новые**.
  3. Для каждого из следующих пакетов введите имя пакета, щелкните **Поиск**, а затем **Установить**.

## <a name="insert-data"></a>Добавление данных

Используйте следующий скрипт, чтобы выбрать данные из таблицы Person.CountryRegion и вставить их в кадр данных. Измените переменные строки подключения: 'server', 'database', 'username' и 'password' для подключения к SQL.

Чтобы создать записную книжку:

1. В Azure Data Studio выберите пункт **Файл** и **Новая записная книжка**.
2. В записной книжке выберите ядро **Python3** и нажмите **+ Код**.
3. Вставьте код в записную книжку и нажмите **Запустить все**.

```python
import pyodbc
import pandas as pd
# Some other example server values are
# server = 'localhost\sqlexpress' # for a named instance
# server = 'myserver,port' # to specify an alternate port
server = 'servername' 
database = 'AdventureWorks' 
username = 'yourusername' 
password = 'databasename'  
cnxn = pyodbc.connect('DRIVER={SQL Server};SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
# select 26 rows from SQL table to insert in dataframe.
query = "SELECT [CountryRegionCode], [Name] FROM Person.CountryRegion;"
df = pd.read_sql(query, cnxn)
print(df.head(26))
```

**Выходные данные**

Команда `print` в предыдущем скрипте отображает строки данных из кадра данных `pandas` `df`.

```text
CountryRegionCode                 Name
0                 AF          Afghanistan
1                 AL              Albania
2                 DZ              Algeria
3                 AS       American Samoa
4                 AD              Andorra
5                 AO               Angola
6                 AI             Anguilla
7                 AQ           Antarctica
8                 AG  Antigua and Barbuda
9                 AR            Argentina
10                AM              Armenia
11                AW                Aruba
12                AU            Australia
13                AT              Austria
14                AZ           Azerbaijan
15                BS         Bahamas, The
16                BH              Bahrain
17                BD           Bangladesh
18                BB             Barbados
19                BY              Belarus
20                BE              Belgium
21                BZ               Belize
22                BJ                Benin
23                BM              Bermuda
24                BT               Bhutan
25                BO              Bolivia
```

## <a name="next-steps"></a>Дальнейшие действия

+ [Вставка кадра данных Python в SQL](../data-exploration/python-dataframe-sql-server.md)