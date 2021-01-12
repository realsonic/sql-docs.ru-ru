---
description: restorehistory (Transact-SQL)
title: restorehistory (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- restorehistory
- restorehistory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- restorehistory system table
ms.assetid: 9140ecc1-d912-4d76-ae70-e2a857da6d44
author: cawrites
ms.author: chadam
ms.openlocfilehash: c4d164bdceaea91650a0d1bd8226133b22692912
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096167"
---
# <a name="restorehistory-transact-sql"></a>restorehistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит по одной строке для каждой операции восстановления. Эта таблица хранится в базе данных **msdb** .  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**restore_history_id**|**int**|Уникальный идентификационный номер, который определяет каждую операцию восстановления. Удостоверение, первичный ключ.|  
|**restore_date**|**datetime**|Дата и время начала операции восстановления. Может иметь значение NULL.|  
|**destination_database_name**|**nvarchar(128)**|Имя целевой базы данных для операции восстановления. Может иметь значение NULL.|  
|**user_name**|**nvarchar(128)**|Имя пользователя, выполнявшего операцию восстановления. Может иметь значение NULL.|  
|**backup_set_id**|**int**|Уникальный идентификационный номер, которым определяется восстанавливаемый резервный набор данных. Ссылается на **резервный архив (backup_set_id)**.|  
|**restore_type**|**char(1)**|Тип операции восстановления:<br /><br /> D = база данных<br /><br /> F = Файл<br /><br /> G = Файловая группа<br /><br /> I = Дифференциал<br /><br /> L = журнал<br /><br /> V = Только проверка<br /><br /> Может иметь значение NULL.|  
|**replace**|**bit**|Показывает, определен ли операцией восстановления параметр REPLACE:<br /><br /> 1 = Определен<br /><br /> 0 = Не определен<br /><br /> Может иметь значение NULL.<br /><br /> Если база данных возвращается к состоянию по моментальному снимку базы данных, значение 0 является единственным параметром.|  
|**recovery**|**bit**|Показывает, определен ли операцией восстановления параметр RECOVERY или NORECOVERY:<br /><br /> 1 = RECOVERY<br /><br /> Может иметь значение NULL.<br /><br /> Когда база данных возвращается к моментальному снимку базы данных, единственным вариантом является 1.<br /><br /> 0 = NORECOVERY|  
|**restart**|**bit**|Показывает, определен ли операцией восстановления параметр RESTART:<br /><br /> 1 = Определен<br /><br /> 0 = Не определен<br /><br /> Может иметь значение NULL.<br /><br /> Если база данных возвращается к состоянию по моментальному снимку базы данных, значение 0 является единственным параметром.|  
|**stop_at**|**datetime**|Момент времени, когда база данных была восстановлена. Может иметь значение NULL.|  
|**device_count**|**tinyint**|Количество устройств, вовлеченных в операцию восстановления. Это число может быть меньше количества носителей резервной копии. Может иметь значение NULL.<br /><br /> Если база данных возвращается к состоянию по моментальному снимку базы данных, это число всегда равно 1.|  
|**stop_at_mark_name**|**nvarchar(128)**|Показывает, что восстановление совершается к состоянию на момент выполнения транзакции, содержащей именованную метку. Может иметь значение NULL.<br /><br /> Если база данных возвращается к состоянию по моментальному снимку базы данных, используется значение NULL.|  
|**stop_before**|**bit**|Показывает, была ли включена транзакция, содержащая именованную метку, в процесс восстановления:<br /><br /> 0 = Процесс восстановления был остановлен перед помеченной транзакцией.<br /><br /> 1 = Помеченная транзакция была включена в процесс восстановления.<br /><br /> Может иметь значение NULL.<br /><br /> Если база данных возвращается к состоянию по моментальному снимку базы данных, используется значение NULL.|  
  
## <a name="remarks"></a>Комментарии  
 Чтобы уменьшить количество строк в этой таблице и в других таблицах резервного копирования и журнала, выполните хранимую процедуру [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) .  
  
## <a name="see-also"></a>См. также:  
 [Резервное копирование и восстановление таблиц &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [restorefile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/restorefile-transact-sql.md)   
 [restorefilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-tables/restorefilegroup-transact-sql.md)   
 [Системные таблицы (Transact-SQL)](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
