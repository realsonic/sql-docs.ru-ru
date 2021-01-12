---
description: sysmergesubscriptions (Transact-SQL)
title: sysmergesubscriptions (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysmergesubscriptions_TSQL
- sysmergesubscriptions
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergesubscriptions system table
ms.assetid: 6adc78da-991d-4c08-98c3-ecb4762e0e99
author: cawrites
ms.author: chadam
ms.openlocfilehash: e3caa66b5e8e938a0e54f7212b37e0442e29ebfe
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98090969"
---
# <a name="sysmergesubscriptions-transact-sql"></a>sysmergesubscriptions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Таблица в базе данных издателя, которая содержит по одной строке на каждого известного подписчика. Эта таблица хранится в базах данных публикации и подписки.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|subscriber_server|**sysname**|Идентификатор сервера. Используется для сопоставления поля srvid с уникальным для сервера значением при размещении копии базы данных подписки на другом сервере.|  
|db_name|**sysname**|Имя подписывающейся базы данных.|  
|pubid|**uniqueidentifier**|Идентификатор публикации, на которую осуществляется подписка.|  
|datasource_type|**int**|Тип источника данных:<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> **2** = Jet OLE DB.|  
|subid|**uniqueidentifier**|Уникальный идентификационный номер подписки.|  
|replnickname|**binary**|Сжатый псевдоним реплики.|  
|replicastate|**uniqueidentifier**|Уникальный идентификатор, используемый для определения того, была ли успешна предыдущая синхронизация путем сравнения значений на стороне издателя и подписчика.|  
|status|**tinyint**|Состояние подписки.<br /><br /> **0** = неактивно.<br /><br /> **1** = активно.<br /><br /> **2** = удалено.|  
|subscriber_type|**int**|Тип подписчика:<br /><br /> **1** = глобальный.<br /><br /> **2** = локальная.<br /><br /> **3** = анонимный.|  
|subscription_type|**int**|Тип подписки.<br /><br /> **0** = принудительная отправка.<br /><br /> **1** = по запросу.<br /><br /> **2** = анонимный.|  
|sync_type|**tinyint**|Тип синхронизации:<br /><br /> **1** = автоматический.<br /><br /> **2** = нет синхронизации.|  
|description|**nvarchar(255)**|Краткое описание подписки.|  
|priority|**real**|Указывает приоритет подписки и позволяет реализовать разрешение связанных с ним конфликтов. Равняется **0,00** для всех локальных или анонимных подписок.|  
|recgen|**bigint**|Номер последнего полученного поколения данных.|  
|recguid|**uniqueidentifier**|Уникальный идентификатор полученного поколения данных.|  
|sentgen|**bigint**|Номер последнего отправленного поколения данных.|  
|sentguid|**uniqueidentifier**|Уникальный идентификатор последнего отправленного поколения данных.|  
|schemaversion|**int**|Номер последней полученной схемы.|  
|schemaguid|**uniqueidentifier**|Уникальный идентификатор последней полученной схемы.|  
|last_validated|**datetime**|**Дата и время** последней успешной проверки данных подписчика.|  
|attempted_validate|**datetime**|Последняя **Дата и время** попытки проверки подписки.|  
|last_sync_date|**datetime**|**Дата и время** синхронизации.|  
|last_sync_status|**int**|Состояние подписки:<br /><br /> **0** = все задания ожидают запуска.<br /><br /> **1** = одно или несколько заданий запускаются.<br /><br /> **2** = все задания выполнены успешно.<br /><br /> **3** = выполняются по крайней мере одно задание.<br /><br /> **4** = все задания запланированы и находятся в состоянии простоя.<br /><br /> **5** = по крайней мере одно задание пытается выполнить после предыдущего сбоя.<br /><br /> **6** = не удалось успешно выполнить по крайней мере одно задание.|  
|last_sync_summary|**sysname**|Описание результатов последней синхронизации.|  
|metadatacleanuptime|**datetime**|Последняя **Дата и время** , когда метаданные с истекшим сроком действия были удалены из системных таблиц репликации слиянием.|  
|partition_id|**int**|Идентифицирует предварительно вычисляемую секцию, которой принадлежит подписка.|  
|cleanedup_unsent_changes|**bit**|Указывает, что метаданные для неотправленных изменений были удалены на стороне подписчика.|  
|replica_version|**int**|Идентифицирует версию [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] владельца данной подписки и может принимать одно из следующих значений:<br /><br /> **90** = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]<br /><br /> **100** = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|  
|supportability_mode|**int**|Только для внутреннего использования.|  
|application_name|**nvarchar(128)**|Только для внутреннего использования.|  
|subscriber_number|**int**|Только для внутреннего использования.|  
|last_makegeneration_datetime|**datetime**|Последняя **Дата и время** запуска процесса Makegeneration для издателя. Дополнительные сведения см. в описании параметра-MakeGenerationInterval в [агент слияния репликации](../../relational-databases/replication/agents/replication-merge-agent.md).|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации (Transact-SQL)](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
