---
title: Подключение и запрос к базе данных SQL Azure или управляемому экземпляру Azure с помощью SQL Server Management Studio (SSMS)
description: Подключение к базе данных SQL Azure или управляемому экземпляру Azure в SSMS. Создание и запрос базы данных SQL Azure или управляемого экземпляра Azure в SSMS с использованием базовых запросов T-SQL.
ms.prod: sql
ms.technology: ssms
ms.topic: quickstart
author: markingmyname
ms.author: maghan
ms.reviewer: sstein, mikeray
ms.custom: contperf-fy21q2
ms.date: 12/15/2020
ms.openlocfilehash: c3745ac4aacbc76ae1c308f44214c1bdc3927857
ms.sourcegitcommit: 8a8c89b0ff6d6dfb8554b92187aca1bf0f8bcc07
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97619329"
---
# <a name="quickstart-connect-and-query-an-azure-sql-database-or-an-azure-managed-instance-using-sql-server-management-studio-ssms"></a>Краткое руководство. Подключение и запрос к базе данных SQL Azure или управляемому экземпляру Azure с помощью SQL Server Management Studio (SSMS)

[!INCLUDE [asdb](../../includes/applies-to-version/asdb.md)]

Начало работы с SQL Server Management Studio (SSMS) для подключения к базе данных SQL Azure и выполнения некоторых команд Transact-SQL (T-SQL).

В статье показано, как выполнять следующие задачи:

> [!div class="checklist"]
> - Подключение к базе данных SQL Azure
> - Создание базы данных
> - Создание таблицы в новой базе данных
> - Вставка строк в новую таблицу
> - Выполнение запросов к новой таблице и просмотр результатов
> - Проверка свойств подключения с помощью таблицы окна запросов

## <a name="prerequisites"></a>Предварительные условия

- установленная среда [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md);
- [База данных SQL Azure](https://azure.microsoft.com/free/sql-database/search/?&ef_id=CjwKCAiA17P9BRB2EiwAMvwNyDTqtKIHvBKK_qCTAnj3san_fx4KFjrSR8c58InygXQX5m_G71ZoMhoCzSMQAvD_BwE:G:s&OCID=AID2100131_SEM_CjwKCAiA17P9BRB2EiwAMvwNyDTqtKIHvBKK_qCTAnj3san_fx4KFjrSR8c58InygXQX5m_G71ZoMhoCzSMQAvD_BwE:G:s&gclid=CjwKCAiA17P9BRB2EiwAMvwNyDTqtKIHvBKK_qCTAnj3san_fx4KFjrSR8c58InygXQX5m_G71ZoMhoCzSMQAvD_BwE) или [Управляемый экземпляр SQL Azure](https://azure.microsoft.com/services/azure-sql/sql-managed-instance/)

## <a name="connect-to-an-azure-sql-database-or-azure-sql-managed-instance"></a>Подключение к базе данных SQL Azure или управляемому экземпляру SQL Azure

[!INCLUDE[ssms-connect-azure-ad](../../includes/ssms-connect-azure-ad.md)]

1. Запустите среду SQL Server Management Studio. При первом запуске SSMS откроется окно **Подключение к серверу**. Если этого не происходит, вы можете открыть его вручную, последовательно выбрав **Обозреватель объектов** > **Подключить** > **Ядро СУБД**.

    :::image type="content" source="media/ssms-connect-query-azure-sql/connect-object-explorer.png" alt-text="Ссылка для подключения в обозревателе объектов":::

2. Откроется диалоговое окно **Соединение с сервером** . Введите следующие сведения:

    |   Параметр   |   Рекомендуемые значения   |   Описание   |
    |-------------|------------------------|-----------------|
    | **Тип сервера** | Ядро СУБД | В поле **Тип сервера** выберите **Ядро СУБД** (обычно это параметр по умолчанию). |
    | **Имя сервера** | Полное имя сервера | В поле **Имя сервера** введите имя *базы данных SQL Azure* или *управляемого экземпляра Azure*. |
    | **Аутентификация** | Проверка подлинности SQL Server | Используйте для подключения к SQL Azure режим **Проверка подлинности SQL Server**. </br> </br> Режим **Проверка подлинности Windows** для SQL Azure не поддерживается. Дополнительные сведения см. в разделе [Проверка подлинности SQL Azure](/azure/sql-database/sql-database-security-overview#access-management). |
    | **Имя входа** | Идентификатор пользователя учетной записи сервера | Идентификатор пользователя учетной записи сервера, используемой для создания сервера. |
    | **Пароль** | Пароль учетной записи сервера | Пароль учетной записи сервера, используемой для создания сервера. |

    Вы также можете изменить дополнительные параметры подключения, выбрав **Параметры**. Примеры параметров подключения: база данных, к которой вы подключаетесь, время ожидания подключения и сетевой протокол. Эта статья использует во всех параметрах значения по умолчанию.

    :::image type="content" source="media/ssms-connect-query-azure-sql/connect-to-azure-sql-object-explorer.png" alt-text="Поле имени сервера для SQL Azure":::

3. После заполнения всех полей выберите **Подключить**.

    Вы также можете изменить дополнительные параметры подключения, выбрав **Параметры**. Примеры параметров подключения: база данных, к которой вы подключаетесь, время ожидания подключения и сетевой протокол. Эта статья использует во всех параметрах значения по умолчанию.

   Если вы еще не настроили параметры брандмауэра, появится соответствующий запрос. После входа введите данные для входа в учетную запись Azure и продолжите настройку правил брандмауэра. Нажмите кнопку **ОК**. Этот запрос появляется один раз. После настройки брандмауэра он не будет отображаться.

    :::image type="content" source="media/ssms-connect-query-azure-sql/azure-sql-firewall-sign-in-3.png" alt-text="Новое правило брандмауэра SQL Azure":::

4. Чтобы убедиться в успешном подключении к базе данных SQL Azure или управляемому экземпляру Azure, разверните и изучите объекты в **обозревателе объектов**, для которых отображаются имя сервера, версия SQL Server и имя пользователя. Эти объекты могут различаться в зависимости от типа сервера.

    :::image type="content" source="media/ssms-connect-query-azure-sql/connect-azure-sql.png" alt-text="Подключение к базе данных SQL Azure":::

## <a name="troubleshoot-connectivity-issues"></a>Устранение проблем подключения

Вы можете столкнуться с проблемами при подключении к Azure Synapse Analytics. Дополнительные сведения об устранении неполадок с подключением см. в [этой статье](https://docs.microsoft.com/azure/azure-sql/database/troubleshoot-common-errors-issues).

Возможны предотвращение, диагностика и устранение ошибок подключения и временных ошибок, которые возникают в клиентском приложении во время взаимодействия с базой данных SQL Azure или управляемым экземпляром SQL Azure. Дополнительные сведения см. в статье [Устранение временных ошибок подключения](https://docs.microsoft.com/azure/azure-sql/database/troubleshoot-common-connectivity-issues).

## <a name="create-a-database"></a>Создание базы данных

Выполните следующие действия, чтобы создать базу данных с именем TutorialDB:

1. Щелкните правой кнопкой мыши экземпляр сервера в обозревателе объектов и выберите **Создать запрос**.

   :::image type="content" source="media/ssms-connect-query-azure-sql/new-query.png" alt-text="Ссылка &quot;Создать запрос&quot;":::

2. Вставьте в окно запроса следующий фрагмент кода T-SQL:

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

3. Чтобы запустить запрос, нажмите кнопку **Выполнить** (или клавишу F5).

   :::image type="content" source="media/ssms-connect-query-azure-sql/execute.png" alt-text="Команда &quot;Выполнить&quot;":::
  
    После выполнения запроса в списке баз данных в обозревателе объектов появится новая база данных TutorialDB. Если она не отображается, щелкните правой кнопкой мыши узел **Базы данных** и выберите **Обновить**.

## <a name="create-a-table-in-the-new-database"></a>Создание таблицы в новой базе данных

В этом разделе вы создадите таблицу в новой базе данных TutorialDB. Так как редактор запросов все еще находится в контексте базы данных *master*, переключите контекст подключения на базу *TutorialDB*, сделав следующее.

1. Выберите нужную базу данных в раскрывающемся списке, как показано здесь:

   :::image type="content" source="media/ssms-connect-query-azure-sql/change-db.png" alt-text="Изменение базы данных":::

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

   :::image type="content" source="media/ssms-connect-query-azure-sql/new-table.png" alt-text="Новая таблица":::

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

   :::image type="content" source="media/ssms-connect-query-azure-sql/query-results.png" alt-text="Список результатов":::

    Вы также можете изменить представление результатов одним из следующих способов:

   ![Три варианта отображения результатов запроса](media/ssms-connect-query-azure-sql/results.png)

   - Первая кнопка отображает результаты в **текстовом представлении**, как показано на снимке в следующем разделе.
   - Кнопка посередине отображает результаты в **представлении сетки**; это параметр по умолчанию.
       - Это задано по умолчанию.
   - Третья кнопка позволяет сохранить результаты в файл, по умолчанию имеющий расширение .RPT.

## <a name="verify-your-connection-properties-by-using-the-query-window-table"></a>Проверка свойств подключения с помощью таблицы окна запросов

Сведения о свойствах подключения приводятся под результатами запроса. После выполнения запроса из предыдущего этапа просмотрите свойства подключения в нижней части окна запросов.

- Вы можете определить, к какому серверу и какой базе данных вы подключены и под каким именем пользователя выполнен вход.
- Кроме того, вы можете проверить длительность запроса и число строк, возвращенных предыдущим запросом.

   :::image type="content" source="media/ssms-connect-query-azure-sql/connection-properties.png" alt-text="Свойства подключения":::

## <a name="additional-tools"></a>Дополнительные средства

Также с помощью [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) вы можете выполнять подключения и запросы к [SQL Server](../../azure-data-studio/quickstart-sql-server.md), [базе данных SQL Azure](../../azure-data-studio/quickstart-sql-database.md) и [Azure Synapse Analytics](../../azure-data-studio/quickstart-sql-dw.md).

## <a name="next-steps"></a>Дальнейшие действия

Лучший способ познакомиться с SSMS — это поработать в среде самостоятельно. Эти статьи помогут вам ознакомиться с различными функциями SSMS.

- [Редактор запросов SQL Server Management Studio (SSMS)](../f1-help/database-engine-query-editor-sql-server-management-studio.md)
- [Создание скриптов](../tutorials/scripting-ssms.md)
- [Использование шаблонов в SSMS](../template/templates-ssms.md)
- [Конфигурация SSMS](../tutorials/ssms-configuration.md)
- [Дополнительные советы и рекомендации по использованию SSMS](../tutorials/ssms-tricks.md)