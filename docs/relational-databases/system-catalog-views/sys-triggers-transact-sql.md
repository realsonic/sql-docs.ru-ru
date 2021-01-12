---
description: sys.triggers (Transact-SQL)
title: sys. triggers (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- triggers
- triggers_TSQL
- sys.triggers
- sys.triggers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.triggers catalog view
ms.assetid: cefa4fc4-b8b9-4cd7-b124-eed5283acbfc
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b9fbf3f59de6e7508fd8ee1fac13a98bb36170b1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094327"
---
# <a name="systriggers-transact-sql"></a>sys.triggers (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Содержит по одной строке для каждого объекта, являющегося триггером типа TR или TA. Имена триггеров DML имеют область видимости схемы и, следовательно, видимы в **представлении sys. Objects**. Область существования имен триггеров DDL определяется родительской сущностью, поэтому эти имена видимы только в этом представлении.  
  
 Столбцы **parent_class** и **Name** однозначно идентифицируют триггер в базе данных.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Имя триггера. Имена триггеров DML ограничены областью схемы. Область имен триггеров DDL определяется в соответствии с родительской сущностью.|  
|**object_id**|**int**|Идентификационный номер объекта. Уникален в базе данных.|  
|**parent_class**|**tinyint**|Класс родителя триггера.<br /><br /> 0 = база данных (для триггеров DDL).<br /><br /> 1 = объект или столбец (для триггеров DML).|  
|**parent_class_desc**|**nvarchar(60)**|Описание родительского класса триггера.<br /><br /> DATABASE<br /><br /> OBJECT_OR_COLUMN|  
|**parent_id**|**int**|Идентификатор родителя триггера, определяющийся следующим образом:<br /><br /> 0 = триггеры, родителями которых являются базы данных.<br /><br /> Для триггеров DML это **object_id** таблицы или представления, для которых ОПРЕДЕЛЕН триггер DML.|  
|**type**|**char(2)**|Тип объекта:<br /><br /> TA = триггер сборки (среда CLR)<br /><br /> TR = триггер SQL|  
|**type_desc**|**nvarchar(60)**|Описание типа объекта.<br /><br /> CLR_TRIGGER<br /><br /> SQL_TRIGGER|  
|**create_date**|**datetime**|Дата создания триггера.|  
|**modify_date**|**datetime**|Дата последнего изменения объекта с помощью инструкции ALTER.|  
|**is_ms_shipped**|**bit**|Триггер создан от лица пользователя внутренним компонентом сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**is_disabled**|**bit**|Триггер выключен.|  
|**is_not_for_replication**|**bit**|Триггер создан с аргументом NOT FOR REPLICATION.|  
|**is_instead_of_trigger**|**bit**|1 = триггеры INSTEAD OF<br /><br /> 0 = триггеры AFTER|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога безопасности (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
