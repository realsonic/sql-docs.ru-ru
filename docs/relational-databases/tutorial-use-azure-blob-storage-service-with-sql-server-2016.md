---
description: Учебник. Использование службы хранилища больших двоичных объектов Azure с базами данных SQL Server 2016
title: Учебник. Использование службы хранилища больших двоичных объектов Azure с базами данных SQL Server 2016
ms.custom: seo-dt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.technology: ''
ms.prod_service: database-engine
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- SQL Server 2016
ms.assetid: e69be67d-da1c-41ae-8c9a-6b12c8c2fb61
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 31407e6d056ce8ec847f52346b8aa3ddf8e621f3
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644190"
---
# <a name="tutorial-use-azure-blob-storage-service-with-sql-server-2016"></a>Учебник. Использование службы хранилища больших двоичных объектов Azure с базами данных SQL Server 2016

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
Добро пожаловать в учебник по работе с SQL Server 2016 в службе хранилища BLOB-объектов Microsoft Azure. С помощью этого учебника вы научитесь использовать службу хранилища BLOB-объектов Microsoft Azure для сохранения файлов данных и резервных копий SQL Server.  
  
Поддержка интеграции SQL Server со службой хранилища BLOB-объектов Microsoft Azure появилась в SQL Server 2012 с пакетом обновления 1 (SP1) и накопительным пакетом обновления  2 и в дальнейшем была улучшена в SQL Server 2014 и SQL Server 2016. Обзор возможностей и преимуществ использования этих функций см. в статье [Файлы данных SQL Server в Microsoft Azure](../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md). Работа функции показана в [видеоролике, посвященном восстановлению до точки во времени](https://channel9.msdn.com/Blogs/Windows-Azure/File-Snapshot-Backups-Demo).  

В этом руководстве в нескольких разделах показано, как работать с файлами данных SQL Server в службе хранилища BLOB-объектов Microsoft Azure. В каждом разделе рассматривается определенная задача, и их следует выполнять по порядку. Сначала вы узнаете, как создать контейнер в хранилище BLOB-объектов с помощью хранимой политики доступа и подписанного URL-адреса. Затем вы узнаете, как создать учетные данные SQL Server, чтобы интегрировать SQL Server с хранилищем BLOB-объектов Azure. Далее вы выполните резервное копирование базы данных в хранилище BLOB-объектов и восстановите ее в виртуальной машине Azure. После этого вы используете резервную копию журнала транзакций SQL Server 2016 на основе моментального снимка файла, чтобы выполнить восстановление в новой базе данных на определенный момент времени. Наконец, в учебнике будет продемонстрировано использование хранимых процедур и функций системы метаданных, что позволит вам понять, как работать с резервными копиями моментальных снимков файлов.
  
## <a name="prerequisites"></a>Предварительные требования

Чтобы выполнить задания этого руководства, необходимо владеть основными понятиями резервного копирования и восстановления [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] и синтаксисом T-SQL. Для работы с этим руководством требуется учетная запись хранилища Azure, SQL Server Management Studio (SSMS), доступ к экземпляру SQL Server в локальной среде, доступ к виртуальной машине Azure под управлением SQL Server 2016 и база данных AdventureWorks2016. Кроме того, учетная запись, используемая для выдачи команд резервного копирования и восстановления, должна находиться в роли базы данных **db_backupoperator** с разрешениями **изменение любых учетных данных**. 

- Получите бесплатную [учетную запись Azure](https://azure.microsoft.com/offers/ms-azr-0044p/).
- Создайте [учетную запись хранения Azure](/azure/storage/common/storage-quickstart-create-account?tabs=portal).
- Установите выпуск [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Подготовка [виртуальной машины Azure под управлением SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/)
- Установите [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md).
- Скачайте [образцы баз данных AdventureWorks2016](../samples/adventureworks-install-configure.md).
- Назначьте учетной записи пользователя роль [db_backupoperator](./security/authentication-access/database-level-roles.md) и предоставьте разрешения на [изменение любых учетных данных](../t-sql/statements/alter-credential-transact-sql.md). 

## <a name="1---create-stored-access-policy-and-shared-access-storage"></a>1. Создание хранимой политики доступа и хранилища с общим доступом

В этом разделе вы примените скрипт [Azure PowerShell](/powershell/azure/), чтобы создать подписанный URL-адрес для контейнера BLOB-объектов Azure с помощью хранимой политики доступа.  
  
> [!NOTE]  
> Этот скрипт написан с помощью Azure PowerShell 5.0.10586.  
  
Подписанный URL-адрес — это универсальный код ресурса (URI), который предоставляет ограниченные права доступа к контейнерам, большим двоичным объектам, очередям и таблицам. Хранимая политика доступа предоставляет дополнительный уровень контроля над сервером, включая отзыв, истечение срока действия и продление доступа. При использовании этого расширения необходимо создать политику в контейнере как минимум с правами на чтение, запись и перечисление.  
  
Хранимую политику доступа и подписанный URL-адрес можно создать с помощью Azure PowerShell, пакета SDK службы хранилища Azure, REST API Azure или служебной программы стороннего разработчика. В этом учебнике демонстрируется применение скрипта Azure PowerShell для выполнения данной задачи. В скрипте используется модель развертывания диспетчера ресурсов и создаются следующие ресурсы:  
  
-   Группа ресурсов   
-   Учетная запись хранения  
-   Контейнер BLOB-объектов Azure   
-   Политика SAS    

Выполнение скрипта начинается с объявления ряда переменных для указания имен перечисленных выше ресурсов и имен следующих обязательных входных значений:  
  
-   имя префикса, используемое для именования других объектов ресурсов;    
-   имя подписки;    
-   расположение центра обработки данных.  

  
В результате выполнения скрипта создается соответствующая инструкция CREATE CREDENTIAL, которая будет использоваться в разделе [2. Создание учетных данных SQL Server с помощью подписанного URL-адреса](#2---create-a-sql-server-credential-using-a-shared-access-signature). Эта инструкция копируется в буфер обмена и выводится в консоль.  
  
Чтобы создать политику для контейнера и ключ подписанного URL-адреса, выполните указанные ниже действия.  
  
1.  Откройте интегрированную среду сценариев Window PowerShell или Windows PowerShell (см. требования к версии выше).  
  
2.  Измените, а затем выполните приведенный ниже скрипт:  
  
    ```powershell
    # Define global variables for the script  
    $prefixName = '<a prefix name>'  # used as the prefix for the name for various objects  
    $subscriptionID = '<your subscription ID>'   # the ID  of subscription name you will use  
    $locationName = '<a data center location>'  # the data center region you will use  
    $storageAccountName= $prefixName + 'storage' # the storage account name you will create or use  
    $containerName= $prefixName + 'container'  # the storage container name to which you will attach the SAS policy with its SAS token  
    $policyName = $prefixName + 'policy' # the name of the SAS policy 

    # Set a variable for the name of the resource group you will create or use  
    $resourceGroupName=$prefixName + 'rg'   
  
    # Add an authenticated Azure account for use in the session   
    Connect-AzAccount    
  
    # Set the tenant, subscription and environment for use in the rest of   
    Set-AzContext -SubscriptionId $subscriptionID   
  
    # Create a new resource group - comment out this line to use an existing resource group  
    New-AzResourceGroup -Name $resourceGroupName -Location $locationName   
  
    # Create a new Azure Resource Manager storage account - comment out this line to use an existing Azure Resource Manager storage account  
    New-AzStorageAccount -Name $storageAccountName -ResourceGroupName $resourceGroupName -Type Standard_RAGRS -Location $locationName   
  
    # Get the access keys for the Azure Resource Manager storage account  
    $accountKeys = Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName  
  
    # Create a new storage account context using an Azure Resource Manager storage account  
    $storageContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $accountKeys[0].Value

    # Creates a new container in blob storage  
    $container = New-AzStorageContainer -Context $storageContext -Name $containerName  
  
    # Sets up a Stored Access Policy and a Shared Access Signature for the new container  
    $policy = New-AzStorageContainerStoredAccessPolicy -Container $containerName -Policy $policyName -Context $storageContext -StartTime $(Get-Date).ToUniversalTime().AddMinutes(-5) -ExpiryTime $(Get-Date).ToUniversalTime().AddYears(10) -Permission rwld

    # Gets the Shared Access Signature for the policy  
    $sas = New-AzStorageContainerSASToken -name $containerName -Policy $policyName -Context $storageContext
    Write-Host 'Shared Access Signature= '$($sas.Substring(1))''  

    # Sets the variables for the new container you just created
    $container = Get-AzStorageContainer -Context $storageContext -Name $containerName
    $cbc = $container.CloudBlobContainer 
  
    # Outputs the Transact SQL to the clipboard and to the screen to create the credential using the Shared Access Signature  
    Write-Host 'Credential T-SQL'  
    $tSql = "CREATE CREDENTIAL [{0}] WITH IDENTITY='Shared Access Signature', SECRET='{1}'" -f $cbc.Uri,$sas.Substring(1)   
    $tSql | clip  
    Write-Host $tSql 

    # Once you're done with the tutorial, remove the resource group to clean up the resources. 
    # Remove-AzResourceGroup -Name $resourceGroupName  
    ```  

3.  По завершении выполнения скрипта инструкция CREATE CREDENTIAL будет находиться в буфере обмена для использования в следующем разделе.  


## <a name="2---create-a-sql-server-credential-using-a-shared-access-signature"></a>2. Создание учетных данных SQL Server с помощью подписанного URL-адреса

В этом разделе вы создадите учетные данные для хранения сведений о безопасности, которые SQL Server будет использовать для записи в контейнер Azure, созданный на предыдущем шаге, и чтения из этого контейнера.  
  
Учетные данные SQL Server — это объект, который используется для хранения сведений, необходимых для проверки подлинности при подключении к ресурсу вне SQL Server. В учетных данных хранится URI-путь к контейнеру хранилища и подписанный URL-адрес этого контейнера.  

Чтобы создать учетные данные SQL Server, выполните указанные ниже действия.  
  
1.  Подключитесь к среде Microsoft SQL Server Management Studio.  
2.  Откройте новое окно запроса и подключитесь к экземпляру SQL Server ядра СУБД в локальной среде.  
  
3.  В новое окно запроса вставьте инструкцию CREATE CREDENTIAL с подписанным URL-адресом из раздела 1, а затем выполните этот скрипт.  
  
    Код скрипта будет выглядеть следующим образом.  
  
    ```sql   
    /* Example:
    USE master  
    CREATE CREDENTIAL [https://msfttutorial.blob.core.windows.net/containername] 
    WITH IDENTITY='SHARED ACCESS SIGNATURE'   
    , SECRET = 'sharedaccesssignature' 
    GO */
    
    USE master  
    CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>] 
      -- this name must match the container path, start with https and must not contain a forward slash at the end
    WITH IDENTITY='SHARED ACCESS SIGNATURE' 
      -- this is a mandatory string and should not be changed   
     , SECRET = 'sharedaccesssignature' 
       -- this is the shared access signature key that you obtained in section 1.   
    GO    
    ```  
  
4.  Чтобы увидеть все доступные учетные данные, можно выполнить следующую инструкцию в окне запроса, подключенном к экземпляру:  
  
    ```sql  
    SELECT * from sys.credentials  
    ```  
  
5.  Откройте новое окно запроса и подключитесь к экземпляру SQL Server ядра СУБД на виртуальной машине Azure.  
  
6.  В новое окно запроса вставьте инструкцию CREATE CREDENTIAL с подписанным URL-адресом из раздела 1, а затем выполните этот скрипт.  
  
7.  Повторите шаги 5 и 6 для дополнительных экземпляров SQL Server, которые должны иметь доступ к контейнеру Azure.  

## <a name="3---database-backup-to-url"></a>3. Резервное копирование базы данных по URL-адресу

В этом разделе вы выполните резервное копирование базы данных AdventureWorks2016, размещенной в локальном экземпляре SQL Server 2016, в контейнер Azure, созданный в [разделе 1](#1---create-stored-access-policy-and-shared-access-storage).
  
> [!NOTE]  
> Если необходимо выполнить резервное копирование базы данных SQL Server 2012 с пакетом обновления 1 (SP1) и накопительным пакетом обновления 2 или более поздней версии либо базы данных SQL Server 2014 в этот контейнер Azure, можно воспользоваться устаревшим синтаксисом, который описывается [здесь](/previous-versions/sql/2014/relational-databases/backup-restore/sql-server-backup-to-url?view=sql-server-2014&preserve-view=true) . Таким образом можно выполнить резервное копирование по URL-адресу с помощью синтаксиса WITH CREDENTIAL.  
  
Чтобы выполнить резервное копирование в хранилище BLOB-объектов, выполните указанные ниже действия.  
  
1.  Подключитесь к среде Microsoft SQL Server Management Studio.  
2.  Откройте новое окно запроса и подключитесь к экземпляру SQL Server 2016 ядра СУБД в виртуальной машине Azure.  
3.  Скопируйте и вставьте приведенный ниже скрипт Transact-SQL в окне запроса. Измените в URL-адресе имя учетной записи хранения и контейнер, которые вы указали ранее в разделе 1, а затем выполните этот скрипт.  
  
    ```sql  
  
    -- To permit log backups, before the full database backup, modify the database to use the full recovery model.  
    USE master;  
    ALTER DATABASE AdventureWorks2016  
       SET RECOVERY FULL;  
  
    -- Back up the full AdventureWorks2016 database to the container that you created in section 1  
    BACKUP DATABASE AdventureWorks2016   
       TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_onprem.bak'  
  
    ```  
  
4.  Откройте обозреватель объектов и подключитесь к хранилищу Azure с помощью учетной записи хранения и ключа учетной записи. 
    1.  Разверните узел "Контейнеры", разверните созданный в разделе 1 контейнер и убедитесь, что в нем отображается резервная копия из шага 3.  
  
   ![Подключение к учетной записи хранения Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/connect-to-azure-storage.png)


## <a name="4----restore-database-to-virtual-machine-from-url"></a>4. Восстановление базы данных на виртуальной машине по URL-адресу

В этом разделе вы восстановите базу данных AdventureWorks2016 в экземпляр SQL Server 2016 на виртуальной машине Azure.
  
> [!NOTE]  
> В целях упрощения в этом учебнике для файлов данных и журналов применяется тот же контейнер, который использовался для резервной копии базы данных. В рабочей среде обычно используется несколько контейнеров, а также несколько файлов данных. В SQL Server 2016 можно также распределить резервную копию по нескольким BLOB-объектам, чтобы повысить производительность при резервном копировании большой базы данных.  
  
Чтобы восстановить базу данных AdventureWorks2016 из хранилища BLOB-объектов Azure в экземпляр SQL Server 2016 на виртуальной машине Azure, выполните указанные ниже действия.  
  
1.  Подключитесь к среде Microsoft SQL Server Management Studio.   
2.  Откройте новое окно запроса и подключитесь к экземпляру SQL Server 2016 ядра СУБД в виртуальной машине Azure.   
3.  Скопируйте и вставьте приведенный ниже скрипт Transact-SQL в окне запроса. Измените в URL-адресе имя учетной записи хранения и контейнер, которые вы указали ранее в разделе 1, а затем выполните этот скрипт.  
  
    ```sql  
    -- Restore AdventureWorks2016 from URL to SQL Server instance using Azure blob storage for database files  
    RESTORE DATABASE AdventureWorks2016   
       FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_onprem.bak'   
       WITH  
          MOVE 'AdventureWorks2016_data' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_Data.mdf'  
         ,MOVE 'AdventureWorks2016_log' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_Log.ldf'  
    --, REPLACE  
  
    ```  
  
4.  Откройте обозреватель объектов и подключитесь к экземпляру Azure SQL Server 2016.   
5.  В обозревателе объектов разверните узел "Базы данных" и убедитесь в том, что база данных AdventureWorks2016 восстановлена (при необходимости обновите узел).    
    1. Щелкните AdventureWorks2016 правой кнопкой мыши и выберите "Свойства".
    1. Щелкните "Файлы" и убедитесь в том, что пути к двум файлам базы данных представляют собой URL-адреса, указывающие на BLOB-объекты в контейнере BLOB-объектов Azure (затем щелкните "Отмена").
    
     ![База данных AdventureWorks на виртуальной машине Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/adventureworks-on-azure-vm.png)

    
8.  В обозревателе объектов подключитесь к хранилищу Azure.
    1. Разверните узел "Контейнеры", разверните созданный в разделе 1 контейнер и убедитесь, что в нем есть файлы AdventureWorks2016_Data.mdf и AdventureWorks2016_Log.ldf из шага 3, а также файл резервной копии из раздела 3 (при необходимости обновите узел).  
  
   ![Файлы данных в контейнере Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/data-files-in-container.png)

## <a name="5---backup-database-using-file-snapshot-backup"></a>5. Резервное копирование базы данных с помощью резервного копирования моментальных снимков файлов

В этом разделе вы выполните резервное копирование базы данных AdventureWorks2016 с виртуальной машины Azure с помощью резервной копии моментального снимка файлов. Такой способ позволяет выполнять почти мгновенное резервное копирование с использованием моментальных снимков Azure. Дополнительные сведения о резервных копиях моментальных снимков файлов см. в разделе [Резервные копии моментальных снимков файлов для файлов базы данных в Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
Чтобы создать резервную копию базы данных AdventureWorks2016 с помощью резервного копирования моментальных снимков файлов, выполните указанные ниже действия.  
  
1.  Подключитесь к среде Microsoft SQL Server Management Studio.   
2.  Откройте новое окно запроса и подключитесь к экземпляру SQL Server 2016 ядра СУБД в виртуальной машине Azure.  
3.  Скопируйте и вставьте приведенный ниже скрипт Transact-SQL в окно запроса, а затем выполните его (не закрывайте окно запроса — этот скрипт необходимо будет выполнить еще раз на шаге 5). Эта системная хранимая процедура позволяет просмотреть существующие резервные копии моментальных снимков файлов для каждого файла, входящего в состав указанной базы данных. Обратите внимание на то, что для данной базы данных резервных копий моментальных снимков файлов нет.  
  
    ```sql  
    -- Verify that no file snapshot backups exist  
    SELECT * FROM sys.fn_db_backup_file_snapshots ('AdventureWorks2016');  
    ```  
  
4.  Скопируйте и вставьте приведенный ниже скрипт Transact-SQL в окне запроса. Измените в URL-адресе имя учетной записи хранения и контейнер, которые вы указали ранее в разделе 1, а затем выполните этот скрипт. Обратите внимание на то, как быстро выполняется это резервное копирование.  
  
    ```sql  
    -- Backup the AdventureWorks2016 database with FILE_SNAPSHOT  
    BACKUP DATABASE AdventureWorks2016   
       TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_Azure.bak'   
       WITH FILE_SNAPSHOT;    
    ```  
  
5.  Проверив успешность выполнения скрипта в шаге 4, выполните приведенный ниже скрипт еще раз. Обратите внимание на то, что в результате операции резервного копирования моментальных снимков файлов в шаге 4 были созданы моментальные снимки как файлов данных, так и файлов журналов.  
  
    ```sql  
    -- Verify that two file-snapshot backups exist  
    SELECT * FROM sys.fn_db_backup_file_snapshots ('AdventureWorks2016');  
  
    ```  
  
    ![Результаты для fn_db_backup_file_snapshots с отображением моментальных снимков](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/results-showing-snapshot.png) 
  
6.  В обозревателе объектов в экземпляре SQL Server 2016 на виртуальной машине Azure разверните узел "Базы данных" и убедитесь в том, что база данных AdventureWorks2016 восстановлена в этом экземпляре (при необходимости обновите узел).  
7.  В обозревателе объектов подключитесь к хранилищу Azure.   
8.  Разверните узел "Контейнеры", разверните контейнер, созданный в разделе 1, и проверьте, есть ли в нем файл AdventureWorks2016_Azure.bak из шага 4, а также файл резервной копии из раздела 3 и файлы базы данных из раздела 4 (при необходимости обновите узел).  
  
    ![Резервная копия моментального снимка в Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/snapshot-backup-on-azure.PNG)

## <a name="6----generate-activity-and-backup-log-using-file-snapshot-backup"></a>6. Создание действия и журнала резервного копирования с помощью резервного копирования моментальных снимков файлов

В этом разделе вы создадите действие в базе данных AdventureWorks2016 и будете периодически создавать резервные копии журналов транзакций с помощью резервного копирования моментальных снимков файлов. Дополнительные сведения об использовании резервных копий моментальных снимков файлов см. в разделе [Резервные копии моментальных снимков файлов для файлов базы данных в Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
Чтобы создать действие в базе данных AdventureWorks2016 и периодически создавать резервные копии журналов транзакций с помощью резервного копирования моментальных снимков файлов, выполните указанные ниже действия.  
  
1.  Подключитесь к среде Microsoft SQL Server Management Studio.   
2.  Откройте два новых окна запросов и подключите каждое из них к экземпляру SQL Server 2016 ядра СУБД в виртуальной машине Azure.   
3.  Скопируйте и вставьте приведенный ниже скрипт Transact-SQL в одно из окон запросов, а затем выполните этот скрипт. Обратите внимание на то, что таблица Production.Location содержит 14 строк до того, как в нее будут добавлены новые строки в шаге 4.  
  
    ```sql 
    -- Verify row count at start  
    SELECT COUNT (*) from AdventureWorks2016.Production.Location;    
    ```  
  
4.  Скопируйте и вставьте приведенные ниже скрипты Transact-SQL в два отдельных окна запросов. Измените в URL-адресе имя учетной записи хранения и контейнер, которые вы указали в разделе 1, а затем одновременно выполните эти скрипты в отдельных окнах запросов. На выполнение скриптов потребуется приблизительно семь минут.  
  
    ```sql  
    -- Insert 30,000 new rows into the Production.Location table in the AdventureWorks2014 database in batches of 75  
    DECLARE @count INT=1, @inner INT;  
    WHILE @count < 400  
       BEGIN  
          BEGIN TRAN;  
             SET @inner =1;  
                WHILE @inner <= 75  
                   BEGIN;  
                      INSERT INTO AdventureWorks2016.Production.Location    
                         (Name, CostRate, Availability, ModifiedDate)   
                            VALUES (NEWID(), .5, 5.2, GETDATE());  
                      SET @inner = @inner + 1;  
                   END;  
          COMMIT;  
       WAITFOR DELAY '00:00:01';   
       SET @count = @count + 1;  
       END;  
    SELECT COUNT (*) from AdventureWorks2014.Production.Location;    
    ```  
  
    ```sql  
    --take 7 transaction log backups with FILE_SNAPSHOT, one per minute, and include the row count and the execution time in the backup file name   
    DECLARE @count INT=1, @device NVARCHAR(120), @numrows INT;  
    WHILE @count <= 7  
       BEGIN  
             SET @numrows = (SELECT COUNT (*) FROM AdventureWorks2016.Production.Location);  
             SET @device = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/tutorial-' + CONVERT (varchar(10),@numrows) + '-' + FORMAT(GETDATE(), 'yyyyMMddHHmmss') + '.bak';  
             BACKUP LOG AdventureWorks2016 TO URL = @device WITH FILE_SNAPSHOT;  
             SELECT * from sys.fn_db_backup_file_snapshots ('AdventureWorks2016');  
          WAITFOR DELAY '00:1:00';   
             SET @count = @count + 1;  
       END;  
    ```  
  
5.  Просмотрите выходные данные первого скрипта и обратите внимание на то, что последняя строка теперь имеет номер 29 939.  
  
    ![Отображение числа строк 29 939](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/29-thousand-rows.png)
  
6.  Просмотрите выходные данные второго скрипта и обратите внимание на то, что при каждом выполнении инструкции BACKUP LOG для каждого файла создаются два моментальных снимка файлов: один снимок файла журнала и один снимок файла данных. После завершения выполнения второго скрипта должно быть создано всего 16 моментальных снимков файлов (по 8 для каждого файла базы данных): по одному при каждом выполнении инструкции BACKUP DATABASE и еще по одному при каждом выполнении инструкции BACKUP LOG.  
  
   ![Результаты резервного копирования моментального снимка](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/snapshot-back-results.png)
  
7.  В обозревателе объектов подключитесь к хранилищу Azure.  
  
8.  Разверните узел "Контейнеры", а также контейнер, созданный в разделе 1, и убедитесь в том, что появилось семь новых файлов резервных копий и файлы данных из предыдущих разделов (при необходимости обновите узел).  
  
    ![Несколько моментальных снимков в контейнере Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/tutorial-snapshots-in-container.png)

## <a name="7---restore-a-database-to-a-point-in-time"></a>7. Восстановление базы данных на момент времени

В этом разделе вы восстановите базу данных AdventureWorks2016 на определенный момент времени между двумя резервными копиями журнала транзакций.  
  
Чтобы выполнить восстановление на определенный момент времени из традиционных резервных копий, потребуется полная резервная копия базы данных, возможно, разностная резервная копия и все файлы журналов транзакций вплоть до того момента, на который необходимо выполнить восстановление, и сразу после него. При использовании резервных копий моментальных снимков файлов требуются только два ближайших файла резервных копий журнала с обеих сторон от целевой точки восстановления. Требуются только два резервных набора моментальных снимков файлов, так как каждая операция резервного копирования журнала создает моментальный снимок каждого файла базы данных (то есть файла данных и файла журнала).  
  
Чтобы восстановить базу данных на определенный момент времени из резервных наборов моментальных снимков файлов, выполните указанные ниже действия.  
  
1.  Подключитесь к среде Microsoft SQL Server Management Studio.  
2.  Откройте новое окно запроса и подключитесь к экземпляру SQL Server 2016 ядра СУБД в виртуальной машине Azure.   
3.  Скопируйте и вставьте приведенный ниже скрипт Transact-SQL в окно запроса, а затем выполните его. Убедитесь в том, что таблица Production.Location содержит 29 939 строк, прежде чем восстанавливать ее на момент времени, когда было меньше строк, в шаге 5.  
  
    ```sql
    -- Verify row count at start  
    SELECT COUNT (*) from AdventureWorks2016.Production.Location   
    ```  
  
    ![Отображение числа строк 29 939](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/29-thousand-rows.png) 
  
4.  Скопируйте и вставьте приведенный ниже скрипт Transact-SQL в окне запроса. Выберите два смежных файла резервных копий журнала и укажите вместо их имен дату и время, требуемые для этого скрипта. В URL-адресе измените имя учетной записи хранения и контейнер, которые вы указали в разделе 1, укажите имена файлов для первой и второй резервных копий, укажите время STOPAT в формате "June 26, 2018 01:48 PM" (26 июня, 2018 13:48), а затем выполните этот скрипт. Выполнение скрипта займет несколько минут.  
  
    ```sql  
    -- restore and recover to a point in time between the times of two transaction log backups, and then verify the row count  
    ALTER DATABASE AdventureWorks2016 SET SINGLE_USER WITH ROLLBACK IMMEDIATE;  
    RESTORE DATABASE AdventureWorks2016   
       FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<firstbackupfile>.bak'   
       WITH NORECOVERY,REPLACE;  
    RESTORE LOG AdventureWorks2016   
       FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<secondbackupfile>.bak'    
       WITH RECOVERY, STOPAT = 'June 26, 2018 01:48 PM';  
    ALTER DATABASE AdventureWorks2016 set multi_user;  
    -- get new count  
    SELECT COUNT (*) FROM AdventureWorks2016.Production.Location ;
    ```  
  
5.  Просмотрите выходные данные. Обратите внимание на то, что после восстановления число строк равно 18 389 — это число строк межу резервными копиями журнала 5 и 6 (ваше число строк может быть другим).  
  
    ![18-thousand-rows.JPG](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/18-thousand-rows.png)

## <a name="8----restore-as-new-database-from-log-backup"></a>8. Восстановление новой базы данных из резервной копии журнала

В этом разделе вы восстановите базу данных AdventureWorks2016 в виде новой базы данных из резервной копии журнала транзакций на основе моментального снимка файлов.  
  
В этом сценарии вы восстановите базу данных в экземпляре SQL Server на другой виртуальной машине, предназначенной для бизнес-анализа и создания отчетов. Восстановление в другом экземпляре, размещенном в другой виртуальной машине, позволяет перенести нагрузку на выделенную виртуальную машину, специально предназначенную для этой цели, и снизить требования к ресурсам, предъявляемые к системе обработки транзакций.  
  
Восстановление из резервной копии журнала транзакций, созданной посредством резервного копирования моментальных снимков файлов, выполняется очень быстро — существенно быстрее, чем при использовании традиционных потоковых резервных копий. В случае с традиционными потоковыми резервными копиями вам потребовалось бы использовать полную резервную копию базы данных, а также, возможно, разностную резервную копию и все или часть резервных копий журнала транзакций (либо новую полную резервную копию базы данных). При использовании же резервных копий журнала на основе моментальных снимков файлов требуется только самая последняя резервная копия журнала (либо любая другая резервная копия журнала, либо две смежные резервные копии журнала для восстановления на определенный момент времени). Если точнее, требуется только один резервный набор моментальных снимков файлов, так как каждая операция резервного копирования журнала с помощью моментальных снимков файлов создает моментальный снимок каждого файла базы данных (то есть файла данных и файла журнала).  
  
Чтобы выполнить восстановление в новую базу данных из резервной копии журнала транзакций, созданной посредством резервного копирования моментальных снимков файлов, выполните указанные ниже действия.  
  
1.  Подключитесь к среде Microsoft SQL Server Management Studio.   
2.  Откройте новое окно запроса и подключитесь к экземпляру SQL Server 2016 ядра СУБД в виртуальной машине Azure.  
  
    > [!NOTE]  
    > Если это не та виртуальная машина Azure, которую вы использовали в предыдущих разделах, выполните инструкции из раздела [2. Создание учетных данных SQL Server с помощью подписанного URL-адреса](#2---create-a-sql-server-credential-using-a-shared-access-signature). Если вы хотите восстановить данные в другой контейнер, выполните для нового контейнера действия из раздела [1. Создание хранимой политики доступа и хранилища с общим доступом](#1---create-stored-access-policy-and-shared-access-storage).  
  
3.  Скопируйте и вставьте приведенный ниже скрипт Transact-SQL в окне запроса. Выберите файл резервной копии журнала, который нужно использовать. В URL-адресе измените имя учетной записи хранения и контейнер, которые вы указали в разделе 1, укажите имя файла резервной копии журнала, а затем выполните этот скрипт.  
  
    ```sql  
    -- restore as a new database from a transaction log backup file  
    RESTORE DATABASE AdventureWorks2016_EOM   
        FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<logbackupfile.bak>'    
        WITH MOVE 'AdventureWorks2016_data' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2014_EOM_Data.mdf'  
       , MOVE 'AdventureWorks2016_log' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2014_EOM_Log.ldf'  
       , RECOVERY  
    --, REPLACE   
    ```  
  
4.  Просмотрите выходные данные, чтобы проверить успешность восстановления.   
5.  В обозревателе объектов подключитесь к хранилищу Azure.    
6.  Разверните узел "Контейнеры", разверните созданный в разделе 1 контейнер (при необходимости обновите его) и убедитесь в том, что в нем появились новые файлы данных и журналов, а также BLOB-объекты из предыдущих разделов.  
  
    ![Контейнер Azure с файлами данных и журналов для новой базы данных](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/new-db-in-azure-container.png)

## <a name="9---manage-backup-sets-and-file-snapshot-backups"></a>9. Управление резервными наборами данных и резервными копиями моментальных снимков файлов

В этом разделе вы удалите резервный набор данных с помощью системной хранимой процедуры [sp_delete_backup (Transact-SQL)](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md). Эта процедура удаляет файл резервной копии и моментальный снимок файла для каждого файла базы данных, связанного с резервным набором данных.  
  
> [!NOTE]  
> Если вы попытаетесь удалить резервный набор данных, просто удалив файл резервной копии из контейнера BLOB-объектов Azure, будет удален только сам файл резервной копии, а связанные с ним моментальные снимки файлов сохранятся. Если у вас возникла такая ситуация, используйте системную функцию [sys.fn_db_backup_file_snapshots (Transact-SQL)](../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md) для определения URL-адреса потерянных моментальных снимков файлов, а затем используйте системную хранимую процедуру [sp_delete_backup_file_snapshot (Transact-SQL)](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md) для удаления каждого утерянного моментального снимка файла. Дополнительные сведения см. в разделе  [Резервные копии моментальных снимков файлов для файлов базы данных в Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
Чтобы удалить резервный набор моментальных снимков файлов, выполните указанные ниже действия.  
  
1.  Подключитесь к среде Microsoft SQL Server Management Studio.  
2.  Откройте новое окно запроса и подключитесь к экземпляру SQL Server 2016 ядра СУБД в виртуальной машине Azure (или к любому экземпляру SQL Server 2016 с разрешениями на чтение и запись в этот контейнер).   
3.  Скопируйте и вставьте приведенный ниже скрипт Transact-SQL в окне запроса. Выберите резервную копию журнала, которую нужно удалить вместе со связанными моментальными снимками файлов. В URL-адресе измените имя учетной записи хранения и контейнер, которые вы указали в разделе 1, укажите имя файла резервной копии журнала, а затем выполните этот скрипт.  
  
  ```sql
  sys.sp_delete_backup 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/tutorial-21764-20181003205236.bak';  
  ```  
  
4.  В обозревателе объектов подключитесь к хранилищу Azure.  
5.  Разверните узел "Контейнеры", разверните созданный в разделе 1 контейнер и убедитесь в том, что файл резервной копии из раздела 3 больше не отображается в этом контейнере (при необходимости обновите узел).  
  
    ![Контейнер Azure, в котором видно, что большой двоичный объект резервной копии журнала удален](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/deleted-backup-snapshot.png)
  
6.  Чтобы убедиться в том, что оба моментальных снимка файлов были удалены, скопируйте и вставьте приведенный ниже скрипт Transact-SQL в окно запроса, а затем выполните его.  
  
    ```sql  
    -- verify that two file snapshots have been removed  
    SELECT * from sys.fn_db_backup_file_snapshots ('AdventureWorks2016');   
    ```  
    ![Область результатов, где подтверждается удаление двух моментальных снимков файлов](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/results-of-two-deleted-snapshot-files.png)

## <a name="10---remove-resources"></a>10. Удаление ресурсов

Когда вы закончите работу с этим руководством, не забудьте в целях экономии ресурсов удалить группу ресурсов, созданную в этом руководстве. 

Чтобы удалить группу ресурсов, выполните следующий код PowerShell:

  ```powershell
  # Define global variables for the script  
  $prefixName = '<prefix name>'  # should be the same as the beginning of the tutorial
  
  # Set a variable for the name of the resource group you will create or use  
  $resourceGroupName=$prefixName + 'rg'   
  
  # Adds an authenticated Azure account for use in the session   
  Connect-AzAccount    
  
  # Set the tenant, subscription and environment for use in the rest of   
  Set-AzContext -SubscriptionId $subscriptionID    
    
  # Remove the resource group
  Remove-AzResourceGroup -Name $resourceGroupName   
  ```


  
## <a name="see-also"></a>См. также

[Файлы данных SQL Server в Microsoft Azure](../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md)  
[Резервные копии моментальных снимков файлов для файлов базы данных в Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)  
[Резервное копирование в SQL Server по URL-адресу](../relational-databases/backup-restore/sql-server-backup-to-url.md) 
[Подписанные URL-адреса. Часть 1. Общие сведения о модели SAS](/azure/storage/common/storage-sas-overview)  
[Create Container (Создание контейнера)](/rest/api/storageservices/Create-Container)  
[Set Container ACL](/rest/api/storageservices/Set-Container-ACL)  
[Get Container ACL](/rest/api/storageservices/Get-Container-ACL)
[Учетные данные (компонент Database Engine)](../relational-databases/security/authentication-access/credentials-database-engine.md)  
[CREATE CREDENTIAL &#40;Transact-SQL&#41;](../t-sql/statements/create-credential-transact-sql.md)  
[sys.credentials (Transact-SQL)](../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)  
[sp_delete_backup (Transact-SQL)](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md)  
[sys.fn_db_backup_file_snapshots (Transact-SQL)](../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md)  
[sp_delete_backup_file_snapshot &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md) [Резервные копии моментальных снимков файлов для файлов базы данных в Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)