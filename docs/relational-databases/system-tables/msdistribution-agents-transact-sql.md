---
description: MSdistribution_agents (Transact-SQL)
title: MSdistribution_agents (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 10/28/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSdistribution_agents_TSQL
- MSdistribution_agents
dev_langs:
- TSQL
helpviewer_keywords:
- MSdistribution_agents system table
ms.assetid: 0e8f0653-1351-41d1-95d2-40f6d5a050ca
author: cawrites
ms.author: chadam
ms.openlocfilehash: 20b2d5f012c33ea12d4b60ed3a8d44ee475f3bf2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091496"
---
# <a name="msdistribution_agents-transact-sql"></a>MSdistribution_agents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Таблица **MSdistribution_agents** содержит по одной строке для каждого агент распространения, выполняемого на локальном распространителе. Эта таблица хранится в базе данных распространителя.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**идентификатор**|**int**|Идентификатор агента распространителя.|  
|**name**|**nvarchar (100)**|Имя агента распространителя.|  
|**publisher_database_id**|**int**|Идентификатор базы данных издателя.|  
|**publisher_id**|**smallint**|Идентификатор издателя.|  
|**publisher_db**|**sysname**|Имя базы данных издателя.|  
|**публикации**|**sysname**|Имя публикации.|  
|**subscriber_id**|**smallint**|Идентификатор подписчика, указан только для общеизвестных агентов. Для анонимных агентов этот столбец зарезервирован.|  
|**subscriber_db**|**sysname**|Имя базы данных подписки.|  
|**subscription_type**|**int**|Тип подписки.<br /><br /> **0** = принудительная отправка.<br /><br /> **1** = по запросу.<br /><br /> **2** = анонимный.|  
|**local_job**|**bit**|Указывает, имеется ли задание агента SQL Server на локальном распространителе.|  
|**job_id**|**двоичный (16)**|Идентификационный номер задания.|  
|**subscription_guid**|**двоичный (16)**|Идентификатор подписок данного агента.|  
|**profile_id**|**int**|Идентификатор конфигурации из [MSagent_profiles &#40;таблице&#41;Transact-SQL ](../../relational-databases/system-tables/msagent-profiles-transact-sql.md) .|  
|**anonymous_subid**|**uniqueidentifier**|Идентификатор анонимного агента.|  
|**subscriber_name**|**sysname**|Имя подписчика, указывается только для анонимных агентов.|  
|**virtual_agent_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**anonymous_agent_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**creation_date**|**datetime**|Дата и время, когда был создан агент распространителя или агент слияния.|  
|**queue_id**|**sysname**|Идентификатор для поиска очереди для очереди обновляемых подписок. Для подписок, не использующих очереди, значение равно NULL. Для публикаций на основе очереди сообщений [!INCLUDE[msCoName](../../includes/msconame-md.md)] значением является идентификатор GUID, который однозначно идентифицирует очередь, используемую подпиской. Для публикаций очередей на основе SQL Server столбец содержит значение **SQL**.<br /><br /> Примечание. Использование [!INCLUDE[msCoName](../../includes/msconame-md.md)] очереди сообщений устарело и больше не поддерживается.|  
|**queue_status**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**offload_enabled**|**bit**|Указывает, может ли агент быть активизирован удаленно.<br /><br /> значение **0** указывает, что агент не может быть активирован удаленно.<br /><br /> значение **1** указывает, что агент будет активирован удаленно и на удаленном компьютере, указанном в свойстве *offload_server* .|  
|**offload_server**|**sysname**|Сетевое имя сервера для удаленной активации.|  
|**dts_package_name**;|**sysname**|Имя пакета служб DTS. Например, для пакета с именем **DTSPub_Package** укажите `@dts_package_name = N'DTSPub_Package'` .|  
|**dts_package_password**;|**nvarchar (524)**|Пароль пакета.|  
|**dts_package_location**.|**int**|Местонахождение пакета. Расположение пакета может быть **распространителем** или **подписчиком**.|  
|**трансляцию**|**varbinary(85)**|Идентификатор безопасности (SID) агента распространителя или агента слияния при первом выполнении.|  
|**queue_server**|**sysname**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**subscriber_security_mode**|**smallint**|Режим безопасности, используемый агентом при подключении к подписчику. Предусмотрены следующие режимы:<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server проверка подлинности<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] Проверка подлинности Windows.|  
|**subscriber_login**|**sysname**|Имя входа для подключения к подписчику.|  
|**subscriber_password**|**nvarchar (524)**|Зашифрованное значение пароля, используемое для подключения к подписчику.|  
|**reset_partial_snapshot_progress**|**bit**|Установлен, если частично загруженный моментальный снимок будет отменен, затем загружен с самого начала.|  
|**job_step_uid**|**uniqueidentifier**|Уникальный идентификатор шага задания агента SQL Server, на котором запущен агент.|  
|**SubscriptionStreams**|**tinyint**|Устанавливает максимальное число соединений, разрешенных каждому из агентов распространителя для параллельного применения пакетов изменений на подписчике. Поддерживаются значения в диапазоне от 1 до 64.|  
|**memory_optimized**|**bit**|значение 1 указывает, что подписчик может использоваться для оптимизированных для памяти таблиц.|  
|**job_login**|**sysname**||  
|**job_password**|**nvarchar (524)**||  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации (Transact-SQL)](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
