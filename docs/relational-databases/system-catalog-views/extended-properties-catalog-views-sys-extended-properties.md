---
description: Представления каталога расширенных свойств — sys.extended_properties
title: sys.extended_properties (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.extended_properties
- sys.extended_properties_TSQL
- extended_properties
- extended_properties_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.extended_properties catalog view
ms.assetid: 439b7299-dce3-4d26-b1c7-61be5e0df82a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5a70bfd5e379dae4a68fa871ba7792cdf038d2ad
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092754"
---
# <a name="extended-properties-catalog-views---sysextended_properties"></a>Представления каталога расширенных свойств — sys.extended_properties
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает по одной строке для каждого из расширенных свойств в текущей базе данных.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|class|**tinyint**|Идентифицирует класс элемента, для которого определено свойство. Может принимать следующие значения:<br /><br /> 0 = база данных;<br /><br /> 1 = Объект или столбец<br /><br /> 2 = параметр<br /><br /> 3 = схема<br /><br /> 4 = участник базы данных<br /><br /> 5 = Сборка<br /><br /> 6 = Тип<br /><br /> 7 = индекс<br /><br /> 10 = коллекция схем XML<br /><br /> 15 = тип сообщений<br /><br /> 16 = контракт службы<br /><br /> 17 = служба<br /><br /> 18 = привязка удаленной службы<br /><br /> 19 = Маршрут<br /><br /> 20 = пространство данных (файловая группа или схема секционирования)<br /><br /> 21 = функция секционирования<br /><br /> 22 = файл базы данных<br /><br /> 27 = структура плана|  
|class_desc|**nvarchar(60)**|Описание класса элемента, для которого определено расширенное свойство. Может принимать следующие значения:<br /><br /> DATABASE<br /><br /> OBJECT_OR_COLUMN<br /><br /> ПАРАМЕТР<br /><br /> SCHEMA<br /><br /> DATABASE_PRINCIPAL<br /><br /> ASSEMBLY<br /><br /> TYPE<br /><br /> INDEX<br /><br /> XML_SCHEMA_COLLECTION<br /><br /> MESSAGE_TYPE<br /><br /> SERVICE_CONTRACT<br /><br /> SERVICE<br /><br /> REMOTE_SERVICE_BINDING<br /><br /> ROUTE<br /><br /> DATASPACE<br /><br /> PARTITION_FUNCTION<br /><br /> DATABASE_FILE<br /><br /> PLAN_GUIDE|  
|major_id|**int**|Идентификатор элемента, для которого определено расширенное свойство, интерпретируемый в соответствии с его классом. Для большинства элементов этот идентификатор отражает сущность, которую представляет класс. Большинство нестандартных идентификаторов интерпретируются следующим образом:<br /><br /> Если столбец class равен 0, то столбец major_id всегда равен 0.<br /><br /> Если столбец class равен 1, 2 или 7, то столбец major_id равен столбцу object_id.|  
|minor_id|**int**|Вторичный идентификатор элемента, для которого определено расширенное свойство, интерпретируемый в соответствии с его классом. Для большинства элементов содержит 0. В противном случае интерпретируется следующим образом:<br /><br /> Если столбец class = 1, то столбец minor_id равен столбцу column_id для столбцов и 0 для объектов.<br /><br /> Если столбец class = 2, то столбец minor_id равен столбцу parameter_id.<br /><br /> Если столбец class = 7, то столбец minor_id равен столбцу index_id.|  
|name|**sysname**|Имя свойства, уникальное в пределах столбцов class, major_id и minor_id.|  
|value|**sql_variant**|Значение расширенного свойства.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Представления каталога расширенных свойств &#40;Transact-SQL&#41;](./catalog-views-transact-sql.md)   
 [sys.fn_listextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-listextendedproperty-transact-sql.md)   
 [sp_addextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md)   
 [sp_dropextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropextendedproperty-transact-sql.md)   
 [sp_updateextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updateextendedproperty-transact-sql.md)  
  
