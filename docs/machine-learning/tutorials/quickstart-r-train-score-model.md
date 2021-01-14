---
title: Краткое руководство. Обучение модели в R
titleSuffix: SQL machine learning
description: В этом кратком руководстве вы создадите и обучите прогнозную модель с помощью T. Затем вы сохраните эту модель в таблицу и примените ее для прогнозирования значений на основе новых данных с помощью машинного обучения SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/23/2020
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 21e815a95ebc6a407749195323748d6c73c19e00
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100176"
---
# <a name="quickstart-create-and-score-a-predictive-model-in-r-with-sql-machine-learning"></a>Краткое руководство. Создание и оценка прогнозной модели в R с помощью машинного обучения SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
В этом кратком руководстве вы создадите и обучите прогнозную модель с помощью T. Затем вы сохраните ее в таблицу в экземпляре SQL Server и примените эту модель для прогнозирования значений на основе новых данных с помощью [Служб машинного обучения SQL Server](../sql-server-machine-learning-services.md) в [кластерах больших данных](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
В этом кратком руководстве вы создадите и обучите прогнозную модель с помощью T. Затем вы сохраните модель в таблицу в экземпляре SQL Server и примените эту модель для прогнозирования значений на основе новых данных с помощью [Служб машинного обучения SQL Server](../sql-server-machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2016"
В этом кратком руководстве вы создадите и обучите прогнозную модель с помощью T. Затем вы сохраните ее в таблицу в экземпляре SQL Server и примените эту модель для прогнозирования значений на основе новых данных с помощью служб [SQL Server R Services](../r/sql-server-r-services.md).
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
В этом кратком руководстве вы создадите и обучите прогнозную модель с помощью T. Затем вы сохраните модель в таблице в экземпляре SQL Server и примените эту модель для прогнозирования значений на основе новых данных с помощью [Служб машинного обучения в Управляемом экземпляре SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

Вы создадите и запустите две хранимые процедуры, выполняемые в SQL. В первой из них используется набор данных **mtcars**, входящий в состав R, на основе которого создается простая обобщенная линейная модель (ОЛМ), которая прогнозирует вероятность оснащения автомобиля механической КПП. Вторая процедура предназначена для оценки — она вызывает модель, созданную в первой процедуре, для вывода набора прогнозов на основе новых данных. Поместив код R в хранимую процедуру SQL, вы переносите операции в SQL, благодаря чему они могут многократно использоваться и вызываться другими хранимыми процедурами и клиентскими приложениями.

> [!TIP]
> Если вам требуется средство обновления линейных моделей, ознакомьтесь со следующим руководством, в котором описывается процесс компоновки моделей с помощью функции rxLinMod: [Fitting Linear Models using RevoScaleR](/machine-learning-server/r/how-to-revoscaler-linear-model) (Подготовка линейных моделей с помощью RevoScaleR)

Выполнив это краткое руководство, вы узнаете, как делать следующее.

> [!div class="checklist"]
> - Внедрение кода R в хранимую процедуру
> - Передача входных данных в код с помощью входных данных хранимой процедуры
> - Использование хранимых процедур для эксплуатации моделей

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

## <a name="create-the-model"></a>Создание модели

Чтобы создать модель, вам необходимо сформировать исходные данные для обучения, создать модель и обучить ее с использованием этих данных, а затем сохранить эту модель в базе данных, где она будет использоваться для создания прогнозов на основе новых данных.

### <a name="create-the-source-data"></a>Создание исходных данных

1. Откройте Azure Data Studio, подключитесь к своему экземпляру и откройте новое окно запроса.

1. Создайте таблицу, чтобы сохранить данные для обучения.

   ```sql
   CREATE TABLE dbo.MTCars(
       mpg decimal(10, 1) NOT NULL,
       cyl int NOT NULL,
       disp decimal(10, 1) NOT NULL,
       hp int NOT NULL,
       drat decimal(10, 2) NOT NULL,
       wt decimal(10, 3) NOT NULL,
       qsec decimal(10, 2) NOT NULL,
       vs int NOT NULL,
       am int NOT NULL,
       gear int NOT NULL,
       carb int NOT NULL
   );
   ```

1. Вставьте данные из встроенного набора данных `mtcars`.

   ```SQL
   INSERT INTO dbo.MTCars
   EXEC sp_execute_external_script @language = N'R'
       , @script = N'MTCars <- mtcars;'
       , @input_data_1 = N''
       , @output_data_1_name = N'MTCars';
   ```

   > [!TIP]
   > Многие наборы данных, малые и большие, включаются в среду выполнения R. Чтобы получить список наборов данных, установленных с R, введите `library(help="datasets")` в командной строке R.

### <a name="create-and-train-the-model"></a>Создание и обучение модели

Данные о скорости автомобиля содержат два численных столбца — мощность в лошадиных силах (`hp`) и масса (`wt`). На основе этих данных вы создадите обобщенную линейную модель (ОЛМ), которая оценивает вероятность того, что автомобиль оснащен механической КПП.

Чтобы создать модель, необходимо определить формулу внутри кода R, а затем передать данные в качестве входных параметров.

```sql
DROP PROCEDURE IF EXISTS generate_GLM;
GO
CREATE PROCEDURE generate_GLM
AS
BEGIN
    EXEC sp_execute_external_script
    @language = N'R'
    , @script = N'carsModel <- glm(formula = am ~ hp + wt, data = MTCarsData, family = binomial);
        trained_model <- data.frame(payload = as.raw(serialize(carsModel, connection=NULL)));'
    , @input_data_1 = N'SELECT hp, wt, am FROM MTCars'
    , @input_data_1_name = N'MTCarsData'
    , @output_data_1_name = N'trained_model'
    WITH RESULT SETS ((model VARBINARY(max)));
END;
GO
```

- Первый аргумент для функции `glm` — это параметр *formula*, где дистанция (`am`) является значением, зависимым от скорости (`hp + wt`).
- Входные данные хранятся в переменной `MTCarsData`, которая заполняется с помощью SQL-запроса. Если для входных данных не назначено определенное имя, имя переменной по умолчанию будет _InputDataSet_.

### <a name="store-the-model-in-the-database"></a>Сохранение модели в базе данных

Далее необходимо сохранить модель в базе данных, где ее можно будет использовать для прогнозирования или повторного обучения. 

1. Создайте таблицу для хранения модели.

   Выходные данные пакета R, с помощью которого создается модель, обычно являются двоичным объектом. Это означает, что таблица, в которой хранится модель, должна содержать столбец типа **varbinary(max)** .

   ```sql
   CREATE TABLE GLM_models (
       model_name varchar(30) not null default('default model') primary key,
       model varbinary(max) not null
   );
   ```

1. Выполните приведенную ниже инструкцию Transact-SQL, чтобы вызвать хранимую процедуру, создать модель и сохранить ее в созданной таблице.

   ```sql
   INSERT INTO GLM_models(model)
   EXEC generate_GLM;
   ```

   > [!TIP]
   > Если вы выполните этот код во второй раз, появится следующая ошибка: "Нарушение ограничения первичного ключа... Невозможно вставить повторяющийся ключ в объект dbo.stopping_distance_models". Один из вариантов, который поможет избежать этой ошибки, — обновить имя для каждой новой модели. Например, можно изменить имя на нечто более описательное и включить тип модели, день создания и т. д.

     ```sql
     UPDATE GLM_models
     SET model_name = 'GLM_' + format(getdate(), 'yyyy.MM.HH.mm', 'en-gb')
     WHERE model_name = 'default model'
     ```

## <a name="score-new-data-using-the-trained-model"></a>Оценка новых данных с использованием обученной модели

По *оценкой* в области обработки и анализа данных понимается вычисление прогнозов, вероятностей и других значений на основе данных, подаваемых в обученную модель. Для оценки прогнозов на основе новых данных вы будете использовать модель, созданную в предыдущем разделе.

### <a name="create-a-table-of-new-data"></a>Создание таблицы с новыми данными

Для начала создайте таблицу с новыми данными.

```sql
CREATE TABLE dbo.NewMTCars(
    hp INT NOT NULL
    , wt DECIMAL(10,3) NOT NULL
    , am INT NULL
)
GO

INSERT INTO dbo.NewMTCars(hp, wt)
VALUES (110, 2.634)

INSERT INTO dbo.NewMTCars(hp, wt)
VALUES (72, 3.435)

INSERT INTO dbo.NewMTCars(hp, wt)
VALUES (220, 5.220)

INSERT INTO dbo.NewMTCars(hp, wt)
VALUES (120, 2.800)
GO
```

### <a name="predict-manual-transmission"></a>Прогнозирование вероятности оснащения автомобиля механической КПП

Чтобы получить прогнозы на основе вашей модели, необходимо написать скрипт SQL, который выполняет следующие функции:

1. Получение требуемой модели.
1. Получение новых входных данных.
1. Вызов функции прогнозирования R, совместимой с моделью.

Со временем ваша таблица может содержать несколько моделей R, созданных с помощью различных параметров и алгоритмов или обученных на основе разных подмножеств данных. В этом примере мы будем использовать модель `default model`.

```sql
DECLARE @glmmodel varbinary(max) = 
    (SELECT model FROM dbo.GLM_models WHERE model_name = 'default model');

EXEC sp_execute_external_script
    @language = N'R'
    , @script = N'
            current_model <- unserialize(as.raw(glmmodel));
            new <- data.frame(NewMTCars);
            predicted.am <- predict(current_model, new, type = "response");
            str(predicted.am);
            OutputDataSet <- cbind(new, predicted.am);
            '
    , @input_data_1 = N'SELECT hp, wt FROM dbo.NewMTCars'
    , @input_data_1_name = N'NewMTCars'
    , @params = N'@glmmodel varbinary(max)'
    , @glmmodel = @glmmodel
WITH RESULT SETS ((new_hp INT, new_wt DECIMAL(10,3), predicted_am DECIMAL(10,3)));
```

Приведенный выше скрипт выполняет следующие действия:

- Используйте инструкцию SELECT для получения одной модели из таблицы и передайте ее в качестве входного параметра.

- После извлечения модели из таблицы вызовите функцию `unserialize` для модели.

- Примените функцию `predict` с соответствующими аргументами к модели и предоставьте новые входные данные.

> [!NOTE]
> В этом примере на этапе тестирования мы добавили функцию `str` для проверки схемы данных, возвращаемой из кода R. В дальнейшем вы можете удалить эту инструкцию.
>
> Имена столбцов, используемые в скрипте R, не обязательно передаются в выходные данные хранимой процедуры. В этом случае предложение WITH RESULTS используется для определения новых имен столбцов.

**Результаты**

![Результирующий набор для прогнозирования вероятности оснащения автомобиля механической КПП](./media/r-predict-am-resultset.png)

Кроме того, вы можете использовать инструкцию [PREDICT (Transact-SQL)](../../t-sql/queries/predict-transact-sql.md) для получения прогнозируемого значения или оценки на основе сохраненной модели.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об учебниках по использованию R и машинного обучения SQL:

- [Учебники по R](r-tutorials.md)