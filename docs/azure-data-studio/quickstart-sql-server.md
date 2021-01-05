---
title: Краткое руководство. Подключение и отправка запроса к SQL Server
description: Выполните краткое руководство, в котором вы используете Azure Data Studio для подключения к SQL Server, а затем с помощью инструкций Transact-SQL (T-SQL) создадите базу данных.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: quickstart
author: yualan
ms.author: alayu
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18, sqlfreshmay19
ms.date: 08/02/2019
ms.openlocfilehash: 6a637791eac7858e905f766126ca93d9277b65c1
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637630"
---
# <a name="quickstart-use-azure-data-studio-to-connect-and-query-sql-server"></a>Краткое руководство. Использование Azure Data Studio для подключения и запросов к SQL Server

В этом кратком руководстве показано, как использовать Azure Data Studio для подключения к SQL Server, а затем с помощью инструкций Transact-SQL (T-SQL) создать базу данных *TutorialDB*, которая используется в руководствах по Azure Data Studio.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим кратким руководством требуется Azure Data Studio и доступ к SQL Server.

- [Установите Azure Data Studio](./download-azure-data-studio.md).

Если у вас нет доступа к SQL Server, выберите платформу по следующим ссылкам (обязательно запомните имя для входа и пароль SQL):

- [Windows — скачать выпуск SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads)
- [macOS — скачать SQL Server 2017 с Docker](../linux/quickstart-install-connect-docker.md)
- [Linux — скачать выпуск SQL Server 2017 Developer Edition](../linux/sql-server-linux-overview.md#install). Необходимо выполнить действия только до этапа *создания и отправки запроса к данным*.

## <a name="connect-to-a-sql-server"></a>Подключение к SQL Server

1. Запустите **Azure Data Studio**.

2. При первом запуске Azure Data Studio должна открыться страница **приветствия**. Если вы не видите **страницу приветствия** , выберите **Справка** > **Добро** пожаловать. Выберите **Создать подключение**, чтобы открыть панель **Подключение**.

   ![Значок нового подключения](media/quickstart-sql-server/new-connection-icon.png)

3. В этой статье используется *имя входа SQL*, но поддерживается *проверка подлинности Windows*. Заполните поля следующим образом:

   - **Имя сервера:** введите здесь имя сервера. Например: localhost.
   - **Тип проверки подлинности:** Имя входа SQL
   - **Имя пользователя:** имя пользователя SQL Server
   - **Пароль:** пароль для SQL Server
   - **Имя базы данных:** \<Default\>
   - **Группа серверов:** \<Default\>

   ![Экран нового подключения](media/quickstart-sql-server/new-connection-screen.png)

## <a name="create-a-database"></a>Создание базы данных

Ниже приведены инструкции по созданию базы данных с именем **TutorialDB**.

1. Щелкните правой кнопкой мыши сервер **localhost** и выберите **Создать запрос**.

2. Вставьте в окно запроса следующий фрагмент кода и щелкните **Выполнить**.

    ```sql
    USE master
    GO
    IF NOT EXISTS (
     SELECT name
     FROM sys.databases
     WHERE name = N'TutorialDB'
    )
     CREATE DATABASE [TutorialDB];
    GO
    IF SERVERPROPERTY('ProductVersion') > '12'
     ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON;
    GO
    ```

   После выполнения запроса новая база данных **TutorialDB** появится в списке баз данных. Если она не отображается, щелкните правой кнопкой мыши узел **Базы данных** и выберите пункт **Обновить**.

   ![Создание базы данных](media/quickstart-sql-server/create-database.png)

## <a name="create-a-table"></a>Создание таблицы

Редактор запросов все еще подключен к базе данных *master*, но нам нужно создать таблицу в базе данных *TutorialDB*.

1. Измените контекст подключения на **TutorialDB**.

   ![Изменение контекста](media/quickstart-sql-server/change-context.png)

2. Вставьте в окно запроса следующий фрагмент и нажмите кнопку **Выполнить**.

   > [!NOTE]
   > Можно добавить этот код или перезаписать предыдущий запрос в редакторе. Обратите внимание, что при нажатии кнопки **Выполнить** выполняется только выбранный запрос. Если ничего не выбрано, при нажатии кнопки **Выполнить** выполняются все запросы в редакторе.

    ```sql
    -- Create a new table called 'Customers' in schema 'dbo'
    -- Drop the table if it already exists
    IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
     DROP TABLE dbo.Customers;
    GO
    -- Create the table in the specified schema
    CREATE TABLE dbo.Customers
    (
     CustomerId int NOT NULL PRIMARY KEY, -- primary key column
     Name nvarchar(50) NOT NULL,
     Location nvarchar(50) NOT NULL,
     Email nvarchar(50) NOT NULL
    );
    GO
    ```

После выполнения запроса новая таблица **Customers** появится в списке таблиц. Может потребоваться щелкнуть **правой кнопкой мыши узел TutorialDB > Таблицы** и выбрать пункт **Обновить**.

## <a name="insert-rows"></a>Вставка строк

- Вставьте в окно запроса следующий фрагмент и нажмите кнопку **Выполнить**.

    ```sql
    -- Insert rows into table 'Customers'
    INSERT INTO dbo.Customers
     ([CustomerId], [Name], [Location], [Email])
    VALUES
     ( 1, N'Orlando', N'Australia', N''),
     ( 2, N'Keith', N'India', N'keith0@adventure-works.com'),
     ( 3, N'Donna', N'Germany', N'donna0@adventure-works.com'),
     ( 4, N'Janet', N'United States', N'janet1@adventure-works.com')
    GO
    ```

## <a name="view-the-data-returned-by-a-query"></a>Просмотр данных, возвращенных запросом

 - Вставьте в окно запроса следующий фрагмент и нажмите кнопку **Выполнить**.

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

   ![Выбор результатов](media/quickstart-sql-server/select-results.png)

## <a name="next-steps"></a>Дальнейшие действия

После успешного подключения к SQL Server и выполнения запроса можно перейти к руководству по [редактору кода](tutorial-sql-editor.md).