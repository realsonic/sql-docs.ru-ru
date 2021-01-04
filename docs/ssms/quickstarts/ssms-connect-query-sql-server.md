---
title: Подключение к экземпляру SQL Server и выполнение запросов с помощью SQL Server Management Studio (SSMS)
description: Подключение к экземпляру SQL Server в SSMS. Создание базы данных SQL Server и выполнение запросов к ней в SSMS с использованием базовых запросов T-SQL.
ms.prod: sql
ms.technology: ssms
ms.topic: quickstart
author: markingmyname
ms.author: maghan
ms.reviewer: sstein, mikeray
ms.custom: contperf-fy21q2
ms.date: 12/15/2020
ms.openlocfilehash: 519b60f63da38192e2196014e0ea7820dafd5491
ms.sourcegitcommit: 8a8c89b0ff6d6dfb8554b92187aca1bf0f8bcc07
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97619229"
---
# <a name="quickstart-connect-and-query-a-sql-server-instance-using-sql-server-management-studio-ssms"></a>Краткое руководство. Подключение к экземпляру SQL Server и выполнение запросов с помощью SQL Server Management Studio (SSMS)

[!INCLUDE [sqlserver](../../includes/applies-to-version/sqlserver.md)]

Начало работы с SQL Server Management Studio (SSMS) для подключения к экземпляру базы данных SQL Server и выполнения некоторых команд Transact-SQL (T-SQL).

В статье показано, как выполнять следующие задачи:

> [!div class="checklist"]
> - Подключение к экземпляру SQL Server
> - Создание базы данных
> - Создание таблицы в новой базе данных
> - Вставка строк в новую таблицу
> - Выполнение запросов к новой таблице и просмотр результатов
> - Проверка свойств подключения с помощью таблицы окна запросов

## <a name="prerequisites"></a>Предварительные условия

- установленная среда [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md);
- Установленный и настроенный [экземпляр SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads).

## <a name="connect-to-a-sql-server-instance"></a>Подключение к экземпляру SQL Server

1. Запустите среду SQL Server Management Studio. При первом запуске SSMS откроется окно **Подключение к серверу**. Если этого не происходит, вы можете открыть его вручную, последовательно выбрав **Обозреватель объектов** > **Подключить** > **Ядро СУБД**.

    :::image type="content" source="media/ssms-connect-query-sql-server/connect-object-explorer.png" alt-text="Ссылка для подключения в обозревателе объектов":::

2. Откроется диалоговое окно **Соединение с сервером** . Введите следующие сведения:

    |   Параметр   |   Рекомендуемые значения   |   Описание   |
    |--------------|-----------------------|-----------------|
    | **Тип сервера** | Ядро СУБД | В поле **Тип сервера** выберите **Ядро СУБД** (обычно это параметр по умолчанию). |
    | **Имя сервера** | Полное имя сервера | В поле **Имя сервера** введите имя SQL Server (при локальном подключении в качестве имени сервера также можно использовать *localhost*). Если вы НЕ ИСПОЛЬЗУЕТЕ экземпляр по умолчанию — ***MSSQLSERVER** _ — необходимо ввести имя сервера и имя экземпляра. </br> </br> Если вы не знаете, как определить имя экземпляра SQL Server, см. раздел [Дополнительные советы и рекомендации по использованию SSMS](../tutorials/ssms-tricks.md#find-sql-server-instance-name). |
    | _ *Проверка подлинности** | Проверка подлинности Windows </br> </br> Проверка подлинности SQL Server | По умолчанию используется проверка подлинности Windows. </br> </br> Также для подключения можно использовать режим **Проверка подлинности SQL Server**. Если выбран режим **Проверка подлинности SQL Server**, необходимо ввести имя пользователя и пароль. </br> </br> Дополнительные сведения о типах проверки подлинности см. в разделе [Подключение к серверу (ядро СУБД)](../f1-help/connect-to-server-database-engine.md). |
    | **Имя входа** | Идентификатор пользователя учетной записи сервера | Идентификатор пользователя учетной записи сервера, используемой для входа на сервер. Имя для входа, используемое для **проверки подлинности SQL Server**. |
    | **Пароль** | Пароль учетной записи сервера | Пароль учетной записи сервера, используемой для входа на сервер. Пароль, используемый для **проверки подлинности SQL Server**. |

    :::image type="content" source="media/ssms-connect-query-sql-server/connect-to-sql-server-object-explorer.png" alt-text="Поле имени сервера для SQL Server":::

3. После заполнения всех полей выберите **Подключить**.

    Вы также можете изменить дополнительные параметры подключения, выбрав **Параметры**. Примеры параметров подключения: база данных, к которой вы подключаетесь, время ожидания подключения и сетевой протокол. В этой статье во всех полях указываются значения по умолчанию.

4. Чтобы убедиться в успешном подключении к экземпляру SQL Server, разверните и изучите объекты в **обозревателе объектов**, для которых отображаются имя сервера, версия SQL Server и имя пользователя. Эти объекты могут различаться в зависимости от типа сервера.

    :::image type="content" source="media/ssms-connect-query-sql-server/connect-on-prem.png" alt-text="Подключение к локальному серверу":::

## <a name="troubleshoot-connectivity-issues"></a>Устранение проблем подключения

Сведения о способах устранения неполадок с подключением к экземпляру ядра СУБД SQL Server на отдельном сервере см. в статье [Устранение неполадок при соединении с ядром СУБД SQL Server](../../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md).

## <a name="create-a-database"></a>Создание базы данных

Выполните следующие действия, чтобы создать базу данных с именем TutorialDB:

1. Щелкните правой кнопкой мыши экземпляр сервера в обозревателе объектов и выберите **Создать запрос**.

   :::image type="content" source="media/ssms-connect-query-sql-server/new-query.png" alt-text="Ссылка &quot;Создать запрос&quot;":::

2. Вставьте в окно запроса следующий фрагмент кода T-SQL:

    ```sql
    USE master
    GO
    IF NOT EXISTS (
       SELECT name
       FROM sys.databases
       WHERE name = N'TutorialDB'
    )
    CREATE DATABASE [TutorialDB]
    GO
   ```

3. Чтобы запустить запрос, нажмите кнопку **Выполнить** (или клавишу F5).

   :::image type="content" source="media/ssms-connect-query-sql-server/execute.png" alt-text="Команда &quot;Выполнить&quot;":::
  
    После выполнения запроса в списке баз данных в обозревателе объектов появится новая база данных TutorialDB. Если она не отображается, щелкните правой кнопкой мыши узел **Базы данных** и выберите **Обновить**.

## <a name="create-a-table-in-the-new-database"></a>Создание таблицы в новой базе данных

В этом разделе вы создадите таблицу в новой базе данных TutorialDB. Так как редактор запросов все еще находится в контексте базы данных *master*, переключите контекст подключения на базу *TutorialDB*, сделав следующее.

1. Выберите нужную базу данных в раскрывающемся списке, как показано здесь:

   :::image type="content" source="media/ssms-connect-query-sql-server/change-db.png" alt-text="Изменение базы данных":::

2. Вставьте в окно запроса следующий фрагмент кода T-SQL:

    ```sql
    USE [TutorialDB]
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

3. Чтобы запустить запрос, нажмите кнопку **Выполнить** (или клавишу F5).

После выполнения запроса в списке таблиц в обозревателе объектов появится новая таблица Customers. Если таблица не отображается, щелкните правой кнопкой мыши узел **TutorialDB** > **Таблицы** в обозревателе объектов, а затем выберите **Обновить**.

   :::image type="content" source="media/ssms-connect-query-sql-server/new-table.png" alt-text="Новая таблица":::

## <a name="insert-rows-into-the-new-table"></a>Вставка строк в новую таблицу

Вставьте в созданную таблицу Customers какие-нибудь строки. Вставьте следующий фрагмент кода T-SQL в окно запросов и нажмите кнопку **Выполнить**.

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

## <a name="query-the-table-and-view-the-results"></a>Запрос к таблице и просмотр результатов

Результаты запроса выводятся под текстовым окном запроса. Чтобы запросить таблицу Customers и просмотреть вставленные строки, выполните следующие действия:

1. Вставьте следующий фрагмент кода T-SQL в окно запросов и нажмите кнопку **Выполнить**.

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

    Результаты запроса отображаются под областью, где был введен текст.

   :::image type="content" source="media/ssms-connect-query-sql-server/query-results.png" alt-text="Список результатов":::

    Вы также можете изменить представление результатов одним из следующих способов:

   ![Три варианта отображения результатов запроса](media/ssms-connect-query-sql-server/results.png)

   - Первая кнопка отображает результаты в **текстовом представлении**, как показано на снимке в следующем разделе.
   - Кнопка посередине отображает результаты в **представлении сетки**; это параметр по умолчанию.
       - Это задано по умолчанию.
   - Третья кнопка позволяет сохранить результаты в файл, по умолчанию имеющий расширение .RPT.

## <a name="verify-your-connection-properties-by-using-the-query-window-table"></a>Проверка свойств подключения с помощью таблицы окна запросов

Сведения о свойствах подключения приводятся под результатами запроса. После выполнения запроса из предыдущего этапа просмотрите свойства подключения в нижней части окна запросов.

- Вы можете определить, к какому серверу и какой базе данных вы подключены и под каким именем пользователя выполнен вход.
- Кроме того, вы можете проверить длительность запроса и число строк, возвращенных предыдущим запросом.

   :::image type="content" source="media/ssms-connect-query-sql-server/connection-properties.png" alt-text="Свойства подключения":::

## <a name="additional-tools"></a>Дополнительные средства

Также с помощью [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) вы можете выполнять подключения и запросы к [SQL Server](../../azure-data-studio/quickstart-sql-server.md), [базе данных SQL Azure](../../azure-data-studio/quickstart-sql-database.md) и [Azure Synapse Analytics](../../azure-data-studio/quickstart-sql-dw.md).

## <a name="next-steps"></a>Дальнейшие действия

Лучший способ познакомиться с SSMS — это поработать в среде самостоятельно. Эти статьи помогут вам ознакомиться с различными функциями SSMS.

- [Редактор запросов SQL Server Management Studio (SSMS)](../f1-help/database-engine-query-editor-sql-server-management-studio.md)
- [Создание скриптов](../tutorials/scripting-ssms.md)
- [Использование шаблонов в SSMS](../template/templates-ssms.md)
- [Конфигурация SSMS](../tutorials/ssms-configuration.md)
- [Дополнительные советы и рекомендации по использованию SSMS](../tutorials/ssms-tricks.md)