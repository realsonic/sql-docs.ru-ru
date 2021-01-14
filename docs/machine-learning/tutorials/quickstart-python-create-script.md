---
title: Краткое руководство. Выполнение скриптов Python
titleSuffix: SQL machine learning
description: Запустите ряд простых сценариев Python с помощью Служб машинного обучения в SQL Server, Кластерах больших данных или Управляемых экземплярах SQL Azure. Узнайте, как применять хранимую процедуру sp_execute_external_script для выполнения сценария.
ms.prod: sql
ms.technology: machine-learning
ms.date: 09/28/2020
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.custom: contperf-fy21q1
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 95c009dda19220fb0f5f5901f1da9d5539ca9e11
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101961"
---
# <a name="quickstart-run-simple-python-scripts-with-sql-machine-learning"></a>Краткое руководство. Выполнение простых скриптов Python с использованием машинного обучения SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

В этом кратком руководстве вы запустите ряд простых сценариев Python с помощью [Служб машинного обучения SQL Server](../sql-server-machine-learning-services.md), [Служб машинного обучения управляемых экземпляров SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview) или в [Кластерах больших данных SQL Server](../../big-data-cluster/machine-learning-services.md). Также вы узнаете, как применить хранимую процедуру [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) для выполнения скрипта в экземпляре SQL Server.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим кратким руководством необходимо следующее.

- База данных SQL на одной из следующих платформ:
  - [Службы машинного обучения SQL Server](../sql-server-machine-learning-services.md). Сведения об установке см. в [руководстве по установке для Windows](../install/sql-machine-learning-services-windows-install.md) или [руководстве по установке для Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json).
  - Кластеры больших данных SQL Server. [Применение служб машинного обучения в кластерах больших данных SQL Server](../../big-data-cluster/machine-learning-services.md).
  - Службы машинного обучения в управляемом экземпляре SQL Azure. Дополнительные сведения см. в статье [Общие сведения о службах машинного обучения в управляемом экземпляре SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview).

- Средство для выполнения SQL-запросов, содержащих сценарии Python. В этом кратком руководстве используется [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

## <a name="run-a-simple-script"></a>Выполнение простого сценария

Чтобы выполнить сценарий Python, необходимо передать его в качестве аргумента в системную хранимую процедуру [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Эта системная хранимая процедура запускает среду выполнения Python в контексте машинного обучения SQL, передает данные в Python, безопасно управляет пользовательскими сеансами в Python и возвращает результаты на клиент.

На следующих этапах вы запустите этот скрипт Python в базе данных:

```python
a = 1
b = 2
c = a/b
d = a*b
print(c, d)
```

1. Откройте новое окно запроса в среде **Azure Data Studio**, подключенной к вашему экземпляру SQL.

1. Передайте весь сценарий Python в хранимую процедуру `sp_execute_external_script`.

   Сценарий передается с помощью аргумента `@script`. Все, что находится внутри аргумента `@script`, должно быть допустимым кодом Python.

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'
    a = 1
    b = 2
    c = a/b
    d = a*b
    print(c, d)
    '
    ```

1. Далее вычисляется правильный результат, и функция Python `print` возвращает результат в окне **Сообщения**.

   Он должен выглядеть примерно так.

    **Результаты**

    ```text
    STDOUT message(s) from external script:
    0.5 2
    ```

## <a name="run-a-hello-world-script"></a>Выполнение сценария Hello World

Типичный пример — сценарий, который просто выводит строку "Hello World". Выполните следующую команду.

```sql
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'OutputDataSet = InputDataSet'
    , @input_data_1 = N'SELECT 1 AS hello'
WITH RESULT SETS(([Hello World] INT));
GO
```

В хранимую процедуру `sp_execute_external_script` передаются следующие входные данные.

| Входные данные | Описание |
|-|-|
| @language | Определяет вызываемое расширение языка (в данном случае Python). |
| @script | Определяет команды, которые передаются в среду выполнения Python. Весь скрипт Python должен быть включен в этот аргумент в виде текста в Юникоде. Также можно добавить текст в переменную типа **nvarchar**, а затем вызвать ее. |
| @input_data_1 | Данные, возвращаемые запросом, передаются в среду выполнения Python, которая возвращает их в виде кадра данных. |
| WITH RESULT SETS | Это предложение определяет схему возвращаемой таблицы данных для машинного обучения SQL. В данном случае добавляется "Hello World" в качестве имени столбца и **int** в качестве типа данных. |

Эта команда выводит следующий текст:

| Hello World |
|-------------|
| 1 |

## <a name="use-inputs-and-outputs"></a>Использование входных и выходных данных

По умолчанию процедура `sp_execute_external_script` принимает в качестве входных данных один набор данных, обычно предоставляемый в виде допустимого SQL-запроса. Затем она возвращает один кадр данных Python в качестве выходных данных.

Сейчас мы будем использовать заданные по умолчанию входные и выходные переменные процедуры `sp_execute_external_script`: **InputDataSet** и **OutputDataSet**.

1. Создайте небольшую таблицу тестовых данных.

    ```sql
    CREATE TABLE PythonTestData (col1 INT NOT NULL)
    
    INSERT INTO PythonTestData
    VALUES (1);
    
    INSERT INTO PythonTestData
    VALUES (10);
    
    INSERT INTO PythonTestData
    VALUES (100);
    GO
    ```

1. Используйте инструкцию `SELECT` для запроса в эту таблицу.
  
    ```sql
    SELECT *
    FROM PythonTestData
    ```

    **Результаты**

    ![Содержимое таблицы PythonTestData](./media/select-pythontestdata.png)

1. Выполните следующий скрипт Python. Он получает данные из таблицы с помощью инструкции `SELECT`, передает их через среду выполнения Python и возвращает результаты в виде кадра данных. Предложение `WITH RESULT SETS` определяет схему таблицы возвращаемых данных для SQL Server, добавляя имя столбца *NewColName*.

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'OutputDataSet = InputDataSet;'
        , @input_data_1 = N'SELECT * FROM PythonTestData;'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    **Результаты**

    ![Результат скрипта Python, возвращающий данные из таблицы](./media/python-output-pythontestdata.png)

1. Теперь измените имена входных и выходных переменных. Имена входных и выходных переменных по умолчанию°— **InputDataSet** и **OutputDataSet**. Следующий сценарий изменяет их на **SQL_in** и **SQL_out**.

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'SQL_out = SQL_in;'
        , @input_data_1 = N'SELECT 12 as Col;'
        , @input_data_1_name  = N'SQL_in'
        , @output_data_1_name = N'SQL_out'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    Обратите внимание, что в Python учитывается регистр. Входные и выходные переменные, используемые в сценарии Python (**SQL_out**, **SQL_in**), должны соответствовать именам, определенным в аргументах `@input_data_1_name` и `@output_data_1_name`, включая регистр.

    > [!TIP]
    > В качестве параметра может быть передан только один входной набор данных, и можно возвращать только один набор данных. Однако вы можете вызывать из кода Python другие наборы данных, а также возвращать выходные данные других типов в дополнение к этому набору данных. Вы также можете добавить ключевое слово OUTPUT к любому параметру, чтобы он возвращался с результатами.

1. Можно также формировать значения только с помощью сценария Python, без каких-либо входных данных (в аргументе `@input_data_1` задано пустое значение).

    Следующий сценарий выводит текст "hello" и "world".

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'
    import pandas as pd
    mytextvariable = pandas.Series(["hello", " ", "world"]);
    OutputDataSet = pd.DataFrame(mytextvariable);
    '
        , @input_data_1 = N''
    WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
    ```

    **Результаты**

    ![Результаты запроса с использованием @script в качестве входных данных](./media/python-data-generated-output.png)

> [!NOTE]
> В Python начальные пробелы используются для группирования инструкций. Поэтому когда внедренный сценарий Python разделяется на несколько строк, как в предыдущем примере, не пытайтесь выровнять команды Python по одной линии с командами SQL. Например, следующий сценарий выдаст ошибку.
>
> ```sql
> EXECUTE sp_execute_external_script @language = N'Python'
>       , @script = N'
>       import pandas as pd
>       mytextvariable = pandas.Series(["hello", " ", "world"]);
>       OutputDataSet = pd.DataFrame(mytextvariable);
>       '
>       , @input_data_1 = N''
> WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
> ```

## <a name="check-python-version"></a>Проверка версии Python

Если вы хотите узнать, какая версия Python установлена на сервере, выполните следующий сценарий.

```sql
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
import sys
print(sys.version)
'
GO
```

Функция Python `print` возвращает версию в окне **Сообщения**. В приведенном ниже примере можно видеть, что в данном случае установлен Python версии 3.5.2.

**Результаты**

```text
STDOUT message(s) from external script:
3.5.2 |Continuum Analytics, Inc.| (default, Jul  5 2016, 11:41:13) [MSC v.1900 64 bit (AMD64)]
```

## <a name="list-python-packages"></a>Список пакетов Python

Корпорация Майкрософт предоставляет ряд пакетов Python, которые устанавливаются вместе со Службами машинного обучения.

Чтобы просмотреть список установленных пакетов Python вместе с их версиями, выполните следующий сценарий.

```SQL
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
import pkg_resources
import pandas
dists = [str(d) for d in pkg_resources.working_set]
OutputDataSet = pandas.DataFrame(dists)
'
WITH RESULT SETS(([Package] NVARCHAR(max)))
GO
```

Список возвращается из `pkg_resources.working_set` в Python в формате кадра данных.

**Результаты**

:::image type="content" source="media/python-package-list.png" alt-text="Просмотр всех установленных пакетов Python":::

## <a name="next-steps"></a>Дальнейшие действия

Сведения о том, как применять структуры данных при использовании Python в машинном обучении SQL, см. в этом кратком руководстве:

> [!div class="nextstepaction"]
> [Краткое руководство. Использование структур данных и объектов с помощью Python](quickstart-python-data-structures.md)