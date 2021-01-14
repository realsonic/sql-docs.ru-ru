---
title: Краткое руководство. Функции R
titleSuffix: SQL machine learning
description: В этом кратком руководстве вы узнаете, как использовать математические и служебные функции R в машинном обучении SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 0da73dd1202d4b5eab2c14077dfe54b04dfe9bd8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092672"
---
# <a name="quickstart-r-functions-with-sql-machine-learning"></a>Краткое руководство. Функции R с использованием машинного обучения SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
В этом кратком руководстве вы узнаете, как использовать математические и служебные функции R в [Службах машинного обучения SQL Server](../sql-server-machine-learning-services.md) или [Кластерах больших данных](../../big-data-cluster/machine-learning-services.md). Зачастую статистические функции, которые сложно реализовать в T-SQL, выполняются в R всего парой строк кода.
::: moniker-end
::: moniker range="=sql-server-2017"
В этом кратком руководстве вы узнаете, как использовать математические и служебные функции R в [Службах машинного обучения SQL Server](../sql-server-machine-learning-services.md). Зачастую статистические функции, которые сложно реализовать в T-SQL, выполняются в R всего парой строк кода.
::: moniker-end
::: moniker range="=sql-server-2016"
В этом кратком руководстве вы узнаете, как использовать математические и служебные функции R в службах [SQL Server R Services](../r/sql-server-r-services.md). Зачастую статистические функции, которые сложно реализовать в T-SQL, выполняются в R всего парой строк кода.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
В этом кратком руководстве вы узнаете, как использовать структуры и типы данных при работе с R в [Службах машинного обучения управляемого экземпляра SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview). Вы узнаете о том, как перемещать данные между R и Управляемым экземпляром SQL, и о типичных проблемах, возникающих при этом процессе.
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

## <a name="create-a-stored-procedure-to-generate-random-numbers"></a>Создание хранимой процедуры для формирования случайных чисел

Для простоты давайте воспользуемся пакетом R `stats`, который устанавливается и загружается по умолчанию. Он содержит сотню функций для общих статистических задач, в том числе функцию `rnorm`, которая формирует указанное количество случайных чисел с нормальным распределением при заданном среднем значении и стандартном отклонении.

Например, следующий код R возвращает 100 чисел со средним значением 50 и стандартным отклонением 3.

```R
as.data.frame(rnorm(100, mean = 50, sd = 3));
```

Чтобы вызвать строку кода R из T-SQL, добавьте функцию R в параметр скрипта R `sp_execute_external_script`, как показано ниже:

```sql
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
         OutputDataSet <- as.data.frame(rnorm(100, mean = 50, sd =3));'
    , @input_data_1 = N'   ;'
      WITH RESULT SETS (([Density] float NOT NULL));
```

Как упростить формирование другого набора случайных чисел?

С помощью T-SQL это несложно. Вы определяете хранимую процедуру, которая получает предоставленные пользователем аргументы и передает их в качестве переменных в скрипт R.

```sql
CREATE PROCEDURE MyRNorm (
    @param1 INT
    , @param2 INT
    , @param3 INT
    )
AS
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
         OutputDataSet <- as.data.frame(rnorm(mynumbers, mymean, mysd));'
    , @input_data_1 = N'   ;'
    , @params = N' @mynumbers int, @mymean int, @mysd int'
    , @mynumbers = @param1
    , @mymean = @param2
    , @mysd = @param3
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

- В первой строке определяется каждый из входных параметров SQL, необходимых при выполнении хранимой процедуры.

- Строка, начинающаяся с `@params`, определяет все переменные, используемые в коде R, и соответствующие типы данных SQL.

- Следующие строки сопоставляют имена параметров SQL с соответствующими именами переменных R.

Теперь, когда функция R упакована в хранимой процедуре, вы можете легко вызвать функцию и передать различные значения, как показано ниже.

```sql
EXECUTE MyRNorm @param1 = 100,@param2 = 50, @param3 = 3
```

## <a name="use-r-utility-functions-for-troubleshooting"></a>Использование служебных функций R для устранения неполадок

Установленный по умолчанию пакет **utils** предоставляет широкий набор служебных функций для изучения текущей среды R. Они могут оказаться полезными, если вы нашли несоответствия в выполнении кода R в SQL Server и внешних средах.

Например, можно использовать функции системного времени в R, такие как `system.time` и `proc.time`, чтобы фиксировать время, затрачиваемое процессами R, и анализировать проблемы производительности. Например, см. руководство [Создание характеристик данных](../tutorials/walkthrough-create-data-features.md), где функции времени R внедрены в решение.

```sql
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
        library(utils);
        start.time <- proc.time();
        
        # Run R processes
        
        elapsed_time <- proc.time() - start.time;'
```

Сведения о других полезных функциях см. в статье [Использование функций профилирования кода R для повышения производительности](../r/using-r-code-profiling-functions.md).

## <a name="next-steps"></a>Дальнейшие действия

Сведения о создании модели машинного обучения с использованием R и машинного обучения SQL приведены в следующем кратком руководстве:

> [!div class="nextstepaction"]
> [Создание и оценка прогнозной модели в R с помощью машинного обучения SQL](quickstart-r-train-score-model.md)