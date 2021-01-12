---
description: sysmail_mailattachments (Transact-SQL)
title: sysmail_mailattachments (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysmail_mailattachments_TSQL
- sysmail_mailattachments
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_mailattachments database mail view
ms.assetid: aee87059-a4c1-459a-a95c-641b4e3f0e73
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9fe55316cc27c21849379afe400d2950a5bcf882
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100145"
---
# <a name="sysmail_mailattachments-transact-sql"></a>sysmail_mailattachments (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит по одной строке для каждого вложения, полученного компонентом Database Mail. Это представление следует использовать в том случае, когда необходима информация о вложениях, принятых компонентом Database Mail. Чтобы проверить все сообщения электронной почты, обработанные Database Mail используйте [sysmail_allitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md).  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**attachment_id**|**int**|Идентификатор вложения.|  
|**mailitem_id**|**int**|Идентификатор письма, содержавшего вложение.|  
|**filename**|**nvarchar (520)**|Имя файла вложения. Если **attach_query_result** равен 1, а **query_attachment_filename** имеет значение null, Database Mail создает произвольное имя файла.|  
|**Размер файла**|**int**|Размер вложения в байтах.|  
|**содержатся**|**varbinary(max)**|Содержимое вложения.|  
|**last_mod_date**|**datetime**|Дата и время последнего изменения строки.|  
|**last_mod_user**|**sysname**|Пользователь, внесший последнее изменение в строку.|  
  
## <a name="remarks"></a>Комментарии  
 Это представление следует использовать для просмотра свойств вложений при устранении неполадок в работе компонента Database Mail.  
  
 Вложения, хранящиеся в системных таблицах, могут привести к росту базы данных **msdb** . Для удаления элементов электронной почты и связанных с ними вложений используйте **sysmail_delete_mailitems_sp** . Дополнительные сведения см. в разделе [создание агент SQL Server задания для архивации Database Mail сообщений и журналов событий](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md).  
  
## <a name="permissions"></a>Разрешения  
 Предоставлены предопределенной роли сервера **sysadmin** и роли базы данных **DatabaseMailUserRole** . При выполнении членом предопределенной роли сервера **sysadmin** в этом представлении отображаются все вложения. Все остальные пользователи могут видеть только вложения, отправленные ими самими.  
  
## <a name="see-also"></a>См. также:  
 [sysmail_allitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md)   
 [sysmail_faileditems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-faileditems-transact-sql.md)   
 [sysmail_sentitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-sentitems-transact-sql.md)   
 [sysmail_unsentitems &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-unsentitems-transact-sql.md)   
 [sysmail_event_log (Transact-SQL)](../../relational-databases/system-catalog-views/sysmail-event-log-transact-sql.md)  
  
  
