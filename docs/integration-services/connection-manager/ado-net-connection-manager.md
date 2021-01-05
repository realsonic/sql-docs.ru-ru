---
title: Диспетчер подключений ADO.NET | Документация Майкрософт
description: Диспетчер подключений ADO.NET позволяет пакету обращаться к источникам данных с помощью поставщика .NET.
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.adonetconnection.f1
helpviewer_keywords:
- connection managers [Integration Services], ADO.NET
- ADO.NET connection manager [Integration Services]
- connections [Integration Services], ADO.NET
ms.assetid: fc5daa2f-0159-4bda-9402-c87f1035a96f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8c2efe0145643f3f9e969c298e76115967477019
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637675"
---
# <a name="adonet-connection-manager"></a>Диспетчер соединений ADO.NET

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Диспетчер соединений [!INCLUDE[vstecado](../../includes/vstecado-md.md)] позволяет пакету обращаться к источникам данных с помощью поставщика .NET. Как правило, этот диспетчер подключений используется для получения доступа к таким источникам данных, как [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Кроме того, он обеспечивает доступ к источникам данных, предоставляемым посредством OLE DB и XML в пользовательских задачах, которые реализованы с помощью управляемого кода с использованием таких языков, как C#.  
  
При добавлении диспетчера подключений [!INCLUDE[vstecado](../../includes/vstecado-md.md)] к пакету [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] создает диспетчер подключений, который разрешается как подключение [!INCLUDE[vstecado](../../includes/vstecado-md.md)] во время выполнения. При этом задается свойство диспетчера подключений и диспетчер подключений добавляется в коллекцию **подключений** пакета.  
  
Свойству `ConnectionManagerType` диспетчера соединений присваивается значение `ADO.NET`. Значение `ConnectionManagerType` уточняется: в него включается имя поставщика .NET, используемого диспетчером соединений.  
  
## <a name="adonet-connection-manager-troubleshooting"></a>Устранение неполадок с диспетчером подключений ADO.NET  
В журнал можно записывать вызовы, сделанные диспетчером соединений [!INCLUDE[vstecado](../../includes/vstecado-md.md)] к внешним источникам данных. Это позволяет устранять неполадки с подключением к внешним источникам данных, которые устанавливаются диспетчером подключений [!INCLUDE[vstecado](../../includes/vstecado-md.md)]. Чтобы протоколировать вызовы диспетчера подключений [!INCLUDE[vstecado](../../includes/vstecado-md.md)] к внешним поставщикам данных, необходимо разрешить ведение журнала пакета и выбрать событие **Диагностика** на уровне пакета. Дополнительные сведения см. в разделе [Инструменты устранения неполадок при выполнении пакетов](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).  
  
При чтении данных диспетчером подключений [!INCLUDE[vstecado](../../includes/vstecado-md.md)] определенные типы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] формируют результаты, показанные в приведенной ниже таблице.  
  
|Тип данных SQL Server|Результат|  
|--------------------------|------------|  
|**time**, **datetimeoffset**|Выполнение пакета завершается неудачей, если в пакете не используются параметризованные команды SQL. Чтобы применить параметризованные команды SQL, используйте в пакете задачу «Выполнение SQL». Дополнительные сведения см. в разделах [Задача "Выполнение SQL"](../../integration-services/control-flow/execute-sql-task.md) и [Параметры и коды возврата в задаче "Выполнение SQL"](../control-flow/execute-sql-task.md).|  
|**datetime2**|Диспетчер соединений [!INCLUDE[vstecado](../../includes/vstecado-md.md)] отбрасывает миллисекунды.|  
  
> [!NOTE]  
>  Дополнительные сведения о типах данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и их сопоставлении с типами данных [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] см. в разделе [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md) и [Типы данных службы Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="adonet-connection-manager-configuration"></a>Настройка диспетчера подключений ADO.NET  
  
Значения свойств можно задавать с помощью конструктора [!INCLUDE[ssIS](../../includes/ssis-md.md)] или программными средствами.  
  
-   Предоставьте специальную строку подключения, настроенную таким образом, чтобы удовлетворить требования выбранного поставщика .NET.  
  
-   В зависимости от поставщика предоставьте имя источника данных, к которому производится подключение.  
  
-   Предоставьте безопасные учетные данные, соответствующие выбранному поставщику.  
  
-   Укажите, сохраняется ли в среде выполнения подключение, созданное диспетчером подключений.  
  
Многие параметры конфигурации диспетчера соединений [!INCLUDE[vstecado](../../includes/vstecado-md.md)] зависят от используемого им поставщика .NET.  
  
Дополнительные сведения о свойствах, которые можно задавать в конструкторе [!INCLUDE[ssIS](../../includes/ssis-md.md)], см. в [следующем разделе]().  
  
 Дополнительные сведения о программной настройке диспетчера подключений см. в разделах <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> и [Добавление соединений программным образом](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md).  
  
### <a name="configure-adonet-connection-manager"></a>Настройка диспетчера подключений ADO.NET
С помощью диалогового окна **Настройка диспетчера соединений OLE DB** можно добавлять подключения к источнику данных, доступ к которому можно осуществлять с помощью поставщика данных .NET Framework. Например, с помощью поставщика SqlClient. Диспетчер соединений использует существующее соединение, либо можно создать новое.  
  
 Дополнительные сведения о диспетчере соединений ADO.NET см. в разделе [ADO.NET Connection Manager](../../integration-services/connection-manager/ado-net-connection-manager.md).  
  
#### <a name="options"></a>Параметры  
**Подключения к данным**  
Выберите из списка существующее подключение к данным ADO.NET.  
  
**Свойства подключения к данным**  
Просмотрите свойства и значения выбранного подключения к данным ADO.NET.  
  
**Создать**  
Создание подключения к данным ADO.NET с помощью диалогового окна **Диспетчер соединений** .  
  
**Удаление**  
Выберите подключение и затем удалите его, щелкнув **Удалить**.  
  
#### <a name="managed-identities-for-azure-resources-authentication"></a>Управляемые удостоверения для проверки подлинности ресурсов Azure
При выполнении пакетов служб SSIS в [Azure-SSIS Integration Runtime в Фабрике данных Azure](/azure/data-factory/concepts-integration-runtime#azure-ssis-integration-runtime) вы можете использовать [управляемое удостоверение](/azure/data-factory/connector-azure-sql-database#managed-identity), связанное с вашей фабрикой данных, для проверки подлинности базы данных SQL Azure или Управляемого экземпляра SQL Azure. С помощью этого удостоверения назначенная фабрика может обращаться к данным и копировать их из вашей базы данных или в нее.

> [!NOTE]
>  При использовании проверки подлинности Azure Active Directory (Azure AD), включая проверку подлинности с помощью управляемого удостоверения, для подключения к базе данных SQL Azure или Управляемому экземпляру SQL Azure могут возникнуть проблемы, связанные со сбоем при выполнении пакета или непредвиденным изменением поведения. Дополнительные сведения см. в разделе о [функциях и ограничениях Azure AD](/azure/sql-database/sql-database-aad-authentication#azure-ad-features-and-limitations).

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

Наконец, настройте проверку подлинности с помощью управляемого удостоверения для диспетчера подключений ADO.NET. Это можно сделать такими способами:
    
- **Настройка во время разработки.** В конструкторе SSIS щелкните диспетчер подключений ADO.NET правой кнопкой мыши и выберите **Свойства**. Установите для свойства `ConnectUsingManagedIdentity` значение `True`.
    > [!NOTE]
    >  Сейчас свойство `ConnectUsingManagedIdentity` диспетчера подключений не оказывает никакого влияния (то есть проверка подлинности с помощью управляемого удостоверения не работает) при выполнении пакета SSIS в конструкторе SSIS или [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server.
    
- **Настройка во время выполнения.** При выполнении пакета с помощью [SQL Server Management Studio (SSMS)](../ssis-quickstart-run-ssms.md) или [действия "Выполнить пакет SSIS" Фабрики данных Azure](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) найдите диспетчер подключений ADO.NET. Установите для его свойства `ConnectUsingManagedIdentity` значение `True`.
    > [!NOTE]
    >  В Azure-SSIS Integration Runtime все остальные методы проверки подлинности (например, встроенная аутентификация и пароль), предварительно настроенные в диспетчере подключений ADO.NET, переопределяются, если для подключения к базе данных используется проверка подлинности с помощью управляемого удостоверения.

> [!NOTE]
>  Чтобы настроить проверку подлинности с помощью управляемого удостоверения для существующих пакетов, рекомендуем как минимум однократно перестроить проект SSIS с использованием [конструктора SSIS последней версии](../../ssdt/download-sql-server-data-tools-ssdt.md). Повторно разверните этот проект служб SSIS в Azure-SSIS Integration Runtime, чтобы новое свойство `ConnectUsingManagedIdentity` диспетчера подключений автоматически добавилось во все диспетчеры подключений ADO.NET в проекте SSIS. Кроме того, можно непосредственно переопределить свойство, указав при выполнении путь **\Package.Connections[{имя_диспетчера_подключений}].Properties[ConnectUsingManagedIdentity]** .

## <a name="see-also"></a>См. также раздел  
 [Соединения в службах Integration Services (SSIS)](../../integration-services/connection-manager/integration-services-ssis-connections.md)  
  
