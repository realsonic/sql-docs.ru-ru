---
title: Подключение и отправка запроса к базе данных SQL Azure
description: Выполните краткое руководство, в котором вы используете Azure Data Studio для подключения к серверу базы данных SQL Azure, а затем создадите базу данных и отправите к ней запрос.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.reviewer: alayu; maghan; sstein
ms.topic: quickstart
author: yualan
ms.author: alayu
ms.custom: seodec18; sqlfreshmay19; seo-lt-2019
ms.date: 05/14/2019
ms.openlocfilehash: c168ceb916ac1b65f4e6d45c9ee1054b15b0cb75
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637661"
---
# <a name="quickstart-use-azure-data-studio-to-connect-and-query-azure-sql-database"></a>Краткое руководство. Использование Azure Data Studio для подключения и обращения к базе данных SQL Azure

В этом кратком руководстве вы будете использовать Azure Data Studio для подключения к серверу базы данных SQL Azure. Затем вы с помощью инструкций Transact-SQL (T-SQL) создадите базу данных TutorialDB, применяемую в других руководствах по Azure Data Studio, и отправите к ней запрос.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим кратким руководством потребуется Azure Data Studio и сервер Базы данных SQL Azure.

- [Установите Azure Data Studio](./download-azure-data-studio.md).

Если у вас нет сервера SQL Azure, выполните одно из следующих кратких руководств по базе данных SQL Azure. Запомните полное имя сервера и учетные данные для входа для последующих шагов.

- [Создание базы данных — портал](/azure/sql-database/sql-database-get-started-portal)
- [Создание базы данных — CLI](/azure/sql-database/sql-database-get-started-cli)
- [Создание базы данных — PowerShell](/azure/sql-database/sql-database-get-started-powershell)


## <a name="connect-to-your-azure-sql-database-server"></a>Подключение к серверу базы данных SQL Azure

С помощью Azure Data Studio установите подключение к серверу Базы данных SQL Azure.

1. При первом запуске Azure Data Studio должна открыться страница **приветствия**. Если вы не видите **страницу приветствия** , выберите **Справка** > **Добро** пожаловать. Выберите **Создать подключение**, чтобы открыть панель **Подключение**.
   
   ![Снимок экрана: диалоговое окно приветствия Azure Delta Studio с выделенным пунктом "Создать подключение".](media/quickstart-sql-database/new-connection-icon.png)

2. В этой статье используется вход SQL, но также поддерживается проверка подлинности Windows. Заполните следующие поля, указав имя сервера, имя пользователя и пароль для вашего сервера SQL Azure.

   | Параметр       | Рекомендуемое значение | Описание |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Имя сервера** | Полное имя сервера | Что-то вроде: **имя_сервера.база_данных.windows.net**. |
   | **Аутентификация** | Имя входа SQL| В этом руководстве используется проверка подлинности SQL. |
   | **User name** | Имя пользователя для учетной записи администратора сервера | Имя пользователя из учетной записи, использованной для создания сервера. |
   | **Пароль (имя входа SQL)** | Пароль для учетной записи администратора сервера | Пароль из учетной записи, использованной для создания сервера. |
   | **Сохранить пароль?** | "Да" или "Нет". | Чтобы не вводить пароль каждый раз, выберите **Да**. |
   | **Имя базы данных** | *Оставьте пустым* | Здесь вы просто подключаетесь к серверу. |
   | **Группа серверов** | Выберите <Default> | В этом поле можно задать определенную группу серверов, которую вы создали. | 

   ![Снимок экрана: страница "Подключение" в Azure Data Studio.](media/quickstart-sql-database/new-connection-screen.png)  

3. Выберите **Подключиться**.

4. Если на сервере не настроено правило брандмауэра, разрешающее подключение Azure Data Studio, откроется форма **Создание нового правила брандмауэра**. Заполните форму, чтобы создать правило брандмауэра. Подробные сведения см. в разделе [Правила брандмауэра](/azure/sql-database/sql-database-firewall-configure).

   ![Новое правило брандмауэра](media/quickstart-sql-database/firewall.png)  

После успешного подключения сервер откроется на боковой панели **Серверы**.

## <a name="create-the-tutorial-database"></a>Создание учебной базы данных

В следующих разделах создается база данных TutorialDB, используемая в других руководствах по Azure Data Studio.

1. Щелкните сервер SQL Azure в боковой панели **Серверы** правой кнопкой мыши и выберите **Создать запрос**.

1. Вставьте этот код SQL в редактор запросов.

   ```sql
   IF NOT EXISTS (
      SELECT name
      FROM sys.databases
      WHERE name = N'TutorialDB'
   )
   CREATE DATABASE [TutorialDB]
   GO

   ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
   GO
   ```

1. На панели инструментов выберите **Выполнить**. На панели **Сообщения** отображаются уведомления о ходе выполнения запроса.

## <a name="create-a-table"></a>Создание таблицы

Редактор запросов подключен к базе данных **master**, но нам нужно создать таблицу в базе данных **TutorialDB**. 

1. Подключитесь к базе данных **TutorialDB**.

   ![Изменение контекста](media/quickstart-sql-database/change-context2.png)



1. Создайте таблицу `Customers`. 

   Замените предыдущий запрос в редакторе запросов на этот и выберите **Выполнить**.

   ```sql
   -- Create a new table called 'Customers' in schema 'dbo'
   -- Drop the table if it already exists
   IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
   DROP TABLE dbo.Customers
   GO
   -- Create the table in the specified schema
   CREATE TABLE dbo.Customers
   (
      CustomerId        INT    NOT NULL   PRIMARY KEY, -- primary key column
      Name      [NVARCHAR](50)  NOT NULL,
      Location  [NVARCHAR](50)  NOT NULL,
      Email     [NVARCHAR](50)  NOT NULL
   );
   GO
   ```


## <a name="insert-rows-into-the-table"></a>Вставка строк в таблицу

Замените предыдущий запрос на этот и выберите **Выполнить**.

   ```sql
   -- Insert rows into table 'Customers'
   INSERT INTO dbo.Customers
      ([CustomerId],[Name],[Location],[Email])
   VALUES
      ( 1, N'Orlando', N'Australia', N''),
      ( 2, N'Keith', N'India', N'keith0@adventure-works.com'),
      ( 3, N'Donna', N'Germany', N'donna0@adventure-works.com'),
      ( 4, N'Janet', N'United States', N'janet1@adventure-works.com')
   GO
   ```

## <a name="view-the-result"></a>Просмотр результата

Замените предыдущий запрос на этот и выберите **Выполнить**.

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

Отображаются результаты запроса.

   ![Выбор результатов](media/quickstart-sql-database/select-results2.png)


## <a name="clean-up-resources"></a>Очистка ресурсов

Последующие статьи краткого руководства основываются на созданных здесь ресурсах. Если вы планируете работать с этими статьями, не удаляйте эти ресурсы. В противном случае удалите ненужные ресурсы на портале Azure. Подробные сведения см. в разделе [Очистка ресурсов](/azure/sql-database/sql-database-get-started-portal#clean-up-resources).

## <a name="next-steps"></a>Дальнейшие действия

После успешного подключения к базе данных SQL Azure и выполнения запроса можно перейти к [руководству по редактору кода](tutorial-sql-editor.md).