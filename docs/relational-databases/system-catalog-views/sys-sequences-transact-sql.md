---
description: sys.sequences (Transact-SQL)
title: sys. Sequences (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sequences_TSQL
- sys.sequences
- sequences
- sequences_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sequence number object, sys. sequences catalog view
- sys.sequences catalog view
ms.assetid: 0e1b0e32-1cce-40f7-83c8-860ec660138a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c64793ccb6d7280bcadd79de1587f09a8ac3eb29
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101805"
---
# <a name="syssequences-transact-sql"></a>sys.sequences (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Содержит строку для каждого объекта последовательности в базе данных.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|\<inherited columns>||Наследует все столбцы из представления [sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**start_value**|**sql_variant NOT NULL**|Стартовое значение для объекта последовательности. Если объект последовательности перезапускается с помощью инструкции ALTER SEQUENCE, он начинается с этого значения. Когда объект последовательности выполняет цикл, он переходит к **minimum_value** или **maximum_value**, а не **start_value**.|  
|**increment**|**sql_variant NOT NULL**|Значение, на которое увеличивается значение объекта последовательности после каждого созданного значения.|  
|**minimum_value**|**sql_variant NULL**|Минимальное значение, возвращаемое объектом последовательности. По достижении этого значения объект последовательности либо возвращает ошибку при попытке создать дополнительные значения, либо перезапускается, если для него указан параметр CYCLE. Если значение MINVALUE не задано, этот столбец возвращает минимальное значение, поддерживаемое типом данных генератора последовательностей.|  
|**maximum_value**|**sql_variant NULL**|Максимальное значение, возвращаемое объектом последовательности. По достижении этого значения объект последовательности либо начинает возвращать ошибку при попытке создать дополнительные значения, либо перезапускается, если для него указан параметр CYCLE. Если параметр MAXVALUE не задан, этот столбец возвращает максимальное значение, допустимое типом данных объекта последовательности.|  
|**is_cycling**|**бит не равен NULL**|Возвращает значение 0, если для объекта последовательности указан параметр NO CYCLE, и 1, если указан параметр CYCLE.|  
|**is_cached**|**бит не равен NULL**|Возвращает значение 0, если для объекта последовательности указан параметр NO CACHE, и 1, если указан параметр CACHE.|  
|**cache_size**|**int NULL**|Возвращает заданный размер кэша для объекта последовательности. Этот столбец содержит значение NULL, если последовательность была создана с параметром NO CACHE или был указан параметр CACHE без указания размера кэша. Если значение cache_size больше максимального числа значений, которые может возвращать объект последовательности, все равно показывается такой недостижимый размер кэша.|  
|**system_type_id**|**TINYINT NOT NULL**|Идентификатор системного типа для типа данных объекта последовательности.|  
|**user_type_id**|**int NOT NULL**|Определенный пользователем идентификатор типа данных для объекта последовательности.|  
|**precision**|**TINYINT NOT NULL**|Максимальная точность типа данных.|  
|**масштаб**|**TINYINT NOT NULL**|Максимальный масштаб типа данных. Масштаб возвращается вместе с точностью для предоставления пользователю полных метаданных. Масштаб объектов последовательности всегда равен 0, поскольку для них допустимы только целочисленные типы.|  
|**current_value**|**sql_variant NOT NULL**|Последнее предоставленное значение. То есть значение, возвращенное из последнего выполнения функции NEXT VALUE FOR или последнего значения для выполнения процедуры **sp_sequence_get_range** . Если последовательность не использовалась, возвращается значение START WITH.|  
|**is_exhausted**|**бит не равен NULL**|0 указывает, что последовательность еще может предоставлять новые значения. 1 указывает, что объект последовательности достиг значения MAXVALUE и для последовательности не задан параметр CYCLE. Функция NEXT VALUE FOR будет возвращать ошибку, пока последовательность не будет перезапущена с помощью инструкции ALTER SEQUENCE.|  
|**last_used_value**|**sql_variant NULL**|Возвращает последнее значение, созданное функцией [Next value for](../../t-sql/functions/next-value-for-transact-sql.md) . Применимо к SQL Server 2017 и более поздних версий.|  
  
## <a name="permissions"></a>Разрешения  
 В [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] и более поздних версиях видимость метаданных в представлениях каталогов ограничивается защищаемыми объектами, которыми пользователь владеет или на которые ему были предоставлены разрешения. Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Порядковые номера](../../relational-databases/sequence-numbers/sequence-numbers.md)   
 [CREATE SEQUENCE (Transact-SQL)](../../t-sql/statements/create-sequence-transact-sql.md)   
 [ALTER SEQUENCE (Transact-SQL)](../../t-sql/statements/alter-sequence-transact-sql.md)   
 [DROP SEQUENCE (Transact-SQL)](../../t-sql/statements/drop-sequence-transact-sql.md)   
 [NEXT VALUE FOR (Transact-SQL)](../../t-sql/functions/next-value-for-transact-sql.md)   
 [sp_sequence_get_range (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-sequence-get-range-transact-sql.md)  
  
  
