---
description: sys.external_data_sources (Transact-SQL)
title: sys.external_data_sources (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 1016db6e-9950-4ae2-a004-bd4171e27359
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6558dda3bf0cb87f3cdfc14b3beda0fc2c329557
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094638"
---
# <a name="sysexternal_data_sources-transact-sql"></a>sys.external_data_sources (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Содержит по одной строке для каждого внешнего источника данных в текущей базе данных для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , [!INCLUDE[ssSDS](../../includes/sssds-md.md)] и [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] .  
  
 Содержит по одной строке для каждого внешнего источника данных на сервере для [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] .  
  
|Имя столбца|Тип данных|Description|Диапазон|  
|-----------------|---------------|-----------------|-----------|  
|data_source_id|**int**|Идентификатор объекта для внешнего источника данных.||  
|name|**sysname**|Имя внешнего источника данных.||  
|location|**nvarchar(4000)**|Строка подключения, которая включает протокол, IP-адрес и порт для внешнего источника данных.||  
|type_desc|**nvarchar(255)**|Тип источника данных, отображаемый в виде строки.|HADOOP, RDBMS, SHARD_MAP_MANAGER, Ремотедатаарчиветипикстдатасаурце|  
|тип|**tinyint**|Тип источника данных, отображаемый в виде числа.|0 — HADOOP<br /><br /> 1 — RDBMS<br /><br /> 2 — SHARD_MAP_MANAGER<br /><br /> 3 — Ремотедатаарчиветипикстдатасаурце|  
|resource_manager_location|**nvarchar(4000)**|Для типа HADOOP — IP-адрес и расположение порта для диспетчера ресурсов Hadoop. Используется для отправки задания в источнике данных Hadoop.<br /><br /> Значение NULL для других типов внешних источников данных.||  
|credential_id|**int**|Идентификатор объекта учетных данных области базы данных, используемый для подключения к внешнему источнику данных.||  
|database_name|**sysname**|Для типа RDBMS — имя удаленной базы данных. В поле Тип введите SHARD_MAP_MANAGER имя базы данных диспетчера сопоставления сегментов. Значение NULL для других типов внешних источников данных.||  
|shard_map_name|**sysname**|Для типа SHARD_MAP_MANAGER имя сопоставления сегментов. Значение NULL для других типов внешних источников данных.||  
  
## <a name="permissions"></a>Разрешения  
 Видимость метаданных в представлениях каталогов ограничивается защищаемыми объектами, которыми пользователь владеет или на которые ему были предоставлены разрешения. Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [sys.external_file_formats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-external-file-formats-transact-sql.md)   
 [sys.external_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-external-tables-transact-sql.md)   
 [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md)  
  
  
