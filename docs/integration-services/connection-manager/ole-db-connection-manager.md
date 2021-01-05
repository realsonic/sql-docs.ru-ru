---
title: Диспетчер подключений OLE DB | Документация Майкрософт
description: Диспетчер соединений OLE DB позволяет пакету подключаться к источнику данных с помощью поставщика OLE DB.
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.oledbconnection.f1
helpviewer_keywords:
- OLE DB connection manager
- data sources [Integration Services], connections
- connection managers [Integration Services], OLE DB
- connections [Integration Services], OLE DB
ms.assetid: 91e3622e-4b1a-439a-80c7-a00b90d66979
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7c50a2e201bcba99299998dc232e1246005eb168
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97639503"
---
# <a name="ole-db-connection-manager"></a>Диспетчер соединений OLE DB

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Диспетчер соединений OLE DB позволяет пакету подключаться к источнику данных с помощью поставщика OLE DB. Например, диспетчер соединений OLE DB, который подключается к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , может использовать поставщик [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].    
    
> [!NOTE]
>  Поставщик OLE DB для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 11.0 не поддерживает новые ключевые слова строки подключения (MultiSubnetFailover=True) для отказоустойчивых кластеров с несколькими подсетями. Дополнительные сведения см. в [заметках о выпуске SQL Server](https://go.microsoft.com/fwlink/?LinkId=247824).    
> 
> [!NOTE]
>  Если источником данных является [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Excel 2007 или [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Access 2007, то для него понадобится источник данных, отличный от поставщиков для более ранних версий Excel или Access. Дополнительные сведения см. в разделе [Подключение к книге Excel](../load-data-to-from-excel-with-ssis.md) и [Подключение к базе данных Access](./integration-services-ssis-connections.md).    
    
Некоторые задачи служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] и компоненты потока данных применяют диспетчер соединений OLE DB. Например, источник OLE DB и назначение OLE DB используют этот диспетчер подключений для извлечения и загрузки данных. Задача "Выполнение SQL" может использовать этот диспетчер подключений, чтобы подключиться к базе данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для выполнения запросов.    
    
Кроме того, вы можете использовать диспетчер подключений OLE DB для получения доступа к источникам данных OLE DB в пользовательских задачах, написанных с использованием неуправляемого кода на языке, подобном C++.    
    
При добавлении к пакету диспетчера подключений OLE DB службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] создают диспетчер подключений, который обслуживает подключение OLE DB во время выполнения, задает свойства диспетчера подключений и добавляет его к коллекции **Connections** пакета.    
    
Свойству `ConnectionManagerType` диспетчера соединений присваивается значение `OLEDB`.    
    
Диспетчер подключений OLE DB можно настроить следующими способами:    
    
-   Укажите конкретную строку соединения, соответствующую требованиям конфигурации выбранного поставщика.    
    
-   В зависимости от поставщика предоставьте имя источника данных, к которому производится подключение.    
    
-   Предоставьте безопасные учетные данные, соответствующие выбранному поставщику.    
    
-   Укажите, сохраняется ли в среде выполнения подключение, созданное диспетчером подключений.    
    
## <a name="log-calls-and-troubleshoot-connections"></a>Ведение журналов вызовов и устранение проблем с подключениями    
 В журнал можно записывать вызовы, сделанные диспетчером соединений OLE DB к внешним поставщикам данных. Ведение журналов помогает устранять проблемы с подключениями, которые устанавливает диспетчер подключений OLE DB к внешним источникам данных. Чтобы вести журнал вызовов, которые диспетчер подключений OLE DB выполняет к внешним поставщикам данных, необходимо включить ведение журнала пакетов и выбрать событие **Диагностика** на уровне пакета. Дополнительные сведения см. в разделе [Инструменты устранения неполадок при выполнении пакетов](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).    
    
## <a name="configure-the-ole-db-connection-manager"></a>Настройка диспетчера подключений OLE DB    
 Значения свойств можно задавать с помощью конструктора [!INCLUDE[ssIS](../../includes/ssis-md.md)] или программными средствами. Дополнительные сведения о свойствах, которые можно задавать в конструкторе служб [!INCLUDE[ssIS](../../includes/ssis-md.md)] , см. в разделе [Настройка диспетчера соединений OLE DB](). Дополнительные сведения о настройке диспетчера соединений программными средствами см. в документации по классу **T:Microsoft.SqlServer.Dts.Runtime.ConnectionManager** в руководстве для разработчиков.    
    
### <a name="configure-ole-db-connection-manager"></a>Настройка диспетчера подключений OLE DB

Диалоговое окно **Настройка диспетчера соединений OLE DB** используется для добавления подключения к источнику данных. Это подключение может быть новым или копией существующего подключения.  
  
> [!NOTE]  
>  Для источника данных [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Excel 2007 потребуется поставщик данных, отличный от того, который использовался в предыдущих версиях Excel. Дополнительные сведения см. в разделе [Подключение к книге Excel](../load-data-to-from-excel-with-ssis.md).  
>   
>  Если источником данных является [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Access 2007, то для него понадобится поставщик OLE DB, отличающийся от поставщиков для более ранних версий Access. Дополнительные сведения см. в разделе [Подключение к базе данных Access](./integration-services-ssis-connections.md).  
  
 Дополнительные сведения о диспетчере соединений OLE DB см. в разделе [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
#### <a name="options"></a>Параметры  
 **Подключения к данным**  
 Выберите из списка существующее подключение к данным OLE DB.  
  
 **Свойства подключения к данным**  
 Просмотрите свойства и значения выбранного подключения к данным OLE DB.  
  
 **Создать**  
 Создайте подключение к данным OLE DB с помощью диалогового окна **Диспетчер соединений** .  
  
 **Удаление**  
 Выберите подключение и затем удалите его, нажав кнопку **Удалить**.  
  
#### <a name="managed-identities-for-azure-resources-authentication"></a>Управляемые удостоверения для проверки подлинности ресурсов Azure
При выполнении пакетов SSIS в [среде Azure-SSIS Integration Runtime Фабрики данных Azure](/azure/data-factory/concepts-integration-runtime#azure-ssis-integration-runtime) используйте [управляемое удостоверение](/azure/data-factory/connector-azure-sql-database#managed-identity), связанное с вашей фабрикой данных, для проверки подлинности базы данных или Управляемого экземпляра SQL Azure. С помощью этого удостоверения назначенная фабрика может обращаться к данным и копировать их из вашей базы данных или в нее.

> [!NOTE]
>  При использовании проверки подлинности Azure Active Directory (Azure AD), включая проверку подлинности с помощью управляемого удостоверения, для подключения к базе данных или Управляемому экземпляру SQL Azure могут возникнуть проблемы, связанные со сбоем при выполнении пакета или непредвиденным изменением поведения. Дополнительные сведения см. в разделе о [функциях и ограничениях Azure AD](/azure/sql-database/sql-database-aad-authentication#azure-ad-features-and-limitations).

Чтобы использовать проверку подлинности управляемого удостоверения для базы данных SQL Azure, выполните следующие действия для настройки базы данных:

1. [Подготовьте администратора Azure Active Directory](/azure/sql-database/sql-database-aad-authentication-configure#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server) для своего сервера SQL Azure на портале Azure, если вы еще этого не сделали. Администратор Azure AD может быть пользователем Azure AD или группой Azure AD. Если вы предоставляете группе с управляемым удостоверением роль администратора, пропустите шаги 2 и 3. Администратор будет иметь полный доступ к базе данных.

1. [Создайте пользователей автономной базы данных](/azure/sql-database/sql-database-aad-authentication-configure#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities) для управляемого удостоверения фабрики данных. Подключитесь к базе данных, откуда или куда вы хотите скопировать данные с помощью таких средств, как SSMS, с используя удостоверение Azure AD, которое имеет по меньшей мере разрешение ALTER ANY USER. Выполните следующий код T-SQL: 
    
    ```sql
    CREATE USER [your data factory name] FROM EXTERNAL PROVIDER;
    ```

1. Предоставьте управляемому удостоверению фабрики данных необходимые разрешения, как обычно делаете это для пользователей SQL и других лиц. Сведения о соответствующих ролях см. в статье [Роли уровня базы данных](../../relational-databases/security/authentication-access/database-level-roles.md). Выполните следующий код. Дополнительные параметры см. в [этом документе](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md).

    ```sql
    EXEC sp_addrolemember [role name], [your data factory name];
    ```

Чтобы использовать проверку подлинности с помощью управляемого удостоверения для Управляемого экземпляра SQL Azure, выполните следующие действия по настройке базы данных:
    
1. [Подготовьте администратора Azure Active Directory](/azure/sql-database/sql-database-aad-authentication-configure#provision-an-azure-active-directory-administrator-for-your-managed-instance) для своего управляемого экземпляра на портале Azure, если вы еще этого не сделали. Администратор Azure AD может быть пользователем Azure AD или группой Azure AD. Если вы предоставляете группе с управляемым удостоверением роль администратора, пропустите шаги 2–4. Администратор будет иметь полный доступ к базе данных.

1. [Создайте имена входа](../../t-sql/statements/create-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true) для управляемого удостоверения фабрики данных. В SQL Server Management Studio (SSMS) подключитесь к управляемому экземпляру с помощью учетной записи SQL Server с ролью **sysadmin**. Запустите следующий код T-SQL для базы данных **master**:

    ```sql
    CREATE LOGIN [your data factory name] FROM EXTERNAL PROVIDER;
    ```

1. [Создайте пользователей автономной базы данных](/azure/sql-database/sql-database-aad-authentication-configure#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities) для управляемого удостоверения фабрики данных. Подключитесь к базе данных, откуда или куда вы хотите скопировать данные, запустите следующий код T-SQL: 
  
    ```sql
    CREATE USER [your data factory name] FROM EXTERNAL PROVIDER;
    ```

1. Предоставьте управляемому удостоверению фабрики данных необходимые разрешения, как обычно делаете это для пользователей SQL и других лиц. Выполните следующий код. Дополнительные параметры см. в [этом документе](../../t-sql/statements/alter-role-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true).

    ```sql
    ALTER ROLE [role name e.g., db_owner] ADD MEMBER [your data factory name];
    ```

Затем настройте поставщик OLE DB для диспетчера подключений OLE DB. Это можно сделать такими способами:
    
- **Настройка во время разработки.** В конструкторе SSIS дважды щелкните диспетчер подключений OLE DB, чтобы открыть окно **Диспетчер подключений**. В раскрывающемся списке **Поставщик** выберите [**Microsoft OLE DB Driver for SQL Server**](https://go.microsoft.com/fwlink/?linkid=871294).
    > [!NOTE]
    >  Другие поставщики, указанные в раскрывающемся списке, могут не поддерживать проверку подлинности с помощью управляемого удостоверения.
    
- **Настройка во время выполнения.** При выполнении пакета с помощью [SQL Server Management Studio (SSMS)](../ssis-quickstart-run-ssms.md) или [действия "Выполнить пакет SSIS" Фабрики данных Azure](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) найдите свойство диспетчера подключений OLE DB **ConnectionString**. Измените свойство `Provider` подключения на `MSOLEDBSQL` (то есть Microsoft OLE DB Driver for SQL Server).
    ```vb
    Data Source=serverName;Initial Catalog=databaseName;Provider=MSOLEDBSQL;...
    ```

Наконец, настройте проверку подлинности с помощью управляемого удостоверения для диспетчера подключений OLE DB. Это можно сделать такими способами:
    
- **Настройка во время разработки.** В конструкторе SSIS щелкните диспетчер подключений OLE DB правой кнопкой мыши и выберите **Свойства**. Установите для свойства `ConnectUsingManagedIdentity` значение `True`.
    > [!NOTE]
    >  Сейчас свойство `ConnectUsingManagedIdentity` диспетчера подключений не оказывает никакого влияния (то есть проверка подлинности с помощью управляемого удостоверения не работает) при выполнении пакета SSIS в конструкторе SSIS или [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server.

- **Настройка во время выполнения.** При выполнении пакета с помощью SSMS или действия **Выполнить пакет SQL** найдите диспетчер подключений OLE DB и измените значение его свойства `ConnectUsingManagedIdentity` на `True`.
    > [!NOTE]
    >  В Azure-SSIS Integration Runtime все остальные методы проверки подлинности (например, встроенная система безопасности, пароль), предварительно настроенные в диспетчере подключений OLE DB, переопределяются, если для подключения к базе данных используется проверка подлинности с помощью управляемого удостоверения.

> [!NOTE]
>  Чтобы настроить проверку подлинности с помощью управляемого удостоверения для существующих пакетов, рекомендуем как минимум однократно перестроить проект SSIS с использованием [конструктора SSIS последней версии](../../ssdt/download-sql-server-data-tools-ssdt.md). Повторно разверните этот проект служб SSIS в среде Azure-SSIS Integration Runtime, чтобы новое свойство `ConnectUsingManagedIdentity` диспетчера подключений автоматически добавилось во все диспетчеры подключений OLE DB в проекте SSIS. Кроме того, можно непосредственно переопределить свойство, указав при выполнении путь **\Package.Connections[{имя_диспетчера_подключений}].Properties[ConnectUsingManagedIdentity]** .

## <a name="see-also"></a>См. также раздел    
 [Источник OLE DB](../../integration-services/data-flow/ole-db-source.md)     
 [Назначение OLE DB](../../integration-services/data-flow/ole-db-destination.md)     
 [Задача "Выполнение SQL"](../../integration-services/control-flow/execute-sql-task.md)     
 [Соединения в службах Integration Services (SSIS)](../../integration-services/connection-manager/integration-services-ssis-connections.md)    
    
