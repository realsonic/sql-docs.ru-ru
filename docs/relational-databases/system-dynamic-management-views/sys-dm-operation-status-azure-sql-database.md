---
description: sys.dm_operation_status
title: sys.dm_operation_status | Документация Майкрософт
ms.custom: ''
ms.date: 06/05/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- dm_operation_status_TSQL
- dm_operation_status
- sys.dm_operation_status
- sys.dm_operation_status_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_operation_status dynamic management view
- sys.dm_operation_status dynamic management view
ms.assetid: cc847784-7f61-4c69-8b78-5f971bb24d61
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: 21028fd5cdabf435d7672074907b20ebeaaad3c4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101556"
---
# <a name="sysdm_operation_status"></a>sys.dm_operation_status

[!INCLUDE [asdb-asdbmi-asa](../../includes/applies-to-version/asdb-asdbmi-asa.md)]

  Возвращает сведения об операциях, выполненных в базах данных на сервере [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|session_activity_id|**uniqueidentifier**|Идентификатор операции. Не равно NULL.|  
|resource_type|**int**|Обозначает тип ресурса, в котором выполняется операция. Не равно NULL. В текущем выпуске это представление отслеживает операции, выполняемые только в [!INCLUDE[ssSDS](../../includes/sssds-md.md)] и соответствующие целочисленному значению 0.|  
|resource_type_desc|**nvarchar (2048)**|Описание типа ресурса, в котором выполняется операция. В текущем выпуске это представление отслеживает операции, выполняемые только в [!INCLUDE[ssSDS](../../includes/sssds-md.md)].|  
|major_resource_id|**sql_variant**|Имя ресурса [!INCLUDE[ssSDS](../../includes/sssds-md.md)], в котором выполняется операция. Не равно NULL.|  
|minor_resource_id|**sql_variant**|Только для внутреннего использования. Не равно NULL.|  
|Операция|**nvarchar(60)**|Операции, выполняемые на [!INCLUDE[ssSDS](../../includes/sssds-md.md)], такие как CREATE или ALTER.|  
|Состояние|**tinyint**|Состояние операции.<br /><br /> 0 = Ожидает согласования<br />1 = выполняется<br />2 = завершена<br />3 = ошибка<br />4 = отмена|  
|state_desc|**nvarchar(120)**|PENDING = операция ожидает доступности ресурсов или квоты.<br /><br /> IN_PROGRESS = операция запущена и выполняется.<br /><br /> COMPLETED = операция успешно завершена.<br /><br /> FAILED = ошибка операции. Дополнительные сведения см. в столбце **error_desc** .<br /><br /> CANCELLED = выполнение операции остановлено по запросу пользователя.|  
|percent_complete|**int**|Процент завершения выполнения операции. Значения не являются непрерывными, а допустимые значения перечислены ниже. Не равно NULL.<br/><br/>0 = операция не запущена<br/>50 = выполняется операция<br/>100 = операция завершена|  
|Error_Code|**int**|Код ошибки, возникшей при неудачном выполнении операции. Если значение равно 0, операция завершилась успешно.|  
|error_desc|**nvarchar (2048)**|Описание ошибки, которая возникла во время неудачного выполнения операции.|  
|error_severity|**int**|Степень серьезности ошибки, которая возникла во время неудачного выполнения операции. Дополнительные сведения о серьезности ошибок см. в разделе [ядро СУБД серьезности ошибок](../errors-events/database-engine-error-severities.md).|  
|error_state|**int**|Зарезервировано для будущего использования. Совместимость с будущими версиями не гарантируется.|  
|start_time|**datetime**|Метка времени начала операции.|  
|last_modify_time|**datetime**|Метка времени последнего изменения записи для длительных операций. Для успешно выполненных операций в этом поле отображается метка времени завершения операции.|  
  
## <a name="permissions"></a>Разрешения  
 Это представление доступно в базе данных **master** только для входа субъекта уровня сервера.  
  
## <a name="remarks"></a>Комментарии  
 Чтобы использовать это представление, необходимо подключиться к базе данных **master** . Используйте `sys.dm_operation_status` представление в базе данных **master** [!INCLUDE[ssSDS](../../includes/sssds-md.md)] сервера для наблюдения за состоянием следующих операций, выполняемых над [!INCLUDE[ssSDS](../../includes/sssds-md.md)] :  
  
-   Создание базы данных  
  
-   Копирование базы данных. Копирование базы данных создает запись данного представления на исходном и целевом серверах.  
  
-   Изменение баз данных.  
  
-   Изменение уровня производительности уровня службы  
  
-   Изменение уровня службы базы данных, например с Basic на Standard.  
  
-   Настройка связи георепликации.  
  
-   Завершение связи георепликации.  
  
-   Восстановление базы данных  
  
-   Удаление базы данных  

Информация в этом представлении хранится примерно в течение 1 часа. Используйте [Журнал действий Azure](/azure/azure-monitor/platform/activity-log) для просмотра сведений об операциях за последние 90 дней. Для хранения данных более 90 дней рекомендуется [отправлять записи журнала действий](/azure/azure-monitor/platform/activity-log#send-to-log-analytics-workspace) в log Analytics рабочую область.

## <a name="example"></a>Пример  
 Отображение последних операций георепликации, связанных с базой данных "MyDB".  
  
```  
SELECT * FROM sys.dm_operation_status   
   WHERE major_resource_id = 'myddb'   
   ORDER BY start_time DESC;  
```  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции георепликации &#40;базе данных SQL Azure&#41;](../../relational-databases/system-dynamic-management-views/geo-replication-dynamic-management-views-and-functions-azure-sql-database.md)   
 [sys.dm_geo_replication_link_status &#40;базе данных SQL Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md)   
 [sys.geo_replication_links &#40;базе данных SQL Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database.md)   
 [ALTER DATABASE (база данных SQL Azure)](../../t-sql/statements/alter-database-transact-sql.md)  
  
