---
title: Заголовок и журнал резервного копирования | Документация Майкрософт
description: В базе данных msdb хранится журнал операций резервного копирования и восстановления SQL Server экземпляра сервера. Сведения об инструкциях Transact-SQL, обращающихся к журналу резервного копирования.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- backup headers [SQL Server]
- history tables [SQL Server]
- file restores [SQL Server], status information
- backup sets [SQL Server], status information
- listing backed up databases
- status information [SQL Server], backups
- backing up [SQL Server], viewing backup sets
- restoring [SQL Server], history tables
- restoring databases [SQL Server], status information
- backups [SQL Server], status information
- headers [SQL Server]
- media headers [SQL Server]
- backup history tables [SQL Server]
- viewing backup information
- restoring files [SQL Server], viewing backup information
- restoring databases [SQL Server], history tables
- displaying backup information
- restoring files [SQL Server], status information
- historical information [SQL Server], backups
- database restores [SQL Server], history tables
- restore history tables [SQL Server]
- listing backed up files
ms.assetid: 799b9934-0ec2-4f43-960b-5c9653f18374
author: cawrites
ms.author: chadam
ms.openlocfilehash: 49788f0329cb55106fcb7ce85746fca0860f0ee3
ms.sourcegitcommit: b652ff2f0f7edbb5bd2f8fdeac56348e4d84f8fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98112666"
---
# <a name="backup-history-and-header-information-sql-server"></a>Журнал и сведения о заголовке резервной копии (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Полный журнал резервных копий и операций восстановления на экземпляре сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] хранится в базе данных **msdb** . В этом разделе рассказывается о таблицах журнала восстановления, а также об инструкциях [!INCLUDE[tsql](../../includes/tsql-md.md)] , которые используются для доступа к журналам резервного копирования. В этом разделе также обсуждается удобство составления списка файлов базы данных и журнала транзакций и использование данных в заголовке носителя в сравнении с использованием данных в заголовке резервной копии.  
  
> [!IMPORTANT]  
>  Чтобы снизить риск потери недавних изменений, следует чаще создавать резервные копии базы данных **msdb** и журнала. Сведения о том, резервную копию какой системной базы данных нужно создать, см. в разделе [Резервное копирование и восстановление системных баз данных (SQL Server)](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md).  
  
 **В этом разделе:**  
  
-   [Таблицы журналов резервного копирования и восстановления](#BnRHistoryTables)  
  
-   [Инструкции Transact-SQL для доступа к журналу резервного копирования](#TsqlStatementsForBackupHistory)  
  
-   [Файлы базы данных и журнала транзакций](#ListDbTlogFiles)  
  
-   [Данные в заголовке носителя](#MediaHeader)  
  
-   [Данные в заголовке резервной копии](#BackupHeader)  
  
-   [Сравнение данных в заголовке носителя и в заголовке резервной копии](#CompareMediaHeaderBackupHeader)  
  
-   [Проверки резервных копий](#Verification)  
  
-   [Связанные задачи](#RelatedTasks)  
  
##  <a name="backup-and-restore-history-tables"></a><a name="BnRHistoryTables"></a> Таблицы журналов резервного копирования и восстановления  
 В этом разделе рассказывается о журнальных таблицах, в которых в системной базе данных **msdb** хранятся метаданные резервного копирования и восстановления.  
  
|Таблица журнала|Описание|  
|-------------------|-----------------|  
|[backupfile;](../../relational-databases/system-tables/backupfile-transact-sql.md)|Содержит по одной строке для каждого файла данных или журнала, подвергаемого резервному копированию.|  
|[backupfilegroup](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)|Содержит по одной строке для каждой файловой группы в резервном наборе данных.|  
|[backupmediafamily;](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)|Содержит по одной строке для каждого семейства носителей. Если семейство носителей хранится на зеркальном наборе носителей, семейство имеет отдельную строку для каждого зеркала в наборе носителей.|  
|[backupmediaset;](../../relational-databases/system-tables/backupmediaset-transact-sql.md)|Содержит по одной строке для каждого резервного набора носителей.|  
|[backupset;](../../relational-databases/system-tables/backupset-transact-sql.md)|Содержит по одной строке для каждого резервного набора данных.|  
|[restorefile;](../../relational-databases/system-tables/restorefile-transact-sql.md)|Содержит по одной строке для каждого восстановленного файла. Это файлы, восстановленные неявно по имени файловой группы.|  
|[restorefilegroup;](../../relational-databases/system-tables/restorefilegroup-transact-sql.md)|Содержит по одной строке для каждой восстановленной файловой группы.|  
|[restorehistory.](../../relational-databases/system-tables/restorehistory-transact-sql.md)|Содержит по одной строке для каждой операции восстановления.|  
  
> [!NOTE]  
>  При восстановлении изменяются таблицы журналов резервного копирования и восстановления.  
  
##  <a name="transact-sql-statements-for-accessing-backup-history"></a><a name="TsqlStatementsForBackupHistory"></a> Инструкции Transact-SQL для доступа к журналу резервного копирования  
 Инструкции восстановления данных соответствуют сведениям, сохраненным в некоторых таблицах журналов резервного копирования.  
  
> [!IMPORTANT]  
>  Инструкциям Transact-SQL RESTORE FILELISTONLY, RESTORE HEADERONLY, RESTORE LABELONLY и RESTORE VERIFYONLY требуется разрешение CREATE DATABASE. Тем самым обеспечивается более надежная защита файлов резервных копий и данных, чем в предыдущих версиях. Дополнительные сведения об этом разрешении см. в разделе[ Разрешения базы данных GRANT (Transact-SQL)](../../t-sql/statements/grant-database-permissions-transact-sql.md).  
  
|Информационная инструкция|Таблица журнала резервного копирования|Описание|  
|---------------------------|--------------------------|-----------------|  
|[RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)|[backupfile;](../../relational-databases/system-tables/backupfile-transact-sql.md)|Возвращает результирующий набор со списком файлов базы данных и журнала, которые содержит указанный резервный набор данных.<br /><br /> Дополнительные сведения см. далее в разделе «Составление списка файлов базы данных и журналов транзакций».|  
|[инструкция RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)|[backupset;](../../relational-databases/system-tables/backupset-transact-sql.md)|Извлекает все данные заголовка резервной копии для всех резервных наборов данных в определенном устройстве резервного копирования. Результатом выполнения RESTORE HEADERONLY является результирующий набор.<br /><br /> Дополнительные сведения см. далее в разделе «Просмотр данных заголовка резервной копии».|  
|[RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)|[backupmediaset;](../../relational-databases/system-tables/backupmediaset-transact-sql.md)|Возвращает результирующий набор, который содержит сведения о резервном носителе в указанном устройстве резервного копирования.<br /><br /> Дополнительные сведения см. далее в разделе «Просмотр данных заголовка носителя».|  
  
##  <a name="database-and-transaction-log-files"></a><a name="ListDbTlogFiles"></a> Файлы базы данных и журнала транзакций  
 При подготовке списка файлов базы данных и журнала транзакций в резервной копии отображаются сведения о логическом имени, физическом имени, типе файла (база данных или журнал), членстве в файловой группе, размере файла (в байтах), максимально допустимом размере файла и заранее заданный рост файла (в байтах). Эти сведения позволяют в следующих ситуациях определять имена файлов в резервной копии базы данных перед восстановлением.  
  
-   Был утрачен дисковый накопитель, который содержал один или несколько файлов базы данных.  
  
     По списку файлов в резервной копии базы данных можно определить затронутые файлы, затем восстановить эти файлы на другом диске при восстановлении всей базы данных или восстановить только эти файлы и применить любые резервные копии журналов транзакций, созданные со времени резервного копирования базы данных.  
  
-   База данных из одного сервера восстанавливается на другом сервере, но на сервере отсутствуют структура каталогов и сопоставление носителей.  
  
     Составление списка файлов в резервной копии позволяет определить затронутые файлы. Например, резервная копия содержит файл, который нужно восстановить на диск E:, но на целевом сервере диск E: отсутствует. При восстановлении этот файл необходимо перенести в другое расположение, например на диск Z:.  
  
##  <a name="media-header-information"></a><a name="MediaHeader"></a> Данные в заголовке носителя  
 При просмотре данных в заголовке носителя отображаются сведения о самом носителе, а не о резервных копиях на нем. К отображаемым сведениям из заголовка носителя относятся имя носителя, описание, имя программы, с помощью которой был создан заголовок носителя, а также дата записи заголовка носителя.  
  
> [!NOTE]  
>  Просмотр данных в заголовке носителя занимает мало времени.  
  
 Дополнительные сведения см. далее в подразделе [Сравнение данных в заголовках носителя и резервной копии](#CompareMediaHeaderBackupHeader)далее в этом разделе.  
  
##  <a name="backup-header-information"></a><a name="BackupHeader"></a> Данные в заголовке резервной копии  
 В заголовке резервной копии отображаются сведения о всех резервных наборах данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и отличных от [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на носителях. К отображаемым сведениям относятся типы применяемых устройств резервного копирования, типы резервных копий (например: копия базы данных, транзакции, файла или разностная копия базы данных), дата-время начала и конца резервного копирования. С помощью этих сведений можно определить, какой резервный набор данных на ленте подлежит восстановлению или какие резервные копии находятся на носителе.  
  
> [!NOTE]  
>  Просмотр данных в заголовке носителя для магнитных лент большой емкости может занимать длительное время, так как для отображения информации обо всех резервных копиях на носителе необходимо просмотреть весь носитель.  
  
 Дополнительные сведения см. далее в подразделе [Сравнение данных в заголовках носителя и резервной копии](#CompareMediaHeaderBackupHeader)далее в этом разделе.  
  
### <a name="which-backup-set-to-restore"></a>Резервные наборы, которые необходимо восстановить  
 Сведения из заголовка резервной копии можно использовать для определения резервного набора данных, который будет использован в процессе восстановления. Компонент Database Engine нумерует каждый резервный набор данных на резервном носителе. Это позволяет выявить резервный набор данных, подлежащий восстановлению, по его положению на носителе. Например, на следующем носителе содержатся три резервных набора данных.  
  
 ![Носитель данных резервных копий, содержащий резервные наборы данных SQL Server](../../relational-databases/backup-restore/media/bnr-media-backup-sets.gif "Носитель данных резервных копий, содержащий резервные наборы данных SQL Server")  
  
 Чтобы восстановить определенный резервный набор данных, укажите номер позиции резервного набора данных, который нужно восстановить. Например, чтобы восстановить второй резервный набор данных, следует указать «2» в качестве номера резервного набора данных, подлежащего восстановлению.  
  
##  <a name="comparison-of-media-header-and-backup-header-information"></a><a name="CompareMediaHeaderBackupHeader"></a> Сравнение данных в заголовке носителя и в заголовке резервной копии  
 На следующем рисунке показано отличие между просмотром данных в заголовке носителя и просмотром данных в заголовке резервной копии. Получение заголовка носителя требует извлечения данных только с начала ленты. Получение заголовка резервной копии требует просмотра всей ленты в поисках заголовков всех резервных наборов данных.  
  
 ![Набор носителей с тремя наборами резервных копий баз данных SQL Server](../../relational-databases/backup-restore/media/bnr-media-label.gif "Набор носителей с тремя наборами резервных копий баз данных SQL Server")  
  
> [!NOTE]  
>  При использовании наборов носителей с несколькими семействами носителей заголовок носителя и резервный набор данных записываются на все семейства носителей. Поэтому для этих учетных операций необходимо указать лишь одно семейство носителей.  
  
 Дополнительные сведения о просмотре заголовка носителя см. выше в разделе «Просмотр данных заголовка носителя».  
  
 Дополнительные сведения о просмотре заголовка резервной копии для всех резервных наборов данных на устройстве резервного копирования см. далее в разделе «Просмотр данных заголовка резервной копии».  
  
##  <a name="backup-verification"></a><a name="Verification"></a> Проверки резервных копий  
 Проверка резервных копий хотя и не обязательна, но полезна. С помощью проверки резервной копии можно проконтролировать ее физическую доступность, убедиться в том, что все файлы резервной копии могут быть прочитаны и восстановлены, и в том, что резервную копию можно восстановить в любой момент, когда это понадобится. Важно понимать, что проверка резервной копии не является проверкой структуры данных в данной резервной копии. Однако если резервная копия была создана с использованием предложения WITH CHECKSUMS, проверка резервной копии с использованием предложения WITH CHECKSUMS может также дать полное представление о надежности данных в данной резервной копии.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
 **Удаление старых строк из таблиц журналов резервного копирования и восстановления**  
  
-   [sp_delete_backuphistory (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md)  
  
 **Удаление всех строк для заданной базы данных из таблиц журналов резервного копирования и восстановления**  
  
-   [sp_delete_database_backuphistory (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-database-backuphistory-transact-sql.md)  
  
 **Просмотр файлов данных и журналов в резервном наборе данных**  
  
-   [RESTORE FILELISTONLY (Transact-SQL)](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.ReadFileList%2A> (SMO)  
  
 **Просмотр данных в заголовке носителя**  
  
-   [RESTORE LABELONLY (Transact-SQL)](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)  
  
-   [Просмотр свойств и содержимого логического устройства резервного копирования (SQL Server)](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)  
  
-   [Просмотр содержимого ленты или файла резервной копии (SQL Server)](../../relational-databases/backup-restore/view-the-contents-of-a-backup-tape-or-file-sql-server.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.ReadMediaHeader%2A> (SMO)  
  
 **Просмотр данных в заголовке резервной копии**  
  
-   [RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
-   [Просмотр содержимого ленты или файла резервной копии (SQL Server)](../../relational-databases/backup-restore/view-the-contents-of-a-backup-tape-or-file-sql-server.md)  
  
-   [Просмотр свойств и содержимого логического устройства резервного копирования (SQL Server)](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.ReadBackupHeader%2A> (SMO)    
  
-   [Просмотр содержимого ленты или файла резервной копии (SQL Server)](../../relational-databases/backup-restore/view-the-contents-of-a-backup-tape-or-file-sql-server.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.ReadMediaHeader%2A> (SMO)  
  
 **Просмотр файлов в резервном наборе данных**  
  
-   [Просмотр файлов данных и журналов в резервном наборе данных (SQL Server)](../../relational-databases/backup-restore/view-the-data-and-log-files-in-a-backup-set-sql-server.md)  
  
-   [RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
 **Проверка резервной копии**  
  
-   [RESTORE VERIFYONLY (Transact-SQL)](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.SqlVerify%2A> (SMO)  
  
## <a name="see-also"></a>См. также:  
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [Наборы носителей, семейства носителей и резервные наборы данных (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [Устройства резервного копирования (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [Зеркальные наборы носителей резервных копий (SQL Server)](../../relational-databases/backup-restore/mirrored-backup-media-sets-sql-server.md)   
 [Возможные ошибки носителей во время резервного копирования и восстановления (SQL Server)](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md)  
  
  
