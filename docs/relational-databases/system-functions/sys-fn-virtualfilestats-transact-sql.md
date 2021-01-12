---
description: sys.fn_virtualfilestats (Transact-SQL)
title: sys.fn_virtualfilestats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/16/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_virtualfilestats_TSQL
- fn_virtualfilestats
dev_langs:
- TSQL
helpviewer_keywords:
- I/O [SQL Server], statistics
- fn_virtualfilestats function
- sys.fn_virtualfilestats function
- statistical information [SQL Server], I/O
ms.assetid: 96b28abb-b059-48db-be2b-d60fe127f6aa
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 21ebb356139cf33f87a4e3da84a5004bd7405ae9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096395"
---
# <a name="sysfn_virtualfilestats-transact-sql"></a>sys.fn_virtualfilestats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает статистику ввода-вывода для файлов базы данных, включая файлы журналов. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] эти сведения также доступны в динамическом административном представлении [sys.dm_io_virtual_file_stats](../../relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql.md) .  

 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
fn_virtualfilestats ( { database_id | NULL } , { file_id | NULL } )  
```  
  
## <a name="arguments"></a>Аргументы  
 *database_id* | ЗАКАНЧИВАЮЩ  
 Идентификатор базы данных. Аргумент *database_id* имеет тип **int** и не имеет значения по умолчанию. Укажите значение NULL, чтобы вернуть сведения для всех баз данных в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *file_id* | ЗАКАНЧИВАЮЩ  
 Идентификатор файла. *file_id* имеет **тип int** и не имеет значения по умолчанию. Чтобы получить сведения обо всех файлах в базе данных, укажите значение NULL.  
  
## <a name="table-returned"></a>Возвращаемая таблица  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**DbId**|**smallint**|Идентификатор базы данных.|  
|**FileId**|**smallint**|Идентификатор файла.|  
|**TimeStamp**|**bigint**|Отметка времени базы данных, указывающая, когда была получена информация. **int** в версиях до [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] . |  
|**NumberReads**|**bigint**|Количество считываний для этого файла.|  
|**битесреад**|**bigint**|Число считанных из файла байтов.|  
|**иосталлреадмс**|**bigint**|Общее количество времени, в миллисекундах, затраченного пользователями на ожидание выполнения операций чтения из файла.|  
|**NumberWrites**|**bigint**|Количество операций записи для этого файла.|  
|**BytesWritten**|**bigint**|Число байтов, записанных в файл.|  
|**IoStallWriteMS**|**bigint**|Общее количество времени, в миллисекундах, затраченного пользователями на ожидание выполнения операций записи в файл.|  
|**IoStallMS**|**bigint**|Сумма **иосталлреадмс** и **иосталлвритемс**.|  
|**FileHandle**|**bigint**|Значение дескриптора файла.|  
|**BytesOnDisk**|**bigint**|Физический размер файла на диске (в байтах).<br /><br /> Для файлов базы данных это значение совпадает с **размером** в **sys.database_files**, но выражается в байтах, а не на страницах.<br /><br /> Для разреженных файлов моментального снимка базы данных это пространство, используемое операционной системой для данного файла.|  
  
## <a name="remarks"></a>Комментарии  
 **fn_virtualfilestats** — это системная функция, возвращающая табличное значение, которая предоставляет статистические сведения, например общее количество операций ввода-вывода, выполненных над файлом. Эту функцию можно использовать для контроля времени, затрачиваемого пользователями на ожидание выполнения чтения или записи в файл. Эта функция также помогает выявить файлы, над которыми выполняется много операций ввода-вывода.  
  
## <a name="permissions"></a>Разрешения  
 необходимо разрешение VIEW SERVER STATE на сервере.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-displaying-statistical-information-for-a-database"></a>A. Просмотр статистической информации для базы данных  
 В следующем примере выводится статистическая информация для идентификатора файла 1 в базе данных с идентификатором `1`.  
  
```sql  
SELECT *  
FROM fn_virtualfilestats(1, 1);  
GO  
```  
  
### <a name="b-displaying-statistical-information-for-a-named-database-and-file"></a>Б. Просмотр статистической информации для именованной базы данных и файла  
 В следующем примере выводятся статистические данные для файла журнала в образце базы данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Системная функция `DB_ID` используется для указания параметра *database_id* .  
  
```sql  
SELECT *  
FROM fn_virtualfilestats(DB_ID(N'AdventureWorks2012'), 2);  
GO  
```  
  
### <a name="c-displaying-statistical-information-for-all-databases-and-files"></a>В. Просмотр статистической информации для всех баз данных и файлов  
 В следующем примере выводится статистическая информация для всех файлов во всех базах данных экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
SELECT *  
FROM fn_virtualfilestats(NULL,NULL);  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [DB_ID &#40;Transact-SQL&#41;](../../t-sql/functions/db-id-transact-sql.md)   
 [FILE_IDEX (Transact-SQL)](../../t-sql/functions/file-idex-transact-sql.md)   
 [sys.database_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  
