---
description: sys.fn_db_backup_file_snapshots (Transact-SQL)
title: sys.fn_db_backup_file_snapshots (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/03/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 45010ff2-219f-4086-9ea4-016a6c17cddd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4976e0ae436fe1c9c59742b6580ae083b3e3c5c4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094948"
---
# <a name="sysfn_db_backup_file_snapshots-transact-sql"></a>sys.fn_db_backup_file_snapshots (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Возвращает моментальные снимки Azure, связанные с файлами базы данных. Если указанная база данных не найдена или файлы базы данных не хранятся в службе хранилища BLOB-объектов Microsoft Azure, строки не возвращаются. Используйте эту системную функцию совместно с системной хранимой процедурой **sys.sp_delete_backup_file_snapshot** для обнаружения и удаления потерянных моментальных снимков резервных копий. Дополнительные сведения см. в разделе [Резервные копии моментальных снимков файлов для файлов базы данных в Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sys.fn_db_backup_file_snapshots   
   [ ( database_name ) ]  
```  
  
## <a name="arguments"></a>Аргументы  
 *Database_name*  
 Имя запрашиваемой базы данных. Если значение равно NULL, эта функция выполняется в текущей области базы данных.  
  
## <a name="table-returned"></a>Возвращаемая таблица  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|file_id|**int**|Идентификатор файла для базы данных. Не допускает значение NULL.|  
|snapshot_time|**nvarchar(260)**|Метка времени моментального снимка, возвращаемая REST API. Возвращает значение NULL, если моментальный снимок не существует.|  
|snapshot_url|**nvarchar(360)**|Полный URL-адрес моментального снимка файла. Возвращает значение NULL, если моментальный снимок не существует.|  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение VIEW DATABASE STATE на базу данных.  
  
## <a name="see-also"></a>См. также:  
 [sp_delete_backup_file_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md)   
 [sp_delete_backup (Transact-SQL)](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md)  
  
  
