---
description: sys.sql_modules (Transact-SQL)
title: sys.sql_modules (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 11/06/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sql_modules_TSQL
- sql_modules
- sql_modules_TSQL
- sys.sql_modules
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sql_modules catalog view
ms.assetid: 23d3ccd2-f356-4d89-a2cd-bee381243f99
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a3b841674bd7abffada31bd2baf7e0ab39686a3e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093093"
---
# <a name="syssql_modules-transact-sql"></a>sys.sql_modules (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает по строке для каждого объекта, который является определенным языком SQL в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , включая скалярно скомпилированную определяемую пользователем функцию. Объекты типа P, RF, V, TR, FN, IF, TF и R имеют сопоставленный с ними SQL модуль. Изолированные значения по умолчанию и объекты типа D в этом представлении также имеют определение SQL модуля. Описание этих типов см. в столбце **тип** в представлении каталога [sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) .  
  
 Дополнительные сведения см. в разделе [Скалярные определяемые пользователем функции для выполняющейся в памяти OLTP](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Идентификатор объекта, содержащего данный объект. Уникален в базе данных.|  
|**definition**|**nvarchar(max)**|Текст на языке SQL, определяющий этот модуль. Это значение можно также получить с помощью встроенной функции [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md) .<br /><br /> NULL = зашифрован.|  
|**uses_ansi_nulls**|**bit**|Модуль был создан с параметром SET ANSI_NULLS ON.<br /><br /> Всегда будет равен 0 (нулю) для правил и умолчаний.|  
|**uses_quoted_identifier**|**bit**|Модуль был создан с параметром SET QUOTED_IDENTIFIER ON.|  
|**is_schema_bound**|**bit**|Модуль был создан с параметром SCHEMABINDING.<br /><br /> Всегда содержит значение 1 для скомпилированных собственными средствами хранимых процедур.|  
|**uses_database_collation**|**bit**|1 = определение модуля, ограниченное схемой, зависит от принятых по умолчанию параметров сортировки базы данных для правильной оценки; в противном случае — 0. Такая зависимость предотвращает изменение параметров сортировки по умолчанию для базы данных.|  
|**is_recompiled**|**bit**|Процедура была создана с параметром WITH RECOMPILE.|  
|**null_on_null_input**|**bit**|Модуль был объявлен, чтобы обеспечить выходные значения NULL для любых входных значений NULL.|  
|**execute_as_principal_id**|**Int**|ID-идентификатор участника базы данных, указанного в инструкции EXECUTE AS.<br /><br /> По умолчанию и в случае EXECUTE AS CALLER имеет значение NULL.<br /><br /> ИДЕНТИФИКАТОР указанного участника, если он выполняется как SELF или EXECUTE AS \<principal> .<br /><br /> -2 = EXECUTE AS OWNER.|  
|**uses_native_compilation**|**bit**|**Применимо к**: с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] до [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].<br /><br /> 0 = не скомпилированы в собственном коде<br /><br /> 1 = скомпилированы в собственном коде<br /><br /> Значение по умолчанию — 0.|  
|**is_inlineable**|**bit**|**Область применения**: [!INCLUDE[ssSQL15](../../includes/sssqlv15-md.md)] и более поздних версий.<br/><br />Указывает, является ли модуль встроенным. Встраивание зависит от условий, указанных [здесь](../user-defined-functions/scalar-udf-inlining.md#inlineable-scalar-udfs-requirements).<br /><br /> 0 = невстраиваемые<br /><br /> 1 = является встроенным. <br /><br /> Для скалярных пользовательских функций значение будет равно 1, если определяемая пользователем функция является встроенной, и 0 в противном случае. Он всегда содержит значение 1 для встроенного возвращающие табличное, а 0 — для всех других типов модулей.<br />|  
|**inline_type**|**bit**|**Область применения**: [!INCLUDE[ssSQL15](../../includes/sssqlv15-md.md)] и более поздних версий.<br /><br />Указывает, включено ли встраивание для модуля в настоящее время. <br /><br />0 = встраивание отключено<br /><br /> 1 = Встраивание включено.<br /><br /> Для скалярных пользовательских функций значение будет равно 1, если встраивание включено (явно или неявно). Значение всегда будет равно 1 для встроенных возвращающие табличное и 0 для других типов модулей.<br />|  

  
## <a name="remarks"></a>Комментарии  
 Выражение SQL для ограничения по УМОЛЧАНИю, объект типа D, находится в представлении каталога [sys.default_constraints](../../relational-databases/system-catalog-views/sys-default-constraints-transact-sql.md) . Выражение SQL для ПРОВЕРОЧного ограничения, объект типа C, находится в представлении каталога [sys.CHECK_CONSTRAINTS](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md) .  
  
 Эти сведения также описаны в [sys.dm_db_uncontained_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md).  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Примеры  
 В ходе выполнения представленного ниже примера выполняется отображение имен, типов и определений всех модулей в текущей базе данных.  
  
```  
SELECT sm.object_id, OBJECT_NAME(sm.object_id) AS object_name, o.type, o.type_desc, sm.definition  
FROM sys.sql_modules AS sm  
JOIN sys.objects AS o ON sm.object_id = o.object_id  
ORDER BY o.type;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Запросы к системному каталогу SQL Server вопросы и ответы](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [Выполняющаяся в памяти OLTP (оптимизация в памяти)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
