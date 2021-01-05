---
title: Обнаружение и классификация данных SQL | Документы Майкрософт
description: Обнаружение и классификация данных SQL
documentationcenter: ''
ms.reviewer: vanto
ms.assetid: 89c2a155-c2fb-4b67-bc19-9b4e03c6d3bc
ms.service: sql-database
ms.prod_service: sql-database,sql
ms.custom: security
ms.topic: conceptual
ms.date: 06/10/2020
ms.author: datrigan
author: DavidTrigano
ms.openlocfilehash: eab3c943f1191c2d52f6d476c1e0a83b74b6f0df
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97638240"
---
# <a name="sql-data-discovery-and-classification"></a>Обнаружение и классификация данных SQL
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Обнаружение и классификация данных — это новое средство, встроенное в [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md), для **обнаружения**, **классификации**, **пометки** и **создания отчетов** о конфиденциальных данных в базах данных.
Обнаружение и классификация наиболее важных данных (деловых, финансовых, персональных и т. д.) может играть ключевую роль в защите информации в вашей организации. На основе этих процессов может формироваться инфраструктура для решения следующих задач:
* соблюдение стандартов конфиденциальности данных;
* управление доступом к базам данных и столбцам, содержащим конфиденциальные данные, а также усиление их безопасности.

> [!NOTE]
> Средство обнаружения и классификации данных **поддерживается в SQL Server 2012 и более поздних версий и может использоваться с [SSMS 17.5](../../ssms/download-sql-server-management-studio-ssms.md) и более поздних версий**. Сведения, касающиеся Базы данных SQL Azure, см. в статье [Обнаружение и классификация данных в Базе данных SQL Azure](/azure/sql-database/sql-database-data-discovery-and-classification/).

## <a name="overview"></a><a id="subheading-1"></a>Обзор
Средство обнаружения и классификации данных включает в себя набор эффективных служб, которые образуют новую парадигму SQL Information Protection, направленную на защиту данных, а не только базы данных.

* **Обнаружение и рекомендации**. Подсистема классификации проверяет базу данных и определяет столбцы, содержащие потенциально конфиденциальные данные. Затем вы можете легко просмотреть и применить рекомендации по классификации, а также классифицировать столбцы вручную.
* **Метки**. Столбцам можно назначать постоянные метки классификации конфиденциальных данных.
* **Видимость**. Состояние классификации базы данных можно просматривать в подробном отчете, который можно напечатать или экспортировать для использования в целях соблюдения требований или аудита, а также в других целях.

## <a name="discovering-classifying--labeling-sensitive-columns"></a><a id="subheading-2"></a>Обнаружение, классификация конфиденциальных столбцов и назначение им меток
В следующем разделе описываются действия по обнаружению в базе данных столбцов, содержащих конфиденциальные данные, их классификации и назначению им меток, а также просмотру текущего состояния классификации базы данных и экспорту отчетов.

Классификация включает в себя два типа атрибутов метаданных:
* метки — основные атрибуты классификации, которые служат для определения уровня конфиденциальности данных, хранящихся в столбце;  
* типы сведений — предоставляют более подробные сведения о типе данных, хранящихся в столбце.

**Классификация базы данных SQL Server**

1. В SQL Server Management Studio (SSMS) подключитесь к серверу SQL Server.

2. В обозревателе объектов SSMS щелкните правой кнопкой мыши базу данных, которую необходимо классифицировать, и выберите пункты **Задачи** > **Обнаружение и классификация данных** > **Классифицировать данные...** .

   ![Снимок экрана: обозреватель объектов SSMS с выбранным пунктом "Задачи > Обнаружение и классификация данных > Классификация данных...".][0]

3. Подсистема классификации ищет в базе данных столбцы с потенциально конфиденциальными данными и предоставляет список **рекомендованных классификаций столбцов**.

    * Чтобы просмотреть список рекомендованных классификаций столбцов, щелкните поле уведомления о рекомендациях в верхней части окна или панель рекомендаций в его нижней части.

        ![Снимок экрана: уведомление "Найдены столбцы (39) с рекомендациями по классификации. Щелкните здесь, чтобы просмотреть их".][2]

        ![Снимок экрана: уведомление "Найдены столбцы (39) с рекомендациями по классификации (щелкните, чтобы просмотреть)".][3]

    * Просмотрите список рекомендаций.
        * Чтобы применить рекомендацию для определенного столбца, установите флажок в левом поле соответствующей строки. Вы также можете принять *все рекомендации*, установив флажок в заголовке таблицы рекомендаций.

        * Кроме того, вы можете изменить рекомендованные тип сведений и метку конфиденциальности с помощью полей с раскрывающимися списками.        

        ![Снимок экрана со списком рекомендаций.][4]

    * Чтобы применить выбранные рекомендации, нажмите синюю кнопку **Принять выбранные рекомендации**.

        ![Снимок экрана: кнопка "Принять выбранные рекомендации".][5]

4. Вы также можете **классифицировать столбцы вручную** вместо использования рекомендованных классификаций или в дополнение к ним.

    * В меню в верхней части страницы щелкните **Добавить классификацию**.

        ![Снимок экрана: верхнее меню с выделенным пунктом "Добавить классификацию".][6]

    * В открывшемся контекстном окне выберите схему, таблицу и столбец, которые нужно классифицировать, а затем выберите тип сведений и метку классификации. После этого нажмите синюю кнопку **Добавить классификацию** в нижней части контекстного окна.

        ![Снимок экрана: контекстное окно "Добавление классификации".][7]

5. Чтобы завершить классификацию и назначить столбцам базы данных новые метаданные (метки) классификации, щелкните элемент **Сохранить** в меню в верхней части окна.

    ![Снимок экрана: верхнее меню с выделенным пунктом "Сохранить".][8]


6. Чтобы создать отчет с полной сводкой состояния классификации базы данных, в меню в верхней части окна щелкните **Просмотреть отчет**. (Отчет также можно создать с помощью SSMS. Щелкните правой кнопкой мыши базу данных, в которой необходимо создать отчет, и выберите пункты **Задачи** > **Обнаружение и классификация данных** > **Создать отчет...** .)

    ![Снимок экрана: верхнее меню с выделенным пунктом "Просмотреть отчет".][9]

    ![Снимок экрана: отчет по классификации данных SQL.][10]

## <a name="manage-information-protection-policy-with-ssms"></a><a id="subheading-3"></a>Управление политикой защиты информации с помощью SSMS

Управлять политикой защиты информации можно с помощью [SSMS 18.4](../../ssms/download-sql-server-management-studio-ssms.md) или более поздней версии.

1. В SQL Server Management Studio (SSMS) подключитесь к серверу SQL Server.

2. В обозревателе объектов SSMS щелкните правой кнопкой мыши одну из баз данных и выберите пункты **Задачи** > **Обнаружение и классификация данных**.

   Указанные ниже пункты меню позволяют управлять политикой защиты информации.

* **Задать файл политики защиты информации**: используется политика защиты информации, указанная в выбранном файле JSON.

* **Экспорт политики защиты информации**: политика защиты информации экспортируется в файл JSON.

* **Сброс политики защиты информации**: политика защиты информации сбрасывается в политику по умолчанию.

> [!IMPORTANT]
> Файл политики защиты информации не хранится в SQL Server.
> SSMS использует политику защиты информации по умолчанию. В случае сбоя настраиваемой политики защиты информации SSMS не может использовать политику по умолчанию. Происходит сбой классификации данных. Чтобы устранить проблему, выберите пункт **Сброс политики защиты информации** для использования политики по умолчанию и повторного включения классификации данных.

## <a name="accessing-the-classification-metadata"></a><a id="subheading-4"></a>Доступ к метаданным классификации

В SQL Server 2019 появилось представление системного каталога [`sys.sensitivity_classifications`](../system-catalog-views/sys-sensitivity-classifications-transact-sql.md). Это представление возвращает типы информации и метки конфиденциальности. 

> [!NOTE]
> Оно требует разрешения **VIEW ANY SENSITIVITY CLASSIFICATION**. Дополнительные сведения см. в разделе [Metadata Visibility Configuration](./metadata-visibility-configuration.md).

В экземплярах SQL Server 2019 выполните запрос к `sys.sensitivity_classifications`, чтобы просмотреть все классифицированные столбцы с соответствующими классификациями. Пример: 

```sql
SELECT 
    schema_name(O.schema_id) AS schema_name,
    O.NAME AS table_name,
    C.NAME AS column_name,
    information_type,
    label,
    rank,
    rank_desc
FROM sys.sensitivity_classifications sc
    JOIN sys.objects O
    ON  sc.major_id = O.object_id
    JOIN sys.columns C 
    ON  sc.major_id = C.object_id  AND sc.minor_id = C.column_id
```

До версии SQL Server 2019 метаданные классификации для типов информации и меток конфиденциальности хранились в следующих расширенных свойствах: 

* `sys_information_type_name`
* `sys_sensitivity_label_name`

Доступ к метаданным осуществляется с помощью представления каталога расширенных свойств [`sys.extended_properties`](../system-catalog-views/extended-properties-catalog-views-sys-extended-properties.md).

Для экземпляров SQL Server 2017 и более ранних версий следующий пример возвращает все классифицированные столбцы с соответствующими классификациями:

```sql
SELECT
    schema_name(O.schema_id) AS schema_name,
    O.NAME AS table_name,
    C.NAME AS column_name,
    information_type,
    sensitivity_label 
FROM
    (
        SELECT
            IT.major_id,
            IT.minor_id,
            IT.information_type,
            L.sensitivity_label 
        FROM
        (
            SELECT
                major_id,
                minor_id,
                value AS information_type 
            FROM sys.extended_properties 
            WHERE NAME = 'sys_information_type_name'
        ) IT 
        FULL OUTER JOIN
        (
            SELECT
                major_id,
                minor_id,
                value AS sensitivity_label 
            FROM sys.extended_properties 
            WHERE NAME = 'sys_sensitivity_label_name'
        ) L 
        ON IT.major_id = L.major_id AND IT.minor_id = L.minor_id
    ) EP
    JOIN sys.objects O
    ON  EP.major_id = O.object_id 
    JOIN sys.columns C 
    ON  EP.major_id = C.object_id AND EP.minor_id = C.column_id
```

## <a name="manage-classifications"></a><a id="subheading-5"></a>Управление классификациями

# <a name="t-sql"></a>[T-SQL](#tab/t-sql)
Используйте T-SQL, чтобы добавить или удалить классификацию столбца или извлечь все классификации для целой базы данных.

- Добавление и обновление классификации одного или нескольких столбцов: [ADD SENSITIVITY CLASSIFICATION](../../t-sql/statements/add-sensitivity-classification-transact-sql.md)
- Удаление классификации одного или нескольких столбцов: [DROP SENSITIVITY CLASSIFICATION](../../t-sql/statements/drop-sensitivity-classification-transact-sql.md)

# <a name="powershell-cmdlet"></a>[Командлет PowerShell](#tab/sql-powelshell)
С помощью командлетов PowerShell можно добавлять и удалять классификации столбцов, а также извлекать все классификации и получать рекомендации для всей базы данных.

- [Get-SqlSensitivityClassification](/powershell/module/sqlserver/Get-SqlSensitivityClassification)
- [Get-SqlSensitivityRecommendations](/powershell/module/sqlserver/Get-SqlSensitivityRecommendations)
- [Set-SqlSensitivityClassification](/powershell/module/sqlserver/Set-SqlSensitivityClassification)
- [Remove-SqlSensitivityClassification](/powershell/module/sqlserver/Remove-SqlSensitivityClassification)

## <a name="next-steps"></a><a id="subheading-6"></a>Следующие шаги

Сведения, касающиеся Базы данных SQL Azure, см. в статье [Обнаружение и классификация данных в Базе данных SQL Azure](/azure/azure-sql/database/data-discovery-and-classification-overview).

Рекомендуем защитить конфиденциальные столбцы путем применения механизмов защиты на уровне столбцов:

* [динамическое маскирование данных](./dynamic-data-masking.md) для затемнения конфиденциальных столбцов в процессе использования;
* [Always Encrypted](./encryption/always-encrypted-database-engine.md) для шифрования конфиденциальных столбцов в процессе хранения.

<!--Anchors-->
[SQL Data Discovery & Classification overview]: #subheading-1
[Discovering, classifying & labeling sensitive columns]: #subheading-2
[Manage information protection policy with SSMS]: #subheading-3
[Accessing the classification metadata]: #subheading-4
[Manage classifications]: #subheading-5
[Next Steps]: #subheading-6

<!--Image references-->

[0]: ./media/sql-data-discovery-and-classification/0-data-classification-explorer.png
[2]: ./media/sql-data-discovery-and-classification/2-recommendations-notification-box.png
[3]: ./media/sql-data-discovery-and-classification/3-recommendations-panel.png
[4]: ./media/sql-data-discovery-and-classification/4-recommendations.png
[5]: ./media/sql-data-discovery-and-classification/5-accept-recommendations-button.png
[6]: ./media/sql-data-discovery-and-classification/6-add-classification-button.png
[7]: ./media/sql-data-discovery-and-classification/7-manual-classification.png
[8]: ./media/sql-data-discovery-and-classification/8-save.png
[9]: ./media/sql-data-discovery-and-classification/9-view-report.png
[10]: ./media/sql-data-discovery-and-classification/10-report.png