---
description: MSreplication_monitordata (Transact-SQL)
title: MSreplication_monitordata (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSreplication_monitordata_TSQL
- MSreplication_monitordata
dev_langs:
- TSQL
helpviewer_keywords:
- MSreplication_monitordata system table
ms.assetid: 843d3ffd-a1ef-4fd5-a744-c2252199793e
author: cawrites
ms.author: chadam
ms.openlocfilehash: bbe408296895f593de0af1da852b807082e8e7cf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098164"
---
# <a name="msreplication_monitordata-transact-sql"></a>MSreplication_monitordata (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Таблица **MSreplication_monitordata** содержит кэшированные данные, используемые монитором репликации, с одной строкой для каждой отслеживаемой подписки. Эта таблица хранится в базе данных распространителя.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**lastrefresh**|**datetime**|Дата и время обновления монитора.|  
|**computetime**|**int**|Время (в секундах), которое заняло вычисление отслеживаемых данных.|  
|**publication_id**|**int**|Идентификатор публикации.|  
|**publisher**|**sysname**|Имя издателя.|  
|**publisher_srvid**|**int**|Идентификатор сервера издателя.|  
|**publisher_db**|**sysname**|Имя базы данных публикации.|  
|**публикации**|**sysname**|Имя публикации.|  
|**publication_type**|**int**|Тип публикации, может принимать одно из следующих значений:<br /><br /> **0** = публикация транзакций<br /><br /> **1** = публикация моментальных снимков<br /><br /> **2** = публикация слиянием|  
|**agent_type**|**int**|Тип агента репликации, может принимать одно из следующих значений.<br /><br /> **1** = агент моментальных снимков<br /><br /> **2** = агент чтения журнала<br /><br /> **3** = агент распространения<br /><br /> **4** = агент слияния<br /><br /> **9** = агент чтения очереди|  
|**agent_id**|**int**|Идентификатор агента репликации.|  
|**agent_name**|**sysname**|Имя задания агента репликации.|  
|**job_id**|**uniqueidentifier**|Идентификатор GUID задания агента репликации.|  
|**status**|**int**|Состояние агента репликации, может принимать одно из следующих значений:<br /><br /> **1** = запущено<br /><br /> **2** = успех<br /><br /> **3** = выполняется<br /><br /> **4** = бездействие<br /><br /> **5** = повторная попытка<br /><br /> **6** = сбой|  
|**isagentrunningnow**|**bit**|Флаг, указывающий, выполняется ли в данный момент задание агента, где значение **1** означает, что задание выполняется.|  
|**warning**|**int**|Пороговое предупреждение, сформированное подпиской, может быть результатом логической операции OR следующих значений:<br /><br /> **1** = истечение срока действия — подписка на публикацию транзакций превысила срок хранения более допустимого порога в процентах от срока хранения.<br /><br /> **2** = задержка — время, затраченное на репликацию данных от издателя транзакций подписчику, превышает пороговое значение в секундах.<br /><br /> **4** = mergeexpiration — подписка на публикацию слиянием превысила срок хранения, превышающий допустимый порог, в процентах от срока хранения. 8 = mergefastrunduration — время, затраченное на завершение синхронизации подписки слиянием, превысило пороговое значение, указанное в секундах, для быстрого сетевого соединения.<br /><br /> **16** = mergeslowrunduration — время, затраченное на выполнение синхронизации подписки на публикацию слиянием, превышает пороговое значение (в секундах) при низком или коммутируемом сетевом подключении.<br /><br /> **32** = mergefastrunspeed — скорость доставки строк во время синхронизации подписки на публикацию слиянием не смогла поддерживать пороговое значение скорости (в строках в секунду) при быстром сетевом подключении.<br /><br /> **64** = mergefastrunspeed — скорость доставки строк во время синхронизации подписки на публикацию слиянием не смогла поддерживать пороговое значение скорости (в строках в секунду) по отношению к низкому или коммутируемому сетевому соединению.|  
|**last_distsync**|**datetime**|Дата и время последнего запуска агента распространителя.|  
|**agentstoptime**|**datetime**|Дата и время остановки агента.|  
|**distdb**|**sysname**|Имя базы данных распространителя для данной подписки.|  
|**хранения**|**int**|Срок хранения для публикации.|  
|**time_stamp**|**datetime**|Только для внутреннего использования.|  
|**worst_latency**|**int**|Наибольшая задержка (в секундах) при изменении данных, зафиксированная для публикации транзакций агентом чтения журнала или агентом распространителя.|  
|**best_latency**|**int**|Наименьшая задержка (в секундах) изменения данных, зафиксированная для публикации транзакций агентом чтения журнала или агентом распространителя.|  
|**avg_latency**|**int**|Средняя задержка (в секундах) изменения данных, зафиксированная для публикации транзакций агентом чтения журнала или агентом распространителя.|  
|**cur_latency**|**int**|Задержка (в секундах) при изменении данных, зафиксированная для публикации транзакций агентом чтения журнала или агентом распространителя для текущего запуска.|  
|**worst_runspeedPerf**|**int**|Наибольшая длительность синхронизации для публикации слиянием|  
|**best_runspeedPerf**|**int**|Наименьшая длительность синхронизации для публикации слиянием|  
|**average_runspeedPerf**|**int**|Средняя длительность синхронизации для публикации слиянием|  
|**mergePerformance**|**int**|Производительность последней синхронизации по сравнению со всеми синхронизациями для данной подписки. Вычисляется как скорость доставки последней синхронизации, поделенная на среднее арифметическое скоростей доставки для всех предыдущих синхронизаций.|  
|**mergelatestsessionrunduration**|**int**|Длительность самого последнего выполнения агента слияния.|  
|**mergelatestsessionrunspeed**|**float(53)**|Скорость доставки при самом последнем выполнении агента слияния.|  
|**mergelatestsessionconnectiontype**|**int**|Тип соединения, использованный последним сеансом агента слияния, может принимать одно из следующих значений:<br /><br /> **1** = локальная сеть (LAN)<br /><br /> **2** = коммутируемое сетевое подключение|  
|**retention_period_unit**|**tinyint**|Определяет единицу измерения для указания срока хранения, может принимать одно из следующих значений:<br /><br /> **1** = Неделя<br /><br /> **2** = Месяц<br /><br /> **3** = Год|  
  
## <a name="see-also"></a>См. также:  
 [Программный мониторинг репликации](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)   
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_replmonitorhelpsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelpsubscription-transact-sql.md)   
 [sp_replmonitorhelppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelppublication-transact-sql.md)   
 [sp_replmonitorhelppublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelppublisher-transact-sql.md)   
 [sp_replmonitorhelpmergesession &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelpmergesession-transact-sql.md)   
 [sp_replmonitorhelppublicationthresholds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelppublicationthresholds-transact-sql.md)   
 [sp_replmonitorhelpmergesessiondetail &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelpmergesessiondetail-transact-sql.md)  
  
  
