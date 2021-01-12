---
description: Представления каталога следящего сервера зеркального отображения базы данных — sys.database_mirroring_witnesses
title: sys.database_mirroring_witnesses (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.database_mirroring_witnesses
- sys.database_mirroring_witnesses_TSQL
- database_mirroring_witnesses
- database_mirroring_witnesses_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database mirroring [SQL Server], catalog views
- sys.database_mirroring_witnesses catalog view
- witness [SQL Server], sys.database_mirroring_witnesses catalog view
ms.assetid: 0dd5b794-733b-4a3c-b5a4-62f9f1f0f22d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 09eae37eb4eb380ad56c36d837a8454a50a58f8d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092659"
---
# <a name="database-mirroring-witness-catalog-views---sysdatabase_mirroring_witnesses"></a>Представления каталога следящего сервера зеркального отображения базы данных — sys.database_mirroring_witnesses
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит по одной строке для каждой из следящих ролей, исполняемых сервером при участии в зеркальном отображении базы данных. 
  
  При зеркальном отображении базы данных для автоматической отработки отказа требуется следящий сервер. Желательно, чтобы следящий сервер находился на компьютере, отдельном от основного и зеркального серверов. Следящий сервер не обслуживает базу данных, а производит мониторинг состояния основного и зеркального серверов. В случае сбоя основного сервера следящий сервер может инициировать автоматический переход на зеркальный диск. 
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**database_name**|**sysname**|Имя двух копий базы данных в сеансе зеркального отображения базы данных.|  
|**principal_server_name**|**sysname**|Имя сервера-участника, копия базы данных которого в настоящее время является основной базой данных.|  
|**mirror_server_name**|**sysname**|Имя сервера-участника, копия базы данных которого в настоящее время является зеркальной базой данных.|  
|**safety_level**|**tinyint**|Уровень безопасности транзакции для выполнения изменений в зеркальной базе данных.<br /><br /> 0 = неизвестное состояние.<br /><br /> 1 = выключен (асинхронно).<br /><br /> 2 = полный (синхронно).<br /><br /> Использование слежения для автоматической отработки отказа требует полного уровня безопасности, который включен по умолчанию.|  
|**safety_level_desc**|**nvarchar(60)**|Описание гарантий безопасности изменений в зеркальной базе данных.<br /><br /> UNKNOWN<br /><br /> OFF<br /><br /> FULL|  
|**safety_sequence_number**|**int**|Порядковый номер обновления для изменений в **safety_level**.|  
|**role_sequence_number**|**int**|Последовательный номер обновления для переключения ролей главного и зеркального сервера, исполняемых каждым из участников зеркального отображения.|  
|**mirroring_guid**|**uniqueidentifier**|Идентификатор участия зеркального отображения.|  
|**family_guid**|**uniqueidentifier**|Идентификатор семейства для резервирования базы данных. Используется для выявления совпадающих состояний восстановления.|  
|**is_suspended**|**bit**|Зеркальное отображение базы данных приостановлено.|  
|**is_suspended_sequence_number**|**int**|Порядковый номер для параметра **is_suspended**.|  
|**partner_sync_state**|**tinyint**|Состояние синхронизации сеанса зеркального отображения базы данных:<br /><br /> 5 = партнеры синхронизированы. Отработка отказа возможна. Сведения о требованиях к отработке отказа см. в разделе [Переключение ролей во время сеанса зеркального отображения базы данных &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md).<br /><br /> 6 = партнеры не синхронизированы. Отработка отказа сейчас невозможна;|  
|**partner_sync_state_desc**|**nvarchar(60)**|Описание состояния синхронизации сеанса зеркального отображения:<br /><br /> SYNCHRONIZED<br /><br /> UNSYNCHRONIZED|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Database Mirroring Witness](../../database-engine/database-mirroring/database-mirroring-witness.md)   
 [sys.database_mirroring &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md)   
 [sys.database_mirroring_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)   
 [Часто задаваемые вопросы о запросах к системному каталогу сервера SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
