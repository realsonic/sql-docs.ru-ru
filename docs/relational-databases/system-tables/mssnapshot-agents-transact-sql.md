---
description: MSsnapshot_agents (Transact-SQL)
title: MSsnapshot_agents (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSsnapshot_agents
- MSsnapshot_agents_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsnapshot_agents system table
ms.assetid: aeae0a2e-4c21-4c45-be65-1e426fa52bdd
author: cawrites
ms.author: chadam
ms.openlocfilehash: bcd51354cab293afcdce2845fef5bd6e51e1c70f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096191"
---
# <a name="mssnapshot_agents-transact-sql"></a>MSsnapshot_agents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Таблица **MSsnapshot_agents** содержит по одной строке для каждого агент моментальных снимков, связанного с локальным распространителем. Эта таблица хранится в базе данных распространителя.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**идентификатор**|**int**|Идентификатор агента моментальных снимков.|  
|**name**|**nvarchar (100)**|Имя агента моментальных снимков.|  
|**publisher_id**|**smallint**|Идентификатор издателя.|  
|**publisher_db**|**sysname**|Имя базы данных издателя.|  
|**публикации**|**sysname**|Имя публикации.|  
|**publication_type**|**int**|Тип публикации:<br /><br /> **0** = транзакционная.<br /><br /> **1** = моментальный снимок.<br /><br /> **2** = слияние.|  
|**local_job**|**bit**|Показывает, есть ли задание агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на локальном распространителе.|  
|**job_id**|**двоичный (16)**|Идентификационный номер задания.|  
|**profile_id**|**int**|Идентификатор конфигурации из [MSagent_profiles &#40;таблице&#41;Transact-SQL ](../../relational-databases/system-tables/msagent-profiles-transact-sql.md) .|  
|**dynamic_filter_login**|**sysname**|Значение, используемое для вычисления [SUSER_SNAME &#40;функции&#41;Transact-SQL ](../../t-sql/functions/suser-sname-transact-sql.md) в параметризованных фильтрах, определяющих секцию. Столбец используется для секционированного моментального снимка.|  
|**dynamic_filter_hostname**|**sysname**|Значение, используемое для вычисления [HOST_NAME &#40;функции&#41;Transact-SQL ](../../t-sql/functions/host-name-transact-sql.md) в параметризованных фильтрах, определяющих секцию. Столбец используется для секционированного моментального снимка.|  
|**publisher_security_mode**|**smallint**|Режим безопасности, используемый агентом при подключении к издателю, может принимать одно из следующих значений:<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Проверка подлинности<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] Проверка подлинности Windows.|  
|**publisher_login**|**sysname**|Имя входа, используемое для соединения с издателем.|  
|**publisher_password**|**nvarchar (524)**|Зашифрованное значение пароля, используемое для соединения с издателем.|  
|**job_step_uid**|**uniqueidentifier**|Уникальный идентификатор шага задания агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], на котором запущен агент.|  
|**job_login**|**sysname**||  
|**job_password**|**nvarchar (524)**||  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
