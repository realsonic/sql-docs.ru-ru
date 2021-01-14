---
title: Вставка кадра данных Python в таблицу SQL
titleSuffix: SQL machine learning
description: Как вставлять данные из кадра данных в таблицу SQL.
author: dphansen
ms.author: davidph
ms.date: 07/23/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current||=azuresqldb-current'
ms.openlocfilehash: 9a8b30cc1fa5af3e22ce0294e8eff21cf413e2c6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102070"
---
# <a name="insert-python-dataframe-into-sql-table"></a>Вставка кадра данных Python в таблицу SQL
[!INCLUDE[SQL Server SQL DB SQL MI](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

В этой статье описывается, как вставить кадр данных [pandas](https://pandas.pydata.org/) в базу данных SQL с помощью пакета [pyodbc](../../connect/python/pyodbc/python-sql-driver-pyodbc.md) в Python.

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

Чтобы убедиться, что восстановленная база данных существует, выполните запрос к таблице **HumanResources.Department**.

```sql
USE AdventureWorks;
SELECT * FROM HumanResources.Department;
```

## <a name="install-python-packages"></a>Установка пакетов Python

* [Скачивание и установка Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md)

Установите следующие пакеты Python.
  * pyodbc
  * pandas

  Чтобы установить эти пакеты, выполните приведенные ниже действия.

  1. В записной книжке Azure Data Studio выберите **Управление пакетами**.
  2. В области **Управление пакетами** выберите вкладку **Добавить новые**.
  3. Для каждого из следующих пакетов введите имя пакета, щелкните **Поиск**, а затем **Установить**.

## <a name="connect-to-sql-server-using-azure-data-studio"></a>Подключение к SQL Server с помощью Azure Data Studio

[Подключение с помощью Azure Data Studio](../../azure-data-studio/quickstart-sql-server.md).

1. Подключитесь к базе данных AdventureWorks, чтобы создать новую таблицу HumanResources.DepartmentTest. Таблица SQL будет использоваться для вставки кадров данных.

```sql
CREATE TABLE [HumanResources].[DepartmentTest](
[DepartmentID] [smallint] NOT NULL,
[Name] [dbo].[Name] NOT NULL,
[GroupName] [dbo].[Name] NOT NULL
)
GO
```

## <a name="create-csv-file"></a>Создание CSV-файла

Скопируйте текст и сохраните файл как department.csv для кадра данных.

```text
DepartmentID,Name,GroupName,
1,Engineering,Research and Development,
2,Tool Design,Research and Development,
3,Sales,Sales and Marketing,
4,Marketing,Sales and Marketing,
5,Purchasing,Inventory Management,
6,Research and Development,Research and Development,
7,Production,Manufacturing,
8,Production Control,Manufacturing,
9,Human Resources,Executive General and Administration,
10,Finance,Executive General and Administration,
11,Information Services,Executive General and Administration,
12,Document Control,Quality Assurance,
13,Quality Assurance,Quality Assurance,
14,Facilities and Maintenance,Executive General and Administration,
15,Shipping and Receiving,Inventory Management,
16,Executive,Executive General and Administration
```

## <a name="connect-to-sql-using-python"></a>Подключение к SQL с помощью Python

1. Измените переменные строки подключения: 'server', 'database', 'username' и 'password' для подключения к базе данных SQL.

2. Изменить путь к CSV-файлу.

## <a name="load-dataframe-from-csv-file"></a>Загрузка кадра данных из CSV-файла

Используйте пакет `pandas` Python, чтобы создать кадр данных и загрузить CSV-файл. Подключитесь к SQL, чтобы загрузить кадр данных в новую таблицу SQL HumanResources.DepartmentTest.

Чтобы создать записную книжку:

1. В Azure Data Studio выберите пункт **Файл** и **Новая записная книжка**.
2. В записной книжке выберите ядро **Python3** и нажмите **+ Код**.
3. Вставьте код в записную книжку и нажмите **Запустить все**.

 ```Python
import pyodbc
import pandas as pd
# insert data from csv file into dataframe.
# working directory for csv file: type "pwd" in Azure Data Studio or Linux
# working directory in Windows c:\users\username
df = pd.read_csv("c:\\user\\username\department.csv")
# Some other example server values are
# server = 'localhost\sqlexpress' # for a named instance
# server = 'myserver,port' # to specify an alternate port
server = 'yourservername' 
database = 'AdventureWorks' 
username = 'username' 
password = 'yourpassword' 
cnxn = pyodbc.connect('DRIVER={SQL Server};SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
# Insert Dataframe into SQL Server:
for index, row in df.iterrows():
     cursor.execute("INSERT INTO HumanResources.DepartmentTest (DepartmentID,Name,GroupName) values(?,?,?)", row.DepartmentID, row.Name, row.GroupName)
cnxn.commit()
cursor.close()
```

## <a name="confirm-row-count-in-sql"></a>Подтверждение числа строк в SQL

Выполните инструкцию SQL, чтобы убедиться, что таблица успешно загружена с данными из кадра данных.

```sql
SELECT count(*) from HumanResources.DepartmentTest;
```

Результаты

```bash
(No column name)
16
```

## <a name="next-steps"></a>Дальнейшие действия

+ [Построение гистограммы для изучения данных с помощью Python](../data-exploration/python-plot-histogram.md)