---
description: sys.masked_columns (Transact-SQL)
title: sys.masked_columns (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 02/25/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.masked_columns
- masked_columns_tsql
- sys.masked_columns_tsql
- masked_columns
helpviewer_keywords:
- sys.masked_columns catalog view
ms.assetid: 671577e4-d757-4b8d-9aa9-0fc8d51ea9ca
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e5096f57c8e0b06c1f0403c446eb5417cd13b6f1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095565"
---
# <a name="sysmasked_columns-transact-sql"></a>sys.masked_columns (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Используйте представление **sys.masked_columns** для запроса столбцов таблицы, к которым применена функция динамического маскирования данных. Это представление наследуется из представления **sys.columns** . Оно возвращает все столбцы в представлении **sys.columns** , а также столбцы **is_masked** и **masking_function** , указывающие, маскирован ли столбец, и если да, то какая функция маскирования определена. В этом представлении отображаются только столбцы, к которым применена функция маскирования.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|object_id|**int**|Идентификатор объекта, которому принадлежит этот столбец.|  
|name|**sysname**|Имя столбца. Уникален в пределах объекта.|  
|column_id|**int**|Идентификатор столбца. Уникален в пределах объекта.<br /><br /> Идентификаторы столбца могут быть непоследовательными.|  
|**sys.masked_columns** возвращает много столбцов, наследуемых от **sys. Columns**.|Различная|Дополнительные определения столбцов см. в разделе [sys. columns &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md) .|  
|is_masked|**bit**|Указывает, является ли столбец маскированным. 1 означает маскирование.|  
|masking_function|**nvarchar(4000)**|Функция маскирования для столбца.|  
  
## <a name="remarks"></a>Комментарии  
  
## <a name="permissions"></a>Разрешения  
 Это представление возвращает сведения о таблицах, в которых пользователь имеет определенное разрешение для таблицы или пользователь имеет разрешение VIEW ANY DEFINITION.  
  
## <a name="example"></a>Пример  
 Следующий запрос присоединяет **sys.masked_columns** к **sys. Tables** , чтобы получить сведения обо всех маскированных столбцах.  
  
```  
SELECT tbl.name as table_name, c.name AS column_name, c.is_masked, c.masking_function  
FROM sys.masked_columns AS c  
JOIN sys.tables AS tbl   
    ON c.object_id = tbl.object_id  
WHERE is_masked = 1;  
```  
  
## <a name="see-also"></a>См. также:  
 [динамическое маскирование данных](../../relational-databases/security/dynamic-data-masking.md)   
 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)  
  
  
