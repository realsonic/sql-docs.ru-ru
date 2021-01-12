---
description: conflict_ &lt; Schema &gt; _ &lt; Table &gt; (Transact-SQL)
title: conflict_ &lt; Schema &gt; _ &lt; Table &gt; (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 01/15/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- conflict_
- conflict_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- conflict_<schema>_<table>
ms.assetid: 15ddd536-db03-454e-b9b5-36efe1f756d7
author: cawrites
ms.author: chadam
ms.openlocfilehash: a7e5f2752557f63acffa69908f1c95025df83ae5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093782"
---
# <a name="conflict_ltschemagt_lttablegt-transact-sql"></a>conflict_ &lt; Schema &gt; _ &lt; Table &gt; (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Таблица conflict_ \<schema> _ \<table> содержит сведения о конфликтующих строках в одноранговой репликации. Для каждой реплицируемой таблицы в публикации существует таблица конфликтов, при этом к имени таблицы конфликтов добавляются имя схемы и имя статьи. Такие таблицы конфликтов существуют во всех базах данных публикаций для каждой из статей.  
  
 При одноранговой репликации агент распространителя при обнаружении конфликта завершается ошибкой. Эта ошибка заносится в журнал, но данные конфликта не записываются в таблицу конфликтов и поэтому недоступны для просмотра. Если агенту распространителя разрешено продолжение работы, то конфликт заносится в локальный журнал на каждом из узлов, где он обнаружен. Дополнительные сведения см. в подразделе «Обработка конфликтов» раздела [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|__$originator_id|**int**|Идентификатор узла, из которого было произведено изменение, вызвавшее конфликт. Чтобы получить список идентификаторов, выполните [sp_help_peerconflictdetection](../../relational-databases/system-stored-procedures/sp-help-peerconflictdetection-transact-sql.md).|  
|__$origin_datasource|**int**|Узел, на котором было произведено изменение, вызвавшее конфликт.|  
|__$tranid|**nvarchar (40)**|Регистрационный номер транзакции в журнале (LSN) изменения, вызвавшего конфликт, при его применении на __$origin_datasource.|  
|__$conflict_type|**int**|Тип конфликта, который может принимать одно из следующих значений:<br /><br /> 1: не удалось выполнить обновление, так как локальная строка была изменена другим обновлением, или она была удалена, а затем вновь вставлена.<br /><br /> 2: не удалось выполнить обновление, так как локальная строка уже удалена.<br /><br /> 3: произошел сбой удаления, так как локальная строка была изменена другим обновлением или была удалена, а затем вновь вставлена.<br /><br /> 4: не удалось выполнить удаление, так как локальная строка уже удалена.<br /><br /> 5: не удалось выполнить вставку, так как локальная строка уже вставлена или была вставлена, а затем обновлена.|  
|__$is_winner|**bit**|Указывает, была ли строка в этой таблице победителем конфликта, что означает, что она была применена к локальному узлу.|  
|__$pre_version|**varbinary (32)**|Версия базы данных, из которой было произведено изменение, вызвавшее конфликт.|  
|__$reason_code|**int**|Код разрешения конфликта. Может иметь одно из следующих значений:<br /><br /> 0<br /><br /> 1<br /><br /> 2<br /><br /> <br /><br /> Дополнительные сведения см. в разделе **__ $ reason_text**.|  
|__$reason_text|**nvarchar (720)**|Разрешение конфликта. Может иметь одно из следующих значений:<br /><br /> 1 = Разрешен<br /><br /> 2 = Не разрешен<br /><br /> 0 = Неизвестно|  
|__$update_bitmap|**varbinary (** *n* **)**. Размер зависит от содержимого.|Битовая карта, указывающая столбцы, которые были обновлены в случае конфликта «обновление-обновление».|  
|__$inserted_date|**datetime**|Дата и время вставки конфликтующей строки в эту таблицу.|  
|__$row_id|**timestamp**|Версия строки, которая связана со строкой, вызвавшей конфликт.|  
|__$change_id|**Binary (8)**|Для локальной строки это значение равно __$row_id входящей строки, вызвавшей конфликт с локальной строкой. Для входящей строки это значение равно NULL.|  
|\<base table column names>|\<base table column types>|Конфликтующая строка содержит один столбец для каждого из столбцов базовой таблицы.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
