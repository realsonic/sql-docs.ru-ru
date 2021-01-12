---
description: sys.external_tables (Transact-SQL)
title: sys.external_tables (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: fac4720c-b679-4ab2-864b-ff7810a9b559
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 443086b2deb37b8c1cce1170e2869fe09db81560
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098361"
---
# <a name="sysexternal_tables-transact-sql"></a>sys.external_tables (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Содержит по одной строке для каждой внешней таблицы в текущей базе данных.  
  
|Имя столбца|Тип данных|Description|Диапазон|  
|-----------------|---------------|-----------------|-----------|  
|\<inherited columns>||Список столбцов, наследуемых этим представлением, см. в разделе [sys. objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).||  
|max_column_id_used|**int**|Максимальный идентификатор столбца, который когда-либо использовался для этой таблицы.||  
|uses_ansi_nulls|**bit**|Таблица была создана при установленном параметре SET ANSI_NULLS = ON.||  
|data_source_id|**int**|Идентификатор объекта для внешнего источника данных.||  
|file_format_id|**int**|Для внешних таблиц по внешнему источнику данных HADOOP это идентификатор объекта для формата внешнего файла.||  
|location|**nvarchar(4000)**|Для внешних таблиц по внешнему источнику данных HADOOP это путь к внешним данным в HDFS.||  
|reject_type|**tinyint**|Для внешних таблиц по внешнему источнику данных HADOOP это способ подсчета отклоненных строк при запросе внешних данных.|ЗНАЧЕНИЕ — количество отклоненных строк.<br /><br /> ПРОЦЕНТ — процент отклоненных строк.|  
|reject_value|**float**|Для внешних таблиц по внешнему источнику данных HADOOP:<br /><br /> Для *reject_type =* value это число отклонений строк, которые должны быть разрешены до сбоя запроса.<br /><br /> Для *reject_type* = процент это процент отклонений строк, которые должны быть разрешены перед отправкой запроса.||  
|reject_sample_value|**int**|Для *reject_type* = процент это число строк для загрузки (успешно или неуспешно) перед вычислением процента отклоненных строк.|NULL, если reject_type = VALUE.|  
|distribution_type|**int**|Для внешних таблиц на SHARD_MAP_MANAGER внешнем источнике данных это распределение данных по базовым таблицам.|0 — сегментированный<br /><br /> 1 — реплицировано<br /><br /> 2 — циклический перебор|  
|distribution_desc|**nvarchar(120)**|Для внешних таблиц на SHARD_MAP_MANAGER внешнем источнике данных это тип распределения, отображаемый в виде строки.||  
|sharding_column_id|**int**|Для внешних таблиц по SHARD_MAP_MANAGER внешнему источнику данных и сегментированном распределении это идентификатор столбца, содержащего значения ключа сегментирования.||  
|remote_schema_name|**sysname**|Для внешних таблиц, посвященных внешнему источнику данных SHARD_MAP_MANAGER, это схема, в которой базовая таблица находится в удаленных базах данных (если она отличается от схемы, в которой определена внешняя таблица).||  
|remote_object_name|**sysname**|Для внешних таблиц, посвященных внешнему источнику данных SHARD_MAP_MANAGER, это имя базовой таблицы в удаленных базах данных (если оно отличается от имени внешней таблицы).||  
  
## <a name="permissions"></a>Разрешения  
 Видимость метаданных в представлениях каталогов ограничивается защищаемыми объектами, которыми пользователь владеет или на которые ему были предоставлены разрешения. Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [sys.external_file_formats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-external-file-formats-transact-sql.md)   
 [sys.external_data_sources &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md)   
 [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md)  
  
  
