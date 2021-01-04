---
title: Подключение к экземпляру SQL Server на виртуальной машине Azure и выполнение запросов с помощью SQL Server Management Studio (SSMS)
description: Подключение к экземпляру SQL Server на виртуальной машине Azure с помощью SSMS. Создание экземпляра SQL Server на виртуальной машине Azure и отправка запросов к нему с помощью базовых запросов T-SQL в SSMS.
ms.prod: sql
ms.technology: ssms
ms.topic: quickstart
author: markingmyname
ms.author: maghan
ms.reviewer: sstein, mikeray
ms.custom: contperf-fy21q2
ms.date: 12/15/2020
ms.openlocfilehash: 29c39caf6885ee974c62ed153df982b435c72c95
ms.sourcegitcommit: 8a8c89b0ff6d6dfb8554b92187aca1bf0f8bcc07
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97619326"
---
# <a name="quickstart-connect-and-query-a-sql-server-instance-on-an-azure-virtual-machine-using-sql-server-management-studio-ssms"></a>Краткое руководство. Подключение к экземпляру SQL Server на виртуальной машине Azure и выполнение запросов с помощью SQL Server Management Studio (SSMS)

[!INCLUDE [sqlserver](../../includes/applies-to-version/sqlserver.md)]

Начало работы с SQL Server Management Studio (SSMS) для подключения к экземпляру SQL Server на виртуальной машине Azure и выполнения некоторых команд Transact-SQL (T-SQL).

> [!div class="checklist"]
> - Подключение к экземпляру SQL Server
> - Создание базы данных
> - Создание таблицы в новой базе данных
> - Вставка строк в новую таблицу
> - Выполнение запросов к новой таблице и просмотр результатов
> - Проверка свойств подключения с помощью таблицы окна запросов
## <a name="prerequisites"></a>Предварительные условия

Для работы с этой статьей необходима среда SQL Server Management Studio и доступ к источнику данных.

- Установите [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md).
- [SQL Server на виртуальной машине Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/?&ef_id=CjwKCAiA17P9BRB2EiwAMvwNyP3g3mY45X8dbqEtIr8HASwSLUYRBrwwciZptYt0vUU4K7TuWnXTbxoCoPQQAvD_BwE:G:s&OCID=AID2100131_SEM_CjwKCAiA17P9BRB2EiwAMvwNyP3g3mY45X8dbqEtIr8HASwSLUYRBrwwciZptYt0vUU4K7TuWnXTbxoCoPQQAvD_BwE:G:s&gclid=CjwKCAiA17P9BRB2EiwAMvwNyP3g3mY45X8dbqEtIr8HASwSLUYRBrwwciZptYt0vUU4K7TuWnXTbxoCoPQQAvD_BwE).

## <a name="connect-to-sql-virtual-machines"></a>Подключение к виртуальным машинам SQL

Ниже показано, как создать необязательную метку DNS для виртуальной машины Azure и подключиться с помощью SQL Server Management Studio (SSMS).

### <a name="configure-a-dns-label-for-the-public-ip-address"></a>Настройка имени DNS для общедоступного IP-адреса

Для подключения к СУБД SQL Server через Интернет рекомендуем создать имя DNS для вашего общедоступного IP-адреса. Вы можете подключиться по IP-адресу, но DNS-метка создает запись А, которую легче идентифицировать, и абстрагирует базовый общедоступный IP-адрес.

> [!NOTE]
> DNS-метки не обязательны, если вы подключаетесь к экземпляру SQL Server в той же виртуальной сети или на локальном компьютере.

1. Чтобы создать DNS-метку, выберите **Виртуальные машины** на портале. Выберите виртуальную машину SQL Server, чтобы открыть ее свойства.

2. В обозревателе виртуальной машины выберите **общедоступный IP-адрес**.

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/azure-sql-vm-ip-address-.png" alt-text="Общедоступный IP-адрес":::

3. В свойствах общедоступного IP-адреса разверните раздел **Конфигурация**.

4. Введите имя DNS. Это имя (запись A), которое можно использовать для прямого подключения к виртуальной машине SQL Server по имени, а не по IP-адресу.

5. Нажмите кнопку **Сохранить**.

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/azure-sql-vm-dns-label.png" alt-text="DNS-метка":::

### <a name="connect"></a>Подключение

1. Запустите среду SQL Server Management Studio. При первом запуске SSMS откроется окно **Подключение к серверу**. Если этого не происходит, вы можете открыть его вручную, последовательно выбрав **Обозреватель объектов** > **Подключить** > **Ядро СУБД**.

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connect-object-explorer.png" alt-text="Ссылка для подключения в обозревателе объектов":::

2. Откроется диалоговое окно **Соединение с сервером** . Введите следующие сведения:

    |   Параметр   |   Рекомендуемые значения   |   Описание   |
    |--------------|-----------------------|-----------------|
    | **Тип сервера** | Ядро СУБД | В поле **Тип сервера** выберите **Ядро СУБД** (обычно это параметр по умолчанию). |
    | **Имя сервера** | Полное имя сервера | В поле **Имя сервера** введите имя своей виртуальной машины Azure SQL. Для подключения можно также использовать IP-адрес виртуальной машины SQL Azure. | 
    | **Аутентификация** | Проверка подлинности SQL Server | Используйте для подключения к виртуальной машине SQL Azure режим **Проверка подлинности SQL Server**. Кроме того, при наличии установки среды Azure Active Directory можно использовать любой из параметров Azure Active Directory. </br> </br> Режим **Проверка подлинности Windows** для виртуальной машины SQL Azure не поддерживается. Дополнительные сведения см. в разделе [Проверка подлинности SQL Azure](/azure/sql-database/sql-database-security-overview#access-management).|
    | **Имя входа** | Идентификатор пользователя учетной записи сервера | Идентификатор пользователя учетной записи сервера, используемой для создания сервера. Имя для входа, используемое для **проверки подлинности SQL Server**. |
    | **Пароль** | Пароль учетной записи сервера | Пароль учетной записи сервера, используемой для создания сервера. Пароль, используемый для **проверки подлинности SQL Server**. |

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connect-to-azure-sql-vm-object-explorer.png" alt-text="Поле имени сервера для виртуальных машин SQL":::

3. После заполнения всех полей выберите **Подключить**.

    Вы также можете изменить дополнительные параметры подключения, выбрав **Параметры**. Примеры параметров подключения: база данных, к которой вы подключаетесь, время ожидания подключения и сетевой протокол. Эта статья использует во всех параметрах значения по умолчанию.

4. Чтобы убедиться в успешном подключении к экземпляру SQL Server на виртуальной машине Azure, разверните и изучите объекты в **обозревателе объектов**, для которых отображаются имя сервера, версия SQL Server и имя пользователя. Эти объекты могут различаться в зависимости от типа сервера.

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connect-azure-sql-vm.png" alt-text="Подключение к виртуальной машине SQL Azure":::

## <a name="troubleshoot-connectivity-issues"></a>Устранение проблем подключения

Хотя портал предоставляет возможности для автоматической настройки подключения, полезно знать, как можно сделать это вручную. Знакомство с требованиями также будет полезным при устранении неполадок.

В следующей таблице перечислены требования для подключения к SQL Server на виртуальной машине Azure.

| Требование | Описание |
|---|---|
| [Включите режим проверки подлинности SQL Server](/sql/database-engine/configure-windows/change-server-authentication-mode#use-ssms) | Для удаленного подключения к виртуальной машине требуется проверка подлинности SQL Server, если в виртуальной сети не настроена служба Active Directory. |
| [Создайте имя входа SQL](/sql/relational-databases/security/authentication-access/create-a-login) | При использовании проверки подлинности SQL требуются учетные данные SQL с именем пользователя и паролем, у которых есть разрешения для доступа к целевой базе данных. |
| Включите протокол TCP/IP | SQL Server должен разрешать подключения по протоколу TCP. |
| [Включите правило брандмауэра для порта SQL Server](/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) | Брандмауэр на виртуальной машине должен разрешать входящий трафик на порту SQL Server (по умолчанию 1433). |
| [Создайте правило группы безопасности сети для TCP 1433](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group#create-a-security-rule) | Разрешите виртуальной машине получать трафик через порт SQL Server (по умолчанию 1433), если нужно выполнить подключение через Интернет. Для локальных подключений и подключений к виртуальным сетям это не требуется. На портале Azure больше не требуется никаких действий. |

> [!TIP]
> Действия, описанные в предыдущей таблице, выполняются при настройке подключения на портале. Используйте их только для проверки конфигурации или настройки подключения к SQL Server вручную.

## <a name="create-a-database"></a>Создание базы данных

Сделайте следующее, чтобы создать базу данных с именем TutorialDB.

1. Щелкните правой кнопкой мыши экземпляр сервера в обозревателе объектов и выберите **Создать запрос**.

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/new-query.png" alt-text="Ссылка &quot;Создать запрос&quot;":::

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

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/execute.png" alt-text="Команда &quot;Выполнить&quot;":::
  
    После выполнения запроса в списке баз данных в обозревателе объектов появится новая база данных TutorialDB. Если она не отображается, щелкните правой кнопкой мыши узел **Базы данных** и выберите **Обновить**.

## <a name="create-a-table-in-the-new-database"></a>Создание таблицы в новой базе данных

В этом разделе вы создадите таблицу в новой базе данных TutorialDB. Так как редактор запросов все еще находится в контексте базы данных *master*, переключите контекст подключения на базу *TutorialDB*, сделав следующее.

1. Выберите нужную базу данных в раскрывающемся списке, как показано здесь:

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/change-db.png" alt-text="Изменение базы данных":::

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

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/new-table.png" alt-text="Новая таблица":::

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

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/query-results.png" alt-text="Список результатов":::

    Вы также можете изменить представление результатов одним из следующих способов:

   ![Три варианта отображения результатов запроса](media/ssms-connect-query-sql-server-azure-vm/results.png)

   - Первая кнопка отображает результаты в **текстовом представлении**, как показано на снимке в следующем разделе.
   - Кнопка посередине отображает результаты в **представлении сетки**; это параметр по умолчанию.
       - Это задано по умолчанию.
   - Третья кнопка позволяет сохранить результаты в файл, по умолчанию имеющий расширение .RPT.

## <a name="verify-your-connection-properties-by-using-the-query-window-table"></a>Проверка свойств подключения с помощью таблицы окна запросов

Сведения о свойствах подключения приводятся под результатами запроса. После выполнения запроса из предыдущего этапа просмотрите свойства подключения в нижней части окна запросов.

- Вы можете определить, к какому серверу и какой базе данных вы подключены и под каким именем пользователя выполнен вход.
- Кроме того, вы можете проверить длительность запроса и число строк, возвращенных предыдущим запросом.

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connection-properties.png" alt-text="Свойства подключения":::

## <a name="additional-tools"></a>Дополнительные средства

Также с помощью [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) вы можете выполнять подключения и запросы к [SQL Server](../../azure-data-studio/quickstart-sql-server.md), [базе данных SQL Azure](../../azure-data-studio/quickstart-sql-database.md) и [Azure Synapse Analytics](../../azure-data-studio/quickstart-sql-dw.md).

## <a name="next-steps"></a>Дальнейшие действия

Лучший способ познакомиться с SSMS — это поработать в среде самостоятельно. Эти статьи помогут вам ознакомиться с различными функциями SSMS.

- [Редактор запросов SQL Server Management Studio (SSMS)](../f1-help/database-engine-query-editor-sql-server-management-studio.md)
- [Создание скриптов](../tutorials/scripting-ssms.md)
- [Использование шаблонов в SSMS](../template/templates-ssms.md)
- [Конфигурация SSMS](../tutorials/ssms-configuration.md)
- [Дополнительные советы и рекомендации по использованию SSMS](../tutorials/ssms-tricks.md)