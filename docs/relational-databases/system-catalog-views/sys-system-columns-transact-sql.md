---
description: sys.system_columns (Transact-SQL)
title: sys.system_columns (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- system_columns_TSQL
- system_columns
- sys.system_columns
- sys.system_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.system_columns catalog view
ms.assetid: 4ab1d48a-d57a-4e76-a08c-9627eeaf4588
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9af22522c2aad0b670ac7b7ff58e727ad2ae510b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171766"
---
# <a name="syssystem_columns-transact-sql"></a>sys.system_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Содержит по одной строке на каждый столбец системных объектов, содержащих столбцы.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Идентификатор объекта, которому принадлежит этот столбец.|  
|**name**|**sysname**|Имя столбца. Уникален в пределах объекта.|  
|**column_id**|**int**|Идентификатор столбца. Уникален в пределах объекта.<br /><br /> Идентификаторы столбца могут быть непоследовательными.|  
|**system_type_id**|**tinyint**|Идентификатор системного типа столбца.|  
|**user_type_id**|**int**|Идентификатор определенного пользователем типа столбца.<br /><br /> Чтобы вернуть имя типа, присоединитесь к представлению каталога [sys. types](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) в этом столбце.|  
|**max_length**|**smallint**|Максимальная ширина столбца (в байтах).<br /><br /> -1 = тип данных столбца — **varchar (max)**, **nvarchar (max)**, **varbinary (max)** или **XML**.<br /><br /> Для **текстовых** столбцов значение **max_length** будет равно 16, а значение задается **sp_tableoption** "text in row".|  
|**precision**|**tinyint**|Точность столбца, если он является числовым; в противном случае — 0.|  
|**масштаб**|**tinyint**|Масштаб столбца, если он является числовым; в противном случае — 0.|  
|**collation_name**|**sysname**|Имя параметров сортировки столбца, если он символьный; в противном случае — значение NULL.|  
|**is_nullable**|**bit**|1 = столбец может принимать значение NULL.|  
|**is_ansi_padded**|**bit**|1 = столбец использует поведение ANSI_PADDING ON, если имеет тип данных character, binary или variant.<br /><br /> 0 = столбец имеет тип данных, отличный от character, binary или variant.|  
|**is_rowguidcol**|**bit**|1 = столбец объявлен как ROWGUIDCOL.|  
|**is_identity**|**bit**|1 = столбец идентификаторов.|  
|**is_computed**|**bit**|1 = столбец является вычисляемым.|  
|**is_filestream**|**bit**|1 = для столбца используется потоковое хранилище.|  
|**is_replicated**|**bit**|1 = столбец реплицирован.|  
|**is_non_sql_subscribed**|**bit**|1 = у столбца есть подписчик, отличный от [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**is_merge_published**|**bit**|1 = столбец публикуется слиянием.|  
|**is_dts_replicated**|**bit**|1 = столбец реплицируется с помощью служб [!INCLUDE[ssIS](../../includes/ssis-md.md)].|  
|**is_xml_document**|**bit**|1 = содержимое является готовым XML-документом.<br /><br /> 0 = содержимое является фрагментом документа, либо тип данных столбца не является **XML**.|  
|**xml_collection_id**|**int**|Ненулевое значение, если тип данных столбца — **XML** , а XML-код типизирован. Значением будет идентификатор коллекции, содержащей пространство имен для проверки схем XML столбца.<br /><br /> 0 = нет коллекции схем XML.|  
|**default_object_id**|**int**|Идентификатор объекта по умолчанию, независимо от того, является ли он изолированным [sys.sp_bindefault](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)или встроенным ограничением по умолчанию на уровне столбцов. **Parent_object_id** столбец встроенного объекта по умолчанию на уровне столбца является ссылкой на саму таблицу. Или 0 — в случае, если нет значения по умолчанию.|  
|**rule_object_id**|**int**|Идентификатор изолированного правила, привязанного к столбцу с помощью **sys.sp_bindrule**.<br /><br /> 0 = изолированное правило отсутствует.<br /><br /> Сведения об ограничениях CHECK на уровне столбцов см. в разделе [sys.check_constraints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md).|  
|is_sparse|**bit**|1 = столбец является разреженным. Дополнительные сведения см. в статье [Использование разреженных столбцов](../../relational-databases/tables/use-sparse-columns.md).|  
|is_column_set|**bit**|1 = столбец является набором столбцов. Дополнительные сведения см. в статье [Использование наборов столбцов](../../relational-databases/tables/use-column-sets.md).|  
|generated_always_type|**tinyint**|Числовое значение, представляющее тип столбца:<br /><br /> 0 = NOT_APPLICABLE<br /><br /> 1 = AS_ROW_START<br /><br /> 2 = AS_ROW_END|  
|generated_always_type_desc|**nvarchar(60)**|Текстовое описание типа столбца:<br /><br /> NOT_APPLICABLE<br /><br /> AS_ROW_START<br /><br /> AS_ROW_END<br /><br /> **Область применения**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] и более поздних версий.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Запросы к системному каталогу SQL Server вопросы и ответы](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.all_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.computed_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)  
  
  
