---
description: sys.dm_continuous_copy_status (база данных SQL Azure)
title: sys.dm_continuous_copy_status
titleSuffix: Azure SQL Database
ms.date: 03/03/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_continuous_copy_status_TSQL
- dm_continuous_copy_status_TSQL
- dm_continuous_copy_status
- sys.dm_continuous_copy_status
dev_langs:
- TSQL
helpviewer_keywords:
- dm_continuous_copy_status
- sys.dm_continuous_copy_status
ms.assetid: 411b2e71-4421-4ef5-900d-5af068750899
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 94370b562160315e53feb46a83cbb68a848e064f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099979"
---
# <a name="sysdm_continuous_copy_status-azure-sql-database"></a>sys.dm_continuous_copy_status (база данных SQL Azure)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Возвращает строку для каждой пользовательской базы данных (версии 11), которая в настоящее время находится в связи непрерывного копирования георепликации. Если для базы данных-источника инициировано несколько связей непрерывного копирования, эта таблица содержит по одной строке для каждой активной базы данных-получателя.  
  
При использовании базы данных SQL версии 12 следует использовать [sys.dm_geo_replication_link_status](../../relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md) (поскольку *sys.dm_continuous_copy_status* применимо только к версии 11).

  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**copy_guid**|**uniqueidentifier**|Уникальный идентификатор реплики базы данных.|  
|**partner_server**|**sysname**|Имя связанного сервера базы данных SQL.|  
|**partner_database**|**sysname**|Имя связанной базы данных на связанном сервере базы данных SQL.|  
|**last_replication**|**datetimeoffset**|Метка времени последней реплицированной транзакции.|  
|**replication_lag_sec**|**int**|Разница во времени в секундах между текущим временем и меткой времени последней, успешно зафиксированной транзакции в базе данных-источнике, которая не была подтверждена активной базой данных-получателем.|  
|**replication_state**|**tinyint**|Состояние репликации непрерывного копирования для этой базы данных. Ниже приведены возможные значения и их описания.<br /><br /> 1: заполнение. Выполняется первоначальное заполнение целевого объекта репликации, который находится в несогласованном состоянии на уровне транзакций. Пока первоначальное заполнение не завершится, подключиться к активной базе данных-получателю нельзя. <br />2: перехват. Активная база данных-получатель в настоящий момент синхронизируется с базой данных-источником и находится в состоянии, согласованном на уровне транзакций.<br />3. повторное заполнение. Активная база данных-получатель автоматически заполняется повторно из-за неустранимой ошибки репликации.<br />4: приостановлено. Это неактивная связь непрерывного копирования. Это состояние обычно означает, что доступной для Interlink полосы пропускания недостаточно для уровня активности транзакций в базе данных-источнике. Однако связь непрерывного копирования не повреждена.|  
|**replication_state_desc**|**nvarchar(256)**|Описание replication_state (одно из следующих значений):<br /><br /> SEEDING<br /><br /> CATCH_UP<br /><br /> RE_SEEDING<br /><br /> SUSPENDED|  
|**is_rpo_limit_reached**|**bit**|Этому параметру всегда задается значение 0.|  
|**is_target_role**|**bit**|0 = Источник связи копирования<br /><br /> 1 = Цель связи копирования|  
|**is_interlink_connected**|**bit**|1 = подключение к Interlink установлено.<br /><br /> 0 = подключение к Interlink не установлено.|  
  
## <a name="permissions"></a>Разрешения  
 Для получения данных необходимо членство в роли базы данных **db_owner** . Пользователь dbo, члены роли базы данных **DBManager** и имя входа sa могут также запрашивать это представление.  
  
## <a name="remarks"></a>Комментарии  
 Представление **sys.dm_continuous_copy_status** создается в базе данных **ресурсов** и отображается во всех базах данных, включая логическую реплику. Однако запрос этого представления для логической базы данных master возвращает пустой набор.  
  
 Если отношение непрерывного копирования к базе данных завершается, строка для этой базы данных в представлении **sys.dm_continuous_copy_status** исчезает.  
  
 Как и представление **sys.dm_database_copies** , **sys.dm_continuous_copy_status** отражает состояние отношения непрерывного копирования, в котором база данных является первичной или активной базой данных-получателем. В отличие от sys.dm_database_copies **sys.dm_continuous_copy_status** содержит несколько столбцов, содержащих сведения об операциях и производительности. К этим столбцам относятся **last_replication** и **replication_lag_sec**..  
  
## <a name="see-also"></a>См. также:  
 [sys.dm_database_copies &#40;базе данных SQL Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database.md)   
 [Активные Geo-Replication хранимые процедуры &#40;Transact-SQL&#41;](../system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
