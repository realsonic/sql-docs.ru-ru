---
description: sys.dm_exec_describe_first_result_set_for_object (Transact-SQL)
title: sys.dm_exec_describe_first_result_set_for_object (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_describe_first_result_set_for_object_TSQL
- sys.dm_exec_describe_first_result_set_for_object
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_describe_first_result_set_for_object catalog view
ms.assetid: 63b0fde7-95d7-4ad7-a219-a9feacf1bd89
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 490f2197f1399bd89b83a920085829c96a4ed52c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101656"
---
# <a name="sysdm_exec_describe_first_result_set_for_object-transact-sql"></a>sys.dm_exec_describe_first_result_set_for_object (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Эта функция динамического управления принимает в @object_id качестве параметра и описывает первый результирующий набор метаданных для модуля с таким идентификатором. Заданный параметр @object_id может быть идентификатором [!INCLUDE[tsql](../../includes/tsql-md.md)] хранимой процедуры или [!INCLUDE[tsql](../../includes/tsql-md.md)] триггера. Если же передан идентификатор любого другого объекта (представления, функции, таблицы или процедуры CLR), то в столбцах ошибки результата будет содержаться ошибка.  
  
 **sys.dm_exec_describe_first_result_set_for_object** имеет то же определение результирующего набора, что и [sys.dm_exec_describe_first_result_set &#40;transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md) и аналогично [sp_describe_first_result_set &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sys.dm_exec_describe_first_result_set_for_object   
    ( @object_id , @include_browse_information )  
```  
  
## <a name="arguments"></a>Аргументы  
 *\@object_id*  
 @object_id [!INCLUDE[tsql](../../includes/tsql-md.md)] Хранимой процедуры или [!INCLUDE[tsql](../../includes/tsql-md.md)] триггера. @object_id имеет тип **int**.  
  
 *\@include_browse_information*  
 @include_browse_information является **битом** типа. Если имеет значение 1, то каждый запрос анализируется так, как будто он имеет параметр FOR BROWSE для запроса. Возвращает дополнительные ключевые столбцы и сведения об исходной таблице.  
  
## <a name="table-returned"></a>Возвращаемая таблица  
 Эти общие метаданные возвращаются в виде результирующего набора с одной строкой для каждого столбца в результирующих метаданных. Каждая строка описывает тип и допустимость значений NULL в столбце в формате, описанном в следующем разделе. Если первая инструкция не существует для каждого пути управления, возвращается результирующий набор с нулем строк.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**is_hidden**|**bit**|Указывает, является ли столбец дополнительным, добавленным для целей просмотра данных, которые фактически не отражаются в результирующем наборе.|  
|**column_ordinal**|**int**|Содержит порядковый номер столбца в результирующем наборе. Позиция первого столбца будет указана как 1.|  
|**name**|**sysname**|Содержит имя столбца, если его можно определить. В противном случае значение равно NULL.|  
|**is_nullable**|**bit**|Содержит значение 1, если столбец допускает значения NULL, значение 0, если столбец не допускает значения NULL, или значение -1, если не удалось определить, допускает ли столбец значения NULL.|  
|**system_type_id**|**int**|Содержит system_type_id типа данных столбца, как указано в таблице sys. types. Для типов CLR, даже если system_type_name возвращает NULL, этот столбец вернет значение 240.|  
|**system_type_name**|**nvarchar(256)**|Содержит имя типа данных. Включает аргументы (длина, точность, масштаб), заданные для типа данных столбца. Если тип данных является пользовательским псевдонимом, то здесь указывается базовый системный тип данных. Если это определяемый пользователем тип данных CLR, то в этом столбце вернется NULL.|  
|**max_length**|**smallint**|Максимальная длина столбца (в байтах).<br /><br /> -1 = тип данных столбца — **varchar (max)**, **nvarchar (max)**, **varbinary (max)** или **XML**.<br /><br /> Для **текстовых** столбцов значение **max_length** будет равно 16, а значение задается **sp_tableoption "text in row"**.|  
|**precision**|**tinyint**|Точность столбца, если он является числовым. В противном случае возвращается 0.|  
|**масштаб**|**tinyint**|Масштаб значений столбца в случае числового выражения. В противном случае возвращается 0.|  
|**collation_name**|**sysname**|Имя параметров сортировки столбца, если он символьный. В противном случае возвращается NULL.|  
|**user_type_id**|**int**|Для типов CLR и псевдонимов содержит user_type_id для типа данных столбца, как указано в sys.types. В противном случае значение равно NULL.|  
|**user_type_database**|**sysname**|Для типов CLR и псевдонимов содержит имя базы данных, в которой этот тип определен. В противном случае значение равно NULL.|  
|**user_type_schema**|**sysname**|Для типов CLR и псевдонимов содержит имя схемы, в которой этот тип определен. В противном случае значение равно NULL.|  
|**user_type_name**|**sysname**|Для типов CLR и псевдонимов содержит имя типа. В противном случае значение равно NULL.|  
|**assembly_qualified_type_name**|**nvarchar(4000)**|Для типов CLR возвращает имя сборки и класса, определяющего тип. В противном случае значение равно NULL.|  
|**xml_collection_id**|**int**|Содержит xml_collection_id для типа данных столбца, как указано в sys.columns. Этот столбец возвратит NULL, если возвращаемый тип не связан с коллекцией схем XML.|  
|**xml_collection_database**|**sysname**|Содержит базу данных, в которой определена коллекция схем XML, связанная с этим типом. Этот столбец возвратит NULL, если возвращаемый тип не связан с коллекцией схем XML.|  
|**xml_collection_schema**|**sysname**|Содержит схему, в которой определена коллекция схем XML, связанная с этим типом. Этот столбец возвратит NULL, если возвращаемый тип не связан с коллекцией схем XML.|  
|**xml_collection_name**|**sysname**|Содержит имя коллекции схем XML, связанной с этим типом. Этот столбец возвратит NULL, если возвращаемый тип не связан с коллекцией схем XML.|  
|**is_xml_document**|**bit**|Возвращает значение 1, если возвращается тип данных XML, который гарантированно будет полным XML-документом (включая корневой узел), а не фрагментом XML. В противном случае возвращается 0.|  
|**is_case_sensitive**|**bit**|Возвращает значение 1, если столбец относится к строковому типу с учетом регистра, либо значение 0 в противном случае.|  
|**is_fixed_length_clr_type**|**bit**|Возвращает значение 1, если столбец относится к типу CLR с фиксированной длиной, либо значение 0 в противном случае.|  
|**source_server**|**sysname**|Имя исходного сервера, возвращаемое столбцом этого результата (если он исходит от удаленного сервера). Имя указывается в том виде, в котором оно отображается в sys. Servers.  Возвращает значение NULL, если столбец поступает с локального сервера или если невозможно определить, с какого сервера он поступил. Заполняется только при запросе просмотра информации.|  
|**source_database**|**sysname**|Имя исходной базы данных, возвращаемое столбцом этого результата. Возвращает NULL, если не удалось определить базу данных. Заполняется только при запросе просмотра информации.|  
|**source_schema**|**sysname**|Имя исходной схемы, возвращаемое столбцом в этом результате. Возвращает NULL, если не удалось определить схему. Заполняется только при запросе просмотра информации.|  
|**source_table**|**sysname**|Имя исходной таблицы, возвращаемое столбцом в этом результате. Возвращает NULL, если не удалось определить таблицу. Заполняется только при запросе просмотра информации.|  
|**source_column**|**sysname**|Имя исходного столбца, возвращаемое столбцом в этом результате. Возвращает NULL, если не удалось определить столбец. Заполняется только при запросе просмотра информации.|  
|**is_identity_column**|**bit**|Возвращает значение 1, если столбец является столбцом идентификаторов, либо значение 0 в противном случае. Возвращает NULL, если не удалось определить, является ли столбец столбцом идентификаторов.|  
|**is_part_of_unique_key**|**bit**|Возвращает значение 1, если столбец является частью уникального индекса (включая ограничение уникальности и первичности), либо значение 0 в противном случае. Возвращает NULL, если не удалось определить, является ли столбец частью уникального индекса. Заполняется только при запросе просмотра информации.|  
|**is_updateable**|**bit**|Возвращает значение 1, если столбец можно обновлять, либо значение 0 в противном случае. Возвращает NULL, если не удалось определить, можно ли обновлять столбец.|  
|**is_computed_column**|**bit**|Возвращает значение 1, если столбец является вычисляемым столбцом, либо значение 0 в противном случае. Возвращает NULL, если не удалось определить, является ли столбец вычисляемым столбцом.|  
|**is_sparse_column_set**|**bit**|Возвращает значение 1, если столбец является разреженным, и значение 0 в противном случае. Возвращает NULL, если не удалось определить, является ли столбец частью набора разреженных столбцов.|  
|**ordinal_in_order_by_list**|**smallint**|Позиция этого столбца в списке ORDER BY. Возвращает значение NULL, если столбец отсутствует в списке ORDER BY либо список ORDER BY не может быть уникальным образом определен.|  
|**order_by_list_length**|**smallint**|Длина списка ORDER BY. Возвращает NULL, если нет списка ORDER BY или если список ORDER BY нельзя однозначно определить. Обратите внимание на то, что это значение будет одним и тем же для всех строк, возвращаемых процедурой sp_describe_first_result_set.|  
|**order_by_is_descending**|**smallint NULL**|Если значение ordinal_in_order_by_list не равно NULL, то столбец **order_by_is_descending** указывает направление упорядочения предложением ORDER BY для этого столбца. В противном случае возвращается значение NULL.|  
|**error_number**|**int**|Содержит номер ошибки, возвращаемый этой функцией. Содержит NULL, если в столбце не возникло ошибок.|  
|**error_severity**|**int**|Содержит серьезность ошибки, возвращаемую этой функцией. Содержит NULL, если в столбце не возникло ошибок.|  
|**error_state**|**int**|Содержит сообщение состояния, возвращаемое этой функцией. При отсутствии ошибок. Столбец будет содержать значение NULL.|  
|**error_message**|**nvarchar (4096)**|Содержит сообщение, возвращаемое этой функцией. Столбец будет содержать NULL, если не возникло ошибок.|  
|**error_type**|**int**|Содержит целое число, представляющее возвращаемую ошибку. Соответствует error_type_desc. См. список под замечаниями.|  
|**error_type_desc**|**nvarchar(60)**|Содержит короткую строку в верхнем регистре, представляющую возвращаемую ошибку. Сопоставляется с error_type. См. список под замечаниями.|  
  
## <a name="remarks"></a>Комментарии  
 В этой функции используется тот же алгоритм, что и в процедуре **sp_describe_first_result_set**. Дополнительные сведения см. в разделе [sp_describe_first_result_set &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md).  
  
 В следующей таблице содержится список типов ошибок и их описания  
  
|error_type|error_type|Описание|  
|-----------------|-----------------|-----------------|  
|1|MISC|Все ошибки, которые не описаны иным образом.|  
|2|SYNTAX|В пакете возникла синтаксическая ошибка.|  
|3|CONFLICTING_RESULTS|Результат удалось определить из-за конфликта между двумя возможными первыми инструкциями.|  
|4|DYNAMIC_SQL|Результат не удалось определить из-за динамического SQL, который потенциально мог вернуть первый результат.|  
|5|CLR_PROCEDURE|Результат не удалось определить из-за того, что хранимая процедура CLR потенциально могла вернуть первый результат.|  
|6|CLR_TRIGGER|Результат не удалось определить из-за того, что триггер CLR потенциально мог вернуть первый результат.|  
|7|EXTENDED_PROCEDURE|Результат не удалось определить из-за того, что расширенная хранимая процедура потенциально могла вернуть первый результат.|  
|8|UNDECLARED_PARAMETER|Не удалось определить результат, так как тип данных одного или нескольких столбцов результирующего набора может зависеть от необъявленного параметра.|  
|9|RECURSION|Не удается определить результат, так как в пакете содержится рекурсивная инструкция.|  
|10|TEMPORARY_TABLE|Результат не удалось определить из-за того, что пакет содержит временную таблицу и не поддерживается хранимой процедурой **sp_describe_first_result_set**.|  
|11|UNSUPPORTED_STATEMENT|Результат не удалось определить из-за того, что пакет содержит инструкцию, не поддерживаемую хранимой процедурой **sp_describe_first_result_set** (FETCH, REVERT и т. д.).|  
|12|OBJECT_ID_NOT_SUPPORTED|@object_idПереданный функции объект не поддерживается (т. е. не является хранимой процедурой)|  
|13|OBJECT_ID_DOES_NOT_EXIST|@object_idПереданный функции объект не найден в системном каталоге.|  
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение для выполнения @tsql аргумента.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-returning-metadata-with-and-without-browse-information"></a>A. Возвращает метаданные со сведениями для просмотра и без них  
 В следующем примере создается хранимая процедура с именем TestProc2, которая возвращает два результирующих набора. Затем пример показывает, что **sys.dm_exec_describe_first_result_set** возвращает сведения о первом результирующем наборе в процедуре, со сведениями для просмотра и без них.  
  
```  
CREATE PROC TestProc2  
AS  
SELECT object_id, name FROM sys.objects ;  
SELECT name, schema_id, create_date FROM sys.objects ;  
GO  
  
SELECT * FROM sys.dm_exec_describe_first_result_set_for_object(OBJECT_ID('TestProc2'), 0) ;  
SELECT * FROM sys.dm_exec_describe_first_result_set_for_object(OBJECT_ID('TestProc2'), 1) ;  
GO  
```  
  
### <a name="b-combining-the-sysdm_exec_describe_first_result_set_for_object-function-and-a-table-or-view"></a>Б. Объединение функции sys.dm_exec_describe_first_result_set_for_object с таблицей или представлением  
 В следующем примере используются как представление каталога sys. процедур, так и функция **sys.dm_exec_describe_first_result_set_for_object** для отображения метаданных для результирующих наборов всех хранимых процедур в [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] базе данных.  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT p.name, r.*   
FROM sys.procedures AS p  
CROSS APPLY sys.dm_exec_describe_first_result_set_for_object(p.object_id, 0) AS r;  
GO  
  
```  
  
## <a name="see-also"></a>См. также:  
 [sp_describe_first_result_set &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md)   
 [sp_describe_undeclared_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md)  
  
  
