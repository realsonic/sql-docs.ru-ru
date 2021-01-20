---
title: Образцы баз данных AdventureWorks
description: Выполните эти инструкции, чтобы скачать и установить образцы баз данных AdventureWorks для SQL Server с помощью Transact-SQL (T-SQL), SQL Server Management Studio (SSMS) или Azure Data Studio.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.date: 06/16/2020
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: ec38d7eeed89d03bab4ebd92446517542e2e51a0
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597005"
---
# <a name="adventureworks-sample-databases"></a>Образцы баз данных AdventureWorks
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

В этой статье содержатся прямые ссылки на загрузку образцов баз данных AdventureWorks, а также инструкции по их восстановлению в SQL Server и базе данных SQL Azure. 

Дополнительные сведения о примерах см. в [репозитории примеров GitHub](https://github.com/microsoft/sql-server-samples/tree/master/samples/databases). 

## <a name="prerequisites"></a>Предварительные требования

- [SQL Server](https://www.microsoft.com/evalcenter/evaluate-sql-server-2019) или [база данных SQL Azure](https://azure.microsoft.com/services/sql-database/)
- [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md) или [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)


## <a name="download-backup-files"></a>Скачать файлы резервных копий 

Используйте эти ссылки, чтобы скачать соответствующий образец базы данных для вашего сценария. 

- Данные **OLTP** представляют собой наиболее типичные рабочие нагрузки обработки транзакций в сети. 
- Данные **хранилища данных (DW)** представляют собой рабочие нагрузки хранилищ данных. 
- **Облегченные (lt)** данные — это упрощенная и урезанныеная версия примера **OLTP** . 

Если вы не уверены, что вам нужно, начните с версии OLTP, соответствующей версии SQL Server. 

|**OLTP** |**Хранилище данных** |**упрощенный интерфейс,**|
|---------|---------|---------|
|[AdventureWorks2019. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2019.bak)|[AdventureWorksDW2019. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2019.bak)|[AdventureWorksLT2019. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2019.bak)|
|[AdventureWorks2017. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2017.bak)|[AdventureWorksDW2017. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2017.bak)|[AdventureWorksLT2017. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2017.bak)|
|[AdventureWorks2016. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2016.bak)|[AdventureWorksDW2016. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2016.bak)|[AdventureWorksLT2016. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2016.bak)|
|[AdventureWorks2016_EXT bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2016_EXT.bak)|[AdventureWorksDW2016_EXT bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2016_EXT.bak)| Н/Д |
|[AdventureWorks2014. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2014.bak)|[AdventureWorksDW2014. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2014.bak)|[AdventureWorksLT2014. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2014.bak)|
|[AdventureWorks2012. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2012.bak)|[AdventureWorksDW2012. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2012.bak)|[AdventureWorksLT2012. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2012.bak)|
|[AdventureWorks2008R2. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks2008r2/adventure-works-2008r2-oltp.bak)| [AdventureWorksDW2008R2. bak](https://github.com/microsoft/sql-server-samples/releases/download/adventureworks2008r2/adventure-works-2008r2-dw.bak) | Н/Д |

Дополнительные файлы можно найти непосредственно на сайте GitHub: 

- [SQL Server 2014-2019](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks)
- [SQL Server 2012](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks2012)
- [SQL Server 2008 и 2008R2](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks2008r2)


## <a name="restore-to-sql-server"></a>Восстановление в SQL Server 

Файл можно использовать `.bak` для восстановления образца базы данных в экземпляре SQL Server. Это можно сделать с помощью команды [Restore (Transact-SQL)](../t-sql/statements/restore-statements-transact-sql.md) или графического интерфейса (GUI) в [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md) или [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md).

# <a name="sql-server-management-studio-ssms"></a>[SQL Server Management Studio (SSMS)](#tab/ssms)

Если вы не знакомы с SQL Server Management Studio (SSMS), можно увидеть [запрос connect &](../ssms/quickstarts/ssms-connect-query-sql-server.md) , чтобы приступить к работе. 

Чтобы восстановить базу данных в SQL Server Management Studio, выполните следующие действия.

1. Скачайте соответствующий `.bak` файл из одной из ссылок, указанных в разделе [Загрузка файлов резервных копий](#download-backup-files) .
2. Переместите `.bak` файл в расположение резервной копии SQL Server. Это зависит от расположения установки, имени экземпляра и версии SQL Server. Например, расположением по умолчанию для экземпляра по умолчанию SQL Server 2019 является:

   `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup`. 

3. Откройте SQL Server Management Studio (SSMS) и подключитесь к SQL Server в. 
4. Щелкните правой кнопкой мыши **базы данных** в **обозревателе объектов**,  >  **восстановить базу данных...** , чтобы запустить мастер **восстановления базы данных** . 

   :::image type="content" source="media/adventureworks-install-configure/restore-db-ssms.png" alt-text="Снимок экрана, показывающий, как выбрать восстановление базы данных, щелкнув правой кнопкой мыши базы данных в обозревателе объектов и выбрав пункт RESTORE DATABASE (восстановить базу данных).":::


1. Выберите **устройство** , а затем нажмите кнопку с многоточием **(...)** , чтобы выбрать устройство. 
1. Нажмите кнопку **Добавить** , а затем выберите `.bak` файл, который вы недавно переместили в это расположение. Если вы переместили файл в это расположение, но не можете увидеть его в мастере, обычно это указывает на проблемы с разрешениями — SQL Server или пользователь, выполнивший вход в SQL Server, не имеет разрешения на доступ к этому файлу в этой папке. 
1. Нажмите кнопку **ОК** , чтобы подтвердить выбор резервной копии базы данных, и закройте окно **Выбор устройств резервного копирования** . 
1. Перейдите на вкладку **файлы** , чтобы подтвердить **Восстановление как** расположение и имена файлов, совпадающие с предполагаемым расположением и именами файлов в мастере **восстановления базы данных** . 
1. Чтобы восстановить базу данных, нажмите кнопку **ОК**. 

   :::image type="content" source="media/adventureworks-install-configure/restore-db-wizard-ssms.png" alt-text="Снимок экрана, показывающий окно восстановления базы данных с выделенным резервным набором данных и параметром ОК.":::

Дополнительные сведения о восстановлении базы данных SQL Server см. в разделе [Восстановление резервной копии базы данных с помощью SSMS](../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md).

# <a name="transact-sql-t-sql"></a>[Transact-SQL (T-SQL)](#tab/tsql)

Образец базы данных можно восстановить с помощью Transact-SQL (T-SQL). Ниже приведен пример восстановления AdventureWorks2019, но имя базы данных и путь к файлу установки могут различаться в зависимости от среды. 

Чтобы восстановить AdventureWorks2019 в **Windows**, измените значения в соответствии с вашей средой, а затем выполните следующую команду TRANSACT-SQL (T-SQL):


```sql
USE [master]
RESTORE DATABASE [AdventureWorks2019] 
FROM  DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup\AdventureWorks2019.bak' 
WITH  FILE = 1,  NOUNLOAD,  STATS = 5
GO

```

Чтобы восстановить AdventureWorks2019 в **Linux**, измените путь файловой системы Windows на Linux, а затем выполните следующую команду TRANSACT-SQL (T-SQL): 


```sql
USE [master]
RESTORE DATABASE [AdventureWorks2019]
FROM DISK = '/var/opt/mssql/backup/AdventureWorks2019.bak'
WITH MOVE 'AdventureWorks2017' TO '/var/opt/mssql/data/AdventureWorks2019.mdf',
MOVE 'AdventureWorks2017_log' TO '/var/opt/mssql/data/AdventureWorks2019_log.ldf',
FILE = 1,  NOUNLOAD,  STATS = 5
GO
```

# <a name="azure-data-studio"></a>[Azure Data Studio](#tab/data-studio)

Если вы не знакомы с [Azure Data Studio Studio](../azure-data-studio/download-azure-data-studio.md), вы можете увидеть [запрос Connect &](../azure-data-studio/quickstart-sql-server.md) , чтобы приступить к работе.

Чтобы восстановить базу данных в Azure Data Studio, выполните следующие действия.

1. Скачайте соответствующий `.bak` файл из одной из ссылок, указанных в разделе [Загрузка файлов резервных копий](#download-backup-files) .
1. Переместите `.bak` файл в расположение резервной копии SQL Server. Это зависит от расположения установки, имени экземпляра и версии SQL Server. Например, расположением по умолчанию для экземпляра по умолчанию SQL Server 2019 является:

    `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup`.

1. Откройте Azure Data Studio Studio и подключитесь к экземпляру SQL Server.
1. Щелкните правой кнопкой мыши сервер и выберите пункт **Управление**.

   :::image type="content" source="media/adventureworks-install-configure/ads-manage.png" alt-text="Снимок экрана, показывающий Azure Data Studio с выделенным и вызываемым параметром управления.":::

1. Выбор **восстановления**

   :::image type="content" source="media/adventureworks-install-configure/ads-restore-database.png" alt-text="В верхнем меню выберите восстановить, чтобы восстановить базу данных.":::

1. На вкладке **Общие** введите значения, перечисленные в разделе **источник**.
    1. В разделе **восстановить из** выберите *файл резервной копии*.
    1. В разделе **путь к файлу резервной копии** выберите расположение, в котором был сохранен файл. bak. 
    
   :::image type="content" source="media/adventureworks-install-configure/ads-source.png" alt-text="Выберите путь к файлу резервной копии":::
    
    При этом автоматически заполняются остальные поля, такие как **база данных**, **Целевая база данных** и **восстановление в**. 

   :::image type="content" source="media/adventureworks-install-configure/ads-destination-restore-plan.png" alt-text="После выбора пути к файлу резервной копии остальные поля автоматически заполняются":::

1. Выберите **восстановить** , чтобы восстановить базу данных. 

   :::image type="content" source="media/adventureworks-install-configure/ads-restore.png" alt-text="Когда будете готовы, нажмите кнопку восстановить, чтобы восстановить базу данных.":::

---

## <a name="deploy-to-azure-sql-database"></a>Развертывание в базе данных SQL Azure

Вы можете просмотреть образцы данных базы данных SQL Azure двумя способами. Вы можете использовать пример при создании новой базы данных или развернуть базу данных из SQL Server непосредственно в Azure с помощью SQL Server Management Studio (SSMS).

Чтобы получить демонстрационные данные для Управляемый экземпляр Azure SQL, см. статью [Восстановление данных в среде sql управляемый экземпляр](/azure/azure-sql/managed-instance/restore-sample-database-quickstart). 

### <a name="deploy-new-sample-database"></a>Развертывание нового образца базы данных

При создании новой базы данных в базе данных SQL Azure можно создать пустую базу данных или образец базы данных. 

Выполните следующие действия, чтобы использовать образец базы данных для создания новой базы данных. 

1. Подключитесь к портал Azure.
1. Выберите **создать ресурс** в верхнем левом углу панели навигации. 
1. Выберите **базы данных** , а затем выберите **база данных SQL**. 
1. Заполните запрошенные данные, чтобы создать базу данных. 
1. На вкладке **Дополнительные параметры** выберите **образец** в качестве существующих данных в разделе **источник данных**: 

   :::image type="content" source="media/adventureworks-install-configure/deploy-sample-to-azure.png" alt-text="Выбор примера в качестве источника данных на вкладке &quot;Дополнительные параметры&quot; в портал Azure при создании базы данных SQL Azure":::

1. Выберите **создать** , чтобы создать новую базу данных SQL, которая является восстановленной копией базы данных AdventureWorksLT. 


### <a name="deploy-database-from-sql-server"></a>Развертывание базы данных из SQL Server

SQL Server Management Studio предоставляет возможность развертывания базы данных непосредственно в базе данных SQL Azure. В настоящее время этот метод не обеспечивает проверку данных, поэтому он предназначен для разработки и тестирования и не должен использоваться для рабочей среды. 

Чтобы развернуть образец базы данных из SQL Server в базу данных SQL Azure, выполните следующие действия.

1. Подключитесь к SQL Server в SQL Server Management Studio. 
1. Если вы еще не сделали этого, [восстановите образец базы данных в SQL Server](#restore-to-sql-server). 
1. Щелкните правой кнопкой мыши восстановленную базу данных в **обозревателе объектов**  >  **задачи**  >  **развертывание базы данных в база данных SQL Microsoft Azure...**. 

   :::image type="content" source="media/adventureworks-install-configure/deploy-db-to-azure.png" alt-text="Выберите развертывание базы данных, чтобы База данных SQL Microsoft Azure, щелкнув правой кнопкой мыши базу данных и выбрав пункт задачи.":::

1. Следуйте указаниям мастера, чтобы подключиться к базе данных SQL Azure и развернуть базу данных. 


## <a name="creation-scripts"></a>Скрипты создания

Вместо восстановления базы данных также можно использовать скрипты для создания баз данных AdventureWorks независимо от версии. 

Приведенные ниже скрипты можно использовать для создания всей базы данных AdventureWorks. 

- [ZIP-файл скриптов OLTP](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks-oltp-install-script.zip)
- [ZIP-скрипты сценариев AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW-data-warehouse-install-script.zip)

Дополнительные сведения об использовании сценариев можно найти на сайте [GitHub](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks). 

## <a name="next-steps"></a>Следующие шаги

После восстановления образца базы данных используйте следующие учебники, чтобы приступить к работе с SQL Server. 


[Учебники по SQL Server ядру СУБД](../relational-databases/database-engine-tutorials.md)   
[Подключение и запрос с помощью SQL Server Management Studio (SSMS)](../ssms/quickstarts/ssms-connect-query-sql-server.md)   
[Подключение и запрос с помощью Azure Data Studio](../ssms/quickstarts/ssms-connect-query-sql-server.md)