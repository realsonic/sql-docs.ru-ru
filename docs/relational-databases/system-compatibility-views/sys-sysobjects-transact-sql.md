---
description: sys.sysobjects (Transact-SQL)
title: sys.sysобъекты (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sysobjects_TSQL
- sysobjects
- sysobjects_TSQL
- sys.sysobjects
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysobjects compatibility view
- sysobjects system table
ms.assetid: 44fdc387-67b0-4139-8bf5-ed26cf640cd1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 30d2b16bb9e0366752418fb3e958cdc0600db14b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099116"
---
# <a name="syssysobjects-transact-sql"></a>sys.sysobjects (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Содержит одну строку для каждого объекта, созданного внутри базы данных, такого, как ограничение, значение по умолчанию, журнал, правило и хранимая процедура.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|name|**sysname**|Имя объекта|  
|идентификатор|**int**|Идентификатор объекта|  
|xtype|**char(2)**|Тип объекта. Может быть одним из следующих типов объекта:<br /><br /> AF = агрегатная функция (среда CLR)<br /><br /> C = ограничение CHECK<br /><br /> D = ограничение по умолчанию или DEFAULT<br /><br /> F = ограничение FOREIGN KEY<br /><br /> L = журнал<br /><br /> FN = скалярная функция<br /><br /> FS = скалярная функция сборки (среда CLR)<br /><br /> FT = функция сборки (среда CLR) с табличным значением<br /><br /> IF = подставляемая табличная функция<br /><br /> IT = внутренняя таблица<br /><br /> P = хранимая процедура<br /><br /> PC = хранимая процедура сборки (среда CLR)<br /><br /> PK = ограничение PRIMARY KEY (тип K)<br /><br /> RF = хранимая процедура фильтра репликации<br /><br /> S = системная таблица<br /><br /> SN = синоним<br /><br /> SQ = очередь обслуживания<br /><br /> TA = триггер DML сборки (среда CLR)<br /><br /> TF = табличная функция<br /><br /> TR = триггер DML SQL<br /><br /> TT = табличный тип<br /><br /> U = пользовательская таблица<br /><br /> UQ = ограничение UNIQUE (тип K)<br /><br /> V = представление<br /><br /> X = расширенная хранимая процедура|  
|uid|**smallint**|Идентификатор схемы владельца объекта. Для баз данных, обновленных из предыдущих версий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Идентификатор схемы эквивалентен идентификатору владельца. Вызывает переполнение или возвращает значение NULL, если количество пользователей и ролей превышает 32 767.<br /><br /> **\* \* Важно \* ! \*** при использовании любой из следующих [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] инструкций DDL необходимо использовать представление каталога [sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) вместо sys.sysобъектов.<br /><br /> СОЗДАНИЕ &#124; ALTER &#124; DROP USER<br /><br /> СОЗДАНИЕ РОЛИ УДАЛЕНИЯ &#124; ALTER &#124;<br /><br /> СОЗДАНИЕ &#124; ALTER &#124; РОЛЬ ПРИЛОЖЕНИЯ<br /><br /> CREATE SCHEMA<br /><br /> ALTER AUTHORIZATION ON OBJECT|  
|сведения|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|status|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|base_schema_ver|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|replinfo|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|parent_obj|**int**|Идентификатор родительского объекта. Например, идентификатор таблицы, если это триггер или ограничение.|  
|crdate|**datetime**|Дата создания объекта.|  
|ftcatid|**smallint**|Идентификатор полнотекстового каталога для всех пользовательских таблиц, зарегистрированных для полнотекстового индексирования, и 0 для всех пользовательских таблиц, незарегистрированных для полнотекстового индексирования.|  
|schema_ver|**int**|Номер версии, который увеличивается каждый раз при изменении схемы для таблицы. Всегда возвращает 0.|  
|stats_schema_ver|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|тип|**char(2)**|Тип объекта. Может иметь одно из следующих значений:<br /><br /> AF = агрегатная функция (среда CLR)<br /><br /> C = ограничение CHECK<br /><br /> D = ограничение по умолчанию или DEFAULT<br /><br /> F = ограничение FOREIGN KEY<br /><br /> FN = скалярная функция<br /><br /> FS = скалярная функция сборки (среда CLR)<br /><br /> FT = функция сборки  с табличным значением (среда CLR) IF = подставляемая табличная функция<br /><br /> IT = внутренняя таблица<br /><br /> K = ограничение PRIMARY KEY или UNIQUE<br /><br /> L = журнал<br /><br /> P = хранимая процедура<br /><br /> PC = хранимая процедура сборки (среда CLR)<br /><br /> R = правило<br /><br /> RF = хранимая процедура фильтра репликации<br /><br /> S = системная таблица<br /><br /> SN = синоним<br /><br /> SQ = очередь обслуживания<br /><br /> TA = триггер DML сборки (среда CLR)<br /><br /> TF = табличная функция<br /><br /> TR = триггер DML SQL<br /><br /> TT = табличный тип<br /><br /> U = пользовательская таблица<br /><br /> V = представление<br /><br /> X = расширенная хранимая процедура|  
|userstat|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|sysstat|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|indexdel|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|refdate|**datetime**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|version|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|deltrig|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|instrig|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|updtrig|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|seltrig|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|категория|**int**|Используется для публикаций, ограничений и идентификаторов.|  
|cache|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
## <a name="see-also"></a>См. также:  
 [Сопоставление системных таблиц с системными представлениями &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Представления совместимости (Transact-SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
