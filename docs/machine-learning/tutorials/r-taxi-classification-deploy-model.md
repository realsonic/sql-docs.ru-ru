---
title: Учебник по R. Запуск прогнозов в хранимых процедурах SQL
titleSuffix: SQL machine learning
description: В последнем, 5-м из серии руководств, вы ознакомитесь с внедрением встроенных скриптов R в хранимые процедуры SQL с функциями T-SQL в машинном обучении SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 10/15/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 026090f3eb61e2bd2ffe2b96eb09751cc3543dbf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101875"
---
# <a name="r-tutorial-run-predictions-in-sql-stored-procedures"></a>Учебник по R. Запуск прогнозов в хранимых процедурах SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

В части 5 серии руководств из пяти частей вы узнаете, как *внедрить* модель, которую вы обучили и сохранили в предыдущей части, используя эту модель для прогнозирования возможных результатов. Модель заключается в хранимую процедуру, которую могут напрямую вызывать другие приложения.

В этой статье показано два способа оценки:

+ **Режим пакетной оценки**. В качестве входных данных для хранимой процедуры используется запрос SELECT. Хранимая процедура возвращает таблицу наблюдений, соответствующую входным вариантам.

+ **Режим индивидуальной оценки**. Передайте набор отдельных значений параметров в качестве входных данных.  Хранимая процедура возвращает одну строку или значение.

Работая с этой статьей, вы узнаете о следующем.

> [!div class="checklist"]
> + Создание и использование хранимых процедур для пакетной оценки
> + Создание и использование хранимых процедур для оценки одной строки

В [первой части](r-taxi-classification-introduction.md) были установлены необходимые компоненты и восстановлена демонстрационная база данных.

Во [второй части](r-taxi-classification-explore-data.md) вы узнали, как проверить пример данных и создать графики.

В [третьей части](r-taxi-classification-create-features.md) вы узнали, как создавать функции из необработанных данных с помощью функции Transact-SQL. Затем вы вызвали эту функцию из хранимой процедуры, чтобы создать таблицу, содержащую значения характеристик.

В [четвертой части](r-taxi-classification-train-model.md) вы научились загружать модули и вызывать необходимые функции для создания и обучения модели с помощью хранимой процедуры SQL Server.

## <a name="basic-scoring"></a>Базовый процесс оценки

Хранимая процедура **RPredict** иллюстрирует базовый синтаксис для заключения вызова `PREDICT` в хранимую процедуру.

```sql
CREATE PROCEDURE [dbo].[RPredict] (@model varchar(250), @inquery nvarchar(max))
AS 
BEGIN 

DECLARE @lmodel2 varbinary(max) = (SELECT model FROM nyc_taxi_models WHERE name = @model);  
EXEC sp_execute_external_script @language = N'R',
  @script = N' 
    mod <- unserialize(as.raw(model));
    print(summary(mod))
    OutputDataSet <- data.frame(predict(mod, InputDataSet, type = "response"));
    str(OutputDataSet)
    print(OutputDataSet)
    ',
  @input_data_1 = @inquery,
  @params = N'@model varbinary(max)',
  @model = @lmodel2 
  WITH RESULT SETS (("Score" float));
END
GO
```

+ Инструкция SELECT получает сериализованную модель из базы данных и сохраняет ее в переменной R `mod` для дальнейшей обработки с помощью языка R.

+ Новые варианты для оценки получаются из запроса [!INCLUDE[tsql](../../includes/tsql-md.md)], указанного в `@inquery`, первом параметре хранимой процедуры. По мере считывания данных запроса строки сохраняются в кадре данных по умолчанию `InputDataSet`. Этот кадр данных передается в функцию [PREDICT](../../t-sql/queries/predict-transact-sql.md), которая формирует оценки.
  
  `OutputDataSet <- data.frame(predict(mod, InputDataSet, type = "response"));`
  
  Так как кадр данных может содержать одну строку, один и тот же код можно использовать как для пакетной, так и для индивидуальной оценки.
  
+ Значение, возвращаемое функцией `PREDICT`, имеет тип **float** и представляет вероятность получения водителем чаевых в любом размере.

## <a name="batch-scoring-a-list-of-predictions"></a>Пакетная оценка (список прогнозов)

Более распространенный сценарий заключается в формировании прогнозов для нескольких наблюдений в пакетном режиме. На данном этапе рассмотрим, как работает пакетная оценка.

1. Сначала получим набор входных данных меньшего размера, с которым будем далее работать. Этот запрос создает список первых 10 поездок с числом пассажиров и другими характеристиками, необходимыми для составления прогноза.
  
   ```sql
   SELECT TOP 10 a.passenger_count AS passenger_count, a.trip_time_in_secs AS trip_time_in_secs, a.trip_distance AS trip_distance, a.dropoff_datetime AS dropoff_datetime, dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude,dropoff_longitude) AS direct_distance
   
   FROM (SELECT medallion, hack_license, pickup_datetime, passenger_count,trip_time_in_secs,trip_distance, dropoff_datetime, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude FROM nyctaxi_sample)a

   LEFT OUTER JOIN

   (SELECT medallion, hack_license, pickup_datetime FROM nyctaxi_sample TABLESAMPLE (70 percent) REPEATABLE (98052)    )b

   ON a.medallion=b.medallion AND a.hack_license=b.hack_license 
   AND a.pickup_datetime=b.pickup_datetime
   WHERE b.medallion IS NULL
   ```

   **Образец результатов**

   ```text
   passenger_count   trip_time_in_secs    trip_distance  dropoff_datetime          direct_distance
   1                 283                  0.7            2013-03-27 14:54:50.000   0.5427964547
   1                 289                  0.7            2013-02-24 12:55:29.000   0.3797099614
   1                 214                  0.7            2013-06-26 13:28:10.000   0.6970098661
   ```

2. Создайте хранимую процедуру с именем **RPredictBatchOutput** в [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].

   ```sql
   CREATE PROCEDURE [dbo].[RPredictBatchOutput] (@model varchar(250), @inquery nvarchar(max))
   AS
   BEGIN
   DECLARE @lmodel2 varbinary(max) = (SELECT model FROM nyc_taxi_models WHERE name = @model);
   EXEC sp_execute_external_script 
     @language = N'R',
     @script = N'
       mod <- unserialize(as.raw(model));
       print(summary(mod))
       OutputDataSet <- data.frame(predict(mod, InputDataSet, type = "response"));
       str(OutputDataSet)
       print(OutputDataSet)
     ',
     @input_data_1 = @inquery,
     @params = N'@model varbinary(max)',
     @model = @lmodel2
     WITH RESULT SETS ((Score float));
   END
   ```

3. Задайте текст запроса в переменной и передайте ее в качестве параметра в хранимую процедуру:

   ```sql
   -- Define the input data
   DECLARE @query_string nvarchar(max)
   SET @query_string='SELECT TOP 10 a.passenger_count as passenger_count, a.trip_time_in_secs AS trip_time_in_secs, a.trip_distance AS trip_distance, a.dropoff_datetime AS dropoff_datetime, dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude,dropoff_longitude) AS direct_distance FROM  (SELECT medallion, hack_license, pickup_datetime, passenger_count,trip_time_in_secs,trip_distance, dropoff_datetime, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude FROM nyctaxi_sample  )a   LEFT OUTER JOIN (SELECT medallion, hack_license, pickup_datetime FROM nyctaxi_sample TABLESAMPLE (70 percent) REPEATABLE (98052))b ON a.medallion=b.medallion AND a.hack_license=b.hack_license AND a.pickup_datetime=b.pickup_datetime WHERE b.medallion is null'
   
   -- Call the stored procedure for scoring and pass the input data
   EXEC [dbo].[RPredictBatchOutput] @model = 'RTrainLogit_model', @inquery = @query_string;
   ```
  
Эта хранимая процедура возвращает ряд значений, представляющих прогнозы для каждой из 10 самых популярных поездок. Однако самые популярные поездки — это поездки с одним пассажиром на относительно короткое расстояние, за которые водители вряд ли получат чаевые.

> [!TIP]
> Вместо получения результатов типа "есть чаевые" или "нет чаевых" можно также получить оценку вероятности для прогноза, а затем применить предложение WHERE к значениям столбца _Score_, чтобы классифицировать оценки как "высокая вероятность чаевых" или "низкая вероятность чаевых", используя пороговое значение, например 0,5 или 0,7. Это действие отсутствует в хранимой процедуре, но его можно легко реализовать.

## <a name="single-row-scoring-of-multiple-inputs"></a>Оценка одной строки нескольких входных значений

Иногда требуется передать несколько входных значений и получить один прогноз на основе этих значений. Например, можно настроить лист Excel, веб-приложение или отчет служб Reporting Services так, чтобы они вызывали хранимую процедуру, и передавать в нее входные значения, введенные или выбранные пользователями в этих приложениях.

В этом разделе будет показано, как создавать отдельные прогнозы с помощью хранимой процедуры, которая принимает несколько входных значений, таких как количество пассажиров, расстояние поездки и т. д. Хранимая процедура создает оценку на основе ранее сохраненной модели R.
  
При вызове хранимой процедуры из внешнего приложения данные должны соответствовать требованиям модели R. К ним могут относиться возможность приведения или преобразования входных данных в тип данных R или проверка типа и длины данных.

1. Создайте хранимую процедуру **RPredictSingleRow**.
  
   ```sql
   CREATE PROCEDURE [dbo].[RPredictSingleRow] @model varchar(50), @passenger_count int = 0, @trip_distance float = 0, @trip_time_in_secs int = 0, @pickup_latitude float = 0, @pickup_longitude float = 0, @dropoff_latitude float = 0, @dropoff_longitude float = 0
   AS
   BEGIN
   DECLARE @inquery nvarchar(max) = N'SELECT * FROM [dbo].[fnEngineerFeatures](@passenger_count, @trip_distance, @trip_time_in_secs,  @pickup_latitude, @pickup_longitude, @dropoff_latitude, @dropoff_longitude)';
   DECLARE @lmodel2 varbinary(max) = (SELECT model FROM nyc_taxi_models WHERE name = @model);
   EXEC sp_execute_external_script  
     @language = N'R',
     @script = N'  
       mod <- unserialize(as.raw(model));  
       print(summary(mod));  
       OutputDataSet <- data.frame(predict(mod, InputDataSet, type = "response"));
       str(OutputDataSet);
       print(OutputDataSet); 
       ',  
     @input_data_1 = @inquery,  
     @params = N'@model varbinary(max),@passenger_count int,@trip_distance float,@trip_time_in_secs int ,  @pickup_latitude float ,@pickup_longitude float ,@dropoff_latitude float ,@dropoff_longitude float', @model = @lmodel2, @passenger_count =@passenger_count, @trip_distance=@trip_distance, @trip_time_in_secs=@trip_time_in_secs, @pickup_latitude=@pickup_latitude, @pickup_longitude=@pickup_longitude, @dropoff_latitude=@dropoff_latitude, @dropoff_longitude=@dropoff_longitude  
     WITH RESULT SETS ((Score float));  
   END
   ```

2. Попробуйте выполнить ее, указав значения вручную.
  
   Откройте новое окно **Запрос** и вызовите хранимую процедуру, введя значения для каждого из параметров. Параметры представляют столбцы характеристик, используемые моделью, и являются обязательными.

   ```sql
   EXEC [dbo].[RPredictSingleRow] @model = 'RTrainLogit_model',
   @passenger_count = 1,
   @trip_distance = 2.5,
   @trip_time_in_secs = 631,
   @pickup_latitude = 40.763958,
   @pickup_longitude = -73.973373,
   @dropoff_latitude =  40.782139,
   @dropoff_longitude = -73.977303
   ```

   Можно также использовать более короткую форму, поддерживаемую для [параметров хранимой процедуры](../../relational-databases/stored-procedures/specify-parameters.md):
  
   ```sql
   EXEC [dbo].[RPredictSingleRow] 'RTrainLogit_model', 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
   ```

3. Результаты говорят о том, что вероятность получения чаевых в этих 10 поездках очень мала (ноль), так как все они являются поездками с одним пассажиром на сравнительно короткое расстояние.

## <a name="conclusions"></a>Выводы

Теперь, когда вы узнали, как внедрять код R в хранимые процедуры, вы можете расширить эти методы для создания собственных моделей. Интеграция с [!INCLUDE[tsql](../../includes/tsql-md.md)] значительно упрощает развертывание моделей R для прогнозирования и включения переобучения моделей в корпоративные рабочие процессы по обработке данных.

## <a name="next-steps"></a>Дальнейшие шаги

Работая с этой статьей, вы выполните следующие задачи:

> [!div class="checklist"]
> + Созданные и используемые хранимые процедуры для пакетной оценки
> + Созданные и используемые хранимые процедуры для оценки одной строки

Дополнительные сведения о языке R см. в статье [Расширение R в SQL Server](../concepts/extension-r.md).