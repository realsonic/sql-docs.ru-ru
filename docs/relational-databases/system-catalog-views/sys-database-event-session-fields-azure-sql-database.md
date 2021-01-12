---
description: sys.database_event_session_fields (база данных SQL Azure)
title: sys.database_event_session_fields (база данных SQL Azure) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
ms.assetid: 9b5c94d6-612c-4e0f-976d-ac6ba55da3ac
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 864bccc837046ebeed303b66d9f3719b8ff67a9d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102785"
---
# <a name="sysdatabase_event_session_fields-azure-sql-database"></a>sys.database_event_session_fields (база данных SQL Azure)
[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Возвращает строку для каждого настраиваемого столбца, явно установленного на события и цели.  
  
||  
|-|  
|**Применимо к** версии [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 12 и всем более поздним версиям.|  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|Идентификатор сеанса событий. Не допускает значение NULL.|  
|object_id|**int**|Идентификатор объекта, с которым связано это поле. Не допускает значение NULL.|  
|name|**sysname**|Имя поля. Не допускает значение NULL.|  
|value|**sql_variant**|Значение поля. Не допускает значение NULL.|  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение VIEW DATABASE STATE на сервере.  
  
## <a name="remarks"></a>Комментарии  
 Это представление имеет следующее количество элементов связей.  
  
| От | Кому | Relationship |
| ---- | -- | ------------ |
|sys.database_event_session_actions sys.database_event_session_actions.event_session_id|sys.database_event_sessions sys.database_event_sessions.event_session_id|Многие к одному|  
|sys.database_event_session_actions sys.database_event_session_actions.event_id<br /><br /> sys.database_event_session_actions sys.database_event_session_actions.object_id<br /><br /> sys.database_event_session_actions sys.database_event_session_actions.event_session_id|sys.database_event_session_events sys.database_event_session_events.event_session_id<br /><br /> sys.database_event_session_events sys.database_event_session_events.event_id|Многие к одному|  
|sys.database_event_session_actions sys.database_event_session_actions.event_session_id<br /><br /> sys.database_event_session_actions sys.database_event_session_actions.object_id|sys.database_event_session_targets sys.database_event_session_targets.event_session_id<br /><br /> sys.database_event_session_targets sys.database_event_session_targets.target_id|Многие к одному|  
  
## <a name="see-also"></a>См. также:  
 [Расширенные события](../../relational-databases/extended-events/extended-events.md)  
  
  
