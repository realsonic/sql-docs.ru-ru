---
description: sys.all_objects (Transact-SQL)
title: sys.all_objects (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 05/20/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.all_objects
- all_objects_TSQL
- all_objects
- sys.all_objects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.all_objects catalog view
ms.assetid: 547e4be4-a8e4-48ce-9d8d-37b169985081
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 39395b18422259219f43ddc34ac96af389490b84
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102215"
---
# <a name="sysall_objects-transact-sql"></a>sys.all_objects (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Показывает объединение UNION для всех пользовательских (заданных в соответствии со схемой) и системных объектов.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|name|**sysname**|Имя объекта.|  
|object_id|**int**|Идентификационный номер объекта. Уникален в базе данных.|  
|principal_id|**int**|Идентификатор непосредственного владельца, если он отличается от владельца схемы. По умолчанию содержащиеся в схеме объекты принадлежат владельцу схемы. Тем не менее, можно указать другого владельца с помощью инструкции ALTER AUTHORIZATION.<br /><br /> Принимает значение NULL, если нет альтернативного частного владельца.<br /><br /> Имеет значение NULL, если типом объекта является один из следующих:<br /><br /> C = ограничение CHECK<br /><br /> D = значение по умолчанию (DEFAULT), в ограничении или независимо заданное<br /><br /> F = ограничение FOREIGN KEY<br /><br /> PK = ограничение PRIMARY KEY<br /><br /> R = правило (старый стиль, изолированный)<br /><br /> TA = триггер сборки (среда CLR)<br /><br /> TR = триггер SQL<br /><br /> UQ = ограничение UNIQUE|  
|schema_id|**int**|Идентификатор схемы, содержащей объект.<br /><br /> Для всех системных объектов с областью действия в рамках схемы, которые входят в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], это значение всегда лежит в интервале (schema_id('sys'), schema_id('INFORMATION_SCHEMA')).|  
|parent_object_id|**int**|Идентификатор объекта, которому принадлежит данный объект.<br /><br /> 0 = не дочерний объект|  
|тип|**char(2)**|Тип объекта:<br /><br /> AF = агрегатная функция (среда CLR)<br /><br /> C = ограничение CHECK<br /><br /> D = значение по умолчанию (DEFAULT), в ограничении или независимо заданное<br /><br /> F = ограничение FOREIGN KEY<br /><br /> FN = скалярная функция SQL<br /><br /> FS = скалярная функция сборки (среда CLR)<br /><br /> FT = функция сборки (среда CLR) с табличным значением<br /><br /> IF = встроенная функция SQL с табличным значением<br /><br /> IT = внутренняя таблица<br /><br /> P = хранимая процедура SQL<br /><br /> PC = хранимая процедура сборки (среда CLR)<br /><br /> PG = структура плана<br /><br /> PK = ограничение PRIMARY KEY<br /><br /> R = правило (старый стиль, изолированный)<br /><br /> RF = процедура фильтра репликации<br /><br /> S = системная базовая таблица<br /><br /> SN = синоним<br /><br /> SO = объект последовательности<br /><br /> SQ = очередь обслуживания<br /><br /> TA = триггер DML сборки (среда CLR)<br /><br /> TF = возвращающая табличное значение функция SQL<br /><br /> TR = триггер DML SQL<br /><br /> TT = табличный тип<br /><br /> U = таблица (пользовательская)<br /><br /> UQ = ограничение UNIQUE<br /><br /> V = представление<br /><br /> X = расширенная хранимая процедура|  
|type_desc|**nvarchar(60)**|Описание типа объекта. AGGREGATE_FUNCTION<br /><br /> CHECK_CONSTRAINT<br /><br /> DEFAULT_CONSTRAINT<br /><br /> FOREIGN_KEY_CONSTRAINT<br /><br /> SQL_SCALAR_FUNCTION<br /><br /> CLR_SCALAR_FUNCTION<br /><br /> CLR_TABLE_VALUED_FUNCTION<br /><br /> SQL_INLINE_TABLE_VALUED_FUNCTION<br /><br /> INTERNAL_TABLE<br /><br /> SQL_STORED_PROCEDURE<br /><br /> CLR_STORED_PROCEDURE<br /><br /> PLAN_GUIDE<br /><br /> PRIMARY_KEY_CONSTRAINT<br /><br /> RULE<br /><br /> REPLICATION_FILTER_PROCEDURE<br /><br /> SYSTEM_TABLE<br /><br /> SYNONYM<br /><br /> SERVICE_QUEUE<br /><br /> CLR_TRIGGER<br /><br /> SQL_TABLE_VALUED_FUNCTION<br /><br /> SQL_TRIGGER<br /><br /> TABLE_TYPE<br /><br /> USER_TABLE<br /><br /> UNIQUE_CONSTRAINT<br /><br /> VIEW<br /><br /> EXTENDED_STORED_PROCEDURE|  
|create_date|**datetime**|Дата создания объекта.|  
|modify_date|**datetime**|Дата последнего изменения объекта с помощью инструкции ALTER. Если объект является таблицей или представлением, modify_date также изменяется при создании или изменении индекса в таблице или представлении.|  
|is_ms_shipped|**bit**|Объект, созданный с помощью внутреннего компонента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|is_published|**bit**|Объект опубликован.|  
|is_schema_published|**bit**|Опубликована только схема объекта.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.system_objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md)  
  
  
