---
title: Краткое руководство. Резервное копирование и восстановление в службе хранилища BLOB-объектов Azure
description: Краткое руководство о том, как создавать и восстанавливать резервные копии, используя службу Хранилища BLOB-объектов Azure. Создание контейнера BLOB-объектов Azure, запись резервной копии и последующее восстановление.
ms.custom: seo-dt-2019
ms.date: 12/21/2020
ms.prod: sql
ms.technology: backup-restore
ms.prod_service: backup-restore
ms.reviewer: ''
ms.topic: quickstart
author: cawrites
ms.author: chadam
ms.openlocfilehash: d27f53f80c8f987106f90816a4566339d777e6f6
ms.sourcegitcommit: bb54e4c9dd8c97365b7a96dfcd557b8b86d06978
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/22/2020
ms.locfileid: "97736902"
---
# <a name="quickstart-sql-backup-and-restore-to-azure-blob-storage-service"></a>Краткое руководство. Резервное копирование и восстановление SQL с помощью службы хранилища BLOB-объектов Azure

[!INCLUDE [sqlserver2016-asdbmi](../includes/applies-to-version/sqlserver2016-asdbmi.md)]

С помощью этого краткого руководства вы научитесь создавать и восстанавливать резервные копии, используя службу хранилища BLOB-объектов Azure.  В этой статье описывается создание контейнера больших двоичных объектов Azure, запись резервной копии в службу BLOB-объектов и выполнение восстановления.

> [!NOTE]
> В SQL Server 2012 SP1 CU2 добавлена поддержка создания резервных копий Хранилища BLOB-объектов Azure. SQL Server 2014 и более ранних версий не поддерживают подписанный URL-адрес, описанный в этом кратком руководстве.
>
> Если вы используете SQL Server 2014 и более ранних версий, изучите документ [Учебник. Резервное копирование и восстановление SQL Server 2014 с помощью Хранилища BLOB-объектов Microsoft Azure](/previous-versions/sql/2014/relational-databases/backup-restore/sql-server-backup-to-url).
>
  
## <a name="prerequisites"></a>Предварительные требования

Чтобы выполнить задания этого руководства, необходимо владеть основными понятиями резервного копирования и восстановления [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] и синтаксисом T-SQL.  Вам потребуется учетная запись хранения Azure, среда SQL Server Management Studio (SSMS) и доступ к серверу SQL Server или Управляемому экземпляру SQL Azure. Кроме того, учетная запись, используемая для выдачи команд резервного копирования и восстановления, должна находиться в роли базы данных **db_backupoperator** с разрешениями **изменение любых учетных данных**. 

- Получите бесплатную [учетную запись Azure](https://azure.microsoft.com/offers/ms-azr-0044p/).
- Создайте [учетную запись хранения Azure](/azure/storage/common/storage-quickstart-create-account?tabs=portal).
- Установите [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md).
- Установите выпуск [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads) или разверните [Управляемый экземпляр SQL Azure](/azure/sql-database/sql-database-managed-instance-get-started) с подключением, установленным через [виртуальную машину SQL Azure](/azure/sql-database/sql-database-managed-instance-configure-vm) или с помощью соединения [точка — сеть](/azure/sql-database/sql-database-managed-instance-configure-p2s).
- Назначьте учетной записи пользователя роль [db_backupoperator](./security/authentication-access/database-level-roles.md) и предоставьте разрешения на [изменение любых учетных данных](../t-sql/statements/alter-credential-transact-sql.md). 

## <a name="create-azure-blob-container"></a>Создание контейнера больших двоичных объектов Azure
Контейнер обеспечивает группирование набора больших двоичных объектов. Все BLOB-объекты должны содержаться в контейнере. Учетная запись хранения может содержать неограниченное количество контейнеров, но не менее одного. Контейнер может хранить неограниченное количество больших двоичных объектов. 

Чтобы создать контейнер, выполните следующие действия.

1. Откройте портал Azure. 
1. Перейдите к своей учетной записи хранения. 
1. Выберите учетную запись хранения, прокрутите вниз до раздела **Службы BLOB-объектов**.
1. Выберите **BLOB-объекты**, а затем щелкните **+ Контейнер**, чтобы добавить новый контейнер. 
1. Введите имя контейнера и запишите его. Эти сведения используются в URL-адресе (пути к файлу резервной копии) в инструкциях T-SQL далее в этом руководстве. 
1. Щелкните **ОК**. 
    
    ![Создание контейнера](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/new-container.png)

  > [!NOTE]
  > Для резервного копирования и восстановления SQL Server проводится проверка подлинности учетной записи хранилища даже при создании открытого контейнера. Можно также создать контейнер программным образом с помощью API-интерфейсов REST. Дополнительные сведения см. в статье [Создание контейнера](/rest/api/storageservices/Create-Container).

## <a name="create-a-test-database"></a>Создание тестовой базы данных 
На этом шаге создайте тестовую базу данных с помощью среды SQL Server Management Studio (SSMS). 

1. Запустите среду [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) и подключитесь к своему экземпляру SQL Server.
1. Откройте окно **Новый запрос**. 
1. Выполните следующий код Transact-SQL (T-SQL) для создания тестовой базы данных. Обновите узел **Базы данных** в **обозревателе объектов** для отображения новой базы данных. Для созданных баз данных в Управляемом экземпляре SQL автоматически включается TDE. Чтобы продолжить, вам необходимо отключить его. 

```sql
USE [master]
GO

-- Create database
CREATE DATABASE [SQLTestDB]
GO

-- Create table in database
USE [SQLTestDB]
GO
CREATE TABLE SQLTest (
    ID INT NOT NULL PRIMARY KEY,
    c1 VARCHAR(100) NOT NULL,
    dt1 DATETIME NOT NULL DEFAULT getdate()
)
GO

-- Populate table 
USE [SQLTestDB]
GO

INSERT INTO SQLTest (ID, c1) VALUES (1, 'test1')
INSERT INTO SQLTest (ID, c1) VALUES (2, 'test2')
INSERT INTO SQLTest (ID, c1) VALUES (3, 'test3')
INSERT INTO SQLTest (ID, c1) VALUES (4, 'test4')
INSERT INTO SQLTest (ID, c1) VALUES (5, 'test5')
GO

SELECT * FROM SQLTest
GO

-- Disable TDE for newly-created databases on SQL Managed Instance 
USE [SQLTestDB];
GO
ALTER DATABASE [SQLTestDB] SET ENCRYPTION OFF;
GO
```

## <a name="create-credential"></a>Создание учетных данных

Выполните приведенные ниже действия, чтобы создать учетные данные, используя графический пользовательский интерфейс в SQL Server Management Studio. Кроме того, вы можете создать учетные данные [программным способом](tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#2---create-a-sql-server-credential-using-a-shared-access-signature). 

1. Разверните узел **Базы данных** в **обозревателе объектов** среды [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md).
1. Щелкните правой кнопкой мыши новую базу данных `SQLTestDB`, наведите указатель мыши на параметр **Задачи**, а затем выберите **Архивировать...** , чтобы запустить мастер **Резервное копирование базы данных**. 
1. В раскрывающемся списке расположений **Архивировать на** выберите **URL-адрес**, а затем выберите **Добавить**, чтобы открыть диалоговое окно **Выбор расположения резервной копии**. 

   ![Резервное копирование по URL-адресу](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/back-up-to-url.png)

1. В диалоговом окне **Выбор расположения резервной копии** щелкните **Новый контейнер**, чтобы открыть окно **Соединение с подпиской Майкрософт**. 

   ![Снимок экрана: диалоговое окно "Выбор расположения резервной копии" с выделенным элементом "Новый контейнер".](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/select-backup-destination.png)

1. Войдите на портал Azure, выбрав **Вход...** , а затем выполните процедуру входа. 
1. Выберите **подписку** в раскрывающемся списке. 
1. Выберите **учетную запись хранения** в раскрывающемся списке. 
1. В раскрывающемся списке выберите контейнер, созданный ранее. 
1. Выберите **Создать учетные данные**, чтобы создать *подписанный URL-адрес (SAS)* .  **Сохраните это значение, так как оно понадобится для восстановления.**

   ![Создание учетных данных](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/create-credential.png)

1. Нажмите кнопку **ОК**, чтобы закрыть диалоговое окно **Соединение с подпиской Майкрософт**. Это действие приведет к заполнению значения *Контейнер службы хранилища Azure* в диалоговом окне **Выбор расположения резервной копии**. Нажмите кнопку **ОК**, чтобы выбрать указанный контейнер хранилища и закройте диалоговое окно. 
1. На этом этапе вы можете перейти к шагу 4 в следующем разделе, чтобы создать резервную копию базы данных, или закрыть мастер **Резервное копирование базы данных**, если вместо этого вы хотите продолжить использовать Transact-SQL для создания резервной копии базы данных. 


## <a name="back-up-database"></a>Резервное копирование базы данных
На этом шаге выполните резервное копирование базы данных `SQLTestDB` в учетную запись хранилища BLOB-объектов Azure, используя графический пользовательский интерфейс в среде SQL Server Management Studio или Transact-SQL (T-SQL). 

# <a name="ssms"></a>[SSMS](#tab/SSMS)

1. Если мастер **Резервное копирование базы данных** еще не открыт, разверните узел **Базы данных** в **обозревателе объектов** в среде [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md).
1. Щелкните правой кнопкой мыши новую базу данных `SQLTestDB`, наведите указатель мыши на параметр **Задачи**, а затем выберите **Архивировать...** , чтобы запустить мастер **Резервное копирование базы данных**. 
1. Выберите **URL-адрес** из раскрывающегося списка **Архивировать на**, а затем выберите **Добавить**, чтобы открыть диалоговое окно **Выбор расположения резервной копии**. 

   ![Резервное копирование по URL-адресу](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/back-up-to-url.png)

1. В раскрывающемся списке **Контейнер службы хранилища Azure** выберите контейнер, созданный на предыдущем шаге. 

   ![Контейнер хранилища Azure](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/azure-storage-container.png)

1. Нажмите кнопку **ОК** в мастере **Резервное копирование базы данных**, чтобы создать резервную копию базы данных. 
1. После успешного создания резервной копии нажмите кнопку **ОК**, чтобы закрыть все окна, связанные с резервным копированием. 

   > [!TIP]
   > Вы можете создать скрипт Transact-SQL для этой команды, выбрав **Скрипт** в верхней части мастера **Резервное копирование базы данных**. ![команда "Скрипт"](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/script-backup-command.png)


# <a name="transact-sql"></a>[Transact-SQL](#tab/tsql)

Создайте резервную копию базы данных с помощью Transact-SQL. Для этого выполните следующую команду: 


```sql
USE [master]

BACKUP DATABASE [SQLTestDB] 
TO  URL = N'https://msftutorialstorage.blob.core.windows.net/sql-backup/sqltestdb_backup_2020_01_01_000001.bak' 
WITH  COPY_ONLY, CHECKSUM
GO
```

---

## <a name="delete-database"></a>Удаление базы данных
На этом шаге удалите базу данных перед выполнением восстановления. Этот этап необходим только для целей данного учебника, но он вряд ли будет использоваться в обычных процедурах управления базой данных. Вы можете пропустить этот шаг, но тогда вам нужно будет либо изменить имя базы данных во время восстановления на управляемом экземпляре, либо выполнить команду восстановления `WITH REPLACE` для успешного восстановления базы данных локально. 

# <a name="ssms"></a>[SSMS](#tab/SSMS)

1. Разверните узел **Базы данных** в **обозревателе объектов**, щелкните правой кнопкой мыши базу данных `SQLTestDB` и выберите "Удалить", чтобы запустить мастер **Удаление объекта**. 
1. В управляемом экземпляре нажмите кнопку **ОК**, чтобы удалить базу данных. В локальной среде установите флажок рядом с параметром **Закрыть существующие соединения**, а затем нажмите кнопку **ОК**, чтобы удалить базу данных. 

# <a name="transact-sql"></a>[Transact-SQL](#tab/tsql)

Удалите базу данных, выполнив следующую команду Transact-SQL:

```sql
USE [master]
GO
DROP DATABASE [SQLTestDB]
GO

-- If connections currently exist on-premises, you'll need to set the database into single user mode first
USE [master]
GO
ALTER DATABASE [SQLTestDB] SET  SINGLE_USER WITH ROLLBACK IMMEDIATE
GO
USE [master]
GO
DROP DATABASE [SQLTestDB]
GO
```

---


## <a name="restore-database"></a>Восстановление базы данных 
На этом шаге восстановите базу данных, используя графический пользовательский интерфейс в среде SQL Server Management Studio или Transact-SQL. 

# <a name="ssms"></a>[SSMS](#tab/SSMS)

1. Щелкните правой кнопкой мыши узел **Базы данных** в **обозревателе объектов** в среде SQL Server Management Studio и выберите **Восстановить базу данных**. 
1. Щелкните **Устройство** и нажмите кнопку с многоточием (...), чтобы выбрать устройство. 

   ![Выбор устройства для восстановления](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/select-restore-device.png)

1. Выберите **URL-адрес** в раскрывающемся списке **Тип носителя резервной копии** и щелкните **Добавить**, чтобы добавить устройство. 

   ![Добавление устройства резервного копирования](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/add-backup-device.png)

1. Выберите контейнер из раскрывающегося списка, а затем вставьте подписанный URL-адрес (SAS), который вы сохранили при создании учетных данных. 

   ![Снимок экрана: диалоговое окно "Выбор расположения файла резервной копии" с заполненным полем "Подписанный URL-адрес".](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/restore-from-container.png)

1. Нажмите кнопку **ОК**, чтобы выбрать расположение файла резервной копии. 
1. Разверните узел **Контейнеры** и выберите контейнер, в котором расположен файл резервной копии. 
1. Выберите файл резервной копии, который необходимо восстановить, а затем нажмите кнопку **ОК**. Если файлы не отображаются, возможно, используется неправильный ключ SAS. Вы можете заново создать ключ SAS, выполнив те же действия, что и перед добавлением контейнера. 

   ![Выбор файла восстановления](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/select-restore-file.png)

1. Снова нажмите **ОК** в диалоговом окне **Выбор устройств резервного копирования**, чтобы закрыть его.
1. Чтобы восстановить базу данных, нажмите кнопку **ОК**.

# <a name="transact-sql"></a>[Transact-SQL](#tab/tsql)

Чтобы восстановить локальную базу данных из хранилища BLOB-объектов Azure, измените следующую команду Transact-SQL для использования собственной учетной записи хранения, а затем выполните команду в новом окне запроса.

```sql
USE [master]
RESTORE DATABASE [SQLTestDB] FROM 
URL = N'https://msftutorialstorage.blob.core.windows.net/sql-backup/sqltestdb_backup_2020_01_01_000001.bak'
```

---


## <a name="see-also"></a>См. также раздел 
Чтобы разобраться в концепциях и рекомендациях использования службы хранилища BLOB-объектов Azure для резервного копирования [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], рекомендуется изучить перечисленные ниже материалы.  
  
-   [Резервное копирование и восстановление SQL Server с помощью службы хранилища BLOB-объектов Microsoft Azure](../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)   
-   [Резервное копирование SQL Server на URL-адрес — рекомендации и устранение неполадок](../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md).  
