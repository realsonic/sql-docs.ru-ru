---
description: suspect_pages (Transact-SQL)
title: suspect_pages (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- suspect_page_table
- suspect_page_table_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- suspect_pages system table
- suspect pages [SQL Server]
ms.assetid: 119c8d62-eea8-44fb-bf72-de469c838c50
author: cawrites
ms.author: chadam
ms.openlocfilehash: c2b5952f12c857a7bf58c066744e1c366279b1e4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096146"
---
# <a name="suspect_pages-transact-sql"></a>suspect_pages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит по одной строке для каждой страницы, возвратившей ошибку с кодом 823 или ошибку с кодом 824. Список содержит все строки, подозреваемые на наличие ошибок. Некоторые из них могут быть исправными. При восстановлении подозрительной страницы ее состояние обновляется в столбце **event_type** .  
  
 В следующей таблице, которая имеет ограничение в 1 000 строк, хранится в базе данных **msdb** .  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|Идентификатор базы данных, которой принадлежит страница.|  
|**file_id**|**int**|Идентификатор файла в базе данных.|  
|**page_id**|**bigint**|Идентификатор подозрительной страницы. Каждая страница имеет идентификатор страницы, который является 32-разрядным значением, определяющим расположение страницы в базе данных. **Page_id** — это смещение в файле данных страницы размером 8 КБ. Каждый идентификатор страницы уникален в пределах файла.|  
|**event_type**|**int**|Тип ошибки; один из следующих.<br /><br /> 1 = ошибка 823, не относящаяся к странице (например, ошибка чтения диска), либо ошибка 824, относящаяся к неверной контрольной сумме или обрыву страницы (например, идентификатор страницы).<br /><br /> 2 = неверная контрольная сумма.<br /><br /> 3 = обрыв страницы.<br /><br /> 4 = восстановленная (страница была восстановлена после того, как была помечена как неверная).<br /><br /> 5 = исправленная (страница исправлена инструкцией DBCC).<br /><br /> 7 = освобождена инструкцией DBCC.|  
|**error_count**|**int**|Количество ошибок.|  
|**last_update_date**|**datetime**|Метка даты и времени последнего обновления.|  
  
## <a name="permissions"></a>Разрешения  
 Сведения в таблице **suspect_pages** доступны любому пользователю, имеющему доступ к базе данных **msdb** . Информация в таблице suspect_pages может обновляться любым пользователем, обладающим разрешением UPDATE. Члены предопределенной роли базы данных **db_owner** в **msdb** или предопределенной роли сервера **sysadmin** могут вставлять, обновлять и удалять записи.  
  
## <a name="see-also"></a>См. также:  
 [Восстановление страниц (SQL Server)](../../relational-databases/backup-restore/restore-pages-sql-server.md)   
 [Класс событий Database подозрения Data Page](../../relational-databases/event-classes/database-suspect-data-page-event-class.md)   
 [Системные таблицы &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/system-tables-transact-sql.md)   
 [Управление таблицей suspect_pages (SQL Server)](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
  
