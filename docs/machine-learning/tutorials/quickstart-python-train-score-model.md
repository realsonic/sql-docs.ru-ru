---
title: Краткое руководство. Обучение модели в Python
titleSuffix: SQL machine learning
description: В этом кратком руководстве вы создадите и обучите прогнозную модель с помощью Python. Затем вы сохраните модель в таблице базы данных и примените эту модель для прогнозирования значений на основе новых данных с помощью машинного обучения SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 09/28/2020
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 6b5d59568f68894b3691287cd581f390101f0b02
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101952"
---
# <a name="quickstart-create-and-score-a-predictive-model-in-python-with-sql-machine-learning"></a>Краткое руководство. Создание и оценка прогнозной модели в Python с помощью машинного обучения SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

В этом кратком руководстве вы создадите и обучите прогнозную модель с помощью Python. Затем вы сохраните модель в таблицу в экземпляре SQL Server и примените эту модель для прогнозирования значений на основе новых данных с помощью [Служб машинного обучения SQL Server](../sql-server-machine-learning-services.md), [Служб машинного обучения управляемого экземпляра SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview) или [Кластеров больших данных SQL Server](../../big-data-cluster/machine-learning-services.md).

Вы создадите и запустите две хранимые процедуры, выполняемые в SQL. В первой из них используется классический набор данных Iris и создается модель упрощенного алгоритма Байеса для прогнозирования вида ирисов на основе характеристик цветка. Вторая процедура предназначена для оценки — она вызывает модель, созданную в первой процедуре, для вывода набора прогнозов на основе новых данных. Поместив код Python в хранимую процедуру SQL, вы переносите операции в SQL, благодаря чему они могут многократно использоваться и вызываться другими хранимыми процедурами и клиентскими приложениями.

Выполнив это краткое руководство, вы узнаете, как делать следующее.

> [!div class="checklist"]
> - Внедрение кода Python в хранимую процедуру
> - Передача входных данных в код с помощью входных данных хранимой процедуры
> - Использование хранимых процедур для эксплуатации моделей

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим кратким руководством необходимо следующее.

- База данных SQL на одной из следующих платформ:
  - [Службы машинного обучения SQL Server](../sql-server-machine-learning-services.md). Сведения об установке см. в [руководстве по установке для Windows](../install/sql-machine-learning-services-windows-install.md) или [руководстве по установке для Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json).
  - Кластеры больших данных SQL Server. [Применение служб машинного обучения в кластерах больших данных SQL Server](../../big-data-cluster/machine-learning-services.md).
  - Службы машинного обучения в управляемом экземпляре SQL Azure. Дополнительные сведения см. в статье [Общие сведения о службах машинного обучения в управляемом экземпляре SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview).

- Средство для выполнения SQL-запросов, содержащих сценарии Python. В этом кратком руководстве используется [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

- В качестве примера данных в этом упражнении используется образец базы данных Iris. Следуйте инструкциям в разделе [Демонстрационные данные Iris](demo-data-iris-in-sql.md), чтобы создать образец базы данных **irissql**.

## <a name="create-a-stored-procedure-that-generates-models"></a>Создание хранимой процедуры, которая порождает модели

На этом шаге вы создадите хранимую процедуру, которая порождает модель для прогнозирования результатов.

1. Откройте Azure Data Studio, подключитесь к своему экземпляру SQL и откройте новое окно запроса.

1. Подключитесь к базе данных irissql.

    ```sql
    USE irissql
    GO
    ```

1. Скопируйте приведенный ниже код, чтобы создать новую хранимую процедуру.

   При выполнении эта процедура вызывает [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) для запуска сеанса Python. 

   Входные данные, необходимые для кода Python, передаются в качестве входных параметров для этой хранимой процедуры. Выходные данные будут обучены на основе библиотеки Python **scikit-learn**, содержащей алгоритмы машинного обучения.

   В этом коде для сериализации модели используется [**pickle**](https://docs.python.org/2/library/pickle.html). Модель будет обучена с использованием данных из столбцов 0–4 в таблице **iris_data**. 

   Параметры во второй части процедуры относятся к вводу данных и выходным данным модели. Рекомендуется добиваться, чтобы код Python, выполняемый в хранимой процедуре, имел четко определенные входные и выходные данные, сопоставленные с входными и выходными данными хранимой процедуры, передаваемыми во время выполнения.

    ```sql
    CREATE PROCEDURE generate_iris_model (@trained_model VARBINARY(max) OUTPUT)
    AS
    BEGIN
        EXECUTE sp_execute_external_script @language = N'Python'
            , @script = N'
    import pickle
    from sklearn.naive_bayes import GaussianNB
    GNB = GaussianNB()
    trained_model = pickle.dumps(GNB.fit(iris_data[["Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width"]], iris_data[["SpeciesId"]].values.ravel()))
    '
            , @input_data_1 = N'select "Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "SpeciesId" from iris_data'
            , @input_data_1_name = N'iris_data'
            , @params = N'@trained_model varbinary(max) OUTPUT'
            , @trained_model = @trained_model OUTPUT;
    END;
    GO
    ```

1. Убедитесь, что хранимая процедура существует. 

   Если скрипт T-SQL из предыдущего шага выполнился без ошибок, создается новая хранимая процедура с именем **generate_iris_model**, которая добавляется в базу данных **irissql**. Хранимые процедуры можно найти в **обозревателе объектов** Azure Data Studio в разделе **Программирование**.

## <a name="execute-the-procedure-to-create-and-train-models"></a>Выполнение процедуры для создания и обучения моделей

На этом шаге выполняется процедура запуска внедренного кода с созданием обученной и сериализованной модели в качестве выходных данных. 

Модели, хранимые для повторного использования в базе данных, сериализуются как поток байтов и хранятся в столбце VARBINARY(MAX) в таблице базы данных. После того как модель создана, обучена, сериализована и сохранена в базе данных, она может быть вызвана другими процедурами или функцией [PREDICT T-SQL](../../t-sql/queries/predict-transact-sql.md) для оценки рабочих нагрузок.

1. Чтобы выполнить процедуру, выполните следующий скрипт. Конкретная инструкция для исполнения хранимой процедуры — `EXECUTE` в четвертой строке.

   Этот скрипт удаляет существующую модель с тем же именем ("Naive Bayes"), чтобы освободить место для новой, созданной путем выполнения той же процедуры. Без удаления модели возникает ошибка, сообщающая, что объект уже существует. Модель хранится в таблице с именем **iris_models**, которая будет подготовлена при создании базы данных **irissql**.

    ```sql
    DECLARE @model varbinary(max);
    DECLARE @new_model_name varchar(50)
    SET @new_model_name = 'Naive Bayes'
    EXECUTE generate_iris_model @model OUTPUT;
    DELETE iris_models WHERE model_name = @new_model_name;
    INSERT INTO iris_models (model_name, model) values(@new_model_name, @model);
    GO
    ```

1. Убедитесь, что модель вставлена.

    ```sql
    SELECT * FROM dbo.iris_models
    ```

    **Результаты**

    | model_name  | model |
    |---|-----------------|
    | упрощенный алгоритм Байеса | 0x800363736B6C6561726E2E6E616976655F62617965730A... | 

## <a name="create-and-execute-a-stored-procedure-for-generating-predictions"></a>Создание и выполнение хранимой процедуры для создания прогнозов

Теперь, когда модель создана, обучена и сохранена, переходите к следующему шагу: создание хранимой процедуры, дающей прогнозы. Это делается путем вызова `sp_execute_external_script` для запуска скрипта Python, который загружает сериализованную модель и передает ей новые входные данные для оценки.

1. Выполните следующий код, чтобы создать хранимую процедуру, которая выполняет оценку. При выполнении эта процедура позволяет загрузить двоичную модель, использовать столбцы `[1,2,3,4]` в качестве входных данных и указать столбцы `[0,5,6]` в качестве выходных данных.

   ```sql
   CREATE PROCEDURE predict_species (@model VARCHAR(100))
   AS
   BEGIN
       DECLARE @nb_model VARBINARY(max) = (
               SELECT model
               FROM iris_models
               WHERE model_name = @model
               );
   
       EXECUTE sp_execute_external_script @language = N'Python'
           , @script = N'
   import pickle
   irismodel = pickle.loads(nb_model)
   species_pred = irismodel.predict(iris_data[["Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width"]])
   iris_data["PredictedSpecies"] = species_pred
   OutputDataSet = iris_data[["id","SpeciesId","PredictedSpecies"]] 
   print(OutputDataSet)
   '
           , @input_data_1 = N'select id, "Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "SpeciesId" from iris_data'
           , @input_data_1_name = N'iris_data'
           , @params = N'@nb_model varbinary(max)'
           , @nb_model = @nb_model
       WITH RESULT SETS((
                   "id" INT
                 , "SpeciesId" INT
                 , "SpeciesId.Predicted" INT
                   ));
   END;
   GO
   ```

2. Выполните хранимую процедуру, указав имя модели "Naive Bayes" (Упрощенный алгоритм Байеса), чтобы процедура знала, какая модель будет использоваться.

   ```sql
   EXECUTE predict_species 'Naive Bayes';
   GO
   ```

   При выполнении хранимой процедуры она возвращает data.frame из Python. Эта строка T-SQL указывает схему возвращаемых результатов: `WITH RESULT SETS ( ("id" int, "SpeciesId" int, "SpeciesId.Predicted" int));`. Результаты можно вставить в новую таблицу или вернуть в приложение.

   ![Результирующий набор из выполняемой хранимой процедуры](media/train-score-using-python-NB-model-results.png)

   Результаты представляют собой 150 прогнозов о видах цветов, основанных на характеристиках цветка, поданных в качестве входных данных. Для большинства наблюдений прогнозируемый вид цветов соответствует реальному.

   Этот пример был упрощен благодаря применению набору данных Iris в Python для обучения и оценки. Более распространенный подход заключается в выполнении SQL-запроса для получения новых данных и их передачи в Python как `InputDataSet`.

## <a name="conclusion"></a>Заключение

В этом упражнении вы узнали, как создавать хранимые процедуры, предназначенные для различных задач, где каждая хранимая процедура использует системную хранимую процедуру `sp_execute_external_script` для запуска процесса Python. Входные данные для процесса Python передаются в `sp_execute_external` в качестве параметров. Как сам скрипт Python, так и переменные данных в базе данных передаются в качестве входных параметров.

Как правило, следует планировать использование Azure Data Studio только с готовым кодом Python или очень простым кодом Python, который возвращает выходные данные на основе строк. В качестве инструментов Azure Data Studio поддерживает языки запросов, такие как T-SQL, и возвращает плоские наборы строк. Если код создает визуальные выходные данные, такие как точечные диаграммы или гистограммы, вам потребуется отдельное средство или приложение для конечных пользователей, способное визуализировать изображение вне хранимой процедуры.

Для некоторых разработчиков на Python, которые привыкли писать всеобъемлющие скрипты, выполняющие целый ряд операций, выделение задач в отдельные процедуры может показаться лишним. Но обучение и оценка имеют разные варианты использования. Разделив их, можно запланировать каждую задачу по отдельному расписанию и задать разрешения области для каждой операции отдельно.

Последнее преимущество заключается в том, что процессы можно изменять с помощью параметров. В этом упражнении код на Python, который создал модель (под названием "Naive Bayes" в этом примере), был передан в качестве входных данных для второй хранимой процедуры, вызывающей модель в процессе оценки. В этом упражнении используется только одна модель, но вы можете представить, как параметризация модели в задаче оценки сделает этот скрипт более полезным.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об учебниках по использованию Python и машинного обучения SQL:

- [Учебники по Python](python-tutorials.md)