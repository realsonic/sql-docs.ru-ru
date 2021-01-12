---
description: sys.dm_os_performance_counters (Transact-SQL)
title: sys.dm_os_performance_counters (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_performance_counters
- sys.dm_os_performance_counters_TSQL
- dm_os_performance_counters_TSQL
- sys.dm_os_performance_counters
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_performance_counters dynamic management view
ms.assetid: a1c3e892-cd48-40d4-b6be-2a9246e8fbff
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 68d8c4eb6b0ee1da0427894821886f98129f6585
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094079"
---
# <a name="sysdm_os_performance_counters-transact-sql"></a>sys.dm_os_performance_counters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает по строке на каждый счетчик производительности, хранимый на сервере. Сведения о каждом счетчике производительности см. в разделе [Use SQL Server Objects](../../relational-databases/performance-monitor/use-sql-server-objects.md).  
  
> [!NOTE]  
>  Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_os_performance_counters**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**object_name**|**nchar (128)**|Категория, к которой принадлежит счетчик.|  
|**counter_name**|**nchar (128)**|Имя счетчика. Для получения дополнительных сведений о счетчике это имя раздела для выбора из списка счетчиков, [используемых SQL Server объектов](../../relational-databases/performance-monitor/use-sql-server-objects.md). |  
|**instance_name**|**nchar (128)**|Имя заданного экземпляра счетчика. Обычно содержит имя базы данных.|  
|**cntr_value**|**bigint**|Текущее значение счетчика.<br /><br /> **Примечание.** Для счетчиков за секунду это значение является кумулятивным. Значение частоты должно быть вычислено выборкой значений в дискретные интервалы времени. Разность между двумя последовательными значениям выборки равна частоте используемого интервала времени.|  
|**cntr_type**|**int**|Тип счетчика, как определено архитектурой производительности Windows. Дополнительные сведения о типах счетчиков производительности см. в разделе [типы счетчиков производительности WMI](/windows/desktop/WmiSdk/wmi-performance-counter-types) в документации или на сервере Windows Server.|  
|**pdw_node_id**|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="remarks"></a>Комментарии  
 Если экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не отображает счетчики производительности операционной системы Windows, выполните следующий запрос [!INCLUDE[tsql](../../includes/tsql-md.md)], чтобы убедиться, что счетчики производительности отключены.  
  
```sql  
SELECT COUNT(*) FROM sys.dm_os_performance_counters;  
```  
  
Если возвращено 0 строк, значит, счетчики производительности отключены. Затем следует просмотреть журнал установки и выполнить поиск ошибки 3409 `Reinstall sqlctr.ini for this instance, and ensure that the instance login account has correct registry permissions.` . Это означает, что счетчики производительности не были включены. Ошибки, находящиеся непосредственно перед ошибкой 3409, должны указывать первопричину сбоя счетчиков производительности. Дополнительные сведения о файлах журнала установки см. в разделе [Просмотр и чтение SQL Server файлов журнала установки](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  

Счетчики производительности, в которых `cntr_type` значение столбца 65792, 272696320 и 537003264, отображают значение счетчика мгновенных снимков.

Счетчики производительности, в которых `cntr_type` значение столбца 272696576, 1073874176 и 1073939712, вместо мгновенного моментального снимка отображает накопительные значения счетчиков. Таким образом, для чтения, подобного моментальному снимку, необходимо сравнить разницу между двумя точками сбора.

## <a name="permission"></a>Разрешение

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   
 
## <a name="examples"></a>Примеры  
 В следующем примере возвращаются все счетчики производительности, отображающие значения счетчика моментальных снимков.  
  
```sql  
SELECT object_name, counter_name, instance_name, cntr_value, cntr_type  
FROM sys.dm_os_performance_counters
WHERE cntr_type = 65792 OR cntr_type = 272696320 OR cntr_type = 537003264;  
```  
  
## <a name="see-also"></a>См. также:  
  [SQL Server динамические административные представления, связанные с операционной системой &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.sysperfinfo (Transact-SQL)](../../relational-databases/system-compatibility-views/sys-sysperfinfo-transact-sql.md)  
  
