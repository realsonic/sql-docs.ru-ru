---
description: sys.database_connection_stats (база данных SQL Azure)
title: sys.database_connection_stats
titleSuffix: Azure SQL Database
ms.date: 01/28/2019
ms.service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- sys.database_connection_stats
- database_connection_stats
- database_connection_stats_TSQL
- sys.database_connection_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_connection_stats
- database_connection_stats
ms.assetid: 5c8cece0-63b0-4dee-8db7-6b43d94027ec
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: seo-dt-2019
monikerRange: = azuresqldb-current
ms.openlocfilehash: b5d01ec490009c2c3b26dd888bd6050b0638e952
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102826"
---
# <a name="sysdatabase_connection_stats-azure-sql-database"></a>sys.database_connection_stats (база данных SQL Azure)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Содержит статистику для [!INCLUDE[ssSDS](../../includes/sssds-md.md)] событий **подключения к** базе данных, предоставляя обзор успешных и неудачных попыток подключения к базе данных. Дополнительные сведения о событиях подключения см. в разделе Типы событий в [sys.event_log &#40;базе данных SQL Azure&#41;](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md).  
  
|Статистика|Type|Описание|  
|---------------|----------|-----------------|  
|**database_name**|**sysname**|Имя базы данных.|  
|**start_time**|**datetime2**|Дата и время начала интервала статистической обработки в формате UTC. Время всегда кратно 5 минутам. Пример:<br /><br /> '2011-09-28 16:00:00'<br />'2011-09-28 16:05:00'<br />'2011-09-28 16:10:00'|  
|**end_time**|**datetime2**|Дата и время окончания интервала статистической обработки в формате UTC. **End_time** всегда находится в течение 5 минут позже, чем соответствующая **start_time** в той же строке.|  
|**success_count**|**int**|Число успешных соединений.|  
|**total_failure_count**|**int**|Общее число неудачных попыток соединения. Это сумма **connection_failure_count**, **terminated_connection_count** и **throttled_connection_count** и не включает события взаимоблокировки.|  
|**connection_failure_count**|**int**|Количество сбоев входа.|  
|**terminated_connection_count**|**int**|**_Применимо только для [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] версии 11._**<br /><br /> Число прерванных соединений.|  
|**throttled_connection_count**|**int**|**_Применимо только для [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] версии 11._**<br /><br /> Число регулируемых соединений.|  
  
## <a name="remarks"></a>Комментарии  
  
### <a name="event-aggregation"></a>Статистическая обработка событий

 Сведения о событиях для этого представления собираются и обрабатываются каждые 5 минут. Столбцы счетчиков представляют количество возникновения определенного события подключения для конкретной базы данных в течение заданного интервала времени.  
  
 Например, если пользователю не удается подключиться к базе данных Database1 семь раз с 11:00 до 11:05 5 февраля 2012 г. (UTC), то эти сведения доступны в одной строке в следующем виде:  
  
|**database_name**|**start_time**|**end_time**|**success_count**|**total_failure_count**|**connection_failure_count**|**terminated_connection_count**|**throttled_connection_count**|  
|------------------------|---------------------|-------------------|------------------------|-------------------------------|------------------------------------|---------------------------------------|--------------------------------------|  
|`Database1`|`2012-02-05 11:00:00`|`2012-02-05 11:05:00`|`0`|`7`|`7`|`0`|`0`|  
  
### <a name="interval-start_time-and-end_time"></a>start_time и end_time интервала

 Событие включается в интервал агрегирования, когда событие происходит *в* или _после_**start_time** и _до_**end_time** для этого интервала. Например, событие, которое происходит точно в `2012-10-30 19:25:00.0000000`, будет включено только во второй интервал, показанный ниже.  
  
```  
  
start_time                    end_time  
2012-10-30 19:20:00.0000000   2012-10-30 19:25:00.0000000  
2012-10-30 19:25:00.0000000   2012-10-30 19:30:00.0000000  
```  
  
### <a name="data-updates"></a>Обновление данных

 Данные в этом представлении с течением времени накапливаются. Как правило, данные накапливаются в течение часа с начала интервала статистической обработки, но для отображения всех данных в представлении может потребоваться до 24 часов. В течение этого времени сведения в одной строке могут периодически обновляться.  
  
### <a name="data-retention"></a>Хранение данных

 Данные в этом представлении могут храниться не более 30 дней, или, возможно, меньше, в зависимости от количества баз данных и числа уникальных событий, создаваемых каждой базой данных. Для сохранения этих данных в течение более длительного периода скопируйте их в отдельную базу данных. После создания первоначальной копии представления строки могут быть обновлены по мере накопления данных. Чтобы копия данных была актуальной, периодически выполняйте просмотр таблицы для определения увеличения числа событий существующих строк и для определения новых строк (вы можете определить уникальные строки с помощью времени начала и окончания интервала), а затем обновить свою копию данных с применением этих изменений.  
  
### <a name="errors-not-included"></a>Исключенные ошибки

 Это представление может содержать не все сведения о подключениях и ошибках:  
  
- Это представление не включает все [!INCLUDE[ssSDS](../../includes/sssds-md.md)] ошибки базы данных, которые могут возникнуть, только те, которые указаны в [sys.event_log &#40;базы данных SQL Azure&#41;](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md).  
  
- Если в центре обработки данных возникает сбой компьютера [!INCLUDE[ssSDS](../../includes/sssds-md.md)] , в таблице событий может отсутствовать небольшой объем данных.  
  
- Если IP-адрес заблокирован через DoSGuard, события подключения с этого IP-адреса не могут собираться и не будут отображаться в этом представлении.  
  
## <a name="permissions"></a>Разрешения

 Пользователи с разрешением на доступ к базе данных **master** имеют доступ только для чтения к этому представлению.  
  
## <a name="example"></a>Пример

 В следующем примере показан запрос **sys.database_connection_stats** для возврата сводки о подключениях к базе данных, которые произошли между полуднем 9/25/2011 и полудня в 9/28/2011 (UTC). По умолчанию результаты запроса сортируются по **start_time** (по возрастанию).  
  
```sql
SELECT *  
FROM sys.database_connection_stats
WHERE start_time>='2011-09-25 12:00:00' and end_time<='2011-09-28 12:00:00';  
```  

## <a name="see-also"></a>См. также:

 [Устранение неполадок подключения к Базе данных SQL Azure](/azure/sql-database/sql-database-troubleshoot-common-connection-issues)  
  
  
