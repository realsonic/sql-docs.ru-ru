---
description: Представления каталога сообщений (для ошибок) — sys.messages
title: sys. messages (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- messages_TSQL
- sys.messages_TSQL
- sys.messages
- messages
dev_langs:
- TSQL
helpviewer_keywords:
- error messages [SQL Server]
- sys.messages catalog view
- error numbers [SQL Server]
ms.assetid: 8c16ecdf-68f4-4a2a-b594-086e3344e58a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 590ad007eebcf37765752272ef775e0d3d2443f0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092568"
---
# <a name="messages-for-errors-catalog-views---sysmessages"></a>Представления каталога сообщений (для ошибок) — sys.messages
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит по одной строке для каждого **message_id** или **language_id** сообщений об ошибках в системе, как для определяемых системой, так и для определяемых пользователем сообщений. Дополнительные сведения см. в разделе [sp_addmessage (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md).  
   
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**message_id**|**int**|Идентификатор сообщения. Уникален в пределах сервера. Сообщения с идентификаторами, меньшими 50000, являются системными.|  
|**language_id**|**smallint**|Идентификатор языка, для которого используется текст в **тексте** , как определено в таблице **syslanguages**. Это значение уникально для указанного **message_id**.|  
|**severity**|**tinyint**|Степень серьезности сообщения, от 1 до 25. Это одинаково для всех языков сообщений в **message_id**.|  
|**is_event_logged**|**bit**|1 = сообщение заносится в журнал событий при возникновении ошибки. Это одинаково для всех языков сообщений в **message_id**.|  
|**text**|**nvarchar (2048)**|Текст сообщения, используемого при активном соответствующем **language_id** .|  
  
## <a name="permissions"></a>Разрешения  
 Необходимо быть членом роли **public**. Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [THROW (Transact-SQL)](../../t-sql/language-elements/throw-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Сообщения &#40;об ошибках&#41; представлениях каталога &#40;Transact-SQL&#41;]()   
 [Программирование окна сообщений об исключениях](/previous-versions/sql/sql-server-2016/ms166343(v=sql.130))   
 [Сообщения об ошибках](../../relational-databases/native-client-odbc-error-messages/error-messages.md)   
 [События и ошибки компонента Database Engine](../../relational-databases/errors-events/database-engine-events-and-errors.md)  
  
