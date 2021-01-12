---
title: Подключение и отправка запросов к Azure Synapse Analytics
description: В этом кратком руководстве показано, как подключиться к выделенному пулу SQL в Azure Synapse Analytics с помощью Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: quickstart
author: yualan
ms.author: alayu
ms.reviewer: maghan, jrasnick
ms.custom: seodec18; seo-lt-2019
ms.date: 10/15/2020
ms.openlocfilehash: 8215e84c4c61935b3f5831ca771e2868e27be428
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091727"
---
# <a name="quickstart-use-azure-data-studio-to-connect-and-query-data-using-a-dedicated-sql-pool-in-azure-synapse-analytics"></a>Краткое руководство. Использование Azure Data Studio для подключения и запрашивания данных с помощью выделенного пула SQL в Azure Synapse Analytics

В этом кратком руководстве показано, как подключиться к выделенному пулу SQL в Azure Synapse Analytics с помощью Azure Data Studio.

## <a name="prerequisites"></a>Предварительные требования
Для работы с этим кратким руководством потребуется Azure Data Studio, выделенный пул SQL и Azure Synapse Analytics.

- [Установите Azure Data Studio](./download-azure-data-studio.md).

Если у вас еще нет выделенного пула SQL, см. статью [Создание выделенного пула SQL](/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision).

Запомните имя сервера и учетные данные для входа.


## <a name="connect-to-your-dedicated-sql-pool"></a>Подключение к выделенному пулу SQL

С помощью Azure Data Studio установите подключение к Azure Synapse Analytics.

1. При первом запуске Azure Data Studio должна открыться страница **подключения**. Если страница **Подключение** не отображается, щелкните **Добавить подключение** или значок **Создать подключение** на боковой панели **Серверы**.
   
   ![Снимок экрана: страница "Соединение" с выделенным значком "Создать подключение".](media/quickstart-sql-dw/new-connection-icon.png)

2. В этой статье используется *имя входа SQL*, но также поддерживается *проверка подлинности Windows*. Заполните поля, указав имя сервера, имя пользователя и пароль для *вашего* сервера SQL Azure.

   |   Параметр    | Рекомендуемое значение | Описание |
   |--------------|-----------------|-------------| 
   | **Имя сервера** | Полное имя сервера | Например, имя должно выглядеть следующим образом: **sqlpoolservername.database.windows.net**. |
   | **Аутентификация** | Имя входа SQL| В этом руководстве используется проверка подлинности SQL. |
   | **User name** | Учетная запись администратора сервера | Это учетная запись, которая была указана при создании сервера. |
   | **Пароль (имя входа SQL)** | Пароль для учетной записи администратора сервера | Это пароль, указанный при создании сервера. |
   | **Сохранить пароль?** | "Да" или "Нет". | Чтобы не вводить пароль каждый раз, выберите Да. |
   | **Имя базы данных** | *Оставьте пустым* | Имя базы данных, с которой необходимо установить соединение. |
   | **Группа серверов** | Выберите <Default> | Если вы создали группу серверов, можно выбрать ее. | 

3. Если на сервере не настроено правило брандмауэра, разрешающее подключение Azure Data Studio, откроется форма **Создание нового правила брандмауэра**. Заполните форму, чтобы создать правило брандмауэра. Подробные сведения см. в разделе [Правила брандмауэра](/azure/sql-database/sql-database-firewall-configure).

4. После успешного подключения сервер откроется на боковой панели *Серверы*.

## <a name="create-a-database-in-your-dedicated-sql-pool"></a>Создание базы данных в выделенном пуле SQL

1. В обозревателе объектов щелкните сервер правой кнопкой мыши и выберите **Создать запрос**.

2. Вставьте в редакторе запросов следующий фрагмент и нажмите кнопку **Выполнить**.

   ```sql
    IF NOT EXISTS (
       SELECT name
       FROM sys.databases
       WHERE name = N'TutorialDB'
    )
    CREATE DATABASE [TutorialDB] (EDITION = 'datawarehouse', SERVICE_OBJECTIVE='DW100');
    GO  
    
    ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
    GO
   ```

## <a name="create-a-table"></a>Создание таблицы

Редактор запросов все еще подключен к базе данных *master*, но нам нужно создать таблицу в базе данных *TutorialDB*. 

1. Измените контекст подключения на **TutorialDB**.

2. Вставьте в редакторе запросов следующий фрагмент и нажмите кнопку **Выполнить**.

   > [!NOTE]
   > Этот фрагмент можно добавить в предыдущий запрос либо можно перезаписать предыдущий запрос в редакторе. Обратите внимание, что при нажатии кнопки **Выполнить** выполняется только выбранный запрос. Если ничего не выбрано, при нажатии кнопки **Выполнить** выполняются все запросы в редакторе.

   ```sql
   -- Create a new table called 'Customers' in schema 'dbo'
   -- Drop the table if it already exists
   IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
   DROP TABLE dbo.Customers
   GO
   -- Create the table in the specified schema
   CREATE TABLE dbo.Customers
   (
      CustomerId        INT     NOT NULL,
      Name      [NVARCHAR](50)  NOT NULL,
      Location  [NVARCHAR](50)  NOT NULL,
      Email     [NVARCHAR](50)  NOT NULL
   );
   GO
   ```

    :::image type="content" source="media/quickstart-sql-dw/create-table.png" alt-text="Создание таблицы в базе данных TutorialDB":::


## <a name="insert-rows"></a>Вставка строк

1. Вставьте в редакторе запросов следующий фрагмент и нажмите кнопку **Выполнить**.

   ```sql
   -- Insert rows into table 'Customers'
   INSERT INTO dbo.Customers
      ([CustomerId],[Name],[Location],[Email])
      SELECT 1, N'Orlando',N'Australia', N'' UNION ALL
      SELECT 2, N'Keith', N'India', N'keith0@adventure-works.com' UNION ALL
      SELECT 3, N'Donna', N'Germany', N'donna0@adventure-works.com' UNION ALL
      SELECT 4, N'Janet', N'United States', N'janet1@adventure-works.com'
   ```

    :::image type="content" source="media/quickstart-sql-dw/create-rows.png" alt-text="Создание строк в таблице":::

## <a name="view-the-result"></a>Просмотр результата

1. Вставьте в редакторе запросов следующий фрагмент и нажмите кнопку **Выполнить**.

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

2. Будут отображены результаты запроса:

    :::image type="content" source="media/quickstart-sql-dw/view-results.png" alt-text="Просмотр результатов":::


## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы не планируете продолжать работу с примерами баз данных, созданными для работы с этой статьей, [удалите группу ресурсов](/azure/synapse-analytics/sql-data-warehouse/create-data-warehouse-portal#clean-up-resources).

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения см. в статье [Подключение к Synapse SQL с помощью Azure Data Studio (предварительная версия)](/azure/synapse-analytics/sql/get-started-azure-data-studio).

После успешного подключения к Azure Synapse Analytics и выполнения запроса можно перейти к [руководству по редактору кода](tutorial-sql-editor.md).