---
title: Краткое руководство. Запуск сценариев R
titleSuffix: SQL machine learning
description: Выполните ряд простых сценариев R с использованием машинного обучения SQL. Узнайте, как применять хранимую процедуру sp_execute_external_script для выполнения сценария.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 540804d2a35758627b24586ec8c29ca2dbab6e5f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101933"
---
# <a name="quickstart-run-simple-r-scripts-with-sql-machine-learning"></a>Краткое руководство. Выполнение простых сценариев R с использованием машинного обучения SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
В этом кратком руководстве вы запустите ряд простых сценариев R с помощью [Служб машинного обучения SQL Server](../sql-server-machine-learning-services.md) или в [Кластерах больших данных](../../big-data-cluster/machine-learning-services.md). Также вы узнаете, как применить хранимую процедуру [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) для выполнения скрипта в экземпляре SQL Server.
::: moniker-end
::: moniker range="=sql-server-2017"
В этом кратком руководстве вы запустите несколько простых скриптов R, используя [Службы машинного обучения SQL Server](../sql-server-machine-learning-services.md). Также вы узнаете, как применить хранимую процедуру [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) для выполнения скрипта в экземпляре SQL Server.
::: moniker-end
::: moniker range="=sql-server-2016"
В этом кратком руководстве вы запустите ряд простых сценариев R, используя службы [SQL Server R Services](../r/sql-server-r-services.md). Также вы узнаете, как применить хранимую процедуру [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) для выполнения скрипта в экземпляре SQL Server.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
В этом кратком руководстве вы запустите ряд простых скриптов R, используя [Службы машинного обучения в управляемом экземпляре SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview). Также вы узнаете, как применить хранимую процедуру [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) для выполнения скрипта в базе данных.
::: moniker-end

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим кратким руководством необходимо следующее.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
- Службы машинного обучения SQL Server. Сведения об установке служб машинного обучения см. в [руководстве по установке для Windows](../install/sql-machine-learning-services-windows-install.md) или [руководстве по установке для Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json). Можно также [включить Службы машинного обучения в кластерах больших данных SQL Server](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
- Службы машинного обучения SQL Server. Сведения об установке служб машинного обучения см. в [руководстве по установке для Windows](../install/sql-machine-learning-services-windows-install.md). 
::: moniker-end
::: moniker range="=sql-server-2016"
- SQL Server 2016 R Services. Сведения об установке служб R Services см. в [руководстве по установке для Windows](../install/sql-r-services-windows-install.md). 
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
- Службы машинного обучения в Управляемом экземпляре SQL Azure. Дополнительные сведения см. в статье [Общие сведения о службах машинного обучения в управляемом экземпляре SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

- Инструмент для выполнения SQL-запросов, содержащих сценарии R. В этом кратком руководстве используется [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

## <a name="run-a-simple-script"></a>Выполнение простого сценария

Чтобы выполнить сценарий R, необходимо передать его в качестве аргумента в системную хранимую процедуру [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Эта системная хранимая процедура запускает среду выполнения R, передает данные в R, безопасно управляет пользовательскими сеансами R и возвращает результаты клиенту.

В следующих шагах вы запустите этот пример сценария R.

```r
a <- 1
b <- 2
c <- a/b
d <- a*b
print(c(c, d))
```

1. Откройте **Azure Data Studio** и подключитесь к своему серверу.

1. Передайте весь сценарий R в хранимую процедуру `sp_execute_external_script`.

   Сценарий передается с помощью аргумента `@script`. Все, что находится внутри аргумента `@script`, должно быть допустимым кодом R.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c(c, d))
    '
    ```

1. Далее вычисляется правильный результат, и функция R `print` возвращает результат в окне **Сообщения**.

   Он должен выглядеть примерно так.

    **Результаты**

    ```text
    STDOUT message(s) from external script:
    0.5 2
    ```

## <a name="run-a-hello-world-script"></a>Выполнение сценария Hello World

Типичный пример — сценарий, который просто выводит строку "Hello World". Выполните следующую команду.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'OutputDataSet<-InputDataSet'
    , @input_data_1 = N'SELECT 1 AS hello'
WITH RESULT SETS(([Hello World] INT));
GO
```

В хранимую процедуру `sp_execute_external_script` передаются следующие входные данные.

| Входные данные | Описание |
|-|-|
| @language | Определяет вызываемое расширение языка (в данном случае R). |
| @script | Определяет команды, которые передаются в среду выполнения R. Весь сценарий R должен содержаться в этом аргументе в виде текста в Юникоде. Также можно добавить текст в переменную типа **nvarchar**, а затем вызвать ее. |
| @input_data_1 | Данные, возвращаемые запросом, передаются в среду выполнения R, которая возвращает их в виде кадра данных. |
|WITH RESULT SETS | Это предложение определяет схему возвращаемой таблицы данных. В данном случае добавляется "Hello World" в качестве имени столбца и **int** в качестве типа данных. |

Эта команда выводит следующий текст:

| Hello World |
|-------------|
| 1 |

## <a name="use-inputs-and-outputs"></a>Использование входных и выходных данных

По умолчанию процедура `sp_execute_external_script` принимает в качестве входных данных один набор данных, обычно предоставляемый в виде допустимого SQL-запроса. Затем она возвращает один кадр данных R в качестве выходных данных.

Сейчас мы будем использовать заданные по умолчанию входные и выходные переменные процедуры `sp_execute_external_script`: **InputDataSet** и **OutputDataSet**.

1. Создайте небольшую таблицу тестовых данных.

    ```sql
    CREATE TABLE RTestData (col1 INT NOT NULL)
    
    INSERT INTO RTestData
    VALUES (1);
    
    INSERT INTO RTestData
    VALUES (10);
    
    INSERT INTO RTestData
    VALUES (100);
    GO
    ```

1. Используйте инструкцию `SELECT` для запроса в эту таблицу.
  
    ```sql
    SELECT *
    FROM RTestData
    ```

    **Результаты**

    ![Содержимое таблицы RTestData](./media/select-rtestdata.png)

1. Выполните следующий сценарий R. Он получает данные из таблицы с помощью инструкции `SELECT`, передает их через среду выполнения R и возвращает результаты в виде кадра данных. Предложение `WITH RESULT SETS` определяет схему таблицы возвращаемых данных для SQL Server, добавляя имя столбца *NewColName*.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'OutputDataSet <- InputDataSet;'
        , @input_data_1 = N'SELECT * FROM RTestData;'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    **Результаты**

    ![Выходные данные сценария R, возвращающего данные из таблицы](./media/r-output-rtestdata.png)

1. Теперь изменим имена входных и выходных переменных. Имена входных и выходных переменных по умолчанию°— **InputDataSet** и **OutputDataSet**. Сценарий изменяет их на **SQL_in** и **SQL_out**:

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N' SQL_out <- SQL_in;'
        , @input_data_1 = N' SELECT 12 as Col;'
        , @input_data_1_name = N'SQL_in'
        , @output_data_1_name = N'SQL_out'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    Обратите внимание, что в R учитывается регистр. Входные и выходные переменные, используемые в сценарии R (**SQL_out**, **SQL_in**), должны соответствовать именам, определенным в аргументах `@input_data_1_name` и `@output_data_1_name`, включая регистр.

   > [!TIP]
   > В качестве параметра может быть передан только один входной набор данных, и можно возвращать только один набор данных. Однако вы можете вызывать другие наборы данных из кода R, а также возвращать выходные данные других типов в дополнение к набору данных. Вы также можете добавить ключевое слово OUTPUT к любому параметру, чтобы он возвращался с результатами.

1. Также можно формировать значения только с помощью сценария R, без каких-либо входных данных (в аргументе `@input_data_1` задано пустое значение).

   Следующий сценарий выводит текст "hello" и "world".

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    mytextvariable <- c("hello", " ", "world");
    OutputDataSet <- as.data.frame(mytextvariable);
    '
        , @input_data_1 = N''
    WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
    ```

    **Результаты**

    ![Результаты запроса с использованием @script в качестве входных данных](./media/r-data-generated-output.png)

## <a name="check-r-version"></a>Проверка версии R

Если вы хотите узнать, какая версия R установлена, выполните приведенный ниже скрипт.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(version)';
GO
```

Функция R `print` возвращает версию в окне **Сообщения**. В приведенном ниже примере можно видеть, что в данном случае установлен R версии 3.4.4.

**Результаты**

```text
STDOUT message(s) from external script:
                   _
platform       x86_64-w64-mingw32
arch           x86_64
os             mingw32
system         x86_64, mingw32
status
major          3
minor          4.4
year           2018
month          03
day            15
svn rev        74408
language       R
version.string R version 3.4.4 (2018-03-15)
nickname       Someone to Lean On
```

## <a name="list-r-packages"></a>Получение списка пакетов R
::: moniker range=">=sql-server-2017"
Корпорация Майкрософт предоставляет ряд пакетов R, которые устанавливаются вместе со Службами машинного обучения.
::: moniker-end
::: moniker range="=sql-server-2016"
Корпорация Майкрософт предоставляет ряд пакетов R, которые устанавливаются вместе со службами R Services.
::: moniker-end

Чтобы просмотреть список установленных пакетов R, включая сведения о версии, зависимостях, лицензии и пути к библиотеке, выполните следующий сценарий.

```SQL
EXEC sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License", "LibPath")]);'
WITH result sets((
            Package NVARCHAR(255)
            , Version NVARCHAR(100)
            , Depends NVARCHAR(4000)
            , License NVARCHAR(1000)
            , LibPath NVARCHAR(2000)
            ));
```

Выходные данные `installed.packages()` в R и возвращаются в виде результирующего набора.

**Результаты**

![Установленные пакеты в R](./media/rsql-installed-packages.png) 

## <a name="next-steps"></a>Дальнейшие действия

Сведения о том, как применять структуры данных при использовании R в машинном обучении SQL, см. в этом кратком руководстве:

> [!div class="nextstepaction"]
> [Обработка типов данных и объектов при работе с R и машинным обучением SQL](quickstart-r-data-types-and-objects.md)