---
description: sys.dm_db_log_stats (Transact-SQL)
title: sys.dm_db_log_stats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 05/17/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_log_stats_TSQL
- sys.dm_db_log_stats
- sys.dm_db_log_stats_TSQL
- dm_db_log_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_log_stats dynamic management function
ms.assetid: ''
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ddfd608b3f2d5bb5c4c259dfb0ec822f245f72b3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096568"
---
# <a name="sysdm_db_log_stats-transact-sql"></a>sys.dm_db_log_stats (Transact-SQL)   
[!INCLUDE[tsql-appliesto-2016sp2-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md)]

Возвращает атрибуты уровня сводки и сведения о файлах журнала транзакций баз данных. Используйте эти сведения для мониторинга и диагностики работоспособности журнала транзакций.   
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
 sys.dm_db_log_stats ( database_id )
```  
  
## <a name="arguments"></a>Аргументы  

*database_id* | NULL | **По умолчанию**

Идентификатор базы данных. `database_id` имеет значение `int`. Допустимые входные данные — это ИДЕНТИФИКАЦИОНный номер базы данных, `NULL` или `DEFAULT` . Значение по умолчанию — `NULL`. `NULL` и `DEFAULT` являются эквивалентными значениями в контексте текущей базы данных.  
Может быть указана встроенная функция [DB_ID](../../t-sql/functions/db-id-transact-sql.md). При использовании `DB_ID` без указания имени базы данных уровень совместимости текущей базы данных должен быть 90 или выше.

  
## <a name="tables-returned"></a>Возвращаемые таблицы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|database_id    |**int**    |Идентификатор базы данных |  
|recovery_model |**nvarchar(60)**   |   Модель восстановления базы данных. Возможные значения: <br /> ПРОСТОЙ<br /> BULK_LOGGED <br /> FULL |  
|log_min_lsn    |**nvarchar(24)**   |   Текущий начальный [регистрационный номер транзакции в журнале](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) в журнале транзакций.|  
|log_end_lsn    |**nvarchar(24)**   |   Регистрационный [номер транзакции в журнале (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) последней записи журнала в журнале транзакций.|  
|current_vlf_sequence_number    |**bigint** |   Текущий порядковый номер [файла виртуального журнала (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) во время выполнения.|  
|current_vlf_size_mb    |**float**  |   Текущий размер [виртуального файла журнала (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) в МБ.|   
|total_vlf_count    |**bigint** |   Общее число [виртуальных файлов журнала (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) в журнале транзакций. |  
|total_log_size_mb  |**float**  |   Общий размер журнала транзакций в МБ. |  
|active_vlf_count   |**bigint** |   Общее число активных [виртуальных файлов журнала (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) в журнале транзакций.|  
|active_log_size_mb |**float**  |   Общий размер журнала активных транзакций в МБ.|  
|log_truncation_holdup_reason   |**nvarchar(60)**   |   Причина усечения журнала задержки. Значение совпадает со  `log_reuse_wait_desc` столбцом `sys.databases` .  (Более подробное объяснение этих значений см. [в журнале транзакций](../../relational-databases/logs/the-transaction-log-sql-server.md)). <br />Возможные значения: <br />NOTHING;<br />CHECKPOINT<br />LOG_BACKUP<br />ACTIVE_BACKUP_OR_RESTORE<br />ACTIVE_TRANSACTION<br />DATABASE_MIRRORING<br />РЕПЛИКАЦИЯ<br />DATABASE_SNAPSHOT_CREATION<br />LOG_SCAN<br />AVAILABILITY_REPLICA<br />OLDEST_PAGE<br />XTP_CHECKPOINT<br />ДРУГИЕ ВРЕМЕННЫЕ |  
|log_backup_time    |**datetime**   |   Время последнего резервного копирования журнала транзакций.|   
|log_backup_lsn |**nvarchar(24)**   |   [Номер LSN](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch)последней резервной копии журнала транзакций.|   
|log_since_last_log_backup_mb   |**float**  |   Размер журнала в МБ с момента последнего резервного копирования журнала транзакций [(номер LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch).|  
|log_checkpoint_lsn |**nvarchar(24)**   |   [Порядковый номер транзакции в журнале](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch)последней контрольной точки (LSN).|  
|log_since_last_checkpoint_mb   |**float**  |   Размер журнала в МБ с момента последней контрольной точки в [журнале (номер LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch).|  
|log_recovery_lsn   |**nvarchar(24)**   |   Регистрационный [номер транзакции в журнале (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) базы данных. Если `log_recovery_lsn` происходит перед номером LSN контрольной точки, то `log_recovery_lsn` это самый старый номер LSN активной транзакции, в противном случае `log_recovery_lsn` — номер LSN контрольной точки.|  
|log_recovery_size_mb   |**float**  |   Размер журнала в МБ, начиная с момента [регистрационного номера транзакции в журнале восстановления журнала (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch).|  
|recovery_vlf_count |**bigint** |   Общее число [файлов виртуального журнала (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) для восстановления при отработке отказа или перезапуске сервера. |  


## <a name="remarks"></a>Комментарии
При запуске `sys.dm_db_log_stats` базы данных, участвующей в группе доступности в качестве вторичной реплики, будет возвращено только подмножество полей, описанных выше.  В настоящее время `database_id` `recovery_model` `log_backup_time` при выполнении в базе данных-получателе будут возвращаться только, и.   

## <a name="permissions"></a>Разрешения  
Требуется `VIEW DATABASE STATE` разрешение в базе данных.   
  
## <a name="examples"></a>Примеры  

### <a name="a-determining-databases-in-a-ssnoversion-instance-with-high-number-of-vlfs"></a>A. Определение баз данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] экземпляре с большим количеством VLF   
Следующий запрос возвращает базы данных с более чем 100 VLF в файлах журнала. Большое число VLF может повлиять на время запуска, восстановления и восстановления базы данных.

```sql  
SELECT name AS 'Database Name', total_vlf_count AS 'VLF count' 
FROM sys.databases AS s
CROSS APPLY sys.dm_db_log_stats(s.database_id) 
WHERE total_vlf_count  > 100;
```   

### <a name="b-determining-databases-in-a-ssnoversion-instance-with-transaction-log-backups-older-than-4-hours"></a>Б. Определение баз данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] экземпляре с резервными копиями журнала транзакций старше 4 часов   
Следующий запрос определяет время последнего резервного копирования журнала для баз данных в экземпляре.

```sql  
SELECT name AS 'Database Name', log_backup_time AS 'last log backup time' 
FROM sys.databases AS s
CROSS APPLY sys.dm_db_log_stats(s.database_id); 
```

## <a name="see-also"></a>См. также:  
[Динамические административные представления и функции (Transact-SQL)](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
[Динамические административные представления, связанные с базами данных &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
[sys.dm_db_log_space_usage (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md)   
[sys.dm_db_log_info (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-info-transact-sql.md)    
  
