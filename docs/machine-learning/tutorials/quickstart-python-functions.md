---
title: Краткое руководство. Функции Python
titleSuffix: SQL machine learning
description: В этом кратком руководстве вы узнаете, как использовать математические и служебные функции Python в машинном обучении SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 09/28/2020
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 646c1decb34adabe645a58d640b0f9adbb4768b7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101918"
---
# <a name="quickstart-python-functions-with-sql-machine-learning"></a>Краткое руководство. Функции Python с использованием машинного обучения SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

В этом кратком руководстве вы узнаете, как использовать математические и служебные функции Python в [Службах машинного обучения SQL Server](../sql-server-machine-learning-services.md), [Службах машинного обучения управляемого экземпляра Azure SQL](/azure/azure-sql/managed-instance/machine-learning-services-overview) или в [Кластерах больших данных SQL Server](../../big-data-cluster/machine-learning-services.md). Зачастую статистические функции, которые сложно реализовать в T-SQL, выполняются в Python всего парой строк кода.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим кратким руководством необходимо следующее.

- База данных SQL на одной из следующих платформ:
  - [Службы машинного обучения SQL Server](../sql-server-machine-learning-services.md). Сведения об установке см. в [руководстве по установке для Windows](../install/sql-machine-learning-services-windows-install.md) или [руководстве по установке для Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json).
  - Кластеры больших данных SQL Server. [Применение служб машинного обучения в кластерах больших данных SQL Server](../../big-data-cluster/machine-learning-services.md).
  - Службы машинного обучения в управляемом экземпляре SQL Azure. Дополнительные сведения см. в статье [Общие сведения о службах машинного обучения в управляемом экземпляре SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview).

- Средство для выполнения SQL-запросов, содержащих сценарии Python. В этом кратком руководстве используется [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

## <a name="create-a-stored-procedure-to-generate-random-numbers"></a>Создание хранимой процедуры для формирования случайных чисел

Для простоты давайте воспользуемся пакетом Python `numpy`, который устанавливается и загружается по умолчанию. Он содержит сотню функций для общих статистических задач, в том числе функцию `random.normal`, которая формирует указанное количество случайных чисел с нормальным распределением при заданном среднем значении и стандартном отклонении.

Например, следующий код Python возвращает 100 чисел со средним значением 50 и стандартным отклонением 3.

```Python
numpy.random.normal(size=100, loc=50, scale=3)
```

Чтобы вызвать эту строку Python из T-SQL, добавьте функцию Python в параметр скрипта Python `sp_execute_external_script`. На выходе должен получиться кадр данных, поэтому используйте для преобразования `pandas`.

```sql
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
import numpy
import pandas
OutputDataSet = pandas.DataFrame(numpy.random.normal(size=100, loc=50, scale=3));
'
    , @input_data_1 = N'   ;'
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

Как упростить формирование другого набора случайных чисел? Вы определяете хранимую процедуру, которая получает предоставленные пользователем аргументы и передает их в качестве переменных в скрипт Python.

```sql
CREATE PROCEDURE MyPyNorm (
      @param1 INT
    , @param2 INT
    , @param3 INT
    )
AS
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
import numpy
import pandas
OutputDataSet = pandas.DataFrame(numpy.random.normal(size=mynumbers, loc=mymean, scale=mysd));
'
    , @input_data_1 = N'   ;'
    , @params = N' @mynumbers int, @mymean int, @mysd int'
    , @mynumbers = @param1
    , @mymean = @param2
    , @mysd = @param3
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

- В первой строке определяется каждый из входных параметров SQL, необходимых при выполнении хранимой процедуры.

- Строка, начинающаяся с `@params`, определяет все переменные, используемые в коде Python, и соответствующие типы данных SQL.

- Следующие строки сопоставляют имена параметров SQL с соответствующими именами переменных Python.

Теперь, когда функция Python упакована в хранимой процедуре, вы можете легко вызвать ее и передать различные значения, как показано ниже:

```sql
EXECUTE MyPyNorm @param1 = 100,@param2 = 50, @param3 = 3
```

## <a name="use-python-utility-functions-for-troubleshooting"></a>Использование служебных функций Python для устранения неполадок

В пакетах Python предусмотрено множество служебных функций для изучения текущей среды Python. Они могут оказаться полезными, если вы нашли несоответствия в выполнении кода Python в SQL Server и внешних средах.

Например, с помощью функций управления системным временем в пакете `time` вы можете оценить количество времени, затрачиваемого процессами Python, и проанализировать проблемы, связанные с производительностью.

```sql
EXECUTE sp_execute_external_script
      @language = N'Python'
    , @script = N'
import time
start_time = time.time()

# Run Python processes

elapsed_time = time.time() - start_time
'
    , @input_data_1 = N' ;';
```

## <a name="next-steps"></a>Дальнейшие действия

Сведения о создании модели машинного обучения с использованием Python и машинного обучения SQL приведены в следующем кратком руководстве:

> [!div class="nextstepaction"]
> [Краткое руководство. Создание и оценка модели прогнозирования на Python](quickstart-python-train-score-model.md)